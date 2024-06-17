---
permalink: /stocksort
---

<html>
    <body class="container">
        <h3 id= "Balance"></h3> <a href="/AtlasIndex/transactionlog">Transaction Log</a> <a href="/AtlasIndex/portfolio">Portfolio</a>
        <table id="stockTable">
            <thead>
                <tr>
                    <th>SYM</th>
                    <th>Company Name</th>
                    <th>Qty Available</th>
                    <th>Action (Buy)</th>
                </tr>
            </thead>
            <tbody>
                <!-- Table content will be dynamically populated using JavaScript -->
            </tbody>
        </table>
        <h3><a href="/AtlasIndex/sort">Can't find the stock your looking for? Sort by industry sectors</a></h3>
        <h3><a href="/AtlasIndex/filter">Can't find the stock your looking for? Sort by data founded</a></h3>
        <script type="module">
            import { uri, options1 } from '/AtlasIndex/assets/js/api/config.js';
            let options = options1
            var darkMode = false;
            //window.onload = function () {
                //applyTheme();
                //fetchData();
            //}
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
                // Function to make API call and update the table
                function fetchData(){
                    var json = localStorage.getItem("GICS Sector");
                    var url ='http://127.0.0.1:8086/api/sort/sort'
                    const authOptions = {
                            method: 'POST', // *GET, POST, PUT, DELETE, etc.
                            credentials: 'include', // include, same-origin, omit
                            body: json,
                            headers: {
                                'Content-Type': 'application/json',
                            },
                        };
                        fetch(url, authOptions)
                            .then(response => response.json())
                            .then(data => {
                                console.log(data);
                                window.localStorage.setItem('GICS Sector', json);
                                createTable(data)
                            })
                            .catch(error => console.error('Error fetching data:', error));
                }
                // Function to update the table with data
                function createTable(data) { 
                    var url ='http://127.0.0.1:8086/api/stocks/sortdisplay'
                    var data = data
                    var json = JSON.stringify(data)
                    const authOptions = {
                            method: 'POST', // *GET, POST, PUT, DELETE, etc.
                            credentials: 'include', // include, same-origin, omit
                            body: json,
                            headers: {
                                'Content-Type': 'application/json',
                            },
                        };
                    fetch(url, authOptions)
                        .then(response => response.json())
                        .then(data => {
                            console.log(data);
                            //window.localStorage.setItem('GICS Sector', json);
                            updateTable(data)
                        })
                        .catch(error => console.error('Error fetching data:', error));
                }
                function updateTable(data) {
                    const tableBody = document.querySelector('#stockTable tbody');
                    tableBody.innerHTML = ''; // Clear existing rows
                    data.forEach(stock => {
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${stock.symbol}</td>
                            <td>${stock.company}</td>
                            <td>${stock.quantity}</td>
                            <td><button class="buy-button" onclick="buyStock('${stock.sym}')">Buy</button></td>
                        `;
                        tableBody.appendChild(row);
                        const buyButton = row.querySelector('.buy-button');
                        buyButton.addEventListener('click', function () {
                            var url ='http://127.0.0.1:8086/api/stocks/singleupdate'
                            var sym = stock.symbol
                            console.log(sym)
                            //var json = JSON.stringfy(sym)
                            var data = {
                                symbol: sym
                            }
                            var json = JSON.stringify(data)
                            const authOptions = {
                                    method: 'POST', // *GET, POST, PUT, DELETE, etc.
                                    credentials: 'include', // include, same-origin, omit
                                    body: json,
                                    headers: {
                                        'Content-Type': 'application/json',
                                    },
                                };
                            console.log(authOptions)
                                fetch(url, authOptions)
                                    .then(response => response.json())
                                    .then(data => {
                                        console.log(data);
                                        var price = data
                                        console.log("this is price")
                                        console.log(price)
                                        buyStock(stock.symbol,stock.quantity,price);
                                    })
                                    .catch(error => console.error('Error fetching data:', error));
                    });})
                }
                // Function to handle buy button click
    // Call the function to decode and extract values
    // Call the function to decode the cookie
            function buyStock(symbol, quantity,price) {
                // You can implement your buy logic here
                console.log(`Buying stock with symbol: ${symbol} for $ ${price}`);
                const quantityToBuy = prompt(`How many stocks of ${symbol} do you wish to buy? The current price is $ ${price}`, '1');
                const availableQuantity = quantity;
                if (quantityToBuy !== null && !isNaN(quantityToBuy) && quantityToBuy > 0) {
                    if (quantityToBuy <= availableQuantity) {
                        alert(`Buying ${quantityToBuy} stocks of ${symbol}`);
                        var url = 'http://127.0.0.1:8086/api/stocks/transaction'
                        const newQuantity = availableQuantity - quantityToBuy;
                        const uid = localStorage.getItem("uid");
                        var data = {
                            buyquantity: Number(quantityToBuy),
                            symbol: symbol,
                            uid: uid,
                            newquantity: newQuantity
                        }
                        var json = JSON.stringify(data)
                       //const authOptions = {
                       //    method: 'POST',
                       //    headers: { 'Content-Type': 'application/json' },
                       //    body: json,
                       //    credentials: 'include'
                       //}
                        const authOptions = {
                                    method: 'POST', // *GET, POST, PUT, DELETE, etc.
                                    credentials: 'include', // include, same-origin, omit
                                    body: json,
                                    headers: {
                                        'Content-Type': 'application/json',
                                    },
                                };
                        fetch(url, authOptions)
                            .then(response => {
                                if (!response.ok) {
                                    throw new Error(`HTTP error! Status: ${response.status}`);
                                }
                                return response.json();
                            })
                            .then(data => {
                                console.log( data);
                                fetchData();  // Refresh the data after a successful buy
                                Balance();    // Update balance after a successful buy
                            })
                            .catch(error => {
                                console.error('error', error);
                                alert('your broke')
                            });
                    } else {
                        alert('Invalid quantity. The requested quantity exceeds the available quantity.');
                    }
                } else {
                    alert('Buy operation canceled or invalid quantity entered.');
                }
            }
            function sellStock(symbol, quantity) {
                console.log(`Selling stock with symbol: ${symbol}`);
                const quantityToSell = prompt(`How many stocks of ${symbol} do you wish to sell?`, '1');
                const ownedQuantity = quantity; // Assuming quantity is the available quantity owned by the user
                if (quantityToSell !== null && !isNaN(quantityToSell) && quantityToSell > 0 && quantityToSell <= ownedQuantity) {
                    alert(`Selling ${quantityToSell} stocks of ${symbol}`);
                    var url = 'http://127.0.0.1:8086/api/stocks/sell';
                    const uid = localStorage.getItem("uid");
                    var data = {
                        quantity: Number(quantityToSell),
                        symbol: symbol,
                        uid: uid,
                    };
                    var json = JSON.stringify(data);
                    //const authOptions = {
                    //    method: 'POST',
                    //    headers: { 'Content-Type': 'application/json' },
                    //    body: json,
                    //    credentials: 'include'
                    //};
                    const authOptions = {
                            ...options,
                            body:json,
                        };
                    fetch(url, authOptions)
                        .then(response => {
                            if (!response.ok) {
                                throw new Error(`HTTP error! Status: ${response.status}`);
                            }
                            return response.json();
                        })
                        .then(data => {
                            console.log('success', data);
                            fetchData();  // Refresh the data after a successful sell
                            Balance();    // Update balance after a successful sell
                        })
                        .catch(error => {
                            console.error('error', error);
                            alert("you don't own this stock")
                        });
                } else {
                    alert('Sell operation canceled or invalid quantity entered.');
                }
            }
                function Balance() {
                    var uid = localStorage.getItem("uid");
                    var data = {
                        uid: uid
                    };
                    var json = JSON.stringify(data);
                    //var authOptions1 = {
                    //    method: 'POST',
                    //    headers: {'Content-Type': 'application/json'},
                    //    body: json,
                    //    mode: 'cors',
                    //    cache: 'no-cache',
                    //    credentials: 'include'
                    //};
                    const authOptions = {
                            ...options,
                            body:json,
                        };
                    var url = 'http://127.0.0.1:8086/api/stocks/stockmoney';
                    fetch(url, authOptions)
                        .then(response => response.json())
                        .then(data => {
                            document.getElementById('Balance').textContent = `Available Balance for Purchase': ${data}`;
                        })
                        .catch(error => {
                            console.error('Error fetching stock money:', error.message);
                        });
                }
                // Call applyTheme() to ensure correct initial styling
                //applyTheme();
                // Initial data fetch
                fetchData();
                Balance();
            });
        </script>
    </body>
</html>