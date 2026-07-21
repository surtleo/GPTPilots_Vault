---
title: 업무 보고 — 전재완(Retrieval)
tags:
  - 업무분장
---

# 업무 보고 — 전재완(Retrieval)

> 이 폴더에서 "새 노트 만들기"를 하면 일일 보고 템플릿이 자동 적용됩니다.

```dataviewjs
// @prerender from="workflow/업무 분장/전재완(Retrieval)" list
const pages = dv.pages('"workflow/업무 분장/전재완(Retrieval)"')
  .where(p => p.file.name !== "index")
  .sort(p => p.file.name, 'desc');
dv.table(["파일", "날짜"], pages.map(p => [
  dv.fileLink(p.file.path, false, p.file.name),
  p.file.name.slice(0, 10)
]));
```
