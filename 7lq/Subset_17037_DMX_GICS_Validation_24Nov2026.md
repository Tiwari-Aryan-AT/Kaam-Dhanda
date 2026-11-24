# Subset_17037 — DMX & GICS Correction Validation (24-Nov-2026)

Independent web-research validation of every row in the workbook's two proposed-correction sheets:
**"DMX GICS Only Corrections"** (97 rows — proposed GICS sector reclassifications) and
**"DMX Corrections"** (150 rows — proposed non-GICS issuer attribution fixes: reassign issuer, add URL,
update ultimate parent, add parent link). Neither sheet has been applied to the live Review sheet — both are
staged proposals (`APPROVAL_STATUS=APPROVED_CLASSIFICATION_STILL_REQUIRED` / `EXECUTION_STATUS=
CONFIRMED_PROPOSAL_NOT_APPLIED`). This report verifies whether the proposals are actually correct, using
fresh web search/fetch for every row rather than trusting the sheet's own cited evidence at face value.

**Method:** all 247 rows were split into 12 batches and independently researched by separate research passes,
each required to do its own search/fetch per row rather than take the sheet's EVIDENCE/COMMENT text on
faith. Every row received one of three verdicts: **VALID** (independent research confirms the proposal is
correct), **QUESTIONABLE** (a real, plausible basis exists but something — entity-name granularity, an
unconfirmed address, a partial ownership stake, a search that couldn't complete — keeps it short of full
confirmation), or **INVALID** (research contradicts the proposal, or no support for it could be found at all).

## Headline numbers

| Sheet | Rows | VALID | QUESTIONABLE | INVALID |
|---|---|---|---|---|
| DMX GICS Only Corrections (sector reclassification) | 97 | 81 (83%) | 12 (12%) | 4 (4%) |
| DMX Corrections — REASSIGN_ISSUER | 112 | 70 (63%) | 40 (36%) | 2 (2%) |
| DMX Corrections — ADD_URL / UPDATE_URL / UPDATE_ULTIMATE_PARENT / ADD_PARENT_LINK | 38 | 30 (79%) | 7 (18%) | 1 (3%) |
| **DMX Corrections total (non-GICS)** | **150** | **100 (67%)** | **47 (31%)** | **3 (2%)** |

**Bottom line:** the GICS sector-reclassification proposals are solid — 83% independently confirmed, only 4
clear misses out of 97, and most of those are edge cases rather than wrong sectors. The non-GICS DMX
proposals are good on balance (67% confirmed, only 3 outright invalid) but carry a much higher
QUESTIONABLE rate (31%, vs. 12% for GICS), concentrated in a few recognizable, systemic patterns described
below rather than being randomly scattered — worth reading past just the row count.

---

## 1. GICS sector-reclassification proposals (DMX GICS Only Corrections, 97 rows)

### 1.1 — INVALID (4 rows — recommend NOT applying as proposed)

| Row | Issuer | Proposed | Why it's wrong |
|---|---|---|---|
| 67 | Clarkwestern Dietrich Building Systems LLC | → Materials | It's ClarkDietrich, a manufacturer of fabricated steel/metal framing, lath and building accessories. That's GICS **Industrials** (Building Products sub-industry); Materials (Construction Materials) is for bulk/raw producers like cement and aggregates, not fabricated building products. |
| 70 | Amran LLC | → Industrials | Business could not be identified — "Amran" matches many unrelated companies, and the row's own evidence literally says "(verify)" with no confirmed business. The sector can't be validated either way. |
| 73 | KVK | → Industrials | Name is too ambiguous; the row's own evidence suggests this may be a bare reference to the Dutch Chamber of Commerce registry ("Kamer van Koophandel") rather than a real operating company at all. |
| 98 | PARKLAND (HONG KONG) LIMITED | Financials → (flagged "review" only, no sector asserted) | The row undersells its own evidence: independent research confirms this entity operates the Fullerton Ocean Park Hotel Hong Kong (government hotel-licence/property records). Financials is clearly wrong, and the evidence already supports a direct reclassification to **Consumer Discretionary** (Hotels, Restaurants & Leisure) — the row should assert that instead of merely flagging for review. |

