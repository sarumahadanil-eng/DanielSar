<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AUREATE PRESTIGE - DANIEL DOLAR</title>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        :root { --gold: #FFD700; --bg: #050505; --card: #111; --border: rgba(255,255,255,0.1); }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background: var(--bg); color: white; overflow: hidden; height: 100vh; }

        /* --- AUTH SCREEN --- */
        #login-screen { position: fixed; inset: 0; z-index: 9999; background: #000; display: flex; align-items: center; justify-content: center; }
        .login-card { background: var(--card); border: 1px solid var(--border); padding: 40px; border-radius: 30px; width: 380px; text-align: center; }
        .login-card input { width: 100%; padding: 14px; margin-bottom: 12px; border-radius: 12px; border: 1px solid var(--border); background: #000; color: white; text-align: center; outline: none; }
        .btn-main { width: 100%; padding: 14px; border-radius: 12px; border: none; font-weight: bold; cursor: pointer; margin-bottom: 10px; transition: 0.3s; }
        .btn-login { background: white; color: black; }
        .btn-reg { background: transparent; border: 1px solid var(--gold); color: var(--gold); }

        /* --- INTRO --- */
        #intro-screen { position: fixed; inset: 0; z-index: 10000; background: black; display: none; flex-direction: column; align-items: center; justify-content: center; }
        #intro-video { width: 450px; border-radius: 20px; box-shadow: 0 0 40px var(--gold); }
        #welcome-text { margin-top: 25px; font-size: 2rem; font-weight: 900; color: var(--gold); display: none; }

        /* --- MAIN APP --- */
        #main-app { display: none; height: 100vh; width: 100%; flex-direction: row; }
        
        /* SIDEBAR (5 MENU LENGKAP) */
        .sidebar { width: 100px; background: #0a0a0a; border-right: 1px solid var(--border); display: flex; flex-direction: column; align-items: center; padding: 40px 0; gap: 40px; flex-shrink: 0; }
        .nav-btn { color: #555; cursor: pointer; transition: 0.3s; display: flex; flex-direction: column; align-items: center; gap: 5px; }
        .nav-btn:hover, .nav-btn.active { color: var(--gold); }
        .nav-btn span { font-size: 9px; font-weight: bold; }

        /* CONTENT */
        .content { flex: 1; padding: 50px; overflow-y: auto; background: var(--bg); }
        .tab-content { display: none; animation: fadeIn 0.4s ease; }
        .tab-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        /* HOME PROFILE SECTION */
        .profile-header { display: flex; align-items: center; gap: 30px; margin-bottom: 50px; background: var(--card); padding: 30px; border-radius: 30px; border: 1px solid var(--border); }
        .my-photo { width: 120px; height: 120px; border-radius: 50%; border: 3px solid var(--gold); background: #222; background-size: cover; background-position: center; flex-shrink: 0; }
        .profile-info h2 { font-size: 24px; color: var(--gold); }
        .follow-btn { margin-top: 15px; padding: 10px 25px; border-radius: 10px; border: none; font-weight: bold; cursor: pointer; transition: 0.2s; }
        .btn-follow-active { background: var(--gold); color: black; }
        .btn-follow-inactive { background: #333; color: white; }

        /* DASHBOARD CIRCLES */
        .stat-row { display: flex; gap: 40px; margin-bottom: 40px; }
        .circle { width: 130px; height: 130px; border-radius: 50%; border: 4px solid var(--gold); display: flex; align-items: center; justify-content: center; position: relative; }
        .circle span { font-size: 1.8rem; font-weight: 800; color: var(--gold); }

        /* CHAT & FEED CARDS */
        .card { background: var(--card); border: 1px solid var(--border); padding: 25px; border-radius: 25px; margin-bottom: 20px; }
        .chat-msg { padding: 12px; border-radius: 15px; margin-bottom: 10px; max-width: 80%; background: #222; }
        .chat-me { align-self: flex-end; background: var(--gold); color: black; margin-left: auto; }
        
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 15px; }
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="login-card">
            <h1 style="color: var(--gold); margin-bottom: 20px;">AUREATE</h1>
            <p id="auth-msg" style="font-size: 11px; margin-bottom: 15px; color: #888;">Silakan mendaftar jika baru pertama kali.</p>
            <input type="text" id="name-in" placeholder="Nama Lengkap">
            <input type="password" placeholder="Password">
            <button class="btn-main btn-login" onclick="doLogin()">LOGIN</button>
            <button class="btn-main btn-reg" onclick="doRegister()">DAFTAR</button>
        </div>
    </div>

    <div id="intro-screen">
        <video id="v-intro" muted><source src="https://www.w3schools.com/html/mov_bbb.mp4" type="video/mp4"></video>
        <div id="welcome-text">WELCOME MASTER DANIEL</div>
    </div>

    <div id="main-app">
        <aside class="sidebar">
            <div class="nav-btn active" onclick="goTab('home', this)"><i data-lucide="home"></i><span>HOME</span></div>
            <div class="nav-btn" onclick="goTab('feed', this)"><i data-lucide="layout"></i><span>FEED</span></div>
            <div class="nav-btn" onclick="goTab('chat', this)"><i data-lucide="message-square"></i><span>CHAT</span></div>
            <div class="nav-btn" onclick="goTab('photo', this)"><i data-lucide="image"></i><span>PHOTO</span></div>
            <div id="menu-admin" class="nav-btn hidden" style="color: red;" onclick="goTab('users', this)"><i data-lucide="users"></i><span>USERS</span></div>
            <div class="nav-btn" style="margin-top: auto;" onclick="location.reload()"><i data-lucide="log-out"></i><span>EXIT</span></div>
        </aside>

        <main class="content">
            <section id="tab-home" class="tab-content active">
                <div class="profile-header">
                    <div class="my-photo" id="profile-pic" style="background-image: url('https://via.placeholder.com/150/FFD700/000000?text=DANIEL');"></div>
                    <div class="profile-info">
                        <h2 id="display-name">Daniel Dolar</h2>
                        <p style="color: #666; font-size: 14px;">Owner & Developer of Aureate</p>
                        <button id="follow-btn" class="follow-btn btn-follow-inactive" onclick="followLogic()">+ FOLLOW</button>
                    </div>
                </div>

                <h3 style="margin-bottom: 25px;">Statistik Real-Time</h3>
                <div class="stat-row">
                    <div style="text-align:center;">
                        <div class="circle"><span id="stat-f">0</span></div>
                        <p style="font-size: 10px; margin-top: 10px; font-weight: bold;">FOLLOWERS</p>
                    </div>
                    <div style="text-align:center;">
                        <div class="circle"><span id="stat-r">0</span></div>
                        <p style="font-size: 10px; margin-top: 10px; font-weight: bold;">RATING LIKES</p>
                    </div>
                </div>
            </section>

            <section id="tab-feed" class="tab-content">
                <div class="card">
                    <textarea id="p-txt" style="width:100%; background:transparent; border:none; color:white; outline:none;" placeholder="Tulis sesuatu..."></textarea>
                    <button class="btn-main btn-login" style="width:auto; padding:8px 30px; margin-top:10px;" onclick="addPost()">KIRIM</button>
                </div>
                <div id="feed-container"></div>
            </section>

            <section id="tab-chat" class="tab-content">
                <div id="chat-box" class="card" style="height: 400px; overflow-y: auto; display: flex; flex-direction: column;"></div>
                <div style="display: flex; gap: 10px; margin-top: 10px;">
                    <input type="text" id="c-in" style="flex:1; padding:15px; border-radius:12px; background:#111; border:1px solid var(--border); color:white;" placeholder="Ketik pesan...">
                    <button class="btn-main btn-login" style="width:auto; padding:0 25px;" onclick="sendChat()">KIRIM</button>
                </div>
            </section>

            <section id="tab-photo" class="tab-content">
                <div style="display:flex; justify-content:space-between; margin-bottom:20px;">
                    <h2>Galeri Photo</h2>
                    <input type="file" id="up-file" class="hidden" onchange="addPhoto(event)">
                    <button class="btn-main btn-login" style="width:auto; padding:5px 20px;" onclick="document.getElementById('up-file').click()">UPLOAD</button>
                </div>
                <div id="photo-grid" class="grid"></div>
            </section>

            <section id="tab-users" class="tab-content">
                <h2 style="color:red; margin-bottom:20px;">Database Admin</h2>
                <div id="user-list-admin"></div>
            </section>
        </main>
    </div>

    <script>
        lucide.createIcons();
        let sessionUser = "";
        
        // Database
        let dbUsers = [{name: "Daniel", role: "admin"}, {name: "Daniel Dolar", role: "admin"}];
        let posts = [];
        let followers = 0;
        let followClicks = 0;

        // --- LOGIKA FOLLOW (+) KLIK 1 (+1) KLIK 2 (-1) ---
        function followLogic() {
            const btn = document.getElementById('follow-btn');
            followClicks++;

            if(followClicks === 1) {
                followers += 1;
                btn.innerText = "✓ UNFOLLOW";
                btn.classList.replace('btn-follow-inactive', 'btn-follow-active');
            } else if (followClicks === 2) {
                followers -= 1;
                btn.innerText = "+ FOLLOW";
                btn.classList.replace('btn-follow-active', 'btn-follow-inactive');
                followClicks = 0; // Reset agar bisa klik lagi
            }
            updateStats();
        }

        // --- AUTH ---
        function doRegister() {
            const n = document.getElementById('name-in').value.trim();
            if(!n) return alert("Isi nama!");
            if(dbUsers.find(u => u.name.toLowerCase() === n.toLowerCase())) {
                document.getElementById('auth-msg').innerText = "Sudah terdaftar! Langsung Login.";
            } else {
                dbUsers.push({name: n, role: "user"});
                document.getElementById('auth-msg').innerText = "Berhasil Daftar! Silakan Login.";
            }
        }

        function doLogin() {
            const n = document.getElementById('name-in').value.trim();
            const user = dbUsers.find(u => u.name.toLowerCase() === n.toLowerCase());
            if(!user) {
                document.getElementById('auth-msg').innerText = "NAMA TIDAK DITEMUKAN. DAFTAR DULU!";
                return;
            }
            sessionUser = user.name;
            document.getElementById('display-name').innerText = sessionUser;
            if(user.role === "admin") document.getElementById('menu-admin').classList.remove('hidden');

            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('intro-screen').style.display = 'flex';
            const vid = document.getElementById('v-intro'); vid.play();

            setTimeout(() => { document.getElementById('welcome-text').style.display = 'block'; }, 7000);
            setTimeout(() => {
                document.getElementById('intro-screen').style.display = 'none';
                document.getElementById('main-app').style.display = 'flex';
                updateStats();
            }, 10000);
        }

        // --- NAV & STATS ---
        function goTab(id, el) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById('tab-' + id).classList.add('active');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            el.classList.add('active');
            lucide.createIcons();
        }

        function updateStats() {
            document.getElementById('stat-f').innerText = followers; // Diambil dari klik follow profil
            let likesTotal = posts.reduce((s, p) => s + p.likes, 0);
            document.getElementById('stat-r').innerText = likesTotal;
            renderUserList();
        }

        // --- FEED & LIKE (KLIK 1 +1, KLIK 2 -1) ---
        function addPost() {
            const t = document.getElementById('p-txt').value;
            if(!t) return;
            posts.unshift({ id: Date.now(), author: sessionUser, text: t, likes: 0, clicks: 0 });
            document.getElementById('p-txt').value = "";
            renderFeed();
            updateStats();
        }

        function renderFeed() {
            const cont = document.getElementById('feed-container');
            cont.innerHTML = posts.map(p => `
                <div class="card">
                    <p><strong>@${p.author}</strong></p>
                    <p style="margin:10px 0;">${p.text}</p>
                    <span style="cursor:pointer; color:var(--gold);" onclick="likeLogic(${p.id})">❤️ ${p.likes} Likes</span>
                </div>
            `).join('');
        }

        function likeLogic(id) {
            const p = posts.find(x => x.id === id);
            p.clicks++;
            if(p.clicks === 1) p.likes += 1;
            else if(p.clicks === 2) { p.likes -= 1; p.clicks = 0; }
            renderFeed();
            updateStats();
        }

        // --- CHAT & PHOTO ---
        function sendChat() {
            const i = document.getElementById('c-in');
            const b = document.getElementById('chat-box');
            if(!i.value) return;
            b.innerHTML += `<div class="chat-msg chat-me"><small>@${sessionUser}</small><br>${i.value}<div style="font-size:8px; opacity:0.5; margin-top:5px; cursor:pointer;" onclick="this.parentElement.remove()">HAPUS</div></div>`;
            i.value = ""; b.scrollTop = b.scrollHeight;
        }

        function addPhoto(e) {
            const f = e.target.files[0];
            if(f) {
                const reader = new FileReader();
                reader.onload = (x) => { document.getElementById('photo-grid').innerHTML += `<div class="photo-item" style="background-image:url('${x.target.result}')"></div>`; };
                reader.readAsDataURL(f);
            }
        }

        function renderUserList() {
            const l = document.getElementById('user-list-admin');
            l.innerHTML = dbUsers.map(u => `<div class="card"><span>${u.name} (${u.role})</span></div>`).join('');
        }
    </script>
</body>
</html>
