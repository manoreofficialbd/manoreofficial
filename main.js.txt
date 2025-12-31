// ==========================
// Add to Cart - image.html
// ==========================
const addButtons = document.querySelectorAll('.add-to-cart');

addButtons.forEach(btn => {
  btn.addEventListener('click', (e) => {
    const name = btn.getAttribute('data-name');
    const price = parseInt(btn.getAttribute('data-price'));
    
    // Get existing cart from localStorage
    let cart = JSON.parse(localStorage.getItem('cart')) || [];

    const existing = cart.find(item => item.name === name);
    if (existing) {
      existing.quantity += 1;
    } else {
      cart.push({name, price, quantity: 1});
    }

    localStorage.setItem('cart', JSON.stringify(cart));
    alert(`${name} added to cart!`);
  });
});

// ==========================
// Cart Page - cart.html
// ==========================
const cartContainer = document.querySelector('#cart-items');
const cartTotal = document.querySelector('#cart-total');
const checkoutBtn = document.querySelector('#checkout-btn');

let cart = JSON.parse(localStorage.getItem('cart')) || [];

function renderCart() {
  if (!cartContainer) return;

  cartContainer.innerHTML = '';
  let total = 0;

  cart.forEach(item => {
    total += item.price * item.quantity;
    cartContainer.innerHTML += `
      <div class="cart-item">
        <span>${item.name} x ${item.quantity}</span>
        <span>৳ ${item.price * item.quantity}</span>
      </div>
    `;
  });

  cartTotal.innerText = `Total: ৳ ${total}`;
}

renderCart();

// Checkout Button
if (checkoutBtn) {
  checkoutBtn.addEventListener('click', () => {
    if (cart.length === 0) {
      alert('Cart is empty!');
      return;
    }

    let summary = 'Order Summary:\n';
    let total = 0;

    cart.forEach(item => {
      summary += `${item.name} x ${item.quantity} = ৳ ${item.price * item.quantity}\n`;
      total += item.price * item.quantity;
    });

    summary += `Total: ৳ ${total}\n\nThank you for your order!`;
    alert(summary);

    // Clear cart
    cart = [];
    localStorage.setItem('cart', JSON.stringify(cart));
    renderCart();
  });
});
