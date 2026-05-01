<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>STOR KING</title>

<style>
:root{
 --black:#0b0b0b;
 --gold:#b08d57;
}

body{
 margin:0;
 font-family:tahoma;
 background:var(--black);
 color:#fff;
}

header{
 background:#000;
 padding:15px;
 text-align:center;
}
header h1{color:var(--gold)}

nav{
 display:flex;
 justify-content:center;
 gap:20px;
 background:#111;
 padding:10px;
}
nav button{
 background:none;
 border:none;
 color:var(--gold);
 font-weight:bold;
 cursor:pointer;
}

.free{
 background:var(--gold);
 color:#000;
 text-align:center;
 padding:8px;
}

.section{padding:20px}

.products{
 display:grid;
 grid-template-columns:repeat(2,1fr);
 gap:15px;
}

.product{
 background:#111;
 border:1px solid var(--gold);
 padding:10px;
 border-radius:10px;
 text-align:center;
 cursor:pointer;
 transition:0.3s;
}
.product:hover{
 transform:scale(1.03);
}

.product img{
 width:100%;
 height:220px;
 object-fit:cover;
 border-radius:10px;
}

.hidden{display:none}

/* DETAILS */
.details img{
 width:100%;
 max-width:520px;
 height:520px;
 object-fit:cover;
 border-radius:15px;
 cursor:zoom-in;
}

.gallery{
 display:flex;
 gap:10px;
 margin:10px 0;
}

.gallery img{
 width:70px;
 height:70px;
 object-fit:cover;
 border-radius:8px;
 cursor:pointer;
 border:2px solid transparent;
}

.gallery img:hover{
 border-color:var(--gold);
}

input,select,textarea{
 width:100%;
 padding:8px;
 margin:5px 0;
}

button{
 background:var(--gold);
 border:none;
 padding:10px;
 width:100%;
 font-weight:bold;
 cursor:pointer;
 margin-top:10px;
}

.whatsapp{
 position:fixed;
 bottom:20px;
 left:20px;
 background:#25d366;
 color:#fff;
 padding:15px;
 border-radius:50px;
 text-decoration:none;
}

/* ZOOM */
#lightbox{
 display:none;
 position:fixed;
 top:0;left:0;
 width:100%;height:100%;
 background:rgba(0,0,0,0.9);
 justify-content:center;
 align-items:center;
 z-index:9999;
}

#lightbox img{
 max-width:90%;
 max-height:90%;
 border-radius:10px;
}
</style>
</head>

<body>

<header>
<h1>STOR KING</h1>
</header>

<nav>
<button onclick="show('men')">رجالي</button>
<button onclick="show('women')">نسائي</button>
<button onclick="showLogin()">🔐 Admin</button>
</nav>

<div class="free">🚚 توصيل مجاني + الدفع عند الاستلام</div>

<!-- MEN -->
<div class="section" id="men"></div>

<!-- WOMEN -->
<div class="section hidden" id="women"></div>

<!-- DETAILS -->
<div class="section hidden details" id="details">

<button onclick="back()">⬅ رجوع</button>

<h2 id="dName"></h2>

<img id="mainImg" onclick="zoom(this.src)">

<div class="gallery" id="gallery"></div>

<h3 id="dPrice"></h3>

<strong>المقاس</strong>
<select id="size">
<option>S</option><option>M</option><option>L</option><option>XL</option>
</select>

<strong>اللون</strong>
<select id="color">
<option>أسود</option>
<option>أبيض</option>
<option>أحمر</option>
</select>

<h3>🧾 بيانات العميل</h3>
<input id="name" placeholder="الاسم">
<input id="phone" placeholder="الهاتف">
<input id="address" placeholder="العنوان">

<button onclick="send()">📲 طلب عبر واتساب</button>
</div>

<!-- LOGIN -->
<div class="section hidden" id="login">
<input id="user" placeholder="user">
<input id="pass" type="password" placeholder="pass">
<button onclick="check()">دخول</button>
</div>

<!-- ADMIN -->
<div class="section hidden" id="admin">
<h2>لوحة التحكم</h2>
<p id="visits"></p>
<div id="orders"></div>
</div>

<!-- ZOOM -->
<div id="lightbox" onclick="closeZoom()">
<img id="zoomImg">
</div>

<a class="whatsapp" href="https://wa.me/212677669487">💬</a>

<script>

let products=[
{name:"قميص رجالي",price:"180 درهم",cat:"men",imgs:["https://via.placeholder.com/500","https://via.placeholder.com/501"]},
{name:"سترة رجالية",price:"300 درهم",cat:"men",imgs:["https://via.placeholder.com/502","https://via.placeholder.com/503"]},
{name:"فستان نسائي",price:"250 درهم",cat:"women",imgs:["https://via.placeholder.com/504","https://via.placeholder.com/505"]},
{name:"بلوزة نسائية",price:"150 درهم",cat:"women",imgs:["https://via.placeholder.com/506","https://via.placeholder.com/507"]}
];

let current,imgs=[],index=0;

function load(){
 products.forEach(p=>{
  let div=document.createElement("div");
  div.className="product";
  div.innerHTML=`<img src="${p.imgs[0]}"><h3>${p.name}</h3><p>${p.price}</p>`;
  div.onclick=()=>open(p);
  document.getElementById(p.cat).appendChild(div);
 });

 let v=localStorage.visits?+localStorage.visits+1:1;
 localStorage.visits=v;
}
load();

function show(x){
 men.classList.add("hidden");
 women.classList.add("hidden");
 details.classList.add("hidden");
 document.getElementById(x).classList.remove("hidden");
}

function open(p){
 current=p;
 imgs=p.imgs;
 index=0;

 men.classList.add("hidden");
 women.classList.add("hidden");
 details.classList.remove("hidden");

 dName.innerText=p.name;
 dPrice.innerText=p.price;
 mainImg.src=imgs[0];

 gallery.innerHTML="";
 imgs.forEach((img,i)=>{
  let im=document.createElement("img");
  im.src=img;
  im.onclick=()=>change(i);
  gallery.appendChild(im);
 });
}

function change(i){
 mainImg.style.opacity=0;
 setTimeout(()=>{
  mainImg.src=imgs[i];
  mainImg.style.opacity=1;
 },150);
}

function back(){
 details.classList.add("hidden");
 show('men');
}

function send(){
 let msg=
`طلب جديد
${current.name}
${current.price}
المقاس:${size.value}
اللون:${color.value}
الاسم:${name.value}
الهاتف:${phone.value}
العنوان:${address.value}
الدفع: عند الاستلام`;

 let orders=JSON.parse(localStorage.orders||"[]");
 orders.push(msg);
 localStorage.orders=JSON.stringify(orders);

 window.open("https://wa.me/212677669487?text="+encodeURIComponent(msg));
}

function zoom(src){
 lightbox.style.display="flex";
 zoomImg.src=src;
}

function closeZoom(){
 lightbox.style.display="none";
}

function showLogin(){
 login.classList.toggle("hidden");
}

function check(){
 if(user.value=="admin" && pass.value=="1234"){
  login.classList.add("hidden");
  admin.classList.remove("hidden");

  visits.innerText="الزوار: "+localStorage.visits;

  let o=JSON.parse(localStorage.orders||"[]");
  orders.innerHTML=o.map(x=>`<div style="border:1px solid #b08d57;padding:10px;margin:5px">${x}</div>`).join("");
 }
}

</script>

</body>
</html>
