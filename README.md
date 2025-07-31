<form method="post" action="@Url.Action("Search", "Booking")">
    <div>
        <label>From:</label>
        <input type="text" name="From" required />
    </div>
    <div>
        <label>To:</label>
        <input type="text" name="To" required />
    </div>
    <div>
        <label>Departure Date:</label>
        <input type="date" name="DepartureDate" required />
    </div>
    <button type="submit">Search</button>
</form>
