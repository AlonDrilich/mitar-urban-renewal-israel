# Urban renewal in Israel by city (pinui-binui / TAMA 38): Mitar open data

City-level figures on urban-renewal compounds and plans in Israel, from government records. For each of 80 cities and local authorities:
- how many compounds the Government Urban Renewal Authority has declared in each track, and how many of them have been approved;
- the median time from plan deposit to approval;
- planned housing units.

This is the data behind Mitar's authorities index (מדד הרשויות), https://mitar.co.il/madad-rashuyot. **It has no score and no ranking.** Each measure is published on its own, with its sample size.

- **Publisher:** Mitar (מתאר), https://mitar.co.il
- **Snapshot in this repository:** generated 2026-11-18, methodology version `2026-11-18-c` (`generated_at` and `methodology_version` inside `data/cities.json`). It is updated by hand, so it may lag the site.
- **Current version:** https://mitar.co.il/data/cities.json. It is regenerated whenever the site is published; its date is in `generated_at`. The table on https://mitar.co.il/madad-rashuyot reads the database live, and daily snapshots are listed there under "גרסאות קודמות".
- **Coverage:** 80 cities and local authorities, 1,304 government records.

## Not an official publication

Mitar's authorities index is Mitar's processing of open government data. It is not an official publication, and it is not made on behalf of, or sponsored by, the State, the Government Urban Renewal Authority or the Planning Administration.

The figures reflect what the government records say. They are not an assessment of how well a local authority works: many planning stages depend on others, such as the district planning committee, developers and apartment owners.

## What changed on 2026-11-18

The previous snapshot in this repository (2026-11-09) included a composite 0–100 score per city (`score`, `realization_score`) and the columns used to compute it. **That score has been withdrawn. Please do not use or cite it.**
- **Why:** it depended on chosen weights, it filled missing data with default values, and it could be read as a rating of named local authorities.
- **The details:** see Mitar's public corrections log, https://mitar.co.il/corrections.
- **The old file:** it is still in this repository's git history, as a record only.

## Files

| File | Contents |
|---|---|
| `data/cities.json` | The file exactly as published on mitar.co.il on the snapshot date: top-level metadata and a `cities` array with one object per city. |
| `data/cities.csv` | The same `cities` array as CSV. It is UTF-8 with BOM, so it opens in Excel with the Hebrew intact. Empty cells mean null. |

Each city has a page on the site at `https://mitar.co.il/city/<slug>` (the `url` column).

## Sources and licences

| Source | Used for | Licence |
|---|---|---|
| Government Urban Renewal Authority: declared-compounds layer `GIS_UrbanRenewal` (ArcGIS, Ministry of Construction and Housing). https://services6.arcgis.com/I08Ekaykft5ELucH/arcgis/rest/services/GIS_UrbanRenewal/FeatureServer/1 | Declared compounds and their track, approval date (`taarichTokef`) and status (`codeStatusHarashut`) | Public; no licence stated |
| Government Urban Renewal Authority / Ministry of Construction and Housing: the urban renewal table on data.gov.il. https://data.gov.il/he/datasets/ministry_of_housing/urban_renewal | Declaration details, permits, "in execution" flag | data.gov.il terms of use, https://data.gov.il/he/terms-of-use |
| Israel Planning Administration: Xplan. https://data.gov.il/he/datasets/iplan/xplan_services | Plan deposit and approval-gazette dates, committee level | CC BY |

Press and developer publications are **not** counted. A record that appears twice is counted once.

## Licence and attribution

- **Mitar's processing:** the processing, selection and arrangement are published under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- **The government data:** copyright in the government data they contain belongs to the State of Israel, and its use is subject to the source licences above.
- **How to credit:** "מתאר (mitar.co.il), על בסיס נתוני הרשות הממשלתית להתחדשות עירונית (משרד הבינוי והשיכון) ומנהל התכנון". Also link to https://mitar.co.il and give the date of the data.

## How the numbers are counted

- **Declared:** a compound is declared if it is in the Authority's layer with a track: the Authority track (מסלול רשויות) or the tax track (מסלול מיסוי).
- **Approved:** any one of these counts:
  - a gazette publication of the plan's approval in Xplan;
  - an approval date (`taarichTokef`) from the Authority;
  - the Authority's own "approved plan" status (`codeStatusHarashut` 3 or 4).
- **Authority-track percentage:** shown only when a city has at least 5 declared compounds in that track. Next to it are the declaration years, and how many compounds were declared in the last two years. A recently declared compound has not had time to go through planning. A low percentage in a city whose compounds are mostly new therefore says nothing about its pace.
- **Tax track:** only counts are given, with no percentage. In this track the declaration often comes after the plan's approval.
- **Median from deposit to approval:** shown only when a city has at least 10 plans with both dates. Each plan is counted once per city.
- **Which committee approves:** Xplan records the deposit date for most district-committee plans and almost no local-committee plans. The median therefore mostly describes the district-committee track, and the district committee is a government body, not the local authority.
- **Removed compounds:** compounds the Authority removed from its layer are not counted as projects or units. Their plans stay in the median, because Xplan's dates for them are still correct.
- **When a value is missing:** it is null, with no default value.

