<script>
    // This function will run when the page has finished loading
    window.onload = function () {
        // Create a Date object for today
        const today = new Date();

        // Calculate tomorrow's date
        const tomorrow = new Date(today);
        tomorrow.setDate(today.getDate() + 1);

        // Extract year, month, and day from tomorrow's date
        const yyyy = tomorrow.getFullYear();
        // getMonth() is 0-indexed, so we add 1
        const mm = String(tomorrow.getMonth() + 1).padStart(2, '0');
        const dd = String(tomorrow.getDate()).padStart(2, '0');

        // Format tomorrow's date into 'YYYY-MM-DD'
        const minDate = `${yyyy}-${mm}-${dd}`;

        // Select the departure date input field and set its 'min' attribute
        const departureDateInput = document.querySelector('input[name="DepartureDate"]');
        if (departureDateInput) {
            departureDateInput.setAttribute('min', minDate);
        }
    };

    // This function can be called on form submission to perform a final validation
    function validateDate() {
        // Get the selected departure date from the input field
        const departureDateInput = document.querySelector('input[name="DepartureDate"]');
        if (!departureDateInput) {
            return true; // No input field, no validation needed
        }

        const selectedDate = new Date(departureDateInput.value);

        // Get tomorrow's date for comparison
        const today = new Date();
        const tomorrow = new Date(today);
        tomorrow.setDate(today.getDate() + 1);

        // Normalize both dates to midnight (00:00:00) for accurate comparison
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
