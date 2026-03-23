# iyf-s10-week-04-Brigid-maker-student-grade-tracker

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Student Grade Tracker</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; background: #f9f9f9; }
        h1 { color: #333; }
        pre { background: #eee; padding: 10px; border-radius: 5px; }
    </style>
</head>
<body>
    <h1>Student Grade Tracker</h1>
    <div id="output"></div>

    <!-- Load the tracker script -->
    <script src="gradeTracker.js"></script>
    <script>
        const outputDiv = document.getElementById("output");

        function show(message) {
            const pre = document.createElement("pre");
            pre.textContent = message;
            outputDiv.appendChild(pre);
        }

        // Display outputs from the tracker
        show("Alice's Average: " + gradeTracker.getStudentAverage("Alice"));
        show("Math Class Average: " + gradeTracker.getSubjectAverage("math"));
        show("Top Student: " + gradeTracker.getTopStudent());
        show("Struggling Students: " + gradeTracker.getStrugglingStudents().join(", "));
        show("\n" + gradeTracker.generateReportCard("Alice"));
        show("\n" + gradeTracker.generateReportCard("Charlie"));
    </script>
</body>
</html>