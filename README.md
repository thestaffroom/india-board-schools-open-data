# India's board-affiliated schools: CBSE, ICSE, Cambridge and IB, matched into one list

This dataset lists every school that India's four main school boards publish as affiliated to them, and matches the listings so that **each school appears once**, even when it holds two or three boards.

- **Snapshot date:** 21 September 2026. Board listings were collected between 25 August and 21 September 2026.
- **Made by:** staffroom (thestaffroom.in), a workplace-transparency platform for India's teachers.
- **Size:** 37,498 board listings, matched to 36,979 schools. 36,710 are in India and 269 are CBSE schools abroad.
- **Disclaimer:** Claude by Anthropic was extensively used for most work. Please help fix issues if any.
 
## The headline numbers (schools in India)

Each school is counted once.

| Which boards the school has | Schools |
|---|--:|
| CBSE only | 32,568 |
| ICSE only | 3,142 |
| Cambridge only | 446 |
| IB only | 110 |
| **One board, total** | **36,266** |
| Cambridge + CBSE | 149 |
| Cambridge + IB | 87 |
| CBSE + ICSE | 67 |
| Cambridge + ICSE | 55 |
| CBSE + IB | 35 |
| ICSE + IB | 8 |
| **Two boards, total** | **401** |
| CBSE + Cambridge + IB | 21 |
| ICSE + Cambridge + IB | 15 |
| CBSE + ICSE + Cambridge | 7 |
| CBSE + ICSE + IB | 0 |
| **Three boards, total** | **43** |
| All four boards | 0 |
| **All schools** | **36,710** |

Per board (a school on two boards appears in both rows):

| Board | Listings | Schools | This board only | Also on another board |
|---|--:|--:|--:|--:|
| CBSE | 32,878 | 32,847 | 32,568 | 279 |
| ICSE (CISCE) | 3,294 | 3,294 | 3,142 | 152 |
| Cambridge | 781 | 780 | 446 | 334 |
| IB | 276 | 276 | 110 | 166 |

From listings to schools (India): 37,229 listings, minus 32 where one board lists a school twice (37,197), minus 487 extra listings of schools on two or three boards, which gives **36,710 schools**.

## The files

| File | One row per | Rows |
|---|---|--:|
| `schools.csv` | School. **Start here.** | 36,979 |
| `school_mapping.csv` | Board listing, pointing to its school, with the reason it was matched | 37,498 |
| `cbse.csv` | CBSE listing (includes 269 schools abroad) | 33,147 |
| `icse.csv` | CISCE listing (ICSE and ISC) | 3,294 |
| `cambridge.csv` | Cambridge International listing | 781 |
| `ib.csv` | International Baccalaureate listing | 276 |

All files are UTF-8 CSV and open in Excel, Google Sheets or any data tool.

## What each column means

### `schools.csv`
| Column | Meaning |
|---|---|
| school_id | This dataset's ID for the school, e.g. `sr90440`. Use it to join files. |
| name | The school's name as its board lists it (CBSE first, then ICSE, Cambridge, IB). |
| boards | The boards the school holds, e.g. `CBSE + Cambridge`. |
| number_of_boards | 1, 2 or 3. |
| state, district | From the school's board listing. Blank for schools abroad. |
| pincode | The school's pincode, if any listing has a trustworthy one. |
| country | `India`, or the country of a CBSE school abroad. |
| udise_plus_id | The school's record number on the government's UDISE+ portal, where we matched one. |
| udise_code_masked | The school's UDISE code as the UDISE+ public portal shows it, with the first six digits hidden. |
| staffroom_url | The school's page on thestaffroom.in. |

### `school_mapping.csv`
| Column | Meaning |
|---|---|
| school_id | The school this listing belongs to. |
| board | `cbse`, `icse`, `cambridge` or `ib`. |
| board_code | The board's own number for the school (CBSE affiliation number, CISCE code, Cambridge centre number, IB school code). |
| udise_plus_id, udise_code_masked | As above. |
| match_type | Why this listing was matched to its school. `own` means it is the school's only listing. |
| match_note | The same reason, in one plain sentence. |
| evidence_url | A public page that backs the match, where the evidence is not already in these files. |

### Board files (`cbse.csv`, `icse.csv`, `cambridge.csv`, `ib.csv`)
Each keeps what the board itself published, with personal details removed.

| Column | Meaning |
|---|---|
| affiliation_no / cisce_code / centre_number / ib_code | The board's own number for the school. |
| school_code (CBSE) | CBSE's separate school number. |
| name, address, district, state | As the board lists them. Phone numbers and e-mail addresses were removed from address text. |
| locality (ICSE) | The locality CISCE gives. |
| country (CBSE) | `India`, or the country of a school abroad. |
| pincode, pincode_source | See **Pincodes** below. |
| level (CBSE) | Secondary (to Class 10) or Senior Secondary (to Class 12). |
| affiliated_from, affiliated_to (CBSE) | The current affiliation period. |
| school_type, medium, gender, year_founded, managing_trust (CBSE) | As CBSE publishes them. `gender` is the school's (boys, girls, co-ed), not a person's. |
| courses (ICSE) | `ICSE` (Class 10) and/or `ISC` (Class 12). |
| classification, on_current_register (ICSE) | Day/boarding, and whether the school is on CISCE's current list. |
| latitude, longitude (ICSE) | The map point CISCE's own school locator gives. |
| programmes (Cambridge) | Cambridge stages offered: Primary, Lower Secondary, Upper Secondary (IGCSE), Advanced (AS & A Level). |
| programmes (IB) | PYP (Primary Years), MYP (Middle Years), DP (Diploma), CP (Career-related). |
| registered_on (Cambridge), ib_since (IB) | When the school joined the board. |
| boarding, teaching_languages (IB) | As IB publishes them. |
| website | The school's website as the board lists it. Entries that were e-mail addresses were removed. |
| source_url, captured_on | The board page the listing came from, and when. |

