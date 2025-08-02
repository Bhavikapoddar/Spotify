
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
        /* --- General Reset and Main Layout with Float --- */
        html, body {
            margin: 0;
            padding: 0;
            height: 100%;
            font-family: 'Inter', sans-serif;
            background-color: #f0f2f5;
        }

        /* Use this class to contain floated elements */
        .clearfix::after {
            content: "";
            display: table;
            clear: both;
        }
        
        /* --- Sidebar Styling --- */
        .admin-sidebar {
            width: 250px;
            background: linear-gradient(135deg, #2c3e50, #34495e);
            color: #ecf0f1;
            padding: 30px 20px;
            box-shadow: -2px 0 10px rgba(0,0,0,0.1); /* Shadow on the left side */
            float: right; /* Align sidebar to the right side of the page */
            height: 100vh;
        }

        .admin-sidebar h3 {
            font-size: 24px;
            font-weight: 600;
            margin-bottom: 40px;
            letter-spacing: 0.5px;
            text-align: center;
        }

        .admin-sidebar nav a {
            display: block;
            color: #ecf0f1;
            text-decoration: none;
            font-size: 16px;
            padding: 12px 15px;
            margin-bottom: 10px;
            border-radius: 8px;
            transition: background 0.3s ease;
        }

        .admin-sidebar nav a:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }
        
        /* --- Main Content Styling --- */
        .admin-main-content {
            margin-right: 250px; /* Pushes content to the left of the sidebar */
            padding: 40px;
            overflow-y: auto;
            min-height: 100vh;
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

        /* --- Statistics Card Styling --- */
        .statistics-card {
            background-color: #fff;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            padding: 30px;
        }
        
        .statistics-card h3 {
            text-align: center;
            font-size: 22px;
            color: #333;
            margin-bottom: 30px;
            border-bottom: 1px solid #e0e0e0;
            padding-bottom: 15px;
        }
        
        /* --- Bar Chart Styling --- */
        .bar-chart-container {
            position: relative;
            height: 300px;
            padding-left: 50px;
            padding-bottom: 30px;
            border-bottom: 2px solid #ccc;
        }

        .y-axis-labels {
            position: absolute;
            left: 0;
            top: 0;
            bottom: 30px;
            width: 40px;
            text-align: right;
            padding-right: 10px;
            font-size: 12px;
            color: #777;
        }

        .y-axis-labels div {
            position: absolute;
            right: 0;
            transform: translateY(-50%);
        }
        .y-axis-labels .y-label-max { top: 0; transform: none; }
        .y-axis-labels .y-label-75 { top: 25%; }
        .y-axis-labels .y-label-50 { top: 50%; }
        .y-axis-labels .y-label-25 { top: 75%; }
        .y-axis-labels .y-label-0 { bottom: 0; top: auto; transform: none; }
        .y-axis-labels div::after {
            content: '';
            position: absolute;
            top: 50%;
            right: -10px;
            width: 10px;
            height: 1px;
            background-color: #eee;
        }

        .chart-grid {
            position: absolute;
            top: 0;
            left: 50px;
            right: 0;
            bottom: 30px;
        }

        .chart-bar-wrapper {
            position: relative;
            float: left;
            width: 15%;
            height: 100%;
            margin-right: 10px;
        }
        .chart-bar-wrapper:last-child {
            margin-right: 0;
        }

        .chart-bar {
            background-color: #3498db;
            width: 100%;
            min-height: 5px;
            border-radius: 5px 5px 0 0;
            transition: height 0.5s ease-in-out;
            position: absolute;
            bottom: 0;
            left: 0;
        }

        .chart-bar:hover {
            background-color: #2980b9;
        }

        .bar-tooltip {
            position: absolute;
            bottom: calc(100% + 5px);
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0,0,0,0.85);
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
            bottom: -25px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 12px;
            color: #555;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            width: 100%;
            text-align: center;
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
    <div class="clearfix">
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
                            <div class="y-label-max">@maxCount</div>
                            <div class="y-label-75">@Math.Round((double)maxCount * 0.75)</div>
                            <div class="y-label-50">@Math.Round((double)maxCount * 0.5)</div>
                            <div class="y-label-25">@Math.Round((double)maxCount * 0.25)</div>
                            <div class="y-label-0">0</div>
                        </div>
                        <div class="chart-grid clearfix">
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
    </div>
</body>
</html>
