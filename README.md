<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VELSTORE - Toko Item Game Terpercaya</title>
    <style>
        :root {
            --bg-color: #f8f9fa;
            --card-bg: #ffffff;
            --text-color: #333333;
            --text-muted: #666666;
            --primary-color: #007bff;
            --primary-hover: #0056b3;
            --border-color: #e0e0e0;
            --accent-yellow: #ffc107;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 0;
        }

        header {
            background-color: #ffffff;
            padding: 15px 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-weight: bold;
            font-size: 20px;
            color: var(--primary-color);
        }

        .admin-btn {
            background-color: #6c757d;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
        }

        .admin-btn:hover {
            background-color: #5a6268;
        }

        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .category-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 15px;
            border-left: 4px solid var(--primary-color);
            padding-left: 10px;
        }

        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .product-card {
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.02);
        }

        .product-title {
            font-weight: bold;
            font-size: 16px;
            text-align: center;
            margin-bottom: 10px;
        }

        .product-note {
            font-size: 12px;
            color: var(--text-muted);
            background-color: #f1f3f5;
            padding: 8px;
            border-radius: 4px;
            margin-bottom: 15px;
            line-height: 1.4;
        }

        .price-box {
            display: flex;
            justify-content: space-between;
            background: #f8f9fa;
            padding: 10px;
            border-radius: 6px;
            margin-bottom: 15px;
            text-align: center;
        }

        .price-item {
            flex: 1;
        }

        .price-item span {
            display: block;
            font-size: 11px;
            color: var(--text-muted);
        }

        .price-item strong {
            font-size: 14px;
            color: #d9534f;
        }

        .btn-group {
            display: flex;
            gap: 10px;
        }

        .btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 4px;
            font-weight: bold;
            cursor: pointer;
            text-align: center;
        }

        .btn-sell {
            background-color: #ffc107;
            color: #000;
        }

        .btn-buy {
            background-color: var(--primary-color);
            color: #fff;
        }

        .btn:hover {
            opacity: 0.9;
        }

        /* Modal Admin */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-color: rgba(0,0,0,0.5);
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            width: 90%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
        }

        .close {
            float: right;
            font-size: 20px;
            cursor: pointer;
        }

        input, textarea {
            width: 100%;
            padding: 8px;
            margin: 6px 0 12px 0;
            border: 1px solid var(--border-color);
            border-radius: 4px;
            box-sizing: border-box;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }

        table, th, td {
            border: 1px solid var(--border-color);
            font-size: 13px;
            padding: 6px;
        }
    </style>
