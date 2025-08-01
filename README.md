hhj
@using (Html.BeginForm("PassengerInfo", "Booking", FormMethod.Post))
{
    @* Use the actual values from ViewBag *@
    <input type="hidden" name="SeatNumber" value="@ViewBag.SeatNumber" />
    <input type="hidden" name="Class" value="@ViewBag.Class" />
    <input type="hidden" name="Price" value="@ViewBag.Price" />
    <input type="hidden" name="AirplaneNumber" value="@ViewBag.AirplaneNumber" />
    <input type="hidden" name="DepartureCity" value="@ViewBag.DepartureCity" />
    <input type="hidden" name="ArrivalCity" value="@ViewBag.ArrivalCity" />
    <input type="hidden" name="DepartureDate" value="@ViewBag.DepartureDate" />
    <input type="hidden" name="DepartureTime" value="@ViewBag.DepartureTime" />
    <input type="hidden" name="ArrivalTime" value="@ViewBag.ArrivalTime" />

    @* Show fields for user to enter passenger personal details *@
    <label>Title</label>
    <input type="text" name="Title" required />

    <label>First Name</label>
    <input type="text" name="FirstName" required />

    <label>Last Name</label>
    <input type="text" name="LastName" required />

    <label>Gender</label>
    <input type="text" name="Gender" required />

    <label>Mobile Number</label>
    <input type="text" name="MobileNumber" required />

    <label>Age</label>
    <input type="number" name="Age" required />

    <button type="submit">Continue</button>
}
