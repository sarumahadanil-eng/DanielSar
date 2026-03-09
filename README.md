<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Social Knowledge Platform</title>

<style>

body{
font-family:system-ui;
margin:0;
background:#f4f4f4;
}

header{
background:#111;
color:white;
padding:12px;
display:flex;
justify-content:space-between;
align-items:center;
}

.container{
max-width:900px;
margin:auto;
padding:20px;
}

.editor,.post{
background:white;
padding:20px;
border-radius:10px;
margin-bottom:20px;
}

textarea{
width:100%;
height:120px;
}

button{
cursor:pointer;
padding:6px 10px;
margin:3px;
}

.rating span{
font-size:20px;
cursor:pointer;
}

.dark{
background:#121212;
color:white;
}

.dark .post,.dark .editor{
background:#1e1e1e;
}

.tag{
color:#3498db;
cursor:pointer;
margin-right:5px;
}

.comment{
margin-top:10px;
font-size:14px;
}

.reply{
margin-left:20px;
font-size:13px;
}

</style>

</head>
<body>

<header>

<b>Social Knowledge</b>

<div>

<input id="search" placeholder="search..." oninput="render()">

<button onclick="theme('dark')">🌙</button>
<button onclick="theme('neon')">🌈</button>

</div>

</header>

<div class="container">

<div class="editor">

<h3>Create Post</h3>

<textarea id="postText"></textarea>

<br>

<button onclick="createPost()">Publish</button>
<button onclick="saveDraft()">Save Draft</button>

</div>

<div id="feed"></div>

</div>

<script>

let posts=[]
let drafts=[]
let bookmarks=[]
let notifications=[]
let currentTag=null

let user={
name:"Guest",
avatar:"😀"
}

function markdown(text){

text=text.replace(/\*\*(.*?)\*\*/g,"<b>$1</b>")
text=text.replace(/\*(.*?)\*/g,"<i>$1</i>")
text=text.replace(/`(.*?)`/g,"<code>$1</code>")

return text

}

