<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BSC Nursing Smart ERP AI</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">

<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Inter',sans-serif}
body{display:flex;background:#f1f5f9}

.sidebar{width:240px;background:#0f172a;color:white;height:100vh;position:fixed;padding:20px}
.sidebar h2{margin-bottom:25px}
.sidebar a{display:block;color:#cbd5e1;padding:10px;border-radius:8px;text-decoration:none;margin-bottom:5px}
.sidebar a:hover{background:#1e293b;color:white}

.main{margin-left:240px;width:100%;padding:20px}
.header{display:flex;justify-content:space-between;margin-bottom:20px}

.card-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:15px}
.card{background:white;padding:18px;border-radius:12px;box-shadow:0 2px 8px rgba(0,0,0,0.05)}

.table{background:white;border-radius:12px;overflow:hidden;margin-top:15px}
table{width:100%;border-collapse:collapse}
th,td{padding:12px}
th{background:#2563eb;color:white;text-align:left}

button{background:#2563eb;color:white;border:none;padding:8px 14px;border-radius:8px;cursor:pointer}
input,select{padding:9px;border:1px solid #ddd;border-radius:8px;margin-right:8px}

.login-screen{position:fixed;inset:0;background:#020617;display:flex;align-items:center;justify-content:center}
.login-box{background:white;padding:30px;border-radius:12px;width:320px}

.ai-box{position:fixed;bottom:20px;right:20px;width:320px;background:white;border-radius:12px;box-shadow:0 5px 20px rgba(0,0,0,0.2)}
.ai-header{background:#2563eb;color:white;padding:10px}
.ai-body{height:240px;overflow:auto;padding:10px;font-size:14px}
.ai-input{display:flex;border-top:1px solid #ddd}
.ai-input input{flex:1;border:none;padding:10px}

</style>
</head>
<body>

<div class="login-screen" id="loginScreen">
<div class="login-box">
<h3>Smart ERP Login</h3><br>
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
<h2>Smart Library AI</h2>
<a>Dashboard</a>
<a>Books</a>
<a>Students</a>
<a>Attendance</a>
<a>Reports</a>
</div>

<div class="main">
<div class="header">
<h2>Dashboard</h2>
<button onclick="logout()">Logout</button>
</div>

<div class="card-grid">
<div class="card"><h3 id="bookCount">0</h3><p>Total Books</p></div>
<div class="card"><h3 id="userCount">0</h3><p>Total Users</p></div>
<div class="card"><h3 id="issueCount">0</h3><p>Issued</p></div>
</div>

<div class="card">
<h3>Add Book</h3>
<input id="bookName" placeholder="Book Name">
<input id="author" placeholder="Author">
<button onclick="addBook()">Add</button>
</div>

<div class="table">
<table>
<thead><tr><th>Book</th><th>Author</th></tr></thead>
<tbody id="bookTable"></tbody>
</table>
</div>

</div>

<div class="ai-box">
<div class="ai-header">Smart ERP AI</div>
<div class="ai-body" id="chat"></div>
<div class="ai-input">
<input id="aiText" placeholder="Ask anything...">
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

let currentRole="";

function login(){
const email=email.value;
const pass=password.value;
currentRole=role.value;

auth.signInWithEmailAndPassword(email,pass)
.then(()=>loginSuccess())
.catch(()=>{
auth.createUserWithEmailAndPassword(email,pass).then(()=>loginSuccess())
});
}

function loginSuccess(){
document.getElementById('loginScreen').style.display='none';
loadBooks();
countUsers();
}

function logout(){
auth.signOut();
document.getElementById('loginScreen').style.display='flex';
}

function addBook(){
if(currentRole==="student") return alert("Students cannot add books");
const name=bookName.value;
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
bookTable.innerHTML=html;
bookCount.innerText=count;
});
}

function countUsers(){
db.collection('users').onSnapshot(s=>{
userCount.innerText=s.size;
});
}

function askAI(){
const text=aiText.value.toLowerCase();
chat.innerHTML+=`<div><b>You:</b> ${text}</div>`;

let reply="I am Smart ERP AI.";

if(text.includes('add book')) reply="Use add book section.";
if(text.includes('attendance')) reply="Attendance module connected.";
if(text.includes('who am i')) reply="You are logged in as "+currentRole;
if(text.includes('stats')) reply="Books: "+bookCount.innerText;
if(text.includes('help')) reply="I can manage books, users, reports.";

chat.innerHTML+=`<div><b>AI:</b> ${reply}</div>`;
}
</script>

</body>
</html>

