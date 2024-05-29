```.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Sign Up</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    <style>
        .flash-danger {
            color: red;
        }
        .flash-success {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Sign Up</h1>

    {% with messages = get_flashed_messages(with_categories=true) %}
      {% if messages %}
        <ul class="flashes">
        {% for category, message in messages %}
          <li class="flash-{{ category }}">{{ message }}</li>
        {% endfor %}
        </ul>
      {% endif %}
    {% endwith %}

    <form action="{{ url_for('signup') }}" method="post">
        <p><label for="username">Username:</label> <input type="text" id="username" name="username" required></p>
        <p><label for="email">Email:</label> <input type="email" id="email" name="email" required></p>
        <p><label for="password">Password:</label> <input type="password" id="password" name="password" required></p>
        <p><button type="submit">Sign Up</button></p>
    </form>
    <div class="login-link">
        <p>Already have an account? <a href="{{ url_for('login') }}">Login</a>.</p>
    </div>
</body>
</html>

```
