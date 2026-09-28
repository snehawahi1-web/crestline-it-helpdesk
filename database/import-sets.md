# Import Sets & Transform Maps — Crestline Employee Import

## Objective

Import employee data from a CSV file into ServiceNow using an Import Set and a Transform Map, load it into the User (`sys_user`) table, and verify that coalescing on email updates existing records instead of creating duplicates.

All data is fictional.

---

## Source Data

| Field | Description |
|---|---|
| `first_name` | Employee first name |
| `last_name` | Employee last name |
| `email` | Employee email address |
| `department` | Employee department |

Three sample employees were used across IT, HR and Finance.

---

## Configuration

### Import Set

**Import Set table:** `Crestline Employee Import`

The table was used as the staging table for the CSV data.

### Transform Map

| Setting | Value |
|---|---|
| Name | Crestline Employee Import |
| Source table | Crestline Employee Import |
| Target table | User (`sys_user`) |
| Active | Yes |
| Run business rules | Yes |
| Enforce mandatory fields | No |
| Copy empty fields | No |

### Field Maps

| Source | Target | Coalesce |
|---|---|---|
| `first_name` | First name | |
| `last_name` | Last name | |
| `email` | Email | Yes |
| `department` | Department | |

Email was configured as the coalesce field.

---

## Workflow

```text
CSV file
   ↓
Load Data
   ↓
Import Set staging table
   ↓
Transform Map
   ↓
User (`sys_user`)
   ↓
Verification
```

---

## Troubleshooting: blank last name after the first import

**Symptom.** After the first transform, imported users had a blank Last name even though the CSV contained one. The Import Set run finished as "Completed with errors," and the row error read: *unable to format Sharma using format yyyy-MM-dd HH:mm:ss*.

**Checked and ruled out:**
- The `last_name` field map pointed at Last name (correct).
- `first_name` and `last_name` on the staging table were both type String.
- The `last_name` field map had no source script and no date format.

**Finding.** The source-field dropdown showed a strangely named column, `u_` followed by a long string of garbled characters, alongside the normal `first_name` column. The CSV had been exported from Excel with invisible characters at the start of the first header. ServiceNow treated that as a separate column, and the First name field map was pointing at it.

**Fix.**
1. Retyped the first header cell and re-saved the CSV as a plain CSV.
2. Loaded the clean file into the same staging table.
3. Re-pointed the First name field map to the normal `first_name` source.
4. Re-ran the transform.

**Result.** The transform completed with 0 errors, and users showed correct first name, last name, email, and department.

**Caveat.** I confirmed the header problem and the wrong source mapping, but I did not isolate why the last name specifically failed on the first run. The working import is the fix, not a proven root cause for that one field.

---

## Coalesce Test

After a second import of the same three employees, filtering Users by email ending in `crestline.example` returned **3 rows**, not 6, so no duplicates were created.

**Update test.** I changed Neha's department from HR to Finance in a new file, loaded it, and ran the transform.

- Total users after the run: **3**
- Neha's department: **Finance** (was HR)
- Transform History: inserts **0**, updates **1** (only Neha's row had a changed value)

This shows coalesce matched the existing record on email and updated it instead of inserting a new one.

---

## What I Learned

- Import Sets are a staging area; nothing reaches the target table until the transform runs.
- Changing a field map does not change data that was already imported; the transform has to be re-run.
- Hidden characters at the start of a CSV header can create a separate, misnamed staging column that field maps then point at.
- Department on `sys_user` is a reference field, so a text value only links if a matching record exists.
- Verify results in the target table, not just the Import Set run status.

---

## Result

The Crestline Employee Import workflow loads CSV data through an Import Set and Transform Map into the User table, with email coalesce confirmed to prevent duplicates.
