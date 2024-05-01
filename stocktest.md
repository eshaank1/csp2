---
toc: true
comments: false
layout: post
title: StockSense-Data
type: hacks
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stock Data Fetcher</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #e8f0fe;
        }
        h1 {
            color: #005a87;
        }
        button {
            padding: 10px;
            margin: 10px 0;
            border-radius: 5px;
            border: 1px solid #ccc;
            background-color: #28a745;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #218838;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid black;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        #stockData, #searchHistory {
            background-color: #000000;
            padding: 20px;
            margin-top: 20px;
            border-radius: 5px;
            color: #ffffff;
            box-shadow: 0 0 10px #cccccc;
        }
    </style>
</head>
<body>
    <h1>Stock Data Fetcher</h1>
    <input type="text" id="stockSymbol" placeholder="Enter Stock Symbol (e.g., AAPL)">
    <button onclick="fetchStockData()">Fetch Stock Data</button>
    <button onclick="fetchSearchHistory()">Show My Search History</button>
    <div id="stockData"></div>
    <div id="searchHistory"></div>
    <script>
        async function fetchStockData() {
            const symbol = document.getElementById('stockSymbol').value;
            const response = await fetch(`http://127.0.0.1:8055/api/stockchart/chart/${symbol}`);
            if (response.ok) {
                const data = await response.json();
                displayStockData(data);
            } else {
                document.getElementById('stockData').innerHTML = 'Failed to fetch stock data.';
            }
        }
        async function fetchSearchHistory() {
            const response = await fetch(`http://127.0.0.1:8055/api/search/`);
            if (response.ok) {
                const historyData = await response.json();
                displaySearchHistory(historyData);
            } else {
                document.getElementById('searchHistory').innerHTML = 'Failed to fetch search history.';
            }
        }
        function displayStockData(data) {
            // Extract the time series data which is under the 'Time Series (Daily)' key
            const timeSeries = data['Time Series (Daily)'];
            if (!timeSeries) {
                document.getElementById('stockData').innerHTML = 'No stock data available.';
                return;
            }
            // Assuming you want the most recent day's data, which should be the first key
            const mostRecentDate = Object.keys(timeSeries)[0];
            const stockData = timeSeries[mostRecentDate];
            const content = `
                <h2>Stock Information</h2>
                <p>Open: ${stockData['1. open']}</p>
                <p>High: ${stockData['2. high']}</p>
                <p>Low: ${stockData['3. low']}</p>
                <p>Close: ${stockData['4. close']}</p>
                <p>Volume: ${stockData['5. volume']}</p>
            `;
            document.getElementById('stockData').innerHTML = content;
        }
        function displaySearchHistory(historyData) {
            let content = '<h2>Search History</h2><table>';
            content += '<tr><th>Symbol</th></tr>';
            historyData.forEach(item => {
                content += `<tr><td>${item.symbol}</td></tr>`;
            });
            content += '</table>';
            document.getElementById('searchHistory').innerHTML = content;
        }
    </script>
</body>
</html>










