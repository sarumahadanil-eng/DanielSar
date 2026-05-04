<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daniel Noxctrya Cyber</title>
    <style>
        /* Dasar Desain */
        body {
            background-color: #94FBAB;
            font-family: "Times New Roman", Times, serif;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        /* Container Utama yang Menyatukan Semuanya */
        .glass-container {
            display: flex;
            width: 90%;
            max-width: 1100px;
            height: 85vh;
            background: rgba(255, 255, 255, 0.25);
            backdrop-filter: blur(10px); /* Efek kaca kekinian */
            border: 1px solid white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }

        /* Bagan Kiri: Sidebar & Foto */
        .sidebar {
            width: 280px;
            background: rgba(255, 255, 255, 0.4);
            padding: 30px;
            display: flex;
            flex-direction: column;
            align-items: center;
            border-right: 1px solid rgba(255, 255, 255, 0.5);
        }

        .profile-pic {
            width: 150px;
            height: auto;
            border: 4px solid white;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            margin-bottom: 25px;
        }

        .menu-nav {
            width: 100%;
            list-style: none;
            padding: 0;
        }

        .menu-nav li {
            margin: 10px 0;
        }

        .menu-nav a {
            text-decoration: none;
            color: #2c3e50;
            font-weight: bold;
            display: block;
            padding: 10px;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 8px;
            transition: 0.3s;
            text-align: center;
        }

        .menu-nav a:hover {
            background: white;
            transform: translateX(5px);
        }

        /* Bagan Kanan: Konten Utama */
        .main-content {
            flex: 1;
            padding: 40px;
            overflow-y: auto; /* Agar bisa di-scroll jika teks panjang */
            background: rgba(255, 255, 255, 0.1);
        }

        .welcome-header {
            text-align: center;
            border-bottom: 2px solid white;
            padding-bottom: 15px;
            margin-bottom: 25px;
        }

        .welcome-header h1 {
            margin: 0;
            letter-spacing: 2px;
            font-size: 1.8em;
        }

        .content-body {
            background: rgba(255, 255, 255, 0.5);
            padding: 25px;
            border-radius: 15px;
        }

        .poem-text {
            line-height: 1.8;
            font-size: 1.1em;
            text-align: center;
        }

        /* Responsif untuk layar HP */
        @media (max-width: 768px) {
            .glass-container {
                flex-direction: column;
                height: auto;
                margin: 20px 0;
            }
            .sidebar {
                width: auto;
                border-right: none;
                border-bottom: 1px solid white;
            }
        }
    </style>
</head>
<body>

<div class="glass-container">
    <!-- Bagan Sidebar -->
    <aside class="sidebar">
        <img src="danieldolar.png" alt="Daniel Photo" class="profile-pic">
        <ul class="menu-nav">
            <li><a href="profile.html">Profile</a></li>
            <li><a href="Aktivitas.html">Aktivitas</a></li>
            <li><a href="Gambar/Foto.html">Gambar/Foto</a></li>
            <li><a href="Puisi.html">Puisi</a></li>
            <li><a href="mailto:danieldolarsarumaha14@gmail.com">Email Me</a></li>
        </ul>
    </aside>

    <!-- Bagan Konten -->
    <main class="main-content">
        <header class="welcome-header">
            <h1>WELCOME TO MY WEBSITE</h1>
            <p><i>Terima kasih atas kunjungan dan dukungan Anda.</i></p>
        </header>

        <section class="content-body">
            <div class="poem-text">
                <p><strong>Aku dan Tuhanku</strong></p>
                <i>
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
                </i>
            </div>
        </section>
    </main>
</div>

</body>
</html>