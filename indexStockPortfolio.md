---
permalink: /portfolio
---
<html>
<a href="/AtlasIndex/stocks">Back</a>
<a href="/AtlasIndex/transactionlog">Transaction Log</a>
<head>
    <style>
        .darkmode {
            background: #252525;
            color: #ffffff;
        }

        .lightmode {
            background: #ffffff;
            color: #000000;
        }
    </style>
    <link id="theme-style" rel="stylesheet" type="text/css" href="assets/css/style.css">
</head>
<body class="lightmode">
    <h1>User Money Over Transactions Graph</h1>
    <div id="result">
        <canvas id="stockChart"></canvas>
    </div>
    <table id="stockTable">
        <thead>
            <tr>
                <th>Symbol</th>
                <th>Total Quantity</th>
                <th>Value</th>
            </tr>
        </thead>
        <tbody>
            <!-- Table content will be dynamically populated using JavaScript -->
        </tbody>
    </table>
    <script>
        var darkMode = false;
        window.onload = function () {
            var themeStyle = document.getElementById('theme-style');
            var body = document.body;
            var storedTheme = localStorage.getItem('theme');
            if (storedTheme === 'dark') {
                themeStyle.href = "assets/css/dark.css";
                body.classList.remove('lightmode');
                body.classList.add('darkmode');
            } else {
                themeStyle.href = "assets/css/style.css";
                body.classList.remove('darkmode');
                body.classList.add('lightmode');
            }
        }
        document.addEventListener("DOMContentLoaded", function () {
            function fetchData() {
                var url = 'http://localhost:8086/api/stocks/portfolio';
                const uid = localStorage.getItem("uid");
                var data = {
                    uid: uid
                };
                var json = JSON.stringify(data);
                const authOptions = {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: json,
                    credentials: 'include'
                };
                fetch(url, authOptions)
                    .then(response => response.json())
                    .then(data => {
                        updateTable(data.portfolio); // Corrected here
                    })
                    .catch(error => console.error('Error fetching data:', error));
            }
            // Function to update the table with data
            function updateTable(data) {
                const tableBody = document.querySelector('#stockTable tbody');
                tableBody.innerHTML = ''; // Clear existing rows
                data.forEach(portfolio_data => {
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${portfolio_data.SYMBOL}</td>
                        <td>${portfolio_data.TOTAL_QNTY}</td>
                        <td>${portfolio_data.VALUE}</td>
                    `;
                    tableBody.appendChild(row);
                });
            }
            // Call fetchData when the page loads
            fetchData();
        });
    </script>
    <script>
        function graph(){
            const uid = localStorage.getItem("uid");
            fetch('http://localhost:8086/api/stocks/graph', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ uid: uid })
            })
            .then(response => response.json())
            .then(data => {
                var img = document.createElement('img');
                img.src = 'data:image/png;base64,' + data.image;
                document.getElementById('result').appendChild(img);
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
        graph()
    </script>
</body>
</html>
