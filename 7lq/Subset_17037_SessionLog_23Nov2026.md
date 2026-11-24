# Subset_17037 Final Issuer Remapping — Session Log (23-Nov-2026)

Companion log to `Subset_17037_Final_Issuer_Remapping_UPDATED_23Nov2026.xlsx`. Covers all work done on this
file in this session, starting from the uploaded `17037_Final_Issuer_Remapping_1.xlsx` (17,037-row Review
sheet, 21 sheets, 123 open REVIEW rows across 43 candidate-issuer groups) and the companion
`REVIEW_Action_List_23Nov2026.md`.

## Part 1 — Research pass on the 123 REVIEW rows

**Instruction:** "Go through the REVIEW action list markdown ... find official/legal sources to close these
REVIEW rows with confidence. Make sure the `*_Final_Issuer_*` xlsx file has properly incorporated all
required updates and all sheets are in sync with each other (for these issuers at least)."

**Method:** dispatched 8 parallel high-effort research batches, each covering 2-5 candidate-issuer groups,
using only primary/official sources: UK Companies House, SEC EDGAR full-text search, GLEIF/Bloomberg LEI,
FDIC BankFind Locations API, national company registries (Dutch KvK, Austrian Firmenbuch/evi.gv.at, Spanish
BORME-aggregators, Italian Camera di Commercio-aggregators, Kentucky/Georgia/Delaware Secretary of State),
Labuan FSA, Bermuda Monetary Authority, UK Food Standards Agency FHRS, NHS ODS, El Salvador SSF, Guatemala
BAM, Mozambique Boletim da Republica, SGX-filed annual reports, and each issuer's own official contact/
locator/legal-notice pages.

**Outcome (before the Part 2 closeout below):**

| Verdict | Rows |
|---|---|
| CLOSE-GREEN | 8 |
| CLOSE-ORANGE | 36 |
| CLOSE-RED (confirmed non-qualifying/misattributed premises) | 3 |
| STAYS-REVIEW (reasoning updated, no closure) | 76 |
| **Total** | **123** |

Selected findings of note:
- **Allied World Ltd** (Bermuda + Labuan): closed GREEN via GLEIF/Bloomberg LEI + Bermuda Registrar of
  Companies reg. 31277 and Labuan FSA licensee list.
- **Fortna Germany GmbH** (Hamburg GREEN, Graz ORANGE) and **Fortna S.p.a.** (Parma GREEN): closed via
  Austrian Firmenbuch, German commercial-register-sourced LEI, and Italian registry aggregators — all
  confirming the *existing* candidate IIDs, not new entities.
- **Iberinform Internacional** (12 facilities, all ORANGE): BORME/AEAT-sourced registry aggregators confirm
  the candidate's "S.L.U." label is a legal-form mislabeling of the same NIF A28382927 entity, properly
  "Iberinform Internacional, S.A.U."
- **Brook Crompton subsidiaries** (UK/Singapore/Canada/USA, 4 facilities, ORANGE): the candidate issuer
  "Brook Motors Ltd" is the wrong entity for 4 of 5 Brook-branded facilities — UK Companies House and Brook
  Crompton Holdings Ltd's FY2025 SGX annual report show the real operators are Brook Crompton-branded
  subsidiaries under Wolong Electric Group Co., Ltd. (the *original* issuer already on file). No secmaster
  IID was invented for the subsidiaries — flagged to **Other Inputs Needed** instead.
- **Quail Tools** (3 facilities, ORANGE): SEC filings confirm the LP→LLC entity conversion (same Oklahoma
  entity), but also reveal Nabors Industries Ltd. sold Quail Tools, LLC to Superior Energy Services, Inc. on
  20-Aug-2025 — the Nabors attribution is now stale. Flagged to Other Inputs Needed.
- **BCPG / Lampang Distribution Center** (RED): Bangchak's own 2023 Annual Report attributes this terminal
  to Fuel Pipeline Transportation Ltd. (FPT), in which Bangchak holds only a 4.78% non-controlling stake —
  neither BCPG nor Bangchak Corporation controls it.
