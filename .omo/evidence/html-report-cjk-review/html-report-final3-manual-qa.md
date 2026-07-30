# Manual QA — HWP 파싱 타임아웃 HTML report (final3)

## manualQa

### surfaceEvidence

| scenario id | criterion reference | surface | exact invocation | verdict | artifactRefs |
|---|---|---|---|---|---|
| desktop-final3 | six-section teammate report; desktop layout and Korean rendering | Web page, 1440px capture | `view_image` on `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-desktop-final3.png`; UTF-8 heading extraction from source | PASS — six sections 01→06, hierarchy, glyphs, tables and code visible | `desktop-final3`, `source-html-final3`, `png-integrity-final3` |
| mobile-final3 | responsive 390px report; labelled stacked table cards and CJK wrapping | Web page, 390px capture | `view_image` on `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-mobile-final3.png`; source inspection of mobile CSS and `data-label` cells | REVISE — tables/code/page bounds pass, but headings split `필요한 설명` and `왜 됐었나` across lines | `mobile-final3`, `source-html-final3`, `png-integrity-final3` |

### adversarialCases

| scenario id | criterion reference | adversarial class | expected behavior | verdict | artifactRefs |
|---|---|---|---|---|---|
| adv-cjk-heading-final3 | Korean/CJK precision | semantic heading line breaking | Korean heading phrases stay together at 390px | REVISE [product] — section 02 renders `본문 레코드에서 필요한 / 설명`; section 04 renders `다른 HWP 파일들은 왜 / 됐었나`; keep phrases together with mobile h2 sizing/spacing or phrase nowrap | `mobile-final3`, `source-html-final3` |
| adv-mobile-tables-final3 | responsive tables | labelled mobile card transformation | each hidden header is restored as a visible row label | PASS — sections 04 and 06 render labelled stacked rows | `mobile-final3`, `source-html-final3` |
| adv-code-final3 | code comparison | long-line readability | patched code remains readable without clipping | PASS — mobile `pre-wrap`/10px code fits both cards | `mobile-final3`, `source-html-final3` |
| adv-byte-record-final3 | byte payload explanation | token grouping | 16-byte Field Start and 2-byte terminator remain grouped | PASS — desktop one-line payload and mobile grouped labels are visible | `desktop-final3`, `mobile-final3` |
| adv-overflow-final3 | responsive layout | horizontal overflow | page content stays within 1440/390 viewport; code scrolling contained | PASS — supplied metrics 1440/1440 and 390/390; no page-level clipping visible | `desktop-final3`, `mobile-final3`, `source-html-final3` |
| adv-glyph-final3 | Korean readability | tofu/baseline clipping | intact CJK glyphs and labels | PASS — no tofu or baseline clipping observed | `desktop-final3`, `mobile-final3` |
| adv-animation-final3 | static report | unexplained motion/interaction | no required motion is omitted | not_applicable — static HTML has no script/animation surface | `source-html-final3` |

### artifactRefs

| id | kind | description | path |
|---|---|---|---|
| desktop-final3 | screenshot | Fresh complete desktop capture; valid PNG, 1440×4328, mtime 10:21:45 | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-desktop-final3.png` |
| mobile-final3 | screenshot | Fresh complete mobile capture; valid PNG, 390×6975, mtime 10:21:45 | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-mobile-final3.png` |
| source-html-final3 | source | Current HTML/CSS; mtime 10:21:28; six `<section>`/`<h2>` and mobile `keep-all` rules | `보고서\2026-07-29 HWP 파싱 타임아웃 원인 및 대응 보고서.html` |
| png-integrity-final3 | inspection | PNG signatures `89 50 4e 47 0d 0a 1a 0a`; RGB dimensions verified | `C:\Users\종선\AppData\Local\Temp\hwp-report-20260730-*-final3.png` |

## Verdict

**REVISE.** Fresh final3 evidence confirms the real six-section DOM, responsive tables, code readability, byte grouping, glyph integrity, and no page overflow. One strict CJK criterion remains blocking: two mobile headings split semantic phrases; adjust mobile heading sizing/nowrap and recapture before PASS.
