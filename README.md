------BookingController----------------
using Airline.Models;
using System;
using System.Collections.Generic;
using System.Web.Mvc;
using Airline.Models;

public class BookingController : Controller
{
    [HttpPost]
    public ActionResult SearchFlights(SearchRequest request)
    {
        // Simulate results based on input (you can later query DB here)
        var results = new List<Flight>
    {
        new Flight
        {
            Id = 1,
            DepartureCity = request.FromLocation,
            ArrivalCity = request.ToLocation,
            DepartureTime = request.DepartureDate.AddHours(16).AddMinutes(44),
            ArrivalTime = request.DepartureDate.AddHours(17).AddMinutes(50),
            EconomyPrice = 1000,
            EconomySeats = 12,
            BusinessPrice = 8000,
            BusinessSeats = 20,
            FirstClassPrice = null,
            FirstClassSeats = null,
            AirplaneId = "A101",
            AirplaneNumber = "IN-AIR123"
        },
        new Flight
        {
            Id = 2,
            DepartureCity = request.FromLocation,
            ArrivalCity = request.ToLocation,
            DepartureTime = request.DepartureDate.AddHours(19).AddMinutes(50),
            ArrivalTime = request.DepartureDate.AddHours(21).AddMinutes(40),
            EconomyPrice = 1002,
            EconomySeats = 13,
            BusinessPrice = 8002,
            BusinessSeats = 10,
            FirstClassPrice = 5002,
            FirstClassSeats = 15,
            AirplaneId = "A102",
            AirplaneNumber = "IN-AIR456"
        }
    };

        ViewBag.From = request.FromLocation;
        ViewBag.To = request.ToLocation;
        ViewBag.DepartureDate = request.DepartureDate.ToString("dd MMM yyyy");

        return View("SearchResult", results);
    }
    public ActionResult PassengerInfo(string flightId, string travelClass, string departureDate)
    {
        ViewBag.FlightId = flightId;
        ViewBag.Class = travelClass;
        ViewBag.DepartureDate = departureDate; // Add this line

        return View();
    }
    public ActionResult SeatSelection(int flightId, string @class, DateTime departureDate)
    {
        var seats = new List<Seat>();
        for (int i = 1; i <= 30; i++)
        {
            seats.Add(new Seat
            {
                SeatNumber = i,
                SeatClass = @class,
                IsAvailable = i <= 10,
                IsWindow = (i % 6 == 1 || i % 6 == 0),
                IsBookedByUser = false
            });
        }
        ViewBag.DepartureDate = departureDate.ToString("yyyy-MM-dd");
        ViewBag.FlightId = flightId;
        ViewBag.Class = @class;

        switch (@class)
        {
            case "Economy":
                ViewBag.Price = 1000;
                break;
            case "BusinessClass":
                ViewBag.Price = 5000;
                break;
            case "FirstClass":
                ViewBag.Price = 8000;
                break;
            default:
                ViewBag.Price = 0;
                break;
        }

        return View(seats); // make sure SeatSelection.cshtml exists
    }
    [HttpPost]
    public ActionResult ConfirmBooking(int flightId, string seatNumber, string @class)
    {
        // Sample flight object (you can fetch this from the database)
        var flight = new Flight
        {
            Id = flightId,
            DepartureCity = "Delhi",
            ArrivalCity = "Mumbai",
            DepartureTime = DateTime.Now.AddHours(2),
            ArrivalTime = DateTime.Now.AddHours(4),
            AirplaneNumber = "IN-AIR123"
        };

        // Use traditional switch statement (compatible with older C# versions)
        int price = 0;
        switch (@class)
        {
            case "Economy":
                price = 1000;
                break;
            case "BusinessClass":
                price = 5000;
                break;
            case "FirstClass":
                price = 8000;
                break;
            default:
                price = 0;
                break;
        }

        // Add ₹100 if seat is window seat (e.g., seats at position 1 or 6 in a row)
        int seatNum;
        if (int.TryParse(seatNumber, out seatNum))
        {
            if (seatNum % 6 == 1 || seatNum % 6 == 0)
            {
                price += 100;
            }
        }

        // Pass details to the view using ViewBag
        ViewBag.SeatNumber = seatNumber;
        ViewBag.Class = @class;
        ViewBag.Price = price;
        ViewBag.FlightId = flightId;

        ViewBag.DepartureCity = flight.DepartureCity;
        ViewBag.ArrivalCity = flight.ArrivalCity;
        ViewBag.DepartureTime = flight.DepartureTime.ToString("hh:mm tt");
        ViewBag.ArrivalTime = flight.ArrivalTime.ToString("hh:mm tt");
        ViewBag.AirplaneNumber = flight.AirplaneNumber;
        ViewBag.DepartureDate = flight.DepartureTime.ToString("yyyy-MM-dd");

        return View("PassengerInfo");
    }
    [HttpPost]
    public ActionResult ConfirmPayment()
    {
        // Dummy logic — in a real app, you'd integrate payment gateway here.
        return View("PaymentSuccess");
    }
}
--------------------------------------------------------------
PassengerInfo.cshtml--------------

