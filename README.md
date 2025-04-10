<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Note Keeper</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>Note Keeper</h1>
    <textarea id="noteInput" placeholder="Write your note here..."></textarea>
    <button onclick="saveNote()">Save Note</button>
    <h2>Saved Notes</h2>
    <ul id="notesList"></ul>
  </div>

  <script src="script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Mountain Dew Shop</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>Mountain Dew Shop</h1>
    <div id="cart-count">Cart: 0</div>
  </header>

  <main>
    <div class="product" data-name="Mountain Dew Original" data-price="1.99">
      <img src="https://upload.wikimedia.org/wikipedia/en/8/8a/Mountain_Dew_logo.svg" alt="Mountain Dew">
      <h2>Mountain Dew Original</h2>
      <p>$1.99</p>
      <button onclick="addToCart(this)">Add to Cart</button>
    </div>

    <div class="product" data-name="Mountain Dew Code Red" data-price="2.49">
      <img src="https://upload.wikimedia.org/wikipedia/en/2/25/Mountain_Dew_Code_Red.png" alt="Code Red">
      <h2>Mountain Dew Code Red</h2>
      <p>$2.49</p>
      <button onclick="addToCart(this)">Add to Cart</button>
    </div>

    <div class="product" data-name="Mountain Dew Baja Blast" data-price="2.79">
      <img src="https://upload.wikimedia.org/wikipedia/en/4/4a/Baja_Blast.png" alt="Baja Blast">
      <h2>Mountain Dew Baja Blast</h2>
      <p>$2.79</p>
      <button onclick="addToCart(this)">Add to Cart</button>
    </div>
  </main>

  <footer>
    <p>&copy; 2025 Mountain Dew Shop. All rights reserved.</p>
  </footer>

  <script src="script.js"></script>
</body>
</html>