```.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Followed Users</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='followed_users.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='sidebar.css') }}">
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        $(document).ready(function() {
            function toggleFollowButton(button, action) {
                if (action === 'follow') {
                    var userItem = button.closest('li');
                    button.remove();
                    userItem.append('<button class="unfollow-button" data-user-id="' + button.data('user-id') + '" data-action="unfollow">Unfollow</button>');
                    $('#followed-users').append(userItem);
                } else if (action === 'unfollow') {
                    var userItem = button.closest('li');
                    button.remove();
                    userItem.append('<button class="follow-button" data-user-id="' + button.data('user-id') + '" data-action="follow">Follow</button>');
                    $('#unfollowed-users').append(userItem);
                }
            }

            $(document).on('click', '.follow-button, .unfollow-button', function() {
                var button = $(this);
                var userId = button.data('user-id');
                var action = button.data('action');

                $.ajax({
                    url: '{{ url_for("follow_unfollow_user") }}',
                    method: 'POST',
                    data: {
                        user_id: userId,
                        action: action,
                        csrf_token: '{{ session.csrf_token }}'
                    },
                    success: function(response) {
                        toggleFollowButton(button, action);
                    },
                    error: function(xhr, status, error) {
                        console.error(error);
                    }
                });
            });
        });
    </script>
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
    <div class="main-content">
        <h1>Followed Users</h1>
        <h2>Unfollowed Users:</h2>
        <ul id="unfollowed-users">
            {% for user in unfollowed_users %}
                <li>
                    <a href="{{ url_for('view_profile', user_id=user.id) }}">{{ user.username }}</a>
                    <button class="follow-button" data-user-id="{{ user.id }}" data-action="follow">Follow</button>
                </li>
            {% endfor %}
        </ul>
        <h2>Followed Users:</h2>
        <ul id="followed-users">
            {% for followed_user in followed_users %}
                <li>
                    <a href="{{ url_for('view_profile', user_id=followed_user.id) }}">{{ followed_user.username }}</a>
                    <button class="unfollow-button" data-user-id="{{ followed_user.id }}" data-action="unfollow">Unfollow</button>
                </li>
            {% endfor %}
        </ul>
    </div>
</body>
</html>


```
