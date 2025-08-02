@{
    ViewBag.Title = "Admin Panel";
    var flightStatistics = ViewBag.FlightStatistics as IEnumerable<Airline_Management_System.Models.FlightStatisticViewModel>;
}

<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
    <style>
        /* --- General Body and Layout --- */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f5f5f5;
            margin: 0;
            padding: 0;
            display: flex;
        }

        .admin-sidebar {
            width: 250px;
            background: linear-gradient(135deg, #1f1c2c, #928dab);
            color: #fff;
            padding: 30px;
            height: 100vh;
            box-shadow: 2px 0 6px rgba(0,0,0,0.1);
        }

        .admin-sidebar h3 {
            font-size: 24px;
            font-weight: 600;
            margin-bottom: 40px;
            letter-spacing: 0.5px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            padding-bottom: 20px;
        }

        .admin-sidebar a {
            display: block;
            color: #fff;
            text-decoration: none;
            font-size: 16px;
            padding: 12px 15px;
            margin-bottom: 10px;
            border-radius: 8px;
            transition: background 0.3s ease, transform 0.2s ease;
        }

        .admin-sidebar a:hover {
            background-color: rgba(255, 255, 255, 0.1);
            transform: translateX(5px);
        }

        .admin-main-content {
            flex-grow: 1;
            padding: 40px;
            overflow-y: auto;
        }

        .admin-main-content h2 {
            font-size: 32px;
            font-weight: 600;
            color: #333;
            margin-bottom: 10px;
        }
        
        .admin-main-content p {
            font-size: 16px;
            color: #555;
            margin-bottom: 30px;
        }
        
        /* --- Statistics Container --- */
        .statistics-card {
            background-color: #fff;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            padding: 30px;
            margin-bottom: 30px;
        }

        .statistics-card h3 {
            text-align: center;
            font-size: 22px;
            color: #333;
            margin-bottom: 30px;
            border-bottom: 2px solid #eee;
            padding-bottom: 15px;
        }
        
        /* --- Bar Chart Styling --- */
        .bar-chart-container {
            display: grid;
            grid-template-columns: 50px 1fr; /* For Y-axis and Chart Area */
            align-items: end;
            height: 350px;
            position: relative;
            padding: 20px 0;
            border-bottom: 2px solid #ccc;
        }

        .y-axis-labels {
            display: flex;
            flex-direction: column-reverse;
            justify-content: space-between;
            height: 100%;
            padding-right: 10px;
            text-align: right;
            border-right: 2px dashed #eee;
        }
        
        .y-axis-labels div {
            font-size: 12px;
            color: #777;
            position: relative;
        }

        .y-axis-labels div::before {
            content: '';
            position: absolute;
            top: 50%;
            right: -10px;
            width: 10px;
            height: 1px;
            background-color: #eee;
        }

        .chart-grid {
            display: flex;
            align-items: flex-end;
            gap: 15px;
            height: 100%;
            padding-left: 20px;
        }

        .chart-bar-wrapper {
            position: relative;
            flex-grow: 1;
            height: 100%;
            text-align: center;
            display: flex;
            flex-direction: column-reverse;
            align-items: center;
        }

        .chart-bar {
            background-color: #3f51b5; /* A vibrant blue color */
            width: 70%;
            min-height: 5px; /* Minimum height for bars with 0 or low values */
            border-radius: 5px 5px 0 0;
            transition: height 0.5s ease-in-out, background-color 0.3s ease;
        }

        .chart-bar:hover {
            background-color: #2c387e; /* Darker shade on hover */
        }
        
        .bar-tooltip {
            position: absolute;
            bottom: calc(100% + 5px); /* Position above the bar */
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0,0,0,0.8);
            color: #fff;
            padding: 5px 10px;
            border-radius: 5px;
            white-space: nowrap;
            opacity: 0;
            transition: opacity 0.3s ease;
            pointer-events: none;
            z-index: 10;
        }

        .chart-bar-wrapper:hover .bar-tooltip {
            opacity: 1;
        }
        
        .x-axis-label {
            position: absolute;
            bottom: -25px; /* Position below the bar */
            left: 50%;
            transform: translateX(-50%);
            font-size: 12px;
            color: #555;
            text-overflow: ellipsis;
            white-space: nowrap;
            overflow: hidden;
            width: 100%;
        }

        .no-data-message {
            text-align: center;
            padding: 50px;
            color: #777;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div class="admin-sidebar">
        <h3>Admin Menu</h3>
        <nav>
            <a href="@Url.Action("AdminPanel", "Admin")">Admin Panel</a>
            <a href="@Url.Action("AddFlight", "Admin")">Add Flight</a>
            <a href="@Url.Action("PassengerInfo", "Admin")">Passenger Info</a>
            <a href="@Url.Action("Flights", "Admin")">Manage Flights</a>
            <a href="@Url.Action("Index", "Home")">Back to Home</a>
        </nav>
    </div>

    <div class="admin-main-content">
        <h2>Welcome to Admin Panel!</h2>
        <p>Use the left menu to manage routes, flights, and passengers efficiently.</p>

        <div class="statistics-card">
            <h3>Flights per Route Statistics</h3>
            @if (flightStatistics != null && flightStatistics.Any())
            {
                var maxCount = flightStatistics.Max(s => s.FlightCount);
                if (maxCount == 0) { maxCount = 1; }

                <div class="bar-chart-container">
                    <div class="y-axis-labels">
                        <div>@maxCount</div>
                        <div>@Math.Round((double)maxCount * 0.75)</div>
                        <div>@Math.Round((double)maxCount * 0.5)</div>
                        <div>@Math.Round((double)maxCount * 0.25)</div>
                        <div>0</div>
                    </div>
                    <div class="chart-grid">
                        @foreach (var stat in flightStatistics)
                        {
                            var barHeight = (int)Math.Round((double)stat.FlightCount / maxCount * 100);
                            <div class="chart-bar-wrapper">
                                <div class="chart-bar" style="height: @barHeight%;"></div>
                                <div class="bar-tooltip">@stat.FlightCount flight@(stat.FlightCount > 1 ? "s" : "")</div>
                                <div class="x-axis-label">@stat.RouteName</div>
                            </div>
                        }
                    </div>
                </div>
            }
            else
            {
                <div class="no-data-message">No flight data available to display.</div>
            }
        </div>
    </div>
</body>
</html>
