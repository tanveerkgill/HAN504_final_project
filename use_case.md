# Use Case: Clinic Intake + Triage Summary

## Problem statement
Small clinics often collect intake information using paper forms or disconnected tools, which causes delays and makes it hard to quickly see trends (e.g., common symptoms, high-urgency visits today).
Users:
- Patients (submit intake)
- Front desk staff (verify submissions)
- Clinician/triage nurse (review summaries)

## Data sources
- Intake form submissions (JSON)
  Example fields: timestamp, age, symptoms, urgency_level, insurance_type (optional), free_text_notes (optional)
- Optional: daily CSV upload of scheduled appointments (CSV)

## Basic workflow (data flow)
1) Patient/staff submits intake form to a Flask web endpoint (/submit).
2) The raw JSON payload is written to Cloud Storage (raw zone) for audit/logging.
3) A serverless function or scheduled job parses raw files and writes cleaned rows into a managed database table (intakes).
4) Flask (or a small API) queries the database for summary metrics (counts by urgency, symptom frequency).
5) A summary page/endpoint displays daily stats for clinic staff.

## Success criteria
- Intake submissions are reliably stored (raw + cleaned)
- Staff can view basic daily summaries in seconds
- No real PHI is required for the student demo (use fake/synthetic data)

