```.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
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
    <h1>Login</h1>

    {% with messages = get_flashed_messages(with_categories=true) %}
      {% if messages %}
        <ul class="flashes">
        {% for category, message in messages %}
          <li class="flash-{{ category }}">{{ message }}</li>
        {% endfor %}
        </ul>
      {% endif %}
    {% endwith %}

    <form action="{{ url_for('login') }}" method="post">
        <p><label for="username">Username:</label> <input type="text" id="username" name="username" required></p>
        <p><label for="password">Password:</label> <input type="password" id="password" name="password" required></p>
        <p><button type="submit">Login</button></p>
    </form>
    <div class="signup-link">
        <p>Don't have an account? <a href="{{ url_for('signup') }}">Create one</a>.</p>
    </div>
</body>
</html>

```
