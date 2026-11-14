# Customer navigation rewrite validation

- Baseline: `e1815a541`; initially clean working tree.
- Scope: `docs/design/20261107-103359/as-is/navigation.md`, consolidating 51 task fragments into 10 main scenarios and a secondary feature table.
- Prior history: no `tests/test-history/` existed. Consulted the earlier static review in `docs/design/20261107-103359/as-is/system-design-as-is.md`, section 8. Its browser and provider limitations remain applicable; past passes are not current results.
- Environment: local repository, Python 3, no running product or diagram parser used.

## NAV-01 — Source and independent review

- Started: 2026-11-14T20:50:38+09:00.
- Method: compare routes, action bodies, checkout workflow and handlers, Razor links, and existing screen scenes; independent agents review account, checkout, and post-purchase flows.
- Findings: corrected the PublicController source path and helper/action names (FinalizeCustomerRegistrationAsync, PasswordRecovery). Confirmed guest checkout, return URLs, payment variants, return submission, and review eligibility against source.
- Result: source review completed; no material flow defects remain in the reviewed scope. This does not establish runtime behavior for all settings or providers.

## NAV-02 — Static document checks

- Started: 2026-11-14T20:50:38+09:00.
- Expected: valid relative links and declared HTML scenes; all N01–N51 anchors retained exactly once; 10 well-formed navigation blocks; all page nodes mapped to routes and local screen links; logical APIs covered by the contract table; CRLF without BOM/final-newline changes.
- Ended: 2026-11-14T20:51:18+09:00.
- Command: `python3 /tmp/check_smartstore_navigation.py`.
- Result: checker failed with 28 false positives because its table-width check excluded valid two-column tables. Link, scene, anchor, flow, API and line-ending checks found no defects. This run is not recorded as passed.
- Correction: compare row width against its own table header rather than hard-coding widths; restrict page-map extraction to the route-map section.
- Follow-up: NAV-03 reruns the corrected checker. No document defect was inferred from these false positives.

## NAV-03 — Corrected static document checks

- Started: 2026-11-14T20:51:18+09:00.
- Command: `python3 /tmp/check_smartstore_navigation.py` (full reproducible checker retained below after completion).
- Expected: the checks listed in NAV-02, with per-table column widths.
- Ended: 2026-11-14T20:51:31+09:00.
- Result: PASS — 323 relative/source/screen links, 51 legacy anchors, 10 flows with 106 edges, 28 mapped pages, 21 logical API contracts; zero static errors.
- Independent review: account, checkout and post-purchase reviewers found no remaining material flow issues after evidence-name/path fixes, registered-only return-list clarification, and removal of irrelevant result scenes from product mail/inquiry forms.
- Limits: syntax checked against the documented navigation grammar; no dedicated parser, diagram renderer, product browser, authentication email, payment provider, build or application test executed.
- Follow-up: none for this document-only scope. Runtime/provider limitations remain documented in the navigation file and prior AS-IS analysis.

### Reproduction

Run the following from the repository root with Python 3. It performs static document checks only.

