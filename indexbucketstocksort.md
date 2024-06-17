---
permalink: /sort
---
<html>
<head>
    <script src=
"https://cdn.jsdelivr.net/npm/chart.js">
    </script>
</head>
<body class = "container">
    <h1>Looking for more stocks? Searching by sectors</h1>
    <div>
        <!-- Create a canvas element to render the chart -->
        <canvas id="pieChart" width="400" height="400">
        </canvas>
    </div>
    <script>
        // Get the 2D rendering context of the canvas
        let ctx = document.getElementById('pieChart')
            .getContext('2d');
        let dataValue = {
            // Labels for each segment of the pie
            labels: ['Communication Services',
                'Consumer Discretionary',
                'Consumer Staples',
                'Energy',
                'Financials',
                'Health Care',
                'Industrials',
                'Information Technology',
                'Materials',
                'Real Estate',
                'Utilities'],
            // Datasets for the chart
            datasets: [{
                data: [10.7, 9.9, 7.2, 3.6, 12.2,14,8.9,24.4,2.5,3.1,3.5],
                // Data points for each segment
            }]
        }
        // Create a new Pie Chart
        let pieChart = new Chart(ctx, {
            // Specify the chart type
            type: 'pie',
            // Provide data for the chart
            data: dataValue,
            // Additional options for the chart
            options: {
                responsive: true, // It make the chart responsive
                // This plugin will display Title of chart
                // Event handler for a click on a chart element
                onClick: function (event, elements) {
                    const clickedElement = elements[0];
                    const datasetIndex = clickedElement.index;
                    const label = dataValue.labels[datasetIndex];
                    const labelValue = dataValue.datasets[0].data[datasetIndex];
                    // Show an alert with information about the clicked segment
                    alert(`Clicked on: ${label} and it accounts of ${labelValue} % of the S&P 500`);
                    console.log(label)
                    var url ='http://127.0.0.1:8086/api/sort/sort'
                    var data = {
                        'GICS Sector': label
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
                    fetch(url, authOptions)
                        .then(response => response.json())
                        .then(data => {
                            console.log(data);
                            window.localStorage.setItem('GICS Sector', json);
                            window.location.href = "/AtlasIndex/stocksort" 
                        })
                        .catch(error => console.error('Error fetching data:', error));
                }
            }
        });
    </script>
    
</body>
 
</html>