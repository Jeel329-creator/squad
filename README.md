# squad
GVP_AI_hackathon_2026
<html >
<head>

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Smart College ERP</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Poppins',sans-serif;
}

/* ANIMATED BACKGROUND */

body{
min-height:100vh;
background:linear-gradient(270deg,#ff00cc,#3333ff,#00ffcc,#ff6600);
background-size:600% 600%;
animation:bgMove 12s ease infinite;
display:flex;
justify-content:center;
align-items:center;
padding:15px;
color:white;
}

@keyframes bgMove{
0%{background-position:0% 50%;}
50%{background-position:100% 50%;}
100%{background-position:0% 50%;}
}

/* GLASS CARD */

.page{
display:none;
width:100%;
max-width:1300px;
background:rgba(255,255,255,0.12);
backdrop-filter:blur(18px);
border-radius:25px;
padding:30px;
box-shadow:0 15px 60px rgba(0,0,0,.5);
animation:fade .5s;
}

@keyframes fade{
from{opacity:0;transform:translateY(20px);}
to{opacity:1;}
}

.active{
display:block;
}

.center{
text-align:center;
}

/* BUTTONS */

button{
border:none;
border-radius:14px;
padding:12px 22px;
margin:6px;
font-weight:600;
cursor:pointer;
color:white;
background:linear-gradient(45deg,#00c6ff,#0072ff);
transition:.3s;
}

button:hover{
transform:scale(1.08);
box-shadow:0 5px 20px rgba(0,0,0,.4);
}

.bigBtn{
width:230px;
height:80px;
font-size:18px;
}

.deleteBtn{
background:linear-gradient(45deg,#ff416c,#ff4b2b);
}

input{
padding:11px;
border:none;
border-radius:10px;
margin:6px;
outline:none;
}

/* TABLE */

.tableWrap{
overflow-x:auto;
margin-top:20px;
}

table{
width:100%;
border-collapse:collapse;
min-width:850px;
}

th{
background:rgba(0,0,0,.6);
padding:14px;
}

td{
background:rgba(255,255,255,.15);
padding:12px;
text-align:center;
}

.good{color:#00ff9d;font-weight:bold;}
.avg{color:#ffd166;font-weight:bold;}
.low{color:#ff6b6b;font-weight:bold;}

/* MOBILE */

@media(max-width:700px){

.bigBtn{
width:100%;
}

input{
width:95%;
}

}

</style>
</head>

<body>

<!-- WELCOME PAGE -->
<div id="welcome" class="page active center">

<h1 style="font-size:45px;">🎓 Smart College ERP</h1>
<br>
<h3>Next-Generation Attendance & Performance System</h3>

<br><br>

<button onclick="showPage('courses')" class="bigBtn">
Enter System →
</button>

</div>


<!-- COURSE PAGE -->
<div id="courses" class="page center">

<h1>Select Course</h1>
<br>

<button class="bigBtn" onclick="selectCourse('BCA')">BCA</button>
<button class="bigBtn" onclick="selectCourse('MCA')">MCA</button>
<button class="bigBtn" onclick="selectCourse('MSc IT')">MSc IT</button>
<button class="bigBtn" onclick="selectCourse('PGDCA')">PGDCA</button>
<button class="bigBtn" onclick="selectCourse('Journalism')">Journalism</button>

<br><br>

<button onclick="showPage('welcome')">⬅ Back</button>

</div>



<!-- MODULE -->
<div id="module" class="page center">

<h1 id="courseTitle"></h1>
<br>

<button class="bigBtn" onclick="showPage('attendance')">
📊 Attendance
</button>

<button class="bigBtn" onclick="showPage('marks')">
📈 Subjects & Performance
</button>

<br><br>
<button onclick="showPage('courses')">Change Course</button>

</div>



<!-- ATTENDANCE -->
<div id="attendance" class="page">

<h1>Attendance Tracker</h1>
<h3 id="dateTime"></h3>

<div class="center">

<input id="rollA" placeholder="Roll Number">
<input id="nameA" placeholder="Student Name">

<br>

<button onclick="addStudent()">Add Student</button>
<button onclick="addLecture()">Add Lecture</button>

</div>

<div class="tableWrap">
<table>

<thead>
<tr>
<th>Roll</th>
<th>Name</th>
<th>Attended</th>
<th>Total</th>
<th>%</th>
<th>Status</th>
<th>Toggle</th>
<th>Delete</th>
</tr>
</thead>

<tbody id="attendanceBody"></tbody>

</table>
</div>

<br>
<button onclick="showPage('module')">⬅ Back</button>

</div>



<!-- MARKS -->
<div id="marks" class="page">

<h1>Performance Tracker</h1>

<div class="center">

<input id="rollM" placeholder="Roll">
<input id="nameM" placeholder="Name">
<input id="s1" placeholder="Subject 1">
<input id="s2" placeholder="Subject 2">
<input id="s3" placeholder="Subject 3">

<br>

<button onclick="addMarks()">Add Student</button>

</div>

<div class="tableWrap">
<table>

<thead>
<tr>
<th>Roll</th>
<th>Name</th>
<th>S1</th>
<th>S2</th>
<th>S3</th>
<th>%</th>
<th>Performance</th>
<th>Delete</th>
</tr>
</thead>

<tbody id="marksBody"></tbody>

</table>
</div>

<br>
<button onclick="showPage('module')">⬅ Back</button>

</div>



<script>

/* PAGE NAVIGATION */

function showPage(id){
document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
document.getElementById(id).classList.add("active");
updateClock();
}

/* CLOCK */

function updateClock(){
let now=new Date();
document.getElementById("dateTime").innerText=
now.toLocaleString();
}

/* DATA */

let data=JSON.parse(localStorage.getItem("ERP")) || {};
let selectedCourse="";

function selectCourse(course){

selectedCourse=course;

if(!data[course]){
data[course]={
attendance:{total:0,students:[]},
marks:[]
};
}

document.getElementById("courseTitle").innerText=course;

showPage("module");

renderAttendance();
renderMarks();
}

function save(){
localStorage.setItem("ERP",JSON.stringify(data));
}


/* ATTENDANCE */

function addLecture(){
data[selectedCourse].attendance.total++;
save();
renderAttendance();
}

function addStudent(){

let roll=rollA.value.trim();
let name=nameA.value.trim();

if(!roll || !name)
return alert("Enter valid data!");

let students=data[selectedCourse].attendance.students;

if(students.some(s=>s.roll===roll))
return alert("Roll already exists!");

students.push({roll,name,attended:0});

rollA.value="";
nameA.value="";

save();
renderAttendance();
}

function toggle(i){

let att=data[selectedCourse].attendance;
let s=att.students[i];

if(s.attended<att.total)
s.attended++;
else
s.attended--;

save();
renderAttendance();
}

function deleteA(i){
if(confirm("Delete this student?")){
data[selectedCourse].attendance.students.splice(i,1);
save();
renderAttendance();
}
}

function renderAttendance(){

let body=document.getElementById("attendanceBody");
body.innerHTML="";

let att=data[selectedCourse].attendance;

att.students.forEach((s,i)=>{

let percent=att.total===0?0:((s.attended/att.total)*100).toFixed(1);

let status="Weak";
let cls="low";

if(percent>=75){status="Excellent";cls="good";}
else if(percent>=50){status="Average";cls="avg";}

body.innerHTML+=`
<tr>
<td>${s.roll}</td>
<td>${s.name}</td>
<td>${s.attended}</td>
<td>${att.total}</td>
<td>${percent}%</td>
<td class="${cls}">${status}</td>
<td><button onclick="toggle(${i})">Toggle</button></td>
<td><button class="deleteBtn" onclick="deleteA(${i})">Delete</button></td>
</tr>`;
});
}


/* MARKS */

function addMarks(){

let roll=rollM.value.trim();
let name=nameM.value.trim();

let s1=Number(document.getElementById("s1").value);
let s2=Number(document.getElementById("s2").value);
let s3=Number(document.getElementById("s3").value);

if(!roll || !name)
return alert("Enter valid data!");

let list=data[selectedCourse].marks;

if(list.some(s=>s.roll===roll))
return alert("Duplicate roll!");

let percent=((s1+s2+s3)/3).toFixed(1);

let perf="Weak";
let cls="low";

if(percent>=80){perf="Excellent";cls="good";}
else if(percent>=60){perf="Good";cls="avg";}
else if(percent>=40){perf="Average";cls="avg";}

list.push({roll,name,s1,s2,s3,percent,perf});

rollM.value=nameM.value="";
s1.value=s2.value=s3.value="";

save();
renderMarks();
}

function deleteM(i){
if(confirm("Delete this student?")){
data[selectedCourse].marks.splice(i,1);
save();
renderMarks();
}
}

function renderMarks(){

let body=document.getElementById("marksBody");
body.innerHTML="";

data[selectedCourse].marks.forEach((s,i)=>{

let cls="low";
if(s.percent>=80) cls="good";
else if(s.percent>=60) cls="avg";

body.innerHTML+=`
<tr>
<td>${s.roll}</td>
<td>${s.name}</td>
<td>${s.s1}</td>
<td>${s.s2}</td>
<td>${s.s3}</td>
<td>${s.percent}%</td>
<td class="${cls}">${s.perf}</td>
<td><button class="deleteBtn" onclick="deleteM(${i})">Delete</button></td>
</tr>`;
});
}

</script>

</body>
</html>
