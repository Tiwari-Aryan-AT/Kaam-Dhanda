# Sheet2 completion - project log (portable, cross-session / cross-LLM)

Date: 2026-10-09. Owner: Aryan (MSCI GSAI Physical Assets). Input: `Updated issuer ID cases to evaluate FINAL.xlsx` (Full data ~2k rows; Sheet2 = issuer x URL).

## Ask / constraints (from Aryan)
- Sheet2 rows 1-99 were manually evaluated (done by Aryan, untouched). Complete the remaining 785 rows (592 unique issuer x URL combos).
- Drop blank NOTES column (and the blank unnamed column H).
- Use only Full-data columns A-L and BP-BT as inputs (rest may give false positives).
- NO triage, NO cluster-level decisions: verify from the website itself, asset by asset; then decide at URL level (as in the BUWOG and Unilin cases: check presence + ownership per asset, then URL).
- COMMENTS and Action come from fixed option lists (comma-combinations allowed); REMARKS is free text. Append extra CORRECTED_* columns only when a data-specific row update is needed (so far only CORRECTED_URL, CORRECTED_ISSUER_ID).
- Unresolved by web lookup -> agentic route (local Cursor/codex) -> still unresolved -> manual review list.
- Code/process must be replicable by Aryan; checkpoint progress in the connected folder.

## Reference facts established
- IID000000002670264 = Vonovia SE, parent of BUWOG Group GmbH (IID000000002695580). "Revert to Parent" in the 99 reviewed rows = map to Vonovia SE.
- Reviewed-99 outcomes: 59 Revert to Parent (+1 with URL Update), 36 DROP (33 sold-out, 2 inaccessible listings, 1 Powercor/USA), 1 URL Update (Tekkie Town), 1 Retain (Zhongtai), 1 individual-cases (Unilin: 2 FRA retained, DEU dropped - see Full data col INDIVIDUAL_RESULT).
- Upstream Final_* columns in Full data said CONFIRMED/FP for the reviewed rows and were overturned by manual review -> not used as ground truth.

## Method (see Verification_Rulebook.md for the full rulebook incl. addenda)
Batches by issuer (57 batches, max 50 assets) -> one web-verification agent per batch using WebFetch/WebSearch only (no scripted HTTP) -> per-asset PRESENT / BELONGS / DECISION -> URL-level COMMENTS/Action -> validate (coverage, no unverified Retain) -> assemble. Secmaster lookups via `lookup.py` against sec_master_20260828.parquet.

## Outputs
- `Updated_issuer_ID_cases_FINAL_completed.xlsx`: Sheet2 (completed; STATUS, ASSET_SUMMARY, ACCESS, ESCALATION_REASON, NEW_ACTION_FLAG, ID_CHECK added), Asset_Level (every asset's decision), Escalation_Queue, Review_Flags.
- `Escalation_Queue_for_Cursor_codex.csv`: URLs not resolvable by WebFetch/WebSearch.

## Known limits / things Aryan must decide
- "Remap to Entity" was used for remaps to a specific child/other entity (not in the fixed Action list) - flagged in NEW_ACTION_FLAG; map to your list or add it.
- Many pages were JS-only, robots-blocked, or rate-limited (HTTP 429 on kruidvat.nl store pages when fetched in parallel); those assets stay PENDING and are in the escalation queue, not forced.
- Entity-unconfirmed Retains: pages that show only a brand and no legal entity are noted "entity unconfirmed" in REMARKS.
- Judgement calls to review: Kicks (Swedish stores under Kicks Norge AS; no Swedish entity / parent has no URL), Excelcomindo (finance shell vs XL operating entities), Quironsalud Colombia/Portugal, franchise-owned DROPs on Laser NZ pages (borderline: family-owned only), Brokk distributor DROPs (no Brokk mention on page).

## Pass 2 (2026-10-10)
- Claude-in-Chrome on Aryan's Windows browser is blocked by org policy for the target sites; Control-Chrome (AppleScript) tool does not run on Windows. Cloud WebFetch/WebSearch only. WebSearch has a per-turn budget (200) that was exhausted in pass 3.
- Kruidvat 429 limit cleared; ~310 of 325 stores matched via per-store pages.
- Wanda Hotel Development (IID000000002123766): HKEX filings show an asset-light hotel manager (managed/franchised for third-party owners), hotel business (Wanda Hotel Management HK, IID000000006378407) sold to eLong/Tongcheng (SPA 17 Apr 2025, completion 16 Oct 2025 per HKEX) -> all 112 Wanda hotel rows DROP on issuer-level evidence (presence per hotel not verifiable; alternative target eLong Inc IID000000002203570).
- Rule-based decisions applied from Aryan's own owner rule (cohort 4a/4b): Kicks SE stores present on kicks.se retained on Kicks Norge AS (Swedish owner absent from secmaster, Kicks Group AB has no URL); Pou Chen Vietnam stores remapped to Pou Sheng International IID000000002289065.
- Totals after pass 2/3: 517/592 URLs resolved; assets Retain 825, DROP 333, Remap 250, PENDING 495 (Rossmann TR 179, Lifan dealers 77, XL 51, Daihatsu 39, Brokk/Lifco sites 20, Maxim's 19, rest <=15 each).
- See Cursor_codex_runbook_remaining_cases.md for the remaining list and merge-back procedure.

## Pass 4 (2026-10-10, final)
- 10 further agents (alternate first-party routes) over the 495 PENDING assets. Gains: Muji x3 + Kruidvat x4 (city-only, review) Retain, Adeplast x4 -> Sika AG, KVK Parabit -> Sika CZ, Technisys -> SoFi, Ascot Signs -> Lifco AB, Keisei HQ / Powermec NO Retain, Villa Riviera / Nashua PO Box / Statista NY DROP, plus E2 remaps (Meet Group, Dyckerhoff PL, Metrics NZ).
- Final totals: 519/592 URLs resolved; assets Retain 827, DROP 334, Remap 251, PENDING 491.
- Not resolvable with WebFetch/WebSearch (JS-only locators, no dealer-ownership statement, robots/org-policy blocks): Rossmann TR 179 (fallback owner Dirk Rossmann GmbH IID000000002405302 once presence proven), Lifan US 77, XL Center 51 (remap target XLSMART IID000000002165881), Daihatsu 39, Starbucks HK 19, Brokk/Lifco sites, 10-K Item 2 cases. -> Cursor/codex runbook.
- Review flags: Remap to Entity (new action), Wanda issuer-level DROPs, fallback Retains, 4 city-only Kruidvat retains, Palisade NY / Metrics / Statista NY address-mismatch calls (offices may have moved), BlackForxx -> KION stop-gap.
