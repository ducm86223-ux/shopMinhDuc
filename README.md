<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minh Đức Shop - Deal Shopee Mỗi Ngày</title>
    <meta name="description" content="Tổng hợp deal hot Shopee chính hãng từ Minh Đức Shop. Mua đúng giá, đúng chỗ.">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        :root { --shopee-orange: #EE4D2D; --bg-light: #F5F5F5; --text-dark: #333; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background-color: var(--bg-light); color: var(--text-dark); line-height: 1.6; }
        a { text-decoration: none; color: inherit; }

        /* Top Bar */
        .top-bar { background: #222; color: white; padding: 8px 20px; font-size: 13px; display: flex; justify-content: center; gap: 20px; flex-wrap: wrap; }
        .top-bar a:hover { color: var(--shopee-orange); }

        /* Header */
        header { background: white; padding: 20px; text-align: center; border-bottom: 1px solid #ddd; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        .logo { font-size: 28px; font-weight: 800; color: var(--shopee-orange); text-transform: uppercase; }
        .verified-badge { display: inline-block; background: #e6fcf5; color: #0ca678; padding: 4px 12px; border-radius: 20px; font-size: 11px; font-weight: 700; margin-top: 5px; }

        /* Hero */
        .hero { background: white; padding: 40px 20px; text-align: center; border-bottom: 1px solid #eee; }
        .hero h1 { font-size: 26px; margin-bottom: 8px; }
        .hero p { color: #666; font-size: 15px; }

        /* Grid */
        .container { max-width: 1200px; margin: 30px auto; padding: 0 15px; min-height: 50vh; }
        .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 20px; }
        
        /* Card */
        .card { background: white; border-radius: 12px; overflow: hidden; display: flex; flex-direction: column; transition: 0.3s ease; border: 1px solid #eee; position: relative; }
        .card:hover { transform: translateY(-8px); box-shadow: 0 12px 25px rgba(0,0,0,0.1); }
        
        .cat-tag { position: absolute; top: 12px; left: 12px; background: rgba(0,0,0,0.8); color: white; padding: 4px 10px; font-size: 10px; font-weight: 700; border-radius: 4px; z-index: 2; text-transform: uppercase; }
        .img-box { width: 100%; aspect-ratio: 1/1; background: #f9f9f9; overflow: hidden; }
        .img-box img { width: 100%; height: 100%; object-fit: cover; }
        
        .info { padding: 15px; flex-grow: 1; display: flex; flex-direction: column; }
        .title { font-size: 15px; font-weight: 600; color: #222; margin-bottom: 12px; height: 42px; overflow: hidden; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; line-height: 1.4; }
        .price-row { margin-bottom: 15px; }
        .old-p { text-decoration: line-through; color: #999; font-size: 13px; display: block; }
        .new-p { color: var(--shopee-orange); font-size: 22px; font-weight: 800; }
        
        .btn-buy { background: var(--shopee-orange); color: white; text-align: center; padding: 12px; border-radius: 8px; font-weight: 700; transition: 0.2s; margin-top: auto; }

        /* Footer */
        footer { background: #111; color: #888; padding: 50px 20px; text-align: center; margin-top: 60px; font-size: 14px; }
        #status { text-align: center; padding: 50px; color: #777; }

        @media (max-width: 600px) {
            .product-grid { grid-template-columns: repeat(2, 1fr); gap: 10px; }
            .info { padding: 10px; }
            .title { font-size: 13px; height: 36px; }
            .new-p { font-size: 18px; }
        }
    </style>
</head>
<body>

    <div class="top-bar">
        <span>📞 0822611300</span>
        <a href="mailto:minhducduong2406@gmail.com">✉️ minhducduong2406@gmail.com</a>
        <a href="https://www.facebook.com/minh.uc.43365" target="_blank">🔵 Facebook</a>
    </div>

    <header>
        <a href="/" class="logo">MINH ĐỨC SHOP</a><br>
        <div class="verified-badge">✓ DỮ LIỆU CẬP NHẬT TỰ ĐỘNG</div>
    </header>

    <div class="container">
        <div id="status">🔄 Đang tải deal từ Minh Đức Shop...</div>
        <div class="product-grid" id="main-grid"></div>
    </div>

    <footer>
        <p>© 2026 MINH ĐỨC SHOP. All rights reserved.</p>
        <p style="font-size: 12px; margin-top: 10px; opacity: 0.6;">Dữ liệu đồng bộ trực tiếp từ Google Sheets</p>
    </footer>

    <script>
        const SHEET_ID = '1vTSfjCVCxt7Cp5GqhN4wC5wg07VQNRuGW4d2zUtyMI4Hb03Vm7okSScD-2Ns37xLTEeE1t8wFfVybIB';
        const DATA_SOURCE = `https://docs.google.com/spreadsheets/d/e/2PACX-${SHEET_ID}/pub?gid=0&single=true&output=csv`;

        async function initWeb() {
            try {
                const response = await fetch(DATA_SOURCE);
                const csvData = await response.text();
                const lines = csvData.split('\n').slice(1);
                const grid = document.getElementById('main-grid');
                const status = document.getElementById('status');
                
                let cardsHtml = '';
                let validItems = 0;

                lines.forEach(line => {
                    const cols = line.split(/,(?=(?:(?:[^"]*"){2})*[^"]*$)/);
                    
                    if (cols.length >= 6) {
                        const category = cols[0]?.replace(/^"|"$/g, '').trim(); // A
                        const name     = cols[1]?.replace(/^"|"$/g, '').trim(); // B
                        const link     = cols[2]?.replace(/^"|"$/g, '').trim(); // C
                        const img      = cols[3]?.replace(/^"|"$/g, '').trim(); // D
                        const oldP     = cols[4]?.replace(/^"|"$/g, '').trim(); // E
                        const newP     = cols[5]?.replace(/^"|"$/g, '').trim(); // F

                        if (name && img && img.includes('http')) {
                            validItems++;
                            cardsHtml += `
                                <div class="card">
                                    <div class="cat-tag">${category}</div>
                                    <div class="img-box">
                                        <img src="${img}" alt="${name}" loading="lazy" onerror="this.src='https://via.placeholder.com/400?text=Minh+Duc+Shop'">
                                    </div>
                                    <div class="info">
                                        <h3 class="title">${name}</h3>
                                        <div class="price-row">
                                            <span class="old-p">${oldP}</span>
                                            <span class="new-p">${newP}</span>
                                        </div>
                                        <a href="${link}" target="_blank" rel="nofollow" class="btn-buy">SĂN DEAL NGAY</a>
                                    </div>
                                </div>
                            `;
                        }
                    }
                });

                if (validItems > 0) {
                    status.style.display = 'none';
                    grid.innerHTML = cardsHtml;
                } else {
                    status.innerHTML = "⚠️ Chưa có sản phẩm nào được cập nhật.";
                }
            } catch (e) {
                document.getElementById('status').innerHTML = "❌ Lỗi tải dữ liệu. Thử F5 lại.";
            }
        }
        initWeb();
    </script>
</body>
</html>
