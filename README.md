# Background

G11 CS Project Unit 4 required us to attempt a recreation of an online board system, for example, Reddit. To be assessed, we will be evaluated using Criteria C, D, and E using an MVP.

My choice was to develop a combination of Instagram and Reddit.

# Criteria C: Development

## Success Criteria
Create a Flask Web application for an online bulletin board system (like reddit). You are free to set the general theme, groups, visuals (CSS), topics, etc, but you need:

1. A login/registration system.

2. A posting system to EDIT/CREATE/DELETE comments.

3. A system to add/remove likes.

4. A system to follow/unfollow users, follow/unfollow topics or groups.

5. A profile page with relevant information

6. [HLs] upload images

7. [HL++] send emails

### Techniques Used

1. **Flask Library**:
   - The code uses Flask to create a web application. Flask is a micro web framework for Python.
   - Routes are defined using Flask decorators like `@app.route`.

2. **Functions**:
   - Functions are used extensively for defining routes and handling logic. Example: `login_required`, `allowed_file`, and all route functions (`login`, `signup`, etc.).

3. **If Statements**:
   - Conditional logic is used throughout the code to handle various conditions. Example: Checking if the user is logged in, if a user exists, or if a file is allowed.

4. **Variables**:
   - Variables are used to store data temporarily within functions and to hold configuration settings like `ALLOWED_EXTENSIONS`.

5. **HTML Template Usage**:
   - Flask's `render_template` function is used to render HTML templates. This allows for separation of HTML content from Python code.

6. **Get/Post Methods**:
   - The code handles different HTTP methods (`GET` and `POST`) for various routes to manage form submissions and data retrieval.

7. **Session Management**:
   - Flask's `session` object is used to manage user sessions, storing the `user_id` to keep track of logged-in users.

8. **Database Interaction**:
   - SQLAlchemy is used for ORM (Object Relational Mapping) to interact with the database. Example: querying users, posts, comments, and likes.
   - Example models are `User`, `Post`, `Comment`, and `Like`.

9. **Encryption**:
   - The code uses `generate_password_hash` and `check_password_hash` from `werkzeug.security` for password hashing and verification.

10. **File Handling**:
    - Functions like `allowed_file` and the usage of `secure_filename` handle file uploads securely.

11. **Flash Messages**:
    - Flask's `flash` function is used to display messages to the user.

12. **JSON**:
    - JSON responses are used for AJAX requests, such as in the `follow_unfollow_user` and `add_comment` routes.

13. **Decorators**:
    - The `login_required` decorator is a custom decorator to ensure routes are accessible only to logged-in users.

14. **Date and Time Handling**:
    - The `datetime` module is used to handle date and time, e.g., when a post or comment is created.

15. **Object-Oriented Programming (OOP)**:
   - The `User`, `Post`, `Like`, and `Comment` classes are defined as models using SQLAlchemy, an ORM library. These classes define the structure and behavior of the data in the database.

16. **Class Relationships and Foreign Keys**:
   - Relationships between models are established using `db.relationship` and `db.ForeignKey`.
   - Many-to-many relationship: The `User` model has a self-referential many-to-many relationship through the `followers` table.
   - One-to-many relationship: The `Post` model has a one-to-many relationship with the `Comment` and `Like` models.
   - The `Comment` and `Like` models use foreign keys to reference the `User` and `Post` models.

17. **Table Relationships**:
   - The `followers` association table is defined to handle the many-to-many relationship between users.
   - Backrefs (`db.backref`) are used to create a bidirectional relationship, allowing easy access to related objects from either side of the relationship.

18. **Default Values and Constraints**:
   - Columns have default values (e.g., `date_posted` with `datetime.utcnow`).
   - Constraints like `unique=True` ensure unique values for columns such as `username` and `email`.

19. **Primary and Unique Constraints**:
   - Primary keys are defined using `primary_key=True`.
   - Unique constraints are defined using `db.UniqueConstraint` for composite keys, ensuring a user cannot like the same post multiple times.

20. **Custom Methods**:
   - The `like_count` method in the `Post` model calculates the number of likes a post has, demonstrating how custom methods can be added to models for additional functionality.

## Existing tools

1. **Flask**:
   - **Description**: A micro web framework for Python based on Werkzeug and Jinja2.
   - **Usage**: Provides the core framework for creating web applications, handling routing, sessions, and request/response cycles.
   - **Website**: https://flask.palletsprojects.com/

2. **SQLAlchemy**:
   - **Description**: An SQL toolkit and Object-Relational Mapping (ORM) library for Python.
   - **Usage**: Used to define the database schema, query the database, and manage relationships between tables.
   - **Website**: https://www.sqlalchemy.org/

