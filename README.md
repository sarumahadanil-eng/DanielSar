<!DOCTYPE html>
<html>
<head>
<title>Home - Daniel</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<nav>
<a href="index.html">Home</a>
<a href="tentang.html">Tentang</a>
<a href="kontak.html">Kontak</a>
<a href="rating.html">Rating</a>
</nav>

<div class="container">
<div class="card">
<h1>Daniel Dolar Sarumaha</h1>
<p>Selamat datang di website pribadi saya.</p>
<p>Total Pengunjung: <span id="visitorCount">0</span></p>
</div>
</div>

<script type="module" src="script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html>
<head>
<title>Tentang - Daniel</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<nav>
<a href="index.html">Home</a>
<a href="tentang.html">Tentang</a>
<a href="kontak.html">Kontak</a>
<a href="rating.html">Rating</a>
</nav>

<div class="container">
<div class="card">
<h2>Tentang Saya</h2>
<p>Nama: Daniel Dolar Sarumaha</p>
<p>Lahir: Hiliamaetaniha, 14-05-2006</p>
<p>Hobby: Swimming & Badminton</p>
<p>Cita-cita: Programmer / Guru</p>
</div>
</div>

</body>
</html>
<!DOCTYPE html>
<html>
<head>
<title>Kontak - Daniel</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<nav>
<a href="index.html">Home</a>
<a href="tentang.html">Tentang</a>
<a href="kontak.html">Kontak</a>
<a href="rating.html">Rating</a>
</nav>

<div class="container">
<div class="card">
<h2>Hubungi Saya</h2>
<a href="https://instagram.com/Danieldolars" class="btn">Instagram</a>
<br><br>
<a href="https://wa.me/081388149795" class="btn">WhatsApp</a>
</div>
</div>

</body>
</html>
<!DOCTYPE html>
<html>
<head>
<title>Rating - Daniel</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<nav>
<a href="index.html">Home</a>
<a href="tentang.html">Tentang</a>
<a href="kontak.html">Kontak</a>
<a href="rating.html">Rating</a>
</nav>

<div class="container">
<div class="card">
<h2>Beri Rating & Komentar</h2>

<select id="ratingSelect">
<option value="">Pilih Rating</option>
<option value="1">1 ⭐</option>
<option value="2">2 ⭐</option>
<option value="3">3 ⭐</option>
<option value="4">4 ⭐</option>
<option value="5">5 ⭐</option>
</select>

<textarea id="commentInput" rows="3" placeholder="Tulis komentar..."></textarea>
<button class="btn" onclick="submitReview()">Kirim</button>

<h3>Rata-rata Rating: <span id="average">0</span> ⭐</h3>

<div id="reviewList"></div>

</div>
</div>

<script type="module" src="script.js"></script>
</body>
</html>
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { 
getFirestore, collection, addDoc, getDocs,
doc, updateDoc, getDoc, setDoc 
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

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

/* REVIEW */
window.submitReview = async function(){
let rating = document.getElementById("ratingSelect")?.value;
let comment = document.getElementById("commentInput")?.value;

if(!rating || !comment){
alert("Isi dulu rating & komentar!");
return;
}

await addDoc(collection(db,"reviews"),{
rating:Number(rating),
comment:comment,
date:new Date()
});

document.getElementById("commentInput").value="";
loadReviews();
}

async function loadReviews(){
let reviewList = document.getElementById("reviewList");
if(!reviewList) return;

let snapshot = await getDocs(collection(db,"reviews"));
let total=0, count=0;

reviewList.innerHTML="";

snapshot.forEach(doc=>{
let r = doc.data();
total+=r.rating;
count++;

reviewList.innerHTML+=`
<div class="review-box">
<strong>${r.rating} ⭐</strong>
<p>${r.comment}</p>
</div>`;
});

document.getElementById("average").innerText =
count ? (total/count).toFixed(1) : 0;
}

/* VISITOR */
async function updateVisitor(){
let visitorEl = document.getElementById("visitorCount");
if(!visitorEl) return;

const ref = doc(db,"stats","visitors");
const snap = await getDoc(ref);

if(!localStorage.getItem("visited")){
if(snap.exists()){
await updateDoc(ref,{count:snap.data().count+1});
}else{
await setDoc(ref,{count:1});
}
localStorage.setItem("visited",true);
}

const updated = await getDoc(ref);
visitorEl.innerText = updated.data().count;
}

loadReviews();
updateVisitor();
