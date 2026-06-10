# Test Strategy: AS400 to SAP Customer Data Migration

**Document Version:** 1.3  
**Status:** Approved  
**Author:** Lucinda Fonseca  
**Date:** 2024-03-15  

---

## 1. Introduction

### 1.1 Purpose

This document defines the test strategy for the migration of customer master data from the IBM AS400 legacy CRM system to SAP S/4HANA. It establishes the scope, approach, tools, resources, and exit criteria for all migration testing activities.

### 1.2 Background

The current AS400 system has been in operation since 1998 and contains over 250,000 customer records across 5 regional databases. The business has decided to consolidate all customer data into SAP S/4HANA as part of a wider ERP transformation programme. This migration is a critical path dependency for go-live.

---

## 2. Scope

### 2.1 In Scope

- Customer master data (all 18 defined data domains)
- ETL transformation and mapping rules
- SAP S/4HANA Business Partner (BP) module validation
- Data quality rules and rejection thresholds
- Reconciliation between source and target
- UAT facilitated by QA team

### 2.2 Out of Scope

- Transactional data (orders, invoices)
- SAP module functional testing beyond BP scope
- Performance testing of SAP infrastructure
- End-user training

---

## 3. Test Objectives

1. Verify all in-scope records are migrated completely and accurately.
2. Validate that ETL transformation rules correctly handle all AS400 data patterns.
3. Confirm data integrity — no corruption, truncation, or encoding errors.
4. Ensure business-critical fields (payment terms, credit limits, account group) are correctly mapped.
5. Validate that migrated records are functional in key SAP transactions (XD03, BP, FD03).
6. Confirm reconciliation totals match between source and target.

---

## 4. Test Types

### 4.1 Data Profiling (Pre-Migration)

**Objective:** Understand source data quality before ETL development.

**Activities:**
- Profile AS400 data for nulls, formats, outliers, and anomalies
- Document known data quality issues to inform cleansing rules
- Agree acceptable rejection thresholds with the business

**Tools:** SQL queries on AS400 views, Python data profiling scripts

---

### 4.2 ETL/Mapping Validation

**Objective:** Verify that transformation rules are correctly implemented in SAP BODS.

**Activities:**
- Review field-level mapping specification against BODS jobs
- Test edge cases: null values, max-length strings, special characters, date formats
- Validate lookup tables (country codes, currency codes, account groups)

**Tools:** SAP Data Services Designer, manual spot-checks

---

### 4.3 Reconciliation Testing

**Objective:** Confirm record volumes and key totals match between source and target.

**Activities:**
- Row count reconciliation per data domain
- Key field reconciliation (customer ID, account group, country)
- Financial total reconciliation (credit limit aggregate)
- Identify and investigate gaps exceeding 0.01% threshold

**Tools:** SQL, custom reconciliation scripts, Excel pivot analysis

---

### 4.4 Functional Validation

**Objective:** Confirm migrated records work correctly within SAP.

**Activities:**
- Display customer master in transactions XD03, BP, FD03
- Verify partner function assignments
- Check address formatting and country-specific rules
- Validate output of SAP standard reports using migrated data

**Tools:** SAP S/4HANA GUI, automated ECATT scripts

---

### 4.5 User Acceptance Testing (UAT)

**Objective:** Obtain business sign-off on migrated data quality.

**Activities:**
- Provide business teams with stratified sample of records to review
- Business validates data accuracy against source documents/screenshots
- Record feedback and raise defects where necessary

**Tools:** UAT tracker (Excel), defect log

---

## 5. Entry and Exit Criteria

### 5.1 Entry Criteria

- [ ] AS400 source extract is complete and frozen
- [ ] BODS mapping jobs are deployed to QA environment
- [ ] Test environment is available and accessible
- [ ] Source data profile report is signed off
- [ ] Test cases reviewed and approved

### 5.2 Exit Criteria

- [ ] All P1 and P2 defects resolved and closed
- [ ] Reconciliation within agreed tolerance (≤0.01% unexplained variance)
- [ ] UAT sign-off obtained from all business stakeholders
- [ ] Final migration validation report approved
- [ ] Go/No-Go decision documented

---

## 6. Defect Management

| Severity | Definition | SLA |
|---|---|---|
| P1 – Critical | Data loss or complete mapping failure | Fix within 24 hours |
| P2 – High | Incorrect values in business-critical fields | Fix within 48 hours |
| P3 – Medium | Non-critical field issues, formatting errors | Fix within 5 days |
| P4 – Low | Cosmetic issues, minor discrepancies | Fix before go-live |

All defects to be logged in the project DefectLog.xlsx and reviewed in daily stand-up.

---

## 7. Roles and Responsibilities

| Role | Name | Responsibility |
|---|---|---|
| QA Lead | Lucinda Fonseca | Strategy, test execution, reporting |
| ETL Developer | R. Mendes | BODS job development, defect fixes |
| SAP Functional Consultant | T. Carvalho | Functional validation, UAT support |
| Data Migration Lead | A. Santos | Mapping specs, source data queries |
| Business SME | M. Gomes | UAT execution, sign-off |
| Project Manager | P. Ferreira | Governance, escalation |

---

## 8. Tools

| Tool | Purpose |
|---|---|
| SAP Data Services (BODS) | ETL transformation and loading |
| SAP S/4HANA GUI | Functional validation |
| SQL (AS400 / DB2) | Source data queries |
| Python / pandas | Reconciliation scripts |
| Excel | Test cases, defect log, reporting |
| Jira | Defect tracking (escalated defects) |
| Confluence | Documentation storage |

---

## 9. Risks and Mitigations

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Source data freeze delayed | Medium | High | Agree freeze date contractually; use incremental delta load |
| AS400 encoding issues missed in profiling | Low | High | Run encoding validation script on full extract |
| SAP downtime during testing | Low | Medium | Agree testing window with Basis team |
| Business UAT availability | Medium | High | Lock in UAT dates 4 weeks in advance |
| Defect volume exceeds capacity | Medium | High | Triage daily; escalate P1s immediately |

---

## 10. Approvals

| Name | Role | Signature | Date |
|---|---|---|---|
| Lucinda Fonseca | QA Lead | ✓ Approved | 2024-03-15 |
| A. Santos | Data Migration Lead | ✓ Approved | 2024-03-15 |
| P. Ferreira | Project Manager | ✓ Approved | 2024-03-16 |
