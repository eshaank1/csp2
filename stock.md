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
        input, button { margin: 5px 0; display: block; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { text-align: left; padding: 8px; border-bottom: 1px solid #ddd; }
        th { background-color: #f2f2f2; }
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
    <table id="stocksTable">
        <thead>
            <tr>
                <th>Company Name</th>
                <th>Shares</th>
                <th>Purchase Price</th>
                <th>Current Price</th>
                <th>User ID</th>
            </tr>
        </thead>
        <tbody>
            <!-- Stocks will be loaded here -->
        </tbody>
    </table>

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
                const stocksTableBody = document.getElementById('stocksTable').getElementsByTagName('tbody')[0];
                stocksTableBody.innerHTML = ''; // Clear existing stocks
                stocks.forEach(stock => {
                    let row = stocksTableBody.insertRow();
                    row.insertCell(0).innerText = stock.company_name;
                    row.insertCell(1).innerText = stock.shares;
                    row.insertCell(2).innerText = stock.purchase_price.toFixed(2);
                    row.insertCell(3).innerText = stock.current_price ? stock.current_price.toFixed(2) : 'N/A';
                    row.insertCell(4).innerText = stock.userID;
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
