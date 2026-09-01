# 📊 Attendify - Attendance Management System

Attendify is a modern and interactive **student attendance management dashboard** built with React.js.

The application allows users to view students, search for specific students, mark attendance as **Present or Absent**, and monitor attendance statistics in real time.

---

## 🌟 Live Demo

🔗 **Live Demo:** Add your deployed website link here

---

## 📌 About the Project

Managing student attendance manually can be time-consuming and difficult to track.

**Attendify** provides a simple and user-friendly dashboard where attendance can be managed digitally.

The dashboard displays:

- Total number of students
- Number of present students
- Number of absent students
- Overall attendance percentage
- Student information
- Department and year
- Current attendance status
- Search functionality
- Present/Absent action buttons

All statistics are automatically updated whenever attendance is changed.

---

## ✨ Features

### 👥 Student Management

Displays student information including:

- Student name
- Student ID
- Department
- Academic year
- Attendance status

### ✅ Attendance Tracking

Users can mark each student as:

- Present
- Absent

The selected status is immediately reflected in the dashboard.

### 📊 Live Statistics

The dashboard automatically calculates:

- Total Students
- Present Count
- Absent Count
- Attendance Percentage
- Absent Percentage

- import { useState } from "react";
import "./App.css";

function App() {
  const students = [
    { id: 1, name: "Shalini Udayakumar Shetty", department: "AI & DS", year: "2nd Year" },
    { id: 2, name: "Vatti Venkatesh Yadav", department: "AI & DS", year: "2nd Year" },
    { id: 3, name: "Shavenn Venkatesh Yadav", department: "ECE", year: "2nd Year" },
    { id: 4, name: "Shaasha Venkatesh Yadav", department: "AI & DS", year: "2nd Year" },
    { id: 5, name: "Haripriya Rajesh", department: "CSE", year: "2nd Year" },
    { id: 6, name: "Alanciya Kareem", department: "IT", year: "2nd Year" },
    { id: 7, name: "Ramajayam Udayakumar", department: "ECE", year: "2nd Year" },
    { id: 8, name: "Udayakumar Shanmugam", department: "CSE", year: "2nd Year" },
  ];

  const [attendance, setAttendance] = useState(() => {
    const initialAttendance = {};

    students.forEach((student) => {
      initialAttendance[student.id] = "Absent";
    });

    return initialAttendance;
  });

  const [search, setSearch] = useState("");

  const markAttendance = (id, status) => {
    setAttendance((previousAttendance) => ({
      ...previousAttendance,
      [id]: status,
    }));
  };

  const presentCount = Object.values(attendance).filter(
    (status) => status === "Present"
  ).length;

  const absentCount = Object.values(attendance).filter(
    (status) => status === "Absent"
  ).length;

  const totalStudents = students.length;

  const attendancePercentage =
    totalStudents > 0
      ? Math.round((presentCount / totalStudents) * 100)
      : 0;

  const filteredStudents = students.filter((student) =>
    student.name.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div className="app">

      <div className="glow glow-one"></div>
      <div className="glow glow-two"></div>

      <main className="dashboard">

        {/* Header */}
        <header className="header">
          <div>
            <div className="brand">
              <div className="brand-icon">✓</div>
              <span>Attendify</span>
            </div>

            <h1>Attendance Dashboard</h1>

            <p className="subtitle">
              Manage today's student attendance with ease.
            </p>
          </div>

          <div className="date-box">
            <span className="date-label">TODAY</span>

            <strong>
              {new Date().toLocaleDateString("en-IN", {
                day: "2-digit",
                month: "short",
                year: "numeric",
              })}
            </strong>
          </div>
        </header>

        {/* Statistics */}
        <section className="stats">

          <div className="stat-card">
            <div className="stat-top">
              <span>Total Students</span>
              <div className="stat-icon blue">👥</div>
            </div>

            <h2>{totalStudents}</h2>

            <p>Enrolled students</p>
          </div>

          <div className="stat-card">
            <div className="stat-top">
              <span>Present</span>
              <div className="stat-icon green">✓</div>
            </div>

            <h2>{presentCount}</h2>

            <p className="green-text">
              {attendancePercentage}% attendance
            </p>
          </div>

          <div className="stat-card">
            <div className="stat-top">
              <span>Absent</span>
              <div className="stat-icon red">×</div>
            </div>

            <h2>{absentCount}</h2>

            <p className="red-text">
              {totalStudents > 0
                ? Math.round((absentCount / totalStudents) * 100)
                : 0}
              % absent
            </p>
          </div>

          <div className="stat-card">
            <div className="stat-top">
              <span>Attendance Rate</span>
              <div className="stat-icon purple">%</div>
            </div>

            <h2>{attendancePercentage}%</h2>

            <div className="progress">
              <div
                className="progress-fill"
                style={{
                  width: `${attendancePercentage}%`,
                }}
              ></div>
            </div>
          </div>

        </section>

        {/* Student Attendance */}
        <section className="students-section">

          <div className="section-header">
            <div>
              <h2>Student Attendance</h2>
              <p>Mark each student's attendance for today.</p>
            </div>

            <div className="search-box">
              <span>⌕</span>

              <input
                type="text"
                placeholder="Search student..."
                value={search}
                onChange={(event) => setSearch(event.target.value)}
              />
            </div>
          </div>

          <div className="table-head">
            <span>STUDENT</span>
            <span>DEPARTMENT</span>
            <span>YEAR</span>
            <span>STATUS</span>
            <span>ACTION</span>
          </div>

          <div className="student-list">

            {filteredStudents.map((student) => {
              const status = attendance[student.id];

              return (
                <div className="student-row" key={student.id}>

                  <div className="student-info">
                    <div className="avatar">
                      {student.name.charAt(0)}
                    </div>

                    <div>
                      <h3>{student.name}</h3>

                      <p>
                        ID: STU-{String(student.id).padStart(3, "0")}
                      </p>
                    </div>
                  </div>

                  <div className="department">
                    {student.department}
                  </div>

                  <div className="year">
                    {student.year}
                  </div>

                  <div>
                    <span
                      className={
                        status === "Present"
                          ? "status status-present"
                          : "status status-absent"
                      }
                    >
                      <span className="status-dot"></span>
                      {status}
                    </span>
                  </div>

                  <div className="actions">

                    <button
                      className={
                        status === "Present"
                          ? "action-btn present active"
                          : "action-btn present"
                      }
                      onClick={() =>
                        markAttendance(student.id, "Present")
                      }
                    >
                      ✓ Present
                    </button>

                    <button
                      className={
                        status === "Absent"
                          ? "action-btn absent active"
                          : "action-btn absent"
                      }
                      onClick={() =>
                        markAttendance(student.id, "Absent")
                      }
                    >
                      × Absent
                    </button>

                  </div>

                </div>
              );
            })}

          </div>

          {filteredStudents.length === 0 && (
            <div className="empty">
              No students found.
            </div>
          )}

        </section>

        <footer>
          <span>Attendify</span>
          <span>•</span>
          <span>Attendance Management System</span>
        </footer>

      </main>
    </div>
  );
}

