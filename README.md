# Urban renewal in Israel by city (pinui-binui / TAMA 38) — Mitar open data

City-level figures on urban-renewal projects in Israel: how many projects each city has, how many have an approved plan, how many housing units are planned, how long plan approval takes, and how many building permits the Government Urban Renewal Authority reports.

- **Publisher:** Mitar (מתאר), https://mitar.co.il
- **Snapshot in this repository:** generated 2026-11-09 (`generated_at` inside `data/cities.json`).
- **Live version, regenerated daily:** https://mitar.co.il/data/cities.json
- **Coverage:** 81 cities and local authorities, 1,598 project records.
- **Licence:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please credit "מתאר (mitar.co.il)", link to https://mitar.co.il, and give the date of the data.

## Files

| File | Contents |
|---|---|
| `data/cities.json` | The file exactly as published on mitar.co.il. Top-level metadata plus a `cities` array with one object per city. |
| `data/cities.csv` | The same `cities` array as CSV (UTF-8 with BOM, so it opens in Excel with Hebrew intact). |

Each city also has a page on the site: `https://mitar.co.il/city/<slug>` (the `url` column).

## Sources

The figures are aggregated by Mitar from public records:

- **Israel Planning Administration** (מינהל התכנון): the national plans layer (Xplan), for plan deposit dates (`pl_date_advertise`) and approval gazette dates (`pl_date_8`).
- **Government Urban Renewal Authority** (הרשות הממשלתית להתחדשות עירונית): declared compounds, planned units, the "in execution" flag and permit counts.
- A minority of project records (274 of 1,606 on 2026-11-10, about one in six) come from press and developer publications. They are counted in `projects_total`, but their unit figures are **not** summed into any units column.

## Columns

| Column | Meaning |
|---|---|
| `slug` | URL slug of the city page. |
| `name` | Name of the city or local authority, in Hebrew. |
| `district` | Planning district, where known. |
| `projects_total` | All project records matched to the city: official-registry records plus records read off publications. |
| `projects_approved` | Records with a plan-approval gazette date. |
| `projects_in_execution` | Records the Authority flags "in execution" (בביצוע). The flag is set only while permits cover **part** of a compound's units, so it is not a completion count. |
| `units_total` | Planned housing units summed over official-registry records. These are proposed figures from plans and declarations, not completed homes. This is the figure the city page shows as "יח״ד מתוכננות". |
| `units_pipeline` | Same as `units_total`, excluding records flagged "in execution". |
| `declared_total` | Records with an Authority declaration date (declared compounds). |
| `declared_approved` | Declared compounds that also have an approval gazette date. |
| `deposit_approval_samples` | Plans with both a deposit date and an approval date (approval on or after deposit). |
| `median_deposit_to_approval_days` | Median days from deposit to approval over those plans. Xplan fills the deposit date for most district-committee plans and almost no local-committee plans, so this is mostly the **district-committee track**. |
| `approved_without_deposit` | Approved records with no deposit date, which the median leaves out. |
| `permits_samples` | Records for which the Authority reports a permit count. |
| `permits_total` | Sum of the Authority's permit counts. |
| `projects_with_permits` | Records with a permit count above zero. |
| `realization_pct` | 100 × `declared_approved` / `declared_total`. Empty when the city has no declared compounds. |
| `realization_scored_pct` | The same rate, shrunk toward the national rate with a pseudo-count of `shrink_k` (5): (`declared_approved` + 5 × national rate) / (`declared_total` + 5), × 100, capped at 100. This keeps cities with one or two declared compounds from reaching 0 or 100 by chance. |
| `national_declared_pct`, `national_declared_n` | The national declared-to-approved rate and the number of declared compounds it rests on. |
| `shrink_k` | The pseudo-count used above (5). |
| `realization_score`, `score` | Mitar's composite city index (מדד הרשויות), 0–100. It weights the shrunk approval rate (40), approval speed (30: 1 − median days / 1825, clipped to 0–1, or 0.4 when there is no median) and planned units not yet in execution (20: up to 5,000 units), and divides by 0.9. `score` is the same value rounded and clipped to 0–100. **This is Mitar's own heuristic, not an official rating.** |
| `events_90d` | Changes recorded for the city's projects in the last 90 days, such as a new deposit, approval or status change. |
| `*_basis` | Plain-text notes on how a column is computed. |
| `url` | The city page on mitar.co.il. |

## Caveats

- **Not a full census.** The data covers declared compounds, renewal plans identified in the Planning Administration layer, and some publicly reported projects.
- **Planned units are proposals**, not homes built.
- **Approval times** mostly describe the district-committee track (see above). For small cities, `deposit_approval_samples` is small, so read the median with care.
- **The data changes daily.** Cite the date with every figure.

## Citation

> Mitar (מתאר). *Urban renewal in Israel by city (pinui-binui / TAMA 38)*, snapshot 2026-11-09. https://mitar.co.il/data/cities.json. Licensed CC BY 4.0.

## בעברית, בקצרה

נתונים לפי עיר על פרויקטי התחדשות עירונית (פינוי-בינוי ותמ"א 38) ב-81 ערים ורשויות. לכל עיר:
- מספר הפרויקטים;
- כמה מהם קיבלו תוכנית מאושרת;
- יחידות הדיור המתוכננות;
- זמן האישור;
- ההיתרים לפי הרשות הממשלתית להתחדשות עירונית.

**המקורות:** מינהל התכנון והרשות הממשלתית להתחדשות עירונית.

**הרישיון:** CC BY 4.0. בשימוש חוזר מבקשים קרדיט "מתאר (mitar.co.il)", קישור לאתר ותאריך הנתונים.

**הגרסה המתעדכנת כל יום:** https://mitar.co.il/data/cities.json

---

Mitar is a private Israeli company that also offers consulting and organizing services in urban-renewal projects. The data is published for free reuse.
