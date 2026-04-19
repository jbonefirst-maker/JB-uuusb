<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JB Uuusb</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial;}

html,body{
    width:100%;
    height:100%;
    overflow:hidden;
    color:white;
}

/* BACKGROUND */
body::before{
    content:"";
    position:fixed;
    width:100%;
    height:100%;
    background:#000;
    z-index:-1;
}

/* BOX */
.box{
    background:#111;
    padding:25px;
    border-radius:15px;
    width:90%;
    max-width:350px;
    text-align:center;
    position:absolute;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);
}

/* INPUT */
input{
    width:100%;
    padding:10px;
    margin:8px 0;
    border:none;
    border-radius:8px;
    background:#222;
    color:white;
}

/* BUTTON */
button{
    width:100%;
    padding:10px;
    margin-top:10px;
    border:none;
    border-radius:8px;
    font-size:16px;
    font-weight:bold;
    background:white;
    color:black;
}

/* TEXT LOGIN REGISTER */
.switchText{
    color:white;
    margin-top:12px;
    display:block;
    cursor:pointer;
}

/* DASHBOARD */
#dashboard{display:none;height:100%;}

/* SIDEBAR */
#sidebar{
    width:80px;
    height:100%;
    float:left;
    background:#111;
    border-right:1px solid #333;
    text-align:center;
    padding-top:20px;
}

/* MENU */
.menuItem{
    font-size:20px;
    margin:15px 5px;
    cursor:pointer;
    display:flex;
    flex-direction:column;
    align-items:center;
    padding:8px 0;
    border-radius:10px;
    transition:0.3s;
}

/* hover */
.menuItem:hover{
    background:#222;
}

/* 🔥 ACTIVE */
.menuItem.active{
    background:white;
    color:black;
    transform:scale(1.05);
}

.menuItem span{
    font-size:11px;
}

/* MAIN */
#mainContent{
    margin-left:80px;
    padding:15px;
    height:100vh;
    overflow-y:auto;
    position:relative;
}

/* PROFILE */
#profileBar{
    display:flex;
    align-items:center;
    gap:10px;
}

#profilePic{
    width:50px;
    height:50px;
    border-radius:50%;
    border:2px solid white;
    cursor:pointer;
}

#logoutBtn{
    position:absolute;
    top:10px;
    right:10px;
    width:auto;
    padding:5px 10px;
    font-size:12px;
}

/* CARD */
.card{
    background:white;
    color:black;
    padding:15px;
    border-radius:10px;
    margin-top:10px;
}

/* POPUP */
#overlay{
    position:fixed;
    top:0;
    left:0;
    width:100%;
    height:100%;
    background:rgba(0,0,0,0.6);
    display:none;
    justify-content:center;
    align-items:center;
}

#popupBox{
    background:#111;
    width:90%;
    max-width:350px;
    padding:20px;
    border-radius:15px;
    position:relative;
}

/* tombol X */
.closePopup{
    position:absolute;
    top:10px;
    right:15px;
    font-size:20px;
    cursor:pointer;
    color:white;
}
</style>
</head>

<body>

<!-- LOGIN -->
<div class="box" id="loginBox">
<h2>Login</h2>
<input id="loginUser" placeholder="Username">
<input id="loginPass" type="password" placeholder="Password">
<button onclick="login()">Masuk</button>
<p class="switchText" onclick="showRegister()">Belum punya akun? Register</p>
</div>

<!-- REGISTER -->
<div class="box" id="registerBox" style="display:none;">
<h2>Register</h2>
<input id="regUser" placeholder="Username">
<input id="regPass" type="password" placeholder="Password">
<button onclick="register()">Daftar</button>
<p class="switchText" onclick="showLogin()">Sudah punya akun? Login</p>
</div>

<!-- DASHBOARD -->
<div id="dashboard">

<!-- 🔥 SIDEBAR -->
<div id="sidebar">

<div onclick="setActiveMenu(this); showNews()" class="menuItem active">
📰
<span>Berita</span>
</div>

