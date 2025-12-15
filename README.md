
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>TOPKI — Product</title>
  <link rel="stylesheet" href="assets/css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container header-inner">
      <a class="logo" href="index.html">TOPKI</a>
      <nav class="main-nav">
        <a href="index.html#products">Products</a>
        <a href="index.html#contact">Contact:+254110405353
        <button id="cart-toggle" class="cart-btn">cart (<span id="cart-count">0</span>)</button>
      
    
  

  <main class="container section" id="product-page">
    <!-- Content populated by JS using ?id=productId in URL -->
  </main>

  <footer class="site-footer">
    <div class="container">
      <p>&copy; <span id="year"></span> TOPKI — Hospital Supplies</p>
    </div>
  </footer>

  <aside id="cart-drawer" class="cart-drawer" aria-hidden="true">
    <div class="cart-inner">
      <button id="cart-close" class="cart-close">&times;</button>
      <h3>Your Cart</h3>
      <div id="cart-items" class="cart-items"></div>
      <div class="cart-summary">
        <div>Total: <strong id="cart-total">KES 0.00</strong></div>
        <button id="checkout-btn" class="btn primary">Checkout</button>
      </div>
    </div>
  </aside>

  <script src="assets/js/products.js"></script>
  <script src="assets/js/cart.js"></script>
  <script>
    document.getElementById('year').textContent = new Date().getFullYear();

    // Render product by id (from query string)
    (function() {
      const params = new URLSearchParams(location.search);
      const id = params.get('id');
      const product = products.find(p => p.id === id) || products[0];
      const el = document.getElementById('product-page');
      el.innerHTML = `
        <div class="product-detail">
          <img class="product-detail-img" src="${product.image}" alt="${product.name}">
          <div class="product-detail-info">
            <h1>${product.name}</h1>
            <p class="price">KES ${product.price.toFixed(2)}</p>
            <p>${product.description}</p>
            <label>
              Quantity
              <input id="qty" type="number" value="1" min="1">
            </label>
            <div>
              <button id="add-to-cart" class="btn primary">Add to Cart</button>
              <a href="index.html#products" class="btn">Back to products</a>
            </div>
          </div>
        </div>
      `;
      document.getElementById('add-to-cart').addEventListener('click', () => {
        const qty = parseInt(document.getElementById('qty').value, 10) || 1;
        addToCart(product.id, qty);
        alert('Added to cart');
      });
    })();
  </script>

