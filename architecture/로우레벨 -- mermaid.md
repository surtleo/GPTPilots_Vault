```
flowchart TD

  RAWCSV[("data/raw/data_list.csv<br/>100행 메타데이터")]:::missingData
  RAWFILES[("data/raw/files/<br/>hwp 96 + pdf 4")]:::missingData

  subgraph S1["1. 추출 — src/ingest/extract.py — extract_one() 호출마다 별도 서브프로세스(120초 타임아웃)"]
    DETECT{"매직바이트로<br/>포맷 판별"}
    HWP["[서브프로세스 1개] extract_hwp.py<br/>hwp_hwpx_parser<br/>TableStyle.MARKDOWN / 이미지 무시"]
    LAYOUT["[서브프로세스 1개] layout 엔진<br/>(기본 시도)"]
    CLASSIC["[서브프로세스 1개] classic 엔진<br/>(layout 실패 시 재시도)"]
    PLAIN["[서브프로세스 1개] plain 엔진<br/>(표 감지 OFF, 최후 재시도)"]
    EXTRACTED[("data/extracted/{doc_id}.json")]
    EXTRACTREPORT[("_extract_report.json")]

    RAWFILES --> DETECT
    DETECT -->|hwp5 / hwpx| HWP
    DETECT -->|pdf| LAYOUT
    LAYOUT -->|timeout/parser_error| CLASSIC
    CLASSIC -->|timeout/parser_error| PLAIN
    HWP -->|성공| EXTRACTED
    LAYOUT -->|성공| EXTRACTED
    CLASSIC -->|성공| EXTRACTED
    PLAIN --> EXTRACTED
    HWP --> EXTRACTREPORT
    LAYOUT --> EXTRACTREPORT
    CLASSIC --> EXTRACTREPORT
    PLAIN --> EXTRACTREPORT
  end

  subgraph S2["2. meta — src/ingest/normalize_meta.py"]
    MATCHGATE{"match_csv_to_files()<br/>CSV↔파일명 전건 대조"}
    METAERR["MetaMatchError<br/>→ 파이프라인 전체 중단"]:::errorNode
    NORMALIZE["기관명 표준화(괄호 제거)<br/>금액·날짜 이원화 파싱<br/>성공: _num/_dt · 실패: 원문 보존"]
    METAJSON[("meta.json")]
    ORGDICT[("org_dictionary.json<br/>표준명 + 별칭 dict")]

    RAWCSV --> MATCHGATE
    RAWFILES --> MATCHGATE
    MATCHGATE -->|미매칭 1건이라도| METAERR
    MATCHGATE -->|전건 성공| NORMALIZE
    NORMALIZE --> METAJSON
    NORMALIZE --> ORGDICT
  end

  subgraph S3["3. validate — src/ingest/validate.py"]
    CHECKS["validate_doc() 4체크<br/>추출량비율 · 표 유효성 ·<br/>문장완결 · 헤더존재"]
    VERDICT{"판정"}
    VEXCLUDED["excluded_ocr<br/>(0자 또는 비율 미달+절대량부족)"]:::errorNode
    VFLAGGED["flagged<br/>(그 외 실패 — 제외 안 함)"]
    VPASS["pass"]
    VREPORT[("validation_report.csv")]
    OCRPENDING[("ocr_pending.json")]

    EXTRACTED --> CHECKS
    RAWFILES -.원본 본문 크기 측정<br/>hwp5 BodyText/hwpx section/pdf fitz.-> CHECKS
    CHECKS --> VERDICT
    VERDICT --> VEXCLUDED
    VERDICT --> VFLAGGED
    VERDICT --> VPASS
    VEXCLUDED --> OCRPENDING
    CHECKS --> VREPORT
    EXTRACTREPORT -.추출 자체 실패 문서<br/>extract_failed 로 병합.-> VREPORT
  end

  subgraph S4["4. index — src/index/build_index.py"]
    LOADINPUT["load_index_inputs()<br/>excluded_ocr 문서 제외"]
    CLEAN["clean.py<br/>NFC정규화 · 목차제거 ·<br/>헤더/푸터제거 · 공백정리"]
    MASK["mask_pii.py<br/>전화 · 이메일 · 실명 → 토큰 치환"]
    CHUNK["chunk.py<br/>의사헤더승격 → 섹션분할 →<br/>표분리/재분할 → 512토큰 재검증"]
    EMBED["e5_embed()<br/>intfloat/multilingual-e5-small<br/>passage: 접두어"]
    CHROMACHUNKS[("Chroma: chunks 컬렉션<br/>본문 검색용 · 추출실패 제외 99건")]

    LOADCARDS["load_doc_cards_inputs()<br/>100건 전부, 실패문서 포함"]
    CARDEMBED["카드 임베딩<br/>사업명 / 발주기관 / 사업요약"]
    CHROMACARDS[("Chroma: doc_cards 컬렉션<br/>카드 검색용 · 100건 전부")]

    GATE{"check_index_gate()<br/>cards - chunks == extract_failed ?"}
    PIPEERR["PipelineError<br/>→ 파이프라인 중단"]:::errorNode

    EXTRACTED --> LOADINPUT
    METAJSON --> LOADINPUT
    OCRPENDING -.제외 목록.-> LOADINPUT
    LOADINPUT --> CLEAN --> MASK --> CHUNK --> EMBED --> CHROMACHUNKS
    METAJSON --> LOADCARDS --> CARDEMBED --> CHROMACARDS
    CHROMACHUNKS --> GATE
    CHROMACARDS --> GATE
    VREPORT -.extract_failed 목록<br/>= expected_missing.-> GATE
    GATE -->|diff 불일치| PIPEERR
  end

  MANIFEST[("pipeline_manifest.json<br/>(src/pipeline.py)<br/>설정 해시 + 코드 해시 비교<br/>→ 동일하면 단계 스킵")]
  MANIFEST -.해시 확인.-> DETECT
  MANIFEST -.해시 확인.-> MATCHGATE
  MANIFEST -.해시 확인.-> CHECKS
  MANIFEST -.해시 확인.-> LOADINPUT

  subgraph S5["5. 온라인 쿼리 — src/app.py answer_once()"]
    QUESTION["사용자 질문 + active_doc_id"]
    ROUTER{"route_question()<br/>router.py _match() fuzzy 매칭<br/>(threshold 80, tie margin 5)"}
    DIRECTMETA["direct_meta<br/>CSV 값 즉답 — LLM 무호출"]
    METAONLY["meta_only<br/>청크 0개 → 메타만 제공 — LLM 무호출"]
    CLARIFY["clarify<br/>후보 나열 반문 — LLM 무호출"]
    EXPLORE["explore<br/>doc_cards 후보 제시 — LLM 무호출"]
    RETRIEVE["retrieve.py<br/>doc_id 필터 + top-5 검색"]
    NOHIT["고정응답: 근거 못 찾음<br/>(필터 해제 전체검색 금지)"]
    GENERATE(["generate.py<br/>gpt-5-mini 생성<br/>환각금지 시스템프롬프트 + 재시도"])
    LEDGER["TokenLedger<br/>세션 누적 비용 추적"]
    COSTSTOP["CostLimitExceeded<br/>$2 상한 초과 → 세션 종료"]:::errorNode
    ANSWER["답변 + 출처표기<br/>+ route/청크수/토큰·비용 로그"]

    QUESTION --> ROUTER
    ORGDICT -.별칭 사전 로드.-> ROUTER
    METAJSON -.문서 메타 로드.-> ROUTER
    ROUTER -->|금액 · 마감일 패턴| DIRECTMETA
    ROUTER -->|문서 확정, 청크 0개| METAONLY
    ROUTER -->|동점 · 미특정| CLARIFY
    ROUTER -->|탐색형 질문| EXPLORE
    ROUTER -->|일반 질문, 문서확정| RETRIEVE
    CHROMACHUNKS -.top-5 검색.-> RETRIEVE
    CHROMACARDS -.후보 검색.-> EXPLORE
    CHROMACARDS -.후보 검색.-> CLARIFY
    METAJSON -.값 조회.-> DIRECTMETA
    METAJSON -.값 조회.-> METAONLY
    RETRIEVE -->|0건| NOHIT
    RETRIEVE -->|hit 있음| GENERATE
    GENERATE --> LEDGER
    LEDGER -->|상한 초과| COSTSTOP
    DIRECTMETA --> ANSWER
    METAONLY --> ANSWER
    CLARIFY --> ANSWER
    EXPLORE --> ANSWER
    NOHIT --> ANSWER
    GENERATE --> ANSWER
  end

  subgraph S6["6. 평가 — src/eval/run_eval.py + goldenset.py"]
    GOLDENSET[("data/goldenset.jsonl<br/>26문항 · 6유형")]:::missingData
    EVALITEM["evaluate_item()<br/>문항마다 신규 SessionState"]
    JUDGE(["gpt-5-nano judge<br/>정확성 · 충실성 1~5점<br/>json_schema 강제"])
    SUMMARIZE["summarize()<br/>doc_hit_rate · routing_accuracy ·<br/>judge 평균 · 비용"]
    REPORT[("data/eval/eval_report_*.json")]

    GOLDENSET --> EVALITEM
    EVALITEM -->|선행_턴 순차 실행 후 본질문| ROUTER
    ANSWER -->|턴 결과| EVALITEM
    EVALITEM --> JUDGE
    JUDGE --> SUMMARIZE
    SUMMARIZE --> REPORT
  end

  subgraph S7["LangSmith — src/query/observability.py"]
    MASKIO["mask_io()<br/>문자열 전부 제거<br/>TRACE_SAFE_KEYS 숫자만 통과"]
    DATASET[["LangSmith Dataset<br/>rfp-rag-goldenset (마스킹본만)"]]
    TRACE[["LangSmith 트레이스<br/>(@traced 함수 호출 자동 기록)"]]
    SUMMARYLOG[["LangSmith 요약 기록<br/>(숫자 지표만)"]]

    RETRIEVE -.@traced.-> MASKIO
    GENERATE -.@traced.-> MASKIO
    MASKIO --> TRACE
    GOLDENSET -.mask_text 마스킹 후.-> DATASET
    SUMMARIZE -.숫자만.-> SUMMARYLOG
  end

  subgraph S8["비교 실험(별도, 프로덕션 미접촉) — scripts/run_experiments.py"]
    VARIANTS["4변형: 청크 300/500/800자<br/>× e5-small / e5-base"]
    EXPCHROMA[("data/chroma_exp/{variant}<br/>별도 persist 디렉토리")]
    HITMETRIC["hit@5 · precision@5<br/>LLM 없음 — 비용 $0"]
    CHOSEN["채택: 500자 + e5-small<br/>(EXPERIMENTS.md)"]

    GOLDENSET -.retrieval성 14문항만 추출<br/>make_questions().-> VARIANTS
    VARIANTS --> EXPCHROMA --> HITMETRIC --> CHOSEN
  end

  classDef missingData stroke-dasharray: 5 5,stroke:#f0932b,stroke-width:2px;
  classDef errorNode stroke:#eb4d4b,stroke-width:2.5px,fill:#ffdddd;
```