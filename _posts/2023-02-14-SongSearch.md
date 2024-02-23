---
toc: true
comments: false
layout: post
title: Song Search
type: plans
courses: { compsci: {week: 0} }
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Song Search</title>
    <style>
        /* Global styles */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f7f7f7;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        /* Container styles */
        #container {
            background-color: #ffffff;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
            width: 100%;
            max-width: 600px;
        }

        /* Header styles */
        h1 {
            text-align: center;
            color: #009688;
            font-size: 28px;
            margin-bottom: 20px;
        }

        /* Search box styles */
        #search-box {
            text-align: center;
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-size: 18px;
            margin-bottom: 10px;
            color: #333;
        }

        #song-search {
            padding: 10px;
            width: 100%;
            border: 2px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
        }

        #search-button {
            padding: 10px 20px;
            background-color: #ff5722;
            color: #fff;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }

        #search-button:hover {
            background-color: #f44336;
        }

        /* Results styles */
        #results {
            text-align: center;
        }

        h2 {
            color: #333;
            font-size: 24px;
            margin-top: 30px;
        }

        /* Song list styles */
        ul {
            list-style-type: none;
            padding: 0;
        }

        .song-item {
            margin: 20px 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            background-color: #fff;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
            transition: transform 0.2s ease-in-out;
        }

        .song-item:hover {
            transform: scale(1.03);
        }

        img {
            max-width: 150px;
            max-height: 150px;
            border-radius: 5px;
        }

        strong {
            color: #009688;
            font-size: 18px;
        }

        audio {
            width: 100%;
            max-width: 300px;
            margin-top: 20px;
        }

        /* Loading spinner styles */
        .loader {
            border: 6px solid #f3f3f3;
            border-top: 6px solid #009688;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 2s linear infinite;
            margin: 20px auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Song Search</h1>

        <div id="search-box">
            <label for="song-search">Search for a song:</label>
            <input type="text" id="song-search" placeholder="Enter a song title">
            <button id="search-button">Search</button>
        </div>

        <div id="results">
            <h2>Search Results</h2>
            <div id="loader" class="loader" style="display: none;"></div>
            <ul id="song-list"></ul>
        </div>
    </div>

    <script>
        document.getElementById("search-button").addEventListener("click", function () {
            searchForSongs();
        });

        document.getElementById("song-search").addEventListener("keypress", function (event) {
            if (event.key === 'Enter') {
                searchForSongs();
            }
        });

        function searchForSongs() {
            const searchTerm = document.getElementById("song-search").value;
            // Display loading spinner while fetching data
            document.getElementById("loader").style.display = "block";
            document.getElementById("song-list").innerHTML = "";

            // Use the iTunes Search API to fetch song data
            const apiUrl = `https://itunes.apple.com/search?term=${searchTerm}&entity=song&limit=10`;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    document.getElementById("loader").style.display = "none"; // Hide loading spinner
                    displayResults(data.results);
                })
                .catch(error => {
                    console.error("Error fetching data: ", error);
                    document.getElementById("loader").style.display = "none"; // Hide loading spinner on error
                    document.getElementById("song-list").innerHTML = "<p>Error fetching data</p>";
                });
        }

        function displayResults(results) {
            const songList = document.getElementById("song-list");

            if (results.length === 0) {
                songList.innerHTML = "<p>No songs found</p>";
                return;
            }

            results.forEach(song => {
                const listItem = document.createElement("li");
                listItem.className = "song-item";
                const albumImage = document.createElement("img");
                albumImage.src = song.artworkUrl100;
                listItem.appendChild(albumImage);
                listItem.innerHTML += `<strong>${song.trackName}</strong> by ${song.artistName}`;
                listItem.innerHTML += `<audio controls><source src="${song.previewUrl}" type="audio/mpeg"></audio>`;
                songList.appendChild(listItem);
            });
        }
    </script>
</body>
</html>
