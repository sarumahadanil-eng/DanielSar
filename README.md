<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Daniel Dolar Sarumaha</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
margin:0;
font-family:Arial;
background:linear-gradient(135deg,#020617,#0f172a,#1e293b);
color:white;
}

header{
background:linear-gradient(90deg,#38bdf8,#0284c7);
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

textarea,input{
width:100%;
padding:10px;
border-radius:8px;
border:none;
}

button{
padding:8px 12px;
border:none;
border-radius:6px;
background:#38bdf8;
color:white;
cursor:pointer;
margin-top:5px;
}

.post{
background:#334155;
padding:15px;
border-radius:8px;
margin-top:10px;
}

.commentBox{
background:#0f172a;
padding:10px;
border-radius:6px;
margin-top:10px;
}

.star{
font-size:30px;
cursor:pointer;
}

.star.active{
color:gold;
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
.item:nth-child(8){top:70px;left:70px;}

.light{
background:white;
color:black;
}

</style>
</head>

<body>

<header>Daniel Website</header>

<div class="container" id="app"></div>

<div class="menu" id="menu">

<div class="center" onclick="toggleMenu()">+</div>

<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="notes()">📝</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="users()">👥</div>
<div class="item" onclick="leaderboard()">🏆</div>
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
deleteDoc,
doc,
increment
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig={
apiKey:"AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
authDomain:"danieldolar-9bca1.firebaseapp.com",
projectId:"danieldolar-9bca1",
storageBucket:"danieldolar-9bca1.firebasestorage.app",
messagingSenderId:"4879222744",
appId:"1:4879222744:web:e441fe6b15b34fb42314ad"
};

const app=initializeApp(firebaseConfig);
const db=getFirestore(app);

let user=localStorage.getItem("user")

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

window.login=async function(){

let name=document.getElementById("name").value

localStorage.setItem("user",name)

await addDoc(collection(db,"users"),{
name:name,
time:Date.now()
})

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

Selamat datang di website Daniel Dolar Sarumaha, Senang berjumpa dengan anda.

<br><br>

<button onclick="theme()">Toggle Mode</button>

</div>

`

}

window.notes=async function(){

let posts=await getDocs(collection(db,"posts"))
let comments=await getDocs(collection(db,"comments"))

let comData={}

comments.forEach(d=>{
let c=d.data()
if(!comData[c.post]) comData[c.post]=[]
comData[c.post].push(c)
})

let html=`

<div class="card">

<h3>Buat Post</h3>

<textarea id="note"></textarea>

<br><br>

<input id="img" placeholder="Link gambar (opsional)">

<br><br>

<button onclick="postNote()">Posting</button>

</div>

`

posts.forEach(d=>{

let p=d.data()

html+=`

<div class="post">

<b onclick="profile('${p.user}')" style="cursor:pointer">${p.user}</b>

<p>${p.text}</p>

${p.img?`<img src="${p.img}" style="width:100%;border-radius:10px">`:""}

<br><br>

❤️ ${p.likes}

<button onclick="like('${d.id}')">Like</button>

${p.user==user?`<button onclick="hapus('${d.id}')">Hapus</button>`:""}

<div class="commentBox">

<b>Komentar</b><br>

`

if(comData[d.id]){

comData[d.id].forEach(c=>{
html+=`<div>${c.user}: ${c.text}</div>`
})

}

html+=`

<input id="c-${d.id}" placeholder="Komentar">

<button onclick="sendComment('${d.id}')">Kirim</button>

</div>

</div>

`

})

document.getElementById("app").innerHTML=html

}

window.postNote=async function(){

let text=document.getElementById("note").value
let img=document.getElementById("img").value

await addDoc(collection(db,"posts"),{

user:user,
text:text,
img:img,
likes:0,
time:Date.now()

})

notes()

}

window.like=async function(id){

let liked=localStorage.getItem("like-"+id)

let ref=doc(db,"posts",id)

if(!liked){

await updateDoc(ref,{likes:increment(1)})
localStorage.setItem("like-"+id,true)

}else{

await updateDoc(ref,{likes:increment(-1)})
localStorage.removeItem("like-"+id)

}

notes()

}

window.hapus=async function(id){

await deleteDoc(doc(db,"posts",id))

notes()

}

window.sendComment=async function(id){

let text=document.getElementById("c-"+id).value

await addDoc(collection(db,"comments"),{

post:id,
user:user,
text:text

})

notes()

}

window.rating=async function(){

let data=await getDocs(collection(db,"ratings"))

let count=[0,0,0,0,0]

data.forEach(d=>{
count[d.data().value-1]++
})

document.getElementById("app").innerHTML=`

<div class="card">

<h2>Rating Website</h2>

<div>

<span class="star" onclick="rate(1)">★</span>
<span class="star" onclick="rate(2)">★</span>
<span class="star" onclick="rate(3)">★</span>
<span class="star" onclick="rate(4)">★</span>
<span class="star" onclick="rate(5)">★</span>

</div>

<br>

<canvas id="chart"></canvas>

</div>

`

new Chart(document.getElementById("chart"),{

type:"bar",

data:{
labels:["1⭐","2⭐","3⭐","4⭐","5⭐"],
datasets:[{
label:"Jumlah Rating",
data:count
}]
}

})

}

window.rate=async function(v){

await addDoc(collection(db,"ratings"),{

user:user,
value:v

})

rating()

}

window.users=async function(){

let data=await getDocs(collection(db,"users"))

let html=`<div class="card"><h2>Daniel Web</h2>`

data.forEach(d=>{
html+=`<p>${d.data().name}</p>`
})

html+=`</div>`

document.getElementById("app").innerHTML=html

}

window.leaderboard=async function(){

let posts=await getDocs(collection(db,"posts"))

let count={}

posts.forEach(d=>{
let u=d.data().user
if(!count[u]) count[u]=0
count[u]++
})

let arr=Object.entries(count).sort((a,b)=>b[1]-a[1])

let html=`<div class="card"><h2>User Teraktif</h2>`

arr.forEach(a=>{
html+=`<p>${a[0]} : ${a[1]} post</p>`
})

html+=`</div>`

document.getElementById("app").innerHTML=html

}

window.profile=async function(name){

let posts=await getDocs(collection(db,"posts"))

let html=`<div class="card"><h2>Profil ${name}</h2>`

posts.forEach(d=>{
let p=d.data()
if(p.user==name){
html+=`<div class="post">${p.text}</div>`
}
})

html+=`</div>`

document.getElementById("app").innerHTML=html

}

window.about=function(){

document.getElementById("app").innerHTML=`

<div class="card">

<h2>Profil Daniel</h2>

Nama : Daniel website

<br><br>

let me introduce my self first, mah name is Daniel dolar sarumaha,can you call me Daniel or Dolar, i am nineteen years old, i am from south Nias,i live in Hiliamaetaniha village, my hobbies a badminton and swimming, my dreams a teacher🧑‍🏫 and officer👨‍💼. nice to meet you🤝

</div>

`

}

window.theme=function(){

document.body.classList.toggle("light")

}

if(!user){

loginPage()

}else{

home()

}

</script>

</body>
</html>