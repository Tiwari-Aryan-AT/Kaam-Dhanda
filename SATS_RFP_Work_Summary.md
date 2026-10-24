# RFP Ser Jin 1 SATS — Facility Mapping: Work Summary

This note covers everything done in this session to produce `SATS_RFP_Facility_Mapping.xlsx`, continuing the same process used for the earlier RFP Natalia project.

## Scope and brief

The source file was `SATS.xlsx`, listing four issuers:

| Issuer ID | Company | Website |
|---|---|---|
| IID000000002183869 | SATS LTD. | sats.com.sg |
| IID000000002838409 | Worldwide Flight Services Limited | wfs.aero |
| IID000000005139531 | Asia Airfreight Terminal Co Ltd | aat.com.hk |
| IID000000005467390 | Country Foods Pte Ltd | countryfoods.com |

The task was to exhaustively identify company-owned physical facilities for each issuer by crawling each company's own site and every site it redirects to (subsidiaries, businesses, brand sites it owns), excluding mobile assets and franchisee/distributor relationships, verifying the owning entity for every facility, and cross-referencing a secmaster dataset for subsidiary issuer IDs where possible. The standing sourcing rule throughout: build everything from scratch off each company's own site/sitemap and official filings; third-party sources (including Wikipedia) could only be used to enrich an already-confirmed fact, never as the primary source for a new one.

## Process, in the order it actually happened

**Scoping.** An initial crawl established that this was a much larger footprint than "four companies" — SATS and WFS combined operate a global network of 200+ locations across 27+ countries. This was surfaced to the user before committing to full execution, along with a proposal to mirror the Natalia project's 8-sheet workbook structure.

**First pass — core crawl.** Using Claude in Chrome and two parallel subagents, the WFS global station directory (18 paginated pages, 171 distinct stations across air cargo/ground handling/ramp/baggage services) and the five SATS "portfolio brand sites" linked from the sats.com.sg footer (SATS Cargo, SATS Baggage, MBCCS/Marina Bay Cruise Centre, SATS Seletar, Monty's Bakehouse) were crawled into separate CSVs, alongside direct crawls of the AAT and Country Foods sites and SATS's own Changi-area facilities. Every facility's activity type was checked against WFS's own service-tag taxonomy to confirm these are legitimate operational cargo/ground-handling stations, not retail or hospitality assets, before being retained.

**Mid-course correction on geocoding.** An initial requirement for 5-decimal-precision coordinates on every facility was dropped once it became clear no geocoding API was reachable from this environment; the user agreed to drop it rather than have the project stall on it.

**Second pass — closing gaps after an honest exhaustiveness check.** When asked directly whether the first pass was genuinely exhaustive, the answer was no, and a second pass was run specifically to close what had been missed:
- Read SATS's own SGX Rule 706A filing (Singapore's periodic subsidiary-change disclosure), surfacing 10 additional entities (SATS Asia-Pacific Star, SATS Investments, SATS-Mit Food Solutions/SATS Food Solutions India, Cemerlang/PT Cardig Aero Services, WFS Sweden AB and its two Swedish subsidiaries), plus 3 correctly-excluded divested/dissolved/minority items.
- Found and read two SATS AGM "operations review" pages (Food Solutions 2021, Gateway Services 2022) — primary-source pages on sats.com.sg not linked from the main navigation — surfacing 11 further named facilities/entities across China, India, Malaysia, Oman and Singapore.
- Found satshk.com (SATS HK Limited) via web search, a joint venture with Hong Kong Airlines, plus its subsidiary AAS (Asia Airfreight Services Ltd) via an unopened nav item on AAT's own site.
- Attempted to read SATS's full Annual Report PDFs (both the SGX-hosted 2022-23 version and the sats.com.sg-hosted 2025 version) for their formal subsidiary list. This failed at the time: WebFetch returned only binary data for both, and Chrome's PDF viewer exposed no extractable text layer.

