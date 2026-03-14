<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Radial Menu</title>

<style>

body{
background:#111;
display:flex;
justify-content:center;
align-items:center;
height:100vh;
font-family:sans-serif;
}

.menu{
position:relative;
width:200px;
height:200px;
}

.center{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
width:60px;
height:60px;
background:#222;
color:white;
display:flex;
justify-content:center;
align-items:center;
border-radius:50%;
cursor:pointer;
font-size:24px;
}

.item{
position:absolute;
width:60px;
height:60px;
background:#222;
border-radius:15px;
display:flex;
justify-content:center;
align-items:center;
color:white;
font-size:20px;
box-shadow:0 0 15px cyan;
transition:0.4s;
opacity:0;
}

.menu.active .item{
opacity:1;
}

.item:nth-child(2){top:-20px;left:70px;}
.item:nth-child(3){top:30px;left:150px;}
.item:nth-child(4){top:120px;left:150px;}
.item:nth-child(5){top:170px;left:70px;}
.item:nth-child(6){top:120px;left:-10px;}
.item:nth-child(7){top:30px;left:-10px;}

.item:hover{
transform:scale(1.2);
box-shadow:0 0 20px #00ffff;
}

</style>

</head>

<body>

<div class="menu" id="menu">

<div class="center" onclick="toggleMenu()">+</div>

<div class="item">🏠</div>
<div class="item">⚙️</div>
<div class="item">📷</div>
<div class="item">🎮</div>
<div class="item">🎥</div>
<div class="item">💬</div>

</div>

<script>

function toggleMenu(){
document.getElementById("menu").classList.toggle("active");
}

</script>

</body>
</html>