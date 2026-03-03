
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DanielSar | Premium Aurora</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-storage.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif;}

:root{
--bg:linear-gradient(270deg,#4e73df,#8e44ad,#ff6ec4);
--card:rgba(255,255,255,0.15);
--text:white;
}

body.dark{
--bg:linear-gradient(270deg,#0f2027,#203a43,#2c5364);
--card:rgba(0,0,0,0.3);
--text:white;
}

body{
background:var(--bg);
background-size:400% 400%;
animation:aurora 15s ease infinite;
color:var(--text);
min-height:100vh;
transition:0.5s;
}

@keyframes aurora{
0%{background-position:0% 50%;}
50%{background-position:100% 50%;}
100%{background-position:0% 50%;}
}

.loading{
position:fixed;
width:100%;
height:100%;
background:black;
display:flex;
align-items:center;
justify-content:center;
color:white;
z-index:999;
}

header{text-align:center;padding:30px;}
button{
padding:8px 16px;
border:none;
border-radius:20px;
cursor:pointer;
margin:5px;
}

.card{
background:var(--card);
backdrop-filter:blur(15px);
padding:25px;
border-radius:20px;
max-width:700px;
margin:20px auto;
box-shadow:0 10px 30px rgba(0,0,0,0.3);
}

input,textarea{width:100%;padding:8px;margin:6px 0;border-radius:8px;border:none;}

.comment{
display:flex;
gap:12px;
background:var(--card);
padding:15px;
border-radius:15px;
margin:10px 0;
}

.avatar{
width:55px;
height:55px;
border-radius:50%;
object-fit:cover;
}

.star{font-size:20px;color:#ccc;cursor:pointer;}
.star.active{color:gold;}

.counter{margin-top:10px;font-weight:600;}

.music-btn{position:fixed;bottom:20px;right:20px;border-radius:50%;width:55px;height:55px;}

</style>
</head>
<body>

<div class="loading" id="loading">Loading Premium...</div>

<header>
<h1>Daniel Dolar Sarumaha</h1>
<p>Premium Aurora Portfolio 🚀</p>
<button onclick="toggleMode()">Dark/Light</button>
<button onclick="login()">Login Google</button>
<button onclick="logout()">Logout</button>
<p>Total Visitors: <span id="visitor">0</span></p>
</header>

<div class="card">
<h2>Komentar Premium</h2>
<input type="text" id="pesan" placeholder="Tulis komentar...">
<input type="file" id="foto">

<div>
<span class="star" onclick="setRating(1)">★</span>
<span class="star" onclick="setRating(2)">★</span>
<span class="star" onclick="setRating(3)">★</span>
<span class="star" onclick="setRating(4)">★</span>
<span class="star" onclick="setRating(5)">★</span>
</div>

<button onclick="kirim()">Kirim</button>

<p class="counter">Total Komentar: <span id="total">0</span></p>

<div id="list"></div>
</div>

<button class="music-btn" onclick="toggleMusic()">🎵</button>

<audio id="bgmusic" loop>
<source src="https://cdn.pixabay.com/audio/2022/03/15/audio_5c1f2c.mp3">
</audio>

<script>
// ===== GANTI DENGAN CONFIG FIREBASE KAMU =====
const firebaseConfig={
apiKey:"ISI_APIKEY",
authDomain:"ISI_AUTHDOMAIN",
databaseURL:"ISI_DATABASEURL",
projectId:"ISI_PROJECTID",
storageBucket:"ISI_BUCKET",
messagingSenderId:"ISI_SENDERID",
appId:"ISI_APPID"
};

firebase.initializeApp(firebaseConfig);
const auth=firebase.auth();
const db=firebase.database();
const storage=firebase.storage();

window.onload=()=>setTimeout(()=>document.getElementById("loading").style.display="none",1500);

function toggleMode(){document.body.classList.toggle("dark");}

function login(){
const provider=new firebase.auth.GoogleAuthProvider();
auth.signInWithPopup(provider);
}

function logout(){auth.signOut();}

let rating=0;
function setRating(r){
rating=r;
document.querySelectorAll(".star").forEach((s,i)=>s.classList.toggle("active",i<r));
}

function kirim(){
if(!auth.currentUser)return alert("Login dulu!");
let pesan=document.getElementById("pesan").value;
let file=document.getElementById("foto").files[0];
if(!pesan)return alert("Isi pesan!");

if(file){
let ref=storage.ref("foto/"+Date.now());
ref.put(file).then(()=>ref.getDownloadURL())
.then(url=>saveComment(pesan,url));
}else{
saveComment(pesan,"https://via.placeholder.com/55");
}
}

function saveComment(pesan,foto){
db.ref("komentar").push({
uid:auth.currentUser.uid,
nama:auth.currentUser.displayName,
fotoUser:auth.currentUser.photoURL,
pesan:pesan,
foto:foto,
rating:rating,
like:0,
timestamp:Date.now()
});
document.getElementById("pesan").value="";
}

db.ref("komentar").on("value",snap=>{
document.getElementById("list").innerHTML="";
let total=0;
snap.forEach(child=>{
let d=child.val();
let key=child.key;
total++;
let stars="";
for(let i=0;i<5;i++)stars+=`<span style="color:${i<d.rating?'gold':'#ccc'}">★</span>`;
document.getElementById("list").innerHTML+=`
<div class="comment">
<img class="avatar" src="${d.fotoUser}">
<div>
<b>${d.nama}</b><br>
${d.pesan}<br>
<img src="${d.foto}" width="100"><br>
${stars}<br>
❤️ ${d.like}
${auth.currentUser && auth.currentUser.uid===d.uid ? `<button onclick="hapus('${key}')">Hapus</button>`:''}
</div>
</div>`;
});
document.getElementById("total").innerText=total;
});

function hapus(key){db.ref("komentar/"+key).remove();}

db.ref("visitors").transaction(c=>(c||0)+1);
db.ref("visitors").on("value",s=>document.getElementById("visitor").innerText=s.val());

function toggleMusic(){
let music=document.getElementById("bgmusic");
music.paused?music.play():music.pause();
}
</script>

</body>
</html>