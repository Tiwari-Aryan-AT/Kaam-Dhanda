# HANDOFF - Sheet2 issuer x URL completion (MSCI GSAI Physical Assets)

Owner: Aryan (MSCI GSAI Physical Assets, Data Collection Operations). Session dates: 2026-10-09 to 2026-10-11.
Purpose of this file: let any new session or LLM (Claude, Cursor, codex) resume without the chat. Full transcript of the original session (if available to you): `/root/.claude/projects/-home-claude/3d381b13-6f27-5d1f-8556-8a4657e4c841.jsonl`.

---

## 1. Status at a glance

| Item | Value |
|---|---|
| Sheet2 rows | 99 manual (Aryan, untouched) + 785 rows to complete = 592 unique issuer x URL combos, 1,903 assets |
| URLs resolved | 519 / 592 (73 URLs still open) |
| Asset decisions | Retain 827 / DROP 334 / Remap 251 / PENDING 491 |
| Final deliverables | In the connected Cowork working folder (see section 6) |
| What is left | 491 PENDING assets that cannot be settled with WebFetch/WebSearch -> Cursor/codex runbook -> then Aryan's manual review |

---

## 2. Inputs and prompts (chronological)

1. **Upload**: `Updated issuer ID cases to evaluate FINAL.xlsx`.
   - "Full data" sheet: ~2k asset rows, 75 columns (one row per asset, with issuer, URL, address, plus many upstream QA columns; last column `INDIVIDUAL_RESULT` holds per-asset results for the Unilin case).
   - "Sheet2": issuer x URL combinations. First 99 rows evaluated manually by Aryan; headers COMMENTS, Action, CORRECTED_URL, CORRECTED_ISSUER_ID, REMARKS were filled after checking each URL and all assets under it. Blank NOTES column and unnamed blank column H to be dropped.
   - Ask: go through the file, understand what was done and give an inference.
2. **"Before you start, share a plan..."** for the remaining ~800 rows, for Aryan's review.
3. **Aryan's detailed feedback** (binding):
   - Unilin = an *individual cases* example (2 FRA assets retained, DEU dropped).
   - Asset-level checking is DEFINITELY required: presence on the URL and ownership per asset, NO cluster-level decisions, then decide at URL level (BUWOG / Unilin pattern).
   - Use ONLY Full-data columns A-L and BP-BT as inputs (other columns can cause false positives).
   - NO TRIAGE: do web lookups and verify from the website itself.
   - COMMENTS and Action come from fixed option lists (comma-combinations allowed); REMARKS is free text. Add extra CORRECTED_* columns only if a data-specific update is needed, and append more as needed.
   - Process: agentic web check -> unresolved go to local Cursor/codex agentic route -> still unresolved go to Aryan's manual review.
   - Reference: the "BUWOG Group asset verification" chat; shared reference files; a connected working folder; checkpoint there; give progress updates while continuing autonomously end to end.
4. **"Continue and resolve all URLs/facilities"**.
5. **"Continue, do not worry about per turn search budget, will queue up continue messages to overcome that. Stop once done"**.
6. **"Create a handoff markdown for all the work done in this chat..."** (this file).

Aryan's own preferences that applied: log project asks/constraints to a standalone file (done: `PROJECT_LOG_Sheet2_completion.md`) and to memory (`/areas/asset-issuer-url-mapping.md` has a short note).

---

## 3. What the 99 manual rows taught (inference used as ground truth)

- 59 Revert to Parent (+1 with URL Update), 36 DROP (33 sold-out projects, 2 inaccessible listings, 1 Powercor/USA impossible-country), 1 URL Update (Tekkie Town: listed URL shows nothing, locator page does), 1 Retain (Zhongtai), 1 individual-cases (Unilin).
- IID000000002670264 = Vonovia SE, parent of BUWOG Group GmbH (IID000000002695580); "Revert to Parent" there means map to Vonovia SE.
- Upstream `Final_*` columns said CONFIRMED/FP for reviewed rows and were overturned by Aryan, so they were NOT used as ground truth.

## 4. Method and rules (full text in `Verification_Rulebook.md`)

Pipeline: batch by issuer (57 batches, max 50 assets) -> one general-purpose subagent per batch, WebFetch/WebSearch only -> per-asset PRESENT_ON_URL / BELONGS_TO_ISSUER / decision -> URL-level COMMENTS/Action -> JSON per batch -> `validate.py` / `validate2.py` (coverage, no unverified Retain) -> `merge2.py` + `assemble.py` -> `build_xlsx.py`.