## Columns (`cities` array)

| Column | Meaning |
|---|---|
| `slug`, `name`, `url` | City page slug, Hebrew name, city page on mitar.co.il. |
| `district` | Planning district, where set. |
| `projects_total` | Government records matched to the city (Authority layer and Xplan), excluding compounds the Authority removed from its layer. Duplicates are counted once. |
| `units_total` | Planned housing units summed over those records. These are proposals from plans and declarations, not homes built. |
| `units_pipeline` | Same as `units_total`, excluding records the Authority flags "in execution". |
| `declared_total` | Declared compounds (both tracks). |
| `authority_track_total`, `authority_track_approved` | Declared compounds in the Authority track, and how many are approved. |
| `authority_track_pct` | 100 × approved / total, only when `authority_track_total` ≥ 5; otherwise null. |
| `authority_track_decl_year_min`, `authority_track_decl_year_max` | Earliest and latest declaration year among the Authority-track compounds. |
| `authority_track_decl_last_2y` | How many of them were declared in the two years before `generated_at`. |
| `authority_track_decl_undated` | How many have no declaration date; these are left out of the years. |
| `tax_track_total`, `tax_track_approved` | Declared compounds in the tax track, and how many are approved. No percentage. |
| `median_deposit_to_approval_days` | Median days from deposit to approval-gazette publication. Only when `approval_samples` ≥ 10; otherwise null. |
| `approval_samples` | Plans with both dates (approval on or after deposit), each plan counted once per city. |
| `approvals_by_district_committee` | How many of those plans the district committee approved (Xplan committee level 2). |
| `approval_year_min`, `approval_year_max` | Years of approval covered by the sample. |
| `projects_approved` | Records with an approval-gazette date in Xplan. |
| `approved_without_deposit` | Approved records with no deposit date; the median leaves these out. |
| `projects_in_execution` | Records the Authority flags "in execution" (בביצוע). The flag is set only while permits cover part of a compound's units, so it is not a completion count. |
| `projects_with_permits` | Records with a permit count above zero, as reported by the Authority. |
| `events_90d` | Changes recorded for the city's records in the last 90 days, such as a new deposit, approval or status change. |
| `*_basis` | Plain-text notes on how a column is computed. |

The top level of `data/cities.json` also has these fields:
- `methodology_version` and `changelog`;
- `tracks_basis`, `attribution`, `source_licenses` and `not_official_he`;
- `projects_not_in_file_breakdown`, which breaks down every government record that is not in the city rows: press reports, marked duplicates, compounds removed from the layer, and records in a locality outside the index.

## Caveats

- **Not a full census.** The data covers the Authority's declared compounds and renewal plans in the Planning Administration's layer.
- **Planned units are proposals**, not homes built.
- **Small samples.** Read a city's figures together with their sample sizes.
- **The data changes.** Cite the date with every figure.
- **Found an error?** Write to info@mitar.co.il with the subject "תיקון במדד הרשויות – [שם הרשות]". Every request is checked within 14 days, and corrections are logged at https://mitar.co.il/corrections.

## Citation

> Mitar (מתאר). *Urban renewal in Israel by city (pinui-binui / TAMA 38)*, snapshot 2026-11-18, methodology 2026-11-18-c. Based on data from the Government Urban Renewal Authority (Ministry of Construction and Housing) and the Israel Planning Administration. https://mitar.co.il/data/cities.json. Mitar's processing licensed CC BY 4.0.

## בעברית, בקצרה

**מה בקובץ:** נתונים לפי רשות על מתחמי התחדשות עירונית, מרשומות ממשלתיות בלבד. לכל אחת מ-80 רשויות:
- כמה מתחמים הוכרזו בכל מסלול, וכמה מהם אושרו;
- החציון מהפקדה לאישור;
- יחידות הדיור המתוכננות.

**אין ציון ואין דירוג.** הציון המשוקלל שהופיע בגרסה הקודמת של המאגר (9.11.2026) בוטל ב-18.11.2026. אין להשתמש בו. הפירוט ביומן התיקונים: https://mitar.co.il/corrections.

**לא פרסום רשמי:** מדד הרשויות הוא עיבוד של מתאר לנתונים ממשלתיים פתוחים. הוא אינו פרסום רשמי, ואינו נעשה מטעם המדינה, הרשות הממשלתית להתחדשות עירונית או מנהל התכנון, או בחסותם.

**רישיון וייחוס:**
- **העיבוד של מתאר:** ב-CC BY 4.0.
- **המידע הממשלתי:** כפוף לרישיונות המקור.
- **הייחוס:** "מתאר (mitar.co.il), על בסיס נתוני הרשות הממשלתית להתחדשות עירונית (משרד הבינוי והשיכון) ומנהל התכנון", עם תאריך הנתונים.
