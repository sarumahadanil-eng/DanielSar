<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DanielSar Website</title>

<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>

<style>
body{
font-family:Arial;
margin:0;
background:linear-gradient(135deg,#667eea,#764ba2);
color:white;
text-align:center;
}

nav{
background:rgba(0,0,0,0.3);
padding:15px;
}

nav button{
padding:10px 15px;
margin:5px;
border:none;
border-radius:20px;
cursor:pointer;
}

section{
display:none;
padding:20px;
}

section.active{
display:block;
}

.card{
background:rgba(255,255,255,0.2);
padding:20px;
border-radius:15px;
max-width:500px;
margin:auto;
}

input,textarea{
width:90%;
padding:10px;
margin:5px;
border-radius:10px;
border:none;
}

.comment-box{
background:white;
color:black;
padding:10px;
margin:5px;
border-radius:10px;
}
</style>
</head>
<body>

<h1>DanielSar</h1>

<nav>
<button onclick="showPage('home')">Home</button>
<button onclick="showPage('tentang')">Tentang</button>
<button onclick="showPage('kontak')">Kontak</button>
<button onclick="showPage('rating')">Rating</button>
</nav>

<!-- HOME -->
<section id="home" class="active">
<div class="card">
<h2>Selamat Datang</h2>
<p>Pengunjung ke: <b id="visitorCount">0</b></p>
</div>
</section>

<!-- TENTANG -->
<section id="tentang">
<div class="card">
<h2>Tentang Saya</h2>
<p>Nama: Daniel Dolar Sarumaha</p>
<p>Cita-cita: Programmer</p>
</div>
</section>

<!-- KONTAK -->
<section id="kontak">
<div class="card">
<h2>Komentar Online</h2>
<input type="text" id="nama" placeholder="Nama">
<textarea id="pesan" placeholder="Tulis komentar..."></textarea>
<button onclick="kirimKomentar()">Kirim</button>

<h3>Komentar:</h3>
<div id="daftarKomentar"></div>
</div>
</section>

<!-- RATING -->
<section id="rating">
<div class="card">
<h2>Beri Rating</h2>
<p>Klik bintang:</p>
<span onclick="rate(1)">⭐</span>
<span onclick="rate(2)">⭐</span>
<span onclick="rate(3)">⭐</span>
<span onclick="rate(4)">⭐</span>
<span onclick="rate(5)">⭐</span>
<p id="hasilRating"></p>
</div>
</section>

<script>

// ================= FIREBASE CONFIG =================
// GANTI DENGAN CONFIG MILIK KAMU
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

// ================= NAVIGATION =================
function showPage(id){
document.querySelectorAll("section").forEach(s=>s.classList.remove("active"));
document.getElementById(id).classList.add("active");
}

// ================= VISITOR COUNTER =================
let visitorRef = db.ref("visitors");
visitorRef.transaction(function(current){
return (current || 0) + 1;
});

visitorRef.on("value",function(snapshot){
document.getElementById("visitorCount").innerText = snapshot.val();
});

// ================= KOMENTAR ONLINE =================
function kirimKomentar(){
let nama = document.getElementById("nama").value;
let pesan = document.getElementById("pesan").value;

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
let data = snapshot.val();
let div = document.createElement("div");
div.className="comment-box";
div.innerHTML="<b>"+data.nama+"</b><br>"+data.pesan;
document.getElementById("daftarKomentar").appendChild(div);
});

// ================= RATING =================
function rate(n){
document.getElementById("hasilRating").innerText =
"Terima kasih memberi rating "+n+" ⭐";
}

</script>

</body>
</html>