---
toc: true
comments: false
layout: post
title: Song Search test
type: plans
courses: { compsci: {week: 0} }
---




<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Song Search</title>
    <style>
        /* Global styles remain the same */

        /* Container styles remain the same */

        /* Header styles remain the same */

        /* Search box styles remain the same */

        /* Results styles remain the same */

        /* Song list styles remain the same */

        /* Loading spinner styles remain the same */
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

        <div id="sorting-filtering">
            <label for="sort-select">Sort by:</label>
            <select id="sort-select">
                <option value="trackName">Title</option>
                <option value="artistName">Artist</option>
                <option value="releaseDate">Release Date</option>
            </select>

            <label for="filter-select">Filter by Genre:</label>
            <select id="filter-select">
                <option value="">All Genres</option>
                <option value="Rock">Rock</option>
                <option value="Pop">Pop</option>
                <option value="Hip-Hop">Hip-Hop</option>
                <option value="Electronic">Electronic</option>
                <!-- Add more genres as needed -->
            </select>
        </div>

        <div id="results">
            <h2>Search Results</h2>
            <div id="loader" class="loader" style="display: none;"></div>
            <ul id="song-list"></ul>
        </div>
    </div>

    <script>
        document.getElementById("search-button").addEventListener("click", function () {
            const searchTerm = document.getElementById("song-search").value;
            const sortBy = document.getElementById("sort-select").value;
            const filterBy = document.getElementById("filter-select").value;
            searchForSongs(searchTerm, sortBy, filterBy);
        });

        function searchForSongs(searchTerm, sortBy, filterBy) {
            document.getElementById("loader").style.display = "block";
            document.getElementById("song-list").innerHTML = "";

            let apiUrl = `https://itunes.apple.com/search?term=${searchTerm}&entity=song&limit=10&sort=${sortBy}`;

            if (filterBy) {
                apiUrl += `&genre=${filterBy}`;
            }

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    document.getElementById("loader").style.display = "none";
                    displayResults(data.results);
                })
                .catch(error => {
                    console.error("Error fetching data: ", error);
                    document.getElementById("loader").style.display = "none";
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
