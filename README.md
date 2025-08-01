hhh
return RedirectToAction("SeatSelection", new
        {
            flightId = passenger.FlightId,      // Make sure this exists in your Passenger model
            seatClass = passenger.SeatClass,    // Also exists
            price = passenger.Price,            // Same here
            departureTime = passenger.DepartureTime // Make sure this field exists
        });
