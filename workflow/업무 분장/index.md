---
title: 업무 분장
tags:
  - 업무분장
  - index
---

# 업무 분장 — 일일 업무 보고

매일 **본인 폴더에서 "새 노트 만들기"** 를 하면 일일 보고 템플릿이 자동 적용됩니다.
오늘 한 일(Done) / 진행 중(Doing) / 다음 할 일(Next) / 블로커만 짧게 적으면 됩니다.

## 팀원별 보고 폴더

- [[workflow/업무 분장/황인홍(PM)/index|황인홍(PM)]]
- [[workflow/업무 분장/김범진(데이터 처리)/index|김범진(데이터 처리)]]
- [[workflow/업무 분장/전재완(Retrieval)/index|전재완(Retrieval)]]
- [[workflow/업무 분장/이찬울(Generation)/index|이찬울(Generation)]]
- [[workflow/업무 분장/박종선/index|박종선]]

## 최종 정리

- [[workflow/업무 분장/최종 정리|최종 정리]] — 일일 보고를 LLM이 종합·갱신하는 문서
- 갱신 방법: Claude Code에서 **"업무 보고 정리해줘"** (스킬 `work-report-digest` 실행)

## 규칙

- 하루 1개, 본인 폴더에만 작성 — 파일명은 자동으로 `YYYY-MM-DD-이름(역할)`이 됩니다
- 일일 보고 원본은 나중에 고치지 않기 — 종합·수정은 [[workflow/업무 분장/최종 정리|최종 정리]]에서
- 새 팀원: 이 폴더 아래에 본인 폴더를 만들고, Templater 설정 → Folder Templates에 `workflow/업무 분장/이름(역할)` → `_templates/업무 분장.md` 한 줄 추가

관련: [[프로젝트 개요]]의 "역할 분담 (권장 가이드)" 참고
