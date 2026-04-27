<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AUREATE PRESTIGE V9 - DANIEL DOLAR</title>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        :root { --gold: #FFD700; --bg: #050505; --card: #111; --border: rgba(255,255,255,0.1); }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background: var(--bg); color: white; overflow: hidden; height: 100vh; }

        /* --- LOGIN WITH FLOATING LABELS --- */
        #login-screen { position: fixed; inset: 0; z-index: 9999; background: #000; display: flex; align-items: center; justify-content: center; }
        .login-card { background: var(--card); border: 1px solid var(--border); padding: 40px; border-radius: 30px; width: 380px; text-align: center; }
        .input-group { position: relative; margin-bottom: 25px; width: 100%; }
        .input-group input { width: 100%; padding: 15px; border-radius: 12px; border: 1px solid var(--border); background: #000; color: white; outline: none; font-size: 1rem; }
        .input-group label { position: absolute; left: 15px; top: 50%; transform: translateY(-50%); color: #666; pointer-events: none; transition: 0.3s; }
        .input-group input:focus ~ label, .input-group input:not(:placeholder-shown) ~ label { top: -10px; left: 10px; font-size: 0.75rem; color: var(--gold); background: var(--card); padding: 0 5px; }
        .btn-main { width: 100%; padding: 14px; border-radius: 12px; border: none; font-weight: bold; cursor: pointer; margin-bottom: 10px; }

        /* --- LAYOUT --- */
        #main-app { display: none; height: 100vh; width: 100%; flex-direction: row; }
        .sidebar { width: 100px; background: #0a0a0a; border-right: 1px solid var(--border); display: flex; flex-direction: column; align-items: center; padding: 30px 0; gap: 35px; flex-shrink: 0; }
        .nav-btn { color: #444; cursor: pointer; transition: 0.3s; display: flex; flex-direction: column; align-items: center; gap: 5px; width: 100%; }
        .nav-btn:hover, .nav-btn.active { color: var(--gold); }
        .nav-btn span { font-size: 9px; font-weight: bold; }

        .content { flex: 1; padding: 40px; overflow-y: auto; background: var(--bg); }
        .tab-content { display: none; animation: fadeIn 0.4s ease; }
        .tab-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* --- DASHBOARD 10 STATS --- */
        .profile-header { display: flex; align-items: center; gap: 20px; margin-bottom: 30px; }
        .p-img { width: 80px; height: 80px; border-radius: 50%; border: 2px solid var(--gold); background: #222; }
        .stat-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 15px; margin-top: 20px; }
        .stat-box { background: var(--card); padding: 20px; border-radius: 15px; border: 1px solid var(--border); text-align: center; }
        .stat-box h2 { color: var(--gold); font-size: 1.5rem; margin-top: 5px; }
        .stat-box p { font-size: 9px; color: #666; font-weight: bold; text-transform: uppercase; }

        .card { background: var(--card); border: 1px solid var(--border); padding: 20px; border-radius: 20px; margin-bottom: 15px; }
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="login-card">
            <h1 style="color:var(--gold); margin-bottom:30px;">AUREATE</h1>
            <p id="auth-msg" style="font-size:10px; color:#555; margin-bottom:20px;">SILAKAN LOGIN ATAU DAFTAR</p>
            <div class="input-group">
                <input type="text" id="n-in" placeholder=" ">
                <label>Nama Lengkap</label>
            </div>
            <div class="input-group">
                <input type="password" id="p-in" placeholder=" ">
                <label>Password</label>
            </div>
            <button class="btn-main" style="background:white; color:black;" onclick="login()">LOGIN</button>
            <button class="btn-main" style="background:transparent; border:1px solid var(--gold); color:var(--gold);" onclick="register()">DAFTAR</button>
        </div>
    </div>

    <div id="main-app">
        <aside class="sidebar">
            <div class="nav-btn active" onclick="tab('home', this)"><i data-lucide="home"></i><span>HOME</span></div>
            <div class="nav-btn" onclick="tab('feed', this)"><i data-lucide="layout"></i><span>FEED</span></div>
            <div class="nav-btn" onclick="tab('chat', this)"><i data-lucide="message-square"></i><span>CHAT</span></div>
            <div class="nav-btn" onclick="tab('photo', this)"><i data-lucide="image"></i><span>PHOTO</span></div>
            <div id="adm-nav" class="nav-btn hidden" style="color:red;" onclick="tab('users', this)"><i data-lucide="shield-check"></i><span>USERS</span></div>
            <div class="nav-btn" style="margin-top:auto;" onclick="location.reload()"><i data-lucide="power"></i><span>EXIT</span></div>
        </aside>

        <main class="content">
            <section id="tab-home" class="tab-content active">
                <div class="profile-header">
                    <div class="p-img" style="background-image: url('https://via.placeholder.com/100/FFD700/000000?text=D'); background-size: cover;"></div>
                    <div>
                        <h2 id="u-display">Daniel Dolar</h2>
                        <button id="f-btn" style="padding:5px 15px; border-radius:5px; border:none; background:#333; color:white; font-size:11px; cursor:pointer; margin-top:5px;" onclick="handleFollow()">+ FOLLOW</button>
                    </div>
                </div>

                <h3>System Dashboard</h3>
                <div class="stat-grid">
                    <div class="stat-box"><p>Followers</p><h2 id="s-1">0</h2></div>
                    <div class="stat-box"><p>Rating</p><h2 id="s-2">0</h2></div>
                    <div class="stat-box"><p>Postings</p><h2 id="s-3">0</h2></div>
                    <div class="stat-box"><p>Views</p><h2 id="s-4">0</h2></div>
                    <div class="stat-box"><p>Gold Coins</p><h2 id="s-5">0</h2></div>
                    <div class="stat-box"><p>User Level</p><h2 id="s-6">0</h2></div>
                    <div class="stat-box"><p>Global Rank</p><h2 id="s-7">0</h2></div>
                    <div class="stat-box"><p>Activity</p><h2 id="s-8">0%</h2></div>
                    <div class="stat-box"><p>Storage</p><h2 id="s-9">0%</h2></div>
                    <div class="stat-box"><p>Security</p><h2 id="s-10">0%</h2></div>
                </div>
            </section>

            <section id="tab-feed" class="tab-content">
                <div class="card">
                    <textarea id="feed-in" style="width:100%; background:transparent; border:none; color:white; outline:none;" placeholder="Apa yang baru hari ini?"></textarea>
                    <button class="btn-main" style="width:auto; padding:5px 20px; background:white; color:black; margin-top:10px;" onclick="addPost()">KIRIM</button>
                </div>
                <div id="feed-wall"></div>
            </section>
            
            <section id="tab-chat" class="tab-content"><h2>Secure Messaging</h2><div id="chat-wall" class="card" style="height:300px;"></div></section>
            <section id="tab-photo" class="tab-content"><h2>Cloud Gallery</h2><div class="stat-grid" id="photo-wall"></div></section>
            <section id="tab-users" class="tab-content"><h2>Admin: User Database</h2><div id="user-list"></div></section>
        </main>
    </div>

    <script>
        lucide.createIcons();
        let db = [{name: "Daniel", role: "admin"}];
        let posts = [];
        let followers = 0;
        let fClicks = 0;
        let curU = "";

        function register() {
            const n = document.getElementById('n-in').value.trim();
            if(!n) return;
            if(db.find(u => u.name.toLowerCase() === n.toLowerCase())) return alert("Sudah terdaftar!");
            db.push({name: n, role: "user"});
            document.getElementById('auth-msg').innerText = "DAFTAR BERHASIL. SILAKAN LOGIN.";
            document.getElementById('auth-msg').style.color = "lime";
        }

        function login() {
            const n = document.getElementById('n-in').value.trim();
            const user = db.find(u => u.name.toLowerCase() === n.toLowerCase());
            if(!user) {
                document.getElementById('auth-msg').innerText = "NAMA TIDAK DITEMUKAN. DAFTAR DULU!";
                document.getElementById('auth-msg').style.color = "red";
                return;
            }
            curU = user.name;
            document.getElementById('u-display').innerText = curU;
            if(user.role === "admin") document.getElementById('adm-nav').classList.remove('hidden');
            
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'flex';
            update();
        }

        function handleFollow() {
            const btn = document.getElementById('f-btn');
            fClicks++;
            if(fClicks === 1) { 
                followers = 1; btn.innerText = "UNFOLLOW"; btn.style.background = "var(--gold)"; btn.style.color = "black";
            } else { 
                followers = 0; btn.innerText = "+ FOLLOW"; btn.style.background = "#333"; btn.style.color = "white"; fClicks = 0;
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
            // Dashboard Logic
            document.getElementById('s-1').innerText = followers;
            document.getElementById('s-2').innerText = posts.reduce((s, p) => s + p.likes, 0);
            document.getElementById('s-3').innerText = posts.length;
            document.getElementById('s-4').innerText = (posts.length * 12); // Simulasi views
            document.getElementById('s-5').innerText = (followers * 100); // Simulasi koin
            document.getElementById('s-6').innerText = "1";
            document.getElementById('s-7').innerText = "#99";
            document.getElementById('s-8').innerText = "45%";
            document.getElementById('s-9').innerText = "12%";
            document.getElementById('s-10').innerText = "100%";

            document.getElementById('user-list').innerHTML = db.map(u => `<div class="card">${u.name} (${u.role})</div>`).join('');
        }

        function addPost() {
            const t = document.getElementById('feed-in').value;
            if(!t) return;
            posts.unshift({ id: Date.now(), author: curU, text: t, likes: 0, clicks: 0 });
            document.getElementById('feed-in').value = "";
            renderFeed();
            update();
        }

        function renderFeed() {
            document.getElementById('feed-wall').innerHTML = posts.map(p => `
                <div class="card">
                    <p><strong>@${p.author}</strong></p>
                    <p style="margin:10px 0; color:#ccc;">${p.text}</p>
                    <span style="cursor:pointer; color:var(--gold); font-size:12px;" onclick="likeLogic(${p.id})">❤️ ${p.likes} Likes</span>
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
    </script>
</body>
</html>
