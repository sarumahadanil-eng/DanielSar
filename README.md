<!DOCTYPE html>
<html>
<head>

<meta charset="UTF-8">
<title>Daniel Galaxy Notes</title>

<style>

body{
margin:0;
font-family:sans-serif;
background:black;
color:white;
overflow-x:hidden;
}

#space{
position:fixed;
top:0;
left:0;
width:100%;
height:100%;
z-index:-1;
}

#login{
height:100vh;
display:flex;
flex-direction:column;
align-items:center;
justify-content:center;
}

#home{
display:none;
padding:20px;
}

.card{
background:rgba(255,255,255,0.1);
backdrop-filter:blur(10px);
border-radius:15px;
padding:20px;
margin-bottom:20px;
}

input,button{
padding:10px;
margin:5px;
border:none;
border-radius:10px;
}

button{
background:cyan;
cursor:pointer;
}

#notes li{
margin:10px 0;
}

.dark{
background:#020617;
}

</style>

</head>

<body>

<canvas id="space"></canvas>

<div id="login">

<h1>Daniel Galaxy Notes</h1>

<button onclick="googleLogin()">Login with Google</button>

</div>

<div id="home">

<div class="card">

<h2>Tambah Catatan</h2>

<input id="noteInput" placeholder="Tulis catatan">

<button onclick="voiceNote()">🎤</button>

<button onclick="addNote()">Simpan</button>

</div>

<div class="card">

<input id="search" placeholder="Cari catatan">

<ul id="notes"></ul>

</div>

<div class="card">

Visitor: <span id="visitor">0</span>

<button onclick="toggleMode()">Dark Mode</button>

</div>

</div>

<script src="https://cdn.jsdelivr.net/npm/three@0.158.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3"></script>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";

import { getFirestore,
collection,
addDoc,
onSnapshot,
doc,
deleteDoc,
updateDoc
}

from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

import {
getAuth,
GoogleAuthProvider,
signInWithPopup
}

from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";

/* FIREBASE CONFIG */

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

const auth = getAuth();

const provider = new GoogleAuthProvider();

/* LOGIN GOOGLE */

window.googleLogin = async function(){

let result = await signInWithPopup(auth,provider);

let user = result.user;

document.getElementById("login").style.display="none";

document.getElementById("home").style.display="block";

}

/* ADD NOTE */

window.addNote = async function(){

let text = document.getElementById("noteInput").value;

if(!text) return;

await addDoc(collection(db,"notes"),{

text:text,
time:Date.now()

});

}

/* REALTIME NOTES */

onSnapshot(collection(db,"notes"),snap=>{

let list=document.getElementById("notes");

list.innerHTML="";

snap.forEach(d=>{

let li=document.createElement("li");

li.innerHTML=`
${d.data().text}

<button onclick="editNote('${d.id}')">Edit</button>
<button onclick="deleteNote('${d.id}')">Delete</button>
`;

list.appendChild(li);

});

});

/* EDIT */

window.editNote=async function(id){

let text=prompt("Edit catatan");

if(!text)return;

await updateDoc(doc(db,"notes",id),{

text:text

});

}

/* DELETE */

window.deleteNote=async function(id){

await deleteDoc(doc(db,"notes",id));

}

/* VISITOR */

addDoc(collection(db,"visitors"),{
time:Date.now()
})

onSnapshot(collection(db,"visitors"),snap=>{
document.getElementById("visitor").innerText=snap.size
})

</script>

<script>

/* THREE JS PLANET */

const scene = new THREE.Scene();

const camera = new THREE.PerspectiveCamera(
75,
window.innerWidth/window.innerHeight,
0.1,
1000
);

const renderer = new THREE.WebGLRenderer({
canvas:document.getElementById("space"),
alpha:true
});

renderer.setSize(window.innerWidth,window.innerHeight);

const geometry = new THREE.SphereGeometry(3,64,64);

const material = new THREE.MeshStandardMaterial({
color:0x00ffff,
wireframe:true
});

const planet = new THREE.Mesh(geometry,material);

scene.add(planet);

const light = new THREE.PointLight(0xffffff,2);
light.position.set(10,10,10);

scene.add(light);

camera.position.z=8;

function animate(){

requestAnimationFrame(animate);

planet.rotation.y+=0.01;

renderer.render(scene,camera);

}

animate();

/* VOICE NOTE */

const recognition = new webkitSpeechRecognition();

recognition.lang="id-ID";

function voiceNote(){

recognition.start();

}

recognition.onresult=(event)=>{

let text=event.results[0][0].transcript;

document.getElementById("noteInput").value=text;

}

/* SEARCH */

document.addEventListener("input",e=>{

if(e.target.id==="search"){

let v=e.target.value.toLowerCase();

document.querySelectorAll("#notes li").forEach(li=>{

li.style.display=
li.innerText.toLowerCase().includes(v)
?"block":"none";

})

}

});

/* DARK MODE */

function toggleMode(){
document.body.classList.toggle("dark")
}

/* PAGE ANIMATION */

gsap.from("body",{
opacity:0,
y:30,
duration:1
});

</script>

</body>
</html>