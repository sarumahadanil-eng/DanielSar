<!DOCTYPE html>
<html>
<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Daniel Internet God App</title>

<style>

body{
margin:0;
font-family:sans-serif;
background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
color:white;
height:100vh;
display:flex;
justify-content:center;
align-items:center;
}

.card{
width:950px;
height:540px;
display:flex;
background:rgba(255,255,255,0.05);
backdrop-filter:blur(20px);
border-radius:20px;
overflow:hidden;
box-shadow:0 0 40px rgba(0,0,0,0.5);
}

.left{
flex:1;
display:flex;
justify-content:center;
align-items:center;
background:linear-gradient(120deg,#89f7fe,#66a6ff);
}

.character{
width:170px;
animation:walk 3s infinite linear;
}

@keyframes walk{
0%{transform:translateX(-30px)}
50%{transform:translateX(30px)}
100%{transform:translateX(-30px)}
}

.right{
flex:1;
padding:30px;
overflow:auto;
}

input,textarea{
width:100%;
padding:10px;
margin:8px 0;
border:none;
border-radius:8px;
}

button{
width:100%;
padding:10px;
margin-top:10px;
border:none;
border-radius:8px;
background:#00ffd5;
font-weight:bold;
cursor:pointer;
}

button:hover{
transform:scale(1.03);
}

.post{
background:rgba(255,255,255,0.08);
padding:10px;
border-radius:10px;
margin-top:10px;
}

#dashboard{
display:none;
}

</style>

</head>

<body>

<div class="card">

<div class="left">

<img class="character"
src="https://cdn-icons-png.flaticon.com/512/4140/4140048.png">

</div>

<div class="right">

<!-- AUTH -->

<div id="auth">

<h2 id="title">Login</h2>

<input id="email" placeholder="Email">

<input id="password" type="password" placeholder="Password">

<button onclick="login()">Login</button>

<button onclick="googleLogin()">Login Google</button>

<p onclick="toggleMode()" style="cursor:pointer;color:#00ffd5">
Register akun
</p>

</div>

<!-- DASHBOARD -->

<div id="dashboard">

<h2>🚀 Dashboard</h2>

<div id="profile"></div>

<input type="file" id="avatar">

<button onclick="uploadAvatar()">Upload Foto</button>

<textarea id="postText" placeholder="Tulis sesuatu..."></textarea>

<button onclick="sendPost()">Post</button>

<div id="posts"></div>

<button onclick="logout()">Logout</button>

</div>

</div>

</div>

<script type="module">

import { initializeApp }
from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";

import {
getAuth,
signInWithEmailAndPassword,
createUserWithEmailAndPassword,
GoogleAuthProvider,
signInWithPopup,
signOut,
onAuthStateChanged
}

from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";

import {
getFirestore,
collection,
addDoc,
onSnapshot
}

from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

import {
getStorage,
ref,
uploadBytes,
getDownloadURL
}

from "https://www.gstatic.com/firebasejs/10.12.2/firebase-storage.js";


const firebaseConfig = {

apiKey:"ISI_API_KEY",
authDomain:"ISI_DOMAIN",
projectId:"ISI_PROJECT_ID",
storageBucket:"ISI_BUCKET",
messagingSenderId:"ISI_ID",
appId:"ISI_APPID"

};

const app=initializeApp(firebaseConfig);

const auth=getAuth(app);

const db=getFirestore(app);

const storage=getStorage(app);

let registerMode=false;

/* LOGIN */

window.login=function(){

const email=emailInput()
const password=passwordInput()

if(registerMode){

createUserWithEmailAndPassword(auth,email,password)

}else{

signInWithEmailAndPassword(auth,email,password)

}

}

/* GOOGLE LOGIN */

window.googleLogin=function(){

const provider=new GoogleAuthProvider()

signInWithPopup(auth,provider)

}

/* TOGGLE */

window.toggleMode=function(){

registerMode=!registerMode

document.getElementById("title").innerText=

registerMode?"Register":"Login"

}

/* LOGOUT */

window.logout=function(){

signOut(auth)

}

/* AUTH STATE */

onAuthStateChanged(auth,user=>{

if(user){

showDashboard(user)

loadPosts()

}else{

showLogin()

}

})

/* UI SWITCH */

function showDashboard(user){

document.getElementById("auth").style.display="none"

document.getElementById("dashboard").style.display="block"

document.getElementById("profile").innerHTML=

"👤 "+user.email

}

function showLogin(){

document.getElementById("auth").style.display="block"

document.getElementById("dashboard").style.display="none"

}

/* POST SYSTEM */

window.sendPost=async function(){

const text=document.getElementById("postText").value

await addDoc(collection(db,"posts"),{

text:text,
time:Date.now()

})

document.getElementById("postText").value=""

}

/* REALTIME POSTS */

function loadPosts(){

onSnapshot(collection(db,"posts"),snapshot=>{

const postsDiv=document.getElementById("posts")

postsDiv.innerHTML=""

snapshot.forEach(doc=>{

const p=document.createElement("div")

p.className="post"

p.innerText=doc.data().text

postsDiv.appendChild(p)

})

})

}

/* AVATAR UPLOAD */

window.uploadAvatar=async function(){

const file=document.getElementById("avatar").files[0]

const user=auth.currentUser

const storageRef=ref(storage,"avatars/"+user.uid)

await uploadBytes(storageRef,file)

const url=await getDownloadURL(storageRef)

document.getElementById("profile").innerHTML+=

"<br><img src='"+url+"' width='60'>"

}

/* HELPERS */

function emailInput(){

return document.getElementById("email").value

}

function passwordInput(){

return document.getElementById("password").value

}

</script>

</body>
</html>