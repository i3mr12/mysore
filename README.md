<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>متجر أناقة</title>
  <style>
    body {
      font-family: sans-serif;
      margin: 0;
      padding: 0;
      direction: rtl;
      background-color: #f5f5f5;
    }
    header {
      background-color: #222;
      color: white;
      padding: 1rem 2rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }
    nav {
      background-color: #333;
      color: white;
      display: flex;
      justify-content: center;
      gap: 1rem;
      padding: 0.5rem;
      flex-wrap: wrap;
    }
    nav button {
      background: none;
      border: none;
      color: white;
      font-size: 1rem;
      cursor: pointer;
    }
    nav button.active {
      font-weight: bold;
      text-decoration: underline;
    }
    header h1 {
      margin: 0;
    }
    header button {
      background: #ff9900;
      border: none;
      padding: 0.5rem 1rem;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }
    main {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 1rem;
      padding: 2rem;
    }
    .product {
      background: white;
      padding: 1rem;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      text-align: center;
    }
    .product img {
      max-width: 100%;
      height: 200px;
      object-fit: cover;
    }
    .product h3 {
      margin: 0.5rem 0;
    }
    .product button {
      margin-top: 0.5rem;
      background-color: #007bff;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 5px;
      cursor: pointer;
    }
    #cart {
      position: fixed;
      top: 70px;
      left: 20px;
      background: white;
      border: 1px solid #ccc;
      padding: 1rem;
      border-radius: 10px;
      width: 300px;
      max-height: 400px;
      overflow-y: auto;
      display: none;
    }
    #cart h2 {
      margin-top: 0;
    }
    #cart ul {
      list-style: none;
      padding: 0;
    }
    #cart li {
      margin-bottom: 0.5rem;
    }
    footer {
      background-color: #222;
      color: white;
      padding: 1rem;
      text-align: center;
    }
    .pay-link {
      background-color: green;
      display: inline-block;
      padding: 0.5rem 1rem;
      color: white;
      border-radius: 5px;
      text-decoration: none;
      margin-top: 1rem;
    }
    #about {
      display: none;
      padding: 2rem;
      background-color: white;
    }
  </style>
