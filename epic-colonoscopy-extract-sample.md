# Epic Colonoscopy Extract — Sample Data

This file documents the structure and sample data for the Epic Clarity extract that drives the colonoscopy prep workflow. It corresponds to `epic-colonoscopy-extract-sample.xlsx`.

---

## Sample Patient Records

| First Name | Last Name | DOB | ZID (.1) | Contact Number | Email Address | Gender | Date of Service | Time of Procedure | Visit Type ID | Visit / Procedure Type | Provider ID | Provider Name | Location ID | Location Name | Prep Instructions |
|------------|-----------|-----|----------|----------------|---------------|--------|-----------------|-------------------|---------------|------------------------|-------------|---------------|-------------|---------------|-------------------|
| James | Harrington | 03/14/1958 | Z43519 | 617-555-0192 | j.harrington@gmail.com | Male | 06/10/2026 | 7:00 AM | 2050 | Screening Colonoscopy | 10042 | Dr. Sarah Whitfield, MD | 1002001 | MGH GI Procedure Unit – Main Campus | GoLYTELY split-dose: 2L evening before + 2L morning of procedure. Clear liquids only day before. No solid food after midnight. |
| Maria | Delgado | 07/22/1964 | Z61047 | 781-555-0347 | mdelgado@yahoo.com | Female | 06/11/2026 | 8:30 AM | 2051 | Diagnostic Colonoscopy | 10078 | Dr. James Okafor, MD | 1002001 | MGH GI Procedure Unit – Main Campus | MiraLAX + Gatorade: 238g MiraLAX split in 64oz Gatorade. Begin evening before, finish 5hr before procedure. |
| Robert | Kim | 11/05/1955 | Z78293 | 978-555-0510 | robertkim55@outlook.com | Male | 06/12/2026 | 9:15 AM | 2052 | Surveillance Colonoscopy | 10042 | Dr. Sarah Whitfield, MD | 1002002 | Newton-Wellesley GI & Endoscopy Center | SUPREP Bowel Prep Kit: First dose 6pm day before, second dose 4hr before procedure. Low-fiber diet 3 days prior. |
| Patricia | O'Brien | 09/30/1967 | Z54821 | 508-555-0784 | pobrien@comcast.net | Female | 06/13/2026 | 10:00 AM | 2050 | Screening Colonoscopy | 10113 | Dr. Anika Patel, MD | 1002002 | Newton-Wellesley GI & Endoscopy Center | GoLYTELY split-dose: 2L evening before + 2L morning of procedure. Clear liquids only day before. No solid food after midnight. |
| David | Patel | 01/18/1960 | Z39174 | 617-555-0921 | dpatel60@gmail.com | Male | 06/16/2026 | 7:30 AM | 2051 | Diagnostic Colonoscopy | 10078 | Dr. James Okafor, MD | 1002001 | MGH GI Procedure Unit – Main Campus | MoviPrep: 2L dose night before + 1L dose morning of procedure. Clear liquids 24hr before. Stop iron supplements 7 days prior. |
| Susan | Nguyen | 05/03/1969 | Z82650 | 339-555-0265 | snguyen@hotmail.com | Female | 06/17/2026 | 11:00 AM | 2053 | Polypectomy / Colonoscopy | 10201 | Dr. Kevin Marsh, MD | 1002003 | Brigham GI Ambulatory Surgery Center | SUPREP Bowel Prep Kit: First dose 6pm day before, second dose 4hr before procedure. Low-fiber diet 3 days prior. |
| Michael | Thornton | 08/27/1953 | Z11438 | 781-555-0638 | mthornton53@gmail.com | Male | 06/18/2026 | 8:00 AM | 2052 | Surveillance Colonoscopy | 10113 | Dr. Anika Patel, MD | 1002003 | Brigham GI Ambulatory Surgery Center | MiraLAX + Gatorade: 238g MiraLAX split in 64oz Gatorade. Begin evening before, finish 5hr before procedure. |
| Linda | Washington | 12/11/1963 | Z96702 | 617-555-0473 | lwashington@aol.com | Female | 06/19/2026 | 9:45 AM | 2050 | Screening Colonoscopy | 10042 | Dr. Sarah Whitfield, MD | 1002001 | MGH GI Procedure Unit – Main Campus | GoLYTELY split-dose: 2L evening before + 2L morning of procedure. Clear liquids only day before. No solid food after midnight. |
| Charles | Reyes | 04/06/1957 | Z25983 | 978-555-0819 | charlesreyes@gmail.com | Male | 06/20/2026 | 1:00 PM | 2051 | Diagnostic Colonoscopy | 10201 | Dr. Kevin Marsh, MD | 1002002 | Newton-Wellesley GI & Endoscopy Center | MoviPrep: 2L dose night before + 1L dose morning of procedure. Clear liquids 24hr before. Stop iron supplements 7 days prior. |
| Barbara | Fitzgerald | 10/19/1970 | Z47316 | 508-555-0142 | bfitz70@yahoo.com | Female | 06/23/2026 | 2:15 PM | 2053 | Polypectomy / Colonoscopy | 10078 | Dr. James Okafor, MD | 1002003 | Brigham GI Ambulatory Surgery Center | SUPREP Bowel Prep Kit: First dose 6pm day before, second dose 4hr before procedure. Low-fiber diet 3 days prior. |
| Thomas | Larson | 02/28/1961 | Z63891 | 617-555-0356 | tlarson@gmail.com | Male | 06/24/2026 | 7:15 AM | 2052 | Surveillance Colonoscopy | 10113 | Dr. Anika Patel, MD | 1002001 | MGH GI Procedure Unit – Main Campus | MiraLAX + Gatorade: 238g MiraLAX split in 64oz Gatorade. Begin evening before, finish 5hr before procedure. |
| Dorothy | Chan | 06/14/1966 | Z18574 | 781-555-0594 | dorothychan@icloud.com | Female | 06/25/2026 | 10:30 AM | 2050 | Screening Colonoscopy | 10201 | Dr. Kevin Marsh, MD | 1002002 | Newton-Wellesley GI & Endoscopy Center | GoLYTELY split-dose: 2L evening before + 2L morning of procedure. Clear liquids only day before. No solid food after midnight. |

