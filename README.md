     
                    <!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>@ViewBag.Title</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet" />
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: 'Inter', sans-serif;
            background-color: #f0f0f0;
            color: #333;
        }

        /* Layout using floats */
        .sidebar {
            float: left;
            width: 230px;
            height: 100%;
            background: #2c3e50;
            color: #fff;
            padding: 20px;
            box-sizing: border-box;
        }

        .sidebar h3 {
            margin-bottom: 20px;
            font-size: 20px;
            font-weight: bold;
        }

        .sidebar a {
            display: block;
            color: #fff;
            text-decoration: none;
            margin-bottom: 10px;
            padding: 8px;
            background-color: #34495e;
            border-radius: 4px;
        }

        .sidebar a:hover {
            background-color: #3b5998;
        }

        .main-content {
            margin-left: 250px; /* Equal to sidebar width + padding */
            padding: 30px;
            box-sizing: border-box;
        }

        .main-content h2 {
            margin-top: 0;
        }

        .statistics-container {
            margin-top: 20px;
            background-color: #fff;
            border: 1px solid #ccc;
            padding: 20px;
            border-radius: 6px;
        }

        .statistics-container h3 {
            text-align: center;
            margin-bottom: 20px;
        }

        /* Bar Graph without flex */
        .graph-container {
            border-top: 1px solid #ccc;
            padding-top: 10px;
            overflow-x: auto;
            white-space: nowrap;
        }

        .bar-wrapper {
            display: inline-block;
            width: 100px;
            margin: 0 10px;
            vertical-align: bottom;
            position: relative;
        }

        .bar {
            background-color: #2980b9;
            width: 60px;
            margin: 0 auto;
            border-radius: 4px 4px 0 0;
        }

        .bar-wrapper:hover .bar {
            background-color: #1c6690;
        }

        .tooltip {
            position: absolute;
            top: -25px;
            left: 50%;
            transform: translateX(-50%);
            background: #000;
            color: #fff;
            padding: 3px 8px;
            font-size: 12px;
            border-radius: 4px;
            display: none;
        }

        .bar-wrapper:hover .tooltip {
            display: block;
        }

        .x-label {
            margin-top: 5px;
            font-size: 12px;
            color: #555;
            text-align: center;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .clearfix::after {
            content: "";
            display: block;
            clear: both;
        }
    </style>
</head>
<body>
    <div class="sidebar">
        <h3>Admin Menu</h3>
        <a href="@Url.Action("AdminPanel", "Admin")">Admin Panel</a>
        <a href="@Url.Action("AddFlight", "Admin")">Add Flight</a>
        <a href="@Url.Action("PassengerInfo", "Admin")">Passenger Info</a>
        <a href="@Url.Action("Flights", "Admin")">Manage Flights</a>
        <a href="@Url.Action("Index", "Home")">Back to Home</a>
    </div>

    <div class="main-content">
        <h2>Welcome to Admin Panel</h2>
        <p>Use the menu to manage flights, passengers, and more.</p>

        <div class="statistics-container">
            <h3>Flights per Route Statistics</h3>

            <div class="graph-container">
                @if (flightStatistics != null && flightStatistics.Any())
                {
                    var maxCount = flightStatistics.Max(s => s.FlightCount);
                    if (maxCount == 0) { maxCount = 1; }

                    foreach (var stat in flightStatistics)
                    {
                        var barHeight = (int)Math.Round((double)stat.FlightCount / maxCount * 200); // max height 200px

                        <div class="bar-wrapper">
                            <div class="bar" style="height: @barHeightpx;"></div>
                            <div class="tooltip">
                                @(stat.FlightCount == 1 ? "1 flight" : stat.FlightCount + " flights")
                            </div>
                            <div class="x-label" title="@stat.RouteName">@stat.RouteName</div>
                        </div>
                    }
                }
                else
                {
                    <p style="text-align:center;">No flight data available.</p>
                }
            </div>
        </div>
    </div>

    <div class="clearfix"></div>
</body>
</html>
                        