<div onclick="setActiveMenu(this); showQuiz()" class="menuItem">
📚
<span>Soal</span>
</div>

<div onclick="setActiveMenu(this); showInvest()" class="menuItem">
💰
<span>Investasi</span>
</div>

</div>

<div id="mainContent">

<button id="logoutBtn" onclick="logout()">Logout</button>

<!-- PROFILE -->
<div id="profileBar">
<img id="profilePic" onclick="document.getElementById('uploadFoto').click()">
<input type="file" id="uploadFoto" accept="image/*" onchange="uploadGambar(event)" style="display:none;">
<div>
<p id="welcome"></p>
</div>
</div>

<!-- POPUP -->
<div id="overlay">
<div id="popupBox">
<span class="closePopup" onclick="tutupPopup()">✖</span>
<h3>Pengumuman</h3>
<p>Website masih pengembangan</p>
</div>
</div>

<div id="newsBox"></div>
<div id="quizBox" style="display:none;"></div>
<div id="investBox" style="display:none;"></div>

</div>
</div>

<script>
window.onload=()=>{loginBox.style.display="block";};

/* ACTIVE MENU */
function setActiveMenu(el){
let menus=document.querySelectorAll(".menuItem");
menus.forEach(m=>m.classList.remove("active"));
el.classList.add("active");
}

/* SWITCH */
function showRegister(){loginBox.style.display="none";registerBox.style.display="block";}
function showLogin(){registerBox.style.display="none";loginBox.style.display="block";}

/* REGISTER */
function register(){
let u=regUser.value,p=regPass.value;
if(!u||!p)return alert("Isi semua!");

let users=JSON.parse(localStorage.getItem("users"))||[];
users.push({username:u,password:p});
localStorage.setItem("users",JSON.stringify(users));
alert("Berhasil!");
showLogin();
}

/* LOGIN */
function login(){
let u=loginUser.value,p=loginPass.value;

if(u==="admin"&&p==="123"){masuk(u);return;}

let users=JSON.parse(localStorage.getItem("users"))||[];
let f=users.find(x=>x.username===u&&x.password===p);
f?masuk(u):alert("Salah!");
}

/* MASUK */
function masuk(user){
loginBox.style.display="none";
registerBox.style.display="none";
dashboard.style.display="block";

welcome.innerText="Halo "+user;

let users=JSON.parse(localStorage.getItem("users"))||[];
let d=users.find(x=>x.username===user);

if(d && d.foto){
profilePic.src=d.foto;
}else{
profilePic.src="https://api.dicebear.com/7.x/avataaars/svg?seed="+user;
}

showNews();
overlay.style.display="flex";
}

/* UPLOAD FOTO */
function uploadGambar(e){
let file=e.target.files[0];
if(!file)return;

let reader=new FileReader();
reader.onload=function(ev){
let foto=ev.target.result;

profilePic.src=foto;

let users=JSON.parse(localStorage.getItem("users"))||[];
let user=welcome.innerText.replace("Halo ","");
let i=users.findIndex(x=>x.username===user);

if(i!=-1){
users[i].foto=foto;
localStorage.setItem("users",JSON.stringify(users));
}
};
reader.readAsDataURL(file);
}

/* LOGOUT */
function logout(){
dashboard.style.display="none";
loginBox.style.display="block";
overlay.style.display="none";
}

/* POPUP */
function tutupPopup(){overlay.style.display="none";}

/* MENU */
function showNews(){
quizBox.style.display="none";
investBox.style.display="none";
newsBox.style.display="block";
newsBox.innerHTML="<div class='card'>Berita</div>";
}

function showQuiz(){
newsBox.style.display="none";
investBox.style.display="none";
quizBox.style.display="block";
quizBox.innerHTML="<div class='card'>Soal</div>";
}

function showInvest(){
newsBox.style.display="none";
quizBox.style.display="none";
investBox.style.display="block";
investBox.innerHTML="<div class='card'>Investasi</div>";
}
</script>

</body>
</html>
