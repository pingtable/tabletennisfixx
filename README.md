<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Table Tennis Predictions - Lithuania</title>
  <style>
    /* Reset margins and paddings */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    /* Styles for body */
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      color: #333;
    }

    /* Main container */
    .container {
      max-width: 600px;
      width: 100%;
      padding: 20px;
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    /* Heading */
    h1 {
      margin-bottom: 20px;
      text-align: center;
      color: #4CAF50;
    }

    /* Form */
    .form {
      margin-bottom: 20px;
    }

    .form h2 {
      margin-bottom: 10px;
      color: #555;
    }

    .form input[type="text"],
    .form input[type="password"],
    .form button {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 16px;
    }

    .form button {
      background-color: #4CAF50;
      color: white;
      cursor: pointer;
    }

    .form button:hover {
      background-color: #45a049;
    }

    /* Ping Pong Image */
    .ping-pong-image {
      margin-top: 20px;
      max-width: 100%;
      height: auto;
      display: block;
      margin-left: auto;
      margin-right: auto;
    }

    /* Predictions List */
    .predictions-list {
      margin-top: 20px;
      text-align: left;
    }

    .predictions-list div {
      background-color: #f9f9f9;
      border: 1px solid #ddd;
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 4px;
      font-size: 16px;
    }

    .predictions-list div:last-child {
      margin-bottom: 0;
    }

    /* Footer */
    footer {
      margin-top: 20px;
      text-align: center;
      color: #888;
    }

    /* Warning in red */
    .warning {
      color: red;
      font-weight: bold;
      text-align: center;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Table Tennis Predictions - Lithuania</h1>

    <div class="warning">
      WARNING: Do not communicate with others to avoid legal risks.
    </div>

    <div id="register-form" class="form" style="display: none;">
      <h2>Registration</h2>
      <input type="text" id="register-username" placeholder="Username">
      <input type="password" id="register-password" placeholder="Password">
      <button onclick="register()">Register</button>
      <p><a href="#" onclick="showLoginForm()">Already have an account? Login here.</a></p>
    </div>

    <div id="login-form" class="form">
      <h2>Administrator Login</h2>
      <input type="text" id="admin-username" placeholder="Username" value="admin">
      <input type="password" id="admin-password" placeholder="Password" value="gico1237">
      <button onclick="login()">Login</button>
      <p><a href="#" onclick="showRegisterForm()">Don't have an account? Register here.</a></p>
    </div>

    <div id="predictions-form" class="form" style="display: none;">
      <h2>Add Prediction</h2>
      <input type="text" id="match" placeholder="Match">
      <input type="text" id="prediction" placeholder="Prediction">
      <button onclick="addPrediction()">Add Prediction</button>
      <button onclick="logout()">Logout</button>
    </div>

    <div id="predictions-list" class="predictions-list"></div>
    
    <img src="ping-pong.jpg" alt="Ping Pong Image" class="ping-pong-image">

    <footer>
      <p>Website for table tennis predictions - Lithuania</p>
      <p>Current time: <span id="current-time"></span></p>
    </footer>
  </div>

  <script>
    document.addEventListener('DOMContentLoaded', () => {
      clearPredictions(); // Clear predictions on page load
      const adminLoggedIn = localStorage.getItem('adminLoggedIn');
      if (adminLoggedIn === 'true') {
        showPredictionsForm();
      }
      updateTime();
      setInterval(updateTime, 1000); // Update time every second
    });

    function clearPredictions() {
      localStorage.removeItem('predictions');
    }

    function login() {
      const username = document.getElementById('admin-username').value;
      const password = document.getElementById('admin-password').value;

      // Check credentials
      if (username === 'admin' && password === 'gico1237') {
        localStorage.setItem('adminLoggedIn', 'true');
        showPredictionsForm();
      } else {
        alert('Invalid username or password');
      }
    }

    function logout() {
      localStorage.removeItem('adminLoggedIn');
      localStorage.removeItem('predictions'); // Remove saved predictions
      showLoginForm();
      document.getElementById('predictions-list').innerHTML = ''; // Clear predictions list
    }

    function showLoginForm() {
      document.getElementById('login-form').style.display = 'block';
      document.getElementById('predictions-form').style.display = 'none';
      document.getElementById('register-form').style.display = 'none';
    }

    function showPredictionsForm() {
      document.getElementById('login-form').style.display = 'none';
      document.getElementById('predictions-form').style.display = 'block';
      document.getElementById('register-form').style.display = 'none';
      fetchPredictions();
    }

    function showRegisterForm() {
      document.getElementById('login-form').style.display = 'none';
      document.getElementById('predictions-form').style.display = 'none';
      document.getElementById('register-form').style.display = 'block';
    }

    function register() {
      const username = document.getElementById('register-username').value;
      const password = document.getElementById('register-password').value;

      // Implement registration logic here (save to localStorage or backend)
      alert(`Registration completed for ${username}`);
      document.getElementById('register-username').value = '';
      document.getElementById('register-password').value = '';
    }

    function addPrediction() {
      const match = document.getElementById('match').value;
      const prediction = document.getElementById('prediction').value;

      if (match && prediction) {
        const predictions = JSON.parse(localStorage.getItem('predictions')) || [];
        predictions.push({ match, prediction });
        localStorage.setItem('predictions', JSON.stringify(predictions));
        fetchPredictions();
        document.getElementById('match').value = '';
        document.getElementById('prediction').value = '';
      } else {
        alert('Please fill in both fields');
      }
    }

    function fetchPredictions() {
      const predictionsList = document.getElementById('predictions-list');
      predictionsList.innerHTML = '';

      const predictions = JSON.parse(localStorage.getItem('predictions')) || [];
      predictions.forEach(prediction => {
        const div = document.createElement('div');
        div.textContent = `Match: ${prediction.match}, Prediction: ${prediction.prediction}`;
        predictionsList.appendChild(div);
      });
    }

    function updateTime() {
      const now = new Date();
      const hours = now.getHours().toString().padStart(2, '0');
      const minutes = now.getMinutes().toString().padStart(2, '0');
      const seconds = now.getSeconds().toString().padStart(2, '0');
      document.getElementById('current-time').textContent = `${hours}:${minutes}:${seconds}`;
    }
  </script>

</body>
</html>