function addTags(text){

let tags=text.match(/#\w+/g)

return tags||[]

}

function readingTime(text){

let words=text.split(" ").length

return Math.ceil(words/200)+" min read"

}

function summarize(text){

let w=text.split(" ")

if(w.length<40) return ""

return w.slice(0,25).join(" ")+"..."

}

function calcTrending(p){

return p.votes*2 +
p.reactions.like +
p.reactions.fire*2 +
p.reactions.love +
p.comments.length*2

}

function createPost(){

let text=document.getElementById("postText").value

posts.unshift({

id:Date.now(),
text:text,
votes:0,
rating:0,
tags:addTags(text),
time:new Date().toLocaleString(),

reactions:{
like:0,
fire:0,
love:0
},

comments:[],
pinned:false,
poll:null

})

render()

}

function vote(id,val){

let p=posts.find(x=>x.id==id)
p.votes+=val

render()

}

function rate(id,val){

let p=posts.find(x=>x.id==id)
p.rating=val

render()

}

function react(id,type){

let p=posts.find(x=>x.id==id)
p.reactions[type]++

notifications.push("Reaction added")

render()

}

function comment(id){

let input=document.getElementById("c"+id)

let p=posts.find(x=>x.id==id)

p.comments.push({
text:input.value,
replies:[]
})

input.value=""

notifications.push("New comment")

render()

}

function reply(postId,index){

let txt=prompt("Reply")

let p=posts.find(x=>x.id==postId)

p.comments[index].replies.push(txt)

render()

}

function editPost(id){

let p=posts.find(x=>x.id==id)

let txt=prompt("Edit",p.text)

if(txt){

p.text=txt
p.tags=addTags(txt)

}

render()

}

function deletePost(id){

posts=posts.filter(p=>p.id!=id)

render()

}

function pinPost(id){

let p=posts.find(x=>x.id==id)

p.pinned=!p.pinned

render()

}

function bookmark(id){

if(bookmarks.includes(id))
bookmarks=bookmarks.filter(x=>x!=id)
else
bookmarks.push(id)

render()

}

function createPoll(id){

let q=prompt("Poll question")
let a=prompt("Option 1")
let b=prompt("Option 2")

let p=posts.find(x=>x.id==id)

p.poll={
question:q,
options:[
{text:a,votes:0},
{text:b,votes:0}
]
}

render()

}

function votePoll(id,i){

let p=posts.find(x=>x.id==id)

p.poll.options[i].votes++

render()

}

function saveDraft(){

let text=document.getElementById("postText").value

drafts.push(text)

alert("Draft saved")

}

function filterTag(tag){

currentTag=tag

render()

}

function theme(mode){

document.body.classList.remove("dark")

document.body.style.background=""

if(mode=="dark")
document.body.classList.add("dark")

if(mode=="neon"){

document.body.style.background="#0f0f0f"
document.body.style.color="#00ff9c"

}

}

function renderPoll(p){

if(!p.poll) return ""

return `

<div>

<b>${p.poll.question}</b>

${p.poll.options.map((o,i)=>`

<div>

<button onclick="votePoll(${p.id},${i})">${o.text}</button>
${o.votes}

</div>

`).join("")}

</div>

`

}

function leaderboard(){

let top=[...posts]

.sort((a,b)=>calcTrending(b)-calcTrending(a))

.slice(0,3)

return `

<div class="post">

<h4>Top Posts</h4>

${top.map(p=>`<div>${p.text.slice(0,40)}</div>`).join("")}

</div>

`

}

function popularTags(){

let tags={}

posts.forEach(p=>{

p.tags.forEach(t=>{

tags[t]=(tags[t]||0)+1

})

})

let sorted=Object.entries(tags)

.sort((a,b)=>b[1]-a[1])

.slice(0,5)

return `

<div class="post">

<b>Popular Tags</b><br>

${sorted.map(t=>`<span class="tag" onclick="filterTag('${t[0]}')">${t[0]}</span>`).join("")}

</div>

`

}

function render(){

let feed=document.getElementById("feed")

feed.innerHTML=""

feed.innerHTML+=leaderboard()

feed.innerHTML+=popularTags()

let search=document.getElementById("search").value.toLowerCase()

posts
.filter(p=>p.text.toLowerCase().includes(search))
.filter(p=>!currentTag || p.tags.includes(currentTag))
.sort((a,b)=>{

if(a.pinned) return -1
if(b.pinned) return 1

return calcTrending(b)-calcTrending(a)

})

.forEach(p=>{

let stars=""

for(let i=1;i<=5;i++){

stars+=`<span onclick="rate(${p.id},${i})">${i<=p.rating?"⭐":"☆"}</span>`

}

let comments=p.comments.map((c,i)=>`

<div class="comment">

💬 ${c.text}

<button onclick="reply(${p.id},${i})">reply</button>

${c.replies.map(r=>`<div class="reply">↳ ${r}</div>`).join("")}

</div>

`).join("")

feed.innerHTML+=`

<div class="post">

<div>${markdown(p.text)}</div>

<div style="font-size:12px;color:gray">

${p.time} • ${readingTime(p.text)}

</div>

<div>${summarize(p.text)}</div>

<div>

<button onclick="vote(${p.id},1)">⬆ ${p.votes}</button>
<button onclick="vote(${p.id},-1)">⬇</button>

<button onclick="editPost(${p.id})">✏</button>
<button onclick="deletePost(${p.id})">🗑</button>

<button onclick="pinPost(${p.id})">📌</button>
<button onclick="bookmark(${p.id})">🔖</button>

<button onclick="createPoll(${p.id})">📊</button>

</div>

<div class="rating">${stars}</div>

<div>

<span onclick="react(${p.id},'like')">👍 ${p.reactions.like}</span>
<span onclick="react(${p.id},'fire')">🔥 ${p.reactions.fire}</span>
<span onclick="react(${p.id},'love')">❤️ ${p.reactions.love}</span>

</div>

${renderPoll(p)}

<div>

<input id="c${p.id}" placeholder="comment">

<button onclick="comment(${p.id})">send</button>

${comments}

</div>

</div>

`

})

}

</script>

</body>
</html>