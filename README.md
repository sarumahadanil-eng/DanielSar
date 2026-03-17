<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Mini Social Platform Daniel</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body{
margin:0;
font-family:sans-serif;
background:linear-gradient(135deg,#0f172a,#1e293b);
color:white;
}

/* HEADER */
header{
padding:15px;
text-align:center;
font-size:22px;
background:rgba(255,255,255,0.1);
backdrop-filter:blur(10px);
}

/* CONTAINER */
.container{
max-width:900px;
margin:auto;
padding:20px;
}

/* CARD GLASS */
.card{
background:rgba(255,255,255,0.05);
backdrop-filter:blur(12px);
padding:20px;
border-radius:15px;
margin-bottom:25px;
box-shadow:0 8px 20px rgba(0,0,0,0.3);
transition:0.3s;
}

.card:hover{
transform:translateY(-3px);
}

/* BUTTON */
button{
padding:10px;
border:none;
border-radius:8px;
background:#38bdf8;
color:white;
cursor:pointer;
margin-top:10px;
width:100%;
transition:0.2s;
}

button:hover{
background:#0ea5e9;
}

/* SELECT */
select{
width:100%;
padding:10px;
border-radius:8px;
border:none;
margin-top:10px;
}

/* MENU BULAT */
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
background:#22c55e;
width:65px;
height:65px;
border-radius:50%;
display:flex;
align-items:center;
justify-content:center;
font-size:30px;
cursor:pointer;
box-shadow:0 5px 15px rgba(0,0,0,0.3);
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

/* POSISI */
.item:nth-child(2){top:-10px;left:90px;}
.item:nth-child(3){top:20px;left:170px;}
.item:nth-child(4){top:90px;left:200px;}
.item:nth-child(5){top:160px;left:140px;}
.item:nth-child(6){top:160px;left:40px;}
.item:nth-child(7){top:90px;left:-20px;}
.item:nth-child(8){top:20px;left:0px;}
</style>
</head>

<body>

<header>Mini Social Platform Daniel</header>
<div class="container" id="app"></div>

<div class="menu" id="menu">
<div class="center" onclick="toggleMenu()">+</div>
<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="leaderboard()">🏆</div>
<div class="item" onclick="theme()">🌓</div>
<div class="item" onclick="logout()">🚪</div>
<div class="item" onclick="platformLinks()">🌐</div>
<div class="item" onclick="home()">🔄</div>
</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getFirestore, collection, addDoc, getDocs, query, where, updateDoc, doc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

/* 🔥 CONFIG FIREBASE (WAJIB ISI) */
const firebaseConfig={
apiKey:"ISI_APIKEY",
authDomain:"ISI_DOMAIN",
projectId:"ISI_PROJECTID"
};

const appFire=initializeApp(firebaseConfig);
const db=getFirestore(appFire);

/* USER */
function user(){ return localStorage.getItem("user") }

/* LOGIN */
function login(){
let name=document.getElementById("name").value
if(!name){alert("Isi dulu");return}
localStorage.setItem("user",name)
location.reload()
}

function logout(){
localStorage.removeItem("user")
location.reload()
}

/* MENU */
function toggleMenu(){
menu.classList.toggle("active")
}

/* HOME */
async function home(){
let snap=await getDocs(collection(db,"followers"))
let total=snap.size

let html=`
<div class="card">
<h2>Halo ${user()}</h2>
<h3>Followers Platform : ${total}</h3>
<button onclick="follow()">Follow Platform</button>
<canvas id="chart"></canvas>
</div>

<div class="card">
<h3>Platform Daniel</h3>
<a href="https://wa.me/6281388149795" target="_blank"><button>WhatsApp</button></a>
<a href="https://instagram.com/Danieldolars" target="_blank"><button>Instagram</button></a>
</div>
`

app.innerHTML=html

new Chart(chart,{
type:"pie",
data:{
labels:["Follow","Belum"],
datasets:[{data:[total,20-total]}]
}
})
}

/* FOLLOW ANTI DOUBLE */
async function follow(){
let q=query(collection(db,"followers"),where("user","==",user()))
let snap=await getDocs(q)

if(snap.empty){
await addDoc(collection(db,"followers"),{user:user()})
alert("Berhasil follow!")
}else{
alert("Kamu sudah follow!")
}

home()
}

/* RATING */
async function rating(){
app.innerHTML=`
<div class="card">
<h2>Rating Platform</h2>
<select id="rate">
<option value="1">⭐</option>
<option value="2">⭐⭐</option>
<option value="3">⭐⭐⭐</option>
<option value="4">⭐⭐⭐⭐</option>
<option value="5">⭐⭐⭐⭐⭐</option>
</select>
<button onclick="kirimRating()">Kirim / Update</button>
</div>

<div class="card">
<h3>Grafik Rating</h3>
<canvas id="ratingChart"></canvas>
</div>
`

loadChart()
}

/* RATING ANTI SPAM */
async function kirimRating(){
let r=Number(rate.value)
let q=query(collection(db,"ratings"),where("user","==",user()))
let snap=await getDocs(q)

if(snap.empty){
await addDoc(collection(db,"ratings"),{user:user(),rating:r})
}else{
let id=snap.docs[0].id
await updateDoc(doc(db,"ratings",id),{rating:r})
}

alert("Rating tersimpan!")
}

/* CHART REAL */
function loadChart(){
onSnapshot(collection(db,"ratings"),(snap)=>{
let count=[0,0,0,0,0]

snap.forEach(d=>{
count[d.data().rating-1]++
})

new Chart(ratingChart,{
type:"bar",
data:{
labels:["1⭐","2⭐","3⭐","4⭐","5⭐"],
datasets:[{data:count}]
}
})
})
}

/* LEADERBOARD */
async function leaderboard(){
let snap=await getDocs(collection(db,"ratings"))
let data={}

snap.forEach(d=>{
let u=d.data().user
let r=d.data().rating
if(!data[u]) data[u]=[]
data[u].push(r)
})

let result=Object.keys(data).map(u=>{
let avg=data[u].reduce((a,b)=>a+b)/data[u].length
return {user:u,avg:avg.toFixed(1)}
})

result.sort((a,b)=>b.avg-a.avg)

let html=`<div class="card"><h2>Leaderboard</h2>`
result.forEach(r=>{
html+=`<p>${r.user} : ${r.avg} ⭐</p>`
})
html+=`</div>`

app.innerHTML=html
}

/* PLATFORM */
function platformLinks(){
app.innerHTML=`
<div class="card">
<h2>Platform Daniel</h2>
<a href="https://wa.me/6281388149795" target="_blank"><button>WhatsApp</button></a>
<a href="https://instagram.com/Danieldolars" target="_blank"><button>Instagram</button></a>
</div>
`
}

/* THEME */
function theme(){
document.body.classList.toggle("light")
}

/* INIT */
if(user()){
home()
}else{
app.innerHTML=`
<div class="card">
<h2>Login</h2>
<input id="name" placeholder="Username">
<button onclick="login()">Masuk</button>
</div>
`
}
</script>

</body>
</html>