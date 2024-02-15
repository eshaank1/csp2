<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Song Search</title>
    <style>
        body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0; 
    margin: 0;
    padding: 0;
}

#container {
    max-width: 800px;
    margin: 20px auto;
    padding: 20px;
    background: linear-gradient(to bottom, #4e73df, #224abe); 
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); 
}

h1 {
    text-align: center;
    color: #fff;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    margin-bottom: 20px;
}

#search-box {
    margin-bottom: 20px;
    text-align: center;
}

label {
    font-weight: bold;
    margin-right: 10px;
    color: #fff;
}

input[type="text"],
select {
    padding: 10px;
    border-radius: 4px;
    border: none;
    background-color: rgba(255, 255, 255, 0.9);
    margin-bottom: 10px; 
    width: calc(100% - 20px); 
}

button {
    padding: 10px 20px; 
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s; 
}

button:hover {
    background-color: #0056b3;
}

#results {
    text-align: center;
    margin-top: 20px; 
}

.loader {
    border: 5px solid #f3f3f3;
    border-top: 5px solid #007bff;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 1s linear infinite;
    margin: 20px auto;
}

#song-list {
    list-style: none;
    padding: 0;
}

.song-item {
    margin-bottom: 20px;
    padding: 20px;
    background-color: #fff; 
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); 
}

.song-item img {
    width: 100px;
    height: 100px;
    border-radius: 8px;
    margin-right: 20px; 
    object-fit: cover; 
}

.song-item strong {
    color: #007bff;
}

audio {
    width: 100%;
    margin-top: 20px; 
}

@media (max-width: 600px) {
    #container {
        padding: 10px;
    }

    .song-item img {
        width: 80px;
        height: 80px;
    }
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
            <button id="shuffle-button">Shuffle</button> <!-- New shuffle button -->
            <div id="loader" class="loader" style="display: none;"></div>
            <ul id="song-list"></ul>
        </div>
    </div>

    <script>
        document.getElementById("search-button").addEventListener("click", function () {
            performSearch();
        });

        document.getElementById("filter-select").addEventListener("change", function () {
            performSearch();
        });

        function performSearch() {
            const searchTerm = document.getElementById("song-search").value;
            const sortBy = document.getElementById("sort-select").value;
            const filterBy = document.getElementById("filter-select").value;
            searchForSongs(searchTerm, sortBy, filterBy);
        }

        function searchForSongs(searchTerm, sortBy, filterBy) {
            document.getElementById("loader").style.display = "block";
            document.getElementById("song-list").innerHTML = "";

            let apiUrl = `https://itunes.apple.com/search?term=${searchTerm}&entity=song&limit=10&sort=${sortBy}`;

            if (filterBy) {
                apiUrl += `&genre=${encodeURIComponent(filterBy)}`;
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
                const shouldHide = Math.random() < 0.5; // Adjust the probability as needed
                if (!shouldHide) {
                    const listItem = document.createElement("li");
                    listItem.className = "song-item";
                    const albumImage = document.createElement("img");
                    albumImage.src = song.artworkUrl100;
                    listItem.appendChild(albumImage);
                    listItem.innerHTML += `<strong>${song.trackName}</strong> by ${song.artistName}`;
                    listItem.innerHTML += `<audio controls><source src="${song.previewUrl}" type="audio/mpeg"></audio>`;
                    songList.appendChild(listItem);
                }
            });

            document.getElementById("shuffle-button").addEventListener("click", function () {
                shuffleResults(results);
            });
        }

        function shuffleResults(results) {
            const songList = document.getElementById("song-list");
            const items = Array.from(songList.children);
            items.forEach(item => {
                songList.removeChild(item);
            });
            const shuffledResults = shuffleArray(results);
            displayResults(shuffledResults);
        }

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
    </script>
</body>
</html>
