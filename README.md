<!DOCTYPE html>
<html lang="id">

<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Daniel Sarumaha Web</title>

<style>

body{
margin:0;
font-family:Segoe UI;
background:linear-gradient(135deg,#020617,#0f172a,#1e293b);
color:white;
}

/* HEADER */

header{
padding:20px;
background:rgba(0,0,0,0.4);
backdrop-filter:blur(10px);
border-bottom:1px solid rgba(255,255,255,0.1);
}

header h1{
margin:0;
font-size:26px;
}

/* LAYOUT */

.container{
display:flex;
min-height:100vh;
}

.sidebar{
width:230px;
padding:20px;
}

.sidebar button{
width:100%;
padding:12px;
margin-bottom:12px;
border:none;
border-radius:10px;
background:rgba(255,255,255,0.08);
color:white;
font-size:16px;
cursor:pointer;
transition:0.3s;
}

.sidebar button:hover{
transform:translateY(-3px);
background:rgba(255,255,255,0.2);
}

.main{
flex:1;
padding:30px;
}

.page{
display:none;
}

.page.active{
display:block;
}

/* 3D CARD */

.card{
background:rgba(255,255,255,0.06);
backdrop-filter:blur(20px);
border-radius:20px;
padding:25px;
box-shadow:0 10px 40px rgba(0,0,0,0.4);
margin-bottom:25px;
transition:0.3s;
}

.card:hover{
transform:translateY(-6px);
}

/* PROFILE */

.profile{
text-align:center;
}

.profile img{
width:130px;
height:130px;
border-radius:50%;
border:3px solid white;
margin-bottom:10px;
}

/* SOCIAL */

.social{
display:flex;
justify-content:center;
gap:15px;
margin-top:15px;
}

.social a{
padding:10px 18px;
border-radius:10px;
background:#1e293b;
text-decoration:none;
color:white;
font-size:14px;
}

/* POST */

textarea,input{
width:100%;
padding:10px;
border:none;
border-radius:8px;
margin-top:10px;
}

button.primary{
margin-top:10px;
background:#3b82f6;
color:white;
padding:10px;
border:none;
border-radius:8px;
cursor:pointer;
}

.post{
background:#020617;
padding:15px;
border-radius:12px;
margin-top:15px;
}

.comment{
background:#0f172a;
padding:7px;
border-radius:8px;
margin-top:5px;
}

.like{
cursor:pointer;
color:#38bdf8;
}

/* FOLLOW */

.follow{
margin-top:15px;
padding:10px 15px;
background:#22c55e;
border:none;
border-radius:10px;
cursor:pointer;
}

.star{
font-size:30px;
cursor:pointer;
color:gray;
}

.star.active{
color:gold;
}

</style>
</head>

<body>

<header>

<h1>Daniel Sarumaha  
<br><small>@Danieldolars</small></h1>

</header>

<div class="container">

<!-- SIDEBAR -->

<div class="sidebar">

<button onclick="showPage('home')">Home</button>
<button onclick="showPage('profil')">Profil</button>
<button onclick="showPage('catatan')">Catatan</button>
<button onclick="showPage('rating')">Rating</button>
<button onclick="showPage('leaderboard')">Leaderboard</button>

</div>

<!-- MAIN -->

<div class="main">

<!-- HOME -->

<div id="home" class="page active">

<div class="card profile">

<img src="https://i.imgur.com/4AiXzf8.jpeg">

<h2>Daniel Sarumaha</h2>

<p>
Seorang kreator digital yang tertarik pada teknologi, website development,
dan eksplorasi ide baru. Saya suka membuat proyek web sederhana,
bereksperimen dengan coding, serta membangun platform digital sendiri.
</p>

<p>
📍 Indonesia  
🎓 Pelajar MIA  
💻 Web Enthusiast
</p>

<div class="social">

<a href="https://wa.me/6281388149795">WhatsApp</a>
<a href="https://instagram.com/Danieldolars">Instagram</a>
<a href="#">Facebook</a>

</div>

<button class="follow" onclick="follow()">Follow Website</button>

<p id="followers"></p>

</div>

<div class="card">

<h3>Posting Publik</h3>

<div id="posts"></div>

</div>

</div>

<!-- PROFIL -->

<div id="profil" class="page">

<div class="card">

<h2>Tentang Saya</h2>

<p>
Nama: Daniel Sarumaha  
</p>

<p>
Hobi: Coding, belajar teknologi, membuat website.
</p>

<p>
Tujuan: Membangun platform digital dan terus mengembangkan skill teknologi.
</p>

</div>

</div>

<!-- CATATAN -->

<div id="catatan" class="page">

<div class="card">

<h2>Buat Catatan</h2>

<input id="username" placeholder="Nama kamu">

<textarea id="postInput" placeholder="Tulis sesuatu..."></textarea>

<button class="primary" onclick="createPost()">Posting</button>

</div>

</div>

<!-- RATING -->

<div id="rating" class="page">

<div class="card">

<h2>Rating Website</h2>

<div>

<span class="star" onclick="rate(1)">★</span>
<span class="star" onclick="rate(2)">★</span>
<span class="star" onclick="rate(3)">★</span>
<span class="star" onclick="rate(4)">★</span>
<span class="star" onclick="rate(5)">★</span>

</div>

<p id="ratingText"></p>

</div>

</div>

<!-- LEADERBOARD -->

<div id="leaderboard" class="page">

<div class="card">

<h2>User Terpopuler</h2>

<ol id="leaderboardList"></ol>

</div>

</div>

</div>
</div>

<script>

function showPage(id){

document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"))
document.getElementById(id).classList.add("active")

}

function createPost(){

let text=document.getElementById("postInput").value
let user=document.getElementById("username").value

if(!text || !user)return

let posts=JSON.parse(localStorage.getItem("posts")||"[]")

posts.push({
user:user,
text:text,
likes:0,
comments:[]
})

localStorage.setItem("posts",JSON.stringify(posts))

document.getElementById("postInput").value=""

loadPosts()

}

function loadPosts(){

let posts=JSON.parse(localStorage.getItem("posts")||"[]")
let area=document.getElementById("posts")

area.innerHTML=""

posts.forEach((p,i)=>{

let commentsHTML=""

p.comments.forEach(c=>{
commentsHTML+=`<div class="comment">${c}</div>`
})

let div=document.createElement("div")

div.className="post"

div.innerHTML=`

<b>${p.user}</b>

<p>${p.text}</p>

<span class="like" onclick="likePost(${i})">👍 ${p.likes}</span>

<br><br>

<input id="c${i}" placeholder="Komentar">

<button onclick="addComment(${i})">Kirim</button>

${commentsHTML}

`

area.appendChild(div)

})

updateLeaderboard()

}

function likePost(i){

let posts=JSON.parse(localStorage.getItem("posts"))

posts[i].likes++

localStorage.setItem("posts",JSON.stringify(posts))

loadPosts()

}

function addComment(i){

let text=document.getElementById("c"+i).value

if(!text)return

let posts=JSON.parse(localStorage.getItem("posts"))

posts[i].comments.push(text)

localStorage.setItem("posts",JSON.stringify(posts))

loadPosts()

}

function rate(n){

localStorage.setItem("rating",n)

let stars=document.querySelectorAll(".star")

stars.forEach(s=>s.classList.remove("active"))

for(let i=0;i<n;i++){
stars[i].classList.add("active")
}

document.getElementById("ratingText").innerText="Rating kamu: "+n

}

function updateLeaderboard(){

let posts=JSON.parse(localStorage.getItem("posts")||"[]")

let score={}

posts.forEach(p=>{
if(!score[p.user])score[p.user]=0
score[p.user]+=p.likes
})

let sorted=Object.entries(score).sort((a,b)=>b[1]-a[1])

let list=document.getElementById("leaderboardList")

list.innerHTML=""

sorted.forEach(s=>{

let li=document.createElement("li")

li.innerText=s[0]+" ("+s[1]+" likes)"

list.appendChild(li)

})

}

function follow(){

let f=localStorage.getItem("followers")||0
f++
localStorage.setItem("followers",f)

document.getElementById("followers").innerText="Followers: "+f

}

loadPosts()

document.getElementById("followers").innerText="Followers: "+(localStorage.getItem("followers")||0)

</script>

</body>
</html>