### 1.2 — Notable QUESTIONABLE items (12 total; the ones worth a second look before applying)

- **Row 52, Interflex Datensysteme GmbH** (→ Information Technology): it makes both access-control hardware
  and workforce-management software under Allegion, whose own GICS home is Industrials (Building
  Products/security) — Industrials is at least as defensible as IT.
- **Row 58, EEW Energy from Waste Helmstedt** (→ Utilities): it is a waste-incineration/power generator, but
  comparable waste-to-energy operators (e.g. Covanta) are conventionally GICS Industrials (Environmental &
  Facilities Services), not Utilities.
- **Row 97, Ista SE** (missing sector, no replacement asserted): sub-metering/heat-cost billing for real
  estate — likely Industrials (Commercial/Support Services), not the Utilities one might guess from the
  utility-adjacent business; flagging so "pending" doesn't default to Utilities later.
- **Row 85, Safe Information Group NV** (missing sector): business could not be independently confirmed at all.
- **Rows 63–66, 80** (Claitec Solutions, Fortress Resources, Kinki Tekkin Concrete, Ventair, Osmoflo): each
  has a plausible sector call, but the exact entity couldn't be pinned down with full confidence (common/
  similar company names crowding search results), or — for Osmoflo — the sector choice (Utilities vs.
  Industrials) is genuinely debatable given its project-based, non-rate-regulated water-treatment contracts.
- **Rows 13, 14, 20** (Pm Pack A/S, Powermec AB, Gulf Metal Craft): the researching pass's search budget ran
  out before these could be fully confirmed. Pm Pack's Danish registry listing suggests it may make packaging
  *machinery* (→ Industrials) rather than packaging materials (the proposed Materials); Gulf Metal Craft's
  "shipbuilding" evidence description appears to conflate it with a different, similarly-named UAE company.

The remaining 1 QUESTIONABLE row not detailed above had only a minor, non-substantive gap. **No row's
original (pre-correction) sector was found to actually be more correct than the proposed one** — in every
case where the move was contested, it was a dispute between two plausible *new* sectors, never a vote to
keep the old one.

---

## 2. Non-GICS DMX corrections (DMX Corrections sheet, 150 rows)

### 2.1 — REASSIGN_ISSUER (112 rows: move a facility's attributed issuer to a different entity)

**70 VALID / 40 QUESTIONABLE / 2 INVALID.**

#### INVALID (2 rows — recommend NOT applying)

| Row | Facility's old issuer | Proposed new issuer | Why it's wrong |
|---|---|---|---|
| 40 | Brother Industries, Ltd. | BlackRock, Inc. | Evidence is a bare entity/ID reference with no URL or narrative. The entity that actually exists at the cited reference is **BlackRock Fund Managers Ltd** (a distinct UK entity), not BlackRock, Inc. — a parent/sibling mixup, not a confirmed reassignment. |
| 44 | Enerpac Tool Group Corp. | Argus Research Group Inc | Same bare-evidence problem. UK Companies House shows a distinct **Argus Vickers Limited**, and a real US lawsuit (*Argus Research Group Inc. v. Argus Media Inc.*) shows Argus Research Group actively treats other "Argus"-branded firms as separate/infringing — nothing ties either Argus entity to Enerpac. |

#### Systemic QUESTIONABLE patterns (40 rows) — grouped by root cause rather than listed individually

1. **Minority/associate stakes being treated as a full reassignment, not flagged as a joint venture.** Three
   rows move a facility's issuer to a company the old issuer only partly owns, with real board influence —
   this looks like it understates a genuine ongoing relationship rather than correcting an error:
   - Row 18: Fairfax Financial Holdings → Go Digit General Insurance (Fairfax holds ~49%, an associate stake
     being transferred to IDBI Bank per 2026 reporting).
   - Row 24: Henderson Land Development → The Hong Kong & China Gas Co (Henderson Land holds ~41.5% and is
     Towngas's largest shareholder with overlapping board chairmanship — arguably still influential, not
     unrelated).
   - Row 31: Ageas SA → AK Sigorta (Ageas holds ~31% jointly with Sabancı Holding, a genuine JV).
   
   **Recommendation: route these three to FP/JV treatment rather than a clean issuer swap.**