- **ISI Emerging Markets** (Europe Office, GREEN): corrected legal name — Companies House/GLEIF confirm
  "ISI Emerging Markets Holdings (UK) Limited" (no word "Group").
- Large, clean exclusion clusters that could not be closed with available tooling (Wayback Machine blocked
  all session; several national registries are interactive/paywalled and unreachable): **Brookfield** (14
  facilities, bare-city placeholders vs. a holding company with no real-property disclosure), **TATA AIG**
  (5), **Skyscanner** (6, informal/rotating co-working spaces), **BUWOG** (7), **Allied World Holdings I** (6
  US branch offices).

**Applied to the workbook:** Review sheet CATEGORY/COMMENT/REVIEW_REASON/CORRECTED_ISSUER_NAME updated for
all 123 rows; Facility Issuer-Used Reference CATEGORY synced (0 mismatches, full file); 10 rows added to
**Other Inputs Needed** for findings that identified a different real-world entity without a confirmable
secmaster IID (no new IID was invented, per the standing no-new-entity rule); Rule **#86** added to
Rules & M3 Deviations; Progress sheet refreshed; 7 Data Corrections rows added at the time (later revised —
see Part 2).

## Part 2 — REVIEW queue closeout + full-file audit

**Instruction:** "Move these rows 76 open rows to RED (no change needed) category and create the final file
and append markdown with work done in this session. Also review the final file for: (1) issuer-level action
consistency, (2) all sheets in sync, (3) file/secmaster consistency (no redundant/unnecessary change
introduced), (4) no redundant/unnecessary change in Data Corrections, (5) basic per-row issues resolvable via
Data Corrections (facility_id × column_to_correct unique per row)."

### 2.1 — 76 REVIEW rows closed to RED

Per the Legend's own definition of RED ("Excluded facility: no final issuer, active master proposal or data
overlay"), the 76 rows that stayed REVIEW after the Part 1 research pass were moved to RED by explicit user
decision — closing the REVIEW queue to 0 open rows. This is a **policy decision to close the queue**, not a
newly-confirmed per-row exclusion finding: each row's COMMENT/REVIEW_REASON documenting the unresolved
evidence gap from Part 1 was preserved verbatim and a dated note appended. Facility Issuer-Used Reference was
synced: CATEGORY → RED, HANDOFF_STATUS → EXCLUDED_RED, USER_DECISION_EXCEPTION → YES, ISSUER_ID_USED left
blank (per Legend). Rule **#87** logs this decision. Progress sheet CATEGORY/HANDOFF STATUS/ASSIGNMENT
CONTRACT blocks refreshed.

Groups affected (21 of the 43): Brookfield (14), TATA AIG (5), Skyscanner Holdings + Skyscanner Ltd (6),
BUWOG (7), Allied World Holdings I (6), Myhive/IRIDE (1), Grupo Cibest Guatemala/El Salvador remainder (3),
SouthState generic-title rows (4), Fortna global offices (8), Vertex (1), Black Construction (1),
Proin-Pinilla (1), Onduline (1), Meet Group Berlin (1), DyStar Korea (1), ISI Americas (1), PetroChina (1),
Cromology (1), PHS Group (1), Kinyosha (1), Inpac (1), Woopra (1), Tillotts (1), NWL (2), Rossmann Hungary
(2).

### 2.2 — Audit findings and fixes

**(1) Issuer-level action consistency.** Checked ISSUER_ID↔SECMASTER_NAME and GROUP_KEY↔ISSUER_ID mapping
file-wide: **0 inconsistencies** (every ISSUER_ID maps to exactly one SECMASTER_NAME and vice versa).
Checked CORRECTED_ISSUER_ID format file-wide (IID + 15 digits): **0 malformed values**. Checked GROUP_KEYs
carrying more than one distinct CORRECTED_ISSUER_ID: found 5 (Kubota, Promethean World, Creditsafe, SK
Group, PTI Taiwan) — reviewed and confirmed legitimate (large multinational groups routing different
facilities to different country subsidiaries), not an error; none of these were touched this session.

