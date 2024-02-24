---
toc: true
comments: false
layout: post
title: Stock Data Fetcher
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
            background-color: #e8f0fe; /* Light blue background */
        }
        h1 {
            color: #005a87; /* Darker blue for the main heading */
        }
        h2 {
            color: #007bff; /* Bright blue for subheadings */
        }
        input[type="text"], button {
            padding: 10px;
            margin: 10px 0;
            border-radius: 5px;
            border: 1px solid #ccc;
            box-shadow: 0px 0px 5px #aaa; /* Subtle shadow for depth */
        }
        input[type="text"] {
            width: calc(100% - 22px); /* Adjust width to ensure it is responsive */
            box-sizing: border-box; /* Ensures padding does not affect width */
        }
        button {
            background-color: #28a745; /* Green background for the button */
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease; /* Smooth transition for hover effect */
        }
        button:hover {
            background-color: #218838; /* Darker green on hover */
        }
        #stockData {
            background-color: #ffffff;
            padding: 20px;
            margin-top: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px #cccccc; /* More pronounced shadow for the result box */
        }
        #stockData p {
            color: black; /* Set text color to black */
        }
        p {
            color: #6c757d; /* Dark gray for text to improve readability */
        }
    </style>
</head>
<body>
    <h1>Stock Data Fetcher</h1>
    <input type="text" id="stockSymbol" placeholder="Enter Stock Symbol (e.g., AAPL)">
    <button onclick="fetchStockData()">Fetch Stock Data</button>

    <div id="stockData"></div>

    <script>
        async function fetchStockData() {
            const symbol = document.getElementById('stockSymbol').value;
            console.log(`Fetching data for symbol: ${symbol}`);
            
            const response = await fetch(`http://localhost:8055/api/stockchart/chart/${symbol}`);

            if (response.ok) {
                const data = await response.json();
                displayStockData(data);
            } else {
                console.error('Failed to fetch stock data.');
                document.getElementById('stockData').innerHTML = 'Failed to fetch stock data.';
            }
        }

        function displayStockData(data) {
            if (!data['Time Series (Daily)']) {
                document.getElementById('stockData').innerHTML = 'No data available for this symbol.';
                return;
            }

            const timeSeries = data['Time Series (Daily)'];
            const latestDate = Object.keys(timeSeries)[0];
            const latestData = timeSeries[latestDate];

            const content = `
                <p>Date: ${latestDate}</p>
                <p>Open: ${latestData['1. open']}</p>
                <p>High: ${latestData['2. high']}</p>
                <p>Low: ${latestData['3. low']}</p>
                <p>Close: ${latestData['4. close']}</p>
                <p>Volume: ${latestData['5. volume']}</p>
            `;

            document.getElementById('stockData').innerHTML = content;
        }
    </script>
</body>
</html>
