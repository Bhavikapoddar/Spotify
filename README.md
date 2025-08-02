window.onload = function () {
    // Create a Date object for today
    const today = new Date();

    // Calculate tomorrow's date
    const tomorrow = new Date(today);
    tomorrow.setDate(today.getDate() + 1);

    // Extract year, month, and day from tomorrow's date
    const yyyy = tomorrow.getFullYear();
    // getMonth() is 0-indexed, so we add 1
    const month = tomorrow.getMonth() + 1;
    const day = tomorrow.getDate();

    // Add leading zero if month/day is less than 10
    const mm = (month < 10) ? '0' + month : month;
    const dd = (day < 10) ? '0' + day : day;

    // Format tomorrow's date into 'YYYY-MM-DD'
    const minDate = yyyy + '-' + mm + '-' + dd;

    // Select the departure date input field and set its 'min' attribute
    const departureDateInput = document.querySelector('input[name="DepartureDate"]');
    if (departureDateInput) {
        departureDateInput.setAttribute('min', minDate);
    }
};