3. **Jinja2**:
   - **Description**: A templating engine for Python.
   - **Usage**: Used to render HTML templates with dynamic data in the Flask application.
   - **Website**: https://jinja.palletsprojects.com/

4. **Werkzeug**:
   - **Description**: A comprehensive WSGI web application library.
   - **Usage**: Provides utilities for request and response handling, URL routing, and security features.
   - **Website**: https://werkzeug.palletsprojects.com/

5. **WTForms**:
   - **Description**: A flexible forms validation and rendering library for Python web development.
   - **Usage**: Handles form data validation and rendering in Flask applications.
   - **Website**: https://wtforms.readthedocs.io/

6. **Flask-Login**:
   - **Description**: A user session management extension for Flask.
   - **Usage**: Manages user authentication, login sessions, and access control.
   - **Website**: https://flask-login.readthedocs.io/

7. **Flask-Migrate**:
   - **Description**: Handles SQLAlchemy database migrations for Flask applications using Alembic.
   - **Usage**: Manages database schema changes over time.
   - **Website**: https://flask-migrate.readthedocs.io/

8. **Werkzeug Security**:
   - **Description**: Part of Werkzeug that provides security utilities.
   - **Usage**: Used for password hashing and verification.
   - **Website**: https://werkzeug.palletsprojects.com/en/2.0.x/utils/#module-werkzeug.security

9. **Flask-WTF**:
   - **Description**: Integrates WTForms with Flask.
   - **Usage**: Provides form handling and validation integrated with Flask.
   - **Website**: https://flask-wtf.readthedocs.io/

10. **Flask-Mail**:
    - **Description**: An extension for Flask that provides simple email sending capabilities.
    - **Usage**: Used to send emails from the Flask application.
    - **Website**: https://pythonhosted.org/Flask-Mail/

11. **Flask-Bootstrap**:
    - **Description**: Integrates Bootstrap into Flask.
    - **Usage**: Used to apply Bootstrap CSS and JavaScript to the Flask application.
    - **Website**: https://pythonhosted.org/Flask-Bootstrap/

### Additional Libraries Used:

1. **functools.wraps**:
   - **Description**: A decorator for updating wrapper functions in a way that they look more like the wrapped function.
   - **Usage**: Used in creating custom decorators such as `login_required`.
   - **Website**: https://docs.python.org/3/library/functools.html#functools.wraps

2. **datetime**:
   - **Description**: A module for manipulating dates and times.
   - **Usage**: Used to handle date and time operations in your models.
   - **Website**: https://docs.python.org/3/library/datetime.html

3. **os**:
   - **Description**: A module providing a portable way of using operating system dependent functionality.
   - **Usage**: Used to handle file paths and perform file operations.
   - **Website**: https://docs.python.org/3/library/os.html

4. **secure_filename from werkzeug.utils**:
   - **Description**: A function that sanitizes a filename to ensure it is safe for storing on a filesystem.
   - **Usage**: Used to safely handle file uploads.
   - **Website**: https://werkzeug.palletsprojects.com/en/2.0.x/utils/#werkzeug.utils.secure_filename

### Sources

1. “Decorators in Python.” GeeksforGeeks, https://www.geeksforgeeks.org/decorators-in-python/.
2. “Flask HTTP Methods, Handle GET & Post Requests.” GeeksforGeeks, GeeksforGeeks, 2 Feb. 2023, https://www.geeksforgeeks.org/flask-http-methods-handle-get-post-requests/.
3. "Password Hashing With Python." Real Python, https://realpython.com/python-hashlib/.
4. "Flask Sessions." Flask Documentation, https://flask.palletsprojects.com/en/2.0.x/quickstart/#sessions.
5. "SQLAlchemy ORM Tutorial for Python Developers." DigitalOcean, https://www.digitalocean.com/community/tutorials/how-to-use-one-to-many-many-to-one-and-many-to-many-relationships-in-sqlalchemy-and-flask.
6. "SQLAlchemy Many-to-Many Relationships." SQLAlchemy Documentation, https://docs.sqlalchemy.org/en/14/orm/basic_relationships.html#many-to-many.
7. "SQLAlchemy ORM Querying." SQLAlchemy Documentation, https://docs.sqlalchemy.org/en/14/orm/query.html.
8. "Flask Flash Messages." Flask Documentation, https://flask.palletsprojects.com/en/2.0.x/patterns/flashing/.
9. "File Uploads in Flask." Flask Documentation, https://flask.palletsprojects.com/en/2.0.x/patterns/fileuploads/.
10. "Flask Templating." Flask Documentation, https://flask.palletsprojects.com/en/2.0.x/tutorial/templates/.
11. "Handling Forms in Flask." Flask Documentation, https://flask.palletsprojects.com/en/2.0.x/patterns/wtforms/.
12. "Flask and AJAX." Real Python, https://realpython.com/flask-by-example-part-2-postgres-sqlalchemy-and-alembic/.
13. "Redirects and Errors." Flask Documentation, https://flask.palletsprojects.com/en/2.0.x/quickstart/#redirects-and-errors.
14. "Configuration Handling." Flask Documentation, https://flask.palletsprojects.com/en/2.0.x/config/.
15. "Basic Date and Time Types." Python Documentation, https://docs.python.org/3/library/datetime.html.
16. "Flask RESTful Quickstart." Flask-RESTful Documentation, https://flask-restful.readthedocs.io/en/latest/quickstart.html.
17. "Cirrus CSS.” Cirrus, https://www.cirrus-ui.com/buttons/basics.


