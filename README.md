<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daniel Portfolio Premium</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
:root {
  --color-light: #f6f6f6;
  --color-dark: #1e1e2f;
  --aurora1: #ff9a9e;
  --aurora2: #fad0c4;
  --aurora3: #a18cd1;
}
body[data-theme="light"]{--bg:var(--color-light);--text:#1e1e2f;}
body[data-theme="dark"]{--bg:var(--color-dark);--text:#f6f6f6;}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg);color:var(--text);scroll-behavior:smooth;overflow-x:hidden;}
nav{display:flex;justify-content:space-between;align-items:center;padding:1rem;background:linear-gradient(90deg,var(--aurora1),var(--aurora2));}
nav a{color:#fff;margin:0 0.5rem;text-decoration:none;}
.toggle-theme{cursor:pointer;}
section{min-height:100vh;padding:3rem;position:relative;}
h1,h2,h3{margin:0 0 1rem 0;}
.reveal{opacity:0;transform:translateY(20px);transition:all 0.8s ease;}
.reveal.active{opacity:1;transform:translateY(0);}
.comment-section{max-width:600px;margin:2rem auto;background:rgba(255,255,255,0.1);padding:1rem;border-radius:10px;}
.comment{border-bottom:1px solid #ccc;padding:0.5rem 0;display:flex;gap:0.5rem;align-items:flex-start;}
.comment img{width:40px;height:40px;border-radius:50%;object-fit:cover;}
.comment-content{flex:1;}
.comment-actions{display:flex;gap:0.5rem;align-items:center;font-size:0.9rem;cursor:pointer;}
.stars{cursor:pointer;color:#ccc;}
.stars.clicked{color:gold;}
.visitor-counter{text-align:center;margin-top:1rem;}
/* Aurora animated background */
.aurora{position:fixed;top:0;left:0;width:200%;height:200%;background:linear-gradient(120deg,var(--aurora1),var(--aurora2),var(--aurora3));animation:auroraMove 20s linear infinite;z-index:-1;}
@keyframes auroraMove{0%{transform:translate(0,0);}50%{transform:translate(-50%,-50%);}100%{transform:translate(0,0);}}
</style>
</head>
<body data-theme="light">

<div class="aurora"></div>

<nav>
  <div class="nav-left">
    <a href="#home">Home</a><a href="#about">About</a><a href="#skills">Skills</a>
    <a href="#projects">Projects</a><a href="#contact">Contact</a>
  </div>
  <div class="nav-right">
    <span class="toggle-theme"><i class="fas fa-adjust"></i></span>
    <a href="https://instagram.com/daniel" target="_blank"><i class="fab fa-instagram"></i></a>
  </div>
</nav>

<section id="home" class="reveal">
  <h1>Hi, I'm Daniel</h1>
  <p>Welcome to my premium portfolio website.</p>
</section>

<section id="about" class="reveal">
  <h2>About Me</h2>
  <p>Swimming & badminton. Aspiring programmer/guru. Desa Hiliamaetaniha.</p>
</section>

<section id="skills" class="reveal">
  <h2>Skills</h2>
  <ul>
    <li>HTML / CSS / JS</li>
    <li>Firebase integration</li>
    <li>Web Animations & Scroll Reveal</li>
  </ul>
</section>

<section id="projects" class="reveal">
  <h2>Projects</h2>
  <ul>
    <li>Project 1</li>
    <li>Project 2</li>
    <li>Project 3</li>
  </ul>
</section>

<section id="contact" class="reveal">
  <h2>Contact Me</h2>
  <p>Email: sarumahadanil@gmail.com</p>
</section>

<section id="comments" class="reveal">
  <h2>Comments</h2>
  <div class="comment-section" id="commentBox"></div>
  <input type="file" id="profilePhoto">
  <input type="text" id="nameInput" placeholder="Your Name"><br>
  <textarea id="commentInput" placeholder="Your Comment"></textarea><br>
  <div class="stars" id="starRating">
    <i class="fas fa-star" data-value="1"></i>
    <i class="fas fa-star" data-value="2"></i>
    <i class="fas fa-star" data-value="3"></i>
    <i class="fas fa-star" data-value="4"></i>
    <i class="fas fa-star" data-value="5"></i>
  </div>
  <button id="sendComment">Send</button>
  <div class="visitor-counter" id="visitorCount"></div>
</section>

<audio id="bgMusic" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" autoplay loop></audio>

<script type="module">  
import { initializeApp } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-app.js";  
import { getDatabase, ref, push, set, onValue, remove } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-database.js";  
import { getStorage, ref as sRef, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-storage.js";  

const firebaseConfig = {  
  apiKey: "AIzaSyBj1j2odizaiO91XtdFaQVo6_k2G8ter7M",  
  authDomain: "daniel-website-f1f3b.firebaseapp.com",  
  projectId: "daniel-website-f1f3b",  
  storageBucket: "daniel-website-f1f3b.firebasestorage.app",  
  messagingSenderId: "113198911345",  
  appId: "1:113198911345:web:bb42832cf783654bf470d0",  
  measurementId: "G-L61ZRVHSZZ"  
};  

const app = initializeApp(firebaseConfig);
const db = getDatabase(app);
const storage = getStorage(app);

// Visitor counter
const visitorRef = ref(db,'visitors');
onValue(visitorRef, snap => document.getElementById('visitorCount').innerText=`Visitors: ${snap.val()||0}`);
push(visitorRef,1);

// Comment functionality
const commentBox = document.getElementById('commentBox');
const nameInput = document.getElementById('nameInput');
const commentInput = document.getElementById('commentInput');
const sendBtn = document.getElementById('sendComment');
const profilePhoto = document.getElementById('profilePhoto');
const starRating = document.getElementById('starRating');
let currentRating = 0;

// Stars
starRating.querySelectorAll('i').forEach(star=>{
  star.addEventListener('click',()=>{
    currentRating=star.dataset.value;
    starRating.querySelectorAll('i').forEach(s=>s.classList.remove('clicked'));
    for(let i=0;i<currentRating;i++) starRating.querySelectorAll('i')[i].classList.add('clicked');
  });
});

// Send comment
sendBtn.addEventListener('click',async()=>{
  const name=nameInput.value.trim();
  const comment=commentInput.value.trim();
  if(!name||!comment)return alert('Fill name and comment');
  let photoURL='';
  if(profilePhoto.files[0]){
    const storageReference=sRef(storage,'profiles/'+profilePhoto.files[0].name);
    await uploadBytes(storageReference,profilePhoto.files[0]);
    photoURL=await getDownloadURL(storageReference);
  }
  const newCommentRef=push(ref(db,'comments'));
  set(newCommentRef,{name,comment,photoURL,rating:currentRating});
  nameInput.value=''; commentInput.value=''; profilePhoto.value=''; currentRating=0;
  starRating.querySelectorAll('i').forEach(s=>s.classList.remove('clicked'));
});

// Load comments realtime
onValue(ref(db,'comments'),snap=>{
  commentBox.innerHTML='';
  snap.forEach(child=>{
    const c=child.val();
    const div=document.createElement('div');
    div.classList.add('comment');
    div.innerHTML=`
      <img src="${c.photoURL||'https://via.placeholder.com/40'}"/>
      <div class="comment-content">
        <strong>${c.name}</strong><br>
        <span>${c.comment}</span><br>
        <span>⭐ ${c.rating||0}</span>
      </div>
      <div class="comment-actions">
        <span onclick="remove(ref(db,'comments/${child.key}'))" style="color:red;cursor:pointer">Delete</span>
      </div>
    `;
    commentBox.appendChild(div);
  });
});

// Dark/Light toggle
document.querySelector('.toggle-theme').addEventListener('click',()=>{
  document.body.dataset.theme=document.body.dataset.theme==='light'?'dark':'light';
});

// Scroll reveal
const revealElements=document.querySelectorAll('.reveal');
window.addEventListener('scroll',()=>{
  revealElements.forEach(el=>{
    if(el.getBoundingClientRect().top<window.innerHeight-100) el.classList.add('active');
  });
});
</script>