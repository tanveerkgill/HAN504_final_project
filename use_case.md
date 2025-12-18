# Remote Blood Pressure Monitoring Mini-Pipeline for a Primary Care Clinic ###

## Problem statement
Primary care clinics often ask patients with hypertension to measure blood pressure at home. A common challenge is that readings come in inconsistently (texts, phone calls, paper logs), and clinic staff can’t quickly identify which patients need follow-up. This project designs a simple cloud pipeline that collects daily readings, stores them safely, flags risky patterns, and provides a daily summary for clinic staff.

Users:
- Patients: submit daily blood pressure readings
- Clinic staff (MA/RN): review alerts and daily summary
- Clinicians: review trends before telehealth or in-person follow-ups

## Data sources
1) Patient-submitted blood pressure readings (JSON)
- Example fields: patient_id (synthetic), timestamp, systolic, diastolic, notes (optional)
- Source: a simple web form/API endpoint (Flask)

2) Optional patient roster file (CSV)
- Example fields: patient_id, risk_group (standard/high-risk)
- Source: exported registry file (demo data)

## Basic workflow 
1) Patient submits a reading to a Flask endpoint (POST /submit-reading).
2) Flask writes the raw JSON to cloud object storage (raw zone).
3) A serverless function triggers when a new file arrives and validates the reading.
4) The function writes a cleaned record to a managed SQL database table.
5) The function assigns a risk flag (normal/high/critical) based on BP values.
6) Clinic staff requests a daily summary from Flask (GET /daily-summary) to view totals and alerts.

## Success criteria
- Readings are stored reliably in a raw zone and a structured database
- Risk flags are generated consistently using clear rules
- Staff can view a daily summary without needing real PHI
- The design scales without requiring an always-on VM

