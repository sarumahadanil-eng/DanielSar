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
        }

        .profile-pic {
            width: 130px;
            height: 130px;
            border: 4px solid white;
            border-radius: 50%; /* Dibuat bulat agar lebih modern */
            object-fit: cover;
            margin-bottom: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .sidebar h2 { font-size: 1.2em; margin-bottom: 20px; text-align: center; }

        .menu-nav {
            width: 100%;
            list-style: none;
            padding: 0;
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

        /* Menyembunyikan section yang tidak aktif */
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
        }

        h2 { border-bottom: 2px solid #fff; padding-bottom: 10px; margin-top: 0; }

        /* Gaya Khusus Galeri */
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

        /* Responsif */
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
        <img src="DANIEL.jpeg" alt="Daniel Sarumaha" class="profile-pic">
        <h2>Daniel Noxctrya</h2>
        <ul class="menu-nav">
            <li><a onclick="showSection('home')">Home</a></li>
            <li><a onclick="showSection('profile')">Profile</a></li>
            <li><a onclick="showSection('aktivitas')">Aktivitas</a></li>
            <li><a onclick="showSection('gambar')">Gambar</a></li>
            <li><a onclick="showSection('puisi')">Puisi</a></li>
            <li><a href="mailto:danieldolarsarumaha14@gmail.com">Email</a></li>
        </ul>
    </aside>

    <!-- Konten -->
    <main class="main-content">
        
        <!-- Section: HOME -->
        <section id="home" class="active">
            <div class="content-card" style="text-align: center;">
                <h1>WELCOME TO MY WEBSITE</h1>
                <hr color="white">
                <p><i>Saya ucapkan terimakasih atas dukungan anda dan kunjungan anda terhadap website saya.</i></p>
                <div style="margin-top: 30px;">
                    <p><strong>Aku dan Tuhanku</strong></p>
                    <i>ketika aku memandang langit...<br>
                    aku bertanya pada diriku sendiri...<br>
                    siapakah aku sebenarnya?<br>
                    darimanakah aku berasal?</i>
                </div>
            </div>
        </section>

        <!-- Section: PROFILE -->
        <section id="profile">
            <div class="content-card">
                <h2>Profile Daniel</h2>
                <p>Nama Lengkap: <strong>Daniel Dolar Sarumaha</strong></p>
                <p>Saya adalah seorang pengembang web yang berdedikasi untuk menciptakan website berstandar internasional. Fokus utama saya adalah pada fungsionalitas dan estetika digital.</p>
                <p><strong>Media Sosial:</strong><br>
                Instagram: @DanielDolarSar<br>
                Facebook: Daniel Sarumaha<br>
				Whatsapp : +6282261500043</p>
            </div>
        </section>

        <!-- Section: AKTIVITAS -->
        <section id="aktivitas">
            <div class="content-card">
                <h2>Aktivitas Terbaru</h2>
                <ul>
                    <li><strong>Proyek Website:</strong> menciptakan sebuah website standar ddengan menggunakan css</li>
                    <li><strong>Edukasi:</strong> Mendalami teknik akselerasi memori untuk memahami logika pemrograman lebih cepat.</li>
                    <li><strong>Teknologi:</strong> Eksperimen infrastruktur database menggunakan Firebase.</li>
                </ul>
            </div>
        </section>

        <!-- Section: GAMBAR -->
        <section id="gambar">
            <div class="content-card">
                <h2>Galeri Gambar</h2>
                    <img src="dolar.jpeg"height="500"width="300">
					<img src="sarumaha.jpeg"height="500"width="300">
            </div>
        </section>

        <!-- Section: PUISI -->
        <section id="puisi">
            <div class="content-card">
                <h2 style="text-align: center;">Karya Puisi</h2>
                <div style="font-style: italic; text-align: center;">
                    <p><strong>Aku dan Tuhanku</strong></p>
                    ketika aku memandang langit<br>
                    aku bertanya pada diriku sendiri<br>
                    aku....<br><br>
                    siapakah aku sebenarnya?<br>
                    darimanakah aku berasal?<br>
                    jauhkah aku dari tuhanku?<br>
                    aku...<br><br>
                    Tuhan....<br>
                    betapa kuasanya engkau<br>
                    menciptakan langit dan bumi<br>
                    untuk menghidupi orang-orang<br>
                    seperti aku
                </div>
				<tr>
				<div style="font-style:italic;text-align:center;">
					<p><strong>IBU</strong></p>
					Di matamu, aku menemukan telaga teduh<br>
					Tempatku membasuh segala lelah dan keluh<br>
					Tak perlu kata, kau tahu kapan hatiku rapuh<br>
					Hanya dengan usapan, duniaku kembali utuh.<br>
					<br>
					Kau adalah doa yang terbang paling pagi<br>
					Mengetuk pintu langit sebelum matahari bersemi<br>
					Meminta keselamatan bagi langkahku yang seringkali<br>
					Lupa jalan pulang karena terlalu sibuk mencari diri.<br>
					<br>
					Ibu, kau adalah rumah yang tak pernah terkunci<br>
					Meski aku pergi jauh dan berkali-kali menyakiti<br>
					Kasihmu tak berkurang, tetap tulus dan suci<br>
					Menjadi kompas saat aku kehilangan arah di bumi.<br>
            </div>
        </section>
    </main>
</div>
<script>
    function showSection(sectionId) {
        // Sembunyikan semua section
        const sections = document.querySelectorAll('section');
        sections.forEach(section => {
            section.classList.remove('active');
        });

        // Tampilkan section yang dipilih
        const activeSection = document.getElementById(sectionId);
        activeSection.classList.add('active');
    }
</script>

</body>
</html>
