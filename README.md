# B105 CS1 Digital Forensic Investigation

## Chrome Web History Analysis — Craigslist Sale, Buyer Communication & Cryptocurrency Payment Reconstruction

**Student:** Nasiru Lawal
**Module:** CIP-B105-CS1
**Case Study:** CS1
**Investigation Type:** Digital Forensic Investigation / Web Browser History Analysis
**Investigation Environment:** Kali Linux
**Primary Evidence:** Google Chrome History Database
**Status:** Completed



## 1. Investigation Overview

This investigation examines browser activity recorded in a Google Chrome History database, submitted as evidence in a suspected online sale conducted through Craigslist and settled via a cryptocurrency payment.

The investigation focused on reconstructing the full sequence of browser activity  from the original Craigslist posting, through buyer communication over Gmail, to a payment receipt shared via Imgur and verified against a public blockchain explorer and a cryptocurrency exchange. The main objective was to determine whether the browser history contained sufficient evidence to reconstruct this transaction end-to-end and to attribute the activity to a specific browser profile with an appropriate level of confidence.

The investigation was conducted using Kali Linux and SQLite-based forensic analysis, while maintaining the original evidence separately from the working copies used for examination.


## 2. Investigation Objectives

The main objectives of this investigation were to:

- Examine the available Chrome browser history database.
- Identify relevant websites and user activity.
- Analyse browser visit timestamps and navigation patterns.
- Decode Chrome transition values (core type and qualifier bits).
- Segment browser activity into meaningful investigation stages.
- Investigate Craigslist activity and posting behaviour.
- Examine Gmail-related activity and message threads.
- Investigate Imgur activity and downloaded evidence.
- Identify cryptocurrency transaction information (TXID, wallet address, exchange activity).
- Correlate the different sources of browser evidence into a single narrative.
- Construct a chronological master timeline.
- Preserve evidence integrity through hashing and documented analysis.



## 3. Evidence Examined

The investigation used the following evidence files:

- `History`
- `Chrome_History_Cryptocurrency_Lab.db`
- `Chrome_History_Recreation_Craigslist`
- `Chrome_History_Recreation_Gmail`
- `Chrome_History_Recreation_Gmail_Headers`
- `Chrome_History_Recreation_Imgur`
- `Chrome_History_to_Database_Demo`

`History` is the primary case evidence file. `Chrome_History_Cryptocurrency_Lab.db` was confirmed to be an identical copy of `History` (matching MD5 hash) and was treated as a verification copy rather than independent evidence. The four "Recreation" databases and the generic "Demo" database are instructor-supplied controlled datasets used to validate the examination methodology against known, isolated examples of each activity type (Craigslist posting flow, Gmail navigation, Imgur linking) they are not part of the case's evidentiary chain.

The original evidence was copied into an `evidence` directory before analysis. Working copies were used for database examination to avoid modifying the original evidence.



## 4. Investigation Structure

The investigation directory was organised into the following areas:

```text
B105_CS1_Forensic_Submission/
│
├── evidence/
├── working/
├── queries/
├── logs/
└── screenshots/
```

- **evidence/** — original, hashed copies of all supplied database files
- **working/** — read-only working copies used for all SQLite queries
- **queries/** — saved CSV/text output from every query run during the examination
- **logs/** — hash manifests and query execution logs
- **screenshots/** — numbered figures referenced from the final report



## 5. Tools & Methodology

| Tool | Purpose |
|---|---|
| `sqlite3` (read-only mode) | Direct querying of the Chrome History schema |
| `md5sum` / `sha256sum` | Evidence and working-copy integrity verification |
| Python 3 | Bulk timestamp conversion and transition-bit decoding |

**Methodology summary:**
1. Preserve evidence — hash all supplied files before any analysis.
2. Document the SQLite schema (`urls`, `visits`, `downloads`, `keyword_search_terms`).
3. Convert all Chrome/WebKit timestamps to UTC (`datetime(ts/1000000-11644473600,'unixepoch')`).
4. Decode the `transition` field into core navigation type and redirect/chain qualifiers.
5. Segment consecutive visits into activity stages based on `visit_duration` and `from_visit` chains.
6. Reconstruct each activity thread (Craigslist, Gmail, Imgur, blockchain/exchange) independently.
7. Cross-verify each reconstruction technique against the corresponding instructor-supplied recreation database.
8. Merge all findings into a single chronological, UTC-normalised timeline.
9. Map findings against the case's stage-based hypothesis and assign calibrated confidence levels.



## 6. Key Findings (Summary)

- Full Craigslist posting workflow reconstructed via `FORM_SUBMIT` transition events, terminating in a confirmed, published listing.
- Buyer communication identified across two distinct Gmail message threads.
- A payment receipt image (`proof_of_payment.png`) was downloaded, with its `referrer`/`tab_url` fields directly matching an Imgur link opened from within a Gmail message (`source=gmail` parameter).
- A Bitcoin transaction ID and wallet address were recovered directly from blockchain-explorer URLs and independently corroborated on a second explorer platform reached via a cryptocurrency exchange redirect chain.
- Findings were mapped against a seven-stage hypothesis, with calibrated language applied per stage — not every stage is equally well-supported by browser history alone (see full report for detail).

Full findings, decoded segment tables, and the calibrated final conclusion are documented in the accompanying report.



## 7. Integrated Timeline

All 74 visit records and 1 download record were merged into a single UTC-ordered timeline (`queries/master_timeline.csv`), used as the basis for the report's Integrated Timeline appendix.


## 8. Limitations

- Message *content* is not stored in the History database only that a reply was composed and its timestamp; the actual wording of any wallet-address message could not be independently confirmed from this evidence alone.
- Browser history does not capture events beyond the browser itself (e.g., physical shipment or delivery of any item sold).
- The recreation databases validate technique, not the case itself — they must not be cited as direct evidence of what occurred in the primary case.


## 9. Conclusion

The browser history evidence supports a coherent, chronological reconstruction of an online sale posted on Craigslist, negotiated over Gmail, and settled via a cryptocurrency payment independently verified across two blockchain-explorer platforms. Confidence is high for the posting, payment, and verification stages, and lower for stages involving message content or real-world fulfilment that fall outside what browser history can directly evidence.


## Author

**Nasiru Lawal**
Module: CIP-B105-CS1
