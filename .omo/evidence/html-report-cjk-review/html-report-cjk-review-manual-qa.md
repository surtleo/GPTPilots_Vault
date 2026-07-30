# Manual QA — HWP 파싱 타임아웃 대응 HTML 보고서

## manualQa

### surfaceEvidence

| scenario id | criterion reference | surface | exact invocation | verdict | artifactRefs |
|---|---|---|---|---|---|
| web-desktop-full-report | desktop report is readable and visually complete | Web page, 1280px viewport | `chrome.exe --headless=new --disable-gpu --hide-scrollbars --no-sandbox --allow-file-access-from-files --window-size=1280,6200 --screenshot=".omo/evidence/html-report-cjk-review/fresh-desktop.png" "file:///C:/Users/종선/OneDrive/바탕 화면/ai11-2-m-project/GPTPilots_Vault/보고서/2026-07-29%20HWP%20파싱%20타임아웃%20원인%20및%20대응%20보고서.html"`; then `view_image` on `fresh-desktop.png` | PASS (desktop sections 01–08 and footer visible; Korean glyphs render) | `desktop-fresh`, `desktop-supplied` |
| web-mobile-top-current | current 390px rendering after responsive CSS edit | Web page, 390px viewport | `chrome.exe --headless=new --disable-gpu --hide-scrollbars --no-sandbox --allow-file-access-from-files --window-size=390,1800 --screenshot=".omo/evidence/html-report-cjk-review/current-mobile-top.png" "file:///C:/Users/종선/OneDrive/바탕 화면/ai11-2-m-project/GPTPilots_Vault/보고서/2026-07-29%20HWP%20파싱%20타임아웃%20원인%20및%20대응%20보고서.html"`; then `view_image` on `current-mobile-top.png` | REVISE (CJK phrase wrapping improved, but subtitle, §01 body/verdict, §02 note and §03 intro still meet/cut at right viewport edge) | `current-mobile-top`, `mobile-fresh` |
| web-mobile-full-current | current full-page mobile coverage | Web page, 390px viewport, 8000px capture | `chrome.exe --headless=new --disable-gpu --hide-scrollbars --no-sandbox --allow-file-access-from-files --window-size=390,8000 --screenshot=".omo/evidence/html-report-cjk-review/current-mobile-full.png" "file:///C:/Users/종선/OneDrive/바탕 화면/ai11-2-m-project/GPTPilots_Vault/보고서/2026-07-29%20HWP%20파싱%20타임아웃%20원인%20및%20대응%20보고서.html"`; then `view_image` on `current-mobile-full.png` | PASS for coverage only (sections 01–08/footer are present; top-level edge clipping still fails) | `current-mobile-full` |
| evidence-png-integrity | supplied and fresh captures are valid PNGs with expected dimensions | Image artifacts | PowerShell `System.Drawing.Image.FromFile` on supplied and fresh PNGs; dimensions checked | PASS (PNG signature/decoding valid; supplied 1280×6200 and 390×1800; fresh 1280×6200 and 390×1800) | `png-integrity` |

### adversarialCases

| scenario id | criterion reference | adversarial class | expected behavior | verdict | artifactRefs |
|---|---|---|---|---|---|
| adv-mobile-overflow-current | responsive layout | horizontal overflow / hidden overflow | all content stays within 390px; no text is cut; overflow is not masked | FAIL | `current-mobile-top`, `current-mobile-full`, `source-css-current`, `overflow-probe-output` |
| adv-cjk-wrap-current | Korean share-report readability | CJK semantic wrapping | Korean prose keeps natural word/phrase boundaries; only long code tokens break safely | REVISE (keep-all is better, but lines still reach/cut at viewport edge) | `current-mobile-top`, `source-css-current` |
| adv-mobile-coverage-current | complete responsive evidence | partial capture / skipped lower sections | mobile evidence covers sections 01–08 and footer | PASS | `current-mobile-full` |
| adv-desktop-glyphs | Korean readability | glyph loss / tofu / baseline clipping | Korean characters render as intact glyphs with no tofu or clipped baselines | PASS | `desktop-fresh`, `mobile-fresh` |
| adv-animation-state | static report | motion/interaction regression | no unexplained animation or required interaction is present | not_applicable — report is static HTML with no scripted interaction or animation | `source-html` |

### artifactRefs

| id | kind | description | path |
|---|---|---|---|
| desktop-fresh | screenshot | Fresh Chrome headless desktop render, full report | `.omo/evidence/html-report-cjk-review/fresh-desktop.png` |
| mobile-fresh | screenshot | Fresh Chrome headless 390px render; clipping visible and capture ends near §03 | `.omo/evidence/html-report-cjk-review/fresh-mobile.png` |
| current-mobile-top | screenshot | Current Chrome headless 390px top render after CSS edit | `.omo/evidence/html-report-cjk-review/current-mobile-top.png` |
| current-mobile-full | screenshot | Current Chrome headless 390px full-page render through section 08/footer | `.omo/evidence/html-report-cjk-review/current-mobile-full.png` |
| current-desktop | screenshot | Current Chrome headless 1280px render | `.omo/evidence/html-report-cjk-review/current-desktop.png` |
| desktop-supplied | screenshot | Supplied desktop preview | `보고서/hwp-parsing-report-desktop-preview.png` |
| mobile-supplied | screenshot | Supplied mobile preview | `보고서/hwp-parsing-report-mobile-preview.png` |
| png-integrity | inspection | PNG decode/dimension check via `System.Drawing.Image.FromFile` | `.omo/evidence/html-report-cjk-review/png-integrity.txt` |
| source-html | source | Standalone report HTML inspected for DOM, typography, responsive CSS | `보고서/2026-07-29 HWP 파싱 타임아웃 원인 및 대응 보고서.html` |
| source-css | source excerpt | Previous capture build used blanket `word-break: break-all` and overflow clipping | `보고서/2026-07-29 HWP 파싱 타임아웃 원인 및 대응 보고서.html` |
| source-css-current | source excerpt | Current mobile rules use `overflow-x: hidden`, `width:100vw`, and `word-break: keep-all; overflow-wrap: break-word` | `보고서/2026-07-29 HWP 파싱 타임아웃 원인 및 대응 보고서.html` |
| overflow-probe-output | diagnostic | Chrome geometry probe identifies `main.page` width 390px vs body client width 375px; `pre` overflow is contained | `.omo/evidence/html-report-cjk-review/overflow-probe-output.txt` |

## Verdict

**REVISE.** Desktop presentation is clean and Korean glyphs are intact, but the 390px mobile rendering has blocking right-edge clipping and semantically poor CJK wrapping. Replace blanket `word-break: break-all`, resolve the over-wide descendant(s) so `scrollWidth <= clientWidth`, remove overflow masking as the fix, and recapture the full mobile page through section 08 plus the footer.
