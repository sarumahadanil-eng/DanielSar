<html lang="id">
<head>
<meta charset="UTF-8">
<title>Daniel Dolar Sarumaha | Ultra God-Level Cinematic</title>
<style>
body{margin:0;font-family:Arial,sans-serif;overflow-x:hidden;transition:0.5s;}
body.dark{background:#020617;color:white;}
body.light{background:#fefefe;color:#111;}
nav{background:rgba(15,23,42,0.9);padding:10px;display:flex;justify-content:center;gap:10px;position:sticky;top:0;z-index:999;}
button{padding:8px 14px;margin:4px;border:none;background:#3b82f6;color:white;border-radius:6px;cursor:pointer;transition:0.3s;} 
button:hover{transform:scale(1.05)}
input,textarea{padding:10px;margin:5px;border-radius:6px;border:none;width:220px;}
.page{display:none;padding:20px;}
#home{display:block;height:100vh;width:100vw;position:relative;}
.comment{background:rgba(30,41,59,0.8);margin:10px;padding:10px;border-radius:8px;position:relative;}
.like-btn{position:absolute;top:5px;right:5px;cursor:pointer;}
.profile-link{cursor:pointer;color:#3b82f6;text-decoration:underline;}
#canvas3d{height:100vh;width:100vw;display:block;position:absolute;top:0;left:0;z-index:-1;}
#toggleMode{position:fixed;top:10px;right:10px;padding:6px 12px;background:#f59e0b;color:#111;border-radius:6px;cursor:pointer;z-index:1000;}
.social-links{margin-top:10px;}
.social-links button{background:#14b8a6;}
.notification{position:fixed;top:50px;right:10px;padding:10px;background:#ef4444;color:white;border-radius:6px;display:none;z-index:1000;}
#map{height:200px;width:100%;margin-top:10px;}
#profilePhoto,#profilePhotoProfile{width:150px;height:150px;border-radius:50%;object-fit:cover;margin-top:10px;}
.popupProfile{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,0,0,0.9);padding:20px;border-radius:10px;color:white;display:none;z-index:10000;}
</style>
</head>
<body class="dark">

<!-- Auth -->
<div id="auth">
<h2>Login / Register</h2>
<input id="email" placeholder="Email"><br>
<input id="password" type="password" placeholder="Password"><br>
<input type="file" id="photo"><br>
<button onclick="register()">Register</button>
<button onclick="login()">Login</button>
</div>

<!-- Dashboard -->
<div id="dashboard" style="display:none">
<div id="toggleMode" onclick="toggleDarkLight()">🌌/☀️</div>
<nav>
<button onclick="showPage('home')">Home</button>
<button onclick="showPage('profile')">Data Diri</button>
<button onclick="showPage('notes')">Catatan</button>
<button onclick="showPage('rating')">Rating</button>
<button onclick="logout()">Logout</button>
</nav>

<!-- Home -->
<div id="home" class="page">
<div id="canvas3d"></div>
<img id="profilePhoto" src="" alt="Foto Profil Utama">
<h1 style="position:relative;z-index:1;text-align:center;margin-top:35vh;color:white;font-size:3em;text-shadow:0 0 15px #14b8a6;">Selamat Datang di Platform Daniel Dolar Sarumaha</h1>
</div>

<!-- Profil -->
<div id="profile" class="page">
<h2>Data Diri Daniel Dolar Sarumaha</h2>
<img id="profilePhotoProfile" src="" alt="Foto Profil Utama">
<p><b>Nama:</b> Daniel Dolar Sarumaha</p>
<p><b>Status:</b> Developer muda</p>
<p><b>Minat:</b> Programming, Web Development, Teknologi</p>
<p><b>Bahasa:</b> HTML, JavaScript, CSS</p>
<p><b>Tujuan:</b> Membuat platform dan aplikasi sendiri</p>
<p><b>Keahlian:</b> Frontend Development, Firebase Integration</p>
<p><b>Hobi:</b> Coding, Eksplorasi Teknologi, Membuat Website Unik</p>
<p><b>Visi:</b> Membuat sistem digital yang bermanfaat untuk banyak orang</p>
<div class="social-links">
  <button onclick="window.open('https://wa.me/6281388149795','_blank')">WhatsApp</button>
  <button onclick="window.open('https://instagram.com/Danieldolars','_blank')">Instagram</button>
  <button onclick="window.open('https://facebook.com/Daniel.Sarumaha','_blank')">Facebook</button>
</div>
<p>Followers: <span id="followerCount">0</span></p>
<button onclick="toggleFollow()">Follow/Unfollow</button>
</div>

<!-- Catatan -->
<div id="notes" class="page">
<h2>Catatan Pribadi</h2>
<textarea id="noteText" placeholder="Tulis catatan..."></textarea><br>
<button onclick="saveNote()">Simpan</button>
<div id="notesList"></div>
</div>

<!-- Rating & Komentar -->
<div id="rating" class="page">
<h2>Komentar & Rating Publik</h2>
<textarea id="commentText" placeholder="Tulis komentar..."></textarea><br>
<input id="ratingNumber" type="number" placeholder="Rating 1-5"><br>
<button onclick="sendComment()">Kirim</button>
<div id="comments"></div>
<p>Pengunjung: <span id="visitorCount">0</span></p>
<div id="map"></div>
</div>

<!-- Popup Profil Teman -->
<div class="popupProfile" id="popupProfile"></div>

<!-- Notifikasi -->
<div class="notification" id="notif">Komentar baru!</div>

<!-- Music -->
<audio id="bgMusic" src="https://www.bensound.com/bensound-music/bensound-epic.mp3" loop autoplay></audio>

<!-- Three.js & Leaflet -->
<script src="https://cdn.jsdelivr.net/npm/three@0.158.0/build/three.min.js"></script>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";
import { getFirestore, collection, addDoc, onSnapshot, query, where, getDocs, doc, updateDoc } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";
import { getStorage, ref, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-storage.js";

// FIREBASE CONFIG
const firebaseConfig = {
apiKey:"AIzaSyBj1j2odizaiO91XtdFaQVo6_k2G8ter7M",
authDomain:"daniel-website-f1f3b.firebaseapp.com",
projectId:"daniel-website-f1f3b",
storageBucket:"daniel-website-f1f3b.firebasestorage.app",
messagingSenderId:"113198911345",
appId:"1:113198911345:web:f9ecb06524b7e27bf470d0"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const storage = getStorage(app);

let visitorCounter=0;
let isFollowing=false;

// Upload & display profile photo
async function setProfilePhoto(user,file){
if(file){
const storageRef=ref(storage,"profile/"+user.uid);
await uploadBytes(storageRef,file);
const url=await getDownloadURL(storageRef);
document.getElementById("profilePhoto").src=url;
document.getElementById("profilePhotoProfile").src=url;
}else if(user.photoURL){
document.getElementById("profilePhoto").src=user.photoURL;
document.getElementById("profilePhotoProfile").src=user.photoURL;
}
}

// Register/Login
window.register=async function(){
const email=document.getElementById("email").value;
const password=document.getElementById("password").value;
const file=document.getElementById("photo").files[0];
const userCredential=await createUserWithEmailAndPassword(auth,email,password);
await setProfilePhoto(userCredential.user,file);
};

window.login=async function(){
const email=document.getElementById("email").value;
const password=document.getElementById("password").value;
const userCredential=await signInWithEmailAndPassword(auth,email,password);
await setProfilePhoto(userCredential.user,null);
};

window.logout=function(){signOut(auth);};

onAuthStateChanged(auth,(user)=>{
if(user){
document.getElementById("auth").style.display="none";
document.getElementById("dashboard").style.display="block";
checkFollow(user.uid);
}else{
document.getElementById("auth").style.display="block";
document.getElementById("dashboard").style.display="none";
}
});

window.showPage=function(page){
document.querySelectorAll(".page").forEach(p=>p.style.display="none");
document.getElementById(page).style.display="block";
};

// Followers
async function checkFollow(uid){
const q=query(collection(db,"followers"),where("followerId","==",uid));
const querySnap=await getDocs(q);
isFollowing=!querySnap.empty;
updateFollowerDisplay();
}

window.toggleFollow=async function(){
const user=auth.currentUser;
if(!user) return;
const uid=user.uid;
const q=query(collection(db,"followers"),where("followerId","==",uid));
const querySnap=await getDocs(q);
if(querySnap.empty){
await addDoc(collection(db,"followers"),{userId:"Daniel Dolar Sarumaha",followerId:uid});
isFollowing=true;
}else{
querySnap.forEach(async(docu)=>{await updateDoc(doc(db,"followers",docu.id),{followerId:uid});});
isFollowing=false;
}
updateFollowerDisplay();
};

function updateFollowerDisplay(){document.getElementById("followerCount").innerText=isFollowing?1:0;}

// Comments & Rating
window.sendComment=async function(){
const text=document.getElementById("commentText").value;
const rating=document.getElementById("ratingNumber").value;
const user=auth.currentUser;
if(!user) return;
await addDoc(collection(db,"comments"),{text,rating,time:Date.now(),likes:0,uid:user.uid,email:user.email});
};

onSnapshot(collection(db,"comments"),(snap)=>{
const list=document.getElementById("comments");
list.innerHTML="";
snap.forEach(docu=>{
const d=docu.data();
list.innerHTML+=`<div class="comment">
<p><span class="profile-link" onclick="viewProfile('${d.uid}')">${d.email}</span>: ${d.text}</p>⭐ ${d.rating} 
<button class="like-btn" onclick="likeComment('${docu.id}',${d.likes})">❤️ ${d.likes}</button>
</div>`;
visitorCounter++;
document.getElementById("visitorCount").innerText=visitorCounter;
});
if(snap.docChanges().some(change=>change.type==="added")) showNotification();
});

// Notification
function showNotification(){const notif=document.getElementById("notif");notif.style.display="block";setTimeout(()=>{notif.style.display="none"},3000);}

// Like
window.likeComment=async function(id,currentLikes){const docRef=doc(db,"comments",id);await updateDoc(docRef,{likes:currentLikes+1});};

// Notes
window.saveNote=async function(){const note=document.getElementById("noteText").value;await addDoc(collection(db,"notes"),{note,time:Date.now(),uid:auth.currentUser.uid});};

// Profile view popup
window.viewProfile=function(uid){const popup=document.getElementById("popupProfile");popup.innerHTML=`<h3>Profil UID: ${uid}</h3><p>Data & Foto Profil bisa ditambahkan</p><button onclick="closePopup()">Close</button>`;popup.style.display="block";};
window.closePopup=function(){document.getElementById("popupProfile").style.display="none";};

// 3D cinematic complex
const scene=new THREE.Scene();
const camera=new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight,0.1,1000);
const renderer=new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth,window.innerHeight);
document.getElementById("canvas3d").appendChild(renderer.domElement);

// Aurora lights
const torus=new THREE.TorusKnotGeometry(0.6,0.15,150,16);
const cube=new THREE.BoxGeometry(0.5,0.5,0.5);
const sphere=new THREE.SphereGeometry(0.3,32,32);
const material=new THREE.MeshStandardMaterial({color:0x14b8a6,metalness:0.7,roughness:0.3});
const meshTorus=new THREE.Mesh(torus,material);
const meshCube=new THREE.Mesh(cube,material);
const meshSphere=new THREE.Mesh(sphere,material);

scene.add(meshTorus,meshCube,meshSphere);

const light=new THREE.PointLight(0xffffff,1,100);
light.position.set(5,5,5);
scene.add(light);

meshCube.position.x=1.5; meshSphere.position.x=-1.5;
camera.position.z=4;

function animate(){requestAnimationFrame(animate);
meshTorus.rotation.x+=0.01; meshTorus.rotation.y+=0.01;
meshCube.rotation.x+=0.02; meshCube.rotation.y+=0.02;
meshSphere.rotation.x+=0.015; meshSphere.rotation.y+=0.01;
renderer.render(scene,camera);}
animate();

// Dark/Light Mode
window.toggleDarkLight=function(){document.body.classList.toggle("dark");document.body.classList.toggle("light");};

// Map lokasi komentar
const map=L.map('map').setView([0,0],1);
L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png',{maxZoom:19}).addTo(map);
</script>
</body>
</html>