## Code explain 

## Login system (SC1)

1. **Route Definition:**
   ```python
   @app.route('/login', methods=['GET', 'POST'])
   def login():
   ```

This line defines the /login route, which handles both GET and POST requests. A GET request renders the login form, while a POST request processes the login credentials submitted by the user.

2. **Form Submission Handling:**
   ```python
   if request.method == 'POST':
       username = request.form['username']
       password = request.form['password']
   ```

When the form is submitted via POST, the username and password entered by the user are retrieved from the form data using the request.form dictionary. This extracts the values associated with the keys username and password.
3. **User Authentication:**
   ```python
   user = User.query.filter_by(username=username).first()
   if user and check_password_hash(user.password, password):
       session['user_id'] = user.id
       return redirect(url_for('dashboard'))
   else:
       flash('Login Unsuccessful. Please check email and password', 'danger')
       return redirect(url_for('login'))
   ```

1) Database Query:
The User.query.filter_by(username=username).first() line queries the database for a user with the provided username. The first() method returns the first result of the query, or None if no user is found.

3) Password Verification:
If a user is found, the check_password_hash(user.password, password) function checks if the hashed password stored in the database matches the password provided by the user. This ensures secure password verification.

3) Session Management:
If the username and password are valid, the user's ID is stored in the session using session['user_id'] = user.id. This keeps the user logged in across subsequent requests.

4) Redirection:
On successful login, the user is redirected to the dashboard with return redirect(url_for('dashboard')).
If authentication fails, an error message is displayed using flash('Login Unsuccessful. Please check email and password', 'danger'), and the user is redirected back to the login page with return redirect(url_for('login')).

4. **Security Considerations:**
   - **Hashed Passwords:** Passwords are stored as hashed values in the database using generate_password_hash. This adds a layer of security by ensuring that even if the database is compromised, the actual passwords are not exposed.
   - **Session Management:** The Flask session is used to maintain user authentication state. The session dictionary securely stores the user ID, ensuring that users do not have to log in again for each request. Flask sessions are signed cryptographically, providing additional security.

Computational Thinking:
- **Decomposition:** The login system decomposes the task of user authentication into smaller, manageable steps: form submission handling, user authentication, and session management.
- **Abstraction:** The system abstracts away the complexities of password hashing and session management, allowing developers to focus on the higher-level logic of user authentication.
- **Algorithm Design:** The algorithm for user authentication involves querying the database, comparing password hashes, and managing sessions, all of which are well-defined steps in the authentication process.



## Signup System (SC1)

1. **Route Definition:**
   ```python
   @app.route('/signup', methods=['GET', 'POST'])
   def signup():
   ```

   This defines a route `/signup` for handling both GET and POST requests. GET requests are used to render the signup form, while POST requests are used for submitting signup data.

2. **Form Submission Handling:**
   ```python
   if request.method == 'POST':
       username = request.form['username']
       email = request.form['email']
       password = request.form['password']
   ```

   When the form is submitted via POST, the username, email, and password entered by the user are retrieved from the form data.

3. **Input Validation:**
   ```python
   if not email.endswith('@gmail.com'):
       flash('Incorrect email format. Please use an email that ends with @gmail.com', 'danger')
       return redirect(url_for('signup'))
   ```

   The code checks if the email provided by the user ends with '@gmail.com'. If not, an error message is flashed, and the user is redirected back to the signup page.

4. **User Existence Check:**
   ```python
   existing_user = User.query.filter((User.username == username) | (User.email == email)).first()
   if existing_user:
       flash('Username or email already exists. Please choose another.', 'danger')
       return redirect(url_for('signup'))
   ```

   This checks if a user with the provided username or email already exists in the database. If a user is found, an error message is flashed, and the user is redirected back to the signup page.

