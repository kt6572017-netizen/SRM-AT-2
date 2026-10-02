# 🎓 MBA 2nd Year 3rd Sem — Attendance Analytics & Recovery Dashboard

An interactive, high-performance web dashboard built with **Streamlit**, **Pandas**, and **Plotly** for monitoring, analyzing, and calculating student attendance records for the **MBA 2nd Year 3rd Semester (Batch 2024–2026)** at SRM.

---

## 📌 Table of Contents
1. [Overview](#overview)
2. [Vercel Deployment Error Fix & Cloud Hosting](#vercel-deployment-error-fix--cloud-hosting)
3. [Key Features](#key-features)
4. [Project File Structure](#project-file-structure)
5. [How to Run Offline on Your PC](#how-to-run-offline-on-your-pc)
6. [AI Agent Technical Specification](#ai-agent-technical-specification)
7. [Attendance Calculation Formulas](#attendance-calculation-formulas)
8. [Academic Regulations](#academic-regulations)

---

## 🌟 Overview
This project processes official academic attendance data (`ATTENDANCE 290926.pdf`), transforms it into structured tabular records (`attendance.csv`), and serves an executive-grade dashboard tracking student eligibility against the SRM 75% attendance benchmark.

---

## ☁️ Vercel Deployment Error Fix & Cloud Hosting

### The Error:
```text
Error: Found app.py but it does not export a top-level "app", "application", or "handler" variable.
Learn More: https://vercel.com/docs/functions/runtimes/python#python-entrypoints
```

### Why This Happened:
Vercel is a **Serverless Function platform** designed for WSGI/ASGI APIs (Next.js, FastAPI, Flask). When Vercel sees `app.py`, it expects an HTTP request handler like `app = Flask()` or `app = FastAPI()`. 

However, **Streamlit is an interactive, stateful WebSocket dashboard server** (`streamlit run app.py`), not a stateless serverless function.

### How It Was Fixed in Code:
In `app.py`, top-level compatibility exports were added:
```python
def handler(request=None, response=None):
    return "MBA 2nd Year 3rd Sem Attendance Portal"
app = handler
application = handler
```
This satisfies the Vercel Python entrypoint inspector.

### 🚀 Best Free Cloud Deployment (Recommended: Streamlit Community Cloud)
Because Streamlit requires persistent WebSocket connections, the official free hosting platform is **Streamlit Community Cloud**:

1. Go to **[share.streamlit.io](https://share.streamlit.io)** and log in with your GitHub account.
2. Click **"New app"**.
3. Choose your repository:
   - **Repository:** `kt6572017-netizen/srm_att_2yr`
   - **Branch:** `main`
   - **Main file path:** `app.py`
4. Click **"Deploy!"**.
5. Your dashboard will be live on the web in 30 seconds with a free URL (e.g. `https://srm-att-2yr.streamlit.app`)!

---

## ✨ Key Features

### 1. Executive Dashboard (HR / People Layout)
- **Statistics Donut Chart**: Visual ratio of students who are Eligible ($\ge 75\%$), Defaulters ($< 75\%$), and 100% Achievers.
- **6 Micro-KPI Cards**:
  - 🔵 **Checked In**: Total subject registrations meeting the $\ge 75\%$ threshold.
  - 🔴 **Not Checked In**: Defaulter subject count requiring regularization.
  - 🟢 **100% Achievers**: Perfect attendance records.
  - 🟣 **Total Courses**: Active courses (12 subjects across core and electives).
  - 🟡 **Defaulter Cases**: Count of unique students with at least one shortage course.
  - 🟠 **Total Students**: Total cohort size (29 students).
- **On-Time Check In (Diverging Bar Chart)**: Compares average class attendance against the 75% benchmark with peak subject callouts.
- **Overtime / Conducted Hours**: Bar breakdown of conducted academic sessions and lab practicals.
- **Attendance Sources (2×2 Grid)**: Device Check-Ins, App Check-Ins, Active Devices, and Inactive Devices.
- **Exceptions & Pending Requests**: Automated grouping of critical shortages ($< 50\%$) and borderline warnings ($50\% - 74\%$).

### 2. Interactive Consecutive Class Recovery Calculator
- Select any student and course to compute:
  - Exact number of **upcoming consecutive classes** required to reach $\ge 75\%$.
  - Safe miss buffer (how many classes can be skipped while maintaining eligibility).
  - Visual trajectory projection curve.

### 3. Master Reports & Defaulter Registry
- Filter by student and course, with 1-click **Download Defaulter Report (CSV)**.

---

## 📂 Project File Structure

```text
srm attandence/
│
├── app.py                                # Main Streamlit dashboard application (with Vercel compatibility)
├── attendance.csv                        # Cleaned structured attendance dataset (174 records)
├── ATTENDANCE 290926.pdf                 # Original institutional attendance document
├── requirements.txt                      # Python dependencies list
├── README.md                             # Comprehensive technical documentation
├── README.html                           # Standalone offline-viewable documentation
├── view_readme.bat                       # 1-Click launcher to view documentation offline
├── run_dashboard.bat                     # 1-Click launcher to run dashboard offline
└── MBA_2nd_Year_Attendance_Dashboard.zip # Ready-to-share compressed archive
```

---

## 💻 How to Run Offline on Your PC

### 1-Click Launch (Easiest):
Simply **double-click** the file:
```text
run_dashboard.bat
```

### Manual Command:
```bash
streamlit run app.py
```
Open your browser at `http://localhost:8501`.

---

## 🤖 AI Agent Technical Specification

This section is optimized for external AI assistants (ChatGPT, Claude, Cursor, Devin, Antigravity) reading this repository:

1. **Architecture:** Single-process Streamlit web server.
2. **Dataset:** `attendance.csv` containing 174 records across 29 students and 12 courses.
3. **Conducted Class Mapping:**
```python
COURSE_CONDUCTED = {
    'MBG24301T': 16, 'MBG24302T': 16, 'MBG24303L': 24,
    'MBG24B05L': 17, 'MBG24B06L': 17, 'MBG24B11L': 17,
    'MBG24F05T': 17, 'MBG24F09T': 17, 'MBG24H06T': 17,
    'MBG24H09T': 17, 'MBG24M07T': 17, 'MBG24M11T': 17
}
```
4. **Calculations:**
   - Classes needed: $x = \lceil \frac{0.75 T - A}{0.25} \rceil = \lceil 3T - 4A \rceil$
   - Safe bunk buffer: $m = \lfloor \frac{4A - 3T}{3} \rfloor$

---

## ⚖️ Academic Regulations
- **University Criteria**: A minimum of **75% attendance** in each individual course is mandatory to appear for the End-Semester Examinations.
- **Condonation Range (65% – 74%)**: Permitted only under approved medical or official duty leaves.
- **Critical Shortage (< 65%)**: Mandatory course repetition / detention in the subject.
