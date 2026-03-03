<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Website Daniel Dolar Sarumaha</title>

<style>
:root{
    --primary:#4e73df;
    --secondary:#1cc88a;
    --text:#ffffff;
}

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family: 'Segoe UI', sans-serif;
    scroll-behavior:smooth;
}

body{
    background:linear-gradient(135deg,#4e73df,#1cc88a);
    color:var(--text);
    text-align:center;
}

/* NAVIGATION */
nav{
    position:fixed;
    width:100%;
    background:rgba(0,0,0,0.3);
    padding:15px;
    backdrop-filter:blur(10px);
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
    width:170px;
    height:170px;
    border-radius:50%;
    border:5px solid white;
    animation:float 3s ease-in-out infinite;
}

.profile h1{
    margin-top:15px;
    font-size:28px;
}

/* CARD */
.card{
    background:rgba(255,255,255,0.15);
    backdrop-filter:blur(10px);
    padding:25px;
    margin:20px auto;
    max-width:600px;
    border-radius:15px;
    animation:fadeUp 1s ease;
}

/* BUTTON */
.btn{
    display:inline-block;
    padding:12px 25px;
    margin:10px;
    background:white;
    color:#333;
    border-radius:30px;
    text-decoration:none;
    transition:0.3s;
    cursor:pointer;
}

.btn:active{
    transform:scale(0.9);
}

.btn:hover{
    background:yellow;
}

/* RATING */
.stars{
    font-size:30px;
    cursor:pointer;
}

.stars span{
    transition:0.3s;
}

.stars span:hover{
    color:yellow;
}

/* ANIMATION */
@keyframes float{
    0%{transform:translateY(0px);}
    50%{transform:translateY(-15px);}
    100%{transform:translateY(0px);}
}

@keyframes fadeUp{
    from{opacity:0; transform:translateY(40px);}
    to{opacity:1; transform:translateY(0);}
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

<section id="home" class="profile">
<img src="foto.jpg" alt="Foto Daniel Dolar Sarumaha">
<h1>Daniel Dolar Sarumaha</h1>
<p>Mahasiswa UNIRAYA | Web Developer Pemula</p>
</section>

<section id="about">
<div class="card">
<h2>Data Diri</h2>
<p>Nama: Daniel Dolar Sarumaha</p>
<p>Lahir: Hiliamaetaniha, 14-05-2006</p>
<p>Alamat: Desa Hiliamaetaniha</p>
<p>Email: sarumahadanil@gmail.com</p>
<p>Hobi: Berenang, Bulutangkis, Ngoding</p>
</div>
</section>

<section id="contact">
<div class="card">
<h2>Hubungi Saya</h2>
<a href="https://instagram.com/Danieldolars" target="_blank" class="btn">Instagram</a>
<a href="https://wa.me/081388149795" target="_blank" class="btn">WhatsApp</a>
</div>
</section>

<section id="rating">
<div class="card">
<h2>Beri Rating Website Ini ⭐</h2>
<div class="stars">
<span onclick="rate(1)">⭐</span>
<span onclick="rate(2)">⭐</span>
<span onclick="rate(3)">⭐</span>
<span onclick="rate(4)">⭐</span>
<span onclick="rate(5)">⭐</span>
</div>
<p id="result"></p>
</div>
</section>

<script>
function rate(star){
    localStorage.setItem("rating", star);
    document.getElementById("result").innerHTML =
        "Terima kasih sudah memberi rating " + star + " ⭐";
}

window.onload = function(){
    let saved = localStorage.getItem("rating");
    if(saved){
        document.getElementById("result").innerHTML =
        "Rating terakhir yang kamu berikan: " + saved + " ⭐";
    }
}
</script>

</body>
</html>