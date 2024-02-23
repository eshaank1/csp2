---
toc: true
comments: false
layout: post
title: StockSense
type: hacks
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Stock Manager</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .stock-list { margin-top: 20px; }
        .stock-item { margin-bottom: 10px; }
        input, button { margin: 5px 0; }
    </style>
</head>
<body>
    <h2>Add Stock</h2>
    <form id="addStockForm">
        <input type="text" id="companyName" placeholder="Company Name" required />
        <input type="number" id="shares" placeholder="Shares" required />
        <input type="number" step="0.01" id="purchasePrice" placeholder="Purchase Price" required />
        <input type="hidden" id="userID" value="1" /> <!-- Adjust the value based on your user IDs -->
        <button type="submit">Add Stock</button>
    </form>

    <h2>Stocks</h2>
    <div id="stocksList" class="stock-list"></div>

    <script>
        document.getElementById('addStockForm').onsubmit = async function(e) {
            e.preventDefault();
            const companyName = document.getElementById('companyName').value;
            const shares = document.getElementById('shares').value;
            const purchasePrice = document.getElementById('purchasePrice').value;
            const userID = document.getElementById('userID').value;

            try {
                const response = await fetch('http://127.0.0.1:8055/api/stock/stocks', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        company_name: companyName,
                        shares: parseInt(shares, 10),
                        purchase_price: parseFloat(purchasePrice),
                        userID: userID
                    })
                });

                if (response.ok) {
                    alert('Stock added successfully!');
                    fetchStocks(); // Refresh the stock list
                } else {
                    alert('Failed to add stock. Please check if your backend is running and CORS is configured.');
                }
            } catch (error) {
                console.error('Error adding stock:', error);
                alert('Error communicating with the backend. Make sure your server is running and accessible.');
            }
        };

        async function fetchStocks() {
            try {
                const response = await fetch('http://127.0.0.1:8055/api/stock/stocks');
                const stocks = await response.json();
                const stocksList = document.getElementById('stocksList');
                stocksList.innerHTML = ''; // Clear existing stocks
                stocks.forEach(stock => {
                    const stockItem = document.createElement('div');
                    stockItem.className = 'stock-item';
                    stockItem.innerHTML = `<strong>${stock.company_name}</strong> - Shares: ${stock.shares}, Purchase Price: ${stock.purchase_price}, Current Price: ${stock.current_price || 'N/A'}`;
                    stocksList.appendChild(stockItem);
                });
            } catch (error) {
                console.error('Error fetching stocks:', error);
                alert('Error communicating with the backend. Make sure your server is running and accessible.');
            }
        }

        // Initial fetch of stocks
        fetchStocks();
    </script>
</body>
</html>
