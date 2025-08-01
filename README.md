@model Airline_Managementnew.Models.Passenger

@{
    ViewBag.Title = "Passenger Info";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2 class="text-center passenger-title mb-4">Passenger Details</h2>

<div class="container passenger-container mt-4">
    @using (Html.BeginForm("PassengerInfo", "Booking", FormMethod.Post))
    {
        <div class="passenger-card p-4 mb-4">
            <h5>Flight Details</h5>
            <div class="row">

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Seat Number</label>
                    @Html.TextBoxFor(m => m.SeatNumber, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["SeatNumber"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Class</label>
                    @Html.TextBoxFor(m => m.Class, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["Class"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Price</label>
                    @Html.TextBoxFor(m => m.Price, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["Price"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Airplane Number</label>
                    @Html.TextBoxFor(m => m.AirplaneNumber, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["AirplaneNumber"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">From</label>
                    @Html.TextBoxFor(m => m.DepartureCity, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["From"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">To</label>
                    @Html.TextBoxFor(m => m.ArrivalCity, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["To"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Departure Date</label>
                    @Html.TextBoxFor(m => m.DepartureDate, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["DepartureDate"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Departure Time</label>
                    @Html.TextBoxFor(m => m.DepartureTime, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["DepartureTime"] })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">Arrival Time</label>
                    @Html.TextBoxFor(m => m.ArrivalTime, new { @class = "form-control passenger-input", @readonly = "readonly", @Value = TempData["ArrivalTime"] })
                </div>
            </div>
        </div>

        <!-- Passenger Info -->
        <div class="passenger-card p-4 mb-4">
            <h5>Passenger Info</h5>
            <div class="row">
                <div class="col-md-3 mb-3">
                    <label class="passenger-label">Title</label>
                    @Html.DropDownListFor(m => m.Title, new SelectList(new[] { "Mr", "Ms", "Mrs" }), "Select", new { @class = "form-select passenger-select", required = "required" })
                </div>

                <div class="col-md-4 mb-3">
                    <label class="passenger-label">First Name</label>
                    @Html.TextBoxFor(m => m.FirstName, new { @class = "form-control passenger-input", required = "required" })
                </div>

                <div class="col-md-5 mb-3">
                    <label class="passenger-label">Last Name</label>
                    @Html.TextBoxFor(m => m.LastName, new { @class = "form-control passenger-input", required = "required" })
                </div>

                <div class="col-md-6 mb-3">
                    <label class="passenger-label">Gender</label>
                    @Html.DropDownListFor(m => m.Gender, new SelectList(new[] { "Male", "Female" }), "Select", new { @class = "form-select passenger-select", required = "required" })
                </div>

                <div class="col-md-6 mb-3">
                    <label class="passenger-label">Mobile Number</label>
                    @Html.TextBoxFor(m => m.MobileNumber, new { @class = "form-control passenger-input", required = "required" })
                </div>

                <div class="col-md-6 mb-3">
                    <label class="passenger-label">Age</label>
                    @Html.TextBoxFor(m => m.Age, new { @class = "form-control passenger-input", required = "required" })
                </div>
            </div>
        </div>

        <div class="text-center passenger-mb-5">
            <a href="/Booking/SeatSelection?flightId=@ViewBag.FlightId&class=@ViewBag.Class" class="btn btn-secondary">Back to Seat Selection</a>
            <button type="submit" class="passenger-btn-primary btn">Proceed to Payment</button>
        </div>
    }
</div>
