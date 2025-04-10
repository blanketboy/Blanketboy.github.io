let cart = JSON.parse(localStorage.getItem("cart")) || [];

function addToCart(button) {
  const product = button.closest('.product');
  const name = product.dataset.name;
  const price = parseFloat(product.dataset.price);

  cart.push({ name, price });
  localStorage.setItem("cart", JSON.stringify(cart));
  updateCartCount();
}

function updateCartCount() {
  document.getElementById("cart-count").textContent = `Cart: ${cart.length}`;
}

window.onload = updateCartCount;
let cart = JSON.parse(localStorage.getItem("cart")) || [];

function addToCart(button) {
  const product = button.closest('.product');
  const name = product.dataset.name;
  const price = parseFloat(product.dataset.price);

  const existingItem = cart.find(item => item.name === name);

  if (existingItem) {
    existingItem.quantity += 1;
  } else {
    cart.push({ name, price, quantity: 1 });
  }

  localStorage.setItem("cart", JSON.stringify(cart));
  updateCartUI();
}

function updateCartUI() {
  const cartList = document.getElementById("cart-items");
  const cartTotal = document.getElementById("cart-total");
  const cartCount = document.getElementById("cart-count");

  cartList.innerHTML = "";
  let total = 0;
  let itemCount = 0;

  cart.forEach(item => {
    const li = document.createElement("li");
    li.textContent = `${item.name} x${item.quantity} - $${(item.price * item.quantity).toFixed(2)}`;
    cartList.appendChild(li);
    total += item.price * item.quantity;
    itemCount += item.quantity;
  });

  cartTotal.textContent = `Total: $${total.toFixed(2)}`;
  if (cartCount) cartCount.textContent = `Cart: ${itemCount}`;
}

window.onload = updateCartUI;
<!-- Add this after your product divs inside <main> -->
<h2>Shopping Cart</h2>
<div id="cart-container">
  <ul id="cart-items"></ul>
  <p id="cart-total">Total: $0.00</p>
</div>