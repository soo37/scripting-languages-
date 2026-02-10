# scripting-languages-
html and javascript e commerce project
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>The Bar Kenya | Online Liquor Store</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f4f4;
    }

    header {
      background: #000;
      color: #fff;
      padding: 15px;
      text-align: center;
    }

    .products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 20px;
      padding: 20px;
    }

    .product {
      background: #fff;
      padding: 15px;
      border-radius: 6px;
      text-align: center;
      box-shadow: 0 2px 6px rgba(0,0,0,0.15);
    }

    .product img {
      width: 100%;
      height: 180px;
      object-fit: contain;
    }

    button {
      background: #1db954;
      color: #fff;
      border: none;
      padding: 10px;
      width: 100%;
      border-radius: 4px;
      cursor: pointer;
      margin-top: 8px;
    }

    button:hover {
      background: #14833b;
    }

    .cart {
      background: #fff;
      margin: 20px;
      padding: 20px;
      border-radius: 6px;
    }

    .hidden {
      display: none;
    }

    input {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
    }

    footer {
      background: #000;
      color: #fff;
      text-align: center;
      padding: 15px;
    }

    #ageGate {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.95);
      color: white;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      z-index: 999;
      text-align: center;
      padding: 20px;
    }
  </style>
</head>
<body>

<!-- AGE GATE -->
<div id="ageGate">
  <h2>🔞 Age Restricted</h2>
  <p>You must be 18 years or older to access this site.</p>
  <button onclick="enterSite()">I am 18+</button>
</div>

<header>
  <h1>🍾 The Bar Kenya</h1>
  <p>Official Online Liquor Store | Drink Responsibly</p>
</header>

<!-- PRODUCTS -->
<section class="products" id="products"></section>

<!-- CART -->
<div class="cart">
  <h2>🛒 Shopping Cart</h2>
  <ul id="cartItems"></ul>
  <h3>Total: KES <span id="total">0</span></h3>
  <button onclick="checkout()">Checkout with M-Pesa</button>
</div>

<!-- CHECKOUT -->
<div class="cart hidden" id="checkoutSection">
  <h2>M-Pesa Checkout</h2>
  <input type="text" id="phone" placeholder="07XXXXXXXX">
  <button onclick="pay()">Pay Now</button>
</div>

<footer>
  © 2026 The Bar Kenya | 18+ Only
</footer>

<script>
  const products = [
    {
      name: "Tusker Lager 500ml",
      price: 260,
      img: "https://drinkiq.com/wp-content/uploads/2020/08/Tusker-Lager.png"
    },
    {
      name: "White Cap Lager 500ml",
      price: 250,
      img: "https://drinkiq.com/wp-content/uploads/2020/08/White-Cap-Lager.png"
    },
    {
      name: "Tusker Cider 330ml",
      price: 280,
      img: "https://drinkiq.com/wp-content/uploads/2020/08/Tusker-Cider.png"
    },
    {
      name: "Baileys Irish Cream 750ml",
      price: 3000,
      img: "https://www.diageobaracademy.com/uploads/images/baileys-original.png"
    },
    {
      name: "Smirnoff No.21 Vodka 750ml",
      price: 1800,
      img: "https://www.diageobaracademy.com/uploads/images/smirnoff-red.png"
    },
    {
      name: "Gilbey's Special Dry Gin 750ml",
      price: 2500,
      img: "https://www.diageobaracademy.com/uploads/images/gilbeys-gin.png"
    },
    {
      name: "Gordon's London Dry Gin 750ml",
      price: 2600,
      img: "https://www.diageobaracademy.com/uploads/images/gordons-gin.png"
    },
    {
      name: "J&B Rare Scotch Whisky 750ml",
      price: 3200,
      img: "https://www.diageobaracademy.com/uploads/images/jb-rare.png"
    },
    {
      name: "Johnnie Walker Red Label 750ml",
      price: 3400,
      img: "https://www.diageobaracademy.com/uploads/images/johnnie-walker-red.png"
    },
    {
      name: "Kenya Cane Smooth 750ml",
      price: 1800,
      img: "https://drinkiq.com/wp-content/uploads/2020/08/Kenya-Cane.png"
    }
  ];

  let cart = [];
  let total = 0;

  function enterSite() {
    document.getElementById("ageGate").style.display = "none";
  }

  const productsDiv = document.getElementById("products");

  products.forEach((p, i) => {
    const div = document.createElement("div");
    div.className = "product";
    div.innerHTML = `
      <img src="${p.img}" onerror="this.src='https://via.placeholder.com/200x180?text=No+Image'">
      <h3>${p.name}</h3>
      <p><strong>KES ${p.price}</strong></p>
      <button onclick="addToCart(${i})">Add to Cart</button>
    `;
    productsDiv.appendChild(div);
  });

  function addToCart(i) {
    cart.push(products[i]);
    total += products[i].price;
    renderCart();
  }

  function renderCart() {
    const list = document.getElementById("cartItems");
    list.innerHTML = "";
    cart.forEach(item => {
      const li = document.createElement("li");
      li.textContent = `${item.name} - KES ${item.price}`;
      list.appendChild(li);
    });
    document.getElementById("total").textContent = total;
  }

  function checkout() {
    if (cart.length === 0) {
      alert("Your cart is empty");
      return;
    }
    document.getElementById("checkoutSection").classList.remove("hidden");
  }

  function pay() {
    const phone = document.getElementById("phone").value;
    if (!phone.startsWith("07") || phone.length < 10) {
      alert("Enter a valid Safaricom number");
      return;
    }

    alert(`📲 M-Pesa STK Push Sent\n\nPhone: ${phone}\nAmount: KES ${total}\n\n(Demo Only)`);

    cart = [];
    total = 0;
    renderCart();
    document.getElementById("checkoutSection").classList.add("hidden");
    document.getElementById("phone").value = "";
  }
</script>

</body>
</html>