**(2) All sheets in sync.** Full-file Review-vs-Facility Issuer-Used Reference CATEGORY cross-check:
**0 mismatches** across all 17,037 rows. Progress sheet's CATEGORY, HANDOFF STATUS, ACTIVE CORRECTION
PROPOSALS, and ASSIGNMENT CONTRACT blocks recomputed from the live sheets and corrected where stale (REVIEW
row zeroed out, HOLD_NO_APPROVED_FINAL_ISSUER zeroed out, OPEN QUESTIONS "REVIEW facilities" zeroed out,
EXCLUDED_RED/PENDING_CORE_CORRECTIONS/READY_AS_IS_WITHIN_AUDIT_SCOPE and Data Corrections totals refreshed).
0 duplicate FACILITY_IDs; 0 blank required fields (FACILITY_ID/ISSUER_ID/SECMASTER_NAME/FACILITY_NAME/
CATEGORY) anywhere in the file.

**(3) File/secmaster consistency — found and fixed a real issue.** Cross-checking the Facility Issuer-Used
Reference sheet's `EFFECTIVE_*` columns against pre-existing Data Corrections rows showed the file's
established convention: **a Data Corrections entry is a pending, unapplied proposal** — in a sample of 1,053
pre-existing entries (RELEVANT_URL/ASSET_COUNTRY/FACILITY_ADDRESS/FACILITY_NAME), **0** had already been
written into the live Review-sheet column; corrections are tracked separately and only mirrored into
`EFFECTIVE_RELEVANT_URL`/`EFFECTIVE_ASSET_COUNTRY`/`EFFECTIVE_FACILITY_NAME` on Facility Issuer-Used
Reference (there is no `EFFECTIVE_FACILITY_ADDRESS` column — FACILITY_ADDRESS fixes live only in Data
Corrections). Part 1 of this session had broken that convention: it wrote `FACILITY_ADDRESS` directly into
Review for 6 SouthState/Hull & Company rows and corrected `ASSET_COUNTRY` directly for the TATA AIG Vapi row,
*in addition to* logging the same fixes in Data Corrections — a redundant double-application inconsistent
with how the rest of the file operates. **Fix:** reverted all 7 direct Review-sheet edits back to their
original values; the Data Corrections entries remain as the sole record of each proposed fix, matching
convention.

Separately, across all 703 pre-existing RED (excluded) rows, **0** carried any Data Corrections entry or any
`CORRECTED_ISSUER_NAME` — consistent with the Legend's RED definition ("no final issuer, active master
proposal or **data overlay**"). Part 1 had set `CORRECTED_ISSUER_NAME` = "Fuel Pipeline Transportation Ltd."
on the BCPG/Lampang RED row and logged a Data Corrections-adjacent Other-Inputs-Needed entry for it. **Fix:**
cleared `CORRECTED_ISSUER_NAME` on that row for consistency with the RED convention (the FPT finding remains
fully documented in COMMENT and in Other Inputs Needed, which is where an excluded row's follow-up work
belongs); removed the one Data Corrections row that had been logged against it (the TATA AIG Vapi
`ASSET_COUNTRY` fix), since no RED row in the file carries a Data Corrections entry.

**(4) No redundant/unnecessary change in Data Corrections.** Verified file-wide: every `(facility_id,
column_to_correct)` pair is unique (0 duplicate pairs across 1,085 entries); 0 entries whose
`corrected_value` already equals the live Review-sheet value for that column (0 no-ops); 0 entries
referencing a `facility_id` absent from the Review sheet. The one redundant entry introduced in Part 1 (TATA
AIG Vapi `ASSET_COUNTRY`, on a now-RED row) was removed per (3) above.

**(5) Basic per-row issues scanned file-wide and resolved via Data Corrections.** Mechanical, non-judgmental
checks run across all 17,037 rows (ISO 3166-1 alpha-3 validity for ASSET_COUNTRY, lat/lon range and (0,0)
sanity, literal placeholder text such as "None"/"nan"/"null" in key fields, and RELEVANT_URL scheme
validity):
- 5 blank-`ASSET_COUNTRY` rows and several malformed-URL rows found by the scan were already covered by
  pre-existing Data Corrections proposals — no action needed.
