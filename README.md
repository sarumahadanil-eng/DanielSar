<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DanielSar | Dolar Portfolio</title>

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
background:linear-gradient(120deg,#4facfe,#00f2fe,#a18cd1,#fbc2eb);
background-size:400% 400%;
animation:aurora 15s ease infinite;
color:white;
transition:0.4s;
}

@keyframes aurora{
0%{background-position:0% 50%;}
50%{background-position:100% 50%;}
100%{background-position:0% 50%;}
}

.dark{
background:#111 !important;
}

#loader{
position:fixed;
width:100%;
height:100%;
background:black;
display:flex;
justify-content:center;
align-items:center;
z-index:9999;
color:white;
font-size:22px;
}

header{
text-align:center;
padding:40px 20px;
}

.profile-img{
width:140px;
height:140px;
border-radius:50%;
object-fit:cover;
border:4px solid white;
box-shadow:0 10px 25px rgba(0,0,0,0.3);
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
border-radius:30px;
background:rgba(255,255,255,0.25);
color:white;
cursor:pointer;
transition:0.3s;
}

nav button:hover{
background:white;
color:#333;
transform:scale(1.1);
}

.toggle{
position:fixed;
top:15px;
right:15px;
cursor:pointer;
background:white;
color:black;
padding:8px 15px;
border-radius:20px;
font-size:14px;
}

section{
display:none;
padding:30px 20px;
text-align:center;
animation:fade 0.5s ease;
}

section.active{
display:block;
}

@keyframes fade{
from{opacity:0; transform:translateY(15px);}
to{opacity:1; transform:translateY(0);}
}

.card{
background:rgba(255,255,255,0.2);
padding:25px;
border-radius:20px;
max-width:600px;
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
color:#333;
border:none;
padding:10px 20px;
border-radius:20px;
cursor:pointer;
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
opacity:0.8;
}
</style>
</head>
<body>

<div id="loader">Loading DanielSar Website...</div>

<div class="toggle" onclick="toggleMode()">Dark/Light</div>

<header>
<img src="profil.jpg" class="profile-img">
<h1>Daniel Dolar Sarumaha</h1>
<p>Future Programmer👨‍💻 and teacher🧑‍🏫</p>
<p>Visitors: <b id="visitorCount">0</b></p>
<p>Total Comments: <b id="commentCount">0</b></p>
<a href="https://instagram.com/Danieldolars" target="_blank" style="color:white;text-decoration:underline;">Instagram</a>
</header>

<nav>
<button onclick="showPage('home')">Home</button>
<button onclick="showPage('profil')">Profile</button>
<button onclick="showPage('portfolio')">Portfolio</button>
<button onclick="showPage('kontak')">contact</button>
</nav>

<section id="home" class="active">
<div class="card">
<h2>Welcome 👋
my name is Daniel Dolar Sarumaha</h2>
<p>Selamat datang di website pribadi aesthetic saya.</p>
</div>
</section>

<section id="profil">profil.jpg</section>
<div class="card">
<h2>Profile</h2>
<p>TTL: Hiliamaetaniha, 14-05-2006</p>
<p>Hobby: Swimming & Badminton</p>
<p>Email: sarumahadanil@gmail.com</p>
</div>
</section>

<section id="portfolio">
<div class="card">
<h2>Portfolio</h2>
<p>• Website Daniel Designer</p>
<p>• Firebase Comment System</p>
<p>• UI Animation Project</p>
</div>
</section>

<section id="kontak">
<div class="card">
<h2>Komentar Online</h2>
<input type="text" id="nama" placeholder="Nama">
<textarea id="pesan" placeholder="Tulis komentar..."></textarea>
<button class="submit" onclick="kirim Komentar()">send</button>
<div id="daftar Komentar pengunjung"></div>
</div>
</section>

<footer>
© 2026 DanielSar | Daniel Edition
</footer>

<script>

// LOADER
window.onload=function(){
document.getElementById("loader").style.display="block";
}

// DARK MODE
function toggleMode(){
document.body.classList.toggle("bluedark");
}

// NAVIGATION
function showPage(id){
document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));
document.getElementById(id).classList.add("active");
}

// FIREBASE CONFIG (GANTI INI)
const firebaseConfig={
apiKey:"Daniel Dolar Sarumaha",
authDomain:"danielsar-website-firebaseapp.com",
databaseURL:" danielsar-website-default-rtdb.asia-southeast1.firebasedatabase.com", 
projectId:"danielsar-website",
storageBucket:"danielsar-website-appspot.com",
messagingSenderId:"14052006",
appId:"1:14052006:web:danielsar"
};

firebase.initializeApp(firebaseConfig);
const db=firebase.database();

// VISITOR
let visitorRef=db.ref("visitors");
visitorRef.transaction(c=>(c||0)+1);
visitorRef.on("value",snap=>{
document.getElementById("visitorCount").innerText=snap.val();
});

// KOMENTAR
function kirimKomentar(){
let nama=document.getElementById("Name").value;
let pesan=document.getElementById("Comment").value;
if(Name&&Comment){
db.ref("Comment").push({Name,Comment});
document.getElementById("Name").value="";
document.getElementById("Comment").value="";
}
}

let count=0;
db.ref("comment").on("child_added",snap=>{
let data=snap.val();
count++;
document.getElementById("commentCount").innerText=count;
let div=document.createElement("div");
div.className="comment-time";
div.innerHTML="<b>"+data.nama+"</b><br>"+data.pesan;
document.getElementById("daftarKomentar").appendChild(div);
});
</script>

</body>
</html>