<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Interactive Student Grade Tracker</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; background: #f9f9f9; }
        h1, h2 { color: #333; }
        label { display: block; margin-top: 10px; }
        input { margin-bottom: 5px; }
        button { margin-top: 10px; padding: 5px 10px; }
        table { border-collapse: collapse; margin-bottom: 20px; width: 100%; }
        table, th, td { border: 1px solid #ccc; }
        th, td { padding: 8px; text-align: center; }
        .section { margin-bottom: 30px; padding: 15px; background: #fff; border-radius: 5px; box-shadow: 0 0 5px #ccc; }
    </style>
</head>
<body>
    <h1>Student Grade Tracker Dashboard</h1>

    <!-- Add student form -->
    <div class="section">
        <h2>Add a Student</h2>
        <label>Name: <input type="text" id="studentName"></label>
        <label>Math: <input type="number" id="mathGrade" min="0" max="100"></label>
        <label>English: <input type="number" id="englishGrade" min="0" max="100"></label>
        <label>Science: <input type="number" id="scienceGrade" min="0" max="100"></label>
        <button id="addStudentBtn">Add Student</button>
    </div>

    <!-- Generate report card -->
    <div class="section">
        <h2>Generate Report Card</h2>
        <label>Student Name: <input type="text" id="reportName"></label>
        <button id="generateReportBtn">Generate Report</button>
    </div>

    <!-- Student Grades Table -->
    <div class="section">
        <h2>All Students</h2>
        <table id="studentTable">
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Math</th>
                    <th>English</th>
                    <th>Science</th>
                    <th>Average</th>
                    <th>Letter</th>
                </tr>
            </thead>
            <tbody>
                <!-- Dynamically filled -->
            </tbody>
        </table>
    </div>

    <!-- Class Summary Table -->
    <div class="section">
        <h2>Class Summary</h2>
        <table>
            <tbody>
                <tr><td>Top Student</td><td id="topStudent"></td></tr>
                <tr><td>Struggling Students</td><td id="strugglingStudents"></td></tr>
                <tr><td>Math Average</td><td id="mathAverage"></td></tr>
                <tr><td>English Average</td><td id="englishAverage"></td></tr>
                <tr><td>Science Average</td><td id="scienceAverage"></td></tr>
            </tbody>
        </table>
    </div>

    <script src="gradeTracker.js"></script>
    <script>
        const addBtn = document.getElementById("addStudentBtn");
        const generateBtn = document.getElementById("generateReportBtn");
        const studentTableBody = document.querySelector("#studentTable tbody");
        const topStudentCell = document.getElementById("topStudent");
        const strugglingCell = document.getElementById("strugglingStudents");
        const mathAvgCell = document.getElementById("mathAverage");
        const englishAvgCell = document.getElementById("englishAverage");
        const scienceAvgCell = document.getElementById("scienceAverage");

        function updateStudentTable() {
            studentTableBody.innerHTML = "";
            gradeTracker.students.forEach(student => {
                const avg = gradeTracker.getStudentAverage(student.name);
                const letter = gradeTracker.getLetterGrade(parseFloat(avg));
                const row = `<tr>
                    <td>${student.name}</td>
                    <td>${student.grades.math}</td>
                    <td>${student.grades.english}</td>
                    <td>${student.grades.science}</td>
                    <td>${avg}</td>
                    <td>${letter}</td>
                </tr>`;
                studentTableBody.innerHTML += row;
            });
        }

        function updateClassSummary() {
            topStudentCell.textContent = gradeTracker.getTopStudent();
            strugglingCell.textContent = gradeTracker.getStrugglingStudents().join(", ") || "None";
            mathAvgCell.textContent = gradeTracker.getSubjectAverage("math");
            englishAvgCell.textContent = gradeTracker.getSubjectAverage("english");
            scienceAvgCell.textContent = gradeTracker.getSubjectAverage("science");
        }

        function refreshAll() {
            updateStudentTable();
            updateClassSummary();
        }

        // Initial display
        refreshAll();

        addBtn.addEventListener("click", () => {
            const name = document.getElementById("studentName").value.trim();
            const math = parseFloat(document.getElementById("mathGrade").value);
            const english = parseFloat(document.getElementById("englishGrade").value);
            const science = parseFloat(document.getElementById("scienceGrade").value);

            if (!name || isNaN(math) || isNaN(english) || isNaN(science)) {
                alert("Please fill in all fields correctly!");
                return;
            }

            gradeTracker.addStudent(name, { math, english, science });
            document.getElementById("studentName").value = "";
            document.getElementById("mathGrade").value = "";
            document.getElementById("englishGrade").value = "";
            document.getElementById("scienceGrade").value = "";

            refreshAll();
        });

        generateBtn.addEventListener("click", () => {
            const name = document.getElementById("reportName").value.trim();
            if (!name) {
                alert("Enter a student name!");
                return;
            }
            alert(gradeTracker.generateReportCard(name));
            document.getElementById("reportName").value = "";
        });
    </script>
</body>
</html>