export default App;


@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Inter", sans-serif;
  background: #07111f;
  color: #e8eef7;
}

button,
input {
  font-family: inherit;
}

.app {
  min-height: 100vh;
  position: relative;
  overflow: hidden;
  background:
    radial-gradient(circle at top left, #102a4d 0%, transparent 35%),
    radial-gradient(circle at bottom right, #151b45 0%, transparent 35%),
    #07111f;
}

/* Decorative Glow */

.glow {
  position: fixed;
  width: 400px;
  height: 400px;
  border-radius: 50%;
  filter: blur(120px);
  opacity: 0.18;
  pointer-events: none;
}

.glow-one {
  background: #2787ff;
  top: -150px;
  left: -150px;
}

.glow-two {
  background: #7c5cff;
  bottom: -180px;
  right: -120px;
}

/* Dashboard */

.dashboard {
  width: 92%;
  max-width: 1400px;
  margin: auto;
  padding: 45px 0 30px;
  position: relative;
  z-index: 1;
}

/* Header */

.header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 35px;
}

.brand {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 25px;
  font-weight: 700;
  font-size: 19px;
}

.brand-icon {
  width: 35px;
  height: 35px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #3188ff, #6c5ce7);
  box-shadow: 0 8px 25px rgba(49, 136, 255, 0.3);
}

.header h1 {
  font-size: 36px;
  letter-spacing: -1.2px;
  margin-bottom: 8px;
}

.subtitle {
  color: #8190a5;
  font-size: 14px;
}

.date-box {
  padding: 15px 22px;
  border: 1px solid rgba(255,255,255,0.08);
  background: rgba(255,255,255,0.035);
  backdrop-filter: blur(15px);
  border-radius: 14px;
  text-align: right;
}

.date-label {
  display: block;
  color: #718096;
  font-size: 10px;
  letter-spacing: 1.5px;
  margin-bottom: 5px;
}

.date-box strong {
  font-size: 14px;
}

/* Stats */

.stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 18px;
  margin-bottom: 28px;
}

