<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daniel Dolars App</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body{
margin:0;
font-family:Arial;
background:linear-gradient(135deg,#020617,#0f172a,#1e293b);
color:white;
}
.container{max-width:420px;margin:auto;padding:15px;}
.card{background:#1e293b;padding:15px;border-radius:15px;margin-top:10px;}
.profile{text-align:center;}
.profile img{
width:100px;height:100px;border-radius:50%;
border:3px solid #38bdf8;object-fit:cover;
}
button{
padding:8px 15px;border:none;border-radius:20px;
background:#38bdf8;color:black;font-weight:bold;margin:5px;
cursor:pointer;
}
textarea,input{
width:100%;padding:8px;border-radius:10px;border:none;margin-top:5px;
}
</style>
</head>

<body>

<div class="container" id="app">Loading...</div>

<script type="module">

/* 🔥 ERROR HANDLER (ANTI BLANK) */
window.onerror = function (msg, url, line) {
  document.body.innerHTML = "<h2 style='color:white;padding:20px'>ERROR:<br>"+msg+"<br>Line:"+line+"</h2>";
};

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import {
getFirestore,collection,addDoc,getDocs,
updateDoc,doc,increment,deleteDoc,
query,where
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

/* 🔥 CONFIG ASLI KAMU */
const firebaseConfig = {
  apiKey: "AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
  authDomain: "danieldolar-9bca1.firebaseapp.com",
  projectId: "danieldolar-9bca1",
};

const appFirebase = initializeApp(firebaseConfig);
const db = getFirestore(appFirebase);
const app = document.getElementById("app");

/* USER ID */
function user(){
let u=localStorage.getItem("user");
if(!u){
u="user_"+Math.random().toString(36).substr(2,5);
localStorage.setItem("user",u);
}
return u;
}

/* HOME */
async function home(){

let followers=await getDocs(collection(db,"followers"));

app.innerHTML=`
<div class="profile">
<img src="https://drive.google.com/file/d/1rXGo576CZ5a7pxeNPq2cutKD1HVA5iAi/view?usp=drivesdk">
<h2>Daniel Dolars</h2>
</div>

<div class="card">
Followers: ${followers.size}
<button onclick="follow()">Follow / Unfollow</button>
</div>

<div class="card">
<button onclick="feed()">Feed</button>
<button onclick="rating()">Rating</button>
<button onclick="leaderboard()">Leaderboard</button>
<button onclick="stats()">Statistik</button>
</div>
`;
}

/* FOLLOW */
window.follow=async function(){

let q=query(collection(db,"followers"),where("user","==",user()));
let snap=await getDocs(q);

if(snap.empty){
await addDoc(collection(db,"followers"),{user:user()});
}else{
await deleteDoc(doc(db,"followers",snap.docs[0].id));
}
home();
}

/* FEED */
window.feed=async function(){

let data=await getDocs(collection(db,"posts"));

let html=`
<div class="card">
<textarea id="txt" placeholder="Tulis sesuatu..."></textarea>
<button onclick="post()">Posting</button>
</div>
`;

data.forEach(d=>{
let p=d.data();
html+=`
<div class="card">
${p.text}<br>
❤️ ${p.likes || 0}
<button onclick="like('${d.id}')">Like</button>
</div>
`;
});

app.innerHTML=html;
}

/* POST */
window.post=async function(){

if(!txt.value.trim()){
alert("Tidak boleh kosong!");
return;
}

await addDoc(collection(db,"posts"),{
user:user(),
text:txt.value,
likes:0
});

feed();
}

/* LIKE */
window.like=async function(id){
await updateDoc(doc(db,"posts",id),{likes:increment(1)});
feed();
}

/* RATING */
window.rating=async function(){

let data=await getDocs(collection(db,"ratings"));
let count=[0,0,0,0,0];

data.forEach(d=>{
let r=d.data().rating;
if(r>=1 && r<=5) count[r-1]++;
});

app.innerHTML=`
<div class="card">
<h3>Rating</h3>
${[1,2,3,4,5].map(i=>`<span onclick="rate(${i})">★</span>`).join("")}
<canvas id="chart"></canvas>
</div>
`;

new Chart(document.getElementById("chart"),{
type:"pie",
data:{labels:["1","2","3","4","5"],datasets:[{data:count}]}
});
}

/* RATE */
window.rate=async function(v){

let q=query(collection(db,"ratings"),where("user","==",user()));
let snap=await getDocs(q);

if(snap.empty){
await addDoc(collection(db,"ratings"),{user:user(),rating:v});
}else{
await updateDoc(doc(db,"ratings",snap.docs[0].id),{rating:v});
}
rating();
}

/* LEADERBOARD */
window.leaderboard=async function(){

let data=await getDocs(collection(db,"posts"));
let map={};

data.forEach(d=>{
let u=d.data().user;
map[u]=(map[u]||0)+1;
});

let arr=Object.entries(map).sort((a,b)=>b[1]-a[1]);

let html=`<div class="card"><h3>Leaderboard</h3>`;
arr.forEach((a,i)=>{
html+=`<p>${i+1}. ${a[0]} - ${a[1]} post</p>`;
});
html+=`</div>`;

app.innerHTML=html;
}

/* STATS */
window.stats=async function(){

let f=(await getDocs(collection(db,"followers"))).size;

app.innerHTML=`
<div class="card">
<canvas id="c"></canvas>
</div>
`;

new Chart(document.getElementById("c"),{
type:"pie",
data:{labels:["Follow","Lainnya"],datasets:[{data:[f,Math.max(0,100-f)]}]}
});
}

home();

</script>

</body>
</html>