<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Mini Social Platform Daniel</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body{
  margin:0;
  font-family:Arial;
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

/* FIX UTAMA DI SINI */
.card{
  background:#1e293b;
  padding:20px;
  border-radius:12px;
  margin-bottom:30px; /* jarak antar card */
}

/* SPACING DALAM CARD */
.card h3, .card h2{
  margin-bottom:15px;
}

.card ul{
  margin-top:15px;
  margin-bottom:15px;
  padding-left:20px;
}

.card li{
  margin-bottom:8px;
}

.card a{
  display:block;
  margin-bottom:15px; /* JARAK WA & IG */
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
}

/* ITEM */
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

/* AKTIF */
.menu.active .item{
  opacity:1;
  transform:scale(1);
}

/* 8 POSISI (RAPI MELINGKAR) */
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

<header>Mini Social Platform Daniel</header>
<div class="container" id="app"></div>

<!-- MENU 8 OPSI -->
<div class="menu" id="menu">
<div class="center" onclick="toggleMenu()">+</div>
<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="alert('Feed')">📷</div>
<div class="item" onclick="alert('Rating')">⭐</div>
<div class="item" onclick="alert('Leaderboard')">🏆</div>
<div class="item" onclick="theme()">🌓</div>
<div class="item" onclick="logout()">🚪</div>
<div class="item" onclick="alert('User')">👤</div>
<div class="item" onclick="alert('Platform')">🌐</div>
</div>

<script>
function user(){
  return localStorage.getItem("user")
}

function toggleMenu(){
  document.getElementById("menu").classList.toggle("active")
}

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

function home(){

let users=["Daniel","Andi","Budi","Rina"]

let html=`

<div class="card">
<h2>Halo ${user()}</h2>
<h3>Followers Platform : 10</h3>
<button>Follow Platform</button>

<br><br>
<canvas id="chart"></canvas>
</div>

<!-- USERS (SUDAH DIPISAH) -->
<div class="card">
<h3>Users Terdaftar</h3>
<ul>
${users.map(u=>`<li>${u}</li>`).join("")}
</ul>
</div>

<!-- PLATFORM (SUDAH JAUH & RAPI) -->
<div class="card">
<h3>Platform Daniel</h3>

<a href="https://wa.me/6281388149795" target="_blank">
<button>WhatsApp</button>
</a>

<a href="https://instagram.com/Danieldolars" target="_blank">
<button>Instagram</button>
</a>

</div>
`

document.getElementById("app").innerHTML=html

setTimeout(()=>{
new Chart(document.getElementById("chart"),{
type:"pie",
data:{
labels:["Follow","Belum"],
datasets:[{data:[10,5]}]
}
})
},200)
}

function theme(){
document.body.classList.toggle("light")
}

if(user()){
home()
}else{
document.getElementById("app").innerHTML=`
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