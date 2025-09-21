# VE_MV
Florist shop
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Profile Shop — Liquid Glass</title>
  <style>
    /* Basic reset */
    *{box-sizing:border-box;margin:0;padding:0}
    html,body{height:100%;background:
      linear-gradient(180deg, #0f1724 0%, #071226 100%);
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      color:#e9eef8; -webkit-font-smoothing:antialiased;}
    .container{max-width:980px;margin:40px auto;padding:20px}

    /* Glass card wrapper */
    .card {
      background: linear-gradient(180deg, rgba(255,255,255,0.04), rgba(255,255,255,0.02));
      border-radius:20px;
      padding:0;
      overflow:hidden;
      backdrop-filter: blur(14px) saturate(1.1);
      -webkit-backdrop-filter: blur(14px) saturate(1.1);
      border: 1px solid rgba(255,255,255,0.06);
      box-shadow: 0 10px 30px rgba(3,7,18,0.6);
    }

    /* Cover and profile layout */
    .cover {
      position:relative;
      height:220px;
      background-position:center;
      background-size:cover;
      display:flex;
      align-items:flex-end;
      justify-content:center;
    }
    .cover::after{
      /* subtle gradient on top of cover */
      content:"";
      position:absolute;left:0;right:0;top:0;bottom:0;
      background:linear-gradient(180deg, rgba(2,6,23,0.3), rgba(2,6,23,0.2));
      pointer-events:none;
    }

    .profile-row{
      position:relative;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:28px 24px 12px;
      gap:18px;
      transform:translateY(-54px);
    }

    .avatar {
      width:128px;height:128px;border-radius:50%;
      overflow:hidden;border:4px solid rgba(255,255,255,0.9);
      box-shadow: 0 8px 24px rgba(2,8,24,0.6);
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    }
    .avatar img{width:100%;height:100%;object-fit:cover;display:block}

    .profile-info{ text-align:center; margin-top:6px; transform:translateY(6px) }
    .display-name{font-size:22px;font-weight:700; color:#f3f7ff}
    .about{margin-top:10px;color:#dbe7ff;opacity:0.92; font-size:15px; max-width:720px; margin-left:auto;margin-right:auto; line-height:1.4; padding:8px 12px; background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.00)); border-radius:12px}

    /* Products grid */
    .section {
      padding:18px 22px 28px;
    }
    .section-title{font-weight:700;font-size:16px;color:#dcecff;margin-bottom:12px}
    .categories {
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      margin-bottom:18px;
    }
    .cat-btn{
      padding:8px 12px;border-radius:999px;border:1px solid rgba(255,255,255,0.06);
      background:rgba(255,255,255,0.02);cursor:pointer;font-weight:600;color:#eaf3ff;
      backdrop-filter: blur(6px);
    }
    .cat-btn.active{
      background: linear-gradient(90deg, rgba(255,255,255,0.06), rgba(255,255,255,0.03));
      box-shadow: 0 6px 18px rgba(0,0,0,0.5);
      transform:translateY(-2px)
    }

    .products-grid{
      display:grid;
      grid-template-columns: repeat(auto-fill,minmax(200px,1fr));
      gap:14px;
    }
    .prod {
      border-radius:14px;padding:8px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border:1px solid rgba(255,255,255,0.04);backdrop-filter: blur(10px);
      display:flex;flex-direction:column;gap:8px;min-height:220px;
    }
    .prod .img{height:120px;border-radius:10px;overflow:hidden;background:#0a1220}
    .prod img{width:100%;height:100%;object-fit:cover;display:block}
    .p-title{font-weight:700;font-size:15px}
    .p-price{margin-top:auto;font-weight:800;color:#b8f6c0}
    .p-desc{font-size:13px;color:#cfe8ff;opacity:0.88}

    /* small screens */
    @media (max-width:520px){
      .avatar{width:96px;height:96px}
      .profile-row{padding:18px}
      .cover{height:170px}
    }

    /* subtle nice scrollbar */
    ::-webkit-scrollbar{height:8px;width:8px}
    ::-webkit-scrollbar-thumb{background:rgba(255,255,255,0.06);border-radius:8px}
  </style>
</head>
<body>
  <div class="container">
    <div class="card" id="mainCard">
      <div class="cover" id="cover" style="background-image: linear-gradient(45deg, rgba(10,14,30,0.35), rgba(5,8,14,0.55));">
        <!-- cover image set by JS -->
      </div>

      <div class="profile-row">
        <div class="avatar" id="avatar">
          <img src="" alt="profile">
        </div>

        <div class="profile-info">
          <div class="display-name" id="displayName">Your Name</div>
          <div class="about" id="aboutUs">About us text will show here. Update it in Google Sheets (Profile sheet).</div>
        </div>
      </div>

      <div class="section">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <div class="section-title">Products</div>
            <div style="font-size:13px;color:#a9c9ff; margin-top:4px">Click a category to filter</div>
          </div>
          <div style="text-align:right">
            <div style="font-size:13px;color:#bcd8ff">Font: <span id="fontName">System</span></div>
          </div>
        </div>

        <div class="categories" id="categoryButtons">
          <!-- generated -->
        </div>

        <div class="products-grid" id="productsGrid">
          <!-- generated -->
        </div>
      </div>
    </div>
  </div>

  <script>
    /****************************************************************
     * CONFIG - replace with your published Google Sheet ID
     *
     * Steps:
     * 1. File -> Publish to the web (choose entire document) in Google Sheets.
     * 2. Make sure the sheet is viewable by anyone (public).
     * 3. Replace SHEET_ID below with your sheet's ID (the long string in the URL).
     *
     ****************************************************************/
    const SHEET_ID = 'YOUR_SHEET_ID_HERE'; // <-- REPLACE THIS
    const PROFILE_SHEET = 'Profile';
    const PRODUCTS_SHEET = 'Products';

    // Utility to fetch Google Sheets via the gviz endpoint and parse JSON
    async function fetchSheet(sheetName) {
      const url = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?sheet=${encodeURIComponent(sheetName)}&tqx=out:json`;
      const res = await fetch(url);
      const txt = await res.text();
      // the response is like: /*O_o*/\ngoogle.visualization.Query.setResponse({...});
      const jsonText = txt.replace(/^[^\(]*\(|\);?$/g, ''); // strip wrapper
      const data = JSON.parse(jsonText);
      const cols = data.table.cols.map(c => c.label || c.id);
      const rows = data.table.rows.map(r => {
        const obj = {};
        r.c.forEach((cell, i) => {
          obj[cols[i]] = cell ? cell.v : '';
        });
        return obj;
      });
      return rows;
    }

    // Load Google font dynamically
    function loadGoogleFont(fontFamily) {
      if (!fontFamily) return;
      // sanitize: replace spaces with +
      const ff = fontFamily.trim().replace(/\s+/g, '+');
      // Avoid loading duplicate
      if (document.getElementById('gf-' + ff)) return;
      const link = document.createElement('link');
      link.id = 'gf-' + ff;
      link.rel = 'stylesheet';
      link.href = `https://fonts.googleapis.com/css2?family=${ff}:wght@300;400;600;700;800&display=swap`;
      document.head.appendChild(link);
      // apply after small delay to ensure font loads
      link.onload = () => {
        document.body.style.fontFamily = `'${fontFamily}', system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial`;
      };
      // set immediately as fallback
      document.body.style.fontFamily = `'${fontFamily}', system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial`;
    }

    // Rendering functions
    function setProfile(profile) {
      if (!profile) return;
      const cover = document.getElementById('cover');
      const avatarImg = document.querySelector('#avatar img');
      const aboutUs = document.getElementById('aboutUs');
      const displayName = document.getElementById('displayName');
      const fontName = document.getElementById('fontName');

      if (profile.Cover_Photo) {
        cover.style.backgroundImage = `url('${profile.Cover_Photo}')`;
      }
      if (profile.Profile_Photo) {
        avatarImg.src = profile.Profile_Photo;
      } else {
        avatarImg.src = 'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="400" height="400"><rect width="100%" height="100%" fill="%230b1220"/><text x="50%" y="55%" fill="%23bcd8ff" font-size="40" text-anchor="middle" font-family="Arial">Profile</text></svg>';
      }
      if (profile.About_Us) aboutUs.textContent = profile.About_Us;
      if (profile.Display_Name) displayName.textContent = profile.Display_Name;
      if (profile.Font_Family) {
        fontName.textContent = profile.Font_Family;
        loadGoogleFont(profile.Font_Family);
      } else {
        fontName.textContent = 'System';
      }
    }

    function renderProducts(products) {
      const grid = document.getElementById('productsGrid');
      const catButtons = document.getElementById('categoryButtons');
      grid.innerHTML = '';
      catButtons.innerHTML = '';

      if (!products || products.length === 0) {
        grid.innerHTML = `<div style="padding:16px;color:#cfe8ff">No products found. Add rows to the 'Products' sheet.</div>`;
        return;
      }

      // normalize categories
      const categories = [...new Set(products.map(p => (p.Category || 'Uncategorized').trim()))];
      // create category buttons
      const allBtn = document.createElement('button');
      allBtn.className = 'cat-btn active';
      allBtn.textContent = 'All';
      allBtn.dataset.cat = 'All';
      catButtons.appendChild(allBtn);

      categories.forEach(cat => {
        const b = document.createElement('button');
        b.className = 'cat-btn';
        b.textContent = cat || 'Uncategorized';
        b.dataset.cat = cat || 'Uncategorized';
        catButtons.appendChild(b);
      });

      // click handler
      catButtons.addEventListener('click', (e) => {
        const btn = e.target.closest('button');
        if (!btn) return;
        [...catButtons.querySelectorAll('.cat-btn')].forEach(x => x.classList.remove('active'));
        btn.classList.add('active');
        const selected = btn.dataset.cat;
        showProducts(selected === 'All' ? products : products.filter(p => (p.Category||'').trim() === selected));
      });

      // initial render all
      showProducts(products);
    }

    function showProducts(items) {
      const grid = document.getElementById('productsGrid');
      grid.innerHTML = '';
      if (!items || items.length === 0) {
        grid.innerHTML = `<div style="padding:16px;color:#cfe8ff">No items in this category.</div>`;
        return;
      }
      items.forEach(it => {
        const card = document.createElement('div');
        card.className = 'prod';
        card.innerHTML = `
          <div class="img"><img src="${it.Image_URL || ''}" alt="${escapeHtml(it.Product_Name || '')}" onerror="this.src='data:image/svg+xml;utf8,<svg xmlns=\\'http://www.w3.org/2000/svg\\' width=\\'800\\' height=\\'400\\'><rect width=\\'100%\\' height=\\'100%\\' fill=\\'%230b1220\\' /><text x=\\'50%\\' y=\\'50%\\' fill=\\'%23bcd8ff\\' font-size=\\'28\\' font-family=\\'Arial\\' text-anchor=\\'middle\\'>No Image</text></svg>'"></div>
          <div class="p-title">${escapeHtml(it.Product_Name || 'Untitled')}</div>
          <div class="p-desc">${escapeHtml(it.Description || '')}</div>
          <div class="p-price">${formatPrice(it.Price)}</div>
        `;
        grid.appendChild(card);
      });
    }

    function formatPrice(v) {
      if (!v && v !== 0) return '';
      // try to keep numeric
      const num = parseFloat(String(v).replace(/[^0-9.\-]/g,''));
      if (isNaN(num)) return escapeHtml(String(v));
      // simple format, no locale to keep consistent
      return '$' + (Math.round(num * 100) / 100).toFixed(2);
    }

    function escapeHtml(s) {
      if (!s && s !== 0) return '';
      return String(s)
        .replaceAll('&','&amp;')
        .replaceAll('<','&lt;')
        .replaceAll('>','&gt;')
        .replaceAll('"','&quot;')
        .replaceAll("'",'&#39;');
    }

    // Main fetch & init
    async function init() {
      if (SHEET_ID === '(https://docs.google.com/spreadsheets/d/1tgN4-4K3-vWcYICry2v7BUjKY4-9SDXZ/edit?usp=drivesdk&ouid=116195287637846477691&rtpof=true&sd=true)') {
        // helpful message in page if user forgot to replace id
        document.getElementById('aboutUs').textContent = '⚠️ Please replace SHEET_ID in the script with your Google Sheet ID and publish the sheet to the web.';
        return;
      }

      try {
        // fetch both sheets in parallel
        const [profileRows, productRows] = await Promise.all([
          fetchSheet(PROFILE_SHEET),
          fetchSheet(PRODUCTS_SHEET)
        ]);
        // profile: use first row
        const profile = profileRows && profileRows.length ? profileRows[0] : null;
        // products: the fetch returns arrays of objects with column names keys as headers
        renderProducts(productRows);
        setProfile(profile);
      } catch (err) {
        console.error('Error loading sheets', err);
        document.getElementById('aboutUs').textContent = 'Unable to load data from Google Sheets. Check network, sheet ID, and that the sheet is published to the web and public.';
      }
    }

    // Start
    init();
  </script>
</body>
</html>
