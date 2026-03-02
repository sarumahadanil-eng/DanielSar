<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Profil Saya</title>

<style>
:root{
    --primary:#4e73df;
    --bg:#f4f6f9;
    --text:#333;
    --card:#ffffff;
}

body.dark{
    --primary:#1cc88a;
    --bg:#1e1e1e;
    --text:#f4f4f4;
    --card:#2c2c2c;
}

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family: Arial, sans-serif;
    scroll-behavior:smooth;
}

body{
    background:var(--bg);
    color:var(--text);
}

/* NAVIGATION */
nav{
    position:fixed;
    width:100%;
    background:var(--card);
    padding:15px 0;
    box-shadow:0 2px 10px rgba(0,0,0,0.1);
    z-index:1000;
}

nav ul{
    display:flex;
    justify-content:center;
    list-style:none;
}

nav ul li{
    margin:0 20px;
}

nav ul li a{
    text-decoration:none;
    color:var(--text);
    font-weight:bold;
}

nav ul li a:hover{
    color:var(--primary);
}

/* SECTION */
section{
    padding:100px 20px;
    text-align:center;
}

/* PROFILE */
.profile img{
    width:160px;
    height:160px;
    border-radius:50%;
    border:5px solid var(--primary);
    margin-bottom:15px;
    animation:fadeIn 1.5s ease-in-out;
}

.profile h1{
    margin-bottom:10px;
}

/* CARD */
.card{
    background:var(--card);
    max-width:600px;
    margin:20px auto;
    padding:25px;
    border-radius:15px;
    box-shadow:0 5px 20px rgba(0,0,0,0.1);
    animation:fadeUp 1s ease;
}

/* BUTTON */
.btn{
    display:inline-block;
    padding:10px 20px;
    margin:10px;
    background:var(--primary);
    color:white;
    border-radius:8px;
    text-decoration:none;
    transition:0.3s;
}

.btn:hover{
    opacity:0.8;
}

/* DARK MODE BUTTON */
.toggle{
    position:fixed;
    right:20px;
    bottom:20px;
    background:var(--primary);
    color:white;
    padding:12px;
    border-radius:50%;
    cursor:pointer;
}

/* ANIMATION */
@keyframes fadeIn{
    from{opacity:0; transform:scale(0.8);}
    to{opacity:1; transform:scale(1);}
}

@keyframes fadeUp{
    from{opacity:0; transform:translateY(30px);}
    to{opacity:1; transform:translateY(0);}
}
</style>
</head>

<body>

<nav>
<ul>
<li><a href="#home">Home</a></li>
<li><a href="#about">Tentang</a></li>
<li><a href="#contact">Kontak</a></li>
</ul>
</nav>

<section id="home" class="profile">
<img src="foto-kamu.jpg" alt="Foto Saya">
<h1>Nama Kamu</h1>
<p>Pelajar | Web Developer Pemula</p>
</section>

<section id="about">
<div class="card">
<h2>Data Diri</h2>
<p><b>Nama Lengkap:</b> Isi Nama Kamu</p>
<p><b>Tempat, Tanggal Lahir:</b> Isi TTL</p>
<p><b>Alamat:</b> Isi Alamat</p>
<p><b>Email:</b> emailkamu@gmail.com</p>
<p><b>Hobi:</b> Coding, Membaca</p>
</div>

<div class="card">
<h2>Tentang Saya</h2>
<p>
Saya adalah seorang pelajar yang sedang belajar membuat website.
Website ini dibuat untuk memperkenalkan diri saya secara profesional.
</p>
</div>
</section>

<section id="contact">
<div class="card">
<h2>Hubungi Saya</h2>

<a href="https://instagram.com/usernamekamu" target="_blank" class="btn">Instagram</a>

<a href="https://wa.me/628xxxxxxxxxx" target="_blank" class="btn">WhatsApp</a>

</div>
</section>

<div class="toggle" onclick="toggleDarkMode()">🌙</div>

<script>
function toggleDarkMode(){
    document.body.classList.toggle("dark");
}
</script>

</body>
</html>
