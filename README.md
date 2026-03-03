<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DanielSar | Personal Website</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Poppins',sans-serif;
}

body{
background:linear-gradient(135deg,#3a7bd5,#8e44ad);
color:white;
min-height:100vh;
}

header{
text-align:center;
padding:40px 20px;
font-size:30px;
font-weight:700;
}

nav{
display:flex;
justify-content:center;
gap:15px;
flex-wrap:wrap;
padding:15px;
}

nav button{
padding:10px 20px;
border:none;
border-radius:25px;
background:rgba(255,255,255,0.2);
color:white;
cursor:pointer;
transition:0.3s;
}

nav button:hover{
background:white;
color:#333;
transform:scale(1.1);
}

section{
display:none;
padding:30px 20px;
text-align:center;
animation:fade 0.5s ease-in-out;
}

section.active{
display:block;
}

@keyframes fade{
from{opacity:0; transform:translateY(10px);}
to{opacity:1; transform:translateY(0);}
}

.card{
background:rgba(255,255,255,0.15);
padding:25px;
border-radius:20px;
max-width:500px;
margin:20px auto;
backdrop-filter:blur(15px);
}

input,textarea{
width:100%;
padding:10px;
margin:8px 0;
border-radius:10px;
border:none;
}

button.submit{
background:white;
color:#3a7bd5;
border:none;
padding:10px 20px;
border-radius:20px;
cursor:pointer;
font-weight:600;
}

.comment-box{
background:white;
color:black;
padding:10px;
margin:8px 0;
border-radius:10px;
text-align:left;
}
</style>
</head>
<body>

<header>Daniel Dolar Sarumaha</header>

<nav>
<button onclick="showPage('home')">Home</button>
<button onclick="showPage('profil')">Profil</button>
<button onclick="showPage('portfolio')">Portfolio</button>
<button onclick="showPage('kontak')">Kontak</button>
</nav>

<section id="home" class="active">
<div class="card">
<h2>Welcome 👋</h2>
<p>Calon Programmer & Guru 🚀</p>
<p>Visitor: <b id="visitorCount">0</b></p>
</div>
</section>

<section id="profil">
<div class="card">
<h2>Profil Saya</h2>
<p>TTL: Hiliamaetaniha, 14-05-2006</p>
<p>Hobby: Swimming & Badminton</p>
<p>Email: sarumahadanil@gmail.com</p>
</div>
</section>

<section id="portfolio">
<div class="card">
<h2>Portfolio</h2>
<p>• Website Pribadi</p>
<p>• Project HTML CSS</p>
<p>• Belajar JavaScript</p>
</div>
</section>

<section id="kontak">
<div class="card">
<h2>Komentar</h2>

<input type="text" id="nama" placeholder="Nama">
<textarea id="pesan" placeholder="Tulis komentar..."></textarea>
<button class="submit" onclick="kirimKomentar()">Kirim</button>

<h3 style="margin-top:20px;">Daftar Komentar:</h3>
<div id="daftarKomentar"></div>
</div>
</section>

<script>

// Navigasi
function showPage(id){
document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));
document.getElementById(id).classList.add("active");
}

// Visitor Counter (Lokal)
let count = localStorage.getItem("visitorCount") || 0;
count++;
localStorage.setItem("visitorCount", count);
document.getElementById("visitorCount").innerText = count;

// Komentar Lokal
function kirimKomentar(){
let nama = document.getElementById("nama").value;
let pesan = document.getElementById("pesan").value;

if(nama && pesan){
let komentar = JSON.parse(localStorage.getItem("komentar")) || [];
komentar.push({nama:nama, pesan:pesan});
localStorage.setItem("komentar", JSON.stringify(komentar));
tampilkanKomentar();
document.getElementById("nama").value="";
document.getElementById("pesan").value="";
}
}

function tampilkanKomentar(){
let komentar = JSON.parse(localStorage.getItem("komentar")) || [];
let daftar = document.getElementById("daftarKomentar");
daftar.innerHTML="";
komentar.forEach(k=>{
let div=document.createElement("div");
div.className="comment-box";
div.innerHTML="<b>"+k.nama+"</b><br>"+k.pesan;
daftar.appendChild(div);
});
}

tampilkanKomentar();

</script>

</body>
</html>