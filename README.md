@{
    // Assuming this is your Admin Panel View
    ViewBag.Title = "Admin Panel";
    var flightStatistics = ViewBag.FlightStatistics as IEnumerable<Airline_Managementnew.Controllers.AdminController.FlightStatisticModel>;
}

<!DOCTYPE html>
<html>
<head>
    <title>@ViewBag.Title</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap" rel="stylesheet">
    <style>
        /* Base Styles */
        html, body {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
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
            float: left; /* Use float for layout */
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

        .sidebar a span {
            margin-right: 10px;
            font-size: 18px;
        }

        /* Main Content */
        .main-content {
            margin-left: 230px; /* Adjust margin to clear the float */
            padding: 40px;
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

        .y-label {
            height: 20%;
            position: relative;
            border-bottom: 1px dashed #ccc;
            line-height: 0;
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
        }

        /* Bar wrapper */
        .bar-wrapper {
            display: inline-block;
            width: 15%; /* Adjust width as needed */
            height: 100%;
            text-align: center;
            margin: 0 2%;
            position: relative;
            vertical-align: bottom;
        }

        /* Bar */
        .bar {
            background-color: #3f51b5;
            width: 60%;
            margin: 0 auto;
            position: absolute;
            bottom: 0;
            left: 20%;
            transition: all 0.3s ease;
            border-radius: 4px 4px 0 0;
        }

        /* Bar Hover */
        .bar-wrapper:hover .bar {
            background-color: #303f9f;
            cursor: pointer;
        }
        
        /* X-axis Label */
        .x-label {
            position: absolute;
            bottom: -20px;
            left: 0;
            right: 0;
            font-size: 12px;
            color: #555;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        /* Tooltip */
        .tooltip {
            position: absolute;
            top: -30px; /* Adjusted to be above the bar */
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
            z-index: 10; /* Ensure tooltip is on top */
        }

        .bar-wrapper:hover .tooltip {
            opacity: 1;
        }
    </style>
</head>
<body>
    <div class="sidebar">
        <h3>Admin Menu</h3>
        <nav>
            <a href="/Admin/AdminPanel"><span class="icon">&#9776;</span> Admin Panel</a>
            <a href="/Admin/AddFlight"><span class="icon">&#9992;</span> Add Flight</a>
            <a href="/Admin/PassengerInfo"><span class="icon">&#128100;</span> Passenger Info</a>
            <a href="/Admin/Flights"><span class="icon">&#128221;</span> Manage Flights</a>
            <a href="/Home/Index"><span class="icon">&#8962;</span> Back to Home</a>
        </nav>
    </div>

    <div class="main-content">
        <h2>Welcome to Admin Panel!</h2>
        <p>Use the left menu to manage routes, flights, and passengers efficiently.</p>

        <div class="statistics-container">
            <h3>Flights per Route Statistics</h3>
            
            <div class="graph-container">
                <div class="y-axis">
                    @for (int i = 5; i > 0; i--)
                    {
                        <div class="y-label" data-value="@(i)"></div>
                    }
                </div>
                
                <div class="bars-container">
                    @if (flightStatistics != null && flightStatistics.Any())
                    {
                        // Find the max flight count to scale the bars
                        var maxCount = flightStatistics.Max(s => s.FlightCount);

                        if (maxCount == 0) // Avoid division by zero
                        {
                            maxCount = 1;
                        }

                        // Loop through flight statistics
                        foreach (var stat in flightStatistics)
                        {
                            // Calculate the height of the bar based on the max count
                            var barHeight = (int)Math.Round((double)stat.FlightCount / maxCount * 100);

                            <div class="bar-wrapper">
                                <a href="@Url.Action("EditFlight", "Admin", new { id = stat.FlightId })">
                                    <div class="bar" style="height: @barHeight%;"></div>
                                </a>
                                <div class="tooltip">@stat.FlightCount flights</div>
                                <div class="x-label" title="@stat.RouteName">@stat.RouteName</div>
                            </div>
                        }
                    }
                    else
                    {
                        <p>No flight data available to display.</p>
                    }
                </div>
            </div>
            <div class="clearfix"></div>
            <div class="x-axis-label" style="text-align: center; margin-top: 30px;">Routes</div>
        </div>
    </div>
</body>
</html>