## How to use it

- **Just want the list?** Open `schools.csv`. One row is one school, and `boards` tells you which boards it has.
- **Want a board's details for a school?** Find the school's rows in `school_mapping.csv`, then look up each `board` + `board_code` in that board's file.
- **In code:** join a board file to `school_mapping.csv` on board and code, then count distinct `school_id`. For example, in pandas: `m = pd.read_csv('school_mapping.csv'); m.groupby('school_id').board.nunique().value_counts()`.

## How listings were matched

The rule: **one school is one campus under one school name.** A campus that teaches two boards is one school, even if each board section has its own head or its own government registration. Two differently named schools with their own websites are two schools, even when they share a compound.

Every listing that shares a school with another listing carries one of these reasons:

| match_type | Listings | What it means |
|---|--:|---|
| same_website | 437 | Same website as the other listing of this school. |
| same_name_pincode | 291 | Same name and pincode, and no other school with that name in that pincode. |
| same_address | 94 | Checked by hand: the listings give the same street address. |
| board_listed_twice | 64 | The board lists the school twice (renewed or re-entered). |
| same_name_locality | 62 | Checked by hand: same school name in the same locality, and no other campus of it there. |
| school_says_one_campus | 33 | Checked by hand: the school's own website says one campus runs both boards. |
| same_govt_code | 6 | Both listings match the same UDISE+ school record. |
| board_map_point | 3 | Checked by hand: CISCE's own map point sits at the other listing's address. |
| same_trust_same_address | 2 | Checked by hand: same managing trust and same address. |
| not_confirmed | 2 | Same name, but one listing gives no address, so the campus is not confirmed. |

Nothing was matched on a similar name alone.

Examples of schools deliberately **kept apart**, because they are separately named schools on one site: Sacred Heart Convent School and Sacred Heart Convent International School (Ludhiana); Cygnus World School and Cygnus International School (Vadodara); Seedling Modern High School and Seedling Modern International Academy (Jaipur); Presidency School and Presidency Indo International School (Bhiwandi).

## Pincodes

Every pincode says where it came from:

| pincode_source | Meaning | Listings |
|---|---|--:|
| board | From the board's own listing | 18,318 |
| cbse_2018_dataset | From a 2018 copy of CBSE's school directory published on GitHub https://github.com/deedy/cbse_schools_data/tree/master | 18,575 |
| india_post_directory | Looked up from the listing's address in India Post's All India Pincode Directory (government open data) | 49 |
| same_school_other_board | Taken from the same school's listing with another board | 26 |
| *(blank)* | No trustworthy source | 530 |

- **India Post lookup:** an address gets a pincode only when it names exactly one post office in the listing's own district, or writes the pincode itself.
  - City-level offices (GPO, head office) are never used.
  - Places whose name also belongs to a different locality are left blank.
- **2018 pincodes:** these can be out of date if a school has moved since 2018.

## Known limits

- **Snapshot.** Boards add, renew and withdraw schools all the time. Check the board's own site before relying on a single school's status.
- **Lookalike branches.** About 670 pairs of same-name schools in the same district, mostly branches of one chain, were not checked one by one. They are kept as separate schools.
- **Two pairs are unproven either way:** S.M. Convent School, Ferozeshah, and Sudarshan Vidya Mandir, Jayanagar. They are kept as separate schools.
- **UDISE codes are masked** by the government portal; we publish them as shown.
- **CBSE's affiliation dates** are as CBSE lists them and can lag renewals.
- **Schools abroad:** 269 CBSE schools outside India are included and marked by `country`.

## Privacy

No personal data is included: no names of principals or heads, no phone numbers, no e-mail addresses. Where a board's address text contained a phone number or e-mail, it was removed.

## Sources

- **CBSE:** SARAS affiliation portal (saras.cbse.gov.in).
- **CISCE:** school locator (locate.cisce.org).
- **Cambridge International:** Find a Cambridge school directory.
- **International Baccalaureate:** IB World School finder (ibo.org).
- **UDISE+:** Ministry of Education's school portal.
- **India Post:** All India Pincode Directory (data.gov.in).
- **CBSE directory copy (2018):** https://github.com/deedy/cbse_schools_data/tree/master.

Each board's own data remains subject to that board's terms of use.

## Licence

The matching, the match notes and `schools.csv` are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): you may reuse them for any purpose, including commercially, if you credit **staffroom (thestaffroom.in)**. Each board's own listing data remains subject to that board's terms.

## Found a mistake?

Open an issue in this repository with the school, the board code and a link to the evidence.
