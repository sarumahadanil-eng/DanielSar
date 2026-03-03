<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daniel Dolar Sarumaha</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family: 'Segoe UI', sans-serif;
    scroll-behavior:smooth;
}

body{
    background: linear-gradient(135deg,#4e73df,#1cc88a);
    color:white;
    text-align:center;
}

/* NAVBAR */
nav{
    position:fixed;
    width:100%;
    background:rgba(0,0,0,0.3);
    padding:15px;
    backdrop-filter:blur(10px);
    z-index:1000;
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
    width:180px;
    height:180px;
    border-radius:50%;
    object-fit:cover;
    border:5px solid white;
    box-shadow:0 10px 30px rgba(0,0,0,0.4);
    animation:float 3s ease-in-out infinite;
}

.profile h1{
    margin-top:15px;
    font-size:28px;
}

.card{
    background:rgba(255,255,255,0.15);
    backdrop-filter:blur(15px);
    padding:25px;
    margin:20px auto;
    max-width:600px;
    border-radius:15px;
    animation:fadeUp 1s ease;
}

.btn{
    display:inline-block;
    padding:10px 20px;
    margin:10px;
    background:white;
    color:#333;
    border-radius:30px;
    text-decoration:none;
    border:none;
    cursor:pointer;
    transition:0.3s;
}

.btn:hover{
    background:yellow;
}

.btn:active{
    transform:scale(0.9);
}

textarea{
    width:100%;
    padding:10px;
    margin-top:10px;
    border-radius:10px;
    border:none;
    resize:none;
}

.stars{
    font-size:30px;
    cursor:pointer;
}

.stars span:hover{
    color:yellow;
}

.review-box{
    background:rgba(255,255,255,0.2);
    padding:10px;
    margin:10px 0;
    border-radius:10px;
}

@keyframes float{
    0%{transform:translateY(0);}
    50%{transform:translateY(-15px);}
    100%{transform:translateY(0);}
}

@keyframes fadeUp{
    from{opacity:0; transform:translateY(30px);}
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
<img src="daniel.jpg" alt="Foto Daniel">
<h1>Daniel Dolar Sarumaha</h1>
<p>Mahasiswa UNIRAYA | Web Developer Pemula</p>
</section>

<section id="about">
<div class="card">
<h2>Tentang Saya</h2>
<p>Halo, saya Daniel Dolar Sarumaha. 
Saya sedang belajar menjadi web developer dan terus mengembangkan kemampuan di bidang pemrograman.</p>
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
<h2>Beri Rating & Komentar</h2>

<div class="stars">
<span onclick="setRating(1)">⭐</span>
<span onclick="setRating(2)">⭐</span>
<span onclick="setRating(3)">⭐</span>
<span onclick="setRating(4)">⭐</span>
<span onclick="setRating(5)">⭐</span>
</div>

<p id="ratingText"></p>

<textarea id="commentInput" rows="3" placeholder="Tulis komentar..."></textarea>
<button class="btn" onclick="submitReview()">Kirim</button>

<h3>Rata-rata Rating: <span id="average">0</span> ⭐</h3>

<div id="reviewList"></div>

</div>
</section>

<script>
let selectedRating = 0;

function setRating(star){
    selectedRating = star;
    document.getElementById("ratingText").innerHTML =
        "Rating dipilih: " + star + " ⭐";
}

function submitReview(){
    let comment = document.getElementById("commentInput").value.trim();

    if(selectedRating === 0){
        alert("Silakan pilih rating dulu!");
        return;
    }

    if(comment === ""){
        alert("Isi komentar dulu ya!");
        return;
    }

    let reviews = JSON.parse(localStorage.getItem("reviews")) || [];

    reviews.push({
        rating: selectedRating,
        comment: comment
    });

    localStorage.setItem("reviews", JSON.stringify(reviews));

    document.getElementById("commentInput").value = "";
    selectedRating = 0;
    document.getElementById("ratingText").innerHTML = "";

    displayReviews();
}

function displayReviews(){
    let reviews = JSON.parse(localStorage.getItem("reviews")) || [];
    let reviewList = document.getElementById("reviewList");
    let total = 0;

    reviewList.innerHTML = "";

    reviews.forEach(r=>{
        total += r.rating;
        reviewList.innerHTML += `
        <div class="review-box">
            <strong>${r.rating} ⭐</strong>
            <p>${r.comment}</p>
        </div>`;
    });

    let average = reviews.length ? (total/reviews.length).toFixed(1) : 0;
    document.getElementById("average").innerText = average;
}

window.onload = displayReviews;
</script>

</body>
</html>