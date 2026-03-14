<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Daniel Social Web</title>

<style>

body{
margin:0;
font-family:Arial;
background:#0f172a;
color:white;
}

header{
background:#020617;
padding:15px;
text-align:center;
font-size:22px;
}

.container{
max-width:900px;
margin:auto;
padding:20px;
}

.card{
background:#1e293b;
padding:20px;
border-radius:10px;
margin-bottom:20px;
}

textarea{
width:100%;
padding:10px;
border-radius:8px;
border:none;
}

button{
padding:10px 15px;
border:none;
border-radius:8px;
background:#38bdf8;
color:white;
cursor:pointer;
}

.post{
background:#334155;
padding:15px;
border-radius:8px;
margin-top:10px;
}

.menu{
position:fixed;
bottom:40px;
right:40px;
width:200px;
height:200px;
}

.center{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:#38bdf8;
width:60px;
height:60px;
border-radius:50%;
display:flex;
align-items:center;
justify-content:center;
font-size:25px;
cursor:pointer;
}

.item{
position:absolute;
width:50px;
height:50px;
background:#1e293b;
border-radius:50%;
display:flex;
align-items:center;
justify-content:center;
cursor:pointer;
opacity:0;
transition:0.4s;
}

.menu.active .item{opacity:1}

.item:nth-child(2){top:-20px;left:70px;}
.item:nth-child(3){top:30px;left:150px;}
.item:nth-child(4){top:120px;left:150px;}
.item:nth-child(5){top:170px;left:70px;}
.item:nth-child(6){top:120px;left:-10px;}
.item:nth-child(7){top:30px;left:-10px;}

</style>

</head>

<body>

<header>
Daniel Social Website
</header>

<div class="container" id="app"></div>

<div class="menu" id="menu">

<div class="center" onclick="toggleMenu()">+</div>

<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="notes()">📝</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="follow()">👥</div>
<div class="item" onclick="about()">👤</div>
<div class="item" onclick="logout()">🚪</div>

</div>

<script type="module">

import { initializeApp }
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import {
getFirestore,
collection,
addDoc,
getDocs,
updateDoc,
doc,
increment
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

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

let user = localStorage.getItem("user");

window.toggleMenu=function(){
document.getElementById("menu").classList.toggle("active")
}

function loginPage(){

document.getElementById("app").innerHTML=`

<div class="card">

<h2>Login</h2>

<input id="name" placeholder="Username">

<br><br>

<button onclick="login()">Masuk</button>

</div>

`

}

window.login=function(){

let name=document.getElementById("name").value

localStorage.setItem("user",name)

location.reload()

}

window.logout=function(){

localStorage.removeItem("user")

location.reload()

}

window.home=function(){

document.getElementById("app").innerHTML=`

<div class="card">

<h2>Halo ${user}</h2>

Selamat datang di website sosial sederhana.

</div>

`

}

window.notes=async function(){

let snap=await getDocs(collection(db,"posts"))

let html=`

<div class="card">

<h3>Buat Catatan</h3>

<textarea id="note"></textarea>

<br><br>

<button onclick="postNote()">Posting</button>

</div>

`

snap.forEach(d=>{

let p=d.data()

html+=`

<div class="post">

<b>${p.user}</b>

<p>${p.text}</p>

❤️ ${p.likes}

<button onclick="like('${d.id}')">Like</button>

</div>

`

})

document.getElementById("app").innerHTML=html

}

window.postNote=async function(){

let text=document.getElementById("note").value

await addDoc(collection(db,"posts"),{

user:user,
text:text,
likes:0,
time:Date.now()

})

notes()

}

window.like=async function(id){

let ref=doc(db,"posts",id)

await updateDoc(ref,{likes:increment(1)})

notes()

}

window.rating=function(){

document.getElementById("app").innerHTML=`

<div class="card">

<h2>Rating Website</h2>

<button onclick="rate(1)">⭐</button>
<button onclick="rate(2)">⭐⭐</button>
<button onclick="rate(3)">⭐⭐⭐</button>
<button onclick="rate(4)">⭐⭐⭐⭐</button>
<button onclick="rate(5)">⭐⭐⭐⭐⭐</button>

</div>

`

}

window.rate=async function(v){

await addDoc(collection(db,"ratings"),{

user:user,
value:v

})

alert("Terima kasih ratingnya")

}

window.follow=async function(){

await addDoc(collection(db,"followers"),{

user:user

})

alert("Sekarang kamu follow Daniel")

}

window.about=function(){

document.getElementById("app").innerHTML=`

<div class="card">

<h2>Tentang Saya</h2>

Nama : Daniel

<br><br>

Saya tertarik dengan teknologi, coding, dan pengembangan website.
Website ini adalah proyek sosial media sederhana yang saya buat sendiri.

</div>

`

}

if(!user){

loginPage()

}else{

home()

}

</script>

</body>
</html>