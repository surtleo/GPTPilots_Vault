<%*
// 오늘 KDI 노트가 이미 있으면 중복 생성하지 않고 기존 노트를 연다
const kdiName = tp.date.now("YYYY-MM-DD") + "-KDI";
const existing = app.vault.getAbstractFileByPath(tp.file.folder(true) + "/" + kdiName + ".md");
if (existing) {
  new Notice("오늘 KDI 노트가 이미 있어 기존 파일을 엽니다.");
  await app.workspace.getLeaf().openFile(existing);
  await app.vault.delete(tp.config.target_file);
  return;
}
await tp.file.rename(kdiName);
%>
---
type: kdi
date: <% tp.date.now("YYYY-MM-DD") %>
member: <% tp.file.folder() %>
---

# KDI — <% tp.date.now("YYYY-MM-DD") %>

### ✅ 한 것 (Done)

<span style="color:#aaa; font-style:italic">최근까지 끝낸 작업. 결과물(노트·커밋·파일) 링크가 있으면 같이.</span>

- 

---

### 🔄 하고 있는 것 (Doing)

<span style="color:#aaa; font-style:italic">지금 붙잡고 있는 작업과 현재 상태 한 줄.</span>

- 

---

### 📋 해야 할 것 (To-Do)

<span style="color:#aaa; font-style:italic">다음에 할 일, 급한 순서대로.</span>

- [ ] 
- [ ] 
- [ ] 

---

### 📍 어디까지 왔나 (진행 상황)

<span style="color:#aaa; font-style:italic">맡은 작업 기준으로 지금 위치. 퍼센트나 단계(설계/구현/검증)로.</span>

| 작업 | 진행률 | 비고 |
| --- | --- | --- |
|  | % |  |

---

### 🧱 막힌 것 / 메모 (선택)

<span style="color:#aaa; font-style:italic">블로커, 기다리는 것, 나중에 볼 링크 등.</span>

- 