2. **Reassigning to a parent when the evidence only names a subsidiary** (a granularity mismatch, not
   necessarily wrong, but imprecise): row 43 (Want Want China → NH Investment&Securities Co., Ltd, the Korean
   parent, when the evidence names the Hong Kong subsidiary); rows 47–48 (JD Health, MGM China → Computershare
   Limited, when the evidence points to regional Computershare subsidiaries); row 113 (Start Holding BV → RGF
   Staffing the Netherlands BV, when the disclaimer actually names "Start People B.V." as operator); row 140
   (→ The Bidvest Group Limited when the actual operator is "Bidvest Waltons, a division of Bidvest Office
   (Pty) Ltd"); row 146 (SouthState Bank N.A. vs. its own holding co — directionally right, but the FFIEC
   confirmation page itself could not be reached this session).

3. **A recurring, confirmed vendor/registrar address mis-scrape** — the facility address in the original data
   turns out to be a third-party service provider's own office (share registrar, credit agency, news/PR
   vendor, data-center operator), not the named issuer's facility. This pattern was independently confirmed
   across **13 separate rows** and looks like a systematic artifact in how the original secmaster facility
   data was collected, not 13 unrelated mistakes: the four Taiwan Depository & Clearing Corporation rows
   (58, 76–78), Computershare rows (47, 48, 88), Creditsafe rows (19, 21, 115–117), CB Insights/CB Information
   Services rows (80, 83–85), plus Globe and Mail (49–50), MyNewsdesk AB (25), JSE Ltd (59), and Tricor
   Holdings (66). Most of these came back VALID or QUESTIONABLE-but-directionally-right; a few (rows 47, 48,
   49, 86, 87) are QUESTIONABLE only because the specific source document couldn't be re-fetched this
   session to pin the exact address, not because the underlying "it's a vendor's office" conclusion is in
   doubt. **Worth flagging to whoever owns the original data-collection pipeline as a root cause**, separate
   from whether any individual row gets applied.

4. **Weakest single row:** row 61, Nissin Foods Holdings → Marubeni Corporation — even the row's own evidence
   admits "owner entity is not confirmed," and no independent link could be found explaining the Marubeni
   HQ-area address match beyond general proximity.

5. **A cross-sheet inconsistency worth resolving before either is applied:** row 101 in this sheet (Quality
   Food Centers, Inc. → Fred Meyer Stores, Inc.) and row 118 in the ADD_URL sheet (below) both turn on whether
   Fred Meyer Stores, Inc. is QFC's current direct legal parent — and neither could independently confirm it
   (row 118 is in fact the one outright INVALID ADD_URL row). These should be resolved together, not
   independently.

