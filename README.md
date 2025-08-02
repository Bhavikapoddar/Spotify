..... 
/*
    Seat Selection UI Styling
    This CSS file provides styling for a modern flight seat selection interface.
    It's designed to be clean, responsive, and easy to read.
*/

/* --- General Layout and Container --- */

/* The main container for the seat selection area */
.seat-selection-container {
    width: 100%;
    max-width: 700px; /* Adjust max-width as needed for your design */
    margin: 40px auto;
    padding: 20px;
    background: #f0f4f8; /* Light background for a clean look */
    border-radius: 12px;
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15); /* More prominent shadow */
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #444;
}

h2 {
    text-align: center;
    color: #2c5282; /* A professional, deep blue color */
    margin-bottom: 25px;
    font-size: 1.8rem;
    font-weight: 600;
}

/* --- Legend for Seat Status --- */

.legend {
    display: flex;
    justify-content: center;
    gap: 30px;
    margin-bottom: 30px;
    font-size: 1rem;
    font-weight: 500;
    color: #555;
}

.legend-item {
    display: flex;
    align-items: center;
}

.legend-item .seat-indicator {
    width: 24px;
    height: 24px;
    border-radius: 6px;
    margin-right: 10px;
    border: 1px solid #c3dae8;
}

.seat-indicator.available {
    background-color: #d1e2f3; /* A soft, light blue */
}

.seat-indicator.occupied {
    background-color: #cbd5e0; /* A neutral gray for occupied seats */
}

.seat-indicator.selected {
    background-color: #4c80b5; /* A more vibrant blue for selected */
    border: 1px solid #3c6590;
}

/* --- Aircraft Cabin Layout --- */

.cabin-layout {
    display: flex;
    flex-direction: column;
    gap: 15px; /* Vertical spacing between rows */
    background: #ffffff;
    border: 1px solid #e2e8f0;
    border-radius: 10px;
    padding: 20px;
}

/* Style for each seat row */
.seat-row {
    display: flex;
    align-items: center;
    justify-content: center; /* Center the entire row */
    gap: 10px; /* Spacing between seat groups */
}

.row-label {
    font-weight: bold;
    color: #718096; /* A subtle, readable gray */
    width: 30px; /* Fixed width for alignment */
    text-align: center;
}

/* A group of seats, e.g., A-B-C */
.seat-group {
    display: flex;
    gap: 8px; /* Spacing between individual seats */
}

/* The space for the aisle */
.aisle-space {
    width: 50px; /* Adjust this to control the width of the aisle */
    height: 100%;
}

/* --- Individual Seat Styling --- */

.seat {
    width: 40px;
    height: 40px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.2s ease-in-out, background-color 0.2s ease-in-out;
    cursor: pointer;
    border: 1px solid #a3b8c6;
    font-size: 0.9rem;
    font-weight: bold;
    color: #333;
}

/* Available seats */
.seat.available {
    background-color: #e3f2fd; /* Light blue */
    border-color: #90caf9;
}

/* Hover effect for available seats */
.seat.available:hover {
    transform: scale(1.1);
    box-shadow: 0 4px 10px rgba(76, 175, 80, 0.3);
    background-color: #b3e5fc;
}

/* Occupied seats */
.seat.occupied {
    background-color: #e5e7eb; /* Light gray */
    border-color: #d1d5db;
    cursor: not-allowed;
    pointer-events: none; /* Prevents clicks on occupied seats */
    color: #a0aec0; /* Lighter text color */
}

/* Selected seats */
.seat.selected {
    background-color: #1e3a8a; /* Deep blue, matching the heading */
    border-color: #1a2a47;
    color: #fff; /* White text for contrast */
    transform: scale(1.1);
    box-shadow: 0 4px 10px rgba(30, 58, 138, 0.4);
}

/* Responsive adjustments for smaller screens */
@media (max-width: 600px) {
    .seat-selection-container {
        margin: 20px 10px;
        padding: 15px;
    }

    .seat-row {
        gap: 5px;
    }

    .seat-group {
        gap: 4px;
    }

    .aisle-space {
        width: 30px;
    }

    .seat {
        width: 35px;
        height: 35px;
    }
}
