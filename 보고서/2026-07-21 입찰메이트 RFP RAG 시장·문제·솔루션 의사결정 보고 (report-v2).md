---
title: 입찰메이트 RFP RAG — 시장·문제·솔루션 의사결정 보고 (report-v2)
author: GPTPilots (deep-research 후처리)
date: 2026-07-21
tags: [보고서, 시장조사, 의사결정, RAG, RFP]
---

# 입찰메이트 RFP RAG — 시장·문제·솔루션 의사결정 보고서 (report-v2)

> 폼: `docs/research/deep-research-prompt-v2.md` (플러그인 연동판, 의사결정 3부 구성)
> 근거: `results/*.json` 41개 항목 (deep-research 수집, 검증 완료) + `_digest/`
> 작성: 원자료 재조직·후처리 (새 웹서치 없음). 모든 표 셀·문장은 III부 근거로 역추적.
> 접근확인 표기: ✓ verified · △ partial · ◐ snippet-only · ✗ blocked · [벤더]=벤더 콘텐츠
>
> **이 문서 하나로 (1) 시장·문제 파악 → (2) 후보 솔루션 비교 → (3) 무엇을 만들지 결정이 가능하도록 작성.**

---

# PART 0 — 한눈 요약 / 의사결정

**시장 (한 줄).** 한국 공공조달은 **225.1조원(2024, GDP의 ~9%, +7.9% 최고치)** 규모이고 그중 **중소기업 몫이 63.1%(142.1조)**, 거래의 64.5%(145.1조)가 나라장터(KONEPS) 한 곳을 지난다 — **연 49만 건의 입찰공고**가 쏟아지지만 개별 중소기업은 이를 걸러 읽을 도구가 없다. 정부는 AI 전환 중이나(ppsai.kr, 2027 통합 플랫폼) **전부 발주기관(buyer) 편** — 입찰기업(bidder) 편 RFP 이해 도구는 비어 있다.

**핵심 문제 (3줄).**
1. **RFP를 읽을 수가 없다.** 공공문서 **91.1%가 AI-불가독 HWP/이미지**이고, RFP 한 건에 평가항목·가점·필수첨부·형식요구가 **수십 개 흩어져** 있어 제안서 1건에 **평균 30시간**이 든다. 그런데 중소기업엔 **제안팀이 없다**(대기업 삼성SDS는 제안지원팀 33명).
2. **규칙을 못 읽어 탈락한다.** PC 조달 표본 **부적격율 53.3%**(그중 89.4%가 가격 하한 미달), 대형건설 **유찰 ~71%**, 간이종심제 **동일가격 투찰 68.96%**(2020년 0.9%→2026년) — 낙찰하한·배점·자격 계산 실패가 곧 탈락.
3. **정보비대칭이 브로커·컨설팅 의존을 낳는다.** 조달 브로커는 낙찰액의 **3~5% 수수료**로 회원 4,000명을 굴리는데 적발은 **0건**. 컨설팅은 "**돈은 냈는데 성과가 없다**"(월 수백만원). 정부 무료 상담(공공조달길잡이)은 **상담원 36명** 병목(2,000상담→시장진입 260개).

**후보 솔루션 요약표 (자세히는 I-2).** 평가 4기준: 독창성 / 임팩트 / 부서급 잠재력 / 데모매력. ●상 ◐중 ○하.

| # | 후보 솔루션 | 독창성 | 임팩트 | 부서급 | 데모 | 종합 |
|---|---|:--:|:--:|:--:|:--:|:--:|
| **1** | **배점표·자격·제출요건 자동 추출 + 부적격 자가진단** | ● | ● | ● | ● | **★ 1순위** |
| 2 | 독소조항·과업불명확 리스크 탐지 | ◐ | ● | ◐ | ◐ | 2순위 |
| 3 | RFP Q&A·요약 어시스턴트 (인용근거) | ○ | ◐ | ◐ | ● | 3순위 |
| 4 | 제안서 초안 + 컴플라이언스 매트릭스 생성 | ◐ | ● | ● | ○ | 보류 |
| 5 | 유사 RFP·낙찰사례 검색 + 배점 벤치마크 | ○ | ◐ | ○ | ◐ | 보류 |

**추천 1순위 (+이유 3줄).** **후보 1 — "배점표·자격·제출요건 자동 추출 + 부적격 자가진단"**.
- (독창성) 국내 discovery 경쟁사(Info21C·KBID·G2B Plus·DeepBID·JodAL)는 전부 **메타데이터·검색까지만**이고 RFP 문서의 배점표·자격을 추출하지 않는다. 가장 근접한 CLIWANT조차 범용 + **미국 GovCon으로 피벗 중**. 해외 GovDash·Brainial은 배점추출을 하지만 **FAR·EU·PDF 대상** — 한국 HWP·국가계약법·협상계약 세부기준은 미커버.
- (임팩트·부서급) 부적격율 53.3%·가격미달 89.4%를 **사전 진단**으로 막고, 중소기업에 없는 **제안·컴플라이언스 부서를 대체**한다(department-in-a-box).
- (데모) **보유 RFP 100건**으로 "이 공고의 기술/가격 배점·자격요건·제출서류·낙찰하한"을 추출·자가진단하는 화면을 바로 시연 — 정답이 문서에 명시돼 **검증 가능**.

**최대 리스크 (1줄).** 정부 플랫폼(ppsai.kr) 또는 CLIWANT가 bidder-side 배점추출로 확장 시 잠식 — 다만 현재 정부는 buyer-side, CLIWANT는 미국 피벗이라 **국내 SW/IT 수직 특화 + 인용근거(환각 가드)**로 창이 열려 있다.

---

# PART I — 문제 정의 → 후보 솔루션 → 추천

## I-1. 문제 정의 (페인포인트)

> 지표·VoC에서 도출. 국내·해외가 같은 고통을 말하면 "국내외 공통"으로 강도 표시. 근거는 수치 링크 + 인용 링크 1:1.

