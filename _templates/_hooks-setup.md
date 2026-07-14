<%*
// tp.system.exec_command는 Templater에 존재하지 않는 API — child_process로 직접 실행한다
const vaultPath = app.vault.adapter.basePath;
try {
  const { execSync } = require("child_process");
  execSync(`git -C "${vaultPath}" config core.hooksPath .githooks`);
  console.log("[hooks-setup] core.hooksPath=.githooks 설정 완료");
} catch(e) {
  console.error("[hooks-setup] git hooks 설정 실패:", e);
}
%>
