<!DOCTYPE html>
<html>
<head>

<meta charset="UTF-8">
<title>Daniel Sarumaha</title>

<style>

body{
margin:0;
font-family:Arial;
background:linear-gradient(135deg,#020617,#0f172a);
color:white;
}

header{
padding:20px;
background:#020617;
display:flex;
justify-content:space-between;
align-items:center;
}

.menu{
position:fixed;
top:70px;
left:0;
width:200px;
height:100%;
background:#020617;
padding:20px;
}

.menu button{
width:100%;
padding:12px;
margin-bottom:10px;
background:#1e293b;
border:none;
color:white;
border-radius:8px;
cursor:pointer;
}

.content{
margin-left:220px;
padding:30px;
}

.card{
background:#1e293b;
padding:20px;
border-radius:12px;
margin-bottom:20px;
}

textarea{
width:100%;
height:80px;
border-radius:8px;
padding:10px;
}

.post{
background:#020617;
padding:15px;
margin-top:15px;
border-radius:10px;
}

.comment{
margin-left:20px;
font-size:14px;
color:#cbd5f5;
}

.star{
font-size:20px;
cursor:pointer;
color:gray;
}

.active{
color:gold;
}

</style>

</head>

<body>

<header>

<h2>Daniel Sarumaha</h2>

<button onclick="login()">Login Google</button>

</header>

<div class="menu">

<button onclick="home()">Home</button>
<button onclick="profile()">Profil</button>
<button onclick="notes()">Catatan</button>
<button onclick="leaderboard()">Leaderboard</button>
<button onclick="ratingPage()">Rating</button>

</div>

<div class="content" id="content"></div>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import { getAuth, signInWithPopup, GoogleAuthProvider } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";

import { getFirestore,
collection,
addDoc,
onSnapshot,
updateDoc,
doc,
increment,
getDocs

}

from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {

apiKey:"APIKEY",
authDomain:"DOMAIN",
projectId:"PROJECTID",
storageBucket:"BUCKET",
messagingSenderId:"MSGID",
appId:"APPID"

};

const app=initializeApp(firebaseConfig);

const auth=getAuth();

const db=getFirestore();

window.login=async function(){

const provider=new GoogleAuthProvider();

await signInWithPopup(auth,provider);

alert("Login berhasil")

}

window.home=function(){

document.getElementById("content").innerHTML=`

<div class="card">

<img src="profile.jpg" width="120">

<h2>Daniel Sarumaha</h2>

<p>

Creator website ini.<br>
IG: Danieldolars<br>
FB: Daniel Sarumaha<br>

</p>

<a href="https://wa.me/6281388149795">WhatsApp</a><br>
<a href="https://instagram.com/Danieldolars">Instagram</a><br>
<a href="#">Facebook</a>

</div>

<div class="card">

<h3>Posting Publik</h3>

<div id="posts"></div>

</div>

`

loadPosts()

}

window.profile=function(){

if(!auth.currentUser){

alert("Login dulu")

return

}

let u=auth.currentUser

document.getElementById("content").innerHTML=`

<div class="card">

<img src="${u.photoURL}" width="100">

<h3>${u.displayName}</h3>

<p>${u.email}</p>

</div>

`

}

window.notes=function(){

document.getElementById("content").innerHTML=`

<div class="card">

<h3>Buat Catatan</h3>

<textarea id="text"></textarea>

<br><br>

<button onclick="post()">Posting</button>

</div>

`

}

window.post=async function(){

let text=document.getElementById("text").value

await addDoc(collection(db,"posts"),{

user:auth.currentUser.displayName,

text:text,

likes:0

})

alert("Posting dibuat")

}

function loadPosts(){

onSnapshot(collection(db,"posts"),(snap)=>{

let html=""

snap.forEach(docu=>{

let p=docu.data()

html+=`

<div class="post">

<b>${p.user}</b>

<p>${p.text}</p>

<button onclick="like('${docu.id}')">

👍 ${p.likes}

</button>

<input id="c${docu.id}" placeholder="komentar">

<button onclick="comment('${docu.id}')">Kirim</button>

<div id="cm${docu.id}"></div>

</div>

`

})

document.getElementById("posts").innerHTML=html

})

}

window.like=async function(id){

let ref=doc(db,"posts",id)

await updateDoc(ref,{likes:increment(1)})

}

window.comment=async function(id){

let text=document.getElementById("c"+id).value

await addDoc(collection(db,"comments"),{

post:id,

user:auth.currentUser.displayName,

text:text

})

}

window.leaderboard=async function(){

const snap=await getDocs(collection(db,"posts"))

let users={}

snap.forEach(d=>{

let p=d.data()

if(!users[p.user])users[p.user]=0

users[p.user]+=p.likes

})

let arr=Object.entries(users).sort((a,b)=>b[1]-a[1])

let html="<div class='card'><h2>Leaderboard</h2>"

arr.forEach((u,i)=>{

html+=`${i+1}. ${u[0]} (${u[1]} likes)<br>`

})

html+="</div>"

document.getElementById("content").innerHTML=html

}

window.ratingPage=function(){

let html=`<div class="card"><h2>Rating Website</h2>`

for(let i=1;i<=5;i++){

html+=`<span class="star" onclick="rate(${i})">★</span>`

}

html+="</div>"

document.getElementById("content").innerHTML=html

}

window.rate=async function(n){

await addDoc(collection(db,"rating"),{

value:n

})

alert("Terima kasih atas ratingnya")

}

home()

</script>

</body>

</html>