**P1. RFP를 다 읽을 수 없다 — 문서량 × 길이의 triage 붕괴.** (국내외 공통, 강)
누가/상황: 나라장터에 **연 49만 건**([조달청 연혁](https://www.pps.go.kr/kor/content.do?key=01230) ✓)·일 3,000건([Google for Startups](https://blog.google/intl/ko-kr/company-news/outreach-initiatives/gfs-yearbook-cliwant/) ✓)의 수십~수백 페이지 RFP. "하루 수천 건의 RFP를 전부 읽는 것은 거의 불가능"([CLIWANT](https://blog.cliwant.com/rfp/) [벤더] ✓). 해외도 동일: *"a required document buried on page 34 of the solicitation… we leave money on the table by not responding to every RFP"*([Reddit r/govcon](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/) ✓).

**P2. RFP 한 건 안에 요구가 수십 개 흩어져 있다 — task-0가 '뭘 해야 하는지 정리'.** (국내외 공통, 강)
"RFP 한 건 안에 평가 항목, 가점 조건, 필수 첨부, 형식 요구가 **수십 개 흩어져** 있어 첫 작업이 '뭘 해야 하는지 정리하는 것'부터… 한 건당 평균 **30시간**"([CLIWANT](https://blog.cliwant.com/ibcal-damdangjaga-jaggu-toesahaeyo-iyureul-alryeodeurilggayo/) [벤더] ✓). 해외: Section L(지시)↔M(평가) 충돌이 **자동 실격(auto-DQ)**을 부르는 게 "the real nightmare"([Reddit](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/) ✓).

**P3. 규칙(배점·자격·낙찰하한)을 못 읽어 탈락한다.** (국내, 최상)
PC 조달 표본 **부적격율 53.3%**(6,406/12,052), 그중 **89.4%가 가격 하한 미달**([조달경제신문](https://www.jodaleconomy.com/news/articleView.html?idxno=2169) ✓). "투찰 업체 2곳 중 1곳은 심사 문턱조차 넘지 못하고 탈락"(同). 대형건설(300억+) 유찰 **~71%**(2024)([대한건설협회/News1](https://www.news1.kr/realestate/general/5810087) ✓).

**P4. HWP/HWPX 파싱 장벽 — RAG의 최대 기술 리스크이자 해자.** (국내 고유, 최상)
공공 행정문서 **91.1%가 AI-불가독**(HWP/이미지/스캔PDF)([위성곤 의원실 국감](https://v.daum.net/v/20251013175850275) ✓). 오픈소스는 사실상 HWP 5.x부터, 3.x는 라이브러리 자체가 없고 표·읽기순서·차트가 깨진다([Polaris](https://www.polarisoffice.com/business-blog/claude-skill-for-hwp-parsing) [벤더] ◐). "**파서 단계에서 80%가 결정**"([KoreaDeep](https://www.koreadeep.com/blog/hwp-parser-ai) [벤더] ◐), 벤치마크 천장도 **84.9%**([LlamaIndex ParseBench](https://www.llamaindex.ai/blog/parsebench) ✓)로 미해결. 실무자: "**HWP는 Word보다 체감상 열 배 어려웠다… 이건 진짜 헬**"([Brunch](https://brunch.co.kr/@1212ac31a500435/329) [벤더] ✓).

**P5. 중소기업엔 제안·컴플라이언스 부서가 없다 — department gap.** (국내외 공통, 최상)
대기업엔 상설 부서: 삼성SDS **제안지원팀 33명**, 제안서 1건당 **10~50명**([아이뉴스24](http://www.inews24.com/view/95674) ✓), 제안비가 매출의 **1~2%**(同). 중소기업 현실: "**제안서를 전담하는 팀이 있는 기업도 있지만, 대부분은 PM이나 기획자가 겸업**"([CLIWANT/APMP](https://blog.cliwant.com/what-is-apmp-proposal-management/) [벤더] ◐), 개발자가 홀로 "기존 문서를 보고 전부 짜내야"([Blind](https://www.teamblind.com/kr/post/%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80-%EC%A0%9C%EC%95%88%EC%84%9C-%EC%9E%91%EC%84%B1%ED%95%98%EB%8A%94-%ED%9A%8C%EC%82%AC-aFRJV56N) ✓). 해외 동일: *"how small shops are handling this without a compliance department… Nobody LOL"*([Reddit](https://www.reddit.com/r/govcon/comments/1uphsvk/small_shops_with_itar_flowdowns_and_no_compliance/) ✓).

**P6. RFP 자체가 불명확 → 무상 재작업·과업변경 비용 전가.** (국내, 강)
공공SW 수행 111개사 중 **50%가 불명확한 RFP로 무상 재작업·과업추가**를 경험([KOSA 2012/SPRi](https://spri.kr/download/5378) ✓). 과업변경 대가지급률은 중앙행정기관 **4.4%**, 지자체 15.3%(同 ✓) — 사실상 발주자가 비용을 안 준다.

**P7. 정보비대칭이 브로커·컨설팅 의존을 낳는다 — 회색시장 WTP.** (국내, 강)
조달 브로커는 낙찰액의 **3~5%**를 떼고 회원 **~4,000명**을 운용하지만 적발 **0건**([언론 단독](https://v.daum.net/v/KnTAM81mhw?f=p) ✓). 브로커 의존이 "간이종심제가 더 이상 정상 작동 안 한다"는 지경([대한경제](https://www.dnews.co.kr/uhtml/view.jsp?idxno=202601081433098030893) ✓)으로 **동일가격 투찰 68.96%**([기재부/뉴스핌](https://www.newspim.com/news/view/20260721000301) ✓). 전통 컨설팅엔 "**돈은 냈는데 성과가 없다**"(월 수백만원)([CLIWANT](https://blog.cliwant.com/bidding-consulting-comparison-ai-vs-traditional/) [벤더] ✓).

**P8. 무료·자력 상담은 인간 병목으로 확장 불가.** (국내, 중)
공공조달길잡이는 **상담원 36명**·11개 지방청으로 2,000여 상담을 했지만 실제 시장진입은 **~260개사**, 초기 1,049상담→쇼핑몰 등록 74개([머니투데이](https://www.mt.co.kr/policy/2025/10/28/2025102813262913576) ✓, [보안뉴스](https://m.boannews.com/html/detail.html?idx=133962) ✓). 정부 스스로 "**정보 부족으로 공공조달에 참여하지 못하는 기업**"을 사각지대로 규정([헤럴드경제](https://biz.heraldcorp.com/article/10814594) ✓) — 자동화가 닿아야 할 수요.

**P9. 신뢰(grounding) 없는 AI는 안 쓴다 — 인용·거절 가드가 전제.** (국내외 공통, 강)
실무자: "*If the tool says a form is required, I'd want it to show the exact page/section… Otherwise it's hard to trust on a live bid*"([Reddit](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/) ✓). 근거: 법률 RAG조차 환각율 **17~34%**([Stanford RegLab](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-or-more-benchmarking-queries) ✓); 한국어 법률 RAG는 과목별로 +22.5%~**-15%**로 오히려 해가 되기도([LRAGE](https://arxiv.org/html/2504.01840v1) ✓). 답변-근거 1:1 인용과 저신뢰 시 거절이 필수.

**P10. discovery는 이미 무료·커모디티 — 차별화는 문서 이해층뿐.** (국내, 전략)
KONEPS OpenAPI 무료([data.go.kr](https://www.data.go.kr/data/15129394/openapi.do) ✓), G2B Plus 무료+월 9,900원([g2bplus](https://www.g2bplus.kr/service_goods_list.php) ✓), DeepBID 무료([deepbid](https://deepbid.com/) ✓). 공고 검색으로는 못 이긴다 → **RFP 문서 내부(배점·자격·요건) 이해**로 올라가야 한다.

## I-2. 후보 솔루션 비교표 (이 표 보고 결정)

> 고정 제약: RAG + 정부문서(RFP) 임베딩으로 풀 수 있고, 보유 RFP 100건(hwp/pdf)+메타데이터로 데모 가능한 것만. 4기준 ●상 ◐중 ○하.

| # | 후보 솔루션 | 겨냥 문제 | RAG+RFP로 푸는 법 | 핵심 근거(링크 2) | 데모 | 독창성 | 임팩트 | 부서급 | 데모매력 | 종합 |
|---|---|---|---|---|---|:--:|:--:|:--:|:--:|:--:|
| **1** | **배점표·자격·제출요건 추출 + 부적격 자가진단** | P3·P5·P2 | HWP 파싱→평가배점표(기술/가격)·자격요건·제출서류·낙찰하한 구조 추출→투찰 전 부적격 경고 | 부적격 [53.3%/89.4%](https://www.jodaleconomy.com/news/articleView.html?idxno=2169)✓ · 협상계약 배점 [기술90/가격10](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000086294)△ | 가능(강) | ● | ● | ● | ● | **★** |
| 2 | 독소조항·과업불명확 리스크 탐지 | P6 | RFP에서 과업변경 무보상·지체상금·과도요구 등 불리조항 탐지·경고 | 불명확RFP 무상재작업 [50%](https://spri.kr/download/5378)✓ · 대가지급 [4.4%](https://spri.kr/download/5378)✓ | 가능(중) | ◐ | ● | ◐ | ◐ | 2위 |
| 3 | RFP Q&A·요약 (인용근거) | P1·P9 | 공고 hwp/pdf 임베딩→"과업범위/자격/일정" 자연어 Q&A, 답변마다 페이지 인용 | "수백페이지 못 읽음"[CLIWANT](https://blog.cliwant.com/rfp/)✓ · 인용요구[Reddit](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/)✓ | 쉬움(강) | ○ | ◐ | ◐ | ● | 3위 |
| 4 | 제안서 초안 + 컴플라이언스 매트릭스 | P5·P2 | 요구→응답 매트릭스 자동생성 + 섹션 초안(HWP 출력) | GovDash [$40M](https://siliconangle.com/2026/01/15/govdash-secures-30m-expand-ai-driven-government-contracting-software/)✓ · 제안 [30h](https://blog.cliwant.com/ibcal-damdangjaga-jaggu-toesahaeyo-iyureul-alryeodeurilggayo/)✓ | 부분 | ◐ | ● | ● | ○ | 보류 |
| 5 | 유사 RFP·낙찰사례 검색 + 배점 벤치마크 | P1·P3 | 공고↔계약 API 조인, 유사 과거 RFP·낙찰가·배점 검색 | 계약 API [공고↔계약키](https://www.data.go.kr/data/15129427/openapi.do)✓ · 브로커가 대신하는 일[대한경제](https://www.dnews.co.kr/uhtml/view.jsp?idxno=202601081433098030893)✓ | 가능(중) | ○ | ◐ | ○ | ◐ | 보류 |

**후보 1 보충.** 문제: 투찰 전에 "내가 이 공고에 적격인가, 배점은 어디서 갈리나"를 못 봐서 절반이 탈락. 근거: 부적격 [53.3%·가격미달 89.4%](https://www.jodaleconomy.com/news/articleView.html?idxno=2169)✓ + 협상계약 [기술85% 이상=협상적격](https://www.law.go.kr/admRulLsInfoP.do?admRulSeq=2100000028030)△. 방향: HWP 배점표·자격표·제출서류표는 **정형 스키마**라 RFP-구조 인지 파싱으로 추출 정확도를 올릴 수 있다. 데모: 100건으로 "배점·자격·제출·하한 체크리스트" 출력 — 정답이 문서에 있어 검증 쉬움. 리스크: CLIWANT/ppsai.kr 확장.

**후보 4 보류 사유.** 임팩트·부서급은 최고지만 (a) 생성은 이미 레드오션(MyBidWise "제안서 90% 단축" [자칭](https://mybidwise.com/), SK AX Proposal AI, 해외 GovDash/AutogenAI/Vultron), (b) **HWP 생성**은 파싱보다 어렵고("표는 처음부터 다 만들어야"[Brunch](https://brunch.co.kr/@1212ac31a500435/329)✓), (c) 생성물 품질은 데모에서 정답 검증이 어렵다. → 후보 1 성공 후 확장 모듈로 적합.

## I-3. 추천 1순위와 이유 — 후보 1

**uniqueness (경쟁 공백).**
- 국내 discovery(Info21C 330K원/년·회원 10만, KBID·G2B Plus·DeepBID·JodAL)는 **문서 내부를 추출하지 않는다** — 메타데이터·검색·통계까지(II-2, III-4). JodAL이 "첨부파일 검색"을 자랑하지만 **검색까지**지 배점 추출·자가진단은 아님([jodal.ai](https://jodal.ai/) ◐).
- 국내 가장 근접한 CLIWANT는 RAG-on-RFP를 검증(독소조항·자격·가격, Pre-A 20억)했지만 **범용 + 2025년부터 미국 GovCon 피벗**([더벨/ZDNet](https://zdnet.co.kr/view/?no=20240819004849) ✓) → 국내 SW/IT RFP의 **배점표 정밀 추출 수직 특화**가 공백.
- 해외 GovDash·Brainial은 컴플라이언스 매트릭스·배점 추출을 하지만 **FAR/EU·PDF** 대상 — 한국 **HWP + 국가계약법/SW진흥법/협상계약 세부기준**은 미커버(III-4·III-6). 시장 겹침 0.

**business impact (정량).**
- 방지 가능 손실: PC 표본에서 투찰 12,052건 중 **6,406건(53.3%)이 부적격**, 그중 **89.4%가 가격 하한 미달**([조달경제](https://www.jodaleconomy.com/news/articleView.html?idxno=2169) ✓) — 낙찰하한 자동계산·경고로 상당수는 사전 회피 가능한 무효투찰.
- 시간 회수: 제안 task-0(요구·배점 정리)는 30h 중 초반 상당부분([CLIWANT](https://blog.cliwant.com/ibcal-damdangjaga-jaggu-toesahaeyo-iyureul-alryeodeurilggayo/) ✓). 가정: 겸업 담당 1인이 건당 2~3h를 요구정리에 쓴다면(P2·P5), 자동 체크리스트가 이를 압축.
- WTP 증거: 브로커에 낙찰액 **3~5%**([언론](https://v.daum.net/v/KnTAM81mhw?f=p) ✓), 컨설팅에 월 수백만원([CLIWANT](https://blog.cliwant.com/bidding-consulting-comparison-ai-vs-traditional/) ✓)을 이미 지불 — 저비용 자가진단으로 흡수 가능.

**부서급 잠재력.** 중소기업이 **없는 부서(제안·컴플라이언스)**를 대체. 대기업엔 그 조직이 실재: 삼성SDS 제안지원팀 33명·제안비 매출 1~2%([아이뉴스24](http://www.inews24.com/view/95674) ✓), 잡코리아 '제안서작성' 채용 **~2,829건**([JobKorea](https://www.jobkorea.co.kr/Search?stext=%EC%A0%9C%EC%95%88%EC%84%9C%EC%9E%91%EC%84%B1) ✓). 이 부서를 못 가진 곳이 시장의 다수(등록기업 57.2만, SME 63.1%).

**visually appealing 데모 (RFP 100건으로).**
- 화면1 — **공고 진단 카드**: RFP 업로드 → "기술 90 / 가격 10 배점, 협상적격 기준 기술 85%↑, 자격: OOO 실적/면허, 제출서류 12종, 낙찰하한 OO%" + 각 항목에 **원문 페이지 인용**.
- 화면2 — **부적격 자가진단 신호등**: 우리 회사 프로필 입력 → 자격 충족/미달·가격 하한 위험을 🟢🟡🔴로. (정답이 RFP·법령에 명시돼 검증 가능)

**왜 2·3순위가 아닌가.** ②독소조항은 CLIWANT가 선점·차별성 약(◐). ③Q&A는 범용이라 범용 LLM/CLIWANT와 겹쳐 독창성 낮음(○). ④생성은 레드오션+HWP생성 난이도+데모 검증난(보류). ⑤유사검색은 discovery와 겹치고 부서급 약(보류).

**최대 리스크와 헤지.**
- 리스크A: 정부 ppsai.kr이 bidder-side로 확장 → 현재는 **발주기관용 RFP 생성·예정가격**(17.8억, 30~40% 시간↓)로 **buyer-side 전용**([etnews](https://www.etnews.com/20260522000281) ✓), 입찰기업용 자가진단은 안 만듦. 헤지: bidder-side·SW/IT 수직에 먼저 깊게.
- 리스크B: CLIWANT가 국내 배점추출 심화 → 미국 피벗 중이 창. 헤지: **인용근거 + 평가지표(LangSmith)**로 신뢰 차별화(P9, [Stanford 17~34%](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-or-more-benchmarking-queries) ✓).
- 리스크C: HWP 파싱 실패 → 파싱이 80% 좌우([KoreaDeep](https://www.koreadeep.com/blog/hwp-parser-ai) ◐). 헤지: HWPX 우선 + 배점표 등 **알려진 RFP 표 스키마** 후처리 + 필요시 유료 파싱 API([Upstage $0.01/page](https://www.upstage.ai/products/document-parse) ✓) baseline.

---

# PART II — 시장 개요

## II-1. 정부/공공조달 개요

**규모·구조 (조달청 2024 공공조달통계연보, 전부 ✓ [PPS](https://www.pps.go.kr/kor/bbs/view.do?key=00634&bbsSn=2506270010)).**
- 총 계약 **225.1조원**(+7.9%, 최고치), GDP의 ~9%. 추세: 175.8조(2020)→184.2(2021)→196(2022)→208.6(2023)→225.1(2024).
- 기업규모별: **중소기업 142.1조(63.1%)** / 중견 34.8조(15.5%) / 대기업 31.5조(14.0%) / 기타 16.7조(7.4%).
- 기관별: 지자체 94.1조(41.8%) / 공공기관 80.5조(35.8%) / 국가기관 50.5조(22.4%).
- 업종별: 공사 85.7조(38.1%) / 물품 84.3조(37.4%) / 용역 55.1조(24.5%).
- **KONEPS(나라장터)**: 거래 145.1조=64.5%(비중 상승 60.8%→64.5%, 2021→24). 연 **전자입찰공고 49만건**, 등록기업 **57.2만**, 이용기관 6.9만, 전자계약 127만건/년([연혁](https://www.pps.go.kr/kor/content.do?key=01230) ✓).

**발주 프로세스·규칙.** 협상에 의한 계약 기본 배점 기술 80/가격 20, SW·정보시스템은 **기술 90/가격 10**([기재부 계약예규 제721호](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000086294) △); 협상적격자=기술배점 **85% 이상**([기재부](https://www.law.go.kr/admRulLsInfoP.do?admRulSeq=2100000028030) △); 섹션별 배점 ±10 가감. SW진흥법 제안보상: 예산 **20억 이상** 사업에 총액 13/1000(~1.3%)([미래부 고시 2016-108](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000061732) △).

**정부 AI 전환 (변곡점, 전부 buyer-side).**
- **공공SW AI 책임형 발주관리 플랫폼**(ppsai.kr) 구축 17.8억, RFP 생성/검토 **30~40%↓**, 행정비용 20~25%↓([etnews](https://www.etnews.com/20260522000281) ✓, [ddaily](https://www.ddaily.co.kr/page/view/2026052614231514644) ✓).
- **지능형 예정가격 작성지원시스템** 2026-04-30 개시(발주기관 연 8,000건 물품계약용)([Daum/파이낸셜](https://v.daum.net/v/20260430111751320) ✓).
- **차세대 나라장터** 955억, **2027년 지능형 공공조달 플랫폼** 통합 목표([etnews](https://www.etnews.com/20260522000281) ✓). → 전부 발주기관 편, 입찰기업용 RFP 이해는 미개척.

**해외 대조.** EU **~€2조/년(GDP 14%)**, 단일입찰 23.5%→41.8%·입찰자 5.7→3.2명으로 경쟁 붕괴([ECA SR28/2023](https://www.eca.europa.eu/ECAPublications/SR-2023-28/SR-2023-28_EN.pdf) ✓); UK 입찰 1건 준비비 **£5,800**([SmallBusiness UK](https://smallbusiness.co.uk/small-businesses-hit-hard-by-costly-public-sector-tender-process-2383643/) △); 미국 연방 **$773.7B**(FY24), 중소기업 몫 23.3%, 그러나 중소 공급자 수 **43% 감소**(148,778→85,013)([GovSpend](https://govspend.com/blog/federal-contract-awards-hit-773-68b-in-fy24-small-businesses-see-4b-increase/) ✓, [SBA/SmallGovCon](https://smallgovcon.com/reports/number-of-small-businesses-awarded-federal-government-contracts-has-dropped-12-7-in-four-years/) △). 공통 신호: 문서·절차 장벽이 SME를 밀어냄 → 도구 수요.

## II-2. 경쟁 맵 (공급측)

> 레이어: 발견(discovery) → 분석(analysis) → 제안생성(proposal_gen) → 데이터/마켓인텔. **빈 칸 = 분석/제안층의 국내 SME 대상.**

**국내 (B).**
| 서비스 | 레이어 | 기능 | 가격 | 규모 |
|---|---|---|---|---|
| Info21C | discovery | 공고+낙찰통계+사정율(특허) | 33K/월~330K/년 | since 2000, 회원 ~10만 ◐ |
| KBID | discovery | 공고+통계+Kport 투찰분석 | 유료(비공개) | 매출 35.7억, 31명 ✓ |
| G2B Plus | discovery(무료) | 25개 자체조달기관 통합검색 | 무료+9,900/월 | 3,456만 레코드 ✓ |
| DeepBID | discovery(무료) | 공고+기업/기관분석+투찰추천 | 무료 | (주)지에이시스템 ✓ |
| JodAL.AI | discovery | **첨부파일 검색**+10분갱신 | 미공개 | 운영 지란지교(매출 156억) ◐ |
| BidPro | discovery+예측 | 맥 예측엔진+입찰대행 | 77만~1,320만 / 대행 2.2% | 매출 7.6억, 6명(2002~) ✓ |
| BidTokTok | market_intel | 모바일 투찰가 분석 | 건당 유료 | 앱스토어 delisted ◐ |
| GOBID | market_intel | 사정률 예측(성공보수) | **낙찰 시 2.0%** | 2021~, 자칭 낙찰률 0.76% ◐ |
| **CLIWANT** | **analysis** | **HWP RAG·독소조항·자격·가격** | 300만~1,000만/년 | **Pre-A 20억, 고객 60~70, 미국 피벗** ✓ |
| 한국조달AI연구소 | analysis(**buyer**) | RAG+TAG 기술평가·RFP생성 | PoC | 그린다AI 등 컨소시엄(2025~) ✓ |
| MyBidWise | proposal_gen | 제안서 자동생성 | 자칭 | 47개사 사용(자칭) ✓ |
| SK AX Proposal AI | proposal_gen | RFP↔제안서 cross-check | 엔터프라이즈 | SK C&C 매출 2.6조 ✓ |

**해외 (C) — 레퍼런스.** AutogenAI $65.3M·270+고객(생성)·G2 4.4 ✓; GovDash $40M·컴플라이언스매트릭스+인용(생성) ✓; Procurement Sciences $40M·HigherGov 인수 ✓; Vultron $22M→**pWin에 흡수**(consolidation) ✓; GovWin IQ(Deltek) $12~42K+/년·"insurance for big, burden for small"(마켓인텔) ✓; Loopio(~$200M)·Responsive(~$27M)·"answers are usually wrong"·라이브러리 유지부담(제안응답) ✓; 유럽 Altura(€11M)·Brainial(**배점추출**)·Tendium(1,395고객)·Stotles($21.6M)·Tussell(데이터) ✓.

**빈 칸(공백).** 국내에서 **RFP 문서 분석층**을 파는 곳은 CLIWANT(범용·미국 피벗)와 한국조달AI연구소(발주기관용)뿐. **입찰기업용, 국내 SW/IT RFP 배점·자격 정밀 추출 + 부적격 자가진단**은 비어 있다 = 추천 1순위의 근거.

---

# PART III — 근거 데이터 카탈로그

## III-1. 핵심 숫자표 (항목 / 숫자 / 해석 / 출처)

> 해석 열은 후처리 작성(현 41개 JSON엔 interpretation 필드 없음 — 다음 리서치부터 필드 수집). 접근확인: ✓/△/◐/✗.

| 항목 | 숫자 | 해석 | 출처 |
|---|---|---|---|
| 공공조달 총액 | 225.1조 (2024, +7.9%) | 국가경제급 수요, 최고치 갱신 | [조달청](https://www.pps.go.kr/kor/bbs/view.do?key=00634&bbsSn=2506270010) ✓ |
| GDP 대비 | ~9% (2024) | 시장 자체가 거시급 | [korea.kr](https://www.korea.kr/briefing/pressReleaseView.do?newsId=156718224) ✓ |
| 중소기업 몫 | 142.1조=63.1% (2024) | 타깃 사용자=시장 다수 | [조달청](https://www.pps.go.kr/kor/bbs/view.do?key=00634&bbsSn=2506270010) ✓ |
| KONEPS 거래 | 145.1조=64.5% (2024) | 구조화 데이터 한 곳 집중 | [조달청](https://www.pps.go.kr/kor/bbs/view.do?key=00634&bbsSn=2506270010) ✓ |
| 전자입찰공고 | 49만건/년 (2023) | 개인이 triage 불가한 firehose | [연혁](https://www.pps.go.kr/kor/content.do?key=01230) ✓ |
| 등록기업 | 57.2만 (2023) | 대부분 SME, 도구 부재 | [연혁](https://www.pps.go.kr/kor/content.do?key=01230) ✓ |
| 일 분석 RFP | ~3,000/일 (2024) | 사람이 다 못 읽음 | [Google for Startups](https://blog.google/intl/ko-kr/company-news/outreach-initiatives/gfs-yearbook-cliwant/) ✓ |
| **부적격율(PC 표본)** | **53.3%** (2025-26) | 투찰 절반이 심사문턱도 못 넘음→사전진단 수요 | [조달경제](https://www.jodaleconomy.com/news/articleView.html?idxno=2169) ✓ |
| 부적격 중 가격미달 | 89.4% (2025-26) | 낙찰하한 계산실패가 탈락 주원인 | [조달경제](https://www.jodaleconomy.com/news/articleView.html?idxno=2169) ✓ |
| 유찰율(PC 물품) | 44.9% (2025-26) | 발주자도 재공고 부담 | [조달경제](https://www.jodaleconomy.com/news/articleView.html?idxno=2169) ✓ |
| 대형건설 유찰 | ~71% (2024) | 고액 사업조차 반복 실패 | [대한건설협회](https://www.news1.kr/realestate/general/5810087) ✓ |
| 동일가격 투찰 | 68.96% (2026-03, 0.9%→) | 규칙 못 읽어 브로커 lottery 폭증 | [기재부/뉴스핌](https://www.newspim.com/news/view/20260721000301) ✓ |
| **행정문서 AI-불가독** | **91.1%** (2025) | RAG 최대 기술장벽=HWP 파싱 | [위성곤 의원실](https://v.daum.net/v/20251013175850275) ✓ |
| 파싱이 RAG품질 좌우 | "80% 파서서 결정" (2025) | 파싱=해자 | [KoreaDeep](https://www.koreadeep.com/blog/hwp-parser-ai) ◐[벤더] |
| ParseBench 천장 | 84.9% (2026) | 파싱 미해결=진입장벽이자 기회 | [LlamaIndex](https://www.llamaindex.ai/blog/parsebench) ✓ |
| Upstage 파싱 단가 | $0.01/page (2025) | buy 옵션·비용 관리가능 | [Upstage](https://www.upstage.ai/products/document-parse) ✓[벤더] |
| 제안서 1건 소요 | 평균 30h (2025) | 반주 묶임, task-0=요구정리 | [CLIWANT](https://blog.cliwant.com/ibcal-damdangjaga-jaggu-toesahaeyo-iyureul-alryeodeurilggayo/) ✓[벤더] |
| RFP 내 요구 산재 | 수십 개/건 (2025) | 첫 작업이 '뭘 해야하나 정리' | [CLIWANT](https://blog.cliwant.com/ibcal-damdangjaga-jaggu-toesahaeyo-iyureul-alryeodeurilggayo/) ✓[벤더] |
| 삼성SDS 제안팀 | 33명, 1건 10~50명 (2003) | 대기업엔 부서, SME엔 0 | [아이뉴스24](http://www.inews24.com/view/95674) ✓ |
| 제안비 매출비중 | 1~2% (대기업) | 부서급 비용 라인 | [아이뉴스24](http://www.inews24.com/view/95674) ✓ |
| '제안서작성' 채용 | ~2,829건 (2026) | 제안 역량이 상시 수요 | [JobKorea](https://www.jobkorea.co.kr/Search?stext=%EC%A0%9C%EC%95%88%EC%84%9C%EC%9E%91%EC%84%B1) ✓ |
| 불명확 RFP 무상재작업 | 50% of 111사 (2012) | RFP 부실→리스크 탐지 수요 | [KOSA/SPRi](https://spri.kr/download/5378) ✓ |
| 과업변경 대가지급률 | 4.4% 중앙 (2011) | 과업변경 비용 전가 | [SPRi](https://spri.kr/download/5378) ✓ |
| 브로커 수수료 | 낙찰액 3~5% (2026) | WTP 정량, 정보독점 대행 | [언론 단독](https://v.daum.net/v/KnTAM81mhw?f=p) ✓ |
| 브로커 회원 | ~4,000명 (2026) | 자력 불가 SME 규모 | [언론 단독](https://v.daum.net/v/KnTAM81mhw?f=p) ✓ |
| 브로커 적발 | 0건 (2026) | 회색시장→합법 대체 여지 | [언론 단독](https://v.daum.net/v/KnTAM81mhw?f=p) ✓ |
| 컨설팅 비용 | 월 수백만원~AI 1천만원대 | 고비용 불만 "돈 냈는데 성과 없다" | [CLIWANT](https://blog.cliwant.com/bidding-consulting-comparison-ai-vs-traditional/) ✓[벤더] |
| 공공조달길잡이 | 36명→2,000상담→260진입 | 인간 병목, 자동화 여지 | [머니투데이](https://www.mt.co.kr/policy/2025/10/28/2025102813262913576) ✓ |
| CLIWANT Pre-A | 20억(2024), 고객60~70, 매출5억 | 국내 RAG-on-RFP 검증+WTP | [더벨](https://www.thebell.co.kr/free/content/ArticleView.asp?key=202412111328413360107617&lcode=00) △ |
| AutogenAI | $65.3M, 270+고객 (2026) | 글로벌 생성층 수요 검증 | [VentureBeat](https://venturebeat.com/ai/automated-proposal-writing-startup-autogenai-raises-39-5m-from-salesforce-ventures-and-others) △ |
| GovDash | $40M, 매트릭스+인용 (2026) | 배점/컴플 추출=핵심기능 검증 | [SiliconANGLE](https://siliconangle.com/2026/01/15/govdash-secures-30m-expand-ai-driven-government-contracting-software/) ✓ |
| GovWin 가격 | $12~42K+/년 (2026) | SME 배제, 상위층 창 | [Fed-Spend](https://fed-spend.com/blog/govwin-iq-pricing-2026-deltek-cost-alternatives) ✓ |
| 협상계약 배점 | 기술90/가격10 (SW, 2024) | 추출 대상=명확한 규칙 스키마 | [법령](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000086294) △ |
| 협상적격 기준 | 기술배점 85%↑ (2024) | 자가진단 임계값 명확 | [법령](https://www.law.go.kr/admRulLsInfoP.do?admRulSeq=2100000028030) △ |
| legal RAG 환각율 | 17~34% (2024) | 인용·거절 가드 필수 | [Stanford](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-or-more-benchmarking-queries) ✓ |
| ppsai.kr | 17.8억, RFP생성 30~40%↓ (2026) | 정부는 buyer-side→bidder 창 열림 | [etnews](https://www.etnews.com/20260522000281) ✓ |
| 미 연방조달 | $773.7B, 중소 43%↓ (FY24) | 문서장벽이 SME 밀어냄=도구수요 | [GovSpend](https://govspend.com/blog/federal-contract-awards-hit-773-68b-in-fy24-small-businesses-see-4b-increase/) ✓ |

## III-2. 링크 라이브러리 (태그별 대표; 전체 488개는 `_digest/links.md`)

**[정부/통계]** 조달청 [2024 통계연보](https://www.pps.go.kr/kor/bbs/view.do?key=00634&bbsSn=2506270010) · [나라장터 연혁](https://www.pps.go.kr/kor/content.do?key=01230) · [e-나라지표](https://www.index.go.kr/unity/potal/main/EachDtlPageDetail.do?idx_cd=1374) · [KONEPS OpenAPI](https://www.data.go.kr/data/15129394/openapi.do) · [계약정보 API](https://www.data.go.kr/data/15129427/openapi.do) · 위성곤 국감 [91.1%](https://v.daum.net/v/20251013175850275) · 부적격 [조달경제](https://www.jodaleconomy.com/news/articleView.html?idxno=2169) · 동일가격 [뉴스핌](https://www.newspim.com/news/view/20260721000301) · SPRi [공공SW/RFP](https://spri.kr/download/5378) · [ppsai.kr etnews](https://www.etnews.com/20260522000281) · [지능형 예정가격](https://v.daum.net/v/20260430111751320)
**[규제/법령]** [협상계약체결기준(계약예규 721호)](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000247066) · [협상·제안서평가 세부기준](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000086294) · [SW 제안서 보상 운영규정](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000061732)
**[기업-국내]** [CLIWANT 블로그](https://blog.cliwant.com/) · [ZDNet CLIWANT](https://zdnet.co.kr/view/?no=20240819004849) · [Info21C](https://www.info21c.net/) · [KBID/Kport ETNews](https://www.etnews.com/20260413000149) · [G2B Plus](https://www.g2bplus.kr/service_goods_list.php) · [DeepBID](https://deepbid.com/) · [JodAL.AI](https://jodal.ai/) · [GOBID/Tech42](https://www.tech42.co.kr/) · [한국조달AI연구소](https://govt.ai.kr/) · [MyBidWise](https://mybidwise.com/) · [SK AX Proposal AI](https://www.skax.co.kr/ax-services/proposal-ai)
**[기업-해외]** [AutogenAI](https://autogenai.com/) · [GovDash Series B](https://siliconangle.com/2026/01/15/govdash-secures-30m-expand-ai-driven-government-contracting-software/) · [GovWin 가격](https://fed-spend.com/blog/govwin-iq-pricing-2026-deltek-cost-alternatives) · [Procurement Sciences](https://www.procurementsciences.com/blog/series-b) · [Vultron→pWin](https://www.pwin.ai/resource/pwin-ai-acquires-vultrons-customer-portfolio/) · [Altura Series A](https://altura.io/en/blog/series-a-announcement) · [Brainial](https://siliconcanals.com/brainial-raises-fresh-funding/) · [Stotles](https://www.actoncapital.com/news/stotles-raises-13-million-series-a-to-help-suppliers-succeed-in-the-new-age-of-government-efficiency)
**[기술/파싱]** [ParseBench(LlamaIndex)](https://www.llamaindex.ai/blog/parsebench) · [Upstage Document Parse](https://www.upstage.ai/products/document-parse) · [KoreaDeep HWP](https://www.koreadeep.com/blog/hwp-parser-ai) · [Polaris HWP 파싱](https://www.polarisoffice.com/business-blog/claude-skill-for-hwp-parsing) · [Velog 공문서 RAG](https://velog.io/@pjg9606/한국-공문서-RAG의-첫-번째-벽-HWP-파싱-해결법) · [Stanford 법률RAG 환각](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-or-more-benchmarking-queries) · [LRAGE(한국어 법률)](https://arxiv.org/html/2504.01840v1) · [LegalBench-RAG](https://arxiv.org/abs/2408.10343)
**[리뷰/VoC]** [Reddit r/govcon](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/) · [Capterra GovWin](https://www.capterra.com/p/154858/GovWin-IQ/reviews/) · [Capterra Loopio](https://www.capterra.com/p/133042/Loopio/reviews/) · [Blind 제안서](https://www.teamblind.com/kr/post/%EC%A0%9C%EC%95%88%EC%84%9C%EB%A5%BC-%EB%A7%8C%EB%93%9C%EB%8A%94-%EA%B1%B4-%EC%96%B4%EB%A0%A4%EC%9B%8C-uTWiwpNz) · [Remember 제안노하우](https://community.rememberapp.co.kr/post/90771) · [전기넷 낙찰코칭](https://www.jungi.net/consulting/review)
**[기사/사건]** 브로커 [언론 단독](https://v.daum.net/v/KnTAM81mhw?f=p) · [대한경제 종심제 붕괴](https://www.dnews.co.kr/uhtml/view.jsp?idxno=202601081433098030893) · [공공발주 AI시대](https://www.dnews.co.kr/uhtml/view.jsp?idxno=202510081144531420108) · [KMA 슈퍼컴 4회 유찰](https://www.jodaleconomy.com/news/articleView.html?idxno=2169)

## III-3. 기사/보도자료 (사건 중심)

- **종심제 붕괴/브로커** — 동일가격 투찰 0.9%(2020)→68.96%(2026-03), 업평예가율 조작 정황([뉴스핌](https://www.newspim.com/news/view/20260721000301) ✓, [대한경제](https://www.dnews.co.kr/uhtml/view.jsp?idxno=202601081433098030893) ✓); 브로커 3~5% 수수료·회원 4,000·적발 0건([언론 단독](https://v.daum.net/v/KnTAM81mhw?f=p) ✓).
- **유찰/부적격 급증** — PC조달 부적격 53.3%(DRAM 쇼크로 단가 +47.4%), KMA 슈퍼컴 4회 연속 유찰([조달경제](https://www.jodaleconomy.com/news/articleView.html?idxno=2169) ✓); 대형건설 유찰 ~71%([News1](https://www.news1.kr/realestate/general/5810087) ✓).
- **공공발주 AI 시대** — 대기업/중견 임원 "AI가 비용 분석하니 데이터로 강점 입증해야, 제안서 작성법 완전히 바꿔야"([대한경제](https://www.dnews.co.kr/uhtml/view.jsp?idxno=202510081144531420108) ✓).
- **정부 정책** — ppsai.kr(17.8억), 지능형 예정가격(2026-04), 차세대 나라장터(955억), 2027 통합 플랫폼([etnews](https://www.etnews.com/20260522000281) ✓).
- **해외 M&A/자금** — GovDash $30M Series B, Procurement Sciences+HigherGov 인수, Vultron→pWin 흡수, Altura+Tendara 인수 — GovCon AI 급속 통합.

## III-4. 기술/채용/API

- **문서포맷 현실**: HWP=독자 OLE2 바이너리(HWPX=XML/ZIP, KS X 6101). 중첩표·병합셀·다단·차트로 파서가 깨짐; 오픈소스 5.x부터, 3.x 없음([Polaris](https://www.polarisoffice.com/business-blog/claude-skill-for-hwp-parsing) ◐). 벤더: Upstage($0.01/page, TEDS 93.48), KoreaDeep(정확도 97.3% 자칭·ParseBench VLM 1위 76.4), Hancom, Polaris.
- **공공 API/데이터**: KONEPS 입찰공고/낙찰/계약 OpenAPI 무료(1,000~10,000콜/일, 20년 계약이력, 공고↔계약 조인키)([data.go.kr](https://www.data.go.kr/data/15129394/openapi.do) ✓). 개방데이터로 ~80개사 연 500억 부가가치(2017 조달청 ✓) — 다운스트림 유료 제품 성립 근거.
- **경쟁 기술스택**: AutogenAI=멀티LLM+커스텀 언어엔진(고객 코퍼스 RAG)+Gamma Review 컴플라이언스; GovDash=Sections L/M/C 문장단위 shredding+인용 grounding+FedRAMP; 한국조달AI연구소=RAG+TAG 하이브리드+멀티에이전트(HWP/PDF, **발주기관용**). → 우리 스택(local e5 + Chroma + gpt-5-mini)과 정합.
- **채용=부서 실증**: 잡코리아 '제안서작성' ~2,829건·'SI 제안서' 284건([JobKorea](https://www.jobkorea.co.kr/Search?stext=%EC%A0%9C%EC%95%88%EC%84%9C%EC%9E%91%EC%84%B1) ✓); 쉬플리코리아(제안컨설팅) 45명·매출 36억([Saramin](https://www.saramin.co.kr/zf_user/company-info/view-inner-finance/csn/RDhpdmdsTndIM3VPcTVYblN4OFlHUT09/company_nm/%EC%89%AC%ED%94%8C%EB%A6%AC%EC%BD%94%EB%A6%AC%EC%95%84(%EC%A3%BC)) ✓); APMP 회원 12,900+·한국챕터([CLIWANT](https://blog.cliwant.com/what-is-apmp-proposal-management/) ◐).
- **RAG 함정(설계 경고)**: dense retriever가 법률 도메인서 BM25에 밀림(도메인 적응 필수), 시맨틱 청킹 이득 미미(<2%), 한국어 법률 RAG는 과목별 -15%도([LRAGE](https://arxiv.org/html/2504.01840v1) ✓) → 도메인 적응 + 하이브리드 검색 + 평가지표 필수.

## III-5. 리뷰/VoC (원문 인용 — 국내외)

**국내.**
- "적절한 근거자료를 검색하느라 **2시간 이상**… 제안서 진짜 머리 터져" — [Blind](https://www.teamblind.com/kr/post/%EC%A0%9C%EC%95%88%EC%84%9C%EB%A5%BC-%EB%A7%8C%EB%93%9C%EB%8A%94-%EA%B1%B4-%EC%96%B4%EB%A0%A4%EC%9B%8C-uTWiwpNz) ✓
- "물어볼 기획 팀도 없고 저 혼자 기존 문서를 보고 전부 짜내야… 개발 업무를 다른 개발자에게 넘긴 적도" — [Blind](https://www.teamblind.com/kr/post/%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80-%EC%A0%9C%EC%95%88%EC%84%9C-%EC%9E%91%EC%84%B1%ED%95%98%EB%8A%94-%ED%9A%8C%EC%82%AC-aFRJV56N) ✓
- "10여명 제안팀으로 **2-3주 밤새며**… 야근·주말 어쩔 수 없었다" — [Remember](https://community.rememberapp.co.kr/post/90771) ✓
- "올 한해 용역 나왔음에도 **한건도 안 되어** 정말 힘들었다" / "낙찰 못하니 입찰대행업체도 우습게 본다" — [전기넷](https://www.jungi.net/consulting/review) ✓, [jungi.net](https://www.jungi.net/community/board/review_detail?code=69608) ✓
- "HWP는 Word보다 **열 배 어려웠다… 진짜 헬**, 표는 처음부터 다 만들어야" — [Brunch/inline AI](https://brunch.co.kr/@1212ac31a500435/329) [벤더] ✓
- "나라장터 검색이 매우 부정확… 공고 검색에 업무시간 낭비" — [KCI 정보화연구](https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART002883147) ✓ (비벤더 학술)

**해외.**
- "a required document **buried on page 34**… we leave money on the table by not responding" — [Reddit](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/) ✓
- "the real nightmare… conflicting clauses between **section L and M** that lead to an automatic DQ" — [Reddit](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/) ✓
- "I'd want it to **show the exact page/section**… Otherwise it's hard to trust on a live bid" — [Reddit](https://www.reddit.com/r/govcon/comments/1tkdst3/we_didnt_respond_to_every_rfp_because_it_took_too/) ✓
- "buried in **80 pages nobody reads till the night before bid day. By then it's too late**" — [Reddit](https://www.reddit.com/r/govcon/comments/1umr5zf/i_built_a_tool_that_reads_publicworks_rfps_and/) ✓
- "how small shops handle this **without a compliance department**… Nobody LOL" — [Reddit](https://www.reddit.com/r/govcon/comments/1uphsvk/small_shops_with_itar_flowdowns_and_no_compliance/) ✓
- "**The answers are usually wrong**" (Loopio Magic) / "not smarter because of prior outcomes" — [Capterra/AutoRFP](https://autorfp.ai/blog/loopio-reviews) ◐, [Tribble](https://tribble.ai/blog/responsive-rfpio-review-pricing-features-limitations-2026/) ◐
- "The price makes it very hard for small companies to use GovWin" — [Capterra](https://www.capterra.com/p/154858/GovWin-IQ/reviews/) ✓

## III-6. 산업/규제/시장구조

- **밸류체인**: 발주기관(연 225조) → 입찰기업(57.2만·SME 63%) ← 정보서비스(Info21C/KBID/G2B Plus/DeepBID/JodAL) · 예측(GOBID/BidTokTok/BidPro) · 분석(CLIWANT) · 브로커/컨설팅(3~5% 수수료·월 수백만원) · 대기업 제안팀/쉬플리(수주율 87%). RFP **문서 이해**층은 국내 공백.
- **규제(추출 대상 스키마)**: 협상계약 기술80/가격20·SW 90/10, 협상적격 기술 85%↑, 배점 ±10 가감([법령](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000247066) △); SW진흥법 제안보상 20억↑·13/1000([고시](https://www.law.go.kr/LSW/admRulLsInfoP.do?admRulSeq=2100000061732) △); 국가계약법 부적격(가격 하한 미달)·과업변경 규정. → 제품이 RFP에서 추출·검증해야 할 대상.
- **변곡점**: 정부 AI 전환(ppsai.kr 등, buyer-side, 2027 통합), 공공발주 AI 평가로 "제안서 작성법을 바꿔야"([대한경제](https://www.dnews.co.kr/uhtml/view.jsp?idxno=202510081144531420108) ✓); 해외 GovCon AI 급속 통합(Vultron→pWin).

## III-7. 접속실패/사용제외 메모 (정직성)

- **✗ blocked**: 중기부 2024 판로지원 실적(.hwpx 첨부 안 파싱), THE VC CLIWANT 재무(Pro 게이트), BidTokTok 앱스토어 평점(404), KBID 구독 가격(로그인/견적), 나라장터 전국 유찰율(동적조회만), 개방데이터 갱신 경제효과.
- **◐ snippet-only (재확인 필요)**: GOBID 낙찰률 0.76%(벤더 자칭), Info21C 회원 10만·낙찰 4만건(자칭), pyhwp 62x 느림(오픈소스 자칭), UK £5,800(1차 PDF 차단).
- **[벤더] 주의**: CLIWANT/GOBID/MyBidWise/AutogenAI/GovDash 성과수치는 자사 마케팅 — 독립감사 없음. VoC의 비벤더(Reddit·Capterra·Blind·KCI)를 신뢰 우선.
- **오래됨 주의**: 삼성SDS 제안팀(2003)·과업변경 대가지급률(2011)·KOSA 50%(2012) — 구조적 사실이나 최신치 아님.

---

*원본 데이터: `results/*.json` (41). 이 보고서는 그 위의 후처리 산출물. 원자료 전량 카탈로그가 필요하면 `/research-report`로 `report.md` 재생성 가능.*
