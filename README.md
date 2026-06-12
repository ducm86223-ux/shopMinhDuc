<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minh Đức Shop - Deal Shopee 2026</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root { --shopee-orange: #EE4D2D; --bg: #F5F5F5; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background-color: var(--bg); color: #333; }
        a { text-decoration: none; color: inherit; }

        /* Top Bar */
        .top-bar { background: #222; color: white; padding: 8px; font-size: 12px; text-align: center; }
        .top-bar span { margin: 0 10px; }

        /* Header */
        header { background: white; padding: 15px; text-align: center; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        .logo { font-size: 24px; font-weight: 800; color: var(--shopee-orange); text-transform: uppercase; }

        /* Thanh Menu Hạng Mục (Category Nav) */
        .cat-nav-wrapper { background: white; padding: 10px 0; border-bottom: 1px solid #eee; overflow-x: auto; white-space: nowrap; position: sticky; top: 65px; z-index: 999; }
        .cat-nav-wrapper::-webkit-scrollbar { display: none; }
        .cat-container { display: flex; justify-content: center; gap: 10px; padding: 0 15px; }
        .cat-btn { padding: 8px 18px; background: #f0f0f0; border-radius: 20px; font-size: 13px; font-weight: 600; cursor: pointer; transition: 0.2s; border: none; }
        .cat-btn.active { background: var(--shopee-orange); color: white; }

        /* Main Content */
        .container { max-width: 1200px; margin: 20px auto; padding: 0 15px; min-height: 60vh; }
        .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 15px; }

        /* Product Card */
        .card { background: white; border-radius: 8px; overflow: hidden; display: flex; flex-direction: column; transition: 0.3s; border: 1px solid #eee; }
        .card:hover { transform: translateY(-5px); box-shadow: 0 8px 20px rgba(0,0,0,0.1); }
        .img-box { width: 100%; aspect-ratio: 1/1; background: #f9f9f9; }
        .img-box img { width: 100%; height: 100%; object-fit: cover; }
        
        .info { padding: 12px; flex-grow: 1; display: flex; flex-direction: column; }
        .title { font-size: 14px; font-weight: 600; color: #333; margin-bottom: 8px; height: 38px; overflow: hidden; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; line-height: 1.4; }
        .old-p { text-decoration: line-through; color: #999; font-size: 12px; }
        .new-p { color: var(--shopee-orange); font-size: 18px; font-weight: 800; margin-bottom: 10px; }
        
        .btn-buy { background: var(--shopee-orange); color: white; text-align: center; padding: 10px; border-radius: 6px; font-weight: 700; font-size: 13px; }

        #status { text-align: center; padding: 50px; color: #888; }
        footer { background: #111; color: #777; padding: 40px 20px; text-align: center; margin-top: 50px; font-size: 13px; }

        @media (max-width: 600px) {
            .product-grid { grid-template-columns: repeat(2, 1fr); gap: 10px; }
            .cat-container { justify-content: flex-start; }
            header { padding: 10px; }
            .logo { font-size: 20px; }
        }
    </style>
</head>
<body>

    <div class="top-bar">
        <span>📞 0822611300</span> | <span>👤 Fb: Minh Đức</span>
    </div>

    <header>
        <a href="/" class="logo">MINH ĐỨC SHOP</a>
    </header>

    <div class="cat-nav-wrapper">
        <div class="cat-container" id="category-menu">
            <!-- Nút hạng mục sẽ tự động hiện ở đây -->
        </div>
    </div>

    <div class="container">
        <div id="status">🔄 Đang tải dữ liệu...</div>
        <div class="product-grid" id="product-list"></div>
    </div>

    <footer>
        <p>© 2026 MINH ĐỨC SHOP. All rights reserved.</p>
        <p style="font-size: 11px; margin-top: 5px; opacity: 0.5;">Cập nhật từ Google Sheets</p>
    </footer>

    <script>
        const SHEET_ID = '1vTSfjCVCxt7Cp5GqhN4wC5wg07VQNRuGW4d2zUtyMI4Hb03Vm7okSScD-2Ns37xLTEeE1t8wFfVybIB';
        const DATA_URL = `https://docs.google.com/spreadsheets/d/e/2PACX-${SHEET_ID}/pub?gid=0&single=true&output=csv`;

        let allProducts = []; // Lưu tất cả sản phẩm
        let categories = ["Tất cả"]; // Danh sách hạng mục

        async function init() {
            try {
                const response = await fetch(DATA_URL);
                const csvData = await response.text();
                const lines = csvData.split('\n').slice(1);
                
                lines.forEach(line => {
                    const cols = line.split(/,(?=(?:(?:[^"]*"){2})*[^"]*$)/);
                    if (cols.length >= 6) {
                        const cat  = cols[0]?.replace(/^"|"$/g, '').trim() || "Khác";
                        const name = cols[1]?.replace(/^"|"$/g, '').trim();
                        const link = cols[2]?.replace(/^"|"$/g, '').trim();
                        const img  = cols[3]?.replace(/^"|"$/g, '').trim();
                        const old  = cols[4]?.replace(/^"|"$/g, '').trim();
                        const newP = cols[5]?.replace(/^"|"$/g, '').trim();

                        if (name && img && img.includes('http')) {
                            allProducts.push({ cat, name, link, img, old, newP });
                            if (!categories.includes(cat)) categories.push(cat);
                        }
                    }
                });

                renderMenu();
                renderProducts("Tất cả");
                document.getElementById('status').style.display = 'none';
            } catch (e) {
                document.getElementById('status').innerHTML = "❌ Lỗi tải dữ liệu.";
            }
        }

        function renderMenu() {
            const menu = document.getElementById('category-menu');
            menu.innerHTML = categories.map(cat => `
                <button class="cat-btn ${cat === "Tất cả" ? "active" : ""}" onclick="filterCat(this, '${cat}')">
                    ${cat}
                </button>
            `).join('');
        }

        function filterCat(btn, catName) {
            // Đổi màu nút
            document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');

            // Lọc sản phẩm
            renderProducts(catName);
        }

        function renderProducts(filter) {
            const list = document.getElementById('product-list');
            const filtered = filter === "Tất cả" 
                ? allProducts 
                : allProducts.filter(p => p.cat === filter);

            list.innerHTML = filtered.map(p => `
                <div class="card">
                    <div class="img-box">
                        <img src="${p.img}" alt="${p.name}" loading="lazy">
                    </div>
                    <div class="info">
                        <h3 class="title">${p.name}</h3>
                        <div class="old-p">${p.old}</div>
                        <div class="new-p">${p.newP}</div>
                        <a href="${p.link}" target="_blank" class="btn-buy">Mua Ngay</a>
                    </div>
                </div>
            `).join('');
            
            if (filtered.length === 0) {
                list.innerHTML = "<p style='grid-column: 1/-1; text-align: center; padding: 50px;'>Chưa có sản phẩm mục này.</p>";
            }
        }

        init();
    </script>
</body>
</html>