</head>
<body>

    <header>
        <div class="logo">VELSTORE</div>
        <button class="admin-btn" onclick="openAdminLogin()">Panel Admin</button>
    </header>

    <div class="container">
        <div class="category-title">KATEGORI TERPILIH: CPS</div>

        <div class="product-grid" id="product-list">
            <!-- Produk akan dimuat melalui Javascript agar bisa disetting harganya -->
        </div>
    </div>

    <!-- Modal Admin / Login -->
    <div id="adminModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <h3>Panel Admin VELSTORE</h3>
            
            <div id="login-section">
                <p>Masukkan Password Admin:</p>
                <input type="password" id="adminPassword" placeholder="Password...">
                <button class="btn btn-buy" onclick="checkAdmin()">Masuk</button>
            </div>

            <div id="dashboard-section" style="display:none;">
                <h4>Pengaturan Harga Produk</h4>
                <label>Harga Jual Blue Gem Lock (Rp):</label>
                <input type="number" id="edit-blue-sell" value="1000">
                <label>Harga Beli Blue Gem Lock (Rp):</label>
                <input type="number" id="edit-blue-buy" value="1100">
                
                <label>Harga Jual Black Gem Lock (Rp):</label>
                <input type="number" id="edit-black-sell" value="105000">
                <label>Harga Beli Black Gem Lock (Rp):</label>
                <input type="number" id="edit-black-buy" value="115000">

                <button class="btn btn-buy" onclick="savePrices()">Simpan Perubahan Harga</button>

                <h4 style="margin-top: 20px;">Daftar Pembeli (Data Transaksi)</h4>
                <table>
                    <thead>
                        <tr>
                            <th>Waktu</th>
                            <th>Produk</th>
                            <th>Tipe</th>
                            <th>Kontak/ID</th>
                        </tr>
                    </thead>
                    <tbody id="buyer-table-body">
                        <!-- Data pembeli masuk sini -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <script>
        // Data Produk Default (Menyerupai Gambar)
        let products = {
            blue: { name: "CPS Blue Gem Lock", sell: 1000, buy: 1100, note: "NOTE: JIKA JUAL UTAMAKAN PUNYA NO REK. BCA, DANA, OVO, SHOPEEPAY, SELAIN ITU KENA BIAYA ADMIN" },
            black: { name: "CPS Black Gem Lock", sell: 105000, buy: 115000, note: "NOTE: JIKA JUAL UTAMAKAN PUNYA NO REK. BCA, DANA, OVO, SHOPEEPAY, SELAIN ITU KENA BIAYA ADMIN" }
        };

        // Data Pembeli (Disimpan di localStorage sementara)
        let buyers = JSON.parse(localStorage.getItem('velstore_buyers')) || [];

        function renderProducts() {
            const container = document.getElementById('product-list');
            container.innerHTML = `
                <div class="product-card">
                    <div class="product-title">${products.blue.name}</div>
                    <div class="product-note">${products.blue.note}</div>
                    <div class="price-box">
                        <div class="price-item">
                            <span>HARGA JUAL</span>
                            <strong>Rp ${products.blue.sell.toLocaleString()}</strong>
                        </div>
                        <div class="price-item">
                            <span>HARGA BELI</span>
                            <strong>Rp ${products.blue.buy.toLocaleString()}</strong>
                        </div>
                    </div>
                    <div class="btn-group">
                        <button class="btn btn-sell" onclick="transaction('Blue Gem Lock', 'Jual')">JUAL</button>
                        <button class="btn btn-buy" onclick="transaction('Blue Gem Lock', 'Beli')">BELI</button>
                    </div>
                </div>

                <div class="product-card">
                    <div class="product-title">${products.black.name}</div>
                    <div class="product-note">${products.black.note}</div>
                    <div class="price-box">
                        <div class="price-item">
                            <span>HARGA JUAL</span>
                            <strong>Rp ${products.black.sell.toLocaleString()}</strong>
                        </div>
                        <div class="price-item">
                            <span>HARGA BELI</span>
                            <strong>Rp ${products.black.buy.toLocaleString()}</strong>
                        </div>
                    </div>
                    <div class="btn-group">
                        <button class="btn btn-sell" onclick="transaction('Black Gem Lock', 'Jual')">JUAL</button>
                        <button class="btn btn-buy" onclick="transaction('Black Gem Lock', 'Beli')">BELI</button>
                    </div>
                </div>
            `;
        }

        function transaction(productName, type) {
            let contact = prompt(`Masukkan Data Akun / No HP / ID Anda untuk ${type} ${productName}:`);
            if (contact) {
                let newTransaction = {
                    time: new Date().toLocaleString(),
                    product: productName,
                    type: type,
                    contact: contact
                };
                buyers.push(newTransaction);
                localStorage.setItem('velstore_buyers', JSON.stringify(buyers));
                alert("Transaksi berhasil dicatat! Admin akan segera menghubungi Anda.");
            }
        }

        function openAdminLogin() {
            document.getElementById('adminModal').style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('adminModal').style.display = 'none';
        }

        function checkAdmin() {
            let pass = document.getElementById('adminPassword').value;
            // Ganti password admin di sini sesuai keinginan Anda (contoh: "velstore123")
            if (pass === "velstore123") {
                document.getElementById('login-section').style.display = 'none';
                document.getElementById('dashboard-section').style.display = 'block';
                
                // Isi form edit dengan data saat ini
                document.getElementById('edit-blue-sell').value = products.blue.sell;
                document.getElementById('edit-blue-buy').value = products.blue.buy;
                document.getElementById('edit-black-sell').value = products.black.sell;
                document.getElementById('edit-black-buy').value = products.black.buy;

                loadBuyersTable();
            } else {
                alert("Password Salah!");
            }
        }

        function savePrices() {
            products.blue.sell = parseInt(document.getElementById('edit-blue-sell').value);
            products.blue.buy = parseInt(document.getElementById('edit-blue-buy').value);
            products.black.sell = parseInt(document.getElementById('edit-black-sell').value);
            products.black.buy = parseInt(document.getElementById('edit-black-buy').value);
            
            renderProducts();
            alert("Harga berhasil diubah!");
            closeModal();
        }

        function loadBuyersTable() {
            const tbody = document.getElementById('buyer-table-body');
            tbody.innerHTML = "";
            if (buyers.length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" style="text-align:center;">Belum ada data transaksi.</td></tr>`;
                return;
            }
            buyers.forEach(b => {
                tbody.innerHTML += `
                    <tr>
                        <td>${b.time}</td>
                        <td>${b.product}</td>
                        <td>${b.type}</td>
                        <td>${b.contact}</td>
                    </tr>
                `;
            });
        }

        // Jalankan saat pertama kali muat
        renderProducts();
    </script>
</body>
</html>
