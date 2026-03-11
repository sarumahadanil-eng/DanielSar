<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Daniel Website</title>

<style>

body{
font-family:Arial;
background:#0f172a;
color:white;
text-align:center;
padding:50px;
}

.box{
background:#1e293b;
padding:30px;
border-radius:10px;
width:300px;
margin:auto;
}

input{
width:100%;
padding:10px;
margin:10px 0;
border:none;
border-radius:5px;
}

button{
padding:10px;
width:100%;
background:#38bdf8;
border:none;
color:white;
border-radius:5px;
cursor:pointer;
}

</style>

</head>

<body>

<div id="app"></div>

<script>

function loginPage(){

document.getElementById("app").innerHTML=`

<div class="box">

<h2>Login</h2>

<input id="user" placeholder="Username">

<input id="pass" type="password" placeholder="Password">

<button onclick="login()">Login</button>

<p>Belum punya akun?</p>

<button onclick="registerPage()">Register</button>

</div>

`;

}



function registerPage(){

document.getElementById("app").innerHTML=`

<div class="box">

<h2>Register</h2>

<input id="ruser" placeholder="Username">

<input id="rpass" type="password" placeholder="Password">

<button onclick="register()">Daftar</button>

<button onclick="loginPage()">Kembali</button>

</div>

`;

}



function register(){

let user=document.getElementById("ruser").value;
let pass=document.getElementById("rpass").value;

localStorage.setItem("user_"+user,pass);

alert("Akun berhasil dibuat");

loginPage();

}



function login(){

let user=document.getElementById("user").value;
let pass=document.getElementById("pass").value;

let saved=localStorage.getItem("user_"+user);

if(saved==pass){

localStorage.setItem("login",user);

home();

}else{

alert("Username atau password salah");

}

}



function home(){

let user=localStorage.getItem("login");

document.getElementById("app").innerHTML=`

<div class="box">

<h2>Halo ${user}</h2>

<p>Login berhasil</p>

<button onclick="logout()">Logout</button>

</div>

`;

}



function logout(){

localStorage.removeItem("login");

loginPage();

}



if(localStorage.getItem("login")){

home();

}else{

loginPage();

}

</script>

</body>
</html>