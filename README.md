<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>💎 Daniel Noctra Cyber</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body{
margin:0;
font-family:sans-serif;
color:white;
display:flex;
justify-content:center;
background:linear-gradient(120deg,#0ea5e9,#8b5cf6,#22c55e);
background-size:300% 300%;
animation:aurora 10s infinite ease;
}
@keyframes aurora{
0%{background-position:0% 50%;}
50%{background-position:100% 50%;}
100%{background-position:0% 50%;}
}

/* BACKGROUND LOGIN (ANIMASI) */
body.login-bg{
  background:linear-gradient(120deg,#0ea5e9,#8b5cf6,#22c55e,#06b6d4);
  background-size:400% 400%;
  animation:aurora 8s ease-in-out infinite;

  height:100vh;
  display:flex;
  justify-content:center;
  align-items:center;
}

/* CONTAINER */
.container{
  max-width:420px;
  width:100%;
}

/* HEADER */
.header{
  text-align:center;
  padding:20px;
  font-size:22px;
  font-weight:bold;
  color:#0ff;
  text-shadow:0 0 10px #0ff;
}

/* =========================
   CARD GLOBAL (HALAMAN)
========================= */
.card{
  margin:10px;
  padding:15px;
  border-radius:20px;
}

/* =========================
   LOGIN CARD (HITAM)
========================= */
body.login-bg .card{
  background:linear-gradient(145deg,#000,#1a1a1a);
  color:#fff;
  border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 25px rgba(0,0,0,0.8);
}

/* =========================
   HALAMAN UTAMA (BLUE)
========================= */
body:not(.login-bg) .card{
  background:linear-gradient(135deg,#a1c4fd,#c2e9fb);
  color:#111;
  box-shadow:0 10px 30px rgba(0,0,0,0.15);
}

/* BUTTON */
button{
  width:100%;
  padding:8px;
  margin:6px 0;
  border:none;
  border-radius:20px;
  background:linear-gradient(90deg,#38bdf8,#a78bfa);
  color:white;
  cursor:pointer;
}
#touchEffect{
  position:fixed;
  top:0;
  left:0;
  width:100%;
  height:100%;
  pointer-events:none;
  z-index:9999;
}

/* INPUT */
input, textarea{
  width:100%;
  padding:8px;
  margin-top:8px;
  border-radius:10px;
  border:1px solid #444;

  background:#111;
  color:#fff;
}

input::placeholder{
  color:#aaa;
}

/* POST / CHAT / USER */
.post{
  background:#eef2ff;
  border-left:4px solid #6366f1;
}

.chat{
  background:#f5f3ff;
  border-left:4px solid #8b5cf6;
}

.user{
  background:#e0f2fe;
  border-left:4px solid #3b82f6;
}

/* ANIMASI BACKGROUND */
@keyframes aurora{
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}
.login-card{
  background:#000;
  color:#fff;
  border:1px solid rgba(255,255,255,0.1);
  box-shadow:0 0 25px rgba(0,0,0,0.8);
}
.welcome-text{
  color:#000;
  font-size:14px;
  line-height:1.6;
  text-align:center;
}
</style>

</head>

<body>

<div class="container" id="app"></div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import {
getFirestore,collection,addDoc,getDocs,
updateDoc,doc,setDoc,deleteDoc,
query,orderBy,onSnapshot,arrayUnion
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const db = getFirestore(initializeApp({
apiKey:"AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
authDomain:"danieldolar-9bca1.firebaseapp.com",
projectId:"danieldolar-9bca1"
}));

const app=document.getElementById("app");

/* USER */
function getUser(){
return JSON.parse(localStorage.getItem("user"));
}

async function setUser(name){
let user={
id:"u"+Math.random().toString(36).substr(2,5),
name:name
};
localStorage.setItem("user",JSON.stringify(user));
await setDoc(doc(db,"users",user.id),user);
}

/* LOGIN */
function loginPage(){
  app.innerHTML = `
  <div class="card login-card" style="text-align:center">

    <h2 style="color:#0ff">⚡ LOGIN ⚡</h2>

    <!-- VIDEO -->
    <video id="vid" controls playsinline
      style="width:180px;border-radius:15px;margin:15px 0;background:black;">
      <source src="https://www.w3schools.com/html/mov_bbb.mp4" type="video/mp4">
    </video>

    <p style="font-size:13px;color:#000;margin-bottom:10px;">
      Halo 👋 selamat datang di web saya 🌐, jangan lupa tonton video singkat ini 🎬✨
    </p>

    <input id="username" placeholder="Masukkan username...">
    <button onclick="login()">Login</button>

  </div>
  `;

  const vid = document.getElementById("vid");

  // 🔊 Auto play saat user klik pertama (biar gak diblok)
  vid.addEventListener("play", ()=>{
    vid.muted = false;
  });

  // 🎬 setelah video pertama selesai → pindah ke tontonan naga
  vid.onended = () => {
    vid.src = "https://media.w3.org/2010/05/sintel/trailer.mp4";
    vid.currentTime = 0;
    vid.play();

    // ⏱️ batasi hanya 20 detik
    vid.ontimeupdate = () => {
      if (vid.currentTime >= 20) {
        vid.pause();
      }
    };
  };
}


/* LOGIN ACTION */
window.login = async () => {
  const input = document.getElementById("username");
  const name = input.value.trim();

  if (!name) {
    alert("Isi username!");
    return;
  }

  await setUser(name);
  document.body.className = "";
  home();
};
/* NAV */
function nav(){
let u=getUser();
if(!u){loginPage();return "";}
return `
<div class="header">⚡ ${u.name} ⚡</div>

<div class="card">
<button onclick="home()">🏠 Home</button>
<button onclick="feed()">📰 Feed</button>
<button onclick="profile()">👤 Profile</button>
<button onclick="chat()">💬 Messenger</button>
<button onclick="rating()">⭐ Rating</button>
<button onclick="users()">👥 Users</button>
<button onclick="logout()">🚪 Logout</button>
</div>`;
}

/* HOME */
window.home=async()=>{
let f=0,u=0;
let snap=await getDocs(collection(db,"followers"));
snap.forEach(d=>d.data().type==="follow"?f++:u++);

app.innerHTML=nav()+`
<div class="card">
<h3>👥 Followers: ${f-u}</h3>
<button onclick="follow()">➕ Follow</button>
<button onclick="unfollow()">➖ Unfollow</button>
<canvas id="chart"></canvas>
</div>`;

new Chart(chart,{
type:"doughnut",
data:{labels:["Follow","Unfollow"],datasets:[{data:[f,u]}]}
});
};

window.follow=()=>addDoc(collection(db,"followers"),{type:"follow"}).then(home);
window.unfollow=()=>addDoc(collection(db,"followers"),{type:"unfollow"}).then(home);

/* FEED */
window.feed=async()=>{
let html=nav()+`
<div class="card">
<textarea id="txt"></textarea>
<button onclick="post()">Posting</button>
</div>`;

let postSnap=await getDocs(collection(db,"posts"));
let komenSnap=await getDocs(collection(db,"comments"));

postSnap.forEach(d=>{
let p=d.data();
let komenHTML="";

komenSnap.forEach(k=>{
let c=k.data();
if(c.postId===d.id){
komenHTML+=`
<div>💬 <b>${c.user}</b>: ${c.text}
<button onclick="hapusKomen('${k.id}')">❌</button>
</div>`;
}
});

html+=`
<div class="post">
<b>${p.user}</b><br>${p.text}
<br>❤️ ${p.likes?.length||0}

<button onclick="like('${d.id}')">Like</button>
<button onclick="hapusPost('${d.id}')">🗑 Hapus</button>

<input id="c${d.id}">
<button onclick="kirimKomentar('${d.id}')">💬</button>

${komenHTML}
</div>`;
});

app.innerHTML=html;
};

/* PROFILE (FULL) */
window.profile=()=>{
app.innerHTML=nav()+`
<div class="card">
<h2>👤 Daniel Dolar Sarumaha</h2>

<p>
<b>Nama:</b><br>
Daniel Dolar Sarumaha adalah seorang individu muda yang memiliki semangat tinggi dalam belajar dan berkembang, terutama di bidang teknologi digital.<br><br>

<b>Umur:</b><br>
19 tahun — usia produktif dimana ia sedang membangun masa depan dan mengasah kemampuan untuk menjadi pribadi sukses.<br><br>

<b>Hobby:</b><br>
Memiliki ketertarikan besar pada dunia coding, desain web, dan teknologi modern.<br><br>

<b>Cita-cita:</b><br>
Menjadi developer sukses dan membangun platform besar.<br><br>

<b>Motto:</b><br>
"Terus belajar dan berkembang."
</p>

</div>`;
};

/* MESSENGER */
window.chat = ()=>{

let u = getUser();
if(!u){alert("Login dulu"); return;}

app.innerHTML = nav() + `
<div class="card">
<h2>💬 Messenger</h2>

<div id="chatBox" style="height:250px;overflow:auto;"></div>

<input id="pesan" placeholder="Ketik pesan...">
<button onclick="kirimChat()">Kirim</button>
</div>
`;

let q = query(collection(db,"chat"), orderBy("time"));

onSnapshot(q,(snap)=>{
let box = document.getElementById("chatBox");
let isi = "";

snap.forEach(d=>{
let c = d.data();

if(c.hiddenFor && c.hiddenFor.includes(u.id)) return;

isi += `
<div>
<b>${c.user}</b>: ${c.deleted ? "Pesan dihapus" : c.text}
<br>
<button onclick="hapusSaya('${d.id}')">❌</button>
<button onclick="hapusSemua('${d.id}')">🗑</button>
</div>
`;
});

box.innerHTML = isi;
});
};

window.kirimChat=async()=>{
let u=getUser();
let teks=pesan.value.trim();
if(!teks)return;

await addDoc(collection(db,"chat"),{
user:u.name,
text:teks,
time:Date.now(),
hiddenFor:[],
deleted:false
});

pesan.value="";
};

window.hapusSaya=async(id)=>{
let u=getUser();
await updateDoc(doc(db,"chat",id),{
hiddenFor:arrayUnion(u.id)
});
};

window.hapusSemua = async(id)=>{
  if(confirm("Hapus untuk semua?")){
    await deleteDoc(doc(db,"chat",id));
  }
}

/* RATING */
window.rating=async()=>{

let total=0,count=0;
let stars=[0,0,0,0,0];

let snap=await getDocs(collection(db,"ratings"));

snap.forEach(d=>{
let v=d.data().value;
if(v>=1 && v<=5){
total+=v;
count++;
stars[v-1]++;
}
});

let avg = count ? (total/count).toFixed(1) : 0;

let persen = stars.map(s =>
(count && s) ? Math.round((s/count)*100) : 0
);

app.innerHTML=nav()+`
<div class="card">
<h2>rating</h2>

<h3>${avg} ⭐ (${count} vote)</h3>

<div style="font-size:25px;cursor:pointer">
<span onclick="kirimRating(1)">☆</span>
<span onclick="kirimRating(2)">☆</span>
<span onclick="kirimRating(3)">☆</span>
<span onclick="kirimRating(4)">☆</span>
<span onclick="kirimRating(5)">☆</span>
</div>

<div style="margin-top:10px">
1⭐: ${persen[0]}%<br>
2⭐: ${persen[1]}%<br>
3⭐: ${persen[2]}%<br>
4⭐: ${persen[3]}%<br>
5⭐: ${persen[4]}%
</div>

</div>`;
};
window.kirimRating=async(v)=>{
await addDoc(collection(db,"ratings"),{
value:v
});
rating();
}
/* USERS */
window.users=async()=>{
let html=nav()+`<div class="card"><h2>Users</h2>`;

let snap=await getDocs(collection(db,"users"));

snap.forEach(d=>{
html+=`
<div class="user">
${d.data().name}
<button onclick="hapusUser('${d.id}')">🗑 Hapus</button>
</div>
`;
});

html+=`</div>`;
app.innerHTML=html;
};
window.hapusUser=async(id)=>{
if(confirm("Yakin hapus user?")){
await deleteDoc(doc(db,"users",id));
users();
}
}
/* LOGOUT */
window.logout=()=>{
localStorage.clear();
loginPage();
};

loginPage();
</script>

</body>
</html>