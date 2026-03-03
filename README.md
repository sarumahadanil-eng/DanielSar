<!DOCTYPE html>
<html>
<head>
    <title>Website Daniel</title>
    <style>
        body { font-family: Arial; margin: 0; }
        nav { background: #333; padding: 10px; }
        nav button {
            background: white;
            border: none;
            padding: 10px;
            margin: 5px;
            cursor: pointer;
        }
        section {
            display: none;
            padding: 20px;
        }
        section.active {
            display: block;
        }
    </style>
</head>
<body>

<nav>
    <button onclick="showPage('home')">Home</button>
    <button onclick="showPage('tentang')">Tentang</button>
    <button onclick="showPage('kontak')">Kontak</button>
    <button onclick="showPage('rating')">Rating</button>
</nav>

<section id="home" class="active">
    <h2>Home</h2>
    <p>Selamat datang di website saya</p>
</section>

<section id="tentang">
    <h2>Tentang</h2>
    <p>Ini halaman tentang saya</p>
</section>

<section id="kontak">
    <h2>Kontak</h2>
    <p>Email: contoh@email.com</p>
</section>

<section id="rating">
    <h2>Rating</h2>
    <p>Beri rating website ini</p>
</section>

<script>
function showPage(pageId) {
    let pages = document.querySelectorAll("section");
    pages.forEach(page => {
        page.classList.remove("active");
    });
    document.getElementById(pageId).classList.add("active");
}
</script>

</body>
</html>