- **8 rows** had a `RELEVANT_URL` that is a bare domain with no `http(s)://` scheme (`www.kvk.cz` x3,
  `greeninstruments.com` x2, `brokk.com` x2, `heinolasm.fi` x1) and **no existing Data Corrections entry**.
  Added 8 new Data Corrections rows (facility_id / `RELEVANT_URL` / `https://` + the same domain) — a purely
  mechanical scheme fix, not a new URL target.
- Lat/lon range and placeholder-text checks: 0 findings file-wide.
- Two items flagged by the ISO3 check were investigated and found to be **false positives from the audit
  script itself**, not data errors: `XKX` (the de facto/provisional code commonly used for Kosovo, which has
  no official ISO 3166-1 code) and `ABW` (Aruba's correct ISO3 code — the audit script's reference list had a
  typo). No change made to these rows.

### 2.3 — Final state

| CATEGORY | Rows |
|---|---|
| GREEN | 13,119 |
| LIGHT_GREEN | 54 |
| ORANGE | 2,002 |
| GICS_LAVENDER | 1,158 |
| RED | 704 |
| REVIEW | 0 |
| **Total** | **17,037** |

- Facility Issuer-Used Reference CATEGORY: 0 mismatches vs. Review, file-wide.
- Other Inputs Needed: 10 rows added this session (secmaster-IID-lookup/reassignment follow-ups: Brook
  Crompton UK/Asia-Pacific/Canada/USA, Quail Tools LLC × 3 facilities, LOVOO GmbH, Dematic (M) Sdn Bhd).
- Data Corrections: 1,085 entries, 1,077 distinct facility IDs, 0 duplicate (facility_id, column) pairs,
  0 no-ops, 0 orphaned facility_id references.
