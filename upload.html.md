```.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Upload Post</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='upload.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='sidebar.css') }}">
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
    <div class="sidebar">
        <ul>
            <li><a href="{{ url_for('dashboard') }}">Feed</a></li>
            <li><a href="{{ url_for('upload') }}">Upload Post</a></li>
            <li><a href="{{ url_for('profile') }}">Profile</a></li>
            <li><a href="{{ url_for('follow_users') }}">Followed Users</a></li>
            <li><a href="{{ url_for('logout') }}">Logout</a></li>
        </ul>
    </div>
    <main>
        <h1>Upload Post</h1>
        {% with messages = get_flashed_messages(with_categories=true) %}
          {% if messages %}
            <ul class="flashes">
            {% for category, message in messages %}
              <li class="flash-{{ category }}">{{ message }}</li>
            {% endfor %}
            </ul>
          {% endif %}
        {% endwith %}
        <form action="{{ url_for('upload') }}" method="post" enctype="multipart/form-data">
            <p><label for="title">Post Title:</label> <input type="text" id="title" name="title" required></p>
            <p><label for="content">Post Content:</label> <textarea id="content" name="content" required></textarea></p>
            <p><label for="image">Post Image:</label> <input type="file" id="image" name="image" required></p>
            <p><button type="submit">Upload</button></p>
        </form>
    </main>
</body>
</html>

```
