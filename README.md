<<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Mini Instagram</title>

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
border-radius:12px;
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
border-radius:8px;
background:#38bdf8;
color:white;
cursor:pointer;
margin-top:5px;
}

.post{
background:#334155;
padding:15px;
border-radius:10px;
margin-top:10px;
}

.post img{
width:100%;
border-radius:10px;
margin-top:10px;
}

/* MENU RADIAL */

.menu{
position:fixed;
bottom:40px;
right:40px;
width:240px;
height:240px;
}

.center{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:#38bdf8;
width:65px;
height:65px;
border-radius:50%;
display:flex;
align-items:center;
justify-content:center;
font-size:30px;
cursor:pointer;
z-index:10;
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
transform:scale(0);
transition:0.3s;
font-size:20px;
}

.menu.active .item{
opacity:1;
transform:scale(1);
}

/* posisi 7 menu melingkar */

.item:nth-child(2){top:-20px;left:95px;}      /* atas */

.item:nth-child(3){top:20px;left:185px;}      /* kanan atas */

.item:nth-child(4){top:100px;left:210px;}     /* kanan */

.item:nth-child(5){top:180px;left:150px;}     /* kanan bawah */

.item:nth-child(6){top:180px;left:40px;}      /* kiri bawah */

.item:nth-child(7){top:100px;left:-20px;}     /* kiri */

.item:nth-child(8){top:20px;left:0px;}        /* kiri atas */

.star{
font-size:28px;
cursor:pointer;
}

.light{
background:white;
color:black;
}

</style>
</head>

<body>

<header>Mini Instagram</header>

<div class="container" id="app"></div>

<div class="menu" id="menu">

<div class="center" onclick="toggleMenu()">+</div>

<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="feed()">📷</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="users()">👥</div>
<div class="item" onclick="searchUser()">🔎</div>
<div class="item" onclick="leaderboard()">🏆</div>
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
increment,
query,
orderBy,
onSnapshot
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig={
apiKey:"AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
authDomain:"danieldolar-9bca1.firebaseapp.com",
projectId:"danieldolar-9bca1"
};

const app=initializeApp(firebaseConfig);
const db=getFirestore(app);

function getUser(){
return localStorage.getItem("user")
}

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

if(name.trim()==""){
alert("Masukkan username")
return
}

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
<h2>Halo ${getUser()}</h2>
Selamat datang di Mini Instagram
<br><br>
<button onclick="theme()">Toggle Mode</button>
</div>
`

}

window.feed=function(){

let q=query(collection(db,"posts"),orderBy("time","desc"))

onSnapshot(q,(snapshot)=>{

let html=`

<div class="card">

<h3>Buat Post</h3>

<textarea id="note"></textarea>

<br><br>

<input id="img" placeholder="Link gambar">

<br><br>

<button onclick="postNote()">Posting</button>

</div>

`

snapshot.forEach(d=>{

let p=d.data()

html+=`

<div class="post">

<b onclick="profile('${p.user}')">${p.user}</b>

<p>${p.text}</p>

${p.img?`<img src="${p.img}">`:""}

<br>

❤️ ${p.likes||0}

<button onclick="like('${d.id}')">Like</button>

</div>

`

})

document.getElementById("app").innerHTML=html

})

}

window.postNote=async function(){

let text=document.getElementById("note").value
let img=document.getElementById("img").value

await addDoc(collection(db,"posts"),{

user:getUser(),
text:text,
img:img,
likes:0,
time:Date.now()

})

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

}

window.rating=async function(){

let data=await getDocs(collection(db,"ratings"))

let count=[0,0,0,0,0]

let html=`

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

<canvas id="barChart"></canvas>

<br>

<canvas id="pieChart"></canvas>

`

data.forEach(d=>{
let r=d.data()
count[r.value-1]++
})

html+=`</div>`

document.getElementById("app").innerHTML=html

new Chart(document.getElementById("barChart"),{
type:"bar",
data:{
labels:["1⭐","2⭐","3⭐","4⭐","5⭐"],
datasets:[{data:count}]
}
})

new Chart(document.getElementById("pieChart"),{
type:"pie",
data:{
labels:["1⭐","2⭐","3⭐","4⭐","5⭐"],
datasets:[{data:count}]
}
})

}

window.rate=async function(v){

await addDoc(collection(db,"ratings"),{

user:getUser(),
value:v

})

rating()

}

window.searchUser=function(){

document.getElementById("app").innerHTML=`

<div class="card">

<h2>Cari User</h2>

<input id="search">

<button onclick="doSearch()">Cari</button>

<div id="hasil"></div>

</div>

