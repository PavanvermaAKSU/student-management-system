# student-management-system
<!DOCTYPE html>
<html>
<head>
    <title>Student Management System</title>
    <style>
        body {
            font-family: Arial;
            background: linear-gradient(to right, #667eea, #764ba2);
            padding: 40px;
        }

        .container {
            background: white;
            padding: 25px;
            border-radius: 10px;
            max-width: 900px;
            margin: auto;
        }

        h2 {
            text-align: center;
        }

        button {
            padding: 8px 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
            background: #667eea;
            color: white;
        }

        input {
            padding: 6px;
            width: 95%;
            margin-bottom: 10px;
        }

        form {
            margin-top: 20px;
            display: none;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            border: 1px solid #ccc;
            padding: 8px;
            text-align: center;
        }

        th {
            background: #667eea;
            color: white;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>Student Management System</h2>

    <button onclick="showAdd()">Add Student</button>
    <button onclick="showDelete()">Delete Student</button>
    <button onclick="showTable()">Show Students</button>

    <form id="addForm">
        <h3>Add Student</h3>
        <input required type="number" id="sid" placeholder="ID">
        <input type="text" id="sname" placeholder="Name">
        <input type="text" id="snumber" placeholder="Number">
        <input type="text" id="scourse" placeholder="Course">
        <input type="number" id="sage" placeholder="Age">
        <button type="button" onclick="addStudent()">Save</button>
    </form>

    <form id="deleteForm">
        <h3>Delete Student</h3>
        <input type="number" id="delId" placeholder="Enter ID">
        <button type="button" onclick="deleteStudent()">Delete</button>
    </form>

    <div id="tableSection"></div>
</div>
<link rel="stylesheet" href="https://cdn.datatables.net/2.3.6/css/dataTables.dataTables.css" />
  <script src="https://code.jquery.com/jquery-4.0.0.js" integrity="sha256-9fsHeVnKBvqh3FB2HYu7g2xseAZ5MlN6Kz/qnkASV8U=" crossorigin="anonymous"></script>
<script src="https://cdn.datatables.net/2.3.6/js/dataTables.js">
    

</script>
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

<script>
   
let students = JSON.parse(localStorage.getItem("students")) || [];

const addForm = document.getElementById("addForm");
const deleteForm = document.getElementById("deleteForm");
const tableSection = document.getElementById("tableSection");

function showAdd() {
    addForm.style.display = "block";
    deleteForm.style.display = "none";
    showTable();
}

function showDelete() {
    deleteForm.style.display = "block";
    addForm.style.display = "none";
    showTable();
}

function showTable() {
    if (students.length === 0) {
        tableSection.innerHTML = "<p>No student data available</p>";
        return;
    }

    let html = `
    <table id="myTable" class="display">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Number</th>
                <th>Course</th>
                <th>Age</th>
            </tr>
        </thead>
        <tbody>`;

    students.forEach(s => {
        html += `
        <tr>
            <td>${s.id}</td>
            <td>${s.name}</td>
            <td>${s.number}</td>
            <td>${s.course}</td>
            <td>${s.age}</td>
        </tr>`;
    });

    html += `
        </tbody>
    </table>`;

    tableSection.innerHTML = html;

    // DataTable init AFTER table exists
    $('#myTable').DataTable({
        destroy: true   // very important
    });
}

function addStudent() {
    let id = document.getElementById("sid").value;
    let name = document.getElementById("sname").value;
    let number = document.getElementById("snumber").value;
    let course = document.getElementById("scourse").value;
    let age = document.getElementById("sage").value;

    if (!id || !name) {
       // alert("ID and Name required");
        Swal.fire({
  title: "error !",
  text: "ID and Name required",
  icon: "question"
});
        return;
    }

    if (students.some(s => s.id == id)) {
        alert("ID already exists");
        return;
    }

    students.push({ id, name, number, course, age });
    localStorage.setItem("students", JSON.stringify(students));

    alert("Student Added");
    addForm.reset();
    showTable();
}

function deleteStudent() {
    let del = document.getElementById("delId").value;
    students = students.filter(s => s.id != del);
    localStorage.setItem("students", JSON.stringify(students));

    alert("Student Deleted");
    deleteForm.reset();
    showTable();
}

showTable();
 $(document).ready(function () {
        $('#myTable').DataTable();
        Swal.fire({
  title: "welcome",
  text: "welcome student management system",
  icon: "happy"
});
       //alert("Welcome to Student Management System");
    });
</script>

</body>
</html>
