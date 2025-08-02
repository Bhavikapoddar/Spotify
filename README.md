<script>
    // This is the function that runs when a seat is clicked
    function selectSeat(element) {
        // --- 1. Find the hidden input and the continue button ---
        var selectedSeatInput = document.getElementById("selectedSeat");
        var continueBtn = document.getElementById("continueBtn");

        // --- 2. Deselect all seats first ---
        var allSeats = document.getElementsByClassName("seat");
        for (var i = 0; i < allSeats.length; i++) {
            allSeats[i].classList.remove("selected");
        }

        // --- 3. Select the clicked seat ---
        // This is a toggle: if the clicked seat was already selected, it will be unselected
        // We'll add the selected class to the element that was clicked
        element.classList.add("selected");

        // --- 4. Update the hidden input field and show the button ---
        var seatNumber = element.getAttribute("data-seatNumber");
        selectedSeatInput.value = seatNumber;
        
        // Show and enable the button
        continueBtn.style.display = "inline-block";
        continueBtn.disabled = false;
    }
</script>
