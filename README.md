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

        /* --- AUTH --- */
        #login-screen { position: fixed; inset: 0; z-index: 9999; background: #000; display: flex; align-items: center; justify-content: center; }
        .login-card { background: var(--card); border: 1px solid var(--border); padding: 40px; border-radius: 30px; width: 380px; text-align: center; }
        .login-card input { width: 100%; padding: 14px; margin-bottom: 12px; border-radius: 12px; border: 1px solid var(--border); background: #000; color: white; text-align: center; outline: none; }
        .btn-main { width: 100%; padding: 14px; border-radius: 12px; border: none; font-weight: bold; cursor: pointer; margin-bottom: 10px; }
        .btn-login { background: white; color: black; }
        .btn-reg { background: transparent; border: 1px solid var(--gold); color: var(--gold); }

        /* --- INTRO --- */
        #intro-screen { position: fixed; inset: 0; z-index: 10000; background: black; display: none; flex-direction: column; align-items: center; justify-content: center; }
        #intro-video { width: 450px; border-radius: 20px; box-shadow: 0 0 40px var(--gold); }
        #welcome-text { margin-top: 25px; font-size: 2rem; font-weight: 900; color: var(--gold); display: none; }

        /* --- LAYOUT --- */
        #main-app { display: none; height: 100vh; width: 100%; flex-direction: row; }
        
        /* SIDEBAR (6 MENU LENGKAP) */
        .sidebar { width: 110px; background: #0a0a0a; border-right: 1px solid var(--border); display: flex; flex-direction: column; align-items: center; padding: 30px 0; gap: 30px; flex-shrink: 0; overflow-y: auto; }
        .nav-btn { color: #444; cursor: pointer; transition: 0.3s; display: flex; flex-direction: column; align-items: center; gap: 4px; width: 100%; padding: 10px 0; }
        .nav-btn:hover, .nav-btn.active { color: var(--gold); background: rgba(255,215,0,0.05); }
        .nav-btn span { font-size: 9px; font-weight: 900; }

        /* CONTENT */
        .content { flex: 1; padding: 40px; overflow-y: auto; background: var(--bg); }
        .tab-content { display: none; animation: fadeIn 0.4s ease; }
        .tab-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }

        /* PROFILE HOME */
        .profile-card { display: flex; align-items: center; gap: 25px; background: var(--card); padding: 25px; border-radius: 25px; border: 1px solid var(--border); margin-bottom: 30px; }
        .p-photo { width: 100px; height: 100px; border-radius: 50%; border: 3px solid var(--gold); background: #222 url('https://via.placeholder.com/150/FFD700/000000?text=DANIEL') center/cover; }
        .follow-btn { margin-top: 10px; padding: 8px 20px; border-radius: 8px; border: none; font-weight: bold; cursor: pointer; transition: 0.2s; }
        .f-on { background: var(--gold); color: black; }
        .f-off { background: #333; color: white; }

        /* STATS */
        .stat-grid { display: flex; gap: 30px; margin-top: 20px; }
        .stat-box { background: var(--card); padding: 20px; border-radius: 20px; border: 1px solid var(--border); flex: 1; text-align: center; }
        .stat-box h1 { color: var(--gold); font-size: 2.5rem; }

        .card { background: var(--card); border: 1px solid var(--border); padding: 20px; border-radius: 20px; margin-bottom: 20px; }
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="login-card">
            <h1 style="color: var(--gold); margin-bottom: 15px;">AUREATE</h1>
            <p id="auth-info" style="font-size: 11px; margin-bottom: 15px; color: #666;">Daniel Dolar System v.7</p>
            <input type="text" id="name-in" placeholder="Username">
            <input type="password" placeholder="Password">
            <button class="btn-main btn-login" onclick="login()">LOGIN</button>
            <button class="btn-main btn-reg" onclick="register()">DAFTAR</button>
        </div>
    </div>

    <div id="intro-screen">
        <video id="vid" muted><source src="https://www.w3schools.com/html/mov_bbb.mp4" type="video/mp4"></video>
        <div id="welcome-text">WELCOME MASTER DANIEL</div>
    </div>

    <div id="main-app">
        <aside class="sidebar">
            <div class="nav-btn active" onclick="tab('home', this)"><i data-lucide="home"></i><span>HOME</span></div>
            <div class="nav-btn" onclick="tab('feed', this)"><i data-lucide="layout"></i><span>FEED</span></div>
            <div class="nav-btn" onclick="tab('chat', this)"><i data-lucide="message-square"></i><span>CHAT</span></div>
            <div class="nav-btn" onclick="tab('photo', this)"><i data-lucide="image"></i><span>PHOTO</span></div>
            <div class="nav-btn" onclick="tab('music', this)"><i data-lucide="music"></i><span>MUSIC</span></div>
            <div id="adm-nav" class="nav-btn hidden" style="color:red;" onclick="tab('users', this)"><i data-lucide="shield-check"></i><span>USERS</span></div>
            <div class="nav-btn" style="margin-top:auto;" onclick="location.reload()"><i data-lucide="power"></i><span>OUT</span></div>
        </aside>

        <main class="content">
            <section id="tab-home" class="tab-content active">
                <div class="profile-card">
                    <div class="p-photo"></div>
                    <div>
                        <h2 id="user-display">Daniel Dolar</h2>
                        <p style="color: #555;">Developer Mode Active</p>
                        <button id="f-btn" class="follow-btn f-off" onclick="handleFollow()">+ FOLLOW</button>
                    </div>
                </div>
                <h3>Live Analytics</h3>
                <div class="stat-grid">
                    <div class="stat-box">
                        <p style="font-size: 10px; font-weight: bold;">FOLLOWERS</p>
                        <h1 id="stat-f">0</h1>
                    </div>
                    <div class="stat-box">
                        <p style="font-size: 10px; font-weight: bold;">RATING</p>
                        <h1 id="stat-r">0</h1>
                    </div>
                </div>
            </section>

            <section id="tab-feed" class="tab-content">
                <div class="card">
                    <textarea id="p-in" style="width:100%; background:transparent; border:none; color:white; outline:none;" placeholder="Tulis sesuatu Daniel..."></textarea>
                    <button class="btn-main btn-login" style="width:auto; padding:5px 20px; margin-top:10px;" onclick="addPost()">KIRIM</button>
                </div>
                <div id="feed-wall"></div>
            </section>

            <section id="tab-chat" class="tab-content">
                <div id="chat-wall" class="card" style="height: 400px; overflow-y: auto;"></div>
                <div style="display: flex; gap: 10px;">
                    <input type="text" id="c-in" style="flex:1; padding:15px; border-radius:12px; background:#111; border:1px solid var(--border); color:white;" placeholder="Pesan...">
                    <button class="btn-main btn-login" style="width:auto; padding:0 20px;" onclick="sendChat()">SEND</button>
                </div>
            </section>

            <section id="tab-photo" class="tab-content">
                <div style="display:flex; justify-content:space-between; margin-bottom:20px;">
                    <h2>Cloud Gallery</h2>
                    <button class="btn-main btn-login" style="width:auto; padding:5px 20px;">UPLOAD</button>
                </div>
                <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap:10px;">
                    <div style="height:150px; background:#222; border-radius:15px;"></div>
                    <div style="height:150px; background:#222; border-radius:15px;"></div>
                </div>
            </section>

            <section id="tab-music" class="tab-content">
                <h2>Music Player</h2>
                <div class="card" style="display:flex; align-items:center; gap:20px; margin-top:20px;">
                    <i data-lucide="play-circle" style="width:50px; height:50px; color:var(--gold);"></i>
                    <div>
                        <p>Aureate Symphony No. 1</p>
                        <small style="color:#555;">Playing Now...</small>
                    </div>
                </div>
            </section>

            <section id="tab-users" class="tab-content">
                <h2 style="color:red;">User Database</h2>
                <div id="user-list"></div>
            </section>
        </main>
    </div>

    <script>
        lucide.createIcons();
        let uName = "";
        let users = [{name: "Daniel", role: "admin"}];
        let posts = [];
        let fCount = 0;
        let fClicks = 0;

        function register() {
            const n = document.getElementById('name-in').value.trim();
            if(!n) return alert("Isi Nama!");
            if(users.find(u => u.name.toLowerCase() === n.toLowerCase())) return alert("Nama sudah ada!");
            users.push({name: n, role: "user"});
            document.getElementById('auth-info').innerText = "Berhasil Daftar! Silakan Login.";
            document.getElementById('auth-info').style.color = "lime";
        }

        function login() {
            const n = document.getElementById('name-in').value.trim();
            const user = users.find(u => u.name.toLowerCase() === n.toLowerCase());
            if(!user) {
                document.getElementById('auth-info').innerText = "NAMA TIDAK ADA. SILAKAN DAFTAR!";
                document.getElementById('auth-info').style.color = "red";
                return;
            }
            uName = user.name;
            document.getElementById('user-display').innerText = uName;
            if(user.role === "admin") document.getElementById('adm-nav').classList.remove('hidden');

            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('intro-screen').style.display = 'flex';
            document.getElementById('vid').play();

            setTimeout(() => { document.getElementById('welcome-text').style.display = 'block'; }, 7000);
            setTimeout(() => {
                document.getElementById('intro-screen').style.display = 'none';
                document.getElementById('main-app').style.display = 'flex';
                update();
            }, 10000);
        }

        function handleFollow() {
            const btn = document.getElementById('f-btn');
            fClicks++;
            if(fClicks === 1) { 
                fCount = 1; btn.innerText = "✓ UNFOLLOW"; btn.className = "follow-btn f-on"; 
            } else { 
                fCount = 0; btn.innerText = "+ FOLLOW"; btn.className = "follow-btn f-off"; fClicks = 0; 
            }
            update();
        }

        function tab(id, el) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById('tab-' + id).classList.add('active');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            el.classList.add('active');
            lucide.createIcons();
        }

        function update() {
            document.getElementById('stat-f').innerText = fCount;
            document.getElementById('stat-r').innerText = posts.reduce((s, p) => s + p.likes, 0);
            const list = document.getElementById('user-list');
            list.innerHTML = users.map(u => `<div class="card">${u.name} (${u.role})</div>`).join('');
        }

        function addPost() {
            const t = document.getElementById('p-in').value;
            if(!t) return;
            posts.unshift({ id: Date.now(), author: uName, text: t, likes: 0, clicks: 0 });
            document.getElementById('p-in').value = "";
            renderFeed();
            update();
        }

        function renderFeed() {
            const wall = document.getElementById('feed-wall');
            wall.innerHTML = posts.map(p => `
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
            else { p.likes -= 1; p.clicks = 0; }
            renderFeed();
            update();
        }

        function sendChat() {
            const i = document.getElementById('c-in');
            if(!i.value) return;
            document.getElementById('chat-wall').innerHTML += `<div style="padding:10px; background:#222; margin-bottom:10px; border-radius:10px;">${i.value}</div>`;
            i.value = "";
        }
    </script>
</body>
</html>