```python
from pathlib import Path
import re, json, unicodedata
from collections import Counter
p=Path('docs/design/20261107-103359/as-is/navigation.md')
s=p.read_text(); errors=[]
def slug(t):
    return ''.join(c for c in t.lower().replace(' ','-') if c in '_-' or unicodedata.category(c)[0] in 'LN')
def fragments(q):
    t=q.read_text(); ids=set(re.findall(r'id=["\x27]([^"\x27]+)',t))
    if q.suffix=='.md':
        ids.update(slug(h) for h in re.findall(r'^#+ (.+)$',t,re.M))
    else:
        ids.update(re.findall(r'<option value="([^"]+)"',t))
    return ids
links=re.findall(r'\[[^\]]*\]\(([^)]+)\)',s)
for link in links:
    if re.match(r'https?://',link): continue
    name, _, frag=link.partition('#')
    q=(p.parent/name).resolve() if name else p
    if not q.exists(): errors.append('Missing path: '+link)
    elif frag and frag not in fragments(q): errors.append('Missing fragment: '+link)
legacy=Counter(re.findall(r'<a id="(n\d+)"',s))
if set(legacy)!={f'n{i:02}' for i in range(1,52)} or any(v!=1 for v in legacy.values()): errors.append('Legacy anchors changed')
blocks=re.findall(r'```navigation\n(.*?)```',s,re.S)
if len(blocks)!=10: errors.append('Expected 10 navigation blocks')
pages={node:scene for node,scene in re.findall(r'^\| (\w+) \| .*?details/screens/([\w-]+)\.html#default',s.split('## 공통 가드·계약',1)[0],re.M)}
contracts=set(re.findall(r'^\| `\((/[^)]+)\)`',s,re.M))
edges=0
for n,block in enumerate(blocks,1):
    section=s.split(f'## {n}. ',1)[1].split('\n## ',1)[0]
    expected=['진입 조건','이동과 분기','결과·복귀','화면 확인 링크','근거·미확인']
    headings=re.findall(r'^### (.+)$',section,re.M)
    if headings!=expected: errors.append(f'Flow {n}: headings mismatch')
    used=set()
    for line in block.strip().splitlines():
        edge=re.fullmatch(r'(\w+|\([a-z0-9_/]+\)|`[^`]+`) --> (\w+|\([a-z0-9_/]+\)|`[^`]+`)(?: : [^()]+)?',line)
        if not edge: errors.append(f'Flow {n}: malformed edge {line}'); continue
        edges+=1
        for node in edge.groups():
            if node.startswith('(/') and node[1:-1] not in contracts: errors.append('API missing contract: '+node)
            elif re.fullmatch(r'\w+',node): used.add(node)
    for node in used:
        if node not in pages: errors.append('Unmapped page: '+node)
        elif f'details/screens/{pages[node]}.html#default' not in section: errors.append(f'Flow {n}: missing base screen for {node}')
table_columns=None
for line in s.splitlines():
    if line.startswith('|'):
        if table_columns is None: table_columns=line.count('|')
        elif line.count('|')!=table_columns: errors.append('Table column count: '+line[:60])
    else: table_columns=None
if s.count('```')!=20: errors.append('Unbalanced fences')
for q in [p,*Path('tests/test-history').glob('*.md')]:
    b=q.read_bytes()
    if b'\n' in b.replace(b'\r\n',b''): errors.append('Non-CRLF: '+str(q))
if p.read_bytes().startswith(b'\xef\xbb\xbf') or not p.read_bytes().endswith(b'\r\n'): errors.append('BOM/final-newline changed')
print(json.dumps({'links':len(links),'legacy_anchors':len(legacy),'flows':len(blocks),'edges':edges,'mapped_pages':len(pages),'contracts':len(contracts),'errors':errors},ensure_ascii=False,indent=2))
raise SystemExit(bool(errors))
```

### Final repository checks

- Plain `git diff --check` classified required CRLF carriage returns as trailing whitespace in this repository configuration; use the explicit CRLF-aware command in NAV-04. No line-ending conversion was made.
- `git ls-files --eol -- docs/design/20261107-103359/as-is/navigation.md`: expected `w/crlf`.
- New validation Markdown files are checked as CRLF by the reproduction script.
- BOM and final newline on navigation.md remain unchanged (no BOM, final CRLF).


## NAV-04 — CRLF-aware final repository checks

- Started: 2026-11-14T20:51:51+09:00.
- Context: plain `git diff --check` reported CR-at-EOL whitespace false positives; the repository requires CRLF. Preserve the required bytes and adjust only the command's whitespace interpretation.
- Commands:
  - `git -c core.whitespace=blank-at-eol,blank-at-eof,space-before-tab,cr-at-eol diff --check`
  - `git ls-files --eol -- docs/design/20261107-103359/as-is/navigation.md`
  - Python byte checks for every touched Markdown file: no bare LF, no space/tab before CRLF, unchanged navigation BOM and final newline.
- Expected: no whitespace errors, tracked navigation `w/crlf`, all touched Markdown CRLF.
- Ended: 2026-11-14T20:51:58+09:00.
- Result: PASS — CRLF-aware diff check returned no errors; tracked navigation reports `i/crlf w/crlf`; all 3 touched Markdown files passed byte/whitespace/BOM/final-newline checks.
- Follow-up: none.


## NAV-05 — Remove navigation overview sections

- Started: 2026-11-14T20:54:28+09:00.
- Request: remove the scenario index, page-node map, and common guard/contracts sections. Retain the 10 detailed scenarios and API summary.
- Change: removed those three sections; moved legacy n01/n50 anchors to the first scenario and order-access scenario respectively.
- Prior reference: NAV-03/NAV-04. The old reproduction checker expects the removed page-map section and is not applicable unchanged.
- Checks: compare remaining body against its pre-edit contents, check removed headings are absent, all 51 legacy anchors remain unique, 10 navigation blocks remain, and preserve CRLF/BOM/final newline; run CRLF-aware git diff check.
- Ended: 2026-11-14T20:54:51+09:00.
- Result: PASS — all three requested headings are absent; 51 unique legacy anchors and 10 navigation blocks remain; CRLF, BOM, final newline and CRLF-aware whitespace checks passed.
- Limits: narrow document edit; no product/runtime/parser execution.
- Follow-up: none for this requested removal.
