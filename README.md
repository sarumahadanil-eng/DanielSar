<!DOCTYPE html>
<html>
<head>

<meta charset="UTF-8">
<title>Daniel Sarumaha Social</title>

<style>

body{
margin:0;
font-family:Arial;
background:linear-gradient(135deg,#020617,#0f172a,#1e293b);
color:white;
transition:0.3s;
}

.light{
background:white;
color:black;
}

header{
display:flex;
justify-content:space-between;
align-items:center;
padding:20px;
background:#020617;
}

nav{
display:flex;
gap:10px;
padding:10px;
background:#020617;
flex-wrap:wrap;
}

nav button{
padding:10px 16px;
background:#1e293b;
border:none;
border-radius:10px;
color:white;
cursor:pointer;
}

main{
padding:20px;
max-width:900px;
margin:auto;
}

.card{
background:rgba(30,41,59,0.9);
padding:20px;
border-radius:16px;
margin-bottom:20px;
box-shadow:0 10px 30px rgba(0,0,0,0.5);
}

.post{
background:#020617;
padding:15px;
border-radius:12px;
margin-top:15px;
}

.comment{
margin-left:20px;
font-size:14px;
color:#cbd5f5;
}

textarea,input{
width:100%;
padding:10px;
border-radius:8px;
border:none;
margin-top:5px;
}

.star{
font-size:30px;
cursor:pointer;
color:gray;
}

</style>

</head>

<body>

<header>

<h2>Daniel Sarumaha</h2>

<div>
<button onclick="mode()">Mode</button>
<button onclick="login()">Login</button>
</div>

</header>

<nav>

<button onclick="home()">Home</button>
<button onclick="profile()">Profil</button>
<button onclick="notes()">Posting</button>
<button onclick="leaderboard()">Leaderboard</button>
<button onclick="ratingPage()">Rating</button>

</nav>

<main id="app"></main>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import {
getAuth,
GoogleAuthProvider,
signInWithPopup,
onAuthStateChanged
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";

import {
getFirestore,
collection,
addDoc,
onSnapshot,
updateDoc,
doc,
increment,
getDocs,
query,
where,
orderBy
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

import {
getStorage,
ref,
uploadBytes,
getDownloadURL
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-storage.js";


const firebaseConfig={
apiKey:"AIzaSyBj1j2odizaiO91XtdFaQVo6_k2G8ter7M",
authDomain:"daniel-website-f1f3b.firebaseapp.com",
projectId:"daniel-website-f1f3b",
storageBucket:"daniel-website-f1f3b.firebasestorage.app",
messagingSenderId:"113198911345",
appId:"1:113198911345:web:f9ecb06524b7e27bf470d0"
};

const app=initializeApp(firebaseConfig);
const auth=getAuth(app);
const db=getFirestore(app);
const storage=getStorage(app);
const provider=new GoogleAuthProvider();


window.login=async function(){
await signInWithPopup(auth,provider);
alert("Login berhasil");
}

onAuthStateChanged(auth,user=>{
if(user){
console.log("Login:",user.displayName);
}
})

window.mode=function(){
document.body.classList.toggle("light");
}

window.home=function(){

document.getElementById("app").innerHTML=`

<div class="card">

<input id="search" placeholder="Cari posting...">
<button onclick="searchPost()">Cari</button>

</div>

<div class="card">

<h3>Posting Publik</h3>

<div id="posts"></div>

</div>

`;

loadPosts();

}

window.profile=function(){

if(!auth.currentUser){
alert("Login dulu");
return;
}

let u=auth.currentUser;

document.getElementById("app").innerHTML=`

<div class="card">

<img src="${u.photoURL}" width="100">

<h3>${u.displayName}</h3>

<p>${u.email}</p>

</div>

`;

}

window.notes=function(){

document.getElementById("app").innerHTML=`

<div class="card">

<h3>Buat Post</h3>

<textarea id="text"></textarea>

<input type="file" id="img">

<br><br>

<button onclick="post()">Posting</button>

</div>

`;

}

window.post=async function(){

if(!auth.currentUser){
alert("Login dulu");
return;
}

let text=document.getElementById("text").value;
let file=document.getElementById("img").files[0];

let url="";

if(file){

const storageRef=ref(storage,"posts/"+Date.now());

await uploadBytes(storageRef,file);

url=await getDownloadURL(storageRef);

}

await addDoc(collection(db,"posts"),{
user:auth.currentUser.displayName,
text:text,
image:url,
likes:0,
time:Date.now()
});

alert("Posting berhasil");

}

function loadPosts(){

const q=query(collection(db,"posts"),orderBy("time","desc"));

onSnapshot(q,snap=>{

let html="";

snap.forEach(docu=>{

let p=docu.data();

html+=`

<div class="post">

<b>${p.user}</b>

<p>${p.text}</p>

${p.image?`<img src="${p.image}" width="100%">`:""}

<button onclick="like('${docu.id}')">
👍 ${p.likes}
</button>

<br><br>

<input id="c${docu.id}" placeholder="Komentar">

<button onclick="comment('${docu.id}')">
Kirim
</button>

<div id="cm${docu.id}"></div>

</div>

`;

loadComments(docu.id);

});

document.getElementById("posts").innerHTML=html;

});

}

function loadComments(postId){

const q=query(
collection(db,"comments"),
where("post","==",postId)
);

onSnapshot(q,snap=>{

let html="";

snap.forEach(d=>{

let c=d.data();

html+=`<div class="comment"><b>${c.user}</b>: ${c.text}</div>`;

});

setTimeout(()=>{
let el=document.getElementById("cm"+postId);
if(el) el.innerHTML=html;
},200);

});

}

window.like=async function(id){

const ref=doc(db,"posts",id);

await updateDoc(ref,{likes:increment(1)});

}

window.comment=async function(id){

if(!auth.currentUser){
alert("Login dulu");
return;
}

let text=document.getElementById("c"+id).value;

await addDoc(collection(db,"comments"),{
post:id,
user:auth.currentUser.displayName,
text:text
});

}

window.searchPost=async function(){

let key=document.getElementById("search").value.toLowerCase();

const snap=await getDocs(collection(db,"posts"));

let html="";

snap.forEach(d=>{

let p=d.data();

if(p.text.toLowerCase().includes(key)){

html+=`<div class="post">${p.text}</div>`;

}

});

document.getElementById("posts").innerHTML=html;

}

window.leaderboard=async function(){

const snap=await getDocs(collection(db,"posts"));

let users={};

snap.forEach(d=>{
let p=d.data();
if(!users[p.user])users[p.user]=0;
users[p.user]+=p.likes;
});

let arr=Object.entries(users).sort((a,b)=>b[1]-a[1]);

let html="<div class='card'><h2>Leaderboard</h2>";

arr.forEach((u,i)=>{

let badge="🙂";

if(u[1]>50) badge="🔥";
else if(u[1]>20) badge="⭐";

html+=`${i+1}. ${u[0]} (${u[1]} likes) ${badge}<br>`;
});

html+="</div>";

document.getElementById("app").innerHTML=html;

}

window.ratingPage=async function(){

const snap=await getDocs(collection(db,"rating"));

let total=0;

snap.forEach(d=>{
total+=d.data().value;
});

let avg=(snap.size?total/snap.size:0).toFixed(1);

let html=`<div class="card">
<h2>Rating Website</h2>
<p>Rating rata-rata: ${avg} ⭐</p>
`;

for(let i=1;i<=5;i++){
html+=`<span class="star" onclick="rate(${i})">★</span>`;
}

html+="</div>";

document.getElementById("app").innerHTML=html;

}

window.rate=async function(n){

await addDoc(collection(db,"rating"),{value:n});

alert("Terima kasih ratingnya");

}

home();

</script>

</body>
</html>