**Third pass — full domain sitemap sweep, and a renewed PDF-access attempt via Claude in Chrome.** Prompted by a direct question about whether the domain and filings had truly been exhaustively searched:
- Pulled the complete sats.com.sg HTML sitemap (144 internal links) directly from the page DOM rather than relying on nav/footer traversal, and read every page not already visited — investor/corporate-information pages, the SATS-WFS acquisition microsite, and every "SATS in the news" article. This surfaced one new facility (the SATS Immersive Centre, a training/simulation facility built with Singapore Polytechnic) and confirmed two other leads (a new BUP handling centre, the Marina Bay Cruise Centre) were already captured under existing rows.
- Re-attempted PDF access: confirmed Chrome's native PDF viewer still gave no extractable text and that screenshotting a loaded PDF tab reliably timed out. Found a working technique — executing a same-origin `fetch()` + `.blob()` call via Chrome's JavaScript execution tool — which successfully retrieved the full 11.2MB 2025 Annual Report PDF and triggered its browser download. Parsing it further then hit two independent walls: this session's automated request for the user's Downloads-folder access was blocked, and loading a PDF-parsing library (pdf.js) into the page was blocked by sats.com.sg's own Content Security Policy (it won't fetch scripts from third-party CDNs, and a neutral page in turn can't fetch the PDF cross-origin since sats.com.sg sends no CORS headers). Both paths were reported to the user rather than worked around.

**Fourth pass — the Annual Report, finally read.** The user uploaded the FY2025 Annual Report PDF directly to the conversation. It was parsed locally with `pypdf` (122 pages, clean text extraction) and searched for its formal subsidiary/associate/joint-venture disclosure (Notes 20–22) and operations-review narrative — the single most authoritative source used across the whole project, being part of the audited financial statements. This:
- Corrected two previously-unstated ownership percentages (Ground Team Red Holdings and Air India SATS/AISATS, both confirmed at exactly 50%).
- Added four new facility entities named as "material" associates/joint ventures: PT Jasa Angkasa Semesta (Indonesia, ground/cargo handling), Evergreen Sky Catering Corporation (Taiwan, inflight catering), FCS Frankfurt Cargo Services GmbH (Germany, air cargo), and Dunwoody Airline Services Limited (United Kingdom, air cargo).
- Added one new Rejected item: SATS-operated kitchens at an Australian defence training area (Camp Tilpal, Camp Growl, Sam Hill) — catering delivered at a client site, excluded on the same logic as earlier hospital-catering contracts.
- Logged five further legal entities (pure holding/treasury subsidiaries, the US WFS operating entity, the newly-acquired Amsterdam and Brazil entities) in the crossref sheet, each mapped back to a facility already captured, rather than creating duplicate rows at airports already in the dataset.

## Technical obstacles and how they were resolved or documented

- **Secmaster parquet cross-referencing** was attempted via multiple installation paths (pip, apt, npm) in both the cloud sandbox and the user's linked computer, and via manual byte-level parquet parsing; no usable engine could be installed or reached in either environment (pypi.org, npm registry, and the raw Thrift/Parquet binary format were all dead ends without undue risk). Concluded infeasible and documented as such; the user later confirmed this was acceptable and deprioritized it.
- **Annual Report PDF access** was blocked by three independent failures in sequence (no text layer in Chrome's viewer, a renderer freeze on screenshot, a sandboxed network that can't reach sats.com.sg directly) before a working in-browser fetch technique was found, and even that hit folder-access and CSP walls — ultimately resolved only once the user supplied the PDF directly.
- **Geocoding** (5-decimal-precision coordinates) was found to be unreachable (no API access in either environment) and was dropped from scope by the user.

## Final deliverable

`SATS_RFP_Facility_Mapping.xlsx`, structured identically to the RFP Natalia precedent with eight sheets: Executive_Summary, Facilities, RegOffice_NonOperating, Rejected, Crawl_Coverage, New_Domains_Discovered, Subsidiary_Secmaster_Crossref, and Methodology_Notes (the latter documenting all four passes, every technical obstacle, and every judgment call in full prose, section by section).

Final counts: **207 Facilities rows**, **18 Rejected rows**, **31 Subsidiary_Secmaster_Crossref rows** — all QA-checked for zero missing issuer IDs, addresses, sources, or activity types. The file was delivered to the conversation and saved to the user's `RFP Ser Jin 1 SATS` folder, with each successive update re-delivered and re-saved in place.