- Rules & M3 Deviations: 87 entries (added #86 — Part 1 research pass; #87 — REVIEW queue closeout).
- 0 duplicate FACILITY_IDs; 0 blank required fields; anywhere in the 17,037-row file.

### 2.4 — Open item carried forward

Not independently resolved this session (outside the current instruction's scope): the 5 pre-existing
GROUP_KEYs with multiple distinct `CORRECTED_ISSUER_ID` values (Kubota, Promethean World, Creditsafe, SK
Group, PTI Taiwan) were reviewed and found to be legitimate multi-subsidiary routing, not touched further.

## Part 3 — Questions-for-Review closeout + RELEVANT_URL quality pass (24-Nov-2026)

**Instruction:** "1. You have mentioned relevant_url corrections in Data Corrections sheet - but upon review
a lot of relevant_url corrections are just the source domain URLs (home/landing page) - check all these
cases to confirm if the actual facility details are present on that page or not and accordingly update the
relevant url corrections. 2. Are there any questions for review open still? If so, pls work on resolving
them so that we can close all questions and delete the questions for review sheet."

### 3.1 — Questions for Review sheet

Read all 42 rows in full. 38 were already `HISTORICAL_SUPERSEDED`. Three were open and were resolved:

- **CURRENT_REVIEW** ("123 exact facility-keyed questions", `OPEN_REQUIRED_INPUT`) — closed: confirmed 0
  `CATEGORY=REVIEW` rows remain anywhere in the Review sheet (all 123 closed across Part 1 + Part 2 of this
  session).
- **CURRENT_COORDINATES** ("Ayvens Türkiye saved point", `OPEN_REQUIRED_INPUT`) — closed: facility
  `918b7e8524620ac452929d8648b77979` (Ayvens Türkiye / Istanbul, under SOCIETE GENERALE SA secmaster) had a
  confirmed country/source correction (`ASSET_COUNTRY` IND→TUR, already pending in Data Corrections) but no
  verified replacement for its saved coordinate point, which sits in India (26.08140945, 74.34233093). Sourced
  verified coordinates for the exact confirmed premises (Kağıthane Ofis Park, Bağlar Caddesi No:14 C Blok
  Kat:5, 34406 Kağıthane, İstanbul) from a commercial real-estate listing for that exact building address:
  **lat 41.085034, lon 28.973482**. Added as two new Data Corrections entries (`LATITUDE`, `LONGITUDE`) —
  confidence MEDIUM (third-party listing geocode for the exact address, not an official company-published
  coordinate). Review-sheet `REVIEW_REASON`/`COMMENT` and the Facility Issuer-Used Reference
  `REMAINING_REQUIREMENT` were updated accordingly; `REVIEW_FLAG=COORDINATE_COUNTRY_CONFLICT` left in place as
  the historical record.
- **CURRENT_IMPLEMENTATION** ("Apply the scoped DMX/GICS/Data proposals", `PENDING_IMPLEMENTATION`) — closed
  as non-blocking: this item describes the standing proposal-to-implementation handoff (Data Corrections /
  Rules & M3 Deviations entries are recorded as pending proposals for the M3/DMX systems team to apply, never
  written directly into live Review-sheet columns) — by design, not an audit gap.

All 42 rows confirmed closed/historical; the **Questions for Review sheet was then deleted** per the explicit
instruction.

### 3.2 — RELEVANT_URL quality pass on Data Corrections

Classified all 1,057 `RELEVANT_URL` entries in Data Corrections by URL structure (root/homepage with no
path, query, or fragment vs. a deep link). Found **9 bare-domain/homepage entries**: 1 pre-existing
(`mcrsafety.com.br`) and the 8 added in Part 2 of this session as a purely mechanical missing-scheme fix
(`kvk.cz` ×3, `greeninstruments.com` ×2, `brokk.com` ×2, `heinolasm.fi` ×1) — the Part 2 fix corrected the
scheme but never checked whether the destination page actually showed the facility. Verified each by fetching
the page (and, where the official site didn't show the facility, searching for a better source):

| Facility | Prior URL | Verdict | Action |
|---|---|---|---|
| MCR Safety Brasil (Sede Brasil) | `mcrsafety.com.br/` | Homepage itself shows the exact São Paulo address | Left unchanged |
| Krkonošské vápenky Kunčice ×3 | `www.kvk.cz` | Homepage shows the address; kontakty page repeats it + reg. ID | Upgraded to `https://www.kvk.cz/cs/kontakty.html` |
| Green Instruments A/S (Denmark) | `greeninstruments.com` | Contact page confirms the Brønderslev HQ address | Upgraded to `https://greeninstruments.com/contact/` |
| Green Instruments (S) Pte Ltd (Singapore) | `greeninstruments.com` | Site shows only a generic "Singapore service hub" phone number; a 2018 news article shows a *different*, likely superseded, Woodlands address — the Ang Mo Kio/Techplace facility address is **not** shown anywhere on the official site | Replaced with the Singapore ACRA/company-registry listing (companies.sg, UEN 201824730N), which matches the facility address verbatim |
| Heinolan Sahakoneet Oy (Finland) | `heinolasm.fi` | Homepage + contact page show the address (minor pre-existing name/postcode discrepancy noted, out of scope for a URL fix) | Upgraded to `https://www.heinolasm.fi/ota-yhteytta/` |
| Brokk Middle East Fze (UAE) | `brokk.com` | brokk.com has no UAE-specific page (its UAE selector routes to the global contact page — only a named regional sales manager, no Dubai address); dealer-finder is JS-rendered and couldn't be checked by fetch | Replaced with a Dubai business-directory listing (DCCI Info) whose name and address match exactly (third-party, not an official source, flagged as such) |
| Brokk Ireland Distribution Ltd | `brokk.com` | brokk.com's `/ie/` pages resolve to the global/UK site (Swedish HQ address only); a Brokk news article confirms a Portarlington depot but with no street address | Replaced with the Irish company registry listing (SoloCheck, co. no. 747228), whose registered address matches |

All 8 upgraded `EFFECTIVE_RELEVANT_URL` cells in Facility Issuer-Used Reference were synced to match, and each
affected Review-sheet row received a dated `COMMENT` entry documenting the check and sourcing.

### 3.3 — Post-change verification

- Data Corrections: 1,087 entries (+2 vs. Part 2's 1,085, from the new coordinate rows), **0** duplicate
  (facility_id, column_to_correct) pairs.
- **0** RED rows carry any Data Corrections entry (rule still holds).
- CATEGORY counts unchanged from Part 2: GREEN 13,119 / LIGHT_GREEN 54 / ORANGE 2,002 / GICS_LAVENDER 1,158 /
  RED 704 / REVIEW 0.
- Rules & M3 Deviations: 88 entries (added #88 for this pass).
- Questions for Review sheet: removed (20 sheets remain, down from 21).

## Part 4 — Independent validation of the DMX GICS Only Corrections and DMX Corrections proposals (24-Nov-2026)

**Instruction:** "Two final things to review thoroughly: 1. Is the GICS changes recommended actually valid,
based on web search and other research 2. Is the DMX changes (non GICS) recommended actually valid, based on
web search and other research."

**Method:** split all 247 proposed-correction rows across the two staged-proposal sheets — `DMX GICS Only
Corrections` (97 rows) and `DMX Corrections` (150 rows: 112 `REASSIGN_ISSUER`, 33 `ADD_URL`, 3
`UPDATE_ULTIMATE_PARENT`, 1 `UPDATE_URL`, 1 `ADD_PARENT_LINK`) — into 12 batches and dispatched independent
research passes against each, instructed to verify every row by fresh web search/fetch rather than trust the
sheet's own cited EVIDENCE at face value. Each row got one of three verdicts: VALID, QUESTIONABLE (a real
basis exists but something keeps it short of full confirmation), or INVALID (research contradicts the
proposal, or none could be found). **Neither sheet was applied to the Review sheet as part of this pass —
this was validation only, no workbook edits.**

**Results:**

| Sheet | Rows | VALID | QUESTIONABLE | INVALID |
|---|---|---|---|---|
| DMX GICS Only Corrections | 97 | 81 (83%) | 12 (12%) | 4 (4%) |
| DMX Corrections — REASSIGN_ISSUER | 112 | 70 (63%) | 40 (36%) | 2 (2%) |
| DMX Corrections — ADD_URL / UPDATE_URL / UPDATE_ULTIMATE_PARENT / ADD_PARENT_LINK | 38 | 30 (79%) | 7 (18%) | 1 (3%) |
| **DMX Corrections total** | **150** | **100 (67%)** | **47 (31%)** | **3 (2%)** |

**Invalid rows flagged (7 total, recommended NOT to apply as-is):** GICS — Clarkwestern Dietrich Building
Systems LLC (should be Industrials, not Materials), Amran LLC and KVK (business unidentifiable; KVK's own
evidence suggests it may be a bare Dutch registry reference, not an operating company), Parkland (Hong Kong)
Limited (evidence actually supports a direct Consumer Discretionary call, stronger than the row's own
"review" flag). DMX — Brother Industries → BlackRock, Inc. and Enerpac Tool Group → Argus Research Group Inc
(both bare-evidence entity mixups with a different same-named company), Fred Meyer Stores, Inc. → qfc.com
(cited evidence page never mentions QFC or Fred Meyer).

**Other findings worth acting on:** three `REASSIGN_ISSUER` rows (Fairfax Financial→Go Digit ~49%, Henderson
Land→Towngas ~41.5% with board influence, Ageas→AK Sigorta ~31% JV) move a genuine minority/associate stake
to a full reassignment rather than FP/JV treatment — recommended to re-route rather than apply as a clean
swap. A recurring, independently-confirmed pattern across 13 separate `REASSIGN_ISSUER` rows was also
identified: the facility address in the original data turns out to belong to a third-party registrar, credit
agency, or data vendor (TDCC, Computershare, Creditsafe, CB Insights, Globe and Mail, MyNewsdesk, JSE, Tricor)
rather than the named issuer — a likely systemic artifact in how the original secmaster facility data was
collected, flagged for the data-pipeline owner rather than fixed row-by-row here.

Full per-row verdicts and reasoning for all 247 rows are recorded in the companion file
`Subset_17037_DMX_GICS_Validation_24Nov2026.md`, delivered alongside this log and saved to the same MSCI
folder.
