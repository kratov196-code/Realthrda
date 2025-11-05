<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Masha — магазин</title>
  <style>
    :root{
      --violet: #6b21a8;
      --violet-2: #7c3aed;
      --bg: #f7f5fb;
      --card: #ffffff;
      --muted: #6b7280;
      --accent: #ef4444;
    }
    *{box-sizing:border-box}
    body{font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; margin:0; background:var(--bg); color:#111}
    header{background:linear-gradient(90deg,var(--violet),var(--violet-2)); color:#fff; padding:14px 18px; display:flex;align-items:center;justify-content:space-between;gap:12px}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:44px;height:44px;border-radius:10px;background:rgba(255,255,255,0.12);display:flex;align-items:center;justify-content:center;font-weight:700;font-size:18px}
    .brand h1{margin:0;font-size:18px}
    nav{display:flex;align-items:center;gap:10px}
    .nav-link{position:relative;padding:8px 10px;border-radius:8px;cursor:pointer}
    .nav-link:hover{background:rgba(255,255,255,0.06)}
    .mega{position:absolute;left:0;top:46px;min-width:560px;background:var(--card);box-shadow:0 8px 28px rgba(16,24,40,0.12);border-radius:10px;padding:14px;display:none;color:#111;z-index:40}
    .nav-link.mega-wrap:hover .mega{display:flex}
    .mega .col{flex:1;padding:8px}

    .container{max-width:1100px;margin:20px auto;padding:0 14px}
    .grid{display:grid;grid-template-columns:1fr 360px;gap:16px}
    .card{background:var(--card);padding:14px;border-radius:12px;box-shadow:0 6px 18px rgba(16,24,40,0.06)}
    .products{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:12px}
    .product{border-radius:10px;overflow:hidden;border:1px solid #eee;background:#fff;display:flex;flex-direction:column}
    .product img{width:100%;height:150px;object-fit:cover;display:block}
    .product .body{padding:10px;flex:1;display:flex;flex-direction:column}
    .product h3{margin:0;font-size:15px}
    .price{font-weight:700;margin-top:6px}
    button{cursor:pointer;border:0;border-radius:8px;padding:8px 12px}
    .btn-primary{background:var(--violet);color:#fff}
    .cart-btn{background:rgba(255,255,255,0.12);color:#fff;padding:8px 10px;border-radius:10px;display:flex;align-items:center;gap:8px}
    .cart-count{background:#fff;color:var(--violet);padding:4px 8px;border-radius:999px;font-weight:700}
    .aside{position:sticky;top:18px}
    .cart-list{max-height:380px;overflow:auto;margin-top:8px}
    .cart-item{display:flex;gap:8px;align-items:center;padding:8px;border-radius:8px}
    .cart-item img{width:64px;height:64px;object-fit:cover;border-radius:6px}
    .small{font-size:13px;color:var(--muted)}
    .hint{font-size:13px;color:var(--muted);margin-top:8px}

    /* responsive */
    @media (max-width:900px){
      .grid{grid-template-columns:1fr;}
      .aside{position:relative;top:0}
      .mega{left:auto;right:14px;min-width:320px}
      header{flex-direction:column;align-items:flex-start;gap:8px}
    }
    @media (max-width:480px){
      .product img{height:140px}
      header{padding:12px}
      .logo{width:40px;height:40px}
      .brand h1{font-size:16px}
    }

    /* small utilities */
    .search{display:flex;gap:8px;margin-bottom:12px}
    .search input{flex:1;padding:8px;border-radius:8px;border:1px solid #ddd}
    .badge{background:var(--violet);color:#fff;padding:4px 8px;border-radius:8px;font-weight:700}
    .actions{display:flex;gap:8px;margin-top:auto}
    .muted{color:var(--muted)}
    .flex{display:flex;gap:8px;align-items:center}

    /* modal */
    .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,0.4);z-index:60}
    .modal.open{display:flex}
    .modal .box{width:92%;max-width:720px;background:#fff;border-radius:10px;padding:12px}

  </style>
</head>
<body>
  <header>
    <div class="brand">
      <div class="logo">M</div>
      <div>
        <h1>Masha</h1>
        <div class="small">Интернет‑магазин </div>
      </div>
    </div>
    <nav style="margin-left:auto;display:flex;align-items:center;gap:10px">
      <div class="nav-link mega-wrap">Мега‑меню
        <div class="mega">
          <div class="col">
            <h4>FaO</h4>
            <p class="small">Часто задаваемые вопросы и правила магазина.</p>
            <ul>
              <li><strong>Доставка:</strong> по договорённости</li>
              <li><strong>Возврат:</strong> 14 дней</li>
              <li><strong>Оплата:</strong> онлайн через ссылку</li>
            </ul>
          </div>
          <div class="col">
            <h4>Рынок</h4>
            <p class="small">Товары удобно просматривать и фильтровать.</p>
            <p><a href="#market">Перейти в Рынок →</a></p>
          </div>
        </div>
      </div>

      <div class="nav-link"><a href="#market" style="color:inherit;text-decoration:none">Рынок</a></div>
      <div id="cartToggle" class="nav-link" style="display:flex;align-items:center;gap:8px">
        <div class="cart-btn">Корзина <span id="cartCount" class="cart-count">0</span></div>
      </div>
    </nav>
  </header>

  <main class="container">
    <div class="grid">
      <section>
        <div class="card">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h2 id="market">Рынок</h2>
            <div class="flex">
              <div class="badge" id="productTotal">0</div>
              <div class="small" style="margin-left:6px">товаров</div>
            </div>
          </div>

          <div class="search">
            <input id="searchInput" placeholder="Поиск по товарам..." />
            <select id="filterCat">
              <option value="all">Все категории</option>
            </select>
            <button id="clearSearch">Сброс</button>
          </div>

          <p class="small">Товары</p>
          <div id="products" class="products"></div>
        </div>

        <div class="card" style="margin-top:12px">
          <h3>Информация</h3>
          <p class="small">Не много об
            <ul>
              <li>Покупка товара ниже или выше заданой суммы является пожертвованием </li>
              <li>...</li>
              <li>...</li>
              <li>...</li>
            </ul>
          </p>
        </div>
      </section>

      <aside class="aside">
        <div class="card">
          <h3>Корзина</h3>
          <div id="cartList" class="cart-list"></div>
          <div style="display:flex;justify-content:space-between;align-items:center;margin-top:12px">
            <div>
              <div class="small">Итого</div>
              <div id="cartTotal" style="font-weight:800">0 ₽</div>
            </div>
            <div style="display:flex;flex-direction:column;gap:8px">
              <button id="checkoutBtn" class="btn-primary">Оплатить</button>
              <button id="clearBtn">Очистить</button>
            </div>
          </div>
          <div class="hint"></div>
        </div>
      </aside>
    </div>
  </main>

  <div id="modal" class="modal"><div class="box" id="modalBox"></div></div>

  <footer style="text-align:center;padding:18px;color:var(--muted)">© Masha — фиолетовый магазин</footer>

  <script>
    // Платёжная ссылка — замените на вашу
    const PAY_LINK = 'https://example.com/checkout?amount=';

    // Товары. Добавьте или редактируйте прямо здесь. maxQty: null — без ограничений, число — максимум в корзине
    let PRODUCTS = [
      { id: 'm1', name: 'Футболка Masha', price: 799, img: 'https://via.placeholder.com/400x300?text=Футболка', category: 'Одежда', maxQty: null },
      { id: 'm2', name: 'Кепка Masha (бу)', price: 499, img: 'https://via.placeholder.com/400x300?text=Кепка+бу', category: 'Аксессуары', maxQty: 2 },
      { id: 'm3', name: 'Чехол Masha', price: 299, img: 'https://via.placeholder.com/400x300?text=Чехол', category: 'Аксессуары', maxQty: 5 }
    ];

    // Cart: [{id, qty}]
    let CART = JSON.parse(localStorage.getItem('masha_cart')||'[]');

    // UI elements
    const productsEl = document.getElementById('products');
    const cartCountEl = document.getElementById('cartCount');
    const cartListEl = document.getElementById('cartList');
    const cartTotalEl = document.getElementById('cartTotal');
    const checkoutBtn = document.getElementById('checkoutBtn');
    const clearBtn = document.getElementById('clearBtn');
    const searchInput = document.getElementById('searchInput');
    const filterCat = document.getElementById('filterCat');
    const productTotal = document.getElementById('productTotal');

    // Fill categories
    function populateCategories(){
      const cats = Array.from(new Set(PRODUCTS.map(p=>p.category||'Без категории')));
      cats.forEach(c=>{
        const opt = document.createElement('option'); opt.value=c; opt.textContent=c; filterCat.appendChild(opt);
      });
    }

    function renderProducts(list=PRODUCTS){
      productsEl.innerHTML='';
      list.forEach(p=>{
        const div = document.createElement('div'); div.className='product';
        div.innerHTML = `
          <img src="${p.img}" alt="${escapeHtml(p.name)}" />
          <div class="body">
            <h3>${escapeHtml(p.name)}</h3>
            <div class="small">Категория: ${escapeHtml(p.category||'—')}</div>
            <div class="price">${formatPrice(p.price)}</div>
            <div style="margin-top:8px;display:flex;gap:8px;align-items:center">
              <button data-id="${p.id}" class="btn-primary addBtn">В корзину</button>
              <button data-id="${p.id}" class="viewBtn">Подробнее</button>
              <div style="margin-left:auto" class="small">${p.maxQty ? 'max: '+p.maxQty : ''}</div>
            </div>
          </div>
        `;
        productsEl.appendChild(div);
      });
      productTotal.textContent = list.length;
      attachProductEvents();
    }

    function attachProductEvents(){
      document.querySelectorAll('.addBtn').forEach(b=>b.addEventListener('click',e=>{ addToCart(e.currentTarget.dataset.id); }));
      document.querySelectorAll('.viewBtn').forEach(b=>b.addEventListener('click',e=>{ openModal(e.currentTarget.dataset.id); }));
    }

    function addToCart(id){
      const p = PRODUCTS.find(x=>x.id===id); if(!p) return;
      const existing = CART.find(x=>x.id===id);
      const currentQty = existing ? existing.qty : 0;
      if(p.maxQty !== null && currentQty >= p.maxQty){ alert('Достигнут лимит для этого товара (max '+p.maxQty+')'); return; }
      if(existing) existing.qty++; else CART.push({id, qty:1});
      saveCart(); renderCart();
    }

    function saveCart(){ localStorage.setItem('masha_cart', JSON.stringify(CART)); }

    function renderCart(){
      cartListEl.innerHTML=''; let total=0; let count=0;
      if(CART.length===0){ cartListEl.innerHTML = '<div class="small">Корзина пуста</div>'; }
      CART.forEach(ci=>{
        const p = PRODUCTS.find(x=>x.id===ci.id); if(!p) return;
        total += p.price*ci.qty; count += ci.qty;
        const el = document.createElement('div'); el.className='cart-item';
        el.innerHTML = `
          <img src="${p.img}"/>
          <div style="flex:1">
            <div style="font-weight:700">${escapeHtml(p.name)}</div>
            <div class="small">${formatPrice(p.price)} × ${ci.qty}</div>
          </div>
          <div style="display:flex;flex-direction:column;gap:6px">
            <button data-id="${ci.id}" class="inc">+</button>
            <button data-id="${ci.id}" class="dec">-</button>
            <button data-id="${ci.id}" class="remove" style="background:transparent;color:var(--accent)">Удалить</button>
          </div>
        `;
        cartListEl.appendChild(el);
      });
      cartCountEl.textContent = count; cartTotalEl.textContent = formatPrice(total);
      // attach events
      cartListEl.querySelectorAll('.inc').forEach(b=>b.addEventListener('click',()=>{ changeQty(b.dataset.id,1); }));
      cartListEl.querySelectorAll('.dec').forEach(b=>b.addEventListener('click',()=>{ changeQty(b.dataset.id,-1); }));
      cartListEl.querySelectorAll('.remove').forEach(b=>b.addEventListener('click',()=>{ removeItem(b.dataset.id); }));
    }

    function changeQty(id, delta){
      const it = CART.find(x=>x.id===id); if(!it) return; const p = PRODUCTS.find(x=>x.id===id);
      let newQty = it.qty + delta;
      if(newQty <= 0){ CART = CART.filter(x=>x.id!==id); saveCart(); renderCart(); return; }
      if(p && p.maxQty !== null && newQty > p.maxQty){ alert('Нельзя добавить больше, чем max: '+p.maxQty); return; }
      it.qty = newQty; saveCart(); renderCart();
    }

    function removeItem(id){ CART = CART.filter(x=>x.id!==id); saveCart(); renderCart(); }

    clearBtn.addEventListener('click',()=>{ CART=[]; saveCart(); renderCart(); });

    checkoutBtn.addEventListener('click',()=>{
      let total = 0; const items = [];
      CART.forEach(ci=>{ const p = PRODUCTS.find(x=>x.id===ci.id); if(p){ total += p.price*ci.qty; items.push({id:p.id,name:p.name,qty:ci.qty,price:p.price}); } });
      if(total<=0){ alert('Корзина пустая'); return; }
      // Формируем ссылку: PAY_LINK + сумма и items JSON (url encoded)
      const url = PAY_LINK + encodeURIComponent(total) + '&items=' + encodeURIComponent(JSON.stringify(items));
      window.location.href = url;
    });

    // Search & filter
    searchInput.addEventListener('input', applyFilters);
    filterCat.addEventListener('change', applyFilters);
    document.getElementById('clearSearch').addEventListener('click',()=>{ searchInput.value=''; filterCat.value='all'; applyFilters(); });

    function applyFilters(){
      const q = searchInput.value.trim().toLowerCase(); const cat = filterCat.value;
      const filtered = PRODUCTS.filter(p=>{
        if(cat!=='all' && (p.category||'Без категории') !== cat) return false;
        if(!q) return true;
        return (p.name||'').toLowerCase().includes(q) || (p.category||'').toLowerCase().includes(q);
      });
      renderProducts(filtered);
    }

    // Modal
    const modal = document.getElementById('modal'); const modalBox = document.getElementById('modalBox');
    function openModal(id){ const p = PRODUCTS.find(x=>x.id===id); if(!p) return; modalBox.innerHTML = `
      <div style="display:flex;gap:12px;align-items:flex-start">
        <img src="${p.img}" style="width:220px;height:180px;object-fit:cover;border-radius:8px"/>
        <div style="flex:1">
          <h3>${escapeHtml(p.name)}</h3>
          <div class="small">Категория: ${escapeHtml(p.category||'—')}</div>
          <div style="margin-top:8px;font-weight:800">${formatPrice(p.price)}</div>
          <p class="small" style="margin-top:8px">Максимум в корзине: ${p.maxQty===null? 'неограниченно' : p.maxQty}</p>
          <div style="margin-top:12px;display:flex;gap:8px">
            <button class="btn-primary" id="modalAdd">Добавить в корзину</button>
            <button id="modalClose">Закрыть</button>
          </div>
        </div>
      </div>
    `;
      document.getElementById('modal').classList.add('open');
      document.getElementById('modalClose').addEventListener('click', closeModal);
      document.getElementById('modalAdd').addEventListener('click', ()=>{ addToCart(id); closeModal(); });
    }
    function closeModal(){ document.getElementById('modal').classList.remove('open'); }
    modal.addEventListener('click', (e)=>{ if(e.target === modal) closeModal(); });

    // helpers
    function formatPrice(n){ return new Intl.NumberFormat('ru-RU',{style:'currency',currency:'RUB',maximumFractionDigits:0}).format(n); }
    function escapeHtml(s){ return String(s).replace(/[&<>\"]/g,ch=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;' }[ch]||ch)); }

    // init
    populateCategories(); renderProducts(); renderCart();

    // scroll to cart
    document.getElementById('cartToggle').addEventListener('click',()=>{ const el = document.querySelector('.aside'); if(el) el.scrollIntoView({behavior:'smooth', block:'start'}); });

  </script>
</body>
</html>
