<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Daniel Dolar Sarumaha - Ultra Cinematic SPA</title>
<style>
body, html{margin:0;padding:0;height:100%;font-family:'Segoe UI',sans-serif;background:#0e0e20;color:white;overflow:hidden;}
*{box-sizing:border-box;}
.container{display:flex;height:100vh;transition:0.5s;}
.sidebar{width:220px;background:#11112a;padding:20px;display:flex;flex-direction:column;transition:width 0.4s;}
.sidebar h3{margin-top:0;margin-bottom:20px;color:#fffb;}
.sidebar div{margin:12px 0;cursor:pointer;padding:6px 8px;border-radius:6px;transition:0.2s;}
.sidebar div:hover{background:#3a3a5c;color:#fff;font-weight:600;}
.main{flex:1;overflow-y:auto;padding:20px;background:linear-gradient(135deg,#1b1b2f,#0e0e20);transition:0.5s;}
.page{opacity:0;transform:translateY(20px);transition:0.4s;display:none;}
.page.active{display:block;opacity:1;transform:translateY(0);}
.card{background:rgba(255,255,255,0.05);backdrop-filter:blur(6px);padding:18px;margin-bottom:20px;border-radius:14px;box-shadow:0 4px 20px rgba(0,0,0,0.4);transition:0.3s transform;}
.card:hover{transform: translateY(-4px) scale(1.01);box-shadow:0 8px 30px rgba(0,0,0,0.5);}
textarea,input[type="text"]{width:100%;padding:10px;border-radius:8px;border:none;background:rgba(255,255,255,0.1);color:white;margin-bottom:10px;}
button{padding:6px 12px;border:none;border-radius:6px;background:#4f46e5;color:white;cursor:pointer;margin-right:6px;transition:0.2s;}
button:hover{background:#3b36c1;transform:scale(1.05);}
.rating span,.reaction span{cursor:pointer;font-size:18px;margin-right:6px;transition:0.2s;}
.rating span:hover,.reaction span:hover{transform:scale(1.3);}
.comment{margin-top:10px;font-size:14px;}
.reply{margin-left:20px;font-size:13px;color:#aaa;}
.tag{color:#3498db;cursor:pointer;margin-right:6px;font-size:13px;}
#profilePhoto{border-radius:50%;width:80px;height:80px;margin-right:16px;box-shadow:0 4px 12px rgba(0,0,0,0.3);}
.social a{margin-right:10px;color:#4f46e5;text-decoration:none;font-weight:bold;}
.social a:hover{text-decoration:underline;}
</style>
</head>
<body>
<div class="container">
  <div class="sidebar">
    <h3>Platform</h3>
    <div onclick="showPage('home')">🏠 Home</div>
    <div onclick="showPage('trending')">🔥 Trending</div>
    <div onclick="showPage('bookmarks')">🔖 Bookmarks</div>
    <div onclick="showPage('drafts')">📝 Drafts</div>
    <div onclick="showPage('notifications')">🔔 Notifications</div>
    <div onclick="showPage('leaderboard')">🏆 Leaderboard</div>
    <div onclick="showPage('profile')">👤 Profile</div>
  </div>
  <div class="main">

    <!-- HOME PAGE -->
    <div id="home" class="page active">
      <h2>Home</h2>
      <input id="search" placeholder="Search..." oninput="render()">
      <div class="card">
        <textarea id="postText" placeholder="Write something..."></textarea><br>
        <button onclick="createPost()">Post</button>
        <button onclick="saveDraft()">Save Draft</button>
      </div>
      <div id="feed"></div>
    </div>

    <!-- PROFILE PAGE -->
    <div id="profile" class="page">
      <h2>Profile</h2>
      <div style="display:flex;align-items:center;margin-bottom:20px;">
        <img id="profilePhoto" src="https://via.placeholder.com/80" alt="Profile Photo">
        <div>
          <h3 id="profileName">Daniel Dolar Sarumaha</h3>
          <p id="profileEmail" style="margin:4px 0;color:#ccc;">albumxiimia@gmail.com</p>
          <p id="profileBio" style="margin:4px 0;font-size:14px;color:#aaa;">Portfolio ultra cinematic, multi-functional SPA</p>
          <div class="social">
            <a href="https://wa.me/6281388149795" target="_blank">WhatsApp</a>
            <a href="https://instagram.com/Danieldolars" target="_blank">Instagram</a>
            <a href="https://facebook.com/Daniel Sarumaha" target="_blank">Facebook</a>
          </div>
        </div>
      </div>
      <div>
        <input type="file" id="uploadPhoto" onchange="changePhoto(event)">
        <p style="margin:8px 0;">Catatan Pribadi:</p>
        <textarea id="personalNote" placeholder="Tulis catatan pribadimu di sini..."></textarea><br>
        <button onclick="saveNote()">Simpan Catatan</button>
      </div>
      <div style="display:flex;gap:16px;margin-top:20px;">
        <div class="card" style="flex:1;text-align:center;">Posts<br><span id="statPosts">0</span></div>
        <div class="card" style="flex:1;text-align:center;">Comments<br><span id="statComments">0</span></div>
        <div class="card" style="flex:1;text-align:center;">Reactions<br><span id="statReactions">0</span></div>
        <div class="card" style="flex:1;text-align:center;"><span id="followBtn" onclick="toggleFollow()" style="cursor:pointer;background:#4f46e5;padding:4px 10px;border-radius:6px;">Follow</span></div>
      </div>
    </div>

  </div>
</div>

<script>
// Data
let posts=[], bookmarks=[], drafts=[], notifications=[], currentTag=null;
let user={name:"Daniel Dolar Sarumaha",email:"albumxiimia@gmail.com",bio:"Portfolio ultra cinematic, multi-functional SPA",photo:"https://via.placeholder.com/80",followed:false,note:""};

// SPA
function showPage(id){
  document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
  document.getElementById(id).classList.add("active");
  render();
}

// Profile
function changePhoto(e){let file=e.target.files[0]; if(file){let reader=new FileReader(); reader.onload=function(){document.getElementById("profilePhoto").src=reader.result; user.photo=reader.result;} reader.readAsDataURL(file);}}
function saveNote(){user.note=document.getElementById("personalNote").value; alert("Catatan tersimpan!");}
function toggleFollow(){user.followed=!user.followed; document.getElementById("followBtn").innerText=user.followed?"Unfollow":"Follow";}

// Post & Interaction
function createPost(){let text=document.getElementById("postText").value; posts.unshift({id:Date.now(),text,rating:0,votes:0,reactions:{like:0,fire:0,love:0},comments:[],tags:text.match(/#\w+/g)||[]}); render();}
function vote(id,val){let p=posts.find(x=>x.id==id);p.votes+=val;render();}
function rate(id,val){let p=posts.find(x=>x.id==id);p.rating=val;render();}
function react(id,type){let p=posts.find(x=>x.id==id);p.reactions[type]++; notifications.push("Reaction added");render();}
function comment(id){let input=document.getElementById("c"+id); let p=posts.find(x=>x.id==id); p.comments.push({text:input.value,replies:[]}); input.value=""; notifications.push("New comment"); render();}
function reply(id,i){let txt=prompt("Reply"); let p=posts.find(x=>x.id==id); p.comments[i].replies.push(txt); render();}
function bookmark(id){if(!bookmarks.includes(id))bookmarks.push(id);render();}
function saveDraft(){drafts.push(document.getElementById("postText").value); render();}

// Render
function render(){
  // Profile
  document.getElementById("profileName").innerText=user.name;
  document.getElementById("profileEmail").innerText=user.email;
  document.getElementById("profileBio").innerText=user.bio;
  document.getElementById("profilePhoto").src=user.photo;
  document.getElementById("personalNote").value=user.note;
  // Stats
  document.getElementById("statPosts").innerText=posts.length;
  let totalComments=posts.reduce((sum,p)=>sum+p.comments.length,0);
  document.getElementById("statComments").innerText=totalComments;
  let totalReactions=posts.reduce((sum,p)=>sum+p.reactions.like+p.reactions.fire+p.reactions.love,0);
  document.getElementById("statReactions").innerText=totalReactions;

  // Feed
  let search=document.getElementById("search")?.value?.toLowerCase()||"";
  let feed="";
  posts.filter(p=>p.text.toLowerCase().includes(search)).forEach(p=>{
    let stars=""; for(let i=1;i<=5;i++){stars+=`<span onclick="rate(${p.id},${i})">${i<=p.rating?"⭐":"☆"}</span>`;}
    let comments=p.comments.map((c,i)=>`<div class="comment">💬 ${c.text} <button onclick="reply(${p.id},${i})">reply</button> ${c.replies.map(r=>`<div class="reply">↳ ${r}</div>`).join("")}</div>`).join("");
    feed+=`<div class="card">
      <div>${p.text}</div>
      <div style="font-size:12px;color:#aaa">Reading time: ${Math.ceil(p.text.split(" ").length/200)} min</div>
      <div>
        <button onclick="vote(${p.id},1)">⬆ ${p.votes}</button>
        <button onclick="vote(${p.id},-1)">⬇</button>
        <button onclick="bookmark(${p.id})">🔖</button>
      </div>
      <div class="rating">${stars}</div>
      <div class="reaction">
        <span onclick="react(${p.id},'like')">👍 ${p.reactions.like}</span>
        <span onclick="react(${p.id},'fire')">🔥 ${p.reactions.fire}</span>
        <span onclick="react(${p.id},'love')">❤️ ${p.reactions.love}</span>
      </div>
      <div><input id="c${p.id}" placeholder="comment"><button onclick="comment(${p.id})">send</button>${comments}</div>
    </div>`;
  });
  document.getElementById("feed").innerHTML=feed;
}

render();
</script>
</body>
</html>