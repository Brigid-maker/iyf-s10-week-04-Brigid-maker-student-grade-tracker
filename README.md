// ------------------------------
// Student Grade Tracker
// ------------------------------
const gradeTracker = {
    students: [],

    // Add a new student
    addStudent(name, grades) {
        this.students.push({ name, grades });
    },

    // Get a student by name
    getStudent(name) {
        return this.students.find(student => student.name === name) || null;
    },

    // Calculate a student's average
    getStudentAverage(name) {
        const student = this.getStudent(name);
        if (!student) return null;
        const grades = Object.values(student.grades);
        const total = grades.reduce((sum, grade) => sum + grade, 0);
        return (total / grades.length).toFixed(2);
    },

    // Get class average for a subject
    getSubjectAverage(subject) {
        const subjectGrades = this.students
            .map(student => student.grades[subject])
            .filter(grade => grade !== undefined);
        const total = subjectGrades.reduce((sum, grade) => sum + grade, 0);
        return (total / subjectGrades.length).toFixed(2);
    },

    // Get top performer
    getTopStudent() {
        if (this.students.length === 0) return null;
        let topStudent = this.students[0];
        let topAverage = parseFloat(this.getStudentAverage(topStudent.name));

        this.students.forEach(student => {
            const avg = parseFloat(this.getStudentAverage(student.name));
            if (avg > topAverage) {
                topAverage = avg;
                topStudent = student;
            }
        });

        return topStudent.name;
    },

    // Get students needing help (average < 70)
    getStrugglingStudents() {
        return this.students
            .filter(student => parseFloat(this.getStudentAverage(student.name)) < 70)
            .map(student => student.name);
    },

    // Get letter grade
    getLetterGrade(score) {
        if (score >= 90) return "A";
        if (score >= 80) return "B";
        if (score >= 70) return "C";
        if (score >= 60) return "D";
        return "F";
    },

    // Generate report card
    generateReportCard(name) {
        const student = this.getStudent(name);
        if (!student) return "Student not found.";

        const grades = student.grades;
        let report = `Report Card for ${student.name}\n`;
        report += "------------------------\n";

        for (const [subject, grade] of Object.entries(grades)) {
            report += `${subject}: ${grade} (${this.getLetterGrade(grade)})\n`;
        }

        report += `Average: ${this.getStudentAverage(name)}\n`;
        report += `Letter Grade: ${this.getLetterGrade(parseFloat(this.getStudentAverage(name)))}\n`;
        return report;
    }
};

// ------------------------------
// Test Data
// ------------------------------
gradeTracker.addStudent("Alice", { math: 95, english: 88, science: 92 });
gradeTracker.addStudent("Bob", { math: 72, english: 85, science: 78 });
gradeTracker.addStudent("Charlie", { math: 60, english: 65, science: 58 });

// ------------------------------
// Demo Outputs
// ------------------------------
console.log("Alice's Average:", gradeTracker.getStudentAverage("Alice"));   // 91.67
console.log("Math Class Average:", gradeTracker.getSubjectAverage("math")); // 75.67
console.log("Top Student:", gradeTracker.getTopStudent());                  // Alice
console.log("Struggling Students:", gradeTracker.getStrugglingStudents());  // [Charlie]
console.log("\n" + gradeTracker.generateReportCard("Alice"));
console.log("\n" + gradeTracker.generateReportCard("Charlie"));