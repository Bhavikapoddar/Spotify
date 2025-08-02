     
@* AdminPanel.cshtml *@
@using Airline_Managementnew.Models
@{
    ViewBag.Title = "Admin Panel";
    var flightStatistics = ViewBag.FlightStatistics as IEnumerable<FlightStatisticViewModel>;
}

<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
    <style>
        /* Base Styles */
        html, body {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            height: 100%;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: #f5f5f5;
            color: #333;
        }

        /* Sidebar Styles */
        .sidebar {
            width: 230px;
            background: linear-gradient(135deg, #1f1c2c, #928dab);
            color: white;
            padding: 30px 20px;
            height: 100vh;
            box-shadow: 2px 0 6px rgba(0,0,0,0.1);
            float: left;
        }

        .sidebar h3 {
            margin-bottom: 30px;
            font-size: 22px;
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        .sidebar a {
            display: block;
            color: white;
            text-decoration: none;
            font-size: 16px;
            padding: 10px 12px;
            margin-bottom: 12px;
            border-radius: 8px;
            transition: background 0.3s ease;
        }

        .sidebar a:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }

        /* Main Content */
        .main-content {
            margin-left: 230px;
            padding: 40px;
            overflow: hidden;
        }

        .main-content h2 {
            font-size: 28px;
            font-weight: 600;
            margin-bottom: 10px;
        }

        .main-content p {
            font-size: 16px;
            color: #555;
        }

        .clearfix::after {
            content: "";
            display: table;
            clear: both;
        }

        /* Statistics Container */
        .statistics-container {
            border: 1px solid #ccc;
            padding: 20px;
            margin-top: 20px;
            background-color: #fff;
            border-radius: 8px;
            overflow: hidden;
        }
        
        .statistics-container h3 {
            text-align: center;
            margin-bottom: 20px;
            color: #333;
        }

        /* Graph Container */
        .graph-container {
            display: table;
            width: 100%;
            height: 300px;
            position: relative;
            border-bottom: 1px solid #eee;
        }

        /* Y-axis */
        .y-axis {
            display: table-cell;
            width: 40px;
            vertical-align: bottom;
            text-align: right;
            padding-right: 5px;
            height: 100%;
            position: relative;
        }

        /* Y-axis labels are now dynamically generated */
        .y-label {
            position: relative;
            border-bottom: 1px dashed #ccc;
            line-height: 0;
            height: 0; /* height will be set dynamically below */
        }
        
        .y-label:first-child {
            border-bottom: none;
        }

        .y-label::after {
            content: attr(data-value);
            position: absolute;
            top: 50%;
            right: 5px;
            transform: translateY(-50%);
            font-size: 12px;
            color: #555;
        }

        /* Bar container */
        .bars-container {
            display: table-cell;
            vertical-align: bottom;
            height: 100%;
            white-space: nowrap;
            overflow-x: auto;
            padding-left: 10px;
        }

        /* Bar wrapper */
        .bar-wrapper {
            display: inline-block;
            width: 15%;
            height: 100%;
            text-align: center;
            margin: 0 1%;
            position: relative;
            vertical-align: bottom;
        }

        /* Bar itself */
        .bar {
            background-color: #3f51b5;
            width: 60%;
            margin: 0 auto;
            position: absolute;
            bottom: 0;
            left: 20%;
            transition: all 0.3s ease;
            border-radius: 4px 4px 0 0;
            cursor: default; /* Change cursor to default since it's not clickable */
        }

        /* Bar Hover Effect */
        .bar-wrapper:hover .bar {
            background-color: #303f9f;
        }
        
        /* X-axis Label (Route Name) */
        .x-label {
            position: absolute;
            bottom: -25px;
            left: 0;
            right: 0;
            font-size: 12px;
            color: #555;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        /* Tooltip (Flight Count) */
        .tooltip {
            position: absolute;
            top: -30px;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 5px 8px;
            border-radius: 5px;
            white-space: nowrap;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
            z-index: 10;
        }

        /* Show tooltip on bar-wrapper hover */
        .bar-wrapper:hover .tooltip {
            opacity: 1;
        }
    </style>
</head>
<body>
    <div class="sidebar">
        <h3>Admin Menu</h3>
        <nav>
            <a href="@Url.Action("AdminPanel", "Admin")"> Admin Panel</a>
            <a href="@Url.Action("AddFlight", "Admin")"> Add Flight</a>
            <a href="@Url.Action("PassengerInfo", "Admin")"> Passenger Info</a>
            <a href="@Url.Action("Flights", "Admin")"> Manage Flights</a>
            <a href="@Url.Action("Index", "Home")"> Back to Home</a>
        </nav>
    </div>

    <div class="main-content">
        <h2>Welcome to Admin Panel!</h2>
        <p>Use the left menu to manage routes, flights, and passengers efficiently.</p>

        <div class="statistics-container">
            <h3>Flights per Route Statistics</h3>
            
            <div class="graph-container">
                <div class="y-axis">
                    @if (flightStatistics != null && flightStatistics.Any())
                    {
                        var maxCount = flightStatistics.Max(s => s.FlightCount);
                        // Ensure maxCount is at least 1 to avoid division by zero
                        if (maxCount == 0) { maxCount = 1; }

                        // Calculate the height of each Y-axis label dynamically
                        var yLabelHeight = (100.0 / maxCount);

                        // Loop from maxCount down to 1
                        for (int i = maxCount; i >= 1; i--)
                        {
                            <div class="y-label" data-value="@i" style="height: @yLabelHeight%;"></div>
                        }
                    }
                    else
                    {
                        <div class="y-label" data-value="1" style="height: 100%;"></div>
                    }
                </div>
                
                <div class="bars-container">
                    @if (flightStatistics != null && flightStatistics.Any())
                    {
                        var maxCount = flightStatistics.Max(s => s.FlightCount);
                        if (maxCount == 0) { maxCount = 1; }

                        foreach (var stat in flightStatistics)
                        {
                            var barHeight = (int)Math.Round((double)stat.FlightCount / maxCount * 100);

                            <div class="bar-wrapper">
                                <div class="bar" style="height: @barHeight%;"></div>
                                <div class="tooltip">
                                    @(stat.FlightCount == 1 ? "1 flight" : stat.FlightCount + " flights")
                                </div>
                                <div class="x-label" title="@stat.RouteName">@stat.RouteName</div>
                            </div>
                        }
                    }
                    else
                    {
                        <p style="text-align: center; width: 100%; padding-top: 50px;">No flight data available to display.</p>
                    }
                </div>
            </div>
            <div class="clearfix"></div>
            <div class="x-axis-label" style="text-align: center; margin-top: 30px;">Routes</div>
        </div>
    </div>
</body>
</html>
