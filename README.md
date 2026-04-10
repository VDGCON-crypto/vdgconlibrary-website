<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BSC Nursing Library - Official</title>
<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}
body{
    font-family:Arial, Helvetica, sans-serif;
    background:#f4f6f8;
}
header{
    background:#0b5ed7;
    color:white;
    padding:20px;
    text-align:center;
}
header h1{
    margin-bottom:5px;
}
nav{
    background:#084298;
    display:flex;
    justify-content:center;
    flex-wrap:wrap;
}
nav a{
    color:white;
    padding:14px 20px;
    text-decoration:none;
    font-weight:bold;
}
nav a:hover{
    background:#06357a;
}
.container{
    padding:20px;
}
.card{
    background:white;
    padding:20px;
    margin-bottom:15px;
    border-radius:10px;
    box-shadow:0 2px 5px rgba(0,0,0,0.1);
}
.card h2{
    color:#0b5ed7;
    margin-bottom:10px;
}
footer{
    background:#084298;
    color:white;
    text-align:center;
    padding:15px;
    margin-top:20px;
}
.search-box{
    width:100%;
    padding:10px;
    border-radius:6px;
    border:1px solid #ccc;
}

/* AI Chat */
.ai-button{
    position:fixed;
    bottom:20px;
    right:20px;
    background:#0b5ed7;
    color:white;
    padding:15px;
    border-radius:50%;
    cursor:pointer;
    box-shadow:0 3px 10px rgba(0,0,0,0.2);
}

.ai-chat{
    position:fixed;
    bottom:80px;
    right:20px;
    width:300px;
    background:white;
    border-radius:10px;
    box-shadow:0 5px 20px rgba(0,0,0,0.2);
    display:none;
    flex-direction:column;
}

.ai-header{
    background:#0b5ed7;
    color:white;
    padding:10px;
    border-radius:10px 10px 0 0;
}

.ai-messages{
    height:250px;
    overflow-y:auto;
    padding:10px;
}

.ai-input{
    display:flex;
    border-top:1px solid #ddd;
}

.ai-input input{
    flex:1;
    border:none;
    padding:10px;
}

.ai-input button{
    background:#0b5ed7;
    color:white;
    border:none;
    padding:10px;
}

.message{
    margin:5px 0;
    padding:8px;
    border-radius:5px;
}

.user{
    background:#d1e7ff;
    text-align:right;
}

.bot{
    background:#e9ecef;
}

</style>
</head>

<body>

<header>
<h1>BSC Nursing Library</h1>
<p>Official Digital Library Portal</p>
</header>

<nav>
<a href="#home">Home</a>
<a href="#books">Books</a>
<a href="#notes">Notes</a>
<a href="#question">Question Papers</a>
<a href="#contact">Contact</a>
</nav>

<div class="container">

<div class="card" id="home">
<h2>Welcome</h2>
<p>This is the official BSC Nursing Library portal. Students can access notes, books, previous year papers and AI assistance.</p>
</div>

<div class="card" id="books">
<h2>Library Books</h2>
<input type="text" class="search-box" placeholder="Search Books..." onkeyup="searchBooks(this.value)">
<ul id="bookList">
<li>Fundamentals of Nursing</li>
<li>Medical Surgical Nursing</li>
<li>Anatomy and Physiology</li>
<li>Pharmacology for Nurses</li>
<li>Community Health Nursing</li>
<li>Microbiology</li>
</ul>
</div>

<div class="card" id="notes">
<h2>Study Notes</h2>
<ul>
<li>1st Year Notes</li>
<li>2nd Year Notes</li>
<li>3rd Year Notes</li>
<li>4th Year Notes</li>
</ul>
</div>

<div class="card" id="question">
<h2>Previous Year Question Papers</h2>
<ul>
<li>MUHS 2025</li>
<li>MUHS 2024</li>
<li>MUHS 2023</li>
</ul>
</div>

<div class="card" id="contact">
<h2>Contact Library</h2>
<p>Email: library@nursingcollege.edu</p>
<p>Phone: +91-0000000000</p>
</div>

</div>

<footer>
<p>© 2026 BSC Nursing College Library | All Rights Reserved</p>
</footer>

<div class="ai-button" onclick="toggleAI()">AI</div>

<div class="ai-chat" id="aiChat">
<div class="ai-header">Library AI Assistant</div>
<div class="ai-messages" id="messages"></div>
<div class="ai-input">
<input type="text" id="userInput" placeholder="Ask something...">
<button onclick="sendMessage()">Send</button>
</div>
</div>

<script>
function searchBooks(value){
let items=document.querySelectorAll("#bookList li");
value=value.toLowerCase();
items.forEach(item=>{
item.style.display=item.textContent.toLowerCase().includes(value)?"":"none";
});
}

function toggleAI(){
let chat=document.getElementById("aiChat");
chat.style.display=chat.style.display=="flex"?"none":"flex";
}

function sendMessage(){
let input=document.getElementById("userInput");
let message=input.value;
if(message=="") return;

addMessage(message,"user");

let response=getAIResponse(message);

setTimeout(()=>addMessage(response,"bot"),500);

input.value="";
}

function addMessage(text,type){
let div=document.createElement("div");
div.className="message "+type;
div.textContent=text;
document.getElementById("messages").appendChild(div);
}

function getAIResponse(msg){
msg=msg.toLowerCase();

if(msg.includes("book")) return "You can search books in the library section.";
if(msg.includes("notes")) return "Notes are available semester wise.";
if(msg.includes("time")) return "Library timing: 9 AM to 5 PM.";
if(msg.includes("hello")) return "Hello! How can I help you?";

return "Please contact librarian for more details.";
}
</script>

</body>
</html>

