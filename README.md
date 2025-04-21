# Spend-Elon-Musk-Money-
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Spend Elon Musk's Money</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #f0f2f5;
      margin: 0;
      padding: 0;
      color: #333;
    }
    header {
      background: #20232a;
      color: white;
      padding: 20px;
      text-align: center;
      position: sticky;
      top: 0;
      z-index: 999;
    }
    header img {
      height: 50px;
      vertical-align: middle;
      border-radius: 50%;
      margin: 0 10px;
    }
    .balance {
      font-size: 24px;
      margin: 10px 0;
    }
    .progress-bar-container {
      background: #ccc;
      height: 20px;
      border-radius: 10px;
      overflow: hidden;
      width: 60%;
      margin: 10px auto;
    }
    .progress-bar {
      height: 100%;
      background: #4caf50;
      width: 0%;
      transition: width 0.3s;
    }
    #items {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 20px;
    }
    .item {
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      margin: 10px;
      padding: 15px;
      width: 200px;
      text-align: center;
      transition: transform 0.2s ease;
    }
    .item img {
      height: 80px;
    }
    .item button {
      margin: 5px;
      padding: 5px 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .item button.buy {
      background: #4caf50;
      color: white;
    }
    .item button.sell {
      background: #f44336;
      color: white;
    }
    .animate-pop {
      animation: pop 0.3s ease-in-out;
    }
    @keyframes pop {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }
  </style>
</head>
<body>
  <header>
    <img src="https://upload.wikimedia.org/wikipedia/commons/e/e8/Elon_Musk_2015.jpg" alt="Elon" />
    <strong>Spend Elon Musk's Money</strong>
    <img src="https://upload.wikimedia.org/wikipedia/commons/a/a0/Bill_Gates_2018.jpg" alt="Bill" />
    <div class="balance" id="balance">Balance: $1,000,000,000,000</div>
    <div class="progress-bar-container">
      <div class="progress-bar" id="progressBar"></div>
    </div>
    <select id="category" onchange="filterItems()">
      <option value="all">All</option>
      <option value="luxury">Luxury</option>
      <option value="tech">Tech</option>
      <option value="transport">Transport</option>
      <option value="fun">Fun</option>
    </select>
  </header>

  <div id="items"></div>

  <div style="padding: 30px; background: #fff;">
    <h2>Purchase Summary</h2>
    <ul id="receipt" style="line-height: 1.6;"></ul>
  </div>

  <audio id="buy-sound" src="https://www.myinstants.com/media/sounds/cash-register.mp3"></audio>
  <audio id="sell-sound" src="https://www.myinstants.com/media/sounds/coin.mp3"></audio>

  <script>
    const items = [
      { name: 'Big Mac', price: 2, image: 'https://img.icons8.com/color/96/hamburger.png', category: 'fun' },
      { name: 'Tesla', price: 75000, image: 'https://img.icons8.com/color/96/tesla-model-x.png', category: 'transport' },
      { name: 'Private Jet', price: 75000000, image: 'https://img.icons8.com/ios-filled/100/private-jet.png', category: 'luxury' },
      { name: 'Mansion', price: 45000000, image: 'https://img.icons8.com/color/96/cottage.png', category: 'luxury' },
      { name: 'VIP House', price: 100000000, image: 'https://img.icons8.com/color/96/luxury-home.png', category: 'luxury' },
      { name: 'Diamond Ring', price: 10000, image: 'https://img.icons8.com/color/96/diamond-ring.png', category: 'luxury' },
      { name: 'Rolex', price: 15000, image: 'https://img.icons8.com/ios/50/000000/wristwatch.png', category: 'luxury' },
      { name: 'Superyacht', price: 7500000, image: 'https://img.icons8.com/color/96/yacht.png', category: 'luxury' },
      { name: 'VIP Yacht', price: 150000000, image: 'https://img.icons8.com/color/96/yacht.png', category: 'luxury' },
      { name: 'Private Island', price: 500000000, image: 'https://img.icons8.com/color/96/island-on-water.png', category: 'luxury' },
      { name: 'Gaming PC', price: 2000, image: 'https://img.icons8.com/color/96/gaming.png', category: 'tech' },
      { name: 'Book', price: 15, image: 'https://img.icons8.com/color/96/book.png', category: 'fun' },
      { name: 'Helicopter', price: 18000000, image: 'https://img.icons8.com/color/96/helicopter.png', category: 'transport' },
      { name: 'Cruise Ship', price: 930000000, image: 'https://img.icons8.com/color/96/cruise-ship.png', category: 'luxury' },
      { name: 'NBA Team', price: 2200000000, image: 'https://img.icons8.com/color/96/basketball.png', category: 'fun' },
      { name: 'Movie Studio', price: 3000000000, image: 'https://img.icons8.com/color/96/movie-projector.png', category: 'fun' },
      { name: 'SpaceX Ticket', price: 55000000, image: 'https://img.icons8.com/color/96/rocket.png', category: 'transport' },
      { name: 'Skyscraper', price: 1200000000, image: 'https://img.icons8.com/color/96/city-buildings.png', category: 'luxury' },
      { name: 'Tech Company', price: 15000000000, image: 'https://img.icons8.com/color/96/artificial-intelligence.png', category: 'tech' }
    ];

    let balance = 1_000_000_000_000;
    const total = balance;
    const itemsEl = document.getElementById('items');
    const balanceEl = document.getElementById('balance');
    const progressBar = document.getElementById('progressBar');
    const receiptEl = document.getElementById('receipt');
    const quantities = {};

    function updateUI() {
      balanceEl.textContent = `Balance: $${balance.toLocaleString()}`;
      const percentSpent = 100 - (balance / total) * 100;
      progressBar.style.width = `${percentSpent}%`;
      renderReceipt();
    }

    function renderItems(filter = 'all') {
      itemsEl.innerHTML = '';
      const filtered = filter === 'all' ? items : items.filter(i => i.category === filter);
      filtered.forEach(item => {
        const quantity = quantities[item.name] || 0;
        const card = document.createElement('div');
        card.className = 'item';
        card.innerHTML = `
          <img src="${item.image}" alt="${item.name}" />
          <h3>${item.name}</h3>
          <p>$${item.price.toLocaleString()}</p>
          <p>Owned: ${quantity}</p>
          <button class="buy">Buy</button>
          <button class="sell">Sell</button>
        `;

        card.querySelector('.buy').onclick = () => {
          if (balance >= item.price) {
            balance -= item.price;
            quantities[item.name] = quantity + 1;
            updateUI();
            renderItems(filter);
            card.classList.add('animate-pop');
            setTimeout(() => card.classList.remove('animate-pop'), 300);
            playSound('buy');
          }
        };

        card.querySelector('.sell').onclick = () => {
          if (quantity > 0) {
            balance += item.price;
            quantities[item.name] = quantity - 1;
            updateUI();
            renderItems(filter);
            card.classList.add('animate-pop');
            setTimeout(() => card.classList.remove('animate-pop'), 300);
            playSound('sell');
          }
        };

        itemsEl.appendChild(card);
      });
    }

    function playSound(type) {
      const sound = document.getElementById(`${type}-sound`);
      if (sound) sound.play();
    }

    function renderReceipt() {
      receiptEl.innerHTML = '';
      for (let [name, qty] of Object.entries(quantities)) {
        if (qty > 0) {
          const item = items.find(i => i.name === name);
          const li = document.createElement('li');
          li.textContent = `${name} x${qty} = $${(item.price * qty).toLocaleString()}`;
          receiptEl.appendChild(li);
        }
      }
    }

    function filterItems() {
      const category = document.getElementById('category').value;
      renderItems(category);
    }

    updateUI();
    renderItems();
  </script>
</body>
</html>