`

}

window.doSearch=async function(){

let key=document.getElementById("search").value

let users=await getDocs(collection(db,"users"))

let html=""

users.forEach(d=>{

let u=d.data().name

if(u.toLowerCase().includes(key.toLowerCase())){

html+=`<p onclick="profile('${u}')">${u}</p>`

}

})

document.getElementById("hasil").innerHTML=html

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

window.theme=function(){
document.body.classList.toggle("light")
}

if(getUser()){
home()
}else{
loginPage()
}

</script>

</body>
</html>
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
border-radius:12px;
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
border-radius:8px;
background:#38bdf8;
color:white;
cursor:pointer;
margin-top:5px;
}

.post{
background:#334155;
padding:15px;
border-radius:10px;
margin-top:10px;
}

.post img{
width:100%;
border-radius:10px;
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
font-size:28px;
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
transform:scale(0);
transition:0.3s;
}

.menu.active .item{
opacity:1;
transform:scale(1);
}

.item:nth-child(2){top:-20px;left:70px;}
.item:nth-child(3){top:30px;left:150px;}
.item:nth-child(4){top:120px;left:150px;}
.item:nth-child(5){top:170px;left:70px;}
.item:nth-child(6){top:120px;left:-10px;}
.item:nth-child(7){top:30px;left:-10px;}
.item:nth-child(8){top:70px;left:70px;}

.star{
font-size:28px;
cursor:pointer;
}

.light{
background:white;
color:black;
}

</style>
</head>

<body>

<header>Mini Instagram Daniel</header>

<div class="container" id="app"></div>

<div class="menu" id="menu">

<div class="center" onclick="toggleMenu()">+</div>

<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="feed()">📷</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="users()">👥</div>
<div class="item" onclick="topUser()">🔥</div>
<div class="item" onclick="leaderboard()">🏆</div>
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

function getUser(){
return localStorage.getItem("user")
}

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

if(name.trim()==""){
alert("Masukkan username")
return
}

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

<h2>Halo ${getUser()}</h2>

Selamat datang di Mini Instagram.

<br><br>

<button onclick="theme()">Toggle Mode</button>

</div>

`

}

window.feed=async function(){

let posts=await getDocs(collection(db,"posts"))

let html=`

<div class="card">

<h3>Buat Post</h3>

<textarea id="note"></textarea>

<br><br>

<input id="img" placeholder="Link gambar">

<br><br>

<button onclick="postNote()">Posting</button>

</div>

`

posts.forEach(d=>{

let p=d.data()

html+=`

<div class="post">

<b onclick="profile('${p.user}')">${p.user}</b>

<p>${p.text}</p>

${p.img?`<img src="${p.img}">`:""}

<br>

❤️ ${p.likes}

<button onclick="like('${d.id}')">Like</button>

${p.user==getUser()?`<button onclick="hapus('${d.id}')">Hapus</button>`:""}

</div>

`

})

document.getElementById("app").innerHTML=html

}

window.postNote=async function(){

let text=document.getElementById("note").value
let img=document.getElementById("img").value

await addDoc(collection(db,"posts"),{

user:getUser(),
text:text,
img:img,
likes:0,
time:Date.now()

})

feed()

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

feed()

}

window.hapus=async function(id){

await deleteDoc(doc(db,"posts",id))

feed()

}

window.rating=async function(){

let data=await getDocs(collection(db,"ratings"))

let count=[0,0,0,0,0]

let html=`

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

<canvas id="barChart"></canvas>

<br>

<canvas id="pieChart"></canvas>

<h3>Daftar Rating</h3>

`

data.forEach(d=>{
let r=d.data()
count[r.value-1]++
html+=`<p>${r.user} memberi ${r.value}⭐</p>`
})

html+=`</div>`

document.getElementById("app").innerHTML=html

new Chart(document.getElementById("barChart"),{
type:"bar",
data:{
labels:["1⭐","2⭐","3⭐","4⭐","5⭐"],
datasets:[{data:count}]
}
})

new Chart(document.getElementById("pieChart"),{
type:"pie",
data:{
labels:["1⭐","2⭐","3⭐","4⭐","5⭐"],
datasets:[{data:count}]
}
})

}

window.rate=async function(v){

await addDoc(collection(db,"ratings"),{

user:getUser(),
value:v

})

rating()

}

window.users=async function(){

let data=await getDocs(collection(db,"users"))

let html=`<div class="card"><h2>Pengguna</h2>`

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

window.topUser=async function(){

let data=await getDocs(collection(db,"followers"))

let count={}

data.forEach(d=>{
let f=d.data()
if(!count[f.target]) count[f.target]=0
count[f.target]++
})

let arr=Object.entries(count).sort((a,b)=>b[1]-a[1])

let html=`<div class="card"><h2>User Populer</h2>`

arr.forEach(a=>{
html+=`<p>${a[0]} : ${a[1]} followers</p>`
})

html+=`</div>`

document.getElementById("app").innerHTML=html

}

window.profile=async function(name){

let posts=await getDocs(collection(db,"posts"))
let followers=await getDocs(collection(db,"followers"))

let followerCount=0
let followingCount=0

followers.forEach(d=>{
let f=d.data()
if(f.target==name) followerCount++
if(f.user==name) followingCount++
})

let html=`<div class="card">

<h2>${name}</h2>

<p>Followers : ${followerCount}</p>
<p>Following : ${followingCount}</p>

<button onclick="followUser('${name}')">Follow / Unfollow</button>

<h3>Posting</h3>
`

posts.forEach(d=>{
let p=d.data()
if(p.user==name){
html+=`<div class="post">${p.text}</div>`
}
})

html+=`</div>`

document.getElementById("app").innerHTML=html

}

window.followUser=async function(target){

let user=getUser()

await addDoc(collection(db,"followers"),{
user:user,
target:target
})

alert("Sekarang kamu follow "+target)

}

window.theme=function(){
document.body.classList.toggle("light")
}

if(getUser()){
home()
}else{
loginPage()
}

</script>

</body>
</html>