5. **Password Hashing and User Creation:**
   ```python
   hashed_password = generate_password_hash(password, method='pbkdf2:sha256')
   new_user = User(username=username, email=email, password=hashed_password)
   db.session.add(new_user)
   db.session.commit()
   session['user_id'] = new_user.id
   ```

   If input validation passes and no existing user is found, the password is hashed using a secure hashing algorithm. A new user object is then created with the provided username, email, and hashed password. The user object is added to the database, and the user is automatically logged in by storing their user ID in the session.

Computational Thinking:
- **Decomposition:** The signup system breaks down the task of user registration into smaller steps: input validation, user existence check, password hashing, and user creation.
- **Abstraction:** The system abstracts away the complexities of password hashing and database management, allowing developers to focus on the higher-level logic of user registration.
- **Algorithm Design:** The algorithm for user registration involves input validation, database querying, password hashing, and session management, all of which are well-defined steps in the signup process.


## Add comment System (SC2)

1. **Route Definition:**
   ```python
   @app.route('/add_comment/<int:post_id>', methods=['POST'])
   @login_required
   def add_comment(post_id):
   ```

   This route `/add_comment/<int:post_id>` is defined to handle POST requests. It expects a post ID as part of the URL path. The `@login_required` decorator ensures that only authenticated users can access this route.

2. **Handling Form Submission:**
   ```python
   content = request.form['content']
   user_id = session['user_id']
   ```

   The content of the comment and the user ID of the authenticated user are extracted from the form data and the session, respectively.

3. **Creating and Adding Comment:**
   ```python
   new_comment = Comment(content=content, user_id=user_id, post_id=post_id)
   db.session.add(new_comment)
   db.session.commit()
   ```

   A new comment object is created using the provided content, user ID, and post ID. The comment object is then added to the database, and the transaction is committed to persist the comment.

4. **Generating Response:**
   ```python
   comment = {
       'user': {'username': new_comment.user.username},
       'content': new_comment.content
   }
   return jsonify({'success': True, 'comment': comment})
   ```

   A JSON response is generated containing information about the newly added comment, including the username of the commenter and the content of the comment.

Computational Thinking:
- **Decomposition:** The `add_comment` system decomposes the task into smaller steps: handling form submission, creating and adding a comment to the database, and generating a response.
- **Pattern Recognition:** The system recognizes patterns in extracting form data, creating database records, and generating JSON responses.
- **Abstraction:** The system abstracts away database manipulation complexities and response handling details, allowing developers to focus on the higher-level logic of comment addition.
- **Algorithm Design:** The algorithm for adding a comment involves creating a comment object, adding it to the database, and generating a JSON response, all of which are well-defined steps executed to complete the task.

## Edit comment System (SC2)

1. **Route Definition:**
   ```python
   @app.route('/edit_comment/<int:comment_id>', methods=['POST'])
   @login_required
   def edit_comment(comment_id):
   ```

   This route `/edit_comment/<int:comment_id>` is configured to handle POST requests and requires user authentication with the `@login_required` decorator. It takes the comment ID as a parameter in the URL.

2. **Retrieving Comment Data:**
   ```python
   content = request.form['content']
   comment = Comment.query.get(comment_id)
   ```

   The system extracts the edited content of the comment from the form submitted by the user. It then retrieves the comment object from the database based on the provided comment ID.

3. **Editing Comment:**
   ```python
   if comment.user_id == session['user_id']:
       comment.content = content
       db.session.commit()
       return jsonify({'success': True, 'message': 'Comment edited successfully'})
   else:
       return jsonify({'success': False, 'message': 'You are not authorized to edit this comment'})
   ```

   If the user is the owner of the comment (identified by matching user IDs), the system updates the comment's content with the edited content provided by the user. The database transaction is committed, and a success message is returned. If the user is not authorized to edit the comment, a failure message is returned.

Computational Thinking:
- **Decomposition:** The `edit_comment` system breaks down the task into smaller steps: handling form data, retrieving comment information, checking user authorization, editing the comment content, and responding to the user.
- **Pattern Recognition:** The system recognizes patterns in form data handling, database querying, user authorization checks, database updates, and response generation.
- **Abstraction:** The system abstracts away complexities of comment editing logic, focusing on user authorization and content updating.
- **Algorithm Design:** The algorithm for editing a comment involves parsing form data, retrieving comment information, checking user authorization, updating the comment content, committing the transaction, and responding to the user, all of which are well-defined steps executed to accomplish the task.

