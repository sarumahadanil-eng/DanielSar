<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>DANIEL NOXCTRYA | CEO</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-firestore.js"></script>
    
    <style>
        /* AUREATE OVERLORD ENGINE - INFINITY MAX 10^9 */
        :root {
            --pure-gold: #FFD700;
            --dark-gold: #B8860B;
            --void-black: #000000;
            --ethereal-glow: rgba(255, 215, 0, 0.4);
        }

        * { cursor: crosshair; box-sizing: border-box; }
        
        body { 
            background: var(--void-black); 
            color: #ffffff; 
            margin: 0; 
            padding: 80px 20px; 
            font-family: 'Times New Roman', serif;
            overflow-x: hidden;
        }

        .throne-container {
            width: 100%;
            max-width: 1600px;
            margin: auto;
            border: 2px solid var(--dark-gold);
            background: rgba(5, 5, 5, 0.98);
            box-shadow: 0 0 120px var(--ethereal-glow), inset 0 0 50px rgba(184, 134, 11, 0.15);
            position: relative;
        }

        /* Header Absolute */
        .header-overlord {
            background: linear-gradient(180deg, #0a0a0a 0%, #000 100%);
            padding: 130px 20px;
            text-align: center;
            border-bottom: 5px solid var(--pure-gold);
            position: relative;
        }

        .ceo-name-max {
            font-size: 85px;
            font-weight: 900;
            color: var(--pure-gold);
            letter-spacing: 35px;
            margin: 0;
            text-transform: uppercase;
            text-shadow: 0 0 30px var(--pure-gold), 0 0 60px var(--dark-gold);
        }

        /* Sidebar: Command Core */
        .sidebar-core {
            background: #000;
            border-right: 1px solid var(--dark-gold);
            vertical-align: top;
            padding: 60px 20px;
        }

        .nav-link-infinity {
            display: block;
            padding: 30px;
            color: #777;
            text-decoration: none;
            font-size: 15px;
            font-weight: bold;
            letter-spacing: 8px;
            transition: 0.7s cubic-bezier(0.19, 1, 0.22, 1);
            margin-bottom: 15px;
            text-transform: uppercase;
            border-left: 0px solid var(--pure-gold);
        }

        .nav-link-infinity:hover {
            color: #fff;
            background: rgba(184, 134, 11, 0.1);
            border-left: 10px solid var(--pure-gold);
            padding-left: 50px;
            text-shadow: 0 0 20px #fff;
        }

        /* Content Area */
        .content-overlord {
            padding: 100px;
            vertical-align: top;
            background: radial-gradient(circle at 50% 0%, #0f0f0f 0%, #000 100%);
        }

        .title-aura {
            font-size: 55px;
            color: #fff;
            margin: 0 0 50px 0;
            letter-spacing: 12px;
            text-transform: uppercase;
            border-left: 20px solid var(--pure-gold);
            padding-left: 35px;
        }

        /* Terminal Firebase Infinity */
        .infinity-terminal {
            background: #050505;
            border: 1px solid var(--dark-gold);
            padding: 60px;
            margin-top: 60px;
            box-shadow: 0 40px 80px rgba(0,0,0,0.9);
        }

        .input-overlord {
            width: 100%;
            background: #000;
            border: 1px solid #222;
            color: var(--pure-gold);
            padding: 35px;
            font-size: 24px;
            font-family: inherit;
            margin: 35px 0;
            outline: none;
            transition: 0.6s;
        }

        .input-overlord:focus {
            border-color: var(--pure-gold);
            box-shadow: 0 0 40px rgba(255, 215, 0, 0.15);
        }

        .btn-overlord {
            background: linear-gradient(45deg, var(--dark-gold), var(--pure-gold));
            color: #000;
            border: none;
            padding: 35px;
            font-weight: 900;
            letter-spacing: 20px;
            cursor: pointer;
            width: 100%;
            transition: 0.5s;
            text-transform: uppercase;
            font-size: 20px;
        }

        .btn-overlord:hover {
            background: #fff;
            box-shadow: 0 0 80px #fff;
            letter-spacing: 25px;
        }

        /* Footer: Eternal Domain */
        .footer-eternal {
            padding: 120px;
            text-align: center;
            background: #000;
            border-top: 1px solid var(--dark-gold);
        }

        .social-infinity {
            color: var(--dark-gold);
            text-decoration: none;
            margin: 0 50px;
            font-size: 16px;
            letter-spacing: 7px;
            font-weight: bold;
            transition: 0.4s;
        }

        .social-infinity:hover { color: var(--pure-gold); text-shadow: 0 0 25px var(--pure-gold); }
    </style>
</head>
<body>

    <table class="throne-container" border="0" cellpadding="0" cellspacing="0">
        <tr>
            <td colspan="2" class="header-overlord">
                <h1 class="ceo-name-max">DANIEL NOXCYTRA CYBER</h1>
                <p style="color: #444; letter-spacing: 25px; margin-top: 30px; font-size: 18px; font-weight: 900;">
                    CEO | MANSA GROUP
                </p>
            </td>
        </tr>

        <tr>
            <td width="22%" class="sidebar-core">
                <center>
                    <div style="padding: 15px; border: 2px solid var(--pure-gold); background: #000; box-shadow: 0 0 60px rgba(255, 215, 0, 0.5);">
                        <img src="danieldolar.png" alt="OVERLORD" width="300" style="display: block;">
                    </div>
                    <br><br><br>
                    <font size="3" color="#1a1a1a" style="letter-spacing: 12px;">SYSTEM CONTROL</font>
                    <hr color="#111" width="60%" style="margin: 60px 0;">
                </center>
                
                <a href="HOME.html" class="nav-link-infinity">1.HOME</a>
                <a href="PROFILE.html" class="nav-link-infinity">2.PROFILE</a>
                <a href="ACTIVITY.html" class="nav-link-infinity">3.ACTIVITY</a>
                <a href="GALLERY.html" class="nav-link-infinity">4.GALLERY</a>
                <a href="POETRY.html" class="nav-link-infinity">5.POETRY</a>
                <a href="mailto:danieldolarsarumaha14@gmail.com" class="nav-link-infinity">6.EMAIL</a>
            </td>

            <td width="78%" class="content-overlord">
                <h2 class="title-aura">Wellcome</h2>
                
                <p style="font-size: 30px; line-height: 2.5; color: #666; text-align: justify; font-weight: 300;">
                    Selamat datang di perusahaan mansa group dengan CEO terbaik atas nama <b>Daniel Noxctrya Cyber</b>. 
                    Seluruh infrastruktur data di sini berada dalam otoritas penuh CEO. 
                    Platform ini beroperasi dalam sinkronisasi instan dengan <b>Firebase Cloud Engine</b>.
                </p>

                <div class="infinity-terminal">
                    <h3 style="color: var(--pure-gold); margin: 0; letter-spacing: 15px; font-size: 24px;">INFINITY SYNC CORE</h3>
                    <p style="color: #333; font-size: 14px; margin-top: 15px; letter-spacing: 4px;">TRANSMIT YOUR SUPREME COMMAND TO THE CLOUD.</p>
                    
                    <textarea id="infinityInput" class="input-overlord" rows="4" placeholder="ENTER YOUR VISION..."></textarea>
                    
                    <button class="btn-overlord" onclick="transmitInfinity()">TRANSMIT TO INFINITY</button>
                    
                    <p id="infinity-msg" style="color: #00FF00; display: none; margin-top: 50px; font-weight: 900; text-align: center; letter-spacing: 12px; text-shadow: 0 0 25px #00FF00;">
                        [ STATUS ] : TRANSMISSION ABSOLUTE. DATA ETERNALIZED IN FIREBASE.
                    </p>
                </div>

                <table border="0" width="100%" cellspacing="35" style="margin-top: 100px;">
                    <tr>
                        <td align="center" bgcolor="#000" style="padding: 70px; border: 1px solid #1a1a1a; transition: 0.8s;" onmouseover="this.style.borderColor='#FFD700'" onmouseout="this.style.borderColor='#1a1a1a'">
                            <a href="Status.html" style="color: #fff; text-decoration: none; letter-spacing: 15px; font-size: 20px; font-weight: 900;">DOMAIN STATUS</a>
                        </td>
                        <td align="center" bgcolor="#000" style="padding: 70px; border: 1px solid #1a1a1a; transition: 0.8s;" onmouseover="this.style.borderColor='#FFD700'" onmouseout="this.style.borderColor='#1a1a1a'">
                            <a href="Skill.html" style="color: #fff; text-decoration: none; letter-spacing: 15px; font-size: 20px; font-weight: 900;">INFINITE SKILLS</a>
                        </td>
                    </tr>
                </table>
            </td>
        </tr>

        <tr>
            <td colspan="2" class="footer-eternal">
                <div style="margin-bottom: 70px;">
                    <a href="https://api.whatsapp.com/send?phone=628226150043" class="social-infinity">WHATSAPP SUPREME</a>
                    <a href="https://instagram.com/daniel_sarumaha" class="social-infinity">INSTAGRAM</a>
                    <a href="#" class="social-infinity">GLOBAL ARCHIVE</a>
                </div>
                <p style="font-size: 14px; color: #111; letter-spacing: 12px; font-weight: 900;">
                    &copy; 2026 AUREATE INFINITY | THE ABSOLUTE SIGNATURE OF DANIEL DOLAR
                </p>
            </td>
        </tr>
    </table>

    <script>
        // --- FIREBASE INFINITY CONFIGURATION (DANIEL'S REAL CONFIG) ---
        const firebaseConfig = {
            apiKey: "AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
            authDomain: "danieldolar-9bca1.firebaseapp.com",
            projectId: "danieldolar-9bca1",
            storageBucket: "danieldolar-9bca1.firebasestorage.app",
            messagingSenderId: "4879222744",
            appId: "1:4879222744:web:e441fe6b15b34fb42314ad",
            measurementId: "G-G5WWSCH9BH"
        };

        // Inisialisasi Sistem
        if (!firebase.apps.length) {
            firebase.initializeApp(firebaseConfig);
        }
        const db = firebase.firestore();

        function transmitInfinity() {
            const data = document.getElementById('infinityInput').value;
            const msg = document.getElementById('infinity-msg');

            if (data.trim() === "") {
                alert("The Overlord cannot transmit void.");
                return;
            }

            // Kirim ke koleksi 'aktivitas_ceo' milik Daniel
            db.collection("aktivitas_ceo").add({
                command: data,
                timestamp: firebase.firestore.FieldValue.serverTimestamp(),
                owner: "Daniel Dolar",
                status: "Absolute"
            })
            .then(() => {
                msg.style.display = "block";
                document.getElementById('infinityInput').value = "";
                setTimeout(() => { msg.style.display = "none"; }, 8000);
            })
            .catch((e) => { 
                console.error("Critical System Error: ", e);
                alert("Koneksi Firebase gagal. Pastikan Firestore 'Rules' kamu sudah diatur ke 'true'.");
            });
        }
    </script>
</body>
</html>
