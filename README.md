# ICU-Bed-Monitoring-And-Data-Management-System
# 🏥 Smart ICU Patient Monitoring System
### Student Prototype · Flask + HTML/CSS/JS · v2.0

---

## 📁 Project Structure

```
icu_system/
├── app.py                ← Flask backend (all API logic)
├── index.html            ← Frontend dashboard (works standalone OR with Flask)
├── requirements.txt      ← Python dependencies
├── README.md             ← This file
└── data/
    ├── patients.csv      ← Patient records
    ├── vitals.csv        ← Vital sign readings (80 pre-loaded rows)
    ├── alerts.csv        ← Generated alerts
    └── observations.csv  ← Manual clinical notes
```

---

## ⚙️ Setup & Run Instructions

### Option A — Standalone Demo (No backend needed)
Just open `index.html` directly in any browser. The dashboard runs fully in-memory using the embedded CSV data. No installation required.

> ⚠️ In standalone mode, simulated readings and newly admitted patients are reset on page refresh (no file persistence).

---

### Option B — Full Mode with Flask Backend

#### Step 1 — Install Python dependencies
```bash
pip install -r requirements.txt
```

#### Step 2 — Start the Flask backend
```bash
python app.py
```
You should see:
```
* Running on http://127.0.0.1:5000
```

#### Step 3 — Open the frontend
Visit:
```
http://127.0.0.1:5000
```
Or open `index.html` directly — it auto-detects the backend at `http://127.0.0.1:5000`.

> In Flask mode, all vitals, alerts, and observations are saved to CSV files and persist across refreshes.

---

## 🔌 API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/patients` | List all patients |
| GET | `/api/patient/<id>` | Get single patient details |
| GET | `/api/vitals/<id>` | Get latest vitals + history + prediction |
| POST | `/api/vitals` | Add a manual vital reading |
| GET | `/api/simulate/<id>` | Generate + store a simulated reading |
| GET | `/api/alerts/<id>` | Get last 10 alerts for patient |
| GET | `/api/alerts/all` | Get last 20 alerts across all patients |
| GET | `/api/observations/<id>` | Get last 5 observations |
| POST | `/api/observations` | Submit a manual observation |

---

## 🖥 Features

### Vitals Monitoring
- **4 vital sign cards** — Heart Rate, SpO₂, Blood Pressure, Temperature
- **Abnormal values** highlighted in red with flashing animation
- **Auto-refresh** every 3 seconds (simulates a new sensor reading)
- **Manual simulate** button to trigger a reading on demand

### Trend Charts
- **Heart Rate trend** chart with last 20 readings + predicted next value (dashed amber line)
- **SpO₂ saturation** trend chart with last 20 readings
- Both charts built with Chart.js 4.4

### Predictive Analysis
The system takes the last 2 heart rate readings and performs linear extrapolation:
```
predicted = HR₂ + (HR₂ - HR₁)
```
| Trend | Assessment |
|-------|-----------|
| Increasing & predicted > 95 | ⚠ Predicted Risk |
| Decreasing | ✅ Improving |
| Flat | ➡ Stable |

### Alert System
Real-time alerts generated on every reading:

| Condition | Severity |
|-----------|----------|
| HR > 120 BPM | Critical |
| HR > 100 BPM | High |
| SpO₂ < 85% | Critical |
| SpO₂ < 90% | High |
| Temp > 103°F | Critical |
| Temp > 101°F | Medium |
| BP systolic > 160 | High |
| BP systolic < 90 | High |

### Clinical Observations
- Log pain level (0–10), breathing difficulty, consciousness level
- Free-text clinical notes
- Select attending doctor or nurse
- Last 5 observations displayed per patient

### Patient Management *(New in v2.0)*
Access via the **⊕ Manage Patients** button in the top bar.

#### Admit Patient
Fill in the admission form with:
- Full name, Age, Gender
- Diagnosis / Condition
- ICU Bed assignment (ICU-05 through ICU-10)
- Blood group
- Attending doctor

Admitted patients are immediately added to the patient selector dropdown and begin receiving simulated readings.

#### Discharge Patient
- View all patients with their current status (Active / Discharged)
- One-click discharge removes the patient from the monitor
- Discharged patients remain visible in the management panel for record-keeping

---

## 📊 CSV File Formats

### patients.csv
```
patient_id, name, age, gender, condition, icu_bed, admission_date, doctor, blood_group
```

### vitals.csv *(expanded to 80 rows in v2.0)*
```
vital_id, patient_id, heart_rate, spo2, bp_systolic, bp_diastolic, temperature, timestamp
```
Pre-loaded with 20 readings per patient showing realistic clinical patterns:
- **P001 (Rajesh Kumar)** — Escalating HR and BP, deteriorating trend
- **P002 (Priya Mehta)** — Gradually improving SpO₂ and HR, recovery pattern
- **P003 (Arun Singh)** — Stable post-operative vitals with minor fluctuations
- **P004 (Sunita Verma)** — Critical septic shock: very high HR, low SpO₂, high fever

### alerts.csv
```
alert_id, patient_id, type, value, severity, timestamp
```

### observations.csv
```
obs_id, patient_id, pain_level, breathing_difficulty, consciousness_level, entered_by, notes, timestamp
```

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3 · Flask · Flask-CORS |
| Storage | CSV files (no database needed) |
| Frontend | Vanilla HTML · CSS · JavaScript |
| Charts | Chart.js 4.4 |
| Fonts | Google Fonts (Barlow, Share Tech Mono) |

---

## 🎨 Design Notes *(v2.0)*

- Background updated to a **dark teal / deep-sea palette** (`#030e14` base) for a cleaner clinical aesthetic
- All original colour tokens preserved (green for normal, red for critical, amber for warnings, cyan for info)
- Scanline overlay retained for the monitor feel
- Modal system added for patient management with tabbed interface

---

## 🔄 Changelog

### v2.0
- ✅ Added Patient Admit form (name, age, gender, diagnosis, bed, blood group, doctor)
- ✅ Added Patient Discharge panel with status tracking
- ✅ Expanded vitals.csv from 14 rows to 80 rows (20 readings × 4 patients)
- ✅ Dashboard now works fully standalone without Flask (demo mode)
- ✅ Changed background colour to dark teal theme
- ✅ Gaussian-randomised simulation engine embedded in frontend

### v1.0
- Initial release with Flask backend
- 4 pre-loaded patients, basic vitals monitoring, alerts, observations

---

*Developed as a student-level ICU monitoring prototype.*