## Delete comment System (SC2)

1. **Route Definition:**
   ```python
   @app.route('/delete_comment/<int:comment_id>', methods=['POST'])
   @login_required
   def delete_comment(comment_id):
   ```

   This route `/delete_comment/<int:comment_id>` is configured to handle POST requests, expecting a comment ID as part of the URL path. The `@login_required` decorator ensures only authenticated users can access this route.

2. **Retrieving Comment Data:**
   ```python
   comment = Comment.query.get(comment_id)
   ```

   The comment to be deleted is retrieved from the database based on the provided comment ID.

3. **Authorization Check and Deletion:**
   ```python
   if comment.user_id == session['user_id']:
       db.session.delete(comment)
       db.session.commit()
       return jsonify({'success': True, 'message': 'Comment deleted successfully'})
   ```

   The system checks if the user attempting to delete the comment is the owner of the comment. If so, the comment is deleted from the database, and the transaction is committed. A success message is returned in the JSON response.

4. **Unauthorized Deletion Handling:**
   ```python
   else:
       return jsonify({'success': False, 'message': 'You are not authorized to delete this comment'})
   ```

   If the user is not authorized to delete the comment (i.e., not the owner of the comment), an error message is returned in the JSON response.

Computational Thinking:
- **Decomposition:** The `delete_comment` system decomposes the task into smaller steps: retrieving comment data, checking authorization, deleting the comment, and handling unauthorized deletion attempts.
- **Pattern Recognition:** The system recognizes patterns in querying database records, user authorization checks, database manipulation, and response generation.
- **Abstraction:** The system abstracts away complexities of database querying, authorization validation, and response handling, allowing developers to focus on the logic of comment deletion.
- **Algorithm Design:** The algorithm for deleting a comment involves querying the database, checking user authorization, deleting the comment if authorized, and generating a JSON response, all of which are well-defined steps executed to accomplish the task.

## Like post System (SC3)

1. **Route Definition:**
   ```python
   @app.route('/like_post', methods=['POST'])
   @login_required
   def like_post():
   ```

   This route `/like_post` is defined to handle POST requests. The `@login_required` decorator ensures only authenticated users can access this route.

2. **Handling Form Data:**
   ```python
   post_id = request.form.get('post_id')
   user_id = session['user_id']
   ```

   The post ID and the user ID of the authenticated user are extracted from the form data and session, respectively.

3. **Checking Existing Like:**
   ```python
   like = Like.query.filter_by(user_id=user_id, post_id=post_id).first()
   ```

   The system checks if the user has already liked the post by querying the database for an existing like record associated with the user and the post.

4. **Adding Like if Not Exists:**
   ```python
   if not like:
       new_like = Like(user_id=user_id, post_id=post_id)
       db.session.add(new_like)
       db.session.commit()
       return jsonify({'success': True})
   ```

   If the user has not already liked the post, a new like record is created and added to the database. The transaction is then committed, and a success response is returned.

5. **Handling Existing Like:**
   ```python
   return jsonify({'success': False, 'message': 'You have already liked this post'})
   ```

   If the user has already liked the post, a failure response with an appropriate message is returned.

Computational Thinking:
- **Decomposition:** The `like_post` system decomposes the task into smaller steps: handling form data, checking for existing likes, adding a new like if not exists, and handling existing likes.
- **Pattern Recognition:** The system recognizes patterns in form data handling, database querying, database manipulation, and response generation.
- **Abstraction:** The system abstracts away complexities of database operations, user authentication, and response handling, allowing developers to focus on the higher-level logic of post liking functionality.
- **Algorithm Design:** The algorithm for liking a post involves querying the database, checking for existing likes, adding a new like if not exists, and generating appropriate responses, all of which are well-defined steps executed to accomplish the task.

## Unlike post System (SC3)

1. **Route Definition:**
   ```python
   @app.route('/unlike_post', methods=['POST'])
   @login_required
   def unlike_post():
   ```

   This route `/unlike_post` is designed to handle POST requests. The `@login_required` decorator ensures only authenticated users can access this route.

2. **Handling Form Data:**
   ```python
   post_id = request.form.get('post_id')
   user_id = session['user_id']
   ```

   The post ID and the user ID of the authenticated user are extracted from the form data and session, respectively.

3. **Checking Existing Like:**
   ```python
   like = Like.query.filter_by(user_id=user_id, post_id=post_id).first()
   ```

   The system checks if the user has already liked the post by querying the database for an existing like record associated with the user and the post.

