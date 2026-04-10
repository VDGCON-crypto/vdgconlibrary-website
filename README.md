<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BSC Nursing Library ERP</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">

<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Inter',sans-serif}
body{display:flex;background:#f1f5f9}

/* Sidebar */
.sidebar{
width:240px;
background:#111827;
color:white;
height:100vh;
position:fixed;
padding:20px;
}

.sidebar h2{font-size:18px;margin-bottom:30px}
.sidebar a{
display:block;
color:#cbd5e1;
padding:10px;
border-radius:8px;
text-decoration:none;
margin-bottom:5px;
}
.sidebar a:hover{background:#1f2937;color:white}

/* Main */
.main{
margin-left:240px;
width:100%;
padding:20px;
}

.header{
display:flex;
justify-content:space-between;
align-items:center;
margin-bottom:20px;
}

.card-grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:15px;
margin-bottom:20px;
}

.card{
background:white;
padding:18px;
border-radius:12px;
box-shadow:0 2px 8px rgba(0,0,0,0.05);
}

.table{
background:white;
border-radius:12px;
overflow:hidden;
}

table{width:100%;border-collapse:collapse}
th,td{padding:12px}
th{background:#2563eb;color:white;text-align:left}

button{
background:#2563eb;
color:white;
border:none;
padding:8px 14px;
border-radius:8px;
cursor:pointer;
}

input,select{
padding:9px;
border:1px solid #ddd;
border-radius:8px;
margin-right:8px;
}

.login-screen{
position:fixed;
top:0;
left:0;
right:0;
bottom:0;
background:#0f172a;
display:flex;
align-items:center;
justify-content:center;
}

.login-box{
background:white;
padding:30px;
border-radius:12px;
width:320px;
}

.ai-box{
position:fixed;
bottom:20px;
right:20px;
width:280px;
background:white;
border-radius:12px;
box-shadow:0 4px 20px rgba(0,0,0,0.15);
}

.ai-header{background:#2563eb;color:white;padding:10px}
.ai-body{height:200px;overflow:auto;padding:10px}
.ai-input{display:flex;border-top:1px solid #ddd}
.ai-input input{flex:1;border:none;padding:8px}

@media(max-width:768px){
.sidebar{width:200px}
.main{margin-left:200px}
}
</style>
</head>

<body>

<!-- Login -->
<div class="login-screen" id="loginScreen">
<div class="login-box">
<h3>ERP Login</h3>
<br>
<input type="email" id="email" placeholder="Email"><br><br>
<input type="password" id="password" placeholder="Password"><br><br>
<select id="role">
<option value="admin">Admin</option>
<option value="librarian">Librarian</option>
<option value="student">Student</option>
</select>
<br><br>
<button onclick="login()">Login</button>
</div>
</div>

<div class="sidebar">
<h2>Library ERP</h2>
<a href="#">Dashboard</a>
<a href="#">Books</a>
<a href="#">Students</a>
<a href="#">Attendance</a>
<a href="#">Reports</a>
</div>

<div class="main">
<div class="header">
<h2>Dashboard</h2>
<button onclick="logout()">Logout</button>
</div>

<div class="card-grid">
<div class="card"><h3 id="bookCount">0</h3><p>Total Books</p></div>
<div class="card"><h3 id="userCount">0</h3><p>Users</p></div>
</div>

<div class="card">
<h3>Add Book</h3>
<input id="bookName" placeholder="Book">
<input id="author" placeholder="Author">
<button onclick="addBook()">Save</button>
</div>

<br>

<div class="table">
<table>
<thead><tr><th>Book</th><th>Author</th></tr></thead>
<tbody id="bookTable"></tbody>
</table>
</div>
</div>

<div class="ai-box">
<div class="ai-header">AI Assistant</div>
<div class="ai-body" id="chat"></div>
<div class="ai-input">
<input id="aiText">
<button onclick="askAI()">Send</button>
</div>
</div>

<script>
const firebaseConfig={
apiKey:"YOUR_API_KEY",
authDomain:"YOUR_DOMAIN",
projectId:"YOUR_PROJECT_ID",
storageBucket:"YOUR_BUCKET",
messagingSenderId:"YOUR_SENDER",
appId:"YOUR_APP_ID"
};

firebase.initializeApp(firebaseConfig);
const db=firebase.firestore();
const auth=firebase.auth();

function login(){
const email=document.getElementById('email').value;
const pass=document.getElementById('password').value;
const role=document.getElementById('role').value;

auth.signInWithEmailAndPassword(email,pass)
.then(()=>{
document.getElementById('loginScreen').style.display='none';
})
.catch(()=>{
auth.createUserWithEmailAndPassword(email,pass);
});
}

function logout(){
auth.signOut();
document.getElementById('loginScreen').style.display='flex';
}

function addBook(){
const name=document.getElementById('bookName').value;
const author=document.getElementById('author').value;

db.collection('books').add({name,author});
}

function loadBooks(){
db.collection('books').onSnapshot(s=>{
let html='';let count=0;
s.forEach(doc=>{
const d=doc.data();
html+=`<tr><td>${d.name}</td><td>${d.author}</td></tr>`;
count++;
});

document.getElementById('bookTable').innerHTML=html;
document.getElementById('bookCount').innerText=count;
});
}

loadBooks();

function askAI(){
const text=document.getElementById('aiText').value;
const chat=document.getElementById('chat');
chat.innerHTML+=`<div>You: ${text}</div>`;
let r="I can manage ERP modules.";
if(text.includes('books')) r="Use add book section.";
if(text.includes('attendance')) r="Attendance module active.";
chat.innerHTML+=`<div>AI: ${r}</div>`;
}
</script>

</body>
</html>
