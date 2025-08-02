   using System;
using System.Collections.Generic;
using System.Linq;
using System.Web.Mvc;
using Airline_Managementnew.Models;

namespace Airline_Managementnew.Controllers
{
    public class AdminController : Controller
    {
        private readonly DatabaseContext db = new DatabaseContext();

        public ActionResult AdminPanel()
        {
            var allRoutes = db.SearchRequest.ToList();
            
            // Get flight counts for each route that has at least one flight.
            var flightCounts = db.Flight
                .GroupBy(f => f.RouteId)
                .Select(g => new
                {
                    RouteId = g.Key,
                    FlightCount = g.Count()
                })
                .ToList();
            
            // Perform a LEFT JOIN to combine all routes with the flight counts.
            // If a route has no flights, its 'FlightCount' will be 0.
            var flightStatistics = allRoutes.GroupJoin(
                flightCounts,
                route => route.RouteId,
                count => count.RouteId,
                (route, group) => new FlightStatisticViewModel
                {
                    RouteName = route.FromLocation + " to " + route.Tolocation,
                    FlightCount = group.Any() ? group.Sum(c => c.FlightCount) : 0
                })
                .ToList();
            
            ViewBag.FlightStatistics = flightStatistics;
            ViewBag.Flights = db.Flight.ToList();
            ViewBag.Routes = allRoutes;
            
            return View();
        }

        // AddRoute Action Methods
        public ActionResult AddRoute()
        {
            ViewBag.Routes = db.SearchRequest.ToList();
            return View();
        }
        
        [HttpPost]
        public ActionResult AddRoute(SearchRequest route)
        {
            if (ModelState.IsValid)
            {
                db.SearchRequest.Add(route);
                db.SaveChanges();
            }
            return RedirectToAction("AddRoute");
        }

        // EditRoute Action Methods
        public ActionResult EditRoute(int id)
        {
            var route = db.SearchRequest.Find(id);
            if (route == null)
            {
                return HttpNotFound();
            }
            return View(route);
        }
        
        [HttpPost]
        public ActionResult EditRoute(SearchRequest updatedRoute)
        {
            if (ModelState.IsValid)
            {
                var route = db.SearchRequest.Find(updatedRoute.RouteId);
                if (route != null)
                {
                    route.FromLocation = updatedRoute.FromLocation;
                    route.Tolocation = updatedRoute.Tolocation;
                    route.DepartureDate = updatedRoute.DepartureDate;
                    route.RouteNumber = updatedRoute.RouteNumber;
                    db.SaveChanges();
                }
            }
            return RedirectToAction("EditRoute", new { id = updatedRoute.RouteId });
        }

        // DeleteRoute Action Method
        [HttpPost]
        public ActionResult DeleteRoute(int id)
        {
            var route = db.SearchRequest.Find(id);
            if (route != null)
            {
                db.SearchRequest.Remove(route);
                db.SaveChanges();
            }
            return RedirectToAction("AddRoute");
        }

        // AddFlight Action Methods
        public ActionResult AddFlight()
        {
            ViewBag.Flights = db.Flight.ToList();
            ViewBag.Routes = db.SearchRequest.ToList();
            return View();
        }

        [HttpPost]
        public ActionResult AddFlight(Flight model)
        {
            if (ModelState.IsValid)
            {
                db.Flight.Add(model);
                db.SaveChanges();
                return RedirectToAction("AdminPanel");
            }
            ViewBag.Flights = db.Flight.ToList();
            ViewBag.Routes = db.SearchRequest.ToList();
            return View(model);
        }

        // EditFlight Action Methods
        public ActionResult EditFlight(int id)
        {
            var flight = db.Flight.Find(id);
            if (flight == null)
            {
                return HttpNotFound();
            }
            ViewBag.Routes = db.SearchRequest.ToList();
            return View(flight);
        }
        
        [HttpPost]
        public ActionResult EditFlight(Flight updatedFlight)
        {
            if (ModelState.IsValid)
            {
                var flight = db.Flight.Find(updatedFlight.FlightId);
                if (flight != null)
                {
                    flight.RouteId = updatedFlight.RouteId;
                    flight.DepartureCity = updatedFlight.DepartureCity;
                    flight.ArrivalCity = updatedFlight.ArrivalCity;
                    flight.DepartureTime = updatedFlight.DepartureTime;
                    flight.ArrivalTime = updatedFlight.ArrivalTime;
                    flight.EconomyPrice = updatedFlight.EconomyPrice;
                    flight.EconomySeats = updatedFlight.EconomySeats;
                    flight.BusinessPrice = updatedFlight.BusinessPrice;
                    flight.BusinessSeats = updatedFlight.BusinessSeats;
                    flight.FirstClassPrice = updatedFlight.FirstClassPrice;
                    flight.FirstClassSeats = updatedFlight.FirstClassSeats;
                    flight.AirplaneId = updatedFlight.AirplaneId;
                    flight.AirplaneNumber = updatedFlight.AirplaneNumber;
                    db.SaveChanges();
                }
            }
            return RedirectToAction("EditFlight", new { id = updatedFlight.FlightId });
        }