4. **Removing Like if Exists:**
   ```python
   if like:
       db.session.delete(like)
       db.session.commit()
       return jsonify({'success': True})
   ```

   If the user has already liked the post, the existing like record is deleted from the database. The transaction is then committed, and a success response is returned.

5. **Handling Non-Existing Like:**
   ```python
   return jsonify({'success': False, 'message': 'You have not liked this post'})
   ```

   If the user has not liked the post, a failure response with an appropriate message is returned.

Computational Thinking:
- **Decomposition:** The `unlike_post` system decomposes the task into smaller steps: handling form data, checking for existing likes, removing a like if exists, and handling non-existing likes.
- **Pattern Recognition:** The system recognizes patterns in form data handling, database querying, database manipulation, and response generation.
- **Abstraction:** The system abstracts away complexities of database operations, user authentication, and response handling, allowing developers to focus on the higher-level logic of post unliking functionality.
- **Algorithm Design:** The algorithm for unliking a post involves querying the database, checking for existing likes, removing a like if exists, and generating appropriate responses, all of which are well-defined steps executed to accomplish the task.


## Follow users System (SC4)

**I have to note that this was the most interesting part of code for me:**


1. **Route Definition:**
   ```python
   @app.route('/follow_unfollow_user', methods=['POST'])
   @login_required
   def follow_unfollow_user():
   ```

   This route `/follow_unfollow_user` is defined to handle POST requests. The `@login_required` decorator ensures that only authenticated users can access this route.
To go more in depth in the decorator:
The @login_required decorator is a feature provided by Flask-Login, an extension for Flask that handles user session management and authentication.

Functionality of @login_required:
Purpose: The @login_required decorator is used to protect routes in a Flask application, ensuring that only authenticated users can access them. If an unauthenticated user attempts to access a protected route, they are redirected to the login page.

How It Works: When applied to a view function, @login_required checks if the current user is authenticated. If the user is not authenticated, they are redirected to the login view. If the user is authenticated, the request proceeds as normal and the view function is executed.



3. **Retrieving User Data:**
   ```python
   user = User.query.get(session['user_id'])
   target_user = User.query.get(request.form['user_id'])
   action = request.form['action']
   ```

   The user object corresponding to the authenticated user is retrieved from the database. The target user object is retrieved based on the user ID provided in the form data. Additionally, the action to be performed (follow or unfollow) is extracted from the form data.

4. **Following/Unfollowing Logic:**
   ```python
   if action == 'follow':
       user.followed.append(target_user)
   elif action == 'unfollow':
       user.followed.remove(target_user)
   ```

   Depending on the action specified in the form data, the authenticated user either follows or unfollows the target user. This is done by adding or removing the target user from the followed users list of the authenticated user.

5. **Committing Changes:**
   ```python
   db.session.commit()
   ```

   After updating the followed users list, the changes are committed to the database to persist the follow/unfollow action.

6. **Returning Response:**
   ```python
   return jsonify({'success': 'Action completed'}), 200
   ```

   A JSON response indicating the success of the action is returned along with an HTTP status code 200.

Computational Thinking:
- **Decomposition:** The `follow_unfollow_user` system decomposes the task into smaller steps: retrieving user data, determining the action to be performed, updating the followed users list, committing changes, and returning a response.
- **Pattern Recognition:** The system recognizes patterns in extracting form data, updating database records, and generating JSON responses.
- **Abstraction:** The system abstracts away database manipulation complexities and response handling details, allowing developers to focus on the higher-level logic of user interaction.
- **Algorithm Design:** The algorithm for following or unfollowing a user involves database manipulation (addition/removal from followed users list) and response generation, all of which are well-defined steps executed to perform the desired action.

## Unfollow_users system (SC4)

1. **Route Definition:**
   ```python
   @app.route('/follow_unfollow_user', methods=['POST'])
   @login_required
   def follow_unfollow_user():
   ```

   This route `/follow_unfollow_user` is defined to handle POST requests. The `@login_required` decorator ensures that only authenticated users can access this route.

2. **Retrieving User Data:**
   ```python
   user = User.query.get(session['user_id'])
   target_user = User.query.get(request.form['user_id'])
   action = request.form['action']
   ```

   The user object corresponding to the authenticated user is retrieved from the database. The target user object is retrieved based on the user ID provided in the form data. Additionally, the action to be performed (follow or unfollow) is extracted from the form data.

3. **Following/Unfollowing Logic:**
   ```python
   if action == 'follow':
       user.followed.append(target_user)
   elif action == 'unfollow':
       user.followed.remove(target_user)
   ```

   Depending on the action specified in the form data, the authenticated user either follows or unfollows the target user. This is done by adding or removing the target user from the followed users list of the authenticated user.

