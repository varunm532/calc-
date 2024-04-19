---
permalink: /portfolio
---
<html>
<a href="/AtlasIndex/stocks">Back</a>
<a href="/AtlasIndex/transactionlog">Transaction Log</a>
<head>
    <script src="https://cdn.anychart.com/releases/8.11.0/js/anychart-base.min.js"></script>
    <style type="text/css">      
     html, body, #container { 
        width: 100%; height: 200%; margin: 0; padding: 0; 
      } 
    </style>
    <link id="theme-style" rel="stylesheet" type="text/css" href="assets/css/style.css">
</head>
<body class="lightmode">
    <h1>User Money Over Transactions Graph</h1>
    <div id="container"></div>
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
        anychart.onDocumentReady(function() {
    // The main JS line charting code will be here.\
            var url = 'http://localhost:8086/api/stocks/owned';
            var uid = localStorage.getItem('uid')
            var payload = {
                uid: uid
            }
            var json = JSON.stringify(payload)
            var authOptions = {
                credentials: 'include',
                body: json,
                method: 'Post',
                headers: { 'Content-Type': 'application/json' }        
            }
            fetch(url,authOptions)
                .then(response => response.json())
                .then( data =>{
                    console.log(data)
                    var dataSet = anychart.data.set(data);
                    // map the data for all series
                    var firstSeriesData = dataSet.mapAs({x: 0, value: 1});
                    // create a line chart
                    var chart = anychart.line();
                    // create the series and name them
                    var firstSeries = chart.line(firstSeriesData);            // add a legend
                    chart.legend().enabled(true);
                    // add a title
                    chart.title("Account Balance");
                    // specify where to display the chart
                    chart.container("container");
                    // draw the resulting chart
                    chart.draw();
                    graph()
                })
    });
    </script>
</body>
</html>
