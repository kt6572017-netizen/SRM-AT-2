# AI AGENT SPECIFICATION & REPOSITORY MANIFEST
> **Audience:** Autonomous AI Coding Assistants (Antigravity, Claude, ChatGPT, Cursor, Windsurf, Devin, GitHub Copilot)  
> **Environment:** Local Offline PC (Windows) / Container / Cloud  
> **Project:** MBA 2nd Year 3rd Sem Attendance Portal

---

## 1. REPOSITORY SUMMARY & ARCHITECTURE CONTRACT
- **Project Type:** Python Data Processing & Interactive Streamlit Web Dashboard.
- **Entry File:** `app.py`
- **Data Source:** `attendance.csv` (Extracted from `ATTENDANCE 290926.pdf`).
- **Primary Frameworks:** `streamlit` (UI / State), `pandas` (ETL / Aggregations), `plotly.graph_objects` (Visualizations).
- **Default Port:** `8501` (WebSocket + HTTP).

```
[ATTENDANCE 290926.pdf] ──(Extraction)──> [attendance.csv] ──> [app.py (Streamlit Engine)] ──> [Browser UI :8501]
```

---

## 2. DATASET SPECIFICATION (`attendance.csv`)
- **Total Records:** 174 rows (29 students × 6 courses per student).
- **Unique Students:** 29 (MBA Cohort 2024–2026, 3rd Semester).
- **Unique Courses:** 12 active course codes.

### Column Definitions:
| Column | Type | Description | Sample Value |
| :--- | :--- | :--- | :--- |
| `S.No` | `int` | Unique record index | `1` |
| `Register_No` | `string` | SRM Registration Number | `RA2452001010001` |
| `Student_Name` | `string` | Full student name | `Kavin R` |
| `Course_Code` | `string` | Unique subject identifier | `MBG24301T` |
| `Course_Title` | `string` | Official subject title | `Strategic Management` |
| `Attendance_Percentage` | `float` | Cumulative percentage (0.0 to 100.0) | `87.5` |
| `Status` | `string` | Category (`Eligible` if >= 75.0 else `Shortage`) | `Eligible` |

---

## 3. ACADEMIC DOMAIN RULES & COURSE MAP
Total conducted class counts vary by course structure:
```python
COURSE_CONDUCTED = {
    'MBG24301T': 16,  # Strategic Management (Theory)
    'MBG24302T': 16,  # International Business Management (Theory)
    'MBG24303L': 24,  # Business Analytics Lab (Extended Practical Lab)
    'MBG24B05L': 17,  # Elective Course 1
    'MBG24B06L': 17,  # Elective Course 2
    'MBG24B11L': 17,  # Elective Course 3
    'MBG24F05T': 17,  # Finance Elective
    'MBG24F09T': 17,  # Investment & Portfolio
    'MBG24H06T': 17,  # Human Resource Elective 1
    'MBG24H09T': 17,  # Human Resource Elective 2
    'MBG24M07T': 17,  # Marketing Elective 1
    'MBG24M11T': 17   # Marketing Elective 2
}
```

---

## 4. MATHEMATICAL FORMULAS (ATTENDANCE RECOVERY)
Given:
- $T$: Total classes conducted so far
- $A$: Classes attended so far
- $P$: Target threshold ($0.75$ for 75%)

### Defaulter Recovery (When $A / T < P$):
Minimum upcoming consecutive classes $x$ the student must attend to reach $P$:
$$x = \left\lceil \frac{P \cdot T - A}{1 - P} \right\rceil = \lceil 3T - 4A \rceil \quad (\text{for } P = 0.75)$$

### Safe Miss Buffer (When $A / T \ge P$):
Maximum upcoming classes $m$ the student can miss without dropping below $P$:
$$m = \left\lfloor \frac{A}{P} - T \right\rfloor = \left\lfloor \frac{4A - 3T}{3} \right\rfloor \quad (\text{for } P = 0.75)$$

---

## 5. LOCAL OFFLINE EXECUTION CONTRACT
To run completely offline on a local PC:

### Command Line:
```bash
streamlit run app.py --server.headless=true --server.port=8501
```

### 1-Click Launchers (Windows):
- `run_dashboard.bat`: Starts the offline Streamlit server and opens `http://localhost:8501`.
- `view_readme.bat`: Opens `README.html` locally in the default browser without requiring internet.

---

## 6. CLOUD & CI/CD DEPLOYMENT CONSTRAINTS
- **Vercel Incompatibility:** Vercel is a stateless Serverless Function platform. Streamlit requires stateful long-running WebSocket connections.
- **Top-Level Handler Fix:** In `app.py`, top-level `handler = app = application` is exported to prevent Vercel entrypoint inspection build errors:
  ```python
  def handler(request=None, response=None): return "MBA Attendance Portal"
  app = handler
  application = handler
  ```
- **Recommended Free Cloud Host:** **Streamlit Community Cloud** (`share.streamlit.io`) connected to GitHub repository `kt6572017-netizen/srm_att_2yr`.