4. **Committing Changes:**
   ```python
   db.session.commit()
   ```

   After updating the followed users list, the changes are committed to the database to persist the follow/unfollow action.

5. **Returning Response:**
   ```python
   return jsonify({'success': 'Action completed'}), 200
   ```

   A JSON response indicating the success of the action is returned along with an HTTP status code 200.

Computational Thinking:
- **Decomposition:** The `follow_unfollow_user` system decomposes the task into smaller steps: retrieving user data, determining the action to be performed, updating the followed users list, committing changes, and returning a response.
- **Pattern Recognition:** The system recognizes patterns in extracting form data, updating database records, and generating JSON responses.
- **Abstraction:** The system abstracts away database manipulation complexities and response handling details, allowing developers to focus on the higher-level logic of user interaction.
- **Algorithm Design:** The algorithm for following or unfollowing a user involves database manipulation (addition/removal from followed users list) and response generation, all of which are well-defined steps executed to perform the desired action.

## Profile with relevant information (SC5)


1. **Route Definition:**
   ```python
   @app.route('/profile', methods=['GET', 'POST'])
   @login_required
   def profile():
       if 'user_id' not in session:
           return redirect(url_for('login'))
       user = User.query.get(session['user_id'])
   ```

   This route `/profile` handles both GET and POST requests. The `@login_required` decorator ensures only authenticated users can access this route.

2. **Authentication Check:**
   ```python
   if 'user_id' not in session:
       return redirect(url_for('login'))
   ```

   Before proceeding, the system checks if the user is logged in. If not, the user is redirected to the login page.

3. **User Retrieval:**
   ```python
   user = User.query.get(session['user_id'])
   ```

   The system retrieves the user data from the database based on the user ID stored in the session.

4. **Handling Form Submission:**
   ```python
   if request.method == 'POST':
       if request.form['action'] == 'save_bio':
           bio = request.form['bio']
           user.bio = bio
           db.session.commit()
           flash('Bio updated successfully', 'success')
       elif request.form['action'] == 'save_pic':
           if 'profile_pic' in request.files:
              profile_pic = request.files['profile_pic']
                 if profile_pic and allowed_file(profile_pic.filename):
                    filename = secure_filename(profile_pic.filename)
                    profile_pic_path = os.path.join(app.config['UPLOAD_FOLDER'], filename)
                    profile_pic.save(profile_pic_path)
                    user.profile_pic = filename#os.path.join('uploads', filename)  
                    db.session.commit()
                    flash('Profile picture updated successfully', 'success')
       date_posted = request.form.get('date_posted', None)
       return redirect(url_for('profile'))
   ```

   If the request method is POST, the system processes form submissions. Depending on the action specified in the form, the system either updates the user's bio or profile picture. Finally, it redirects the user back to the profile page.


1. **Updating User Bio:**
   ```python
   bio = request.form['bio']
   user.bio = bio
   db.session.commit()
   ```

   Here, the system updates the user's bio based on the data submitted through the form. It extracts the bio from the form data and assigns it to the `bio` attribute of the `user` object. Then, it commits the changes to the database.

2. **Updating Profile Picture:**
   ```python
   if 'profile_pic' in request.files:
       profile_pic = request.files['profile_pic']
       if profile_pic and allowed_file(profile_pic.filename):
           filename = secure_filename(profile_pic.filename)
           profile_pic_path = os.path.join(app.config['UPLOAD_FOLDER'], filename)
           profile_pic.save(profile_pic_path)
           user.profile_pic = filename
           db.session.commit()
           flash('Profile picture updated successfully', 'success')
   ```

   This part of the code handles the updating of the user's profile picture. It checks if a file named 'profile_pic' exists in the files submitted with the request. If it does and the file has an allowed extension, it saves the file to the server's upload folder. Then, it updates the `profile_pic` attribute of the `user` object with the filename. Finally, it commits the changes to the database and displays a success flash message.


6. **Rendering Profile Page:**
   ```python
   return render_template('profile.html', user=user, cache_bust=time.time())
   ```

   The system renders the profile page template, passing the user data retrieved from the database and a cache bust parameter to ensure updated content is displayed.

Computational Thinking:
- **Decomposition:** The `profile` system breaks down the task into smaller steps: authentication check, user data retrieval, form submission handling, and page rendering.
- **Pattern Recognition:** The system recognizes patterns in authentication, database querying, form submission handling, and template rendering.
- **Abstraction:** The system abstracts away complexities of user authentication, database operations, form processing, and template rendering, allowing developers to focus on the logic specific to the profile functionality.
- **Algorithm Design:** The algorithm for the profile system involves checking user authentication, retrieving user data, processing form submissions, updating user information in the database, and rendering the profile page with updated content.

