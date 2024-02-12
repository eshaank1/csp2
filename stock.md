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
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StockSense</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            text-align: center;
            padding: 20px;
            border: 2px solid #ddd;
            border-radius: 5px;
            width: 300px;
        }
        input[type="text"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            box-sizing: border-box;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .button-container {
            display: flex;
            justify-content: space-between;
        }
        .button-container button {
            background-color: #4CAF50;
            color: white;
            padding: 5px 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            flex: 1;
        }
        button:hover {
            background-color: #45a049;
        }
        #result {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .button-spacing {
            margin-right: 10px;
        }
    </style>

<body>
    <div class="container">
    <label class="switch">
    <input type="checkbox" id="tempSwitch" onclick="getMetric()">
    <span class="slider round"></span>
</label>


     

        <input type="text" id="location" placeholder="Enter Stock Name" autofocus onkeyup="handleKeyPress(event)">
        <div class="button-container">
            <button class="button-spacing" onclick="getWindSpeed()">Stock Price</button>
            <button class="button-spacing" onclick="getTemperature()">Trend Chart</button>
            <button class="button-spacing" onclick="getPrecipitation()">Time of Day</button>
        </div>
        <div id="result"></div>
    </div>


    <script>
        let isMetric = false;

        function handleKeyPress(event) {
            if (event.key === 'Enter') {
            }
        }

        function getWindSpeed() {
            fetchWeatherData('wind_mph');
        }



        function getTemperature() {
            fetchWeatherData('feelslike_f');
        }

        function getPrecipitation() {
            fetchWeatherData('precip_in');
        }

        function capitalizeFirstLetter(string) {
            return string.charAt(0).toUpperCase() + string.slice(1);
        }

        function getMetric() {
            isMetric = !isMetric;
        }

        function fetchWeatherData(dataType) {
            const locationInput = document.getElementById('location');
            const resultDiv = document.getElementById('result');
            const location = capitalizeFirstLetter(locationInput.value.trim());
        

            if (location === '') {
                resultDiv.innerText = 'Please enter a location';
                return;
            }

            resultDiv.innerText = 'Loading...';

            fetch('http://localhost:8531/api/weather/' + location)
                .then(response => response.json())
                .then(data => {
                    let imageFetchUrl = 'http://localhost:8531/api/cityimage/' + location;
                    fetch(imageFetchUrl)
                        .then(response => response.json())
                        .then(imageData => {
                            let imageUrl = imageData.image_url; // Assuming the URL is stored in a field named 'url'
                            console.log("IMAGE_URL===="+ imageUrl)
                            var image = document.getElementById('weatherIcon')
                            image.src = data["current"]["weatherIcon_url"]

                         
                            
                            let imgElement = document.createElement('img');
                            imgElement.src = imageUrl;
                            resultDiv.appendChild(imgElement);
                            })
                        .catch(error => {
                            console.error('Error fetching image:', error);
                            resultDiv.innerText += 'An error occurred fetching the image. Please try again later.';
                        });
                })
                .catch(error => {
                    console.error('Error fetching data:', error);
                    resultDiv.innerText = 'An error occurred. Please try again later.';
                });
        }
                
               

    </script>           
</body>



<html>


