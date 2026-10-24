# Answer key (instructor only, gitignored)

All numbers below are real output of `python3 reference_solution.py`. Students' numbers may
differ and still be correct: what matters is that their rules are written and their rows add up.

## 1. Bronze

| Question | Answer |
|---|---|
| Rows | **300** |
| Distinct students (`student_id`) | **285** (15 students answered twice) |

## 2. Silver: the decisions

| Problem in bronze | Reference decision | Also acceptable |
|---|---|---|
| City spellings: `Jogja`, `YK`, `Yogya`, `Jaksel`, `Smg`, `Sby`, `Bdg` | Map to one standard name | Any mapping, if written |
| `Kab. Sleman` / `Sleman` | Its own city, in DI Yogyakarta province | Merged into Yogyakarta |
| `Surakarta` | Same as Solo | |
| `Bali` | Treated as Denpasar | Rejected as "not a city" |
| Hometown `-`, `n/a`, `asdf`, `Mars` | **Reject**: `city_unknown` | |
| Dates in 8 formats | Slash and dash dates are **day first** (Indonesian) | Month first, if consistent and written |
| Birthday after the survey date | **Reject**: `birthday_in_future` | Keep with blank birthday |
| Blank or unreadable birthday | Keep the row, birthday blank | |
| Coffee `1-2`, `12+`, `every day`, `none`, `0 cups` | `1.5`, `12`, `7`, `0`, `0` | Lower or upper bound for ranges |
| Coffee `?`, `idk`, blank | Keep the row, coffee blank | |
| Coffee negative | **Reject**: `coffee_negative` | |
| Coffee `50` (over 21 a week) | Keep, flag `coffee_is_outlier = true`, leave out of gold averages | Reject, or cap |
| Same `student_id` twice | **Latest `submitted_at` wins** (a resubmission corrects) | Earliest wins, if written |
| `kopi`, `Es Teh Manis`, `bubble tea` | Coffee, Tea, Boba | |
| `samsung`, `iphone` | Android, iOS | |

Order used: validate each row first (reject), then remove duplicates among valid rows.

## 3. Silver: expected result

| Metric | Value |
|---|---|
| Bronze rows | 300 |
| Rejected | **8** (city unknown 4, birthday in future 2, coffee negative 2) |
| Duplicates removed | **15** |
| Silver rows (one per student) | **277** |
| Coffee outliers flagged | 1 |
| Blank birthday / coffee / drink | 5 / 7 / 4 |

**Accounting check: 300 = 277 + 8 + 15.**

`silver/survey_response.csv` columns:
`student_id, response_id, submitted_at, city, birthday, age, coffee_per_week, coffee_is_outlier,
favorite_drink, device_os, _loaded_at, _source_file`

`silver/survey_rejected.csv`, the 8 rows:

| response_id | student_id | hometown | birthday | coffee | reject_reason |
|---|---|---|---|---|---|
| R0058 | S1135 | `-` | 25 March 2004 | 10 | city_unknown |
| R0060 | S1211 | Jaksel | 2003-07-07 | -1 | coffee_negative |
| R0130 | S1199 | Mars | 13/05/2005 | 6-7 | city_unknown |
| R0151 | S1123 | Semarang | 2027-11-07 | 14 | birthday_in_future |
| R0162 | S1098 | Jakarta | Feb 23 2001 | -2 | coffee_negative |
| R0163 | S1262 | n/a | 22-01-2006 | 4 cups | city_unknown |
| R0184 | S1178 | Yogyakarta | 2027-07-14 | 3 | birthday_in_future |
| R0242 | S1009 | asdf | 06 November 2005 | 7 | city_unknown |

## 4. Gold: `students_by_region.csv` (required)

| Province | City | Students | Share % |
|---|---|---|---|
| DI Yogyakarta | Yogyakarta | 86 | 31.0 |
| DKI Jakarta | Jakarta | 47 | 17.0 |
| West Java | Bandung | 25 | 9.0 |
| East Java | Surabaya | 24 | 8.7 |
| Central Java | Solo | 20 | 7.2 |
| Central Java | Semarang | 16 | 5.8 |
| West Java | Bekasi | 14 | 5.1 |
| East Java | Malang | 14 | 5.1 |
| DI Yogyakarta | Sleman | 13 | 4.7 |
| South Sulawesi | Makassar | 7 | 2.5 |
| North Sumatra | Medan | 6 | 2.2 |
| Bali | Denpasar | 5 | 1.8 |

By province: DI Yogyakarta 99 (35.7%), DKI Jakarta 47 (17.0%), West Java 39 (14.1%),
East Java 38 (13.7%), Central Java 36 (13.0%), South Sulawesi 7, North Sumatra 6, Bali 5.

If a student merged Sleman into Yogyakarta, they get Yogyakarta 99 at city level. That is fine
if it is written down.

## 5. Go further

`gold/coffee_by_city.csv` (blanks and outliers excluded):

| City | Answers | Avg | Median | Min | Max |
|---|---|---|---|---|---|
| Yogyakarta | 83 | 4.32 | 3 | 0 | 14 |
| Jakarta | 46 | 5.07 | 3.75 | 0 | 14 |
| Bandung | 25 | 4.00 | 4 | 0 | 10 |
| Surabaya | 22 | 4.70 | 3.25 | 0 | 13 |
| Solo | 19 | 3.24 | 3 | 0 | 12 |
| Semarang | 16 | 3.94 | 3.25 | 1 | 9 |
| Bekasi | 14 | 6.18 | 6.5 | 1 | 11.5 |
| Malang | 14 | 5.21 | 3.75 | 0 | 14 |
| Sleman | 13 | 4.54 | 4 | 1.5 | 11 |
| Medan | 6 | 3.83 | 4 | 2 | 5 |
| Makassar | 6 | 5.58 | 5 | 1 | 11 |
| Denpasar | 5 | 4.40 | 2 | 0 | 13 |

`gold/drink_preference_by_age_group.csv`, top drink per group (age on the survey day):

| Age group | Top drink | Share in group |
|---|---|---|
| 20 or under | Tea | 31.7% |
| 21-22 | Coffee | 35.9% |
| 23-24 | Coffee | 27.9% |
| 25+ | Coffee | 38.4% |

`silver/ref_city.csv`: 12 cities with province and island (Java, Sumatra, Sulawesi, Bali).

## 6. Debrief (5 minutes)

1. Ask three students for their silver row count. Expect different numbers.
2. Ask what each decided for **Sleman**, the **15 resubmissions**, and the **`50` cups**.
3. Show the accounting check: can each student say where every bronze row went?
4. Point: the same raw data gives different answers until the rules are written down.
   That is the silver layer's job.
