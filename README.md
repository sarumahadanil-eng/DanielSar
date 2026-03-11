<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Daniel Dolar Sarumaha</title>

<style>
body{
margin:0;
font-family:Arial;
background:linear-gradient(135deg,#020617,#0f172a,#1e293b);
color:white;
}

header{
background:#020617;
padding:15px;
display:flex;
justify-content:space-between;
align-items:center;
}

main{
max-width:700px;
margin:auto;
padding:20px;
}

.card{
background:#1e293b;
padding:15px;
border-radius:12px;
margin-top:15px;
}

.post{
background:#020617;
padding:12px;
border-radius:10px;
margin-top:10px;
}

input,textarea{
width:100%;
padding:10px;
margin-top:8px;
border:none;
border-radius:6px;
}

button{
margin-top:10px;
padding:8px 14px;
border:none;
border-radius:6px;
background:#0ea5e9;
color:white;
cursor:pointer;
}

.comment{
font-size:13px;
margin-left:10px;
color:#cbd5f5;
}
</style>
</head>

<body>

<header>
<span>Daniel Dolar Sarumaha</span>
<div id="userArea"></div>
</header>

<main id="app"></main>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import {
getFirestore,
collection,
addDoc,
getDocs,
query,
where,
onSnapshot,
doc,
updateDoc,
increment
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {
apiKey: "AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
authDomain: "danieldolar-9bca1.firebaseapp.com",
projectId: "danieldolar-9bca1",
storageBucket: "danieldolar-9bca1.firebasestorage.app",
messagingSenderId: "4879222744",
appId: "1:4879222744:web:e441fe6b15b34fb42314ad"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);


/* HASH PASSWORD */

async function hash(text){
const msg = new TextEncoder().encode(text);
const buf = await crypto.subtle.digest("SHA-256", msg);
return Array.from(new Uint8Array(buf)).map(b=>b.toString(16).padStart(2,"0")).join("");
}



/* LOGIN PAGE */

window.loginPage = function(){

document.getElementById("app").innerHTML = `

<div class="card">

<h3>Login</h3>

<input id="user" placeholder="Username">

<input id="pass" type="password" placeholder="Password">

<button onclick="login()">Login</button>

<p>Belum punya akun?</p>

<button onclick="registerPage()">Register</button>

</div>

`;

}



/* REGISTER PAGE */

window.registerPage = function(){

document.getElementById("app").innerHTML = `

<div class="card">

<h3>Register</h3>

<input id="ruser" placeholder="Username">

<input id="rpass" type="password" placeholder="Password">

<button onclick="register()">Daftar</button>

<button onclick="loginPage()">Kembali</button>

</div>

`;

}



/* REGISTER */

window.register = async function(){

let user = document.getElementById("ruser").value;
let pass = document.getElementById("rpass").value;

const q = query(collection(db,"users"),where("username","==",user));
const snap = await getDocs(q);

if(!snap.empty){
alert("Username sudah dipakai");
return;
}

let hashed = await hash(pass);

await addDoc(collection(db,"users"),{
username:user,
password:hashed
});

alert("Akun berhasil dibuat");

loginPage();

}



/* LOGIN */

window.login = async function(){

let user = document.getElementById("user").value;
let pass = document.getElementById("pass").value;

let hashed = await hash(pass);

const q = query(
collection(db,"users"),
where("username","==",user),
where("password","==",hashed)
);

const snap = await getDocs(q);

if(snap.empty){

alert("Login gagal");

}else{

localStorage.setItem("user",user);

home();

}

}



/* HOME */

function home(){

let user = localStorage.getItem("user");

document.getElementById("userArea").innerHTML =
user + " <button onclick='logout()'>Logout</button>";

document.getElementById("app").innerHTML = `

<div class="card">

<textarea id="text" placeholder="Tulis sesuatu..."></textarea>

<button onclick="post()">Posting</button>

</div>

<div class="card">

<h3>Postingan</h3>

<div id="posts"></div>

</div>

`;

loadPosts();

}



/* POST */

window.post = async function(){

let text = document.getElementById("text").value;
let user = localStorage.getItem("user");

if(text.trim()==""){
alert("Tulis sesuatu dulu");
return;
}

await addDoc(collection(db,"posts"),{
user:user,
text:text,
likes:0,
time:Date.now()
});

document.getElementById("text").value="";

}



/* LOAD POSTS */

function loadPosts(){

onSnapshot(collection(db,"posts"),snap=>{

let html="";

snap.forEach(docu=>{

let p = docu.data();

html += `

<div class="post">

<b>${p.user}</b>

<p>${p.text}</p>

<button onclick="like('${docu.id}')">
👍 ${p.likes}
</button>

<br>

<input id="c${docu.id}" placeholder="Komentar">

<button onclick="comment('${docu.id}')">Kirim</button>

<div id="cm${docu.id}"></div>

</div>

`;

loadComments(docu.id);

});

document.getElementById("posts").innerHTML = html;

});

}



/* LIKE */

window.like = async function(id){

const ref = doc(db,"posts",id);

await updateDoc(ref,{
likes:increment(1)
});

}



/* COMMENT */

window.comment = async function(id){

let text = document.getElementById("c"+id).value;
let user = localStorage.getItem("user");

await addDoc(collection(db,"comments"),{
post:id,
user:user,
text:text
});

}



/* LOAD COMMENTS */

function loadComments(postId){

const q = query(
collection(db,"comments"),
where("post","==",postId)
);

onSnapshot(q,snap=>{

let html="";

snap.forEach(d=>{

let c=d.data();

html+=`<div class="comment"><b>${c.user}</b>: ${c.text}</div>`;

});

setTimeout(()=>{

let el=document.getElementById("cm"+postId);

if(el) el.innerHTML=html;

},200);

});

}



/* LOGOUT */

window.logout=function(){

localStorage.removeItem("user");

location.reload();

}



/* AUTO LOGIN */

if(localStorage.getItem("user")){

home();

}else{

loginPage();

}

</script>

</body>
</html>