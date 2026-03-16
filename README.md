<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Mini Social Platform</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body{margin:0;font-family:Arial;background:#0f172a;color:white;}
header{background:#38bdf8;padding:15px;text-align:center;font-size:22px;}
.container{max-width:900px;margin:auto;padding:20px;}
.card{background:#1e293b;padding:20px;border-radius:10px;margin-bottom:20px;}
textarea,input{width:100%;padding:8px;border-radius:6px;border:none;}
button{padding:8px 14px;border:none;border-radius:6px;background:#38bdf8;color:white;cursor:pointer;margin-top:6px;}
.post{background:#334155;padding:10px;border-radius:10px;margin-top:10px;}
.post img{width:100%;border-radius:8px;}
.menu{position:fixed;bottom:40px;right:40px;width:220px;height:220px;}
.center{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);background:#38bdf8;width:60px;height:60px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:28px;cursor:pointer;}
.item{position:absolute;width:45px;height:45px;background:#1e293b;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;opacity:0;transform:scale(0);transition:0.3s;}
.menu.active .item{opacity:1;transform:scale(1);}
.item:nth-child(2){top:-10px;left:90px;}
.item:nth-child(3){top:20px;left:170px;}
.item:nth-child(4){top:90px;left:200px;}
.item:nth-child(5){top:160px;left:140px;}
.item:nth-child(6){top:160px;left:40px;}
.item:nth-child(7){top:90px;left:-20px;}
.item:nth-child(8){top:20px;left:0px;}
.light{background:white;color:black;}
</style>
</head>
<body>
<header>Mini Social Platform</header>
<div class="container" id="app"></div>
<div class="menu" id="menu">
<div class="center" onclick="toggleMenu()">+</div>
<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="feed()">📷</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="leaderboard()">🏆</div>
<div class="item" onclick="theme()">🌓</div>
<div class="item" onclick="logout()">🚪</div>
<div class="item" onclick="profileList()">👤</div>
<div class="item" onclick="platformLinks()">🌐</div>
</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getFirestore, collection, addDoc, getDocs, updateDoc, doc, increment, query, orderBy, onSnapshot } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig={apiKey:"AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",authDomain:"danieldolar-9bca1.firebaseapp.com",projectId:"danieldolar-9bca1"};
const appFire=initializeApp(firebaseConfig);
const db=getFirestore(appFire);

function user(){ return localStorage.getItem("user") }
window.toggleMenu=function(){ menu.classList.toggle("active") }

function loginPage(){
app.innerHTML=`<div class="card"><h2>Login</h2><input id="name" placeholder="Username"><button onclick="login()">Masuk</button></div>`;
}

window.login=async function(){
let name=document.getElementById("name").value
if(!name.trim()){ alert("Masukkan username"); return; }
localStorage.setItem("user",name)
await addDoc(collection(db,"users"),{name:name})
location.reload()
}

window.logout=function(){ localStorage.removeItem("user"); location.reload() }

window.home=async function(){
let usersData=await getDocs(collection(db,"users"))
let followersData=await getDocs(collection(db,"followers"))
let totalUsers=usersData.size
let totalFollowers=followersData.size
let followed=false
let myId=""
followersData.forEach(d=>{ let f=d.data(); if(f.user==user()){ followed=true; myId=d.id } })
let unfollow=totalUsers-totalFollowers

let userList="<h3>Users Terdaftar</h3><ul>"
usersData.forEach(d=>{ userList+=`<li>${d.data().name}</li>` })
userList+="</ul>"

app.innerHTML=`
<div class="card">
<h2>Halo ${user()}</h2>
<h3>Followers Platform : ${totalFollowers}</h3>
<button onclick="toggleFollowPlatform('${myId}')">${followed?"Unfollow Platform":"Follow Platform"}</button>
<br><br>
<canvas id="followChart"></canvas>
${userList}
</div>
<div class="card">
<h3>Platform Kami</h3>
<a href="https://wa.me/6281234567890" target="_blank"><button>WhatsApp</button></a>
<br><br>
<a href="https://instagram.com/usernamekamu" target="_blank"><button>Instagram</button></a>
</div>`

setTimeout(()=>{
new Chart(followChart,{type:"pie",data:{labels:["Follow Platform","Belum Follow"],datasets:[{data:[totalFollowers,unfollow]}]}})
},200)
}

window.toggleFollowPlatform=async function(id){
if(id){ await updateDoc(doc(db,"followers",id),{user:"deleted"}) } 
else{ await addDoc(collection(db,"followers"),{user:user()}) }
home()
}

window.feed=function(){
let q=query(collection(db,"posts"),orderBy("time","desc"))
onSnapshot(q,(snap)=>{
let html=`<div class="card"><textarea id="text"></textarea><input id="img" placeholder="Link gambar"><button onclick="post()">Post</button></div>`
snap.forEach(d=>{
let p=d.data()
html+=`<div class="post"><b>${p.user}</b><p>${p.text}</p>${p.img?`<img src="${p.img}">`:""}<br>❤️ ${p.likes||0}<button onclick="like('${d.id}')">Like</button></div>`
})
app.innerHTML=html
})
}

window.post=async function(){ await addDoc(collection(db,"posts"),{user:user(),text:text.value,img:img.value,likes:0,time:Date.now()}) }

window.like=async function(id){ let liked=localStorage.getItem("like-"+id); let ref=doc(db,"posts",id); if(!liked){ await updateDoc(ref,{likes:increment(1)}); localStorage.setItem("like-"+id,true) } else{ await updateDoc(ref,{likes:increment(-1)}); localStorage.removeItem("like-"+id) } }

window.leaderboard=async function(){
let posts=await getDocs(collection(db,"posts"))
let count={}
posts.forEach(d=>{ let u=d.data().user; count[u]=(count[u]||0)+1 })
let arr=Object.entries(count).sort((a,b)=>b[1]-a[1])
let html=`<div class="card"><h2>Leaderboard</h2>`
arr.forEach(a=>{ html+=`<p>${a[0]} : ${a[1]} post</p>` })
html+=`</div>`
app.innerHTML=html
}

window.theme=function(){ document.body.classList.toggle("light") }

window.profileList=async function(){
let usersData=await getDocs(collection(db,"users"))
let html=`<div class="card"><h2>Daftar User</h2><ul>`
usersData.forEach(d=>{ html+=`<li>${d.data().name}</li>` })
html+="</ul></div>"
app.innerHTML=html
}

window.platformLinks=function(){
app.innerHTML=`<div class="card"><h2>Platform Kami</h2>
<a href="https://wa.me/6281234567890" target="_blank"><button>WhatsApp</button></a><br><br>
<a href="https://instagram.com/usernamekamu" target="_blank"><button>Instagram</button></a></div>`
}

if(user()){ home() } else{ loginPage() }

</script>
</body>
</html>