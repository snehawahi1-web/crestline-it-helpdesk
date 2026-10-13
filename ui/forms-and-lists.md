# Forms & Lists — Crestline Asset Request

## Objective

Configure and test the Crestline Asset Request form and list to make support requests easier to read, search, sort, and manage.

---

## Form Configuration

The form layout was reviewed and set in the following order:

| Order | Field |
|---:|---|
| 1 | Number |
| 2 | Requested for |
| 3 | Request type |
| 4 | Description |
| 5 | Asset |
| 6 | Priority |
| 7 | Status |
| 8 | Assignment group |
| 9 | Assigned to |

### Description Field

The default Description field length of 40 characters was insufficient for realistic request descriptions. It was increased to **1000 characters**. A long test description was entered and saved successfully without being truncated.

---

## List Configuration

The list columns were configured in the following order:

1. Number
2. Requested for
3. Request type
4. Description
5. Priority
6. Status
7. Assignment group
8. Assigned to

This layout prioritizes the information support staff need to identify requests, understand the issue, assess priority, and check assignment details.

---

## Test Records

| Number | Description | Priority | Status |
|---|---|---|---|
| CRES0001003 | Laptop keyboard is not responding | High | New |
| CRES0001004 | Microsoft Teams is not launching | Moderate | New |

---

## Testing Results

| Test | Result | Observation |
|---|---|---|
| Priority sorting | Passed | Sorting by Priority was tested, and the list order changed according to the platform's priority sorting order. |
| Status filtering | Passed | Filtering for Status = New returned both records, since both already had that status. |
| Number search | Passed | Searching for `CRES0001004` returned the Microsoft Teams request. |

**Testing note:** The tests confirmed that sorting and search worked, and the Status filter returned the matching records. Since both test records had the same status, a follow-up test using records with different statuses would provide stronger evidence that the filter excludes non-matching records.

---

## Outcome

The Crestline Asset Request form and list are configured for easier use. The form supports longer descriptions, while the list presents key request and assignment information in a consistent order. Sorting, filtering, and search were tested and documented.

---

## Skills Practiced

- Form layout configuration
- Field length configuration
- List column configuration
- List sorting and filtering
- Searching for specific records
- Functional testing and documentation
