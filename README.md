<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Daniel Dolar Sarumaha - Realtime Ultra Cinematic SPA</title>
<style>
body, html{margin:0;padding:0;height:100%;font-family:'Segoe UI',sans-serif;background:#0e0e20;color:white;overflow:hidden;}
*{box-sizing:border-box;}
.container{display:flex;height:100vh;transition:0.5s;}
.sidebar{width:220px;background:#11112a;padding:20px;display:flex;flex-direction:column;transition:width 0.4s;}
.sidebar h3{margin-top:0;margin-bottom:20px;color:#fffb;}
.sidebar div{margin:12px 0;cursor:pointer;padding:6px 8px;border-radius:6px;transition:0.2s;}
.sidebar div:hover{background:#3a3a5c;color:#fff;font-weight:600;}
.main{flex:1;overflow-y:auto;padding:20px;background:linear-gradient(135deg,#1b1b2f,#0e0e20);transition:0.5s;}
.page{opacity:0;transform:translateY(20px);transition:0.4s;display:none;}
.page.active{display:block;opacity:1;transform:translateY(0);}
.card{background:rgba(255,255,255,0.05);backdrop-filter:blur(6px);padding:18px;margin-bottom:20px;border-radius:14px;box-shadow:0 4px 20px rgba(0,0,0,0.4);transition:0.3s transform;}
.card:hover{transform: translateY(-4px) scale(1.01);box-shadow:0 8px 30px rgba(0,0,0,0.5);}
textarea,input[type="text"]{width:100%;padding:10px;border-radius:8px;border:none;background:rgba(255,255,255,0.1);color:white;margin-bottom:10px;}
button{padding:6px 12px;border:none;border-radius:6px;background:#4f46e5;color:white;cursor:pointer;margin-right:6px;transition:0.2s;}
button:hover{background:#3b36c1;transform:scale(1.05);}
.rating span,.reaction span{cursor:pointer;font-size:18px;margin-right:6px;transition:0.2s;}
.rating span:hover,.reaction span:hover{transform:scale(1.3);}
.comment{margin-top:10px;font-size:14px;}
.reply{margin-left:20px;font-size:13px;color:#aaa;}
.tag{color:#3498db;cursor:pointer;margin-right:6px;font-size:13px;}
#profilePhoto{border-radius:50%;width:80px;height:80px;margin-right:16px;box-shadow:0 4px 12px rgba(0,0,0,0.3);}
.social a{margin-right:10px;color:#4f46e5;text-decoration:none;font-weight:bold;}
.social a:hover{text-decoration:underline;}
.card-3d{transform-style:preserve-3d;transition: transform 0.5s;}
.card-3d:hover{transform:rotateY(15deg) rotateX(5deg) scale(1.02);}
</style>
<!-- Firebase SDK -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-app.js";
  import { getFirestore, collection, addDoc, doc, onSnapshot, updateDoc, getDoc } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-firestore.js";

  const firebaseConfig = {
    apiKey: "AIzaSyBj1j2odizaiO91XtdFaQVo6_k2G8ter7M",
    authDomain: "daniel-website-f1f3b.firebaseapp.com",
    projectId: "daniel-website-f1f3b",
    storageBucket: "daniel-website-f1f3b.firebasestorage.app",
    messagingSenderId: "113198911345",
    appId: "1:113198911345:web:f9ecb06524b7e27bf470d0",
    measurementId: "G-5KTV80PG1J"
  };

  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);
</script>
</head>
<body>
<div class="container">
<div class="sidebar">
<h3>Platform</h3>
<div onclick="showPage('home')">🏠 Home</div>
<div onclick="showPage('profile')">👤 Profile</div>
</div>
<div class="main">

<!-- HOME -->
<div id="home" class="page active">
  <h2>Home</h2>
  <textarea id="postText" placeholder="Write something..."></textarea><br>
  <button onclick="createPost()">Post</button>
  <div id="feed"></div>
</div>

<!-- PROFILE -->
<div id="profile" class="page">
  <h2>Profile</h2>
  <div style="display:flex;align-items:center;margin-bottom:20px;">
    <img id="profilePhoto" src="https://via.placeholder.com/80">
    <div>
      <h3 id="profileName">Daniel Dolar Sarumaha</h3>
      <p id="profileEmail" style="color:#ccc;">albumxiimia@gmail.com</p>
      <p id="profileBio" style="font-size:14px;color:#aaa;">Portfolio ultra cinematic, multi-functional SPA</p>
      <div class="social">
        <a href="https://wa.me/6281388149795" target="_blank">WhatsApp</a>
        <a href="https://instagram.com/Danieldolars" target="_blank">Instagram</a>
        <a href="https://facebook.com/Daniel Sarumaha" target="_blank">Facebook</a>
      </div>
    </div>
  </div>
  <input type="file" id="uploadPhoto" onchange="changePhoto(event)">
  <p>Catatan Pribadi:</p>
  <textarea id="personalNote"></textarea><br>
  <button onclick="saveNote()">Simpan Catatan</button><br><br>
  <span id="followBtn" onclick="toggleFollow()" style="cursor:pointer;background:#4f46e5;padding:6px 12px;border-radius:6px;">Follow</span>
  <div style="display:flex;gap:16px;margin-top:20px;">
    <div class="card">Posts<br><span id="statPosts">0</span></div>
    <div class="card">Comments<br><span id="statComments">0</span></div>
    <div class="card">Reactions<br><span id="statReactions">0</span></div>
  </div>
</div>

</div>
</div>

<script type="module">
let user={name:"Daniel Dolar Sarumaha",email:"albumxiimia@gmail.com",bio:"Portfolio ultra cinematic, multi-functional SPA",photo:"https://via.placeholder.com/80",followed:false,note:""};

function showPage(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
  document.getElementById(id).classList.add("active");
  render(); // update stats & inputs
}

// Profile
function changePhoto(e){let file=e.target.files[0];if(file){let reader=new FileReader();reader.onload=function(){document.getElementById("profilePhoto").src=reader.result;user.photo=reader.result;} reader.readAsDataURL(file);}}
function saveNote(){user.note=document.getElementById("personalNote").value; alert("Catatan tersimpan!");}
function toggleFollow(){user.followed=!user.followed; document.getElementById("followBtn").innerText=user.followed?"Unfollow":"Follow";}

// Realtime posts
const postsRef = collection(db,"posts");
function createPost(){
  let text=document.getElementById("postText").value;
  if(!text) return;
  addDoc(postsRef,{text,votes:0,reactions:{like:0,fire:0,love:0},comments:[],timestamp:Date.now()});
  document.getElementById("postText").value="";
}

// Listen realtime feed tanpa overwrite page lain
onSnapshot(postsRef,(snapshot)=>{
  const feed = document.getElementById("feed");
  if(!feed) return;
  feed.innerHTML="";
  snapshot.docs.sort((a,b)=>b.data().timestamp-a.data().timestamp).forEach(docSnap=>{
    const p = docSnap.data();
    const card = document.createElement("div");
    card.className = "card card-3d";
    card.innerHTML = `
      <div>${p.text}</div>
      <div>
        <button onclick="vote('${docSnap.id}',1)">⬆ ${p.votes}</button>
        <button onclick="vote('${docSnap.id}',-1)">⬇</button>
      </div>
      <div class="reaction">
        <span onclick="react('${docSnap.id}','like')">👍 ${p.reactions.like}</span>
        <span onclick="react('${docSnap.id}','fire')">🔥 ${p.reactions.fire}</span>
        <span onclick="react('${docSnap.id}','love')">❤️ ${p.reactions.love}</span>
      </div>
    `;
    feed.appendChild(card);
  });
});

// Vote & React
async function vote(id,val){const p=doc(postsRef,id); const snap=await getDoc(p); const data=snap.data(); await updateDoc(p,{votes:data.votes+val});}
async function react(id,type){const p=doc(postsRef,id); const snap=await getDoc(p); const data=snap.data(); await updateDoc(p,{reactions:{...data.reactions,[type]:data.reactions[type]+1}});}

// Render profile stats
function render(){
  document.getElementById("profileName").innerText=user.name;
  document.getElementById("profileEmail").innerText=user.email;
  document.getElementById("profileBio").innerText=user.bio;
  document.getElementById("profilePhoto").src=user.photo;
  document.getElementById("personalNote").value=user.note;
  // stats from Firestore
  postsRef.get && postsRef.get().then(snapshot=>{
    const posts = snapshot.docs.map(d=>d.data());
    document.getElementById("statPosts").innerText = posts.length;
    document.getElementById("statComments").innerText = posts.reduce((sum,p)=>sum+p.comments.length,0);
    document.getElementById("statReactions").innerText = posts.reduce((sum,p)=>sum+p.reactions.like+p.reactions.fire+p.reactions.love,0);
  });
}

render();
</script>
</body>
</html>