
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

  <!-- Adding a call-to-action -->
<section id="+254110405353">
  <h2>Want to collaborate?</h2>
  <p>Feel free to reach out or connect on social media!</p>
  <a href="mailto:datryspin.github.com" class="button">Get In Touch</a>
</section>
// Simple gallery + lightbox. Put your image filenames below.
// Add your images to assets/images/ (example: assets/images/photo1.jpg)

const IMAGES = [
  {src: 'assets/images/photo1.jpg', caption: 'Photo 1 — A beautiful scene'},
  {src: 'assets/images/photo2.jpg', caption: 'Photo 2 — Another view'},
  {src: 'assets/images/photo3.jpg', caption: 'Photo 3 — City lights'},
  {src: 'assets/images/photo4.jpg', caption: 'Photo 4 — Sunrise'},
  {src: 'assets/images/photo5.jpg', caption: 'Photo 5 — Mountains'},
  {src: 'assets/images/photo6.jpg', caption: 'Photo 6 — Closeup'},
  // Add more images here...
];

// Render gallery
const galleryEl = document.getElementById('gallery');
IMAGES.forEach((img, i) => {
  const card = document.createElement('button');
  card.className = 'card';
  card.setAttribute('aria-label', img.caption || `Image ${i+1}`);
  card.dataset.index = i;
  card.innerHTML = `
    <img loading="lazy" src="${img.src}" alt="${img.caption || 'Photo'}">
    <div class="meta">
      <div class="caption">${img.caption || ''}</div>
      <div class="small">${i+1}/${IMAGES.length}</div>
    </div>
  `;
  card.addEventListener('click', () => openLightbox(i));
  galleryEl.appendChild(card);
});

// Lightbox functionality
const lb = document.getElementById('lightbox');
const lbImage = document.getElementById('lb-image');
const lbCaption = document.getElementById('lb-caption');
let current = 0;

function openLightbox(index){
  current = index;
  lbImage.src = IMAGES[current].src;
  lbImage.alt = IMAGES[current].caption || '';
  lbCaption.textContent = IMAGES[current].caption || '';
  lb.setAttribute('aria-hidden', 'false');
  document.body.style.overflow = 'hidden';
  // preload neighbors
  preload(current + 1);
  preload(current - 1);
}

function closeLightbox(){
  lb.setAttribute('aria-hidden', 'true');
  document.body.style.overflow = '';
  lbImage.src = '';
}

function showNext(delta){
  current = (current + delta + IMAGES.length) % IMAGES.length;
  lbImage.src = IMAGES[current].src;
  lbImage.alt = IMAGES[current].caption || '';
  lbCaption.textContent = IMAGES[current].caption || '';
  preload(current + 1);
}

function preload(index){
  if(index < 0 || index >= IMAGES.length) return;
  const img = new Image();
  img.src = IMAGES[index].src;
}

// Controls
document.getElementById('lb-close').addEventListener('click', closeLightbox);
document.getElementById('lb-next').addEventListener('click', () => showNext(1));
document.getElementById('lb-prev').addEventListener('click', () => showNext(-1));

// Close on overlay click (but not on image)
lb.addEventListener('click', (e) => {
  if (e.target === lb) closeLightbox();
});

// Keyboard
document.addEventListener('keydown', (e) => {
  if (lb.getAttribute('aria-hidden') === 'false') {
    if (e.key === 'Escape') closeLightbox();
    if (e.key === 'ArrowRight') showNext(1);
    if (e.key === 'ArrowLeft') showNext(-1);
  }
});
