jhhu
[HttpPost]
public ActionResult PassengerInfo(Passenger passenger)
{
    if (ModelState.IsValid)
    {
        using (DatabaseContext db = new DatabaseContext())
        {
            db.Passengers.Add(passenger);
            db.SaveChanges();
        }

        return RedirectToAction("SuccessPage"); // or redirect to payment
    }

    return View(passenger); // show errors
}
........... 
public ActionResult PassengerInfo()
{
    using (DatabaseContext db = new DatabaseContext())
    {
        var passengers = db.Passengers.ToList();
        return View(passengers);
    }
}
........ 
