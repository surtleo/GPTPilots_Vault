---
title: 업무 분장
tags:
  - 업무분장
  - index
---

# 업무 분장

각자 **무엇을 하고 있고(Doing) / 해야 하고(To Do) / 했는지(Done)** 를 본인 파일에 기록하는 곳입니다.

## 팀원별 분장표

```dataview
TABLE WITHOUT ID file.link as "팀원", file.mtime as "최근 갱신" FROM "workflow/업무 분장" WHERE file.name != "index" SORT file.name ASC
```

## 작성 규칙

- **본인 파일만 수정**합니다 (파일명: `이름(역할).md`)
- 항목은 체크박스째 옮깁니다: 📋 To Do → 🔄 Doing → ✅ Done
- 갱신할 때 frontmatter의 `updated` 날짜도 함께 바꿔주세요
- 새 팀원은 이 폴더에 `이름(역할).md` 파일을 만들고 `_templates/업무 분장` 템플릿을 적용하면 됩니다

관련: [[프로젝트 개요]]의 "역할 분담 (권장 가이드)" 참고