---

## Field Definitions

| Field Name | Epic Source Table / Field | Data Type | Format / Notes |
|------------|---------------------------|-----------|----------------|
| First Name | `PATIENT.PAT_FIRST_NAME` | VARCHAR | Patient legal first name |
| Last Name | `PATIENT.PAT_LAST_NAME` | VARCHAR | Patient legal last name |
| DOB | `PATIENT.BIRTH_DATE` | DATE | MM/DD/YYYY |
| ZID (.1) | `PATIENT.PAT_ID` | VARCHAR | Z-prefixed internal Epic patient identifier; distinct from MRN. Referenced as `.1` in Clarity extracts. |
| Contact Number | `PAT_ENC.PREF_CONTACT_PHONE` | VARCHAR | NPA-NXX-XXXX; cell preferred, then home |
| Email Address | `PATIENT.EMAIL_ADDRESS` | VARCHAR | Lowercase; blank if on file as 'Declined' |
| Gender | `PATIENT.SEX_C` (lookup `ZC_SEX`) | VARCHAR | Male / Female / Unknown |
| Date of Service | `PAT_ENC.CONTACT_DATE` | DATE | MM/DD/YYYY; upcoming scheduled date |
| Time of Procedure | `PAT_ENC.APPT_TIME` | TIME | HH:MM AM/PM; scheduled appointment start time |
| Visit Type ID | `PAT_ENC.VISIT_TYPE_ID` | INTEGER | Numeric Epic internal ID; joins to `ZC_VISIT_TYPE.VISIT_TYPE_ID` |
| Visit / Procedure Type | `ZC_VISIT_TYPE.NAME` (via `PAT_ENC.VISIT_TYPE_ID`) | VARCHAR | Screening, Diagnostic, Surveillance, Polypectomy |
| Provider ID | `PAT_ENC.VISIT_PROV_ID` | VARCHAR | Epic internal provider ID; joins to `CLARITY_SER.PROV_ID` |
| Provider Name | `CLARITY_SER.PROV_NAME` (via `VISIT_PROV_ID`) | VARCHAR | Format: Dr. [First] [Last], MD/DO |
| Location ID | `PAT_ENC.DEPARTMENT_ID` | VARCHAR | Epic internal department/location ID; joins to `CLARITY_DEP.DEPARTMENT_ID` |
| Location Name | `CLARITY_DEP.DEPARTMENT_NAME` (via `DEPT_ID`) | VARCHAR | Full facility/department name as configured in Epic |
| Prep Instructions | `ORD_SEN_PROC.COMMENTS` / `PROC_NOTE_TEXT` | VARCHAR | Pulled from associated procedure order instructions |

---

## Report Filters (Epic Clarity)

| Filter / Criteria | Details |
|-------------------|---------|
| Report Type | Epic Clarity SQL / SlicerDicer report |
| Source Tables | `PAT_ENC`, `PATIENT`, `ZC_VISIT_TYPE`, `CLARITY_SER`, `CLARITY_DEP`, `ORD_SEN_PROC` |
| Key Joins | `PAT_ENC.VISIT_TYPE_ID → ZC_VISIT_TYPE` \| `PAT_ENC.VISIT_PROV_ID → CLARITY_SER.PROV_ID` \| `PAT_ENC.DEPARTMENT_ID → CLARITY_DEP.DEPARTMENT_ID` |
| Department Filter | `CLARITY_DEP.DEPARTMENT_NAME LIKE '%GI%' OR '%COLON%' OR '%ENDOSCOPY%'` |
| Visit Type Filter | `ZC_VISIT_TYPE.NAME IN ('Screening Colonoscopy', 'Diagnostic Colonoscopy', 'Surveillance Colonoscopy', 'Polypectomy')` |
| Date Range | `PAT_ENC.CONTACT_DATE BETWEEN SYSDATE AND SYSDATE + 60` (rolling 60-day look-ahead) |
| Status Filter | `PAT_ENC.APPT_STATUS_C = 1` (Scheduled) — exclude Canceled, No-Show, Completed |
| Patient Age | Age >= 45 (ANSI/ACG screening guideline threshold); calculated from `PATIENT.BIRTH_DATE` |
| Deceased Exclusion | `PATIENT.DEATH_DATE IS NULL` |
| Consent Filter | `PATIENT.CONSENT_STATUS` excludes 'No Outreach' / 'Opted Out' |
| Duplicate Logic | Deduplicate on `PATIENT.PAT_ID` (ZID); keep earliest upcoming DOS per patient |
| Output Sort | `ORDER BY PAT_ENC.CONTACT_DATE ASC, PAT_ENC.APPT_TIME ASC` |