        // DeleteFlight Action Method
        [HttpPost]
        public ActionResult DeleteFlight(int id)
        {
            var flight = db.Flight.Find(id);
            if (flight != null)
            {
                db.Flight.Remove(flight);
                db.SaveChanges();
            }
            return RedirectToAction("AddFlight");
        }
        
        public ActionResult PassengerInfo()
        {
            var passengers = db.Passengers.ToList();
            return View(passengers);
        }
    }
}
.,.... 
..... 
.... 
@model Airline_Managementnew.Models.FlightStatisticViewModel
@{
    ViewBag.Title = "Admin Panel";
}

<head>
    <title>Admin Panel</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" />
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" />
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f5f6fa;
            color: #333;
            margin: 0;
            padding: 0;
        }

        .sidebar {
            width: 230px;
            background: linear-gradient(135deg, #1f1c2c, #928dab);
            color: white;
            padding: 30px 20px;
            min-height: 100vh;
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
            margin-bottom: 8px;
            border-radius: 8px;
            transition: background 0.3s ease;
        }

        .sidebar a:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }
        
        .main-content {
            margin-left: 230px; /* Offset for the sidebar width */
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
        
        .statistics-container {
            border: 1px solid #ccc;
            padding: 20px;
            margin-top: 20px;
            background-color: #f9f9f9;
            border-radius: 8px;
            overflow: hidden; /* Clear floats */
        }
        
        .statistics-container h3 {
            text-align: center;
            margin-bottom: 20px;
            color: #333;
        }

        .graph-container {
            overflow-x: auto; /* This is the "slider" you requested */
            overflow-y: hidden;
            height: 300px;
            position: relative;
            white-space: nowrap; /* Prevents bars from wrapping to the next line */
        }
        
        .y-axis {
            float: left;
            width: 40px;
            height: 100%;
            display: inline-block;
            vertical-align: bottom;
            text-align: right;
            padding-right: 5px;
        }

        .y-label {
            height: 20%;
            border-bottom: 1px dashed #ccc;
            line-height: 0;
            position: relative;
            display: block;
        }
        
        .y-label:first-child {
            border-bottom: none;
        }
        
        .bars-container {
            height: 100%;
            display: inline-block;
            vertical-align: bottom;
            white-space: nowrap; /* Required for the scrollbar to work */
        }
        
        .bar-wrapper {
            height: 100%;
            text-align: center;
            margin: 0 10px;
            position: relative;
            display: inline-block; /* Use inline-block for side-by-side display */
            vertical-align: bottom;
        }
        
        .bar {
            background-color: #3f51b5;
            width: 30px; /* Fixed width for the bars */
            transition: all 0.3s ease;
            border-radius: 4px 4px 0 0;
            position: relative;
            display: inline-block;
            vertical-align: bottom;
        }
        
        .bar:hover {
            background-color: #303f9f;
            cursor: pointer;
        }

        .tooltip {
            position: absolute;
            bottom: 100%;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 5px 8px;
            border-radius: 5px;
            white-space: nowrap;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
            z-index: 10;
        }

        .bar-wrapper:hover .tooltip {
            opacity: 1;
            visibility: visible;
        }

        .x-label {
            position: absolute;
            bottom: -20px;
            left: 0;
            right: 0;
            font-size: 12px;
            color: #555;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            width: 50px;
            margin: 0 -10px;
        }
    </style>
</head>

<body>
    <div class="sidebar">
        <h3>Admin Menu</h3>
        <a href="/Admin/AddRoute"> Add Route</a>
        <a href="/Admin/AddFlight"> Add Flight</a>
        <a href="/Admin/PassengerInfo"> Passenger Info</a>
        <a href="/Admin/Flights"> Manage Flights</a>
        <a href="/"> Back to Home</a>
    </div>

    <div class="main-content">
        <h2>Welcome to Admin Panel!</h2>
        <p>Use the left menu to manage routes, flights, and passengers efficiently.</p>

        <div class="statistics-container">
            <h3>Flights per Route Statistics</h3>
            <div class="graph-container">
                <div class="y-axis">
                    <div class="y-label">15</div>
                    <div class="y-label">12</div>
                    <div class="y-label">9</div>
                    <div class="y-label">6</div>
                    <div class="y-label">3</div>
                    <div class="y-label" style="border: none;">0</div>
                </div>

                <div class="bars-container">
                    @if (ViewBag.FlightStatistics != null && ((List<FlightStatisticViewModel>)ViewBag.FlightStatistics).Any())
                    {
                        var flightStats = (List<FlightStatisticViewModel>)ViewBag.FlightStatistics;
                        var maxCount = flightStats.Any() ? flightStats.Max(s => s.FlightCount) : 1;
                        maxCount = maxCount > 0 ? maxCount : 1;

                        foreach (var stat in flightStats)
                        {
                            var barHeight = (int)Math.Round((double)stat.FlightCount / maxCount * 100);

                            <div class="bar-wrapper">
                                <div class="bar" style="height: @(barHeight)%">
                                    <div class="tooltip">@stat.FlightCount flights</div>
                                </div>
                                <div class="x-label">@stat.RouteName</div>
                            </div>
                        }
                    }
                    else
                    {
                        <p>No flight data available to display.</p>
                    }
                </div>
            </div>
        </div>
    </div>
</body>
</html>

