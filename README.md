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
}

header{
display:flex;
justify-content:space-between;
align-items:center;
padding:20px;
background:#020617;
}

button{
padding:10px 16px;
background:#1e293b;
border:none;
border-radius:10px;
color:white;
cursor:pointer;
}

main{
padding:20px;
max-width:800px;
margin:auto;
}

.card{
background:#1e293b;
padding:20px;
border-radius:16px;
margin-top:20px;
}

</style>

</head>

<body>

<header>

<h2>Daniel Sarumaha</h2>

<div id="loginArea">
<button onclick="login()">Login Google</button>
</div>

</header>

<main id="app"></main>

<script type="module">

import { initializeApp }
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import {
getAuth,
GoogleAuthProvider,
signInWithRedirect,
getRedirectResult,
onAuthStateChanged,
signOut
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";

import {
getFirestore,
collection,
addDoc,
onSnapshot
}
from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";


const firebaseConfig = {

apiKey: "AIzaSyBj1j2odizaiO91XtdFaQVo6_k2G8ter7M",
authDomain: "daniel-website-f1f3b.firebaseapp.com",
projectId: "daniel-website-f1f3b",
storageBucket: "daniel-website-f1f3b.firebasestorage.app",
messagingSenderId: "113198911345",
appId: "1:113198911345:web:f9ecb06524b7e27bf470d0"

};

const app = initializeApp(firebaseConfig);

const auth = getAuth(app);
const db = getFirestore(app);
const provider = new GoogleAuthProvider();



window.login = async function(){

await signInWithRedirect(auth,provider);

}



getRedirectResult(auth)
.then((result)=>{

if(result && result.user){

alert("Login berhasil sebagai " + result.user.displayName);

}

})
.catch((error)=>{

console.log(error);

});



onAuthStateChanged(auth,(user)=>{

if(user){

document.getElementById("loginArea").innerHTML=`

<img src="${user.photoURL}" width="35" style="border-radius:50%">
<span>${user.displayName}</span>
<button onclick="logout()">Logout</button>

`;

home();

}

});



window.logout = function(){

signOut(auth);

location.reload();

}



function home(){

document.getElementById("app").innerHTML=`

<div class="card">

<h3>Buat Post</h3>

<textarea id="text" style="width:100%;padding:10px"></textarea>

<br><br>

<button onclick="post()">Posting</button>

</div>

<div class="card">

<h3>Postingan</h3>

<div id="posts"></div>

</div>

`;

loadPosts();

}



window.post = async function(){

if(!auth.currentUser){

alert("Login dulu");

return;

}

let text=document.getElementById("text").value;

await addDoc(collection(db,"posts"),{

user:auth.currentUser.displayName,
text:text,
time:Date.now()

});

}



function loadPosts(){

onSnapshot(collection(db,"posts"),snap=>{

let html="";

snap.forEach(d=>{

let p=d.data();

html+=`

<div style="background:#020617;padding:10px;margin-top:10px;border-radius:10px">

<b>${p.user}</b>

<p>${p.text}</p>

</div>

`;

});

document.getElementById("posts").innerHTML=html;

});

}

</script>

</body>
</html>