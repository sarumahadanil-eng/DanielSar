<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daniel Noxctrya Cyber - Official Website</title>
    <style>
        /* --- RESET & DASAR --- */
        * { box-sizing: border-box; }
        body {
            background-color: #94FBAB;
            font-family: "Times New Roman", Times, serif;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #2c3e50;
        }

        /* --- CONTAINER UTAMA --- */
        .glass-container {
            display: flex;
            width: 95%;
            max-width: 1100px;
            height: 90vh;
            background: rgba(255, 255, 255, 0.25);
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.5);
            border-radius: 25px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        }

        /* --- SIDEBAR (MENU) --- */
        .sidebar {
            width: 260px;
            background: rgba(255, 255, 255, 0.4);
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            border-right: 1px solid rgba(255, 255, 255, 0.3);
            z-index: 10;
        }

        .profile-pic {
            width: 130px;
            height: 130px;
            border: 4px solid white;
            border-radius: 50%;
            object-fit: cover;
            margin-bottom: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .sidebar h2 { font-size: 1.2em; margin-bottom: 20px; text-align: center; }

        .menu-nav {
            width: 100%;
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .menu-nav li { margin: 8px 0; }

        .menu-nav a {
            text-decoration: none;
            color: #2c3e50;
            font-weight: bold;
            display: block;
            padding: 12px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 12px;
            text-align: center;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .menu-nav a:hover {
            background: white;
            transform: scale(1.05);
        }

        /* --- KONTEN UTAMA --- */
        .main-content {
            flex: 1;
            padding: 40px;
            overflow-y: auto;
            background: rgba(255, 255, 255, 0.1);
            scroll-behavior: smooth;
        }

        section {
            display: none; 
            animation: fadeIn 0.5s ease;
        }

        section.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .content-card {
            background: rgba(255, 255, 255, 0.6);
            padding: 30px;
            border-radius: 20px;
            line-height: 1.7;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            margin-bottom: 20px;
        }

        h2 { border-bottom: 2px solid #fff; padding-bottom: 10px; margin-top: 0; }

        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 15px;
        }
        .gallery-grid img {
            width: 100%;
            border-radius: 10px;
            border: 2px solid white;
        }

        @media (max-width: 768px) {
            .glass-container { flex-direction: column; height: auto; margin: 10px; }
            .sidebar { width: 100%; border-right: none; border-bottom: 1px solid white; }
        }
    </style>
</head>
<body>

<div class="glass-container">
    <!-- Sidebar -->
    <aside class="sidebar">
        <img src="DANIEL.jpeg" alt="Daniel" class="profile-pic">
        <h2>Daniel Noxctrya</h2>
        <ul class="menu-nav">
            <li><a onclick="showSection('home')">Home</a></li>
            <li><a onclick="showSection('profile')">Profile</a></li>
            <li><a onclick="showSection('aktivitas')">Aktivitas</a></li>
            <li><a onclick="showSection('gambar')">Gambar</a></li>
            <li><a onclick="showSection('puisi')">Puisi</a></li>
            <li><a onclick="showSection('games')">Games</a></li>
            <li><a href="mailto:danieldolarsarumaha14@gmail.com">Email</a></li>
        </ul>
    </aside>

    <!-- Konten -->
    <main class="main-content">
        
        <!-- Section: HOME -->
        <section id="home" class="active">
            <div class="content-card" style="text-align: center;">
                <h1><b>WELCOME TO DANIEL WEBSITE</b></h1>
                <hr color="white">
                <p><i>Terimakasih telah join di website seorang CEO mahal, wkwk.</i></p>
                <div style="margin-top: 30px;">
                    <p><strong>Aku dan Dia</strong></p>
                    <i>Ubur-ubur ikan lele<br>
                    Beli pulsa di terminal.<br>
                    Gaya lu udah kayak CEO kece<br>
                    Tapi login Google aja masih gagal.</i>
                </div>
            </div>
        </section>

        <!-- Section: PROFILE -->
        <section id="profile">
            <div class="content-card">
                <h2>Profile</h2>
                <p>Nama Lengkap: <strong>Daniel Dolar Sarumaha</strong></p>
                <p>Perkenalkan saya adalah Daniel. Saya semangat dalam belajar web, saat ini saya sedang mempelajari CSS maupun JS.</p>
                <p><strong>Media Sosial:</strong><br>
                    <a href="https://www.instagram.com/DanielDolarSar" target="_blank" style="text-decoration: none; color: #2c3e50;">📸 Instagram: @DanielDolarSar</a><br>
                    <a href="https://www.facebook.com/daniel.sarumaha" target="_blank" style="text-decoration: none; color: #2c3e50;">👥 Facebook: Daniel Sarumaha</a><br>
                    <a href="https://wa.me/6282261500043" target="_blank" style="text-decoration: none; color: #2c3e50;">💬 Whatsapp: +6282261500043</a>
                </p>
            </div>
        </section>

        <!-- Section: AKTIVITAS -->
        <section id="aktivitas">
            <div class="content-card">
                <h2>Aktivitas Terbaru</h2>
                <ul>
                    <li><strong>Proyek Website:</strong> Menciptakan website standar dengan CSS.</li>
                    <li><strong>Edukasi:</strong> Teknik akselerasi memori untuk logika pemrograman.</li>
                    <li><strong>Teknologi:</strong> Eksperimen database menggunakan Firebase.</li>
                </ul>
            </div>
        </section>

        <!-- Section: GAMBAR -->
        <section id="gambar">
            <div class="content-card">
                <h2>Gambar</h2>
                <div class="gallery-grid">
                    <img src="dolar.jpeg" alt="Gambar Dolar">
                    <img src="sarumaha.jpeg" alt="Gambar Sarumaha">
                </div>
            </div>
        </section>

        <!-- Section: PUISI -->
        <section id="puisi">
            <div class="content-card">
                <h2 style="text-align: center;">Karya Puisi</h2>
                <div style="font-style: italic; text-align: center; margin-bottom: 30px;">
                    <p><strong>Aku dan Tuhanku</strong></p>
                    ketika aku memandang langit<br>
                    aku bertanya pada diriku sendiri<br>
                    siapakah aku sebenarnya?<br>
                    darimanakah aku berasal?<br>
                    jauhkah aku dari tuhanku?<br><br>
                    Tuhan.... betapa kuasanya engkau
                </div>
                <hr color="white">
                <div style="font-style:italic; text-align:center; margin-top: 30px;">
                    <p><strong>IBU</strong></p>
                    Di matamu, aku menemukan telaga teduh...<br>
                    Kau adalah doa yang terbang paling pagi...<br>
                    Ibu, kau adalah rumah yang tak pernah terkunci.
                </div>
            </div>
        </section>

        <!-- Section: GAMES -->
        <section id="games">
            <div class="content-card">
                <h2>Mini Games</h2>
                <p>Mainkan game santai langsung di sini.</p>
                <div style="width: 100%; height: 500px; border-radius: 15px; overflow: hidden; border: 2px solid white; background: #eee;">
                    <!-- Menggunakan sumber game 2048 yang lebih kompatibel -->
                    <iframe src="https://2048game.com/" 
                            style="width: 100%; height: 100%; border: none;"
                            title="Game 2048">
                    </iframe>
                </div>
                <p style="margin-top: 15px; font-size: 0.9em; text-align: center;">
                    Jika game tidak muncul, <a href="https://poki.com/id" target="_blank" style="color: #2c3e50; font-weight: bold;">Klik di sini untuk main di Poki</a>
                </p>
            </div>
        </section>

    </main>
</div>

<script>
    function showSection(sectionId) {
        // Ambil semua elemen section
        const sections = document.querySelectorAll('section');
        
        // Sembunyikan semua
        sections.forEach(sec => {
            sec.classList.remove('active');
        });

        // Tampilkan yang diklik
        const target = document.getElementById(sectionId);
        if (target) {
            target.classList.add('active');
        }
    }
</script>

</body>
</html>
