# Manual QA — final3 HTML report CJK/wrapping review

## manualQa

### surfaceEvidence

| scenario id | criterion reference | surface | exact invocation | verdict | artifactRefs |
|---|---|---|---|---|---|
| desktop-final3 | desktop report remains complete at 1440px; byte string stays one line | Web page screenshot | `view_image` on `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-desktop-final3.png` | PASS — centered page, all sections/callouts/tables/cards/footer visible; no clipped glyphs; byte string one line | `desktop-final3`, `geometry-final3`, `source-html` |
| mobile-final3 | mobile report is complete at 390px with natural CJK wrapping | Web page screenshot | `view_image` on `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-mobile-final3.png` | PASS — full report through footer; Korean phrases remain intact; no right-edge clipping or horizontal spill | `mobile-final3`, `geometry-final3`, `source-html` |
| mobile-code-final3 | both code blocks remain contained after responsive wrapping | Web page screenshot + supplied geometry | `view_image` on mobile-final3; objective `scrollWidth/clientWidth = 290/290` for both blocks | PASS — dark code blocks fit cards and wrap without clipping/scrollbar | `mobile-final3`, `geometry-final3`, `source-html` |
| mobile-table-cards-final3 | tables become labelled cards on mobile | Web page screenshot | `view_image` on mobile-final3; inspect section 04 and section 06 card rows | PASS — data-label headings and separators remain visible and ordered | `mobile-final3`, `source-html` |

### adversarialCases

| scenario id | criterion reference | adversarial class | expected behavior | verdict | artifactRefs |
|---|---|---|---|---|---|
| adv-cjk-midword-final3 | CJK semantic wrapping | Korean/Hangul mid-word fragmentation | Keep Korean words/phrases together; no orphaned particles or clipped punctuation | PASS — no mid-syllable or semantic phrase breaks observed | `mobile-final3`, `source-html` |
| adv-horizontal-overflow-final3 | responsive layout | horizontal overflow / hidden clipping | Content width must equal viewport and no text/code may be cut | PASS — 390/390, overflow 0, visible edges clean | `mobile-final3`, `geometry-final3` |
| adv-code-overflow-final3 | responsive code presentation | long code/token overflow | Code remains readable within its card, wrapping safely when needed | PASS — both blocks 290/290 and visibly contained | `mobile-final3`, `geometry-final3`, `source-html` |
| adv-table-label-loss-final3 | responsive table semantics | table-to-card label loss | Every mobile value retains a visible header/data label | PASS — section 04 and 06 cards retain labels and row separators | `mobile-final3`, `source-html` |
| adv-capture-integrity-final3 | evidence quality | stale/partial/invalid screenshot | Fresh PNGs must decode, match requested dimensions, and include full page | PASS — RGB PNGs, 1440x4328 and 390x6975, fully composited through footer | `desktop-final3`, `mobile-final3`, `geometry-final3` |
| adv-animation-final3 | static report | motion/interaction regression | No unexplained animation or required interactive state is present | NOT_APPLICABLE — standalone static HTML report has no scripted interaction/animation | `source-html` |

### artifactRefs

| id | kind | description | path |
|---|---|---|---|
| desktop-final3 | screenshot | Fresh supplied desktop final3 capture | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-desktop-final3.png` |
| mobile-final3 | screenshot | Fresh supplied mobile final3 capture | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-mobile-final3.png` |
| geometry-final3 | measurement | Caller-supplied viewport, overflow, code-width, and byte-line checks | `.omo/evidence/html-report-cjk-review/hwp-report-final3-geometry.txt` |
| source-html | source | HTML/CSS inspected for keep-all, pre-wrap/anywhere, and responsive card-table rules | `C:\Users\종선\OneDrive\바탕 화면\ai11-2-m-project\GPTPilots_Vault\보고서\2026-07-29 HWP 파싱 타임아웃 원인 및 대응 보고서.html` |

## Verdict

**PASS.** Both independent read-only visual passes agree: final3 desktop and mobile captures are fresh, complete, and free of CJK wrapping, clipping, overflow, code containment, or labelled-card defects. Minor non-blocking note: mobile code uses a small 10px font but remains legible.
