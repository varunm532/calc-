---
permalink: /transactionlog
---
<html>
    <a href="/AtlasIndex/stocks">Back</a>
    <a href="/AtlasIndex/portfolio">Portfolio</a>
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
        <table id="stockTable" >
        <thead>
            <tr>
                <th>User</th>
                <th>Symbol</th>
                <th>TransactionType</th>
                <th>Quantity</th>
                <th>TransactionAmount</th>
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
            var url = 'http://localhost:8086/api/stocks/transaction/display'
            const uid = localStorage.getItem("uid");
            var data = {
                uid: uid
            }
            var json = JSON.stringify(data)
            const authOptions = {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: json,
                credentials: 'include'
            }
            fetch(url, authOptions)
                .then(response => response.json())
                .then(data => {
                    updateTable(data);
                })
                .catch(error => console.error('Error fetching data:', error));
        }
        // Function to update the table with data
        function updateTable(data) {
            const tableBody = document.querySelector('#stockTable tbody');
            tableBody.innerHTML = ''; // Clear existing rows
            data.forEach(transaction => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${transaction.uid}</td>
                    <td>${transaction.symbol}</td>
                    <td>${transaction.transaction_type}</td>
                    <td>${transaction.quantity}</td>
                    <td>${transaction.transaction_amount}</td>
                `;
                tableBody.appendChild(row);
            });
        }
        // Call fetchData when the page loads
        fetchData();
    });
    </script>