</head>
<body>
  <header>
    <h1>متجر أناقة</h1>
    <button onclick="toggleCart()">🛒 السلة (<span id="cart-count">0</span>)</button>
  </header>

  <nav>
    <button class="active" onclick="filterCategory('all')">الكل</button>
    <button onclick="filterCategory('ملابس')">ملابس</button>
    <button onclick="filterCategory('إلكترونيات')">إلكترونيات</button>
    <button onclick="filterCategory('أحذية')">أحذية</button>
    <button onclick="filterCategory('هواتف')">هواتف</button>
    <button onclick="filterCategory('ساعات')">ساعات</button>
    <button onclick="filterCategory('إكسسوارات')">إكسسوارات</button>
    <button onclick="showAbout()">من نحن</button>
  </nav>

  <main id="products"></main>

  <section id="about">
    <h2>من نحن</h2>
    <p>نحن متجر أناقة، نسعى لتقديم منتجات ذات جودة عالية في مجالات الملابس، الإلكترونيات، الإكسسوارات والهواتف بأسعار تنافسية، مع دعم فني وخدمة عملاء متميزة على مدار الساعة.</p>
  </section>

  <section id="cart">
    <h2>سلة المشتريات</h2>
    <ul id="cart-items"></ul>
    <p>المجموع: <span id="cart-total">0</span> ريال</p>
    <a class="pay-link" href="https://www.paypal.com" target="_blank">إتمام الدفع</a>
  </section>

  <footer>
    <p>رقم التواصل والدعم الفني: <a href="tel:0540733725" style="color:white; text-decoration: underline;">0540733725</a></p>
  </footer>

  <script>
    const products = [
      { id: 1, name: "ساعة ذكية", price: 220, category: "ساعات", image: "https://m.media-amazon.com/images/I/61y2VVWcGBL._AC_SY355_.jpg" },
      { id: 2, name: "سماعة بلوتوث", price: 150, category: "إلكترونيات", image: "https://m.media-amazon.com/images/I/51N5f1TmPOL._AC_SX679_.jpg" },
      { id: 3, name: "تيشيرت رجالي", price: 90, category: "ملابس", image: "https://m.media-amazon.com/images/I/61C1f2aDtbL._AC_UX679_.jpg" },
      { id: 4, name: "حذاء رياضي", price: 180, category: "أحذية", image: "https://m.media-amazon.com/images/I/71v5LdoF8LL._AC_UX679_.jpg" },
      { id: 5, name: "هاتف سامسونج S21", price: 2200, category: "هواتف", image: "https://m.media-amazon.com/images/I/81ZSn2rk9bL._AC_SX679_.jpg" },
      { id: 6, name: "بنطال جينز", price: 120, category: "ملابس", image: "https://m.media-amazon.com/images/I/71GC9Hh9AML._AC_UX679_.jpg" },
      { id: 7, name: "سلسلة نسائية", price: 85, category: "إكسسوارات", image: "https://m.media-amazon.com/images/I/61Ty0WwX8+L._AC_UL1500_.jpg" },
      { id: 8, name: "هاتف آيفون 13", price: 3200, category: "هواتف", image: "https://m.media-amazon.com/images/I/71f5Eu5lJSL._AC_SX679_.jpg" },
      { id: 9, name: "ساعة رجالية فاخرة", price: 540, category: "ساعات", image: "https://m.media-amazon.com/images/I/61aT8jl8THL._AC_SY355_.jpg" },
      { id: 10, name: "سماعة رأس احترافية", price: 360, category: "إلكترونيات", image: "https://m.media-amazon.com/images/I/61u48FEsYtL._AC_SX679_.jpg" },
      { id: 11, name: "نظارة شمسية", price: 130, category: "إكسسوارات", image: "https://m.media-amazon.com/images/I/61+64BBaxpL._AC_UL1500_.jpg" },
      { id: 12, name: "حقيبة يد نسائية", price: 250, category: "إكسسوارات", image: "https://m.media-amazon.com/images/I/71P+u0VjWQL._AC_UL1500_.jpg" }
    ];

    let cart = [];
    let currentCategory = 'all';

    function renderProducts() {
      const container = document.getElementById("products");
      container.innerHTML = "";
      const filtered = currentCategory === 'all' ? products : products.filter(p => p.category === currentCategory);
      filtered.forEach(product => {
        const div = document.createElement("div");
        div.className = "product";
        div.innerHTML = `
          <img src="${product.image}" alt="${product.name}">
          <h3>${product.name}</h3>
          <p>${product.price} ريال</p>
          <button onclick="addToCart(${product.id})">أضف إلى السلة</button>
        `;
        container.appendChild(div);
      });
    }

    function filterCategory(category) {
      currentCategory = category;
      document.querySelectorAll('nav button').forEach(btn => btn.classList.remove('active'));
      event.target.classList.add('active');
      document.getElementById("about").style.display = "none";
      document.getElementById("products").style.display = "grid";
      renderProducts();
    }

    function showAbout() {
      document.getElementById("products").style.display = "none";
      document.getElementById("about").style.display = "block";
      document.querySelectorAll('nav button').forEach(btn => btn.classList.remove('active'));
    }

    function addToCart(id) {
      const product = products.find(p => p.id === id);
      cart.push(product);
      updateCart();
    }

    function updateCart() {
      document.getElementById("cart-count").textContent = cart.length;
      const items = document.getElementById("cart-items");
      const total = document.getElementById("cart-total");
      items.innerHTML = "";
      let sum = 0;
      cart.forEach(item => {
        const li = document.createElement("li");
        li.textContent = `${item.name} - ${item.price} ريال`;
        items.appendChild(li);
        sum += item.price;
      });
      total.textContent = sum;
    }

    function toggleCart() {
      const cartEl = document.getElementById("cart");
      cartEl.style.display = cartEl.style.display === "none" ? "block" : "none";
    }

    renderProducts();
  </script>
</body>
</html>
