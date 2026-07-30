# Manual QA — HWP 파싱 타임아웃 HTML report (final2)

## manualQa

### surfaceEvidence

| scenario id | criterion reference | surface | exact invocation | verdict | artifactRefs |
|---|---|---|---|---|---|
| desktop-final2 | six-section teammate report; desktop layout and Korean rendering | Web page, 1440px capture | `view_image` on `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-desktop-final2.png`; source read via `Get-Content -Raw` and UTF-8 heading extraction | REVISE ([evidence] capture mtime 10:17:10 precedes current HTML mtime 10:21:28; visual content itself shows six sections and intact glyphs) | `desktop-final2`, `source-html-final2`, `png-integrity-final2` |
| mobile-final2 | responsive 390px report; labelled stacked table cards | Web page, 390px capture | `view_image` on `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-mobile-final2.png`; source inspected for `@media (max-width: 760px)` table rules and `data-label` cells | REVISE ([evidence] capture mtime 10:17:10 precedes current HTML mtime 10:21:28; additionally several Korean words/phrases split semantically) | `mobile-final2`, `source-html-final2`, `png-integrity-final2` |

### adversarialCases

| scenario id | criterion reference | adversarial class | expected behavior | verdict | artifactRefs |
|---|---|---|---|---|---|
| adv-cjk-wrap-final2 | Korean/CJK precision | semantic line breaking | Korean prose/headings keep words, particles, and short phrases together at 390px | REVISE — source uses `h1, h2, h3, p { overflow-wrap: break-word; }` without `word-break: keep-all`; capture shows splits such as `범용 파 / 서 결함`, `문 / 제입니다`, `길이 / 를`, `파라미터 / 를` | `mobile-final2`, `source-html-final2` |
| adv-mobile-tables-final2 | responsive tables | labelled mobile card transformation | hidden header context is restored with visible labels and no page-level horizontal overflow | PASS — `td::before { content: attr(data-label) }` renders labels; tables stack in sections 04 and 06 | `mobile-final2`, `source-html-final2`, `png-integrity-final2` |
| adv-code-readability-final2 | code comparison | long-line readability at mobile width | critical before/after code remains readable in static capture or wraps safely | REVISE — section 05 수정 code has a long condition line clipped at the card edge in the static image; `pre` intentionally scrolls horizontally | `mobile-final2`, `source-html-final2` |
| adv-byte-record-final2 | byte payload explanation | token grouping across responsive widths | payload token sequence and final terminator remain visually grouped | REVISE — desktop section 02 payload leaves the final `00` on an isolated next line | `desktop-final2`, `source-html-final2` |
| adv-page-overflow-final2 | responsive layout | horizontal overflow | page width remains within viewport; intentional code scrolling is contained | REVISE ([evidence] supplied geometry is consistent with captures, but metrics are stale until rerun against current HTML) | `desktop-final2`, `mobile-final2`, `source-html-final2` |
| adv-glyph-integrity-final2 | Korean readability | tofu/baseline clipping | no missing glyphs, baseline clipping, or detached labels | PASS — rendered glyphs intact in both captures; no tofu observed | `desktop-final2`, `mobile-final2`, `png-integrity-final2` |
| adv-animation-final2 | static report | unexplained motion/interaction | static report has no required animation or interaction | not_applicable — source contains no script/animation surface | `source-html-final2` |

### artifactRefs

| id | kind | description | path |
|---|---|---|---|
| desktop-final2 | screenshot | Fresh complete desktop capture (PNG, 1440×4379) | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-desktop-final2.png` |
| mobile-final2 | screenshot | Fresh complete mobile capture (PNG, 390×6960) | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-mobile-final2.png` |
| source-html-final2 | source | Final HTML DOM/CSS; six `<section>` and six `<h2>` headings | `보고서\2026-07-29 HWP 파싱 타임아웃 원인 및 대응 보고서.html` |
| png-integrity-final2 | inspection | PNG signature and IHDR inspection: both signatures `89 50 4e 47 0d 0a 1a 0a`; widths 1440 and 390; captures 10:17:10 vs source 10:21:28 (stale) | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-*.png` |

## Verdict

**REVISE.** The report is a real six-section token-driven DOM with strong hierarchy and responsive labelled table cards. The supplied captures are stale against the current HTML (evidence blocker), and the rendered revision still needs strict CJK semantic wrapping plus mobile code/desktop byte-payload readability fixes; recapture both widths before approval.
