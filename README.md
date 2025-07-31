h
// BookingController.cs (UPDATED)
using Airline.Models;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web.Mvc;

public class BookingController : Controller
{
    AirlineDbContext db = new AirlineDbContext(); // Assuming this is your EF context

    [HttpPost]
    public ActionResult SearchFlights(SearchRequest request)
    {
        var results = db.Flight.Where(f =>
            f.DepartureCity == request.From &&
            f.ArrivalCity == request.To &&
            f.DepartureTime.Date == request.DepartureDate.Date).ToList();

        ViewBag.From = request.From;
        ViewBag.To = request.To;
        ViewBag.DepartureDate = request.DepartureDate.ToString("dd MMM yyyy");

        return View("SearchResult", results);
    }

    public ActionResult PassengerInfo(string flightId, string travelClass, string departureDate)
    {
        ViewBag.FlightId = flightId;
        ViewBag.Class = travelClass;
        ViewBag.DepartureDate = departureDate;

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

        return View(seats);
    }

    [HttpPost]
    public ActionResult ConfirmBooking(int flightId, string seatNumber, string @class)
    {
        var flight = db.Flight.FirstOrDefault(f => f.Id == flightId);
        if (flight == null)
            return HttpNotFound();

        int price = 0;
        switch (@class)
        {
            case "Economy":
                price = flight.EconomyPrice;
                break;
            case "BusinessClass":
                price = flight.BusinessPrice;
                break;
            case "FirstClass":
                price = flight.FirstClassPrice ?? 0;
                break;
        }

        if (int.TryParse(seatNumber, out int seatNum))
        {
            if (seatNum % 6 == 1 || seatNum % 6 == 0)
            {
                price += 100;
            }
        }

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
        return View("PaymentSuccess");
    }
}
