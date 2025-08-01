hii
.... 
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;
using Airline_Managementnew.Models;

namespace Airline_Managementnew.Controllers
{
    public class UserController : Controller
    {
        DatabaseContext db = new DatabaseContext();

        // GET: Register
        public ActionResult Register()
        {
            return View();
        }

        // POST: Register
        [HttpPost]
        public ActionResult Register(User user)
        {
            if (ModelState.IsValid)
            {
                db.USERS.Add(user);
                db.SaveChanges();
                ViewBag.Message = "Registration Successfully!";
                return RedirectToAction("Login");
            }
            return View();
        }

        // GET: Login
        public ActionResult Login()
        {
            // Clear any existing session data on page load to prevent issues
            Session.Clear();
            return View();
        }

        // POST: Login
        [HttpPost]
        public ActionResult Login(string Username, string Password)
        {
            var admin = db.Admins.FirstOrDefault(a => a.Username == Username && a.Password == Password);
            if (admin != null)
            {
                Session["AdminId"] = admin.Id;
                Session["Username"] = admin.Username;
                return RedirectToAction("AdminPanel", "Admin");
            }

            var user = db.USERS.FirstOrDefault(u => u.Username == Username && u.Password == Password);
            if (user != null)
            {
                Session["UserId"] = user.Id;
                Session["Username"] = user.Username;
                return RedirectToAction("Index", "Booking");
            }

            ViewBag.Error = "Invalid Username or Password!";
            return View();
        }

        // GET: Logout
        public ActionResult Logout()
        {
            Session.Clear();
            return RedirectToAction("Login", "User");
        }
    }
}
--_-----------
-----
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>

                @* Add this conditional logic to replace the _loginPartial *@
                <ul class="nav navbar-nav navbar-right">
                    @if (Session["Username"] != null)
                    {
                        <li><p class="navbar-text">Welcome, @Session["Username"]</p></li>
                        <li>@Html.ActionLink("Logout", "Logout", "User")</li>
                    }
                    else
                    {
                        <li>@Html.ActionLink("Login", "Login", "User")</li>
                        <li>@Html.ActionLink("Register", "Register", "User")</li>
                    }
                </ul>
            </div>
        </div>
    </div>
    <div class="container body-content">
        @RenderBody()
        <hr />
        <footer>
            <p>&copy; @DateTime.Now.Year - My ASP.NET Application</p>
        </footer>
    </div>
    @Scripts.Render("~/bundles/jquery")
    @Scripts.Render("~/bundles/bootstrap")
    @RenderSection("scripts", required: false)
</body>
</html>
.......... 
... 