Key rules:
- Never Retain/DROP without an asset-specific match (name, street+postcode or coordinates). Otherwise PENDING with the blocker and the exact first-party URL to check.
- DROP needs positive evidence (sold/closed with source, absent from a fully-read authoritative first-party list, wrong entity/facility, franchise/dealer/member-owned by first-party statement). Dealer "nature" alone is not evidence.
- Owner rule (Aryan's): owner in secmaster with own IID + URL -> Remap to Entity; owner absent -> closest ancestor with a URL (Revert to Parent); chain exhausted -> fallback Retain on original issuer (flag as DMX issue); brand-only page -> Retain, remark "entity unconfirmed"; silent + mixed network -> PENDING.
- URL-level derivation: all assets decided -> RESOLVED; all Retain -> CONFIRMED CORRECT / Retain as is; all DROP -> CORRECTION NEEDED / DROP; single remap target -> Remap to Entity or Revert to Parent + CORRECTED_ISSUER_ID; mixed -> COMMENTS "INDIVIDUAL CASES, NO ISSUER URL IN DMX", Action blank, per-asset list.
- New Action value "Remap to Entity" is NOT in Aryan's fixed list; flagged in column NEW_ACTION_FLAG.
- Kruidvat method: per-store page `https://www.kruidvat.nl/store/drogisterij-kruidvat-<store no>`, match street+no+postcode.
- Secmaster lookups: `lookup.py "<name|domain|IID>" [ISO2]` against `sec_master_20260828.parquet` (2.15M entities). The parquet path inside the script points at the cloud scratchpad: repoint it locally.
- Tool constraints: no scripted HTTP (curl/Python/Playwright) to bypass blocks; robots.txt, HTTP 429 and org policy are respected. Claude-in-Chrome is blocked by org policy for target sites; Control_Chrome needs macOS osascript and fails on Aryan's Windows device.

## 5. Work done, by pass

- **Pilot + pass 1**: 57 batches (52 result files). Pilot agents took shortcuts (one fetched 1 of 25 URLs, one retained an unchecked asset, one dropped Lifan dealers without fetching), so rulebook addenda were tightened and a batch re-run. Batches for Wanda, Rossmann, Lifan (6, 7, 28, 29, 40) were deliberately held for pass 2.
- **Pass 2/3**: batches `K1-4, W1-3, R1-2, M11-13, M21-22, E1-8, F1-5`. Kruidvat 429 rate-limit cleared; ~310 of 325 stores matched. Wanda Hotel Development (IID000000002123766): 112 hotel rows DROP on issuer-level HKEX evidence (asset-light manager; hotel business sold to eLong/Tongcheng, SPA 17 Apr 2025). Rule-based overrides: Kicks SE stores -> Retain on Kicks Norge AS; Pou Chen Vietnam -> Remap to IID000000002289065. Totals then: 517/592 URLs; Retain 825, DROP 333, Remap 250, PENDING 495.
- **Pass 4 (G-series, 10 agents)**: GR1/GR2 (Rossmann), GL1 (Lifan), GX1 (XL), GD1 (Daihatsu), GM1 (Muji/Kruidvat/Starbucks HK), GE1-4. Gains: Muji x3 and Kruidvat x4 (city-only) Retain; Adeplast x4 -> Sika AG (IID000000002183735); KVK Parabit -> Sika CZ (IID000000005197182); Technisys -> SoFi (IID000000005045121); Ascot Signs -> Lifco AB (IID000000002715781); Keisei HQ and Powermec Norway Retain; Villa Riviera, Nashua PO Box, Statista NY DROP; Meet Group London -> Parship Group GmbH (IID000000002817614); Dyckerhoff Krakow -> Dyckerhoff Polska (IID000000005157918); Metrics Auckland -> Metrics Credit Partners Pty (IID000000005348282); BlackForxx -> KION Group (IID000000002670343, stop-gap). Merged and rebuilt: final totals in section 1.

## 6. Files and how each is used

In the connected folder `...\Cowork\Cowork Updated Issuer ID FINAL working\`:

| File | Use |
|---|---|
| `Updated_issuer_ID_cases_FINAL_completed.xlsx` | Main deliverable. Sheets: Sheet2 (99 manual rows untouched + completed rows; extra columns STATUS, ASSET_SUMMARY, ACCESS, ESCALATION_REASON, NEW_ACTION_FLAG, ID_CHECK), Asset_Level (every asset's decision, note, batch), Escalation_Queue (URL level), Review_Flags |
| `Remaining_PENDING_assets_for_Cursor_codex.csv` | 491 pending assets, one row each, with ASSET_NOTE naming the blocker and exact first-party page to open |
| `Escalation_Queue_for_Cursor_codex.csv` | 73 open URLs, URL-level view |
| `Cursor_codex_runbook_remaining_cases.md` | Agent prompt, decision rules and merge-back procedure for the pending set |
| `PROJECT_LOG_Sheet2_completion.md` | Asks/constraints, facts, per-pass log |
| `Verification_Rulebook.md`, `lookup.py` | Rules and secmaster lookup (earlier pass; copies in outputs) |
| `HANDOFF_Sheet2_issuer_URL_completion.md` | This file |

Cloud-only working files (will not persist; regenerate from the xlsx if needed): `work_assets.csv` (1,903 assets, cols A-L + BP-BT, ROW1 = Excel row in Full data), `secmaster_slim.parquet`, `batches*/`, `results*/`, `RULEBOOK.md`, `validate*.py`, `merge2.py`, `assemble.py`, `build_xlsx.py`. The Asset_Level sheet is the authoritative copy of per-asset decisions.

## 7. Remaining 491 PENDING assets (73 URLs) and why

| Group | Assets | Blocker | Next step |
|---|---|---|---|
| Rossmann Turkey | 179 | `/magazalar` locator JS-only; no per-store pages in sitemap; careers site gives only city counts | Open https://www.rossmann.com.tr/magazalar in a real browser, match addresses; once present, owner falls back to Dirk Rossmann GmbH IID000000002405302 |
| Lifan US dealers | 77 | No first-party statement that dealers are independent; locator errors; dealer-manuals gated | Check https://www.americanlifan.com/dealer-locator/ and each dealer's own site |
| XL Center | 51 | Pagination broken via fetch (all pages return page 1); xl.co.id locator JS-only | Browser check of https://www.xl.co.id/bantuan/xlcenter-lokasi with province/city filter; remap target XLSMART IID000000002165881 |
| Daihatsu outlets | 39 | No source says Astra-owned vs independent; outlet pages error | https://www.astra-daihatsu.id/outlet; Astra annual report network section |
| Starbucks HK / Maxim's | 19 | District-numbered names unmatchable; locator JS-only (Maxim's operating Starbucks HK confirmed first-party) | Browser locator match |
| Brokk / Lifco-group sites, Sumitomo Saitama, Sensitech Japan, Control Cutter x3, Kurago Korea | ~25 | robots-blocked or JS-only | Per-site browser check |
| 10-K Item 2 cases | ~12 | Primerica, Heritage, Meet Group 10-K copies truncated before Item 2 | Read full 10-K Item 2 |
| Others (<=15 each) | rest | Vertex (no street addresses), Hertz Greece x8 (owner Autohellas IID000000002138912), KVK Czech x3, Care Vision x5, Admiral Taverns pages 3-10, Kruidvat (11 pages without address), Keisei farms, C&C Malaysia, THACO, Nashua branches, PCC, Rolls-Royce, NH Securities, Clean Spot, International Equipment of Guam, IONOS London, KDC, Makuake, Grand Pharma | See each asset's ASSET_NOTE |

## 8. Items for Aryan to review (judgement calls)

1. "Remap to Entity" is a new Action value; map to the fixed list or add it.
2. Wanda 112 DROPs rest on issuer-level evidence (not per hotel); alternatives eLong IID000000002203570 / Wanda Hotel Management HK IID000000006378407.
3. Fallback Retains where the owner is absent and the chain exhausted (Kicks and similar).
4. 4 Kruidvat stores (7902, 7019, 6257, 1384) Retain on store number + city only, address unverified; issuer secmaster URL is kruidvat.be while assets are NL.
5. Address-mismatch calls: Palisade NY (1290 6th Ave on page), Metrics Auckland, Statista NY (page lists 140 Broadway; I DROPped; alternative Remap to Statista Inc IID000000005424600). Offices may have moved.
6. BlackForxx -> KION is a stop-gap (BlackForxx GmbH not in secmaster).
7. Also review: Excelcomindo (finance shell vs XL operating entities), Quironsalud Colombia/Portugal, franchise-owned DROPs on Laser NZ pages (family-owned only, borderline), Brokk distributor DROPs (no Brokk mention on page), planned/not-built projects dropped as "planned".

## 9. How to resume

1. Give Cursor/codex: the pending CSV, runbook, rulebook, `lookup.py` (repointed to the local secmaster). Ask for output columns ROW1, present, belongs, decision (Retain|DROP|Remap|PENDING), corrected_issuer_id, note + proof URL.
2. Merge into `Asset_Level` by FULLDATA_EXCEL_ROW, then recompute the URL-level row with the derivation rules in section 4.
3. Anything still PENDING -> Aryan's manual list.
4. If re-running in Claude: fresh agents may try new first-party routes, but WebFetch/WebSearch cannot read JS-only locators; do not circumvent robots/429/org-policy blocks.