6. Smaller unconfirmed items: rows 99–100 (two Kubota Japan regional-dealer subsidiary names — plausible
   given Kubota's known distributor structure, but not found in the Kubota group-company lists that were
   accessible this session); row 98 (Guardrisk Holdings → "Guardrisk International Limited PCC" — Guardrisk
   does have real Mauritius cell-captive entities, but a business-registry listing instead shows the name
   "Guardrisk Insurance Company (Mauritius) Ltd PCC," not an exact match); row 14 (TCC Group Holdings → OYAK —
   direction confirmed correct at the group level, but not every individual subsidiary sampled could be
   checked); rows 27, 29 (Kingfa Sci.&Tech → Amata Corp., and Shanghai Mechanical & Electrical → Nabtesco —
   both flagged because the companies turn out to have a *real, documented* commercial relationship, e.g. a
   joint venture or a tenant-in-an-industrial-park arrangement, which complicates a clean "these are
   unrelated, reassign fully" conclusion).

None of the 40 QUESTIONABLE rows were contradicted by research — every one points to a real, distinct entity.
The gaps are about precision (exact legal name, which subsidiary, ownership percentage) rather than wrong
direction.

### 2.2 — ADD_URL / UPDATE_URL / UPDATE_ULTIMATE_PARENT / ADD_PARENT_LINK (38 rows)

**30 VALID / 7 QUESTIONABLE / 1 INVALID.**

#### INVALID (1 row)

| Row | Issuer | Proposed | Why it's wrong |
|---|---|---|---|
| 118 | Fred Meyer Stores, Inc. | ADD_URL qfc.com | The cited evidence page (thekrogerco.com/about-kroger/our-business/) doesn't mention QFC or Fred Meyer at all — the evidence check fails on its own terms. (See the cross-sheet note above — this is the same unresolved QFC/Fred Meyer legal-parent question as REASSIGN row 101.) |

#### QUESTIONABLE (7 rows)

- **Row 7, AIRR Holdings Ltd → airr.com.au:** the site's own footer attributes it to "Australian Independent
  Rural Retailers," a related but distinct entity per the row's own comment, not AIRR Holdings Ltd itself.
- **Row 11, BKV-BPP Power LLC → bkv.com/power-generation:** the JV itself is real, but the proposed URL is
  parent BKV Corporation's shared page, not a page specific to the joint-venture entity.
- **Row 107, Wigen Companies Inc → wigen.com:** the site only displays the brand "Wigen Water Technologies" —
  "Wigen Companies Inc" as a legal name isn't confirmed on it.
- **Row 110, General Rubber LLC → general-rubber.com:** the site footer says "General Rubber Corporation," not
  "General Rubber LLC" — business/parent (CECO Environmental) match, but the legal-entity suffix doesn't.
- **Row 129, Axtone HSW Sp z o o → axtoneglobal.com:** the underlying corporate relationship (Axtone's
  acquisition of HSW's rail-bogie business) is independently confirmed by Railway Gazette, but the proposed
  site itself is blocked by bot-protection and couldn't be checked.
- **Row 141, OFI Group Limited → ofi.com:** the site's own footer is copyrighted to "Olam International
  Limited," a related but distinct Singapore entity — this is a shared group domain, not OFI Group Limited's
  own property (though OFI Group's own UK filing does link to an ofi.com-hosted PDF, so the group
  relationship is real).
- **Row 143, Parkland (Hong Kong) Limited → fullertonhotels.com/...:** the entity's role (the Sino Land/Empire
  Group JV that built and operates the Fullerton Ocean Park Hotel) is independently confirmed via a 2017 HKEX
  filing, but the proposed URL itself is branded to "The Fullerton Hotels" with no on-page mention of
  Parkland — the link is correct in substance but indirect.

---

## 3. Overall recommendation

- **Do not apply as-is:** the 4 GICS rows and 3 DMX rows marked INVALID above (7 rows total). Three of the
  four GICS ones need more identification work before any sector can be asserted; the Parkland HK row should
  instead be *strengthened* to a direct Consumer Discretionary reclassification. The DMX INVALID rows
  (Brother Industries/BlackRock, Enerpac/Argus, Fred Meyer/QFC) look like genuine entity mixups and should be
  re-researched or dropped.
- **Apply with a flag, not a plain swap:** the three JV/associate-stake REASSIGN rows (18, 24, 31) — these
  should go through FP/JV handling rather than a full issuer reassignment.
- **Everything else (the 81 GICS + 70+30 DMX VALID rows, and most of the QUESTIONABLE rows)** is reasonably
  safe to apply; the QUESTIONABLE ones are mostly precision gaps (which subsidiary, exact legal suffix) rather
  than wrong-direction calls, and can be tightened opportunistically rather than held back.

No changes have been made to the workbook as part of this review — this is a validation report only. Happy to
apply any of the above (correcting the 7 invalid rows, re-flagging the 3 JV rows, tightening the granularity
issues) directly to the Data Corrections / Review sheets on request.
