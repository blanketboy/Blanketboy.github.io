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