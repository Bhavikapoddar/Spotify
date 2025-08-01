
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Login & Register</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', sans-serif;
    }

    body {
      background: linear-gradient(to right, #4facfe, #00f2fe);
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .container {
      background: white;
      width: 400px;
      padding: 40px 30px;
      border-radius: 15px;
      box-shadow: 0 15px 25px rgba(0, 0, 0, 0.2);
      position: relative;
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
      color: #333;
    }

    .form-group {
      margin-bottom: 15px;
    }

    label {
      font-weight: 600;
      display: block;
      margin-bottom: 5px;
    }

    input {
      width: 100%;
      padding: 10px;
      border: 1px solid #aaa;
      border-radius: 5px;
      outline: none;
    }

    button {
      width: 100%;
      padding: 12px;
      background-color: #4facfe;
      border: none;
      color: white;
      font-weight: bold;
      border-radius: 5px;
      margin-top: 10px;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    button:hover {
      background-color: #00c6ff;
    }

    .toggle {
      text-align: center;
      margin-top: 15px;
    }

    .toggle a {
      color: #4facfe;
      cursor: pointer;
      text-decoration: none;
      font-weight: bold;
    }

    .form-box {
      display: none;
      animation: fadeIn 0.5s ease-in-out;
    }

    .form-box.active {
      display: block;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }
  </style>
</head>
<body>

  <div class="container">
    <!-- Login Form -->
    <div class="form-box active" id="login-form">
      <h2>Login</h2>
      <div class="form-group">
        <label>Email</label>
        <input type="email" placeholder="Enter your email" />
      </div>
      <div class="form-group">
        <label>Password</label>
        <input type="password" placeholder="Enter your password" />
      </div>
      <button>Login</button>
      <div class="toggle">Don't have an account? <a id="show-register">Register</a></div>
    </div>

    <!-- Register Form -->
    <div class="form-box" id="register-form">
      <h2>Register</h2>
      <div class="form-group">
        <label>Name</label>
        <input type="text" placeholder="Enter your name" />
      </div>
      <div class="form-group">
        <label>Email</label>
        <input type="email" placeholder="Enter your email" />
      </div>
      <div class="form-group">
        <label>Password</label>
        <input type="password" placeholder="Create a password" />
      </div>
      <button>Register</button>
      <div class="toggle">Already have an account? <a id="show-login">Login</a></div>
    </div>
  </div>

  <script>
    $(document).ready(function () {
      $("#show-register").click(function () {
        $("#login-form").removeClass("active");
        $("#register-form").addClass("active");
      });

      $("#show-login").click(function () {
        $("#register-form").removeClass("active");
        $("#login-form").addClass("active");
      });
    });
  </script>

</body>
</html>
