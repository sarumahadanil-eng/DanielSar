<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DanielSar | Personal Website</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Poppins',sans-serif;
}

body{
min-height:100vh;
background:linear-gradient(135deg,#4e73df,#9b59b6);
color:white;
}

header{
text-align:center;
padding:40px 20px 20px;
font-size:32px;
font-weight:700;
letter-spacing:2px;
}

nav{
display:flex;
justify-content:center;
gap:15px;
padding:20px;
flex-wrap:wrap;
}

nav button{
padding:10px 20px;
border:none;
border-radius:30px;
background:rgba(255,255,255,0.2);
color:white;
cursor:pointer;
backdrop-filter:blur(10px);
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
backdrop-filter:blur(15px);
padding:30px;
border-radius:20px;
max-width:500px;
margin:20px auto;
box-shadow:0 10px 30px rgba(0,0,0,0.2);
}

input,textarea{
width:100%;
padding:10px;
margin:8px 0;
border-radius:10px;
border:none;
outline:none;
}

button.submit{
background:white;
color:#4e73df;
border:none;
padding:10px 20px;
border-radius:20px;
cursor:pointer;
font-weight:600;
margin-top:10px;
}

.comment-box{
background:white;
color:black;
padding:10px;
margin:8px 0;
border-radius:10px;
text-align:left;
}

footer{
text-align:center;
padding:20px;
margin-top:30px;
font-size:14px;
opacity:0.8;
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
<p>Saya DanielSar, calon Programmer & Guru masa depan 🚀</p>
<p>Visitor: <b id="visitorCount">0</b></p>
</div>
</section>

<section id="profil">
<div class="card">
<h2>Profil Saya</h2>
<p><b>TTL:</b> Hiliamaetaniha, 14-05-2006</p>
<p><b>Hobby:</b> Swimming & Badminton</p>
<p><b>Email:</b> sarumahadanil@gmail.com</p>
</div>
</section>

<section id="portfolio">
<div class="card">
<h2>Portfolio</h2>
<p>• Website Pribadi</p>
<p>• Project HTML CSS</p>
<p>• Belajar JavaScript & Firebase</p>
</div>
</section>

<section id="kontak">
<div class="card">
<h2>Komentar Online</h2>

<input type="text" id="nama" placeholder="Nama">
<textarea id="pesan" placeholder="Tulis komentar..."></textarea>
<button class="submit" onclick="kirimKomentar()">Kirim</button>

<h3 style="margin-top:20px;">Komentar:</h3>
<div id="daftarKomentar"></div>
</div>
</section>

<footer>
© 2026 DanielSar | Personal Aesthetic Website
</footer>

<script>

// ===== FIREBASE CONFIG =====
const firebaseConfig = {
  apiKey: "ISI_APIKEY",
  authDomain: "ISI_AUTHDOMAIN",
  databaseURL: "ISI_DATABASEURL",
  projectId: "ISI_PROJECTID",
  storageBucket: "ISI_BUCKET",
  messagingSenderId: "ISI_SENDERID",
  appId: "ISI_APPID"
};

firebase.initializeApp(firebaseConfig);
const db = firebase.database();

// NAVIGATION
function showPage(id){
document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));
document.getElementById(id).classList.add("active");
}

// VISITOR COUNTER
let visitorRef = db.ref("visitors");
visitorRef.transaction(function(current){
return (current || 0) + 1;
});
visitorRef.on("value",function(snapshot){
document.getElementById("visitorCount").innerText = snapshot.val();
});

// KOMENTAR ONLINE
function kirimKomentar(){
let nama=document.getElementById("nama").value;
let pesan=document.getElementById("pesan").value;

if(nama && pesan){
db.ref("komentar").push({
nama:nama,
pesan:pesan
});
document.getElementById("nama").value="";
document.getElementById("pesan").value="";
}
}

db.ref("komentar").on("child_added",function(snapshot){
let data=snapshot.val();
let div=document.createElement("div");
div.className="comment-box";
div.innerHTML="<b>"+data.nama+"</b><br>"+data.pesan;
document.getElementById("daftarKomentar").appendChild(div);
});
</script>

</body>
</html>