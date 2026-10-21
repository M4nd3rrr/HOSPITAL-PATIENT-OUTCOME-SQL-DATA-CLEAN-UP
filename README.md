# HOSPITAL-PATIENT-OUTCOME-SQL-DATA-CLEAN-UP
DATA EXPLORATION, HANDLING DUPLICATES, NULLS, INCONSISITENCY IN A FICTIONAL HOSPITAL DATASET
## Data Cleaning Process

### 1. Deduplication Using a Composite Key

The dataset did not provide a unique record identifier. To identify repeated records, I used `Encounter_ID` and `Patient_ID` together as a composite key.

I applied the `ROW_NUMBER()` window function, partitioning the data by both columns and ordering records by `Admission_Date` in descending order. This ranked the most recent record first within each patient–encounter combination.

I then filtered the results using `WHERE HD = 1` to retain the most recent record for each combination.

**SQL techniques used:**
- `ROW_NUMBER()`
- `PARTITION BY`
- `ORDER BY DESC`
- `COALESCE()`

### 2. Data Standardization and Missing Value Handling

I standardized inconsistent categorical values and handled selected missing values to improve data consistency for subsequent analysis.

**Sex:** Standardized values such as `F` and `Fem` to `Female`, and `M` to `Male`.

**Department:** Consolidated variations referring to the same department. For example, `I.C.U.` and `Intensive Care` were standardized to `ICU`, while `PEDS` and `Pediatrics` were standardized to `Paediatrics`.

**Treatment Status:** Standardized equivalent statuses, including `Complete` and `DONE` to `Completed`, `stopped` to `Discontinued`, and `In progress` and `Pending` to `Ongoing`.

**Patient Outcome:** Consolidated equivalent outcome descriptions. For example, `DECEASED`, `Dead`, `Died`, and `Expired` were standardized to `Deceased`; `Cured` to `Recovered`; and `Getting Better` to `Improved`.

**Missing Values:** Used `COALESCE()` to replace selected missing values with predefined defaults. Missing ages were replaced with `0`, while missing diagnoses, treatment types, and other selected categorical values were represented as `N/A`.

**Whitespace Removal:** Applied `TRIM()` to selected text columns to remove leading and trailing spaces before standardization.

### SQL Techniques Used

- `CASE` expressions for conditional value mapping.
- `TRIM()` for whitespace removal.
- `COALESCE()` for handling missing values.
- `ROW_NUMBER()` for ranking records.
- Subqueries to separate row ranking from filtering.

### Important Considerations

The deduplication process retains one record per patient–encounter combination based on the latest admission date. This assumes that the latest record is the appropriate record to retain. Tied admission dates may require an additional tie-breaker.

Replacing missing ages with zero may affect age-based analysis because zero is a valid numerical value. This should be considered when interpreting the results.

