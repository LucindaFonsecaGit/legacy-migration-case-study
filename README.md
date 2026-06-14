# Legacy Migration Case Study: AS400 to SAP Customer Data Migration

A comprehensive QA case study documenting the end-to-end test strategy, test cases, defect log, and final validation report for a fictional enterprise migration project: migrating customer master data from an IBM AS400 legacy system to SAP S/4HANA.

---

## 📋 Project Overview

| Attribute | Details |
|---|---|
| **Project Name** | Customer Master Data Migration – AS400 to SAP |
| **Source System** | IBM AS400 (RPG-based legacy CRM) |
| **Target System** | SAP S/4HANA Business Partner module |
| **Migration Scope** | 250,000+ customer records, 18 data domains |
| **QA Lead** | Lucinda Fonseca |
| **Timeline** | Q2–Q3 (12 weeks) |
| **Migration Tool** | SAP Data Services (BODS) |

---

## 📁 Repository Contents

| File | Description                                                                                                                  |
|---|------------------------------------------------------------------------------------------------------------------------------|
| `docs/TestStrategy.md` | Full QA strategy, scope, approach and entry/exit criteria                                                                    |
| `testing/TestCases.xls` | Detailed test cases with steps, expected results and pass/fail status                                                        |
| `artifacts/DefectLog.xls` | Defect register with severity, priority, ownership, and resolution tracking                                                  |
| `reports/FinalReport.md` | Post-migration validation summary, testing results, and go-live recommendation                                               |
| `validation/FieldMapping.xls` | Maps legacy fields to target fields with transformation rules |
---

## 🔄 Migration Scope

### Data Domains Migrated

- Customer master records (name, address, contact details)
- Account classifications and customer groups
- Payment terms and credit limits
- Sales area assignments
- Bank account details
- Open item history

### Out of Scope

- Transactional/historical order data
- Archived records (inactive > 5 years)
- Third-party vendor records

---

## ⚠️ Key Risks Identified

| Risk | Impact | Mitigation |
|---|---|---|
| AS400 date format incompatibility (YYYYMMDD vs ISO) | High | ETL transformation rules in BODS |
| Duplicate customer IDs across legacy regions | High | Deduplication layer + golden record logic |
| Special character encoding (AS400 EBCDIC to UTF-8) | Medium | Character mapping table + validation |
| Missing mandatory SAP fields (e.g. account group) | Medium | Default value mapping + enrichment rules |
| Data volume causing load performance issues | Low | Batch size tuning + parallel load jobs |

---

## 🧪 Testing Approach

1. **Data Profiling** — Analyse source data quality before migration begins
2. **Mapping Validation** — Verify ETL transformation rules against field mapping specs
3. **Reconciliation Testing** — Row counts, key field checks, financial totals
4. **Functional Testing** — Validate migrated records work correctly in SAP transactions
5. **Regression Testing** — Ensure existing SAP processes are not impacted
6. **UAT** — Business sign-off on sample records per customer segment

---

## ✅ Migration Outcome

| Metric | Result |
|---|---|
| Records in Source | 251,438 |
| Records Migrated | 250,917 |
| Records Rejected (rules) | 521 |
| Defects Raised | 34 |
| Defects Resolved | 34 |
| Final Status | ✅ GO-LIVE APPROVED |