## Upload images System (SC6 HLs)

**I have to note that this was the most difficult part of the code for me:**

1. **Route Definition:**
   ```python
   @app.route('/upload', methods=['GET', 'POST'])
   @login_required
   def upload():
   ```

   This defines a route `/upload` for handling both GET and POST requests. The `@login_required` decorator ensures that only authenticated users can access this route.

2. **Form Submission Handling:**
   ```python
   if request.method == 'POST':
       title = request.form['title']
       content = request.form['content']
       file = request.files['image']
   ```

   When the form is submitted via POST, the title, content, and image file uploaded by the user are retrieved from the form data.

3. **File Upload and Storage:**
   ```python
   if file and allowed_file(file.filename):
       filename = secure_filename(file.filename)
       file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
       image_url = url_for('static', filename=f'uploads/{filename}')
   ```

   If a file is uploaded and it has an allowed file extension (e.g., png, jpg), the file is saved to the server's designated upload folder. A URL for accessing the uploaded image is generated using `url_for`, which is then stored in the database.

4. **Post Creation:**
   ```python
   new_post = Post(title=title, content=content, image_url=image_url, user_id=session['user_id'])
   db.session.add(new_post)
   db.session.commit()
   ```

   A new post object is created with the provided title, content, image URL, and the ID of the authenticated user who uploaded the post. The post object is added to the database, committing the transaction.

5. **Redirection:**
   ```python
   return redirect(url_for('dashboard'))
   ```

   Upon successful upload, the user is redirected to the dashboard page where they can view their uploaded post along with other content.

Computational Thinking:
- **Decomposition:** The upload system decomposes the task of uploading a post into smaller steps: form submission handling, file upload and storage, post creation, and redirection.
- **Pattern Recognition:** The system recognizes patterns in file extensions to determine if an uploaded file is allowed or not. It also recognizes patterns in route handling and user authentication to ensure proper access control.
- **Abstraction:** The system abstracts away the complexities of file handling, URL generation, and database management, allowing developers to focus on the higher-level logic of post uploading.
- **Algorithm Design:** The algorithm for uploading a post involves multiple steps such as file validation, storage, database interaction, and redirection, all of which are well-defined and sequentially executed to ensure a smooth user experience.

## Send email System (SC7 HL++)

1. **Route Definition:**
   ```python
   @app.route('/send_email', methods=['POST'])
   @login_required
   def send_email():
   ```

   This route `/send_email` is set up to handle POST requests and requires user authentication with the `@login_required` decorator.

2. **Handling Form Data:**
   ```python
   user_id = request.form.get('user_id')
   subject = request.form.get('subject')
   cc = request.form.get('cc')
   description = request.form.get('description')
   ```

   The system extracts data from the form submitted by the user, including recipient user ID, email subject, CC recipients, and email description.

3. **Retrieving User Data:**
   ```python
   user = User.query.get(user_id)
   ```

   Using the extracted user ID, the system retrieves user data from the database to obtain the email address of the recipient.

4. **Constructing Email Message:**
   ```python
   message = f"""
   To: {recipient}
   CC: {cc}
   Subject: {subject}
   
   {description}
   """
   ```

   The system constructs the email message, including recipient, CC, subject, and description, in a formatted string.

5. **Logging Email Message:**
   ```python
   print(message)
   ```

   For demonstration purposes, the system prints the email message to the terminal, simulating the sending of the email.

6. **Flashing Success Message:**
   ```python
   flash('Email sent (printed to terminal) successfully', 'success')
   ```

   After the email message is logged, a success message is flashed to the user interface, indicating that the email has been "sent."

7. **Redirecting User:**
   ```python
   return redirect(url_for('profile'))
   ```

   Finally, the system redirects the user back to their profile page after the email "sending" process is completed.

Computational Thinking:
- **Decomposition:** The `send_email` system breaks down the task into smaller steps: handling form data, retrieving user information, constructing an email message, logging the message, flashing a success message, and redirecting the user.
- **Pattern Recognition:** The system recognizes patterns in form data handling, database querying, email message construction, logging, and response generation.
- **Abstraction:** The system abstracts away complexities of email sending, focusing on the construction and logging of the email message for demonstration purposes.
- **Algorithm Design:** The algorithm for "sending" an email involves parsing form data, retrieving user information, constructing an email message, logging the message, and redirecting the user, all of which are well-defined steps executed to simulate the email sending process.

# Criteria D: Functionality

# Criteria E: Evaluation
