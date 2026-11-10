# Forensic Services Unit: Dual-Phase Operational SLA & Quality Assurance Audit Suite

An end-to-end data engineering, quality assurance, and compliance auditing suite designed for modern law enforcement forensic laboratories. This project models, tracks, and visualizes forensic casework performance across two integrated phases: operational SLA monitoring and accreditation compliance.

---

## Project Directory & Deliverables

- **Live Tableau Dashboards:** 
  - Phase 1: [Simulated Forensic Services Unit Operational & Quality Audit](https://public.tableau.com/views/SimulatedForensicServicesUnitOperationalQualityAudit/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
  - Phase 2: [Forensic Services Unit | Technical Casework & QA Compliance Audit](https://public.tableau.com/views/ForensicServicesUnitTechnicalCaseworkQA/Dashboard1D?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
- **Executive Excel Workbook:** [Simulated FSU Compliance Analysis Workbook](https://github.com/emmadumonseau/Forensic-Services-Unit-Dual-Phase-Operational-SLA-Quality-Assurance-Audit-Suite/blob/main/simulated_fsu_compliance_audit_workbook.xlsx) (4-tab compliance model with dynamic spill logic) 
- **Python Analytics Pipeline:** [`fsu_performance_model_script`](https://github.com/emmadumonseau/Forensic-Services-Unit-Dual-Phase-Operational-SLA-Quality-Assurance-Audit-Suite/blob/main/fsu_performance_model_script.ipynb) & [`generate_fsu_analysis_data`](https://github.com/emmadumonseau/Forensic-Services-Unit-Dual-Phase-Operational-SLA-Quality-Assurance-Audit-Suite/blob/main/generate_fsu_analysis_data.ipynb) & [`generate_fsu_technical_casework`](https://github.com/emmadumonseau/Forensic-Services-Unit-Dual-Phase-Operational-SLA-Quality-Assurance-Audit-Suite/blob/main/generate_fsu_technical_casework.ipynb)(Automated raw data generation and workload modeling)

---

## Executive Overview & Problem Statement

Modern law enforcement forensic laboratories operate under strict accreditation mandates, requiring rigorous oversight of evidence handling, instrument calibration, chain of custody integrity, and federal database turnaround SLAs (such as CODIS and NIBIN). This project delivers an end-to-end data analytics and auditing suite for a simulated municipal Forensic Services Unit (FSU) to identify operational bottlenecks and ensure full technical compliance.

---

## Phase 1: Operational SLA & Workflow Audit

- **Objective & Scope:** Evaluates laboratory throughput across seven forensic disciplines (DNA, Drug Chemistry, Crime Scene, Latent Prints, Firearms, Toxicology, and Trace Evidence), tracking turnaround times against mandated target SLAs, chain-of-custody verifications, and instrument calibration holds across 1,000 synthetic records.
- **Key Metrics & Findings:**
  - **Total Casework Evaluated:** 1,000 records
  - **Overall SLA Compliance Breach Rate:** 67.3% (driven by strict trial subpoena thresholds and operational constraints)
  - **Chain of Custody Pass Rate:** 98.3%
  - **QC Calibration Pass Rate:** 96.8%
- **Discipline Performance Breakdown:**
  - **Crime Scene:** 130 items | Avg Target SLA: 7.0 days | Avg Actual TAT: 8.7 days | Breach Rate: 66.9% | QC Holds: 4
  - **DNA:** 254 items | Avg Target SLA: 25.0 days | Avg Actual TAT: 26.2 days | Breach Rate: 65.0% | QC Holds: 8
  - **Drug Chemistry:** 266 items | Avg Target SLA: 14.0 days | Avg Actual TAT: 15.9 days | Breach Rate: 70.3% | QC Holds: 8
  - **Firearms:** 105 items | Avg Target SLA: 21.0 days | Avg Actual TAT: 22.7 days | Breach Rate: 69.5% | QC Holds: 2
  - **Latent Prints:** 141 items | Avg Target SLA: 18.0 days | Avg Actual TAT: 19.4 days | Breach Rate: 66.0% | QC Holds: 7
  - **Toxicology:** 57 items | Avg Target SLA: 20.0 days | Avg Actual TAT: 20.9 days | Breach Rate: 61.4% | QC Holds: 0
  - **Trace Evidence:** 47 items | Avg Target SLA: 30.0 days | Avg Actual TAT: 31.6 days | Breach Rate: 70.2% | QC Holds: 3

---

## Phase 2: ISO/IEC 17020 Technical Compliance & Non-Conformance Audit

- **Objective & Scope:** Transitions to accreditation compliance, examining evidence packaging integrity, instrument solvent blank checks (GC-MS, FTIR, LC-MS/MS), measurement uncertainty percentages, and federal database upload SLAs (CODIS 14-day threshold and NIBIN 3-day threshold).
- **Technical Dataset Highlights (1,000 Records):**
  - **Packaging Discrepancies:** 25 items flagged with compromised or unsealed intake packaging.
  - **Chain of Custody Breaks:** 16 items identified with interrupted custodial transfer logs.
  - **Instrument Blank Failures:** 24 items exhibiting solvent blank contamination or calibration flags on confirmatory instruments.
  - **Federal Database Tracking:** CODIS and NIBIN eligible items evaluated against federal upload SLAs.

---

## Excel Companion Workbook Architecture (Phase 2)

The Phase 2 companion workbook (`FSU_ISO17020_Technical_Casework_Companion.xlsx`) utilizes advanced Excel formula architectures across four dedicated tabs:
- **`Executive Summary` Tab:** Houses compliance scorecards and a disciplined summary table driven by dynamic `COUNTIF`, `COUNTIFS`, and `AVERAGEIFS` formulas referencing the master log.
- **`Master Casework Log` Tab:** Contains the raw 1,000-record technical dataset with embedded conditional SLA evaluation formulas.
- **`Instrument QC & Bench Tracking` Tab:** Evaluates solvent blank validation and run volumes across confirmatory instruments (`GC-MS`, `FTIR`, `LC-MS/MS`) with automated pass-rate percentage calculations (`=D3/B3`).
- **`Corrective Action Audit Log` Tab:** 
  - **Dynamic Spilling & Categorization Formula (`B5`):** Uses a combined `CHOOSE` and `FILTER` array to automatically extract non-conforming records and assign exact deficiency categories without breaking spill bounds:
    ```excel
    =FILTER(
        CHOOSE({1, 2, 3, 4}, 
            'Master Casework Log'!A2:A1001, 
            'Master Casework Log'!B2:B1001, 
            'Master Casework Log'!C2:C1001, 
            IF('Master Casework Log'!D2:D1001="Packaging Discrepancy", "Packaging Discrepancy", 
               IF('Master Casework Log'!F2:F1001=0, "Custody Break", 
                  IF('Master Casework Log'!L2:L1001=0, "QC Blank Failure", "Standard")))
        ), 
        ('Master Casework Log'!D2:D1001="Packaging Discrepancy") + 
        ('Master Casework Log'!F2:F1001=0) + 
        ('Master Casework Log'!L2:L1001=0), 
        "No Deficiencies Found"
    )
    ```
  - **Sequential Tracking ID Generation (`A5`):** `=IF(B5<>"", "CAR-2026-" & TEXT(ROW() - 4, "000"), "")`
  - **Automated Corrective Action Assignment (`G5`):** `=IF(E5="Packaging Discrepancy", "Re-seal evidence and log intake discrepancy note", IF(E5="Custody Break", "Initiate chain of custody review and supervisor audit sign-off", "Recalibrate instrument bench and re-run solvent blank"))`

---

## Strategic Recommendations for Laboratory Leadership

- **Targeted Workflow Intervention:** Drug Chemistry and Trace Evidence exhibit the highest SLA breach rates, suggesting staffing constraints or bottlenecks that require resource reallocation. 
- **Automated QC Holds:** Integrate the Excel FILTER logic directly into the Laboratory Information Management System to automatically quarantine items with failed instrument solvent blanks prior to report generation. 
-  **Chain of Custody Audits:** Institute mandatory digital handoff confirmation at evidence intake to eliminate the 16 identified custodial exceptions and ensure full traceability. 
