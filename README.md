<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BSC Nursing Library - Smart Portal</title>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Inter,Segoe UI,Arial}

body{
display:flex;
background:#f1f5f9;
}

/* SIDEBAR */
.sidebar{
width:240px;
background:#0f172a;
color:white;
height:100vh;
position:fixed;
transition:.3s;
}

.sidebar h2{
padding:20px;
font-size:18px;
border-bottom:1px solid #1e293b;
}

.sidebar a{
display:flex;
align-items:center;
gap:10px;
padding:12px 20px;
color:#cbd5e1;
text-decoration:none;
font-size:14px;
}

.sidebar a:hover{background:#1e293b;color:white}

/* MAIN */
.main{
margin-left:240px;
width:100%;
}

/* TOPBAR */
.topbar{
background:white;
padding:15px 20px;
display:flex;
justify-content:space-between;
align-items:center;
box-shadow:0 2px 10px rgba(0,0,0,.05);
}

.search{
padding:8px 12px;
border-radius:8px;
border:1px solid #ddd;
}

.top-icons i{
margin-left:15px;
cursor:pointer;
}

/* DASHBOARD */
.dashboard{
padding:20px;
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:15px;
}

.card{
background:white;
padding:20px;
border-radius:14px;
box-shadow:0 5px 20px rgba(0,0,0,.05);
}

.card h3{color:#2563eb;margin-bottom:5px}

/* TABLE */
.table{
padding:20px;
}

table{
width:100%;
border-collapse:collapse;
background:white;
border-radius:12px;
overflow:hidden;
}

table th{
background:#f8fafc;
text-align:left;
padding:12px;
font-size:13px;
}

table td{
padding:12px;
border-top:1px solid #eee;
font-size:13px;
}

/* AI */
.ai-btn{
position:fixed;
bottom:20px;
right:20px;
width:60px;
height:60px;
border-radius:50%;
background:#2563eb;
color:white;
display:flex;
align-items:center;
justify-content:center;
font-size:20px;
cursor:pointer;
box-shadow:0 10px 25px rgba(0,0,0,.2);
}

.ai-box{
position:fixed;
right:20px;
bottom:90px;
width:360px;
height:520px;
background:white;
border-radius:16px;
display:none;
flex-direction:column;
box-shadow:0 20px 40px rgba(0,0,0,.2);
}

.ai-header{
background:#2563eb;
color:white;
padding:14px;
border-radius:16px 16px 0 0;
font-weight:600;
}

.ai-chat{
flex:1;
padding:10px;
overflow-y:auto;
background:#f8fafc;
}

.ai-input{
display:flex;
border-top:1px solid #ddd;
}

.ai-input input{
flex:1;
padding:12px;
border:none;
}

.ai-input button{
background:#2563eb;
color:white;
border:none;
padding:12px;
}

.msg{padding:8px 12px;margin:5px 0;border-radius:10px;font-size:13px}
.user{background:#dbeafe;margin-left:auto}
.bot{background:white;border:1px solid #ddd}

/* DARK MODE */
.dark body{background:#020617}

/* MOBILE */
@media(max-width:768px){
.sidebar{display:none}
.main{margin-left:0}
}

</style>
</head>

<body>

<div class="sidebar">
<h2>Library Portal</h2>
<a href="#"><i class="fa fa-chart-line"></i>Dashboard</a>
<a href="#"><i class="fa fa-book"></i>Books</a>
<a href="#"><i class="fa fa-user"></i>Students</a>
<a href="#"><i class="fa fa-file"></i>Notes</a>
<a href="#"><i class="fa fa-question"></i>Papers</a>
<a href="#"><i class="fa fa-chart-pie"></i>Reports</a>
<a href="#"><i class="fa fa-gear"></i>Settings</a>
</div>

<div class="main">

<div class="topbar">
<input class="search" placeholder="Search...">
<div class="top-icons">
<i class="fa fa-bell"></i>
<i class="fa fa-moon" onclick="toggleDark()"></i>
</div>
</div>

<div class="dashboard">
<div class="card"><h3>Total Books</h3><p>1250</p></div>
<div class="card"><h3>Students</h3><p>320</p></div>
<div class="card"><h3>Issued</h3><p>210</p></div>
<div class="card"><h3>Visitors</h3><p>89</p></div>
</div>

<div class="table">
<table>
<tr><th>Book</th><th>Dept</th><th>Status</th></tr>
<tr><td>Fundamentals</td><td>1st Year</td><td>Available</td></tr>
<tr><td>Pharmacology</td><td>2nd Year</td><td>Issued</td></tr>
<tr><td>Community Health</td><td>3rd Year</td><td>Available</td></tr>
</table>
</div>

</div>

<div class="ai-btn" onclick="toggleAI()"><i class="fa fa-robot"></i></div>

<div class="ai-box" id="ai">
<div class="ai-header">Smart AI Assistant</div>
<div class="ai-chat" id="chat"></div>
<div class="ai-input">
<input id="msg" placeholder="Ask anything...">
<button onclick="send()">Send</button>
</div>
</div>

<script>
function toggleAI(){
let ai=document.getElementById("ai");
ai.style.display=ai.style.display=="flex"?"none":"flex";
}

function send(){
let input=document.getElementById("msg");
let text=input.value;
if(!text) return;
add(text,"user");
input.value="";
setTimeout(()=>add(aiBrain(text),"bot"),500);
}

function add(text,type){
let div=document.createElement("div");
div.className="msg "+type;
div.innerText=text;
document.getElementById("chat").appendChild(div);
}

function aiBrain(msg){
msg=msg.toLowerCase();

if(msg.includes("book")) return "Books module allows add, edit and issue tracking.";
if(msg.includes("student")) return "Student management system available.";
if(msg.includes("report")) return "Reports include usage analytics and book trends.";
if(msg.includes("hello")) return "Hello 👋 I am Smart Library AI.";
if(msg.includes("issue")) return "210 books currently issued.";

return "I support admin queries, analytics, reports and library automation.";
}

function toggleDark(){
document.body.classList.toggle("dark");
}
</script>

</body>
</html>
