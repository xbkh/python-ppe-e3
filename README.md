# PPE Python — Fundamentals (class E3)

**Log and documentation of the 5 PPE projects of the week** for the course *Python — Fundamentals*,
with screenshots of each program running.

The five projects follow the briefs in the [course repository](https://github.com/Satom-IT-Learning-Solutions/E3/tree/main/02-python-principes-de-base)
(Swiss grading scale, mark out of 6, pass mark 4.0). Each one ships with its code, automated
tests, documented manual tests, user guide and screenshots. Last updated: **30 October 2026**.

## Contents

- [Progress](#progress)
- [Day 1 — Calculator and unit converter](#day-1--calculator-and-unit-converter)
- [Day 2 — Command-line task manager](#day-2--command-line-task-manager)
- [Day 3 — Analysing a sales CSV file](#day-3--analysing-a-sales-csv-file)
- [Day 4 — Public API client](#day-4--public-api-client)
- [Day 5 — Mini application tested with pytest](#day-5--mini-application-tested-with-pytest)
- [Week summary](#week-summary)
- [Reproducing the results](#reproducing-the-results)
- [How the screenshots were produced](#how-the-screenshots-were-produced)

## Progress

The "days" number the projects in the order of the brief. The dates show when each one was
actually completed: several projects were done on the same day.

| Day | Project | Level | Completed on | Auto. tests | Manual cases | Screenshots | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | [Calculator and converter](./projet-1-calculatrice-convertisseur/) | Beginner | 26 Oct | 20 | 28 | 6 | Done |
| 2 | [CLI task manager](./projet-2-gestionnaire-taches-cli/) | Beginner / Intermediate | 28 Oct | 21 | 25 | 6 | Done |
| 3 | [Sales CSV analysis](./projet-3-analyse-csv-ventes/) | Intermediate | 28 Oct | 29 | 21 | 6 | Done |
| 4 | [Public API client](./projet-4-client-api-publique/) | Intermediate / Advanced | 30 Oct | 24 | 22 | 5 | Done |
| 5 | [Mini application tested with pytest](./projet-5-mini-application-tests-pytest/) | Advanced | 30 Oct | 165 | : | 5 | Done |
| | **Total** | | | **259** | **96** | **28** | |

All automated tests pass. Projects 1 to 4 only use the Python standard library (plus `requests`
for project 4, which the brief requires); project 5 needs `pytest` and `pytest-cov`.

---

## Day 1 — Calculator and unit converter

**Project:** [`projet-1-calculatrice-convertisseur/`](./projet-1-calculatrice-convertisseur/) ·
**Full documentation:** [README](./projet-1-calculatrice-convertisseur/README.md),
[manual tests](./projet-1-calculatrice-convertisseur/TESTS-MANUELS.md),
[user guide](./projet-1-calculatrice-convertisseur/NOTICE.md)

Menu-driven command-line tool: six operations (addition, subtraction, multiplication,
division, power, modulo) and three converters (temperature, length, weight). One function
per operation, pure logic separated from the menu, and input handling that asks again until
the value is valid.

**Highlights**
- Logic and interface are separated: the functions neither read nor print anything, so they
  can be tested on their own. The input function is a parameter (`saisie=input`).
- Conversions go through a **pivot unit**: adding a unit means adding one line to a table.
- `nan`, `inf` and `1e999` are rejected (Python accepts them as valid numbers); absolute zero
  is checked for temperatures.
- **Results:** 20 automated tests, 28 manual cases, all passing.

### Screenshots

**1. Main menu**

![Main menu](./projet-1-calculatrice-convertisseur/screenshots/01-menu-principal.png)

**2. Calculations: addition, then division by zero rejected**

![Calculations and division by zero](./projet-1-calculatrice-convertisseur/screenshots/02-calculs-division-par-zero.png)

**3. Conversions: temperature, length, weight**

![Conversions](./projet-1-calculatrice-convertisseur/screenshots/03-conversions.png)

**4. Invalid input: menu choice and numbers**

![Invalid input, numbers](./projet-1-calculatrice-convertisseur/screenshots/04-saisies-invalides-nombres.png)

**5. Invalid input: units and absolute zero**

![Invalid input, units](./projet-1-calculatrice-convertisseur/screenshots/05-saisies-invalides-unites.png)

**6. Automated tests**

![Automated tests](./projet-1-calculatrice-convertisseur/screenshots/06-tests-automatises.png)

**Notable difficulty:** the first version of the screenshot script was out of sync (one extra
input). Spotted while reviewing the image, fixed, screenshots regenerated.

---

## Day 2 — Command-line task manager

**Project:** [`projet-2-gestionnaire-taches-cli/`](./projet-2-gestionnaire-taches-cli/) ·
**Full documentation:** [README](./projet-2-gestionnaire-taches-cli/README.md),
[file format](./projet-2-gestionnaire-taches-cli/FORMAT-FICHIER.md),
[manual tests](./projet-2-gestionnaire-taches-cli/TESTS-MANUELS.md),
[user guide](./projet-2-gestionnaire-taches-cli/NOTICE.md)

Task manager (add, list with filters, complete, delete) saved as JSON: tasks survive closing
the program. Split into three modules (model, storage, interface).

**Highlights**
- **Atomic** saving (temporary file, then rename): a power cut cannot truncate the file.
- An **invalid JSON file is never overwritten**: the program refuses to continue instead of
  starting from an empty list and destroying the data.
- Persistence is proven with **two separate terminals** (the first is closed before the
  second one starts).
- **Results:** 21 automated tests, 25 manual cases, all passing.

### Screenshots

**1. Help and adding tasks**

![Add and list](./projet-2-gestionnaire-taches-cli/screenshots/01-ajout-et-liste.png)

**2. Complete, delete, filter, errors**

![Complete, delete, errors](./projet-2-gestionnaire-taches-cli/screenshots/02-terminer-supprimer-erreurs.png)

**3. Persistence, before closing** (list and JSON content of the file)

![Persistence before](./projet-2-gestionnaire-taches-cli/screenshots/03-persistance-avant.png)

**4. Persistence, after restart** (new terminal: same tasks found)

![Persistence after restart](./projet-2-gestionnaire-taches-cli/screenshots/04-persistance-apres-redemarrage.png)

**5. Corrupted file rejected and never overwritten**

![Corrupted file](./projet-2-gestionnaire-taches-cli/screenshots/05-fichier-corrompu.png)

**6. Automated tests**

![Automated tests](./projet-2-gestionnaire-taches-cli/screenshots/06-tests-automatises.png)

**Notable difficulty:** the screenshots used `clear; command` on a single line, which erased
the command line from the image. Fixed by using two separate keystrokes. Accepted limitation:
the ID of the last deleted task is reused (documented).

---

## Day 3 — Analysing a sales CSV file

**Project:** [`projet-3-analyse-csv-ventes/`](./projet-3-analyse-csv-ventes/) ·
**Full documentation:** [README](./projet-3-analyse-csv-ventes/README.md),
[calculation method](./projet-3-analyse-csv-ventes/METHODE.md),
[manual tests](./projet-3-analyse-csv-ventes/TESTS-MANUELS.md),
[user guide](./projet-3-analyse-csv-ventes/NOTICE.md),
[generated summary](./projet-3-analyse-csv-ventes/resultats/synthese.txt)

Reads a CSV export of 320 sales (fictional dataset, Q3 2026, generated with a fixed seed) and
computes statistics: revenue, average per sale, best-selling product, groupings by month,
region, category and product, filters, sorting and export of a summary. Uses the `csv`
module, no pandas.

**Highlights**
- Amounts use `Decimal`: **exact** sums. Revenue: CHF 89'754.00.
- An invalid row is **skipped and reported** (line number + reason) without stopping the
  analysis.
- **Cross-check:** the indicators are recomputed with `awk`, without the Python code;
  identical results to the cent.
- **Results:** 29 automated tests, 21 manual cases, all passing.

### Screenshots

**1. CSV preview and full analysis**

![Full analysis](./projet-3-analyse-csv-ventes/screenshots/01-analyse-complete.png)

**2. Filters, sorting and top** (August, Vaud region, products sorted by quantity)

![Filters, sorting, top](./projet-3-analyse-csv-ventes/screenshots/02-filtres-tri-top.png)

**3. Invalid rows skipped** (7 rejected out of 10 rows, with their reason)

![Rejected rows](./projet-3-analyse-csv-ventes/screenshots/03-lignes-rejetees.png)

**4. Summary export**

![Summary export](./projet-3-analyse-csv-ventes/screenshots/04-export-synthese.png)

**5. Cross-check with `awk`**

![Cross-check](./projet-3-analyse-csv-ventes/screenshots/05-verification-croisee-awk.png)

**6. Automated tests**

![Automated tests](./projet-3-analyse-csv-ventes/screenshots/06-tests-automatises.png)

**Notable difficulty:** alphabetical sorting placed "Écran" after "Souris" (accented letters
come after `z` in character order). Found by a test, fixed with a sort key that ignores
accents and case.

---

## Day 4 — Public API client

**Project:** [`projet-4-client-api-publique/`](./projet-4-client-api-publique/) ·
**Full documentation:** [README](./projet-4-client-api-publique/README.md),
[error handling](./projet-4-client-api-publique/GESTION-ERREURS.md),
[manual tests](./projet-4-client-api-publique/TESTS-MANUELS.md),
[user guide](./projet-4-client-api-publique/NOTICE.md),
[output examples](./projet-4-client-api-publique/exemples/sortie-exemples.txt)

Command-line client for the public **Frankfurter** API (European Central Bank exchange rates,
no key needed) using `requests`: amount conversion, rate of the day or of a past day, rate
history with minimum, maximum, average and change, list of currencies. The API was chosen
without being able to consult the instructor: to be validated with them.

**Highlights**
- Four distinct kinds of errors (input, network, failed HTTP response, unusable response); no
  Python traceback is ever displayed.
- **Selective retries** with increasing delay: network errors, timeouts, 429 and 5xx are
  retried; other 4xx never.
- A rate requested on a weekend is **flagged**: the API answers with the last business day.
- Tests run against a **fake local HTTP server**: the real network code is tested without
  Internet. The manual cases, on the other hand, were run against the real API.
- **Results:** 24 automated tests in 0.7 s, 22 manual cases, all passing.

### Screenshots

The rates shown are those of 29 October 2026.

**1. Help and conversions**

![Help and conversions](./projet-4-client-api-publique/screenshots/01-aide-et-conversions.png)

**2. Rates, and a weekend rate**

![Rates and weekend](./projet-4-client-api-publique/screenshots/02-taux-et-week-end.png)

**3. One-month history and list of currencies**

![History and currencies](./projet-4-client-api-publique/screenshots/03-historique-et-devises.png)

**4. Error handling** (unknown currency, invalid input, unreachable API with 3 attempts in 1.6 s)

![Error handling](./projet-4-client-api-publique/screenshots/04-gestion-des-erreurs.png)

**5. Automated tests**

![Automated tests](./projet-4-client-api-publique/screenshots/05-tests-automatises.png)

**Notable difficulty:** `convertir 100 CHF CHF` produced an unhelpful `HTTP 422: bad currency
pair`. It is now rejected locally with a clear message, and covered by a test.

---

## Day 5 — Mini application tested with pytest

**Project:** [`projet-5-mini-application-tests-pytest/`](./projet-5-mini-application-tests-pytest/) ·
**Full documentation:** [README](./projet-5-mini-application-tests-pytest/README.md),
[business case and rules](./projet-5-mini-application-tests-pytest/CAS-METIER.md),
[execution report](./projet-5-mini-application-tests-pytest/rapport/)

A **mini bank** (current accounts with overdraft, savings accounts with interest, all-or-nothing
transfers, statements) and its pytest test suite. The business case is a personal choice among
the examples in the brief, to be validated with the instructor. Money is a `Decimal`, never a
`float`.

**Highlights**
- **165 tests** (77 functions, parametrised): nominal cases, edge cases (exact boundary value
  then one cent beyond), rounding, hostile input, transfer atomicity.
- **100% line and branch** coverage.
- **Mutation testing:** 23 bugs deliberately introduced into the code, 23 caught by the tests.
  100% coverage only says the code runs, not that the tests check anything; mutation testing
  verifies that.
- Exported **execution report**: pytest output, JUnit XML, coverage, mutation.

### Screenshots

**1. Application demo**

![Demo](./projet-5-mini-application-tests-pytest/screenshots/01-demonstration.png)

**2. The tests, one per line** (165 passed)

![pytest -v](./projet-5-mini-application-tests-pytest/screenshots/02-pytest-verbose.png)

**3. Line and branch coverage** (100%)

![Coverage](./projet-5-mini-application-tests-pytest/screenshots/03-couverture.png)

**4. A deliberate bug is detected** (temporary copy of the code: 9 failures)

![Deliberate bug detected](./projet-5-mini-application-tests-pytest/screenshots/04-bug-volontaire-detecte.png)

**5. Mutation testing** (23 out of 23)

![Mutation testing](./projet-5-mini-application-tests-pytest/screenshots/05-test-de-mutation.png)

**Notable difficulties:** the `coverage` configuration was in the wrong place, so test files
were counted (a misleading "100%"); mutation testing revealed a **redundant** type check in
the code (removed); a transfer rollback turned out to be **unreachable**, so it was removed.

---

## Week summary

| Finding | Detail |
| --- | --- |
| Everything is verified, not just claimed | every figure in the tables comes from a real run; the manual records note the **observed** output |
| Tests found real defects | alphabetical sorting with accents (day 3), identical currency sent to the API (day 4), redundant and dead code (day 5) |
| Independent cross-checks | `awk` against the Python code (day 3); mutation testing against the test suite (day 5) |
| Common thread | separate logic from input/output so it can be tested; validate before acting; never overwrite data on error; money as `Decimal` |

**Limitations to be aware of**
- The datasets (day 3) are fictional; the API (day 4) is real but the rates change.
- The day 4 API and the day 5 business case are to be validated with the instructor, as the
  briefs require.
- The screenshots come from an isolated virtual terminal, not the machine's desktop (see
  below). The `argparse` messages (invalid options) remain in English.
- Tests and code were written by the same person: a cross-review would be useful.

## Reproducing the results

Prerequisites: Python 3.8 or newer (tested with 3.12.3, Linux).

```bash
git clone https://github.com/xbkh/python-ppe-e3.git
cd python-ppe-e3

# Days 1 to 3: nothing to install
(cd projet-1-calculatrice-convertisseur && python3 -m unittest -v)
(cd projet-2-gestionnaire-taches-cli   && python3 -m unittest -v)
(cd projet-3-analyse-csv-ventes        && python3 -m unittest -v)

# Day 4: requests (the tests do not need Internet)
(cd projet-4-client-api-publique && python3 -m venv .venv && .venv/bin/pip install -r requirements.txt \
   && .venv/bin/python -m unittest -v)

# Day 5: pytest and pytest-cov
(cd projet-5-mini-application-tests-pytest && python3 -m venv .venv && .venv/bin/pip install -r requirements.txt \
   && .venv/bin/pytest --cov)
```

## How the screenshots were produced

Each project contains a `tools/capture.sh` script. It starts an isolated virtual screen (Xvfb)
and a terminal (`xterm`), **actually types the commands** (`xdotool`) and then captures the
screen (`scrot`). The programs therefore really run; no screenshot shows the machine's
desktop. Day 4 queries the real API: the rates change from one run to the next.
