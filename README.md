<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DanielSar PRO CLEAN</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body{
margin:0;
font-family:sans-serif;
background:#020617;
color:white;
}

.header{
padding:15px;
font-size:20px;
color:#38bdf8;
}

.card{
margin:10px;
padding:15px;
border-radius:15px;
background:#0f172a;
}

button{
padding:8px 15px;
border:none;
border-radius:20px;
background:#38bdf8;
margin:5px;
cursor:pointer;
}

input,textarea{
width:100%;
padding:8px;
margin-top:5px;
border-radius:10px;
border:none;
background:#020617;
color:white;
}

img{max-width:100%;border-radius:10px;}
.small{font-size:12px;opacity:0.7;}
</style>
</head>

<body>

<div id="app"></div>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import {
getFirestore,collection,addDoc,getDocs,
updateDoc,doc,getDoc
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const db = getFirestore(initializeApp({
apiKey:"AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
authDomain:"danieldolar-9bca1.firebaseapp.com",
projectId:"danieldolar-9bca1"
}));

const app = document.getElementById("app");

/* USER */
function user(){
let u=localStorage.getItem("user");
if(!u){
u="user_"+Math.random().toString(36).substr(2,5);
localStorage.setItem("user",u);
}
return u;
}

/* UI */
function nav(){
return `
<div class="header">🚀 DanielSar</div>
<div class="card">
<button onclick="feed()">Feed</button>
<button onclick="rating()">Rating</button>
<button onclick="leaderboard()">Leaderboard</button>
<button onclick="stats()">Statistik</button>
<button onclick="chat()">Chat</button>
</div>`;
}

/* FEED */
window.feed=async()=>{
let snap=await getDocs(collection(db,"posts"));

let html=nav()+`
<div class="card">
<textarea id="txt" placeholder="Tulis..."></textarea>
<input id="img" placeholder="Link gambar">
<button onclick="post()">Post</button>
</div>
`;

for(let d of snap.docs){
let p=d.data();

html+=`
<div class="card">
<div class="small">${p.user}</div>
${p.text||""}
${p.image?`<img src="${p.image}">`:""}

<br>❤️ ${p.likes?.length||0}
<button onclick="like('${d.id}')">❤️</button>
<button onclick="comment('${d.id}')">💬</button>

<div id="c-${d.id}"></div>
</div>
`;

loadComments(d.id);
}

app.innerHTML=html;
};

/* POST */
window.post=async()=>{
if(!txt.value && !img.value)return;

await addDoc(collection(db,"posts"),{
user:user(),
text:txt.value,
image:img.value,
likes:[]
});
feed();
};

/* LIKE TOGGLE */
window.like=async(id)=>{
let ref=doc(db,"posts",id);
let snap=await getDoc(ref);
let data=snap.data();

let likes=data.likes||[];

if(likes.includes(user())){
likes=likes.filter(x=>x!==user());
}else{
likes.push(user());
}

await updateDoc(ref,{likes});
feed();
};

/* COMMENT */
window.comment=async(id)=>{
let t=prompt("Komentar...");
if(!t)return;

await addDoc(collection(db,"comments"),{
postId:id,
user:user(),
text:t
});
feed();
};

/* LOAD COMMENT */
async function loadComments(id){
let snap=await getDocs(collection(db,"comments"));
let html="";

snap.forEach(d=>{
let c=d.data();
if(c.postId===id){
html+=`<div class="small">💬 ${c.user}: ${c.text}</div>`;
}
});

setTimeout(()=>{
let el=document.getElementById("c-"+id);
if(el) el.innerHTML=html;
},200);
}

/* RATING */
window.rating=async()=>{
let snap=await getDocs(collection(db,"ratings"));
let c=[0,0,0,0,0];

snap.forEach(d=>c[d.data().rating-1]++);

let total=c.reduce((a,b)=>a+b,0);
let p=c.map(x=>total?((x/total)*100).toFixed(1):0);

app.innerHTML=nav()+`
<div class="card">
${c.map((v,i)=>`⭐ ${i+1}: ${v} (${p[i]}%)`).join("<br>")}
<canvas id="chart"></canvas>
</div>
`;

new Chart(chart,{type:"pie",data:{labels:["1","2","3","4","5"],datasets:[{data:c}]}})
};

/* LEADERBOARD */
window.leaderboard=async()=>{
let snap=await getDocs(collection(db,"posts"));
let map={};

snap.forEach(d=>{
let u=d.data().user;
map[u]=(map[u]||0)+1;
});

let arr=Object.entries(map).sort((a,b)=>b[1]-a[1]);

app.innerHTML=nav()+`
<div class="card">
${arr.map((a,i)=>`${i+1}. ${a[0]} (${a[1]})`).join("<br>")}
</div>
`;
};

/* STATS */
window.stats=async()=>{
let snap=await getDocs(collection(db,"followers"));
let f=0,u=0;

snap.forEach(d=>{
if(d.data().type==="follow")f++;
if(d.data().type==="unfollow")u++;
});

app.innerHTML=nav()+`
<div class="card">
Follow:${f}<br>
Unfollow:${u}
<canvas id="c"></canvas>
</div>
`;

new Chart(c,{type:"doughnut",data:{labels:["F","U"],datasets:[{data:[f,u]}]}})
};

/* CHAT */
window.chat=async()=>{
let snap=await getDocs(collection(db,"messages"));

let html=nav()+`
<div class="card">
<input id="msg">
<button onclick="send()">Kirim</button>
</div>
`;

snap.forEach(d=>{
let m=d.data();
html+=`<div class="card small">${m.user}: ${m.text}</div>`;
});

app.innerHTML=html;
};

window.send=async()=>{
if(!msg.value)return;
await addDoc(collection(db,"messages"),{
user:user(),
text:msg.value
});
chat();
};

feed();

</script>

</body>
</html>