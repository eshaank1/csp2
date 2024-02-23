---
toc: true
comments: false
layout: post
title: StockSense
type: hacks
---


<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Stock Manager</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 100%; border-collapse: collapse; }
        th, td { text-align: left; padding: 8px; border-bottom: 1px solid #ddd; }
        th { background-color: #f2f2f2; }
        input, button { margin: 5px 0; display: block; }
        .profit { color: green; }
        .loss { color: red; }
    </style>
</head>
<body>
    <h2>Finacial Log</h2>
    <form id="addStockForm">
        <input type="text" id="companyName" placeholder="Company Name" required />
        <input type="number" id="shares" placeholder="Shares" required />
        <input type="number" step="0.01" id="purchasePrice" placeholder="Purchase Price" required />
        <input type="number" step="0.01" id="currentPrice" placeholder="Current Price" required />
        <button type="submit">Log Purchase</button>
    </form>

    <h2>Stocks</h2>
    <table id="stocksTable">
        <thead>
            <tr>
                <th>Company Name</th>
                <th>Shares</th>
                <th>Purchase Price</th>
                <th>Current Price</th>
                <th>Profit/Loss</th>
            </tr>
        </thead>
        <tbody>
            <!-- Stocks will be added here -->
        </tbody>
    </table>

    <script>
        document.getElementById('addStockForm').onsubmit = async function(e) {
            e.preventDefault();
            const companyName = document.getElementById('companyName').value;
            const shares = document.getElementById('shares').value;
            const purchasePrice = document.getElementById('purchasePrice').value;
            const currentPrice = document.getElementById('currentPrice').value;

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
                        current_price: parseFloat(currentPrice)
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
                const stocksTableBody = document.getElementById('stocksTable').getElementsByTagName('tbody')[0];
                stocksTableBody.innerHTML = ''; // Clear existing stocks
                stocks.forEach(stock => {
                    const row = stocksTableBody.insertRow();
                    const profitLoss = (stock.current_price - stock.purchase_price) * stock.shares;
                    const profitLossClass = profitLoss >= 0 ? 'profit' : 'loss';

                    row.insertCell(0).textContent = stock.company_name;
                    row.insertCell(1).textContent = stock.shares;
                    row.insertCell(2).textContent = stock.purchase_price;
                    row.insertCell(3).textContent = stock.current_price || 'N/A';
                    const profitLossCell = row.insertCell(4);
                    profitLossCell.textContent = profitLoss.toFixed(2);
                    profitLossCell.classList.add(profitLossClass);
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
