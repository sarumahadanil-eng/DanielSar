<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Daniel Dolar Sarumaha</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
font-family:Arial;
margin:0;
background:#0f172a;
color:white;
}

header{
background:#38bdf8;
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

input,textarea{
width:100%;
padding:8px;
border-radius:6px;
border:none;
}

button{
padding:8px 14px;
border:none;
border-radius:6px;
background:#38bdf8;
color:white;
cursor:pointer;
margin-top:6px;
}

.post{
background:#334155;
padding:10px;
border-radius:10px;
margin-top:10px;
}

.post img{
width:100%;
border-radius:8px;
}

.star{
font-size:30px;
cursor:pointer;
color:#475569;
}

.star.active{
color:#facc15;
}

.menu{
position:fixed;
bottom:40px;
right:40px;
width:220px;
height:220px;
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
width:45px;
height:45px;
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

.item:nth-child(2){top:-10px;left:90px;}
.item:nth-child(3){top:20px;left:170px;}
.item:nth-child(4){top:90px;left:200px;}
.item:nth-child(5){top:160px;left:140px;}
.item:nth-child(6){top:160px;left:40px;}
.item:nth-child(7){top:90px;left:-20px;}
.item:nth-child(8){top:20px;left:0px;}

.light{
background:white;
color:black;
}

</style>
</head>

<body>

<header>Mini Social App Daniel</header>

<div class="container" id="app"></div>

<div class="menu" id="menu">

<div class="center" onclick="toggleMenu()">+</div>

<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="feed()">📷</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="users()">👥</div>
<div class="item" onclick="leaderboard()">🏆</div>
<div class="item" onclick="theme()">🌓</div>
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

const appFirebase=initializeApp(firebaseConfig);
const db=getFirestore(appFirebase);

function user(){
return localStorage.getItem("user")
}

window.toggleMenu=function(){
menu.classList.toggle("active")
}

function loginPage(){
app.innerHTML=`
<div class="card">
<h2>Login</h2>
<input id="name" placeholder="Username">
<button onclick="login()">Masuk</button>
</div>
`
}

window.login=async function(){

let name=nameInput()

localStorage.setItem("user",name)

await addDoc(collection(db,"users"),{
name:name
})

location.reload()

}

function nameInput(){
return document.getElementById("name").value
}

window.logout=function(){
localStorage.removeItem("user")
location.reload()
}

window.home=function(){

app.innerHTML=`
<div class="card">
<h2>Halo ${user()}</h2>
Selamat datang di Mini Social App
</div>
`

}

window.feed=function(){

let q=query(collection(db,"posts"),orderBy("time","desc"))

onSnapshot(q,(snap)=>{

let html=`

<div class="card">

<textarea id="text"></textarea>

<input id="img" placeholder="Link gambar">

<button onclick="post()">Post</button>

</div>

`

snap.forEach(d=>{

let p=d.data()

html+=`

<div class="post">

<b>${p.user}</b>

<p>${p.text}</p>

${p.img?`<img src="${p.img}">`:""}

<br>

❤️ ${p.likes||0}

<button onclick="like('${d.id}')">Like</button>

</div>

`

})

app.innerHTML=html

})

}

window.post=async function(){

await addDoc(collection(db,"posts"),{

user:user(),
text:text.value,
img:img.value,
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
let total=0
let sum=0
let my=0

data.forEach(d=>{

let r=d.data()

count[r.value-1]++
total++
sum+=r.value

if(r.user==user()){
my=r.value
}

})

let avg=(sum/total||0).toFixed(1)

let html=`
<div class="card">
<h2>Rating Website</h2>
<h3>${avg}/5 ⭐</h3>
<div id="stars">
<span class="star" data="1">★</span>
<span class="star" data="2">★</span>
<span class="star" data="3">★</span>
<span class="star" data="4">★</span>
<span class="star" data="5">★</span>
</div>
<canvas id="chart"></canvas>
</div>
`

app.innerHTML=html

document.querySelectorAll(".star").forEach(s=>{

s.onclick=()=>rate(s.getAttribute("data"))

})

new Chart(chart,{
type:"bar",
data:{
labels:["1","2","3","4","5"],
datasets:[{data:count}]
}
})

}

window.rate=async function(v){

let data=await getDocs(collection(db,"ratings"))

let found=false
let id=""

data.forEach(d=>{

if(d.data().user==user()){
found=true
id=d.id
}

})

if(found){

await updateDoc(doc(db,"ratings",id),{value:Number(v)})

}else{

await addDoc(collection(db,"ratings"),{
user:user(),
value:Number(v)
})

}

rating()

}

window.users=async function(){

let data=await getDocs(collection(db,"users"))

let html=`<div class="card"><h2>Users</h2>`

data.forEach(d=>{
html+=`<p>${d.data().name}</p>`
})

html+=`</div>`

app.innerHTML=html

}

window.leaderboard=async function(){

let posts=await getDocs(collection(db,"posts"))

let count={}

posts.forEach(d=>{
let u=d.data().user
count[u]=(count[u]||0)+1
})

let arr=Object.entries(count).sort((a,b)=>b[1]-a[1])

let html=`<div class="card"><h2>Leaderboard</h2>`

arr.forEach(a=>{
html+=`<p>${a[0]} : ${a[1]} post</p>`
})

html+=`</div>`

app.innerHTML=html

}

window.theme=function(){
document.body.classList.toggle("light")
}

if(user()){
home()
}else{
loginPage()
}

</script>

</body>
</html>