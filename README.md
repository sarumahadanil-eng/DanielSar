<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Daniel Social Web</title>

<style>

body{
font-family:Arial;
background:#0f172a;
color:white;
margin:0;
text-align:center;
}

h1{margin-top:20px}

input,textarea{
padding:10px;
margin:5px;
border-radius:6px;
border:none;
}

button{
padding:10px;
border:none;
border-radius:6px;
background:#38bdf8;
color:white;
cursor:pointer;
}

.box{
background:#1e293b;
padding:20px;
margin:20px;
border-radius:10px;
}

.post{
background:#334155;
margin:10px;
padding:10px;
border-radius:8px;
}

.menu{
position:fixed;
bottom:60px;
right:60px;
width:200px;
height:200px;
}

.center{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:#38bdf8;
width:60px;
height:60px;
border-radius:50%;
display:flex;
align-items:center;
justify-content:center;
cursor:pointer;
font-size:25px;
}

.item{
position:absolute;
width:50px;
height:50px;
background:#1e293b;
border-radius:50%;
display:flex;
align-items:center;
justify-content:center;
cursor:pointer;
opacity:0;
transition:0.4s;
}

.menu.active .item{opacity:1}

.item:nth-child(2){top:-20px;left:70px;}
.item:nth-child(3){top:30px;left:150px;}
.item:nth-child(4){top:120px;left:150px;}
.item:nth-child(5){top:170px;left:70px;}
.item:nth-child(6){top:120px;left:-10px;}
.item:nth-child(7){top:30px;left:-10px;}

</style>

</head>

<body>

<div id="app"></div>

<div class="menu" id="menu">

<div class="center" onclick="toggleMenu()">+</div>

<div class="item" onclick="home()">🏠</div>
<div class="item" onclick="notes()">📝</div>
<div class="item" onclick="rating()">⭐</div>
<div class="item" onclick="follow()">👥</div>
<div class="item" onclick="about()">👤</div>
<div class="item" onclick="logout()">🚪</div>

</div>

<script>

function toggleMenu(){
document.getElementById("menu").classList.toggle("active");
}

function loginPage(){

document.getElementById("app").innerHTML=`

<h1>Daniel Social Web</h1>

<div class="box">

<input id="user" placeholder="username">

<input id="pass" type="password" placeholder="password">

<button onclick="login()">Login</button>

<button onclick="register()">Register</button>

</div>

`;

}

function register(){

let u=document.getElementById("user").value
let p=document.getElementById("pass").value

localStorage.setItem("user_"+u,p)

alert("akun dibuat")

}

function login(){

let u=document.getElementById("user").value
let p=document.getElementById("pass").value

let saved=localStorage.getItem("user_"+u)

if(saved==p){

localStorage.setItem("login",u)
home()

}else{

alert("salah")

}

}

function logout(){

localStorage.removeItem("login")
loginPage()

}

function home(){

let u=localStorage.getItem("login")

document.getElementById("app").innerHTML=`

<h1>Halo ${u}</h1>

<div class="box">

Selamat datang di website sosial sederhana.

Gunakan menu bulat di kanan bawah.

</div>

`

}

function notes(){

let posts=JSON.parse(localStorage.getItem("posts")||"[]")

let html=`<h1>Catatan</h1>

<textarea id="note"></textarea><br>

<button onclick="addNote()">Post</button>
`

posts.forEach((p,i)=>{

html+=`

<div class="post">

<b>${p.user}</b><br>

${p.text}<br><br>

❤️ ${p.like}

<button onclick="like(${i})">Like</button>

</div>

`

})

document.getElementById("app").innerHTML=html

}

function addNote(){

let text=document.getElementById("note").value
let user=localStorage.getItem("login")

let posts=JSON.parse(localStorage.getItem("posts")||"[]")

posts.push({

user:user,
text:text,
like:0

})

localStorage.setItem("posts",JSON.stringify(posts))

notes()

}

function like(i){

let posts=JSON.parse(localStorage.getItem("posts"))

posts[i].like++

localStorage.setItem("posts",JSON.stringify(posts))

notes()

}

function rating(){

let r=localStorage.getItem("rating")||0

document.getElementById("app").innerHTML=`

<h1>Rating Website</h1>

<div class="box">

Rating sekarang : ${r}

<br><br>

<button onclick="rate(1)">⭐</button>
<button onclick="rate(2)">⭐⭐</button>
<button onclick="rate(3)">⭐⭐⭐</button>
<button onclick="rate(4)">⭐⭐⭐⭐</button>
<button onclick="rate(5)">⭐⭐⭐⭐⭐</button>

</div>

`

}

function rate(v){

localStorage.setItem("rating",v)

rating()

}

function follow(){

let f=localStorage.getItem("follow")||"no"

document.getElementById("app").innerHTML=`

<h1>Follow Daniel</h1>

<div class="box">

Status : ${f}

<br><br>

<button onclick="toggleFollow()">Follow / Unfollow</button>

</div>

`

}

function toggleFollow(){

let f=localStorage.getItem("follow")

if(f=="yes"){

localStorage.setItem("follow","no")

}else{

localStorage.setItem("follow","yes")

}

follow()

}

function about(){

document.getElementById("app").innerHTML=`

<h1>Tentang Saya</h1>

<div class="box">

Nama : Daniel

<br><br>

Saya tertarik dengan dunia teknologi dan coding.
Saya sedang belajar membuat website dan sistem digital sendiri.

Saya juga tertarik dengan AI dan pengembangan web modern.

Website ini adalah proyek eksperimen saya untuk membuat
mini sosial media sederhana.

</div>

`

}

if(localStorage.getItem("login")){

home()

}else{

loginPage()

}

</script>

</body>
</html>