.stat-card {
  padding: 22px;
  border-radius: 18px;
  border: 1px solid rgba(255,255,255,0.07);
  background: rgba(255,255,255,0.045);
  backdrop-filter: blur(20px);
  transition: 0.3s ease;
}

.stat-card:hover {
  transform: translateY(-4px);
  border-color: rgba(72, 142, 255, 0.3);
  background: rgba(255,255,255,0.065);
}

.stat-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #8492a7;
  font-size: 13px;
  margin-bottom: 15px;
}

.stat-icon {
  width: 36px;
  height: 36px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
}

.blue {
  background: rgba(49,136,255,0.12);
  color: #4d9aff;
}

.green {
  background: rgba(34,197,94,0.12);
  color: #39d77a;
}

.red {
  background: rgba(239,68,68,0.12);
  color: #ff6565;
}

.purple {
  background: rgba(124,92,255,0.12);
  color: #947bff;
}

.stat-card h2 {
  font-size: 30px;
  margin-bottom: 5px;
}

.stat-card p {
  color: #69788d;
  font-size: 12px;
}

.green-text {
  color: #38d77a !important;
}

.red-text {
  color: #ff6565 !important;
}

/* Progress */

.progress {
  width: 100%;
  height: 5px;
  background: rgba(255,255,255,0.08);
  border-radius: 10px;
  margin-top: 15px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #3288ff, #765fff);
  border-radius: 10px;
  transition: width 0.5s ease;
}

/* Student Section */

.students-section {
  border: 1px solid rgba(255,255,255,0.07);
  background: rgba(255,255,255,0.035);
  backdrop-filter: blur(20px);
  border-radius: 20px;
  overflow: hidden;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 25px;
}

.section-header h2 {
  font-size: 18px;
  margin-bottom: 5px;
}

.section-header p {
  color: #718096;
  font-size: 12px;
}

.search-box {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 11px 15px;
  width: 240px;
  border-radius: 10px;
  border: 1px solid rgba(255,255,255,0.08);
  background: rgba(0,0,0,0.15);
}

.search-box span {
  color: #718096;
  font-size: 20px;
}

.search-box input {
  width: 100%;
  outline: none;
  border: none;
  background: transparent;
  color: white;
  font-size: 12px;
}

.search-box input::placeholder {
  color: #657388;
}

/* Table */

.table-head,
.student-row {
  display: grid;
  grid-template-columns: 2.2fr 1fr 1fr 1fr 2fr;
  align-items: center;
  gap: 20px;
}

.table-head {
  padding: 13px 25px;
  background: rgba(0,0,0,0.16);
  color: #637288;
  font-size: 10px;
  letter-spacing: 1px;
}

.student-row {
  padding: 17px 25px;
  border-top: 1px solid rgba(255,255,255,0.055);
  transition: 0.25s ease;
}

.student-row:hover {
  background: rgba(255,255,255,0.025);
}

/* Student */

