<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AUREATE - Daniel Dolar Master</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root { --gold: #e5b026; --cyan: #0fe; --bg: #0a0a0c; --glass: rgba(255,255,255,0.05); }
        body, html { margin: 0; padding: 0; height: 100%; font-family: 'Segoe UI', sans-serif; background: var(--bg); color: #fff; overflow: hidden; }

        /* 1. ANIMASI BOLA */
        .ball { position: absolute; border-radius: 50%; filter: blur(70px); opacity: 0.2; z-index: -1; animation: float 20s infinite alternate; }
        @keyframes float { from { transform: translate(0,0); } to { transform: translate(90vw, 90vh); } }

        /* LOADING & EXIT */
        #loader { position: fixed; inset: 0; background: #000; display: none; flex-direction: column; justify-content: center; align-items: center; z-index: 10000; }
        .welcome-anim { font-size: 2rem; font-weight: bold; color: var(--gold); animation: pulse 2s infinite; letter-spacing: 3px; text-align: center; }
        @keyframes pulse { 0%, 100% { opacity: 0.3; transform: scale(0.9); } 50% { opacity: 1; transform: scale(1); } }

        /* LOGIN */
        #login-page { height: 100%; display: flex; justify-content: center; align-items: center; }
        .login-card { background: var(--glass); padding: 40px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.1); backdrop-filter: blur(20px); text-align: center; width: 300px; }
        input { width: 100%; padding: 12px; margin: 15px 0; border-radius: 8px; border: none; background: rgba(255,255,255,0.1); color: #fff; text-align: center; outline: none; }
        button.btn-main { width: 100%; padding: 12px; border-radius: 8px; border: none; background: var(--cyan); color: #000; font-weight: bold; cursor: pointer; }

        /* APP LAYOUT */
        #app { display: flex; height: 100%; }
        nav { width: 80px; background: rgba(0,0,0,0.9); display: flex; flex-direction: column; align-items: center; padding: 20px 0; border-right: 1px solid var(--glass); }
        nav button { background: none; border: none; color: #555; font-size: 9px; margin: 12px 0; cursor: pointer; font-weight: bold; transition: 0.3s; }
        nav button.active { color: var(--gold); transform: scale(1.1); }

        .view-container { flex: 1; overflow-y: auto; padding: 20px; }
        .view { display: none; animation: fadeIn 0.4s; }
        .view.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .card { background: var(--glass); border-radius: 15px; padding: 20px; margin-bottom: 15px; border: 1px solid rgba(255,255,255,0.05); position: relative; }

        /* HOME & PROFIL */
        .profile-img { width: 110px; height: 110px; border-radius: 50%; border: 3px solid var(--gold); object-fit: cover; }
        .plus-btn { background: var(--gold); border: none; width: 25px; height: 25px; border-radius: 50%; cursor: pointer; font-weight: bold; margin-left: -25px; }

        /* FEED: TITIK TIGA */
        .post-menu { position: absolute; top: 15px; right: 15px; cursor: pointer; font-size: 20px; color: #888; padding: 5px; }
        .delete-btn { color: #ff4444; font-size: 12px; cursor: pointer; margin-top: 10px; display: none; background: rgba(255,0,0,0.15); padding: 8px; border-radius: 5px; text-align: center; border: 1px solid #ff4444; }

        /* RATE: POLOS */
        .rate-btns { display: flex; justify-content: space-between; gap: 8px; margin-top: 20px; }
        .star-btn { flex: 1; padding: 12px 5px; background: transparent; border: 1px solid rgba(255,255,255,0.2); color: #fff; border-radius: 8px; cursor: pointer; }
        .star-btn.voted { background: var(--gold); color: #000; border-color: var(--gold); font-weight: bold; }

        /* USERS LIST */
        .user-item { display: flex; align-items: center; gap: 15px; background: rgba(255,255,255,0.03); padding: 10px; border-radius: 10px; margin-bottom: 10px; }
        .user-avatar { width: 40px; height: 40px; border-radius: 50%; background: var(--gold); display: flex; align-items: center; justify-content: center; color: #000; font-weight: bold; }
    </style>
</head>
<body>

    <div class="ball" style="width:300px; height:300px; background:var(--gold); top:-50px; left:-50px;"></div>
    <div class="ball" style="width:250px; height:250px; background:var(--cyan); bottom:-50px; right:-50px;"></div>

    <div id="loader">
        <div class="welcome-anim" id="loader-msg">Selamat Datang</div>
    </div>

    <div id="login-page">
        <div class="login-card">
            <h1 style="color:var(--gold)">AUREATE</h1>
            <input type="text" id="user-in" placeholder="Nama Kamu">
            <button class="btn-main" onclick="masukApp()">MASUK</button>
        </div>
    </div>

    <div id="app" style="display:none">
        <nav>
            <button class="active" onclick="navigasi('home')">HOME</button>
            <button onclick="navigasi('profil')">BIO</button>
            <button onclick="navigasi('users')">USERS</button>
            <button onclick="navigasi('feed')">FEED</button>
            <button onclick="navigasi('chat')">CHAT</button>
            <button onclick="navigasi('rank')">RANK</button>
            <button onclick="navigasi('rate')">RATE</button>
            <button onclick="keluarApp()" style="color:#ff4444; margin-top:auto">EXIT</button>
        </nav>

        <div class="view-container">
            <section id="home" class="view active">
                <div style="text-align:center">
                    <img src="IMG_20260315_091508.jpg" class="profile-img" onerror="this.src='https://via.placeholder.com/110?text=Daniel'">
                    <button class="plus-btn" onclick="followAction()">+</button>
                    <h2 style="margin-top:15px;">@DanielDolar</h2>
                    <div style="display:flex; justify-content:center; gap:35px; margin:20px 0;">
                        <div><b id="h-fol">0</b><br><small style="opacity:0.6">Pengikut</small></div>
                        <div><b id="h-lik">0</b><br><small style="opacity:0.6">Menyukai</small></div>
                    </div>
                    <div class="card"><p style="font-size:12px; margin-bottom:10px;">Statistik Follower</p><canvas id="chartHome"></canvas></div>
                </div>
            </section>

            <section id="profil" class="view">
                <div class="card">
                    <h2 style="color:var(--gold)">Daniel Dolar Sarumaha</h2>
                    <p><b>Umur:</b> 19 Tahun</p>
                    <p><b>TTL:</b> 14 Mei 2006</p>
                    <p><b>Alamat:</b> Desa Hiliamaetaluo, Nias Selatan</p>
                    <hr style="opacity:0.1; margin:15px 0;">
                    <p style="line-height:1.7; opacity:0.8;">
                        Saya adalah seorang pemuda dari Nias Selatan yang bercita-cita besar dalam dunia teknologi. Di usia ke-19 ini, saya membangun <b>AUREATE</b> sebagai wadah kreativitas dan koneksi digital. Bagi saya, tantangan adalah peluang untuk tumbuh lebih kuat. Dari Desa Hiliamaetaniha, saya siap membawa karya ini mendunia.
                    </p>
                </div>
            </section>

            <section id="users" class="view">
                <h2 style="color:var(--cyan)">Anggota Bergabung</h2>
                <div id="users-list"></div>
            </section>

            <section id="feed" class="view">
                <div class="card">
                    <input type="text" id="post-msg" placeholder="Bagikan cerita kamu...">
                    <button class="btn-main" onclick="gasPost()">Kirim Post</button>
                </div>
                <div id="feed-list"></div>
            </section>

            <section id="chat" class="view">
                <div class="card">
                    <div id="chat-list" style="height:350px; overflow-y:auto; padding:10px; border-radius:10px; background:rgba(0,0,0,0.2); margin-bottom:10px;"></div>
                    <div style="display:flex; gap:10px;">
                        <input type="text" id="chat-msg" placeholder="Ketik pesan..." style="flex:1; margin:0">
                        <button class="btn-main" onclick="gasChat()" style="width:80px">Gas</button>
                    </div>
                </div>
            </section>

            <section id="rank" class="view">
                <h2 style="color:var(--gold)">🏆 Juara Postingan Daniel</h2>
                <div id="rank-list"></div>
            </section>

            <section id="rate" class="view">
                <div class="card" id="rate-panel">
                    <h2>Beri Rating Aureate</h2>
                    <p style="font-size:12px; opacity:0.6;">Satu user hanya bisa vote sekali.</p>
                    <div class="rate-btns">
                        <button class="star-btn" id="s1" onclick="vote(1)">⭐1</button>
                        <button class="star-btn" id="s2" onclick="vote(2)">⭐2</button>
                        <button class="star-btn" id="s3" onclick="vote(3)">⭐3</button>
                        <button class="star-btn" id="s4" onclick="vote(4)">⭐4</button>
                        <button class="star-btn" id="s5" onclick="vote(5)">⭐5</button>
                    </div>
                </div>
                <div class="card">
                    <canvas id="rateChart"></canvas>
                </div>
            </section>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp, doc, setDoc, getDoc, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

        const db = getFirestore(initializeApp({
            apiKey: "AIzaSyAaW8jwL5yT-uZZglS2gA_HWRJvdUG-nZA",
            authDomain: "danieldolar-9bca1.firebaseapp.com",
            projectId: "danieldolar-9bca1"
        }));

        let userSkrg = "";

        // AUTH
        window.masukApp = async () => {
            userSkrg = document.getElementById('user-in').value;
            if(!userSkrg) return alert("Isi namamu!");
            await setDoc(doc(db, "members", userSkrg.toLowerCase()), { nama: userSkrg, joinAt: serverTimestamp() });
            document.getElementById('login-page').style.display = 'none';
            document.getElementById('loader').style.display = 'flex';
            setTimeout(() => {
                document.getElementById('loader').style.display = 'none';
                document.getElementById('app').style.display = 'flex';
                mulaiRealtime();
            }, 5000);
        };

        window.keluarApp = () => {
            document.getElementById('app').style.display = 'none';
            document.getElementById('loader').style.display = 'flex';
            document.getElementById('loader-msg').innerText = "Sampai Jumpa";
            setTimeout(() => location.reload(), 5000);
        };

        window.navigasi = (id) => {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
        };

        // ACTIONS
        window.followAction = async () => {
            const ref = doc(db, "meta", "f_" + userSkrg.toLowerCase());
            if((await getDoc(ref)).exists()) return alert("Sudah follow!");
            await setDoc(ref, { user: userSkrg });
            alert("Followed!");
        };

        window.gasPost = async () => {
            const m = document.getElementById('post-msg');
            if(m.value) { await addDoc(collection(db, "posts"), { user: userSkrg, text: m.value, likes: 0, time: serverTimestamp() }); m.value = ""; }
        };

        window.showMenu = (id) => {
            const btn = document.getElementById('del-' + id);
            btn.style.display = (btn.style.display === 'block') ? 'none' : 'block';
        };

        window.hapusPost = async (id) => {
            if(confirm("Hapus?")) await deleteDoc(doc(db, "posts", id));
        };

        window.vote = async (n) => {
            if(userSkrg.toLowerCase() === "daniel") return alert("Pemilik dilarang vote!");
            const ref = doc(db, "ratings", userSkrg.toLowerCase());
            if((await getDoc(ref)).exists()) return alert("Sudah vote!");
            await setDoc(ref, { val: n });
            document.getElementById('s' + n).classList.add('voted');
            alert("Voted ⭐" + n);
        };

        window.like = async (id, cur) => { await updateDoc(doc(db, "posts", id), { likes: cur + 1 }); };

        window.gasChat = async () => {
            const m = document.getElementById('chat-msg');
            if(m.value) { await addDoc(collection(db, "chats"), { user: userSkrg, text: m.value, time: serverTimestamp() }); m.value = ""; }
        };

        // DATA
        let hChart, rChart;
        function mulaiRealtime() {
            if(userSkrg.toLowerCase() === "daniel") document.getElementById('rate-panel').innerHTML = "<h3>Admin View Only</h3>";

            // Members
            onSnapshot(collection(db, "members"), s => {
                document.getElementById('users-list').innerHTML = s.docs.map(d => `<div class="user-item"><div class="user-avatar">${d.data().nama.charAt(0).toUpperCase()}</div><b>${d.data().nama}</b></div>`).join('');
            });

            // Feed & Rank
            onSnapshot(query(collection(db, "posts"), orderBy("time", "desc")), s => {
                let html = "", dL = 0, all = [];
                s.forEach(d => {
                    const data = d.data();
                    if(data.user.toLowerCase() === 'daniel') dL += (data.likes || 0);
                    all.push({id: d.id, ...data});
                    const isMe = data.user.toLowerCase() === userSkrg.toLowerCase();
                    html += `<div class="card">${isMe ? `<div class="post-menu" onclick="showMenu('${d.id}')">⋮</div>`:''}<b>@${data.user}</b><p>${data.text}</p><button onclick="like('${d.id}', ${data.likes||0})">❤️ ${data.likes||0}</button><div id="del-${d.id}" class="delete-btn" onclick="hapusPost('${d.id}')">Hapus Postingan</div></div>`;
                });
                document.getElementById('feed-list').innerHTML = html;
                document.getElementById('h-lik').innerText = dL;
                const topD = all.filter(p => p.user.toLowerCase() === 'daniel').sort((a,b) => (b.likes||0) - (a.likes||0)).slice(0,5);
                document.getElementById('rank-list').innerHTML = topD.map((p,i) => `<div class="card">Juara ${i+1}: ${p.text} (❤️ ${p.likes||0})</div>`).join('');
            });

            // Chat
            onSnapshot(query(collection(db, "chats"), orderBy("time", "asc")), s => {
                document.getElementById('chat-list').innerHTML = s.docs.map(d => `<div><b>${d.data().user}:</b> ${d.data().text}</div>`).join('');
                document.getElementById('chat-list').scrollTop = 9999;
            });

            // Follower Count & Chart
            onSnapshot(collection(db, "meta"), s => {
                let f = 0; s.forEach(d => { if(d.id.startsWith("f_")) f++; });
                document.getElementById('h-fol').innerText = f;
                if(hChart) hChart.destroy();
                hChart = new Chart(document.getElementById('chartHome'), { type: 'pie', data: { labels: ['Followers', 'Others'], datasets: [{ data: [f, 100-f], backgroundColor: ['#0fe', '#333'] }] }});
            });

            // Rating Chart
            onSnapshot(collection(db, "ratings"), s => {
                let r = [0,0,0,0,0]; s.forEach(d => { r[d.data().val-1]++; });
                if(rChart) rChart.destroy();
                rChart = new Chart(document.getElementById('rateChart'), { type: 'bar', data: { labels: ['⭐1','⭐2','⭐3','⭐4','⭐5'], datasets: [{ data: r, backgroundColor: '#e5b026' }] }});
            });
        }
    </script>
</body>
</html>
