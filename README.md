# Adv-prs-4
<!DOCTYPE html>
<html>
<head>
<title>AI Product Recommendation System</title>
<style>
body{font-family:Arial;margin:0;background:#f2f2f2;}
header{background:#2563eb;color:white;padding:15px;text-align:center;}
.container{width:95%;margin:auto;margin-top:20px;}
.section{background:white;padding:15px;border-radius:10px;margin-bottom:20px;}
input,select{padding:8px;width:100%;margin:5px 0;}
button{padding:8px;background:#2563eb;color:white;border:none;border-radius:5px;cursor:pointer;}
.products{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:15px;}
.card{background:white;padding:10px;border-radius:10px;box-shadow:0 0 8px rgba(0,0,0,0.1);text-align:center;}
.card img{width:100%;height:160px;object-fit:contain;}
.rating{color:orange;}
.buy{background:green;margin-top:5px;}
</style>
</head>
<body>

<header>
<h1>AI Based Product Recommendation System 🤖</h1>
</header>

<div class="container">

<div class="section">
<h2>Search & Customize</h2>

<input type="text" id="search" placeholder="Search product...">

<label>Category</label>
<select id="category">
<option value="all">All</option>
<option value="mobile">Mobile</option>
<option value="laptop">Laptop</option>
<option value="shoes">Shoes</option>
<option value="watch">Watch</option>
</select>

<label>Minimum Rating</label>
<select id="rating">
<option value="0">All</option>
<option value="3">3⭐ & above</option>
<option value="4">4⭐ & above</option>
</select>

<button onclick="filterProducts()">Find Products</button>
</div>

<div class="section">
<h2>Recommended For You 🤖</h2>
<div class="products" id="recommended"></div>
</div>

<div class="section">
<h2>All Products</h2>
<div class="products" id="allProducts"></div>
</div>

</div>

<script>

let userPreference = JSON.parse(localStorage.getItem("preferences")) || { clicks:{} };

const products = [

{
name:"iPhone 14",
category:"mobile",
price:75000,
rating:4.8,
image:"https://m.media-amazon.com/images/I/61cwywLZR-L._SL1500_.jpg",
link:"https://www.amazon.in/dp/B0BDJ6ZMCC"
},

{
name:"Samsung Galaxy S23",
category:"mobile",
price:65000,
rating:4.5,
image:"https://m.media-amazon.com/images/I/61VfL-aiToL._SL1500_.jpg",
link:"https://www.amazon.in/dp/B0BT9CXXXX"
},

{
name:"Dell Inspiron 15",
category:"laptop",
price:55000,
rating:4.2,
image:"https://m.media-amazon.com/images/I/71hX8cB+YgL._SL1500_.jpg",
link:"https://www.amazon.in/dp/B0BSRXXXX"
},

{
name:"HP Pavilion 15",
category:"laptop",
price:58000,
rating:4.3,
image:"https://m.media-amazon.com/images/I/71vFKBpKakL._SL1500_.jpg",
link:"https://www.amazon.in/dp/B09XXXXX"
},

{
name:"Nike Air Max",
category:"shoes",
price:8000,
rating:4.6,
image:"https://static.nike.com/a/images/t_default/8bfa0f7c-3e3f-4f74-a0f3-0a8c8b7d3c3a/air-max-shoes.jpg",
link:"https://www.flipkart.com/nike-air-max"
},

{
name:"Boat Smart Watch",
category:"watch",
price:3000,
rating:4.0,
image:"https://m.media-amazon.com/images/I/61IMRs+o0iL._SL1500_.jpg",
link:"https://www.amazon.in/dp/B09VXXXX"
}

];

function displayProducts(list,id){
const container=document.getElementById(id);
container.innerHTML="";
if(list.length===0){container.innerHTML="<h3>No Products Found</h3>";return;}

list.forEach(p=>{
container.innerHTML+=`
<div class="card" onclick="trackClick('${p.category}')">
<img src="${p.image}">
<h3>${p.name}</h3>
<p><b>₹${p.price}</b></p>
<p class="rating">${"⭐".repeat(Math.floor(p.rating))}</p>
<a href="${p.link}" target="_blank">
<button class="buy">Buy Now</button>
</a>
</div>`;
});
}

function filterProducts(){
let search=document.getElementById("search").value.toLowerCase();
let category=document.getElementById("category").value;
let rating=parseFloat(document.getElementById("rating").value);

let filtered=products.filter(p=>
p.name.toLowerCase().includes(search) &&
(category==="all"||p.category===category) &&
p.rating>=rating
);

displayProducts(filtered,"allProducts");
generateAIRecommendations();
}

function trackClick(category){
if(!userPreference.clicks[category]) userPreference.clicks[category]=0;
userPreference.clicks[category]++;
localStorage.setItem("preferences",JSON.stringify(userPreference));
generateAIRecommendations();
}

function generateAIRecommendations(){
let scored=products.map(p=>{
let score=p.rating*2;
if(userPreference.clicks[p.category]) score+=userPreference.clicks[p.category]*5;
return {...p,score:score};
});
scored.sort((a,b)=>b.score-a.score);
displayProducts(scored.slice(0,4),"recommended");
}

displayProducts(products,"allProducts");
generateAIRecommendations();

</script>

</body>
</html>
