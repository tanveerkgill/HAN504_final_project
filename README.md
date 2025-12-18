# HAN504_final_project
# Instructions for Assignment

This project uses **Azure services** to construct a small, practical healthcare cloud system, with clear analogies to **GCP and OCI**, which were also discussed throughout the course. The objective of this assignment is to demonstrate a complete end-to-end cloud pipeline that integrates **compute, storage, a managed database, and basic analytics**, while following the architectural patterns practiced during the semester.

The scenario implemented is a **Remote Blood Pressure Monitoring mini-pipeline** for a primary care clinic. Patients submit daily blood pressure readings, which are stored, processed, flagged for risk, and summarized for clinical staff. The design intentionally uses **synthetic patient identifiers** and avoids real PHI.

---

## 1. Understand the Use Case

Begin by reviewing the `use_case.md` file.

This document defines the **remote blood pressure monitoring scenario**, identifies the primary users (patients, clinic staff, and clinicians), and explains the clinical problem being addressed—collecting home blood pressure readings and identifying high-risk patterns early.

The use case describes:
- How patients submit daily blood pressure readings via a simple API
- What data is collected (systolic, diastolic, timestamp, synthetic patient ID)
- How readings are evaluated for risk (normal, high, critical)
- How clinic staff access a daily summary view for follow-up and care coordination

Understanding this file provides context for how the system supports remote patient monitoring workflows commonly used in primary care and chronic disease management.

---

## 2. Review the Architecture Diagram

Open the architecture diagram file (`architecture_diagram.png`) or review the embedded Mermaid diagram in `architecture_plan.md`.

The diagram illustrates the high-level system architecture and includes:

- **Frontend / Access Layer**  
  A Flask-based API deployed as a containerized or serverless application where patients submit readings and staff request summaries.

- **Compute Layer**  
  Serverless functions used for automated data validation, transformation, and alert flag generation when new readings arrive.

- **Data Layer**  
  Object storage used to retain raw blood pressure submissions, and a managed relational database used to store cleaned, query-ready records.

- **Optional Analytics Component**  
  An analytics or notebook environment that could be used to explore trends, generate reports, or extend the solution with more advanced analysis.

The diagram demonstrates how data flows through the system from ingestion to storage, processing, and reporting.

---

## 3. Read the Architecture & Implementation Plan

Next, review the `architecture_plan.md` file.

This document explains:

- Which **cloud services** are used at each layer of the architecture and why they were selected
- How the design maps primarily to **Azure services**, with direct equivalents noted for **GCP and OCI**
- How **credentials, identity, RBAC, and governance** are handled at a high level
- How the system avoids using real PHI and follows secure design principles appropriate for a student environment
- Which components are most likely to incur cost and how **serverless and scale-to-zero services** are used to minimize spending

The implementation plan also provides a **step-by-step data flow narrative**, tracing the path from patient submission, to raw storage, to automated processing, to structured database storage, and finally to summary reporting. This ties the project directly back to the concepts practiced in Azure, GCP, and OCI labs throughout the semester.

---

## Optional: Prototype Track

If included, the `prototype/` directory contains a minimal Flask application that demonstrates a subset of the design. The prototype focuses on intake, risk flagging, and daily summaries using local storage to simulate cloud object storage. Instructions for running the prototype locally are provided in `prototype/README.md`.

A short video walkthrough may also be included to assist with review.
