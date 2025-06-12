# besparing-calculator

Deze repository bevat een eenvoudige interactieve rekenmachine waarmee de
besparing voor een franchise kan worden berekend. Open `besparing_calculator.html`
in een webbrowser en vul het aantal vestigingen, het aantal orders per maand,
de besparing per order en het groeipercentage in. De applicatie toont de
verwachte besparing in jaar 1 en projecteert de groei over vijf jaar met een
lijn grafiek.

<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bereken uw Besparing</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
        }
        label {
            display: inline-block;
            margin-top: 10px;
        }
        input[type="number"], input[type="text"] {
            padding: 5px;
            margin-top: 5px;
            width: 100%;
        }
        .input-container {
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: space-between;
            margin-bottom: 20px;
        }
        .input-container label, .input-container input {
            width: 23%;
        }
        button {
            padding: 10px 15px;
            background-color: #3aacf4;
            color: white;
            border: none;
            margin-top: 20px;
            cursor: pointer;
        }
        button:hover {
            background-color: #183055;
        }
        .result {
            margin-top: 20px;
            font-weight: bold;
            font-size: 18px;
        }
        #chart-container {
            margin-top: 30px;
        }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>

    <h2>Bereken uw minimale besparing per jaar</h2>

    <div class="input-container">
        <div>
            <label for="locations">Aantal vestigingen:</label>
            <input type="number" id="locations" placeholder="Bijvoorbeeld 10" required>
        </div>
        <div>
            <label for="ordersPerMonth">Orders per maand per vestiging:</label>
            <input type="number" id="ordersPerMonth" placeholder="Bijvoorbeeld 600" required>
        </div>
        <div>
            <label for="savingsPerOrder">Besparing per order (€):</label>
            <input type="number" id="savingsPerOrder" placeholder="Bijvoorbeeld 1" step="0.01" required>
        </div>
        <div>
            <label for="growthRate">Groei per jaar (%):</label>
            <input type="number" id="growthRate" value="5" step="0.1" required>
        </div>
    </div>

    <button onclick="calculateSavings()">Bereken Besparing</button>

    <div class="result" id="result"></div>

    <div id="chart-container">
        <canvas id="savingsChart"></canvas>
    </div>

    <script>
        var savingsChart;

        function calculateSavings() {
            // Verkrijg de invoer van de gebruiker
            var locations = parseFloat(document.getElementById("locations").value);
            var ordersPerMonth = parseFloat(document.getElementById("ordersPerMonth").value);
            var savingsPerOrder = parseFloat(document.getElementById("savingsPerOrder").value);
            var growthInput = parseFloat(document.getElementById("growthRate").value);

            // Jaarlijkse besparing in jaar 1
            var yearlySavings = locations * ordersPerMonth * 12 * savingsPerOrder;

            // Toon het resultaat
            document.getElementById("result").innerHTML = "BESPARING IN JAAR 1: €" + yearlySavings.toFixed(2);

            // Graph data
            var labels = ["Jaar 1", "Jaar 2", "Jaar 3", "Jaar 4", "Jaar 5"];
            var savingsData = [];
            var growthRate = 1 + (growthInput / 100); // groei per jaar in percentage
            var currentSavings = yearlySavings;
            for (var i = 0; i < 5; i++) {
                savingsData.push(currentSavings);
                currentSavings *= growthRate;
            }

            // Update or create the chart
            if (savingsChart) {
                savingsChart.destroy();
            }

            var ctx = document.getElementById("savingsChart").getContext("2d");
            savingsChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Besparing per Jaar (€)',
                        data: savingsData,
                        borderColor: '#3aacf4',
                        fill: false,
                        tension: 0.1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
        }
    </script>

</body>
</html>