.student-info {
  display: flex;
  align-items: center;
  gap: 13px;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 12px;
  background: linear-gradient(135deg, #1e4f89, #4b3f92);
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 700;
  color: white;
}

.student-info h3 {
  font-size: 13px;
  margin-bottom: 4px;
}

.student-info p {
  color: #627187;
  font-size: 10px;
}

.department,
.year {
  font-size: 12px;
  color: #9ba8b9;
}

/* Status */

.status {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  padding: 7px 11px;
  border-radius: 20px;
  font-size: 11px;
  font-weight: 600;
}

.status-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
}

.status-present {
  color: #3bd87d;
  background: rgba(59,216,125,0.09);
}

.status-present .status-dot {
  background: #3bd87d;
}

.status-absent {
  color: #ff6868;
  background: rgba(255,104,104,0.09);
}

.status-absent .status-dot {
  background: #ff6868;
}

/* Buttons */

.actions {
  display: flex;
  gap: 7px;
}

.action-btn {
  border: 1px solid rgba(255,255,255,0.07);
  padding: 8px 11px;
  border-radius: 8px;
  background: transparent;
  color: #7c8a9d;
  font-size: 10px;
  cursor: pointer;
  transition: 0.2s ease;
}

.action-btn:hover {
  transform: translateY(-1px);
}

.action-btn.present.active {
  background: rgba(59,216,125,0.12);
  color: #3bd87d;
  border-color: rgba(59,216,125,0.25);
}

.action-btn.absent.active {
  background: rgba(255,104,104,0.12);
  color: #ff6868;
  border-color: rgba(255,104,104,0.25);
}

/* Empty */

.empty {
  text-align: center;
  padding: 50px;
  color: #6d7b8f;
}

/* Footer */

footer {
  display: flex;
  justify-content: center;
  gap: 8px;
  color: #536175;
  font-size: 11px;
  margin-top: 25px;
}

footer span:first-child {
  color: #8290a4;
  font-weight: 600;
}

/* Responsive */

@media (max-width: 1000px) {
  .stats {
    grid-template-columns: repeat(2, 1fr);
  }

  .table-head {
    display: none;
  }

  .student-row {
    grid-template-columns: 1fr auto;
    gap: 15px;
  }

  .department,
  .year {
    display: none;
  }

  .student-row > div:nth-child(4) {
    grid-column: 2;
  }

  .actions {
    grid-column: 1 / -1;
  }
}

@media (max-width: 650px) {
  .dashboard {
    width: 94%;
    padding-top: 25px;
  }

  .header {
    flex-direction: column;
    align-items: flex-start;
    gap: 20px;
  }

  .date-box {
    text-align: left;
  }

  .header h1 {
    font-size: 28px;
  }

  .stats {
    grid-template-columns: 1fr;
  }

  .section-header {
    flex-direction: column;
    align-items: stretch;
    gap: 18px;
  }

  .search-box {
    width: 100%;
  }

  .student-row {
    grid-template-columns: 1fr;
  }

  .student-row > div:nth-child(4) {
    grid-column: auto;
  }

  .actions {
    grid-column: auto;
  }

  .action-btn {
    flex: 1;
  }
}


### 🔍 Student Search

Users can search for students by their name using the search bar.

The student list dynamically updates based on the search input.
<img width="1102" height="871" alt="image" src="https://github.com/user-attachments/assets/ab19c704-6883-4b34-ab94-512b1fe90d4d" />


### 📈 Attendance Progress

A visual progress bar displays the current overall attendance percentage.

### 📅 Current Date

The dashboard automatically displays today's date using JavaScript's `Date` object.

### 🎨 Modern UI

The application includes:

- Modern dashboard design
- Statistic cards
- Interactive buttons
- Status indicators
- Background glow effects
- Responsive layout
- Clean typography

---

## 🛠️ Technologies Used

- ⚛️ React.js
- 🟨 JavaScript (ES6+)
- 🎨 CSS3
- 🌐 HTML5
- ⚡ Vite

---

## 📂 Project Structure

```text
attendify/
│
├── public/
│
├── src/
│   ├── App.jsx
│   ├── App.css
│   ├── main.jsx
│   └── index.css
│
├── package.json
├── package-lock.json
└── README.md
