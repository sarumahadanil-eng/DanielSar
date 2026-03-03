<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daniel Dolar Sarumaha</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Segoe UI',sans-serif;
scroll-behavior:smooth;
}

body{
background:linear-gradient(135deg,#4e73df,#1cc88a);
color:white;
text-align:center;
}

/* NAVBAR */
nav{
position:fixed;
width:100%;
background:rgba(0,0,0,0.3);
padding:15px;
backdrop-filter:blur(10px);
z-index:1000;
}

nav a{
color:white;
text-decoration:none;
margin:0 15px;
font-weight:bold;
transition:0.3s;
}

nav a:hover{
color:yellow;
}

/* SECTION */
section{
padding:120px 20px;
}

/* PROFILE */
.profile img{
width:180px;
height:180px;
border-radius:50%;
object-fit:cover;
border:5px solid white;
box-shadow:0 10px 30px rgba(0,0,0,0.4);
}

/* CARD */
.card{
background:rgba(255,255,255,0.15);
backdrop-filter:blur(15px);
padding:25px;
margin:20px auto;
max-width:700px;
border-radius:15px;
}

/* BUTTON */
.btn{
padding:10px 20px;
margin:10px;
border:none;
border-radius:30px;
background:white;
color:#333;
cursor:pointer;
transition:0.3s;
}

.btn:hover{
background:yellow;
}

textarea{
width:100%;
padding:10px;
margin-top:10px;
border-radius:10px;
border:none;
resize:none;
}

.stars span{
font-size:30px;
cursor:pointer;
}

.review-box{
background:rgba(255,255,255,0.2);
padding:10px;
margin:10px 0;
border-radius:10px;
}
</style>
</head>

<body>

<nav>
<a href="#home">Home</a>
<a href="#about">Tentang</a>
<a href="#contact">Kontak</a>
<a href="#rating">Rating</a>
</nav>

<!-- HOME -->
<section id="home" class="profile">
<img src="daniel.jpg" alt="Foto Daniel">
<h1>Daniel Dolar Sarumaha</h1>
<p>Mahasiswa UNIRAYA | Web Developer Pemula</p>
<p>Total Pengunjung: <span id="visitorCount">0</span></p>
</section>

<!-- ABOUT -->
<section id="about">
<div class="card">
<h2>Tentang Saya</h2>
<p>Halo, saya Daniel Dolar Sarumaha. 
Saya sedang belajar menjadi web developer dan ingin terus berkembang di dunia pemrograman.</p>
</div>
</section>

<!-- CONTACT -->
<section id="contact">
<div class="card">
<h2>Hubungi Saya</h2>
<a href="https://instagram.com/Danieldolars" target="_blank" class="btn">Instagram</a>
<a href="https://wa.me/081388149795" target="_blank" class="btn">WhatsApp</a>
</div>
</section>

<!-- RATING -->
<section id="rating">
<div class="card">
<h2>Beri Rating & Komentar</h2>

<div class="stars">
<span onclick="setRating(1)">⭐</span>
<span onclick="setRating(2)">⭐</span>
<span onclick="setRating(3)">⭐</span>
<span onclick="setRating(4)">⭐</span>
<span onclick="setRating(5)">⭐</span>
</div>

<p id="ratingText"></p>

<textarea id="commentInput" rows="3" placeholder="Tulis komentar..."></textarea>
<button class="btn" onclick="submitReview()">Kirim</button>

<h3>Rata-rata Rating: <span id="average">0</span> ⭐</h3>

<div id="reviewList"></div>
</div>
</section>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { 
getFirestore, collection, addDoc, getDocs,
doc, updateDoc, getDoc, setDoc 
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

/* 🔴 GANTI DENGAN CONFIG FIREBASE KAMU */
const firebaseConfig = {
apiKey: "ISI_API_KEY",
authDomain: "ISI_AUTH_DOMAIN",
projectId: "ISI_PROJECT_ID",
storageBucket: "ISI_STORAGE_BUCKET",
messagingSenderId: "ISI_SENDER_ID",
appId: "ISI_APP_ID"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

let selectedRating = 0;

window.setRating = function(star){
selectedRating = star;
document.getElementById("ratingText").innerText =
"Rating dipilih: " + star + " ⭐";
}

window.submitReview = async function(){

let comment = document.getElementById("commentInput").value;

if(selectedRating === 0 || comment.trim() === ""){
alert("Isi rating & komentar dulu!");
return;
}

await addDoc(collection(db,"reviews"),{
rating:selectedRating,
comment:comment,
date:new Date()
});

document.getElementById("commentInput").value="";
selectedRating=0;
loadReviews();
}

async function loadReviews(){

let querySnapshot = await getDocs(collection(db,"reviews"));
let reviewList = document.getElementById("reviewList");
let total = 0;
let count = 0;

reviewList.innerHTML="";

querySnapshot.forEach((doc)=>{
let r = doc.data();
total += r.rating;
count++;

reviewList.innerHTML += `
<div class="review-box">
<strong>${r.rating} ⭐</strong>
<p>${r.comment}</p>
</div>`;
});

let average = count ? (total/count).toFixed(1) : 0;
document.getElementById("average").innerText = average;
}

async function updateVisitorCount(){
const counterRef = doc(db,"stats","visitors");
const counterSnap = await getDoc(counterRef);

if(!localStorage.getItem("visited")){
if(counterSnap.exists()){
await updateDoc(counterRef,{
count: counterSnap.data().count + 1
});
}else{
await setDoc(counterRef,{count:1});
}
localStorage.setItem("visited",true);
}

const updatedSnap = await getDoc(counterRef);
document.getElementById("visitorCount").innerText =
updatedSnap.data().count;
}

loadReviews();
updateVisitorCount();

</script>

</body>
</html>