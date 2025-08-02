@model AirlineManagement.Models.SearchRequest

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Airline Management System</title>
    <link href="~/content/css/style.css" rel="stylesheet" />
</head>
<body>
    <div class="header">
        <h3 style="margin-left:20px;"> JetSetFly</h3>
    </div>
    <div style="text-align:center; color:white">
        <h1>Create Ever-Lasting Memories With Us</h1>
    </div>
    <div class="search-box">
        <form action="/Booking/SearchFlights" method="post" onsubmit="return validateDate();">
            <div class="form-group row">
                <div class="col-md-4">
                    <label>From</label>
                    <select name="FromLocation" class="form-control" required>
                        <option value="">---Select From---</option>
                        @foreach (var item in Model.Select(m => m.FromLocation).Distinct())
                        {
                            <option value="@item">@item</option>
                        }
                    </select>
                </div>
                <div class="col-md-4">
                    <label>To</label>
                    <select name="ToLocation" class="form-control" required>
                        <option value="">---Select To---</option>
                        @foreach (var item in Model.Select(m => m.ToLocation).Distinct())
                        {
                            <option value="@item">@item</option>
                        }
                    </select>
                </div>
            </div>
            
            <div class="form-group row">
                <div class="col-md-4">
                    <label>Departure Date</label>
                    <input name="DepartureDate" type="date" class="form-control" required>
                </div>
                <div class="col-md-4">
                    <button type="submit" class="btn btn-success">Search Flights</button>
                </div>
            </div>
        </form>
    </div>

    <script>
        // This function will set the minimum date to tomorrow
        window.onload = function () {
            // Get tomorrow's date
            const today = new Date();
            const tomorrow = new Date(today);
            tomorrow.setDate(today.getDate() + 1);

            // Format tomorrow's date as YYYY-MM-DD
            const yyyy = tomorrow.getFullYear();
            const mm = String(tomorrow.getMonth() + 1).padStart(2, '0');
            const dd = String(tomorrow.getDate()).padStart(2, '0');
            const minDate = `${yyyy}-${mm}-${dd}`;

            // Select the input field and set the 'min' attribute
            const departureDateInput = document.querySelector('input[name="DepartureDate"]');
            if (departureDateInput) {
                departureDateInput.setAttribute('min', minDate);
            }
        }; // <-- The closing brace for window.onload is now here

        // This function will perform the final validation on form submission
        function validateDate() {
            const departureDateInput = document.querySelector('input[name="DepartureDate"]');
            if (!departureDateInput) {
                return true; // No input field, no validation needed
            }

            const selectedDate = new Date(departureDateInput.value);

            // Get tomorrow's date for comparison
            const today = new Date();
            const tomorrow = new Date(today);
            tomorrow.setDate(today.getDate() + 1);

            // Normalize all dates to midnight for accurate comparison
            selectedDate.setHours(0, 0, 0, 0);
            tomorrow.setHours(0, 0, 0, 0);

            // Check if the selected date is before tomorrow's date
            if (selectedDate < tomorrow) {
                alert("Past dates or today's date are not allowed. Please select a date from tomorrow onwards.");
                return false;
            }

            return true;
        }
    </script>
</body>
</html>