@{
    ViewBag.Title = "Passenger Info";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<!-- Custom Styling -->
<style>
    body {
        background-color: #f4f8fb;
    }

    .passenger-card {
        border: none;
        box-shadow: 0 6px 18px rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        background-color: #ffffff;
    }

        .passenger-card h5 {
            color: #2c3e50;
            font-weight: 600;
            margin-bottom: 20px;
        }

    .passenger-label {
        font-weight: 500;
        color: #34495e;
    }

    .passenger-input,
    .passenger-select {
        border-radius: 8px;
        border: 1px solid #dfe6e9;
        box-shadow: none;
        transition: border 0.2s ease-in-out;
    }

        .passenger-input:focus,
        .passenger-select:focus {
            border-color: #3498db;
            outline: none;
        }

    .passenger-btn-primary {
        background-color: #3498db;
        border-color: #3498db;
        padding: 10px 25px;
        font-size: 16px;
        border-radius: 6px;
        color: #fff;
    }

        .passenger-btn-primary:hover {
            background-color: #2980b9;
            border-color: #2980b9;
        }

    .passenger-btn-secondary {
        background-color: #95a5a6;
        border-color: #95a5a6;
        padding: 10px 25px;
        font-size: 16px;
        border-radius: 6px;
        margin-right: 10px;
        color: #fff;
    }

    h2.passenger-title {
        font-weight: 700;
        margin-top: 40px;
        color: #2d3436;
    }

    .passenger-container {
        max-width: 960px;
    }

    .passenger-mb-5 {
        margin-bottom: 4rem !important;
    }
</style>

<h2 class="text-center passenger-title mb-4">Passenger Details</h2>

<div class="container passenger-container mt-4">
    <form method="post" action="/Booking/ConfirmPayment">

        <!-- FLIGHT INFO -->
        <div class="passenger-card p-4 mb-4">
            <h5>Flight Details</h5>
            <div class="row">
                <!-- Existing flight fields -->
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Seat Number</label>
                    <input type="text" name="SeatNumber" class="form-control passenger-input" value="@ViewBag.SeatNumber" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Class</label>
                    <input type="text" name="Class" class="form-control passenger-input" value="@ViewBag.Class" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Price</label>
                    <input type="text" name="Price" class="form-control passenger-input" value="@ViewBag.Price" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Airplane Number</label>
                    <input type="text" name="AirplaneNumber" class="form-control passenger-input" value="@ViewBag.AirplaneNumber" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">From</label>
                    <input type="text" name="DepartureCity" class="form-control passenger-input" value="@ViewBag.DepartureCity" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">To</label>
                    <input type="text" name="ArrivalCity" class="form-control passenger-input" value="@ViewBag.ArrivalCity" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Departure Date</label>
                    <input type="text" name="DepartureDate" class="form-control passenger-input" value="@ViewBag.DepartureDate" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Departure Time</label>
                    <input type="text" name="DepartureTime" class="form-control passenger-input" value="@ViewBag.DepartureTime" readonly />
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Arrival Time</label>
                    <input type="text" name="ArrivalTime" class="form-control passenger-input" value="@ViewBag.ArrivalTime" readonly />
                </div>
            </div>
        </div>

        <!-- PASSENGER INFO -->
        <div class="passenger-card p-4 mb-4">
            <h5>Passenger Info</h5>
            <div class="row">
                <div class="col-md-3 mb-3">
                    <label class="passenger-label">Title</label>
                    <select name="Passenger.Title" class="form-select passenger-select" required>
                        <option value="">Select</option>
                        <option>Mr</option>
                        <option>Ms</option>
                        <option>Mrs</option>
                    </select>
                </div>
                <div class="col-md-4 mb-3">
                    <label class="passenger-label">First Name</label>
                    <input type="text" name="Passenger.FirstName" class="form-control passenger-input" required />
                </div>
                <div class="col-md-5 mb-3">
                    <label class="passenger-label">Last Name</label>
                    <input type="text" name="Passenger.LastName" class="form-control passenger-input" required />
                </div>
                <div class="col-md-6 mb-3">
                    <label class="passenger-label">Gender</label>
                    <select name="Passenger.Gender" class="form-select passenger-select" required>
                        <option value="">Select</option>
                        <option>Male</option>
                        <option>Female</option>
                    </select>
                </div>
                <div class="col-md-6 mb-3">
                    <label class="passenger-label">Mobile Number</label>
                    <input type="tel" name="Passenger.MobileNumber" pattern="[0-9]{10}" title="Enter 10-digit number" maxlength="10" class="form-control passenger-input" required />
                </div>
                <div class="col-md-6 mb-3">
                    <label class="passenger-label">Date of Birth</label>
                    <input type="date" name="Passenger.DOB" class="form-control passenger-input" required max="2024-12-31" />
                </div>
            </div>
        </div>

        <!-- SUBMIT BUTTONS -->
        <div class="text-center passenger-mb-5">
            <a href="/Booking/SeatSelection?flightId=@ViewBag.FlightId&class=@ViewBag.Class" class="passenger-btn-secondary btn">Back</a>
            <button type="submit" class="passenger-btn-primary btn">Proceed to Payment</button>
        </div>

    </form>
</div>

-------------------------------
-------------------------------PaymentSuccess.cshtml-----------

@{
    ViewBag.Title = "Payment Success";
}

<div class="container text-center mt-5">
    <div class="card shadow-lg p-5 bg-light">
        <h1 class="text-success">✅ Payment Successful!</h1>
        <p class="lead mt-3">Thank you for booking your flight with us.</p>
        <p class="mb-4">A confirmation email has been sent to your registered address.</p>

        <a href="/Home/Index" class="btn btn-primary">Return to Home</a>
    </div>
</div>

<style>
    body {
        background: linear-gradient(to right, #d4fc79, #96e6a1);
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    .card {
        border-radius: 15px;
    }
</style>
----------------------------------------------

---------------------------SeatSelection.cshtml-----------

<h2>Select your Seat - @selectedClass Class</h2>

<div class="seat-grid">
    @for (int i = 0; i < Model.Count; i++)
    {
        var seat = Model[i];
        string seatClass = "seat";
        if (!seat.IsAvailable) { seatClass += " unavailable"; }
        if (seat.IsWindow) { seatClass += " window"; }
        if (seat.IsBookedByUser) { seatClass += " booked-by-user"; }

        int finalPrice = price;
        if (seat.IsWindow) { finalPrice += 100; }

        string tooltip = seat.IsAvailable ? "Seat " + seat.SeatNumber + " - ₹" + finalPrice : "Booked";

        <div class="@seatClass"
             data-seat="@seat.SeatNumber"
             data-price="@finalPrice"
             data-tooltip="@tooltip"
             onclick="selectSeat(this)">
            @seat.SeatNumber
        </div>
    }
</div>

<div class="continue-btn">
    <form action="/Booking/PassengerInfo" method="get">
        <input type="hidden" id="selectedSeat" name="seatNumber" />
        <input type="hidden" name="flightId" value="@flightId" />
        <input type="hidden" name="class" value="@selectedClass" />
        <input type="hidden" name="departureDate" value="@ViewBag.DepartureDate" />
        <button type="submit" class="btn btn-primary mt-3">Continue</button>
    </form>
</div>

<script>
    var selected = null;

    function selectSeat(el) {
        if (el.classList.contains('unavailable')) return;

        if (selected) selected.classList.remove('selected');

        el.classList.add('selected');
        selected = el;

        document.getElementById("selectedSeat").value = el.getAttribute("data-seat");
    }
</script>
------------------------
