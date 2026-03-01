<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>StickerZone PRO</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif;}
:root{
--primary:#ff4d6d;
--dark:#1e1e2f;
--light:#f8f9fa;
--gradient:linear-gradient(135deg,#ff4d6d,#ff9f1c);
}
body{background:var(--light);color:var(--dark);}
header{
position:fixed;top:0;width:100%;background:white;
box-shadow:0 2px 15px rgba(0,0,0,0.08);z-index:1000;
}
.navbar{
display:flex;justify-content:space-between;align-items:center;
padding:1rem 6%;
}
.logo{font-size:1.8rem;font-weight:700;color:var(--primary);}
.cart-btn{
cursor:pointer;font-weight:600;
background:var(--gradient);
color:white;padding:0.5rem 1rem;border-radius:30px;
}
.hero{
margin-top:90px;padding:4rem 6%;
text-align:center;
}
.hero h1{
font-size:2.8rem;margin-bottom:1rem;
background:var(--gradient);
-webkit-background-clip:text;
-webkit-text-fill-color:transparent;
}
.products{
padding:3rem 6%;
display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:2rem;
}
.product{
background:white;padding:1rem;border-radius:20px;
box-shadow:0 10px 25px rgba(0,0,0,0.08);
transition:0.3s;text-align:center;
}
.product:hover{
transform:translateY(-10px);
box-shadow:0 15px 35px rgba(0,0,0,0.15);
}
.product img{
width:100%;height:200px;object-fit:cover;
border-radius:15px;
}
.product h3{margin:1rem 0;}
.price{
color:var(--primary);font-weight:700;margin-bottom:1rem;
}
.add-btn{
background:var(--dark);color:white;
padding:0.6rem 1.5rem;border:none;
border-radius:25px;cursor:pointer;
transition:0.3s;
}
.add-btn:hover{background:var(--primary);}
.cart{
position:fixed;right:-400px;top:0;
width:350px;height:100%;
background:white;box-shadow:-5px 0 30px rgba(0,0,0,0.2);
padding:2rem;transition:0.4s;overflow-y:auto;
z-index:2000;
}
.cart.active{right:0;}
.cart h2{margin-bottom:1rem;}
.cart-item{
display:flex;justify-content:space-between;
margin-bottom:1rem;border-bottom:1px solid #ddd;
padding-bottom:0.5rem;font-size:0.9rem;
}
.remove{
color:red;cursor:pointer;font-weight:600;
}
.total{
margin-top:1rem;font-weight:700;
}
.checkout{
margin-top:1rem;width:100%;
padding:0.8rem;border:none;
background:var(--gradient);color:white;
border-radius:30px;font-weight:600;
cursor:pointer;
}
.notification{
position:fixed;bottom:20px;left:50%;
transform:translateX(-50%) translateY(100px);
background:var(--dark);color:white;
padding:1rem 2rem;border-radius:10px;
transition:0.3s;opacity:0;
}
.notification.show{
transform:translateX(-50%) translateY(0);
opacity:1;
}
footer{
text-align:center;padding:2rem;background:var(--dark);
color:white;margin-top:3rem;
}
@media(max-width:768px){
.hero h1{font-size:2rem;}
}
</style>
</head>

<body>

<header>
<div class="navbar">
<div class="logo">StickerZone PRO</div>
<div class="cart-btn" onclick="toggleCart()">
🛒 Cart (<span id="cart-count">0</span>)
</div>
</div>
</header>

<section class="hero">
<h1>Sticker Premium Anti Air & Tahan Lama 🔥</h1>
<p>Kualitas terbaik untuk laptop, motor, helm & dinding kamar.</p>
</section>

<section class="products">

<div class="product">
<img src="https://picsum.photos/400?random=1">
<h3>Sticker Anime</h3>
<div class="price">15000</div>
<button class="add-btn" onclick="addToCart('Sticker Anime',15000)">Tambah</button>
</div>

<div class="product">
<img src="https://picsum.photos/400?random=2">
<h3>Sticker Gaming</h3>
<div class="price">18000</div>
<button class="add-btn" onclick="addToCart('Sticker Gaming',18000)">Tambah</button>
</div>

<div class="product">
<img src="https://picsum.photos/400?random=3">
<h3>Sticker Quotes</h3>
<div class="price">12000</div>
<button class="add-btn" onclick="addToCart('Sticker Quotes',12000)">Tambah</button>
</div>

</section>

<div class="cart" id="cart">
<h2>Keranjang</h2>
<div id="cart-items"></div>
<div class="total">Total: Rp <span id="total">0</span></div>
<button class="checkout" onclick="checkout()">Checkout via WhatsApp</button>
</div>

<div class="notification" id="notif">Berhasil ditambahkan!</div>

<footer>
© 2026 StickerZone PRO - Website Modern
</footer>

<script>
let cart = JSON.parse(localStorage.getItem("cart")) || [];

function toggleCart(){
document.getElementById("cart").classList.toggle("active");
}

function addToCart(name,price){
cart.push({name,price});
localStorage.setItem("cart",JSON.stringify(cart));
updateCart();
showNotif("Ditambahkan ke keranjang!");
}

function removeItem(index){
cart.splice(index,1);
localStorage.setItem("cart",JSON.stringify(cart));
updateCart();
}

function updateCart(){
let cartItems=document.getElementById("cart-items");
let total=0;
cartItems.innerHTML="";
cart.forEach((item,index)=>{
total+=item.price;
cartItems.innerHTML+=`
<div class="cart-item">
${item.name} - Rp ${item.price}
<span class="remove" onclick="removeItem(${index})">X</span>
</div>`;
});
document.getElementById("total").innerText=total;
document.getElementById("cart-count").innerText=cart.length;
}

function checkout(){
if(cart.length===0){
showNotif("Keranjang masih kosong!");
return;
}
let message="Halo, saya mau pesan:%0A";
cart.forEach(item=>{
message+=`- ${item.name} (Rp ${item.price})%0A`;
});
let total=document.getElementById("total").innerText;
message+=`Total: Rp ${total}`;
window.open(`https://wa.me/6281234567890?text=${message}`);
}

function showNotif(text){
let notif=document.getElementById("notif");
notif.innerText=text;
notif.classList.add("show");
setTimeout(()=>notif.classList.remove("show"),2000);
}

updateCart();
</script>

</body>
</html>
