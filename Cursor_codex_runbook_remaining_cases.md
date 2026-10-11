# Cursor / codex runbook - remaining PENDING assets (491 assets / 73 URLs)

Input: `Remaining_PENDING_assets_for_Cursor_codex.csv` (one row per asset: URL, address, prior blocker in ASSET_NOTE naming the exact first-party page to open) and `Escalation_Queue_for_Cursor_codex.csv` (URL level).
Rules: `Verification_Rulebook.md` (Addenda + PASS 2). Secmaster lookups: `lookup.py` (point the parquet path at sec_master_20260828.parquet).

## Why these are left (cannot be done with web fetch/search tools from the cloud session)
- JS-only locators: Rossmann TR (179), XL Center pages 2-8 (51), Daihatsu outlets (39), Maxim's/Starbucks HK (19), Muji, Cycle & Carriage, Hongkong Land, Control Cutter, Majestic.
- Ownership proof missing for a DROP: Lifan US dealers (77) - need first-party statement dealers are independently owned (americanlifan.com/dealer-locator, dealer-manuals).
- robots.txt / org-policy blocked sites: Lifco-group company sites (Brokk batch, 20), Powermec, Clean Spot, Technisys, NH Securities, Black Construction Guam, KVK Parabit, Adeplast.
- SEC 10-K text truncated before Item 2: Primerica, Heritage, Meet Group.
- Kruidvat: 15 store pages return no address (stores 7902, 2062, 7601, 7019, 6245, 7114, 3913, 6257, 1384 ...).

## Prompt to give the agent (per issuer group)
"For each row in the CSV open the first-party page named in ASSET_NOTE in a real browser. Decide per asset: PRESENT_ON_URL (match street+no+postcode or coordinates), BELONGS_TO_ISSUER (legal entity shown), then apply: owner in secmaster with URL -> Remap to Entity; owner absent -> closest ancestor with URL (Revert to Parent); chain exhausted -> fallback to original issuer (DMX issue); franchise/dealer-owned per first-party statement -> DROP; sold/closed with source -> DROP. Never mark Retain/DROP without a concrete match/evidence. Write results with columns ROW1, present, belongs, decision (Retain|DROP|Remap|PENDING), corrected_issuer_id, note + proof URL/extract. Respect robots.txt and rate limits (one page at a time, 2 s pause)."

## Merge back
Update `Asset_Level` rows by FULLDATA_EXCEL_ROW; then recompute the URL-level row: all assets decided -> STATUS RESOLVED; all Retain -> CONFIRMED CORRECT / Retain as is; all DROP -> CORRECTION NEEDED / DROP; one target ID -> Remap to Entity / Revert to Parent + CORRECTED_ISSUER_ID; mixed -> COMMENTS "INDIVIDUAL CASES, NO ISSUER URL IN DMX", Action blank, per-asset results listed.
