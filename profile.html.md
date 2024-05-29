```.py
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Profile</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='profile.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='sidebar.css') }}">
    <style>
        .flash-success {
            color: green;
        }
        .flash-danger {
            color: red;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.4);
            transition: background-color 0.3s ease-in-out;
        }

        .modal-content {
            background-color: #fff;
            margin: 5% auto;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            width: 60%;
            max-width: 500px;
            animation: slide-down 0.5s ease-out;
        }

        @keyframes slide-down {
            from {
                transform: translateY(-200px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }

        .close:hover,
        .close:focus {
            color: #000;
            text-decoration: none;
        }

        .modal-content label {
            display: block;
            margin: 15px 0 5px;
            font-weight: bold;
        }

        .modal-content input[type="text"],
        .modal-content textarea {
            width: calc(100% - 22px);
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 16px;
            box-sizing: border-box;
        }

        .modal-content button {
            display: block;
            width: 100%;
            padding: 10px;
            background-color: #007BFF;
            color: #fff;
            border: none;
            border-radius: 4px;
            font-size: 18px;
            cursor: pointer;
        }

        .modal-content button:hover {
            background-color: #0056b3;
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
    <div class="main-content">
        <h1>Profile Information</h1>
        {% with messages = get_flashed_messages(with_categories=true) %}
          {% if messages %}
            <ul class="flashes">
            {% for category, message in messages %}
              <li class="flash-{{ category }}">{{ message }}</li>
            {% endfor %}
            </ul>
          {% endif %}
        {% endwith %}
        <div style="text-align: center;">
            {% if user.profile_pic %}
            <img src="{{ url_for('static', filename='/uploads/' + user.profile_pic) }}?{{ cache_bust }}" class="profile-pic">
            {% else %}
                <img src="{{ url_for('static', filename='default_profile.jpg') }}" alt="Profile Picture" class="profile-pic">
            {% endif %}
        </div>
        <div class="user-info">
            <div>
                <span class="bold">Username:</span><span class="user-info"> {{ user.username }} </span>
            </div>
            <div>
                <span class="bold">Email:</span> <span class="user-info"> {{ user.email }} </span>
            </div>
            <div>
                <span class="bold">BIO:</span> <span class="user-info"> {{ user.bio }} </span>
            </div>
        </div>

        <form action="{{ url_for('profile') }}" method="POST" enctype="multipart/form-data">
            <div class="profile-pic-upload-section">
                <label class="bold" for="profile_pic">Upload Profile Picture:</label>
                <input type="file" name="profile_pic" id="profile_pic" accept="image/*">
                <input type="hidden" name="date_posted" value="{{ date_posted }}">
                <button type="submit" name="action" value="save_pic">Save Profile Picture</button>
            </div>
            <div class="bio-section">
                <div class="bold">   
                    <label for="bio">Add Bio:</label>
                </div>
                <span>
                    <textarea name="bio" rows="5" cols="40">{{ user.bio }}</textarea><br>

                </span>
                
                <button type="submit" name="action" value="save_bio">Save Bio</button>
            </div>
        </form>
        <h2>Followed Users:</h2>
        <ul>
            {% for followed_user in user.followed %}
                <li>
                    {{ followed_user.username }}
                    <button type="button" class="send-email-btn" data-user-id="{{ followed_user.id }}" data-user-email="{{ followed_user.email }}" style="float: right;">Send Email</button>
                </li>
            {% endfor %}
        </ul>
    </div>

    <div id="emailModal" class="modal">
        <div class="modal-content">
            <span class="close">&times;</span>
            <form id="emailForm" action="{{ url_for('send_email') }}" method="POST">
                <input type="hidden" name="user_id" id="user_id">
                <div>
                    <label for="recipient">To:</label>
                    <input type="text" id="recipient" name="recipient" readonly>
                </div>
                <div>
                    <label for="subject">Subject:</label>
                    <input type="text" id="subject" name="subject" required>
                </div>
                <div>
                    <label for="cc">CC:</label>
                    <input type="text" id="cc" name="cc">
                </div>
                <div>
                    <label for="description">Description:</label>
                    <textarea id="description" name="description" rows="5" required></textarea>
                </div>
                <button type="submit">Send Email</button>
            </form>
        </div>
    </div>

    <script>
        // Get the modal
        var modal = document.getElementById("emailModal");

        // Get the button that opens the modal
        var btns = document.getElementsByClassName("send-email-btn");

        // Get the <span> element that closes the modal
        var span = document.getElementsByClassName("close")[0];

        // When the user clicks the button, open the modal
        for (var btn of btns) {
            btn.onclick = function() {
                modal.style.display = "block";
                document.getElementById("user_id").value = this.getAttribute("data-user-id");
                document.getElementById("recipient").value = this.getAttribute("data-user-email");
            }
        }

        // When the user clicks on <span> (x), close the modal
        span.onclick = function() {
            modal.style.display = "none";
        }

        // When the user clicks anywhere outside of the modal, close it
        window.onclick = function(event) {
            if (event.target == modal) {
                modal.style.display = "none";
            }
        }
    </script>
</body>
</html>


```
