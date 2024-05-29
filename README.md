# Criteria C: Development
## Login system (SC1)

The login system in the provided code ensures that users can securely access their accounts by verifying their credentials. Let's break down how this works and how it represents computational thinking:

1. **Route Definition:**
   ```python
   @app.route('/login', methods=['GET', 'POST'])
   def login():
   ```

   This defines a route `/login` for handling both GET and POST requests. GET requests are used to render the login form, while POST requests are used for submitting login credentials.

2. **Form Submission Handling:**
   ```python
   if request.method == 'POST':
       username = request.form['username']
       password = request.form['password']
   ```

   When the form is submitted via POST, the username and password entered by the user are retrieved from the form data.

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

   Here, the code queries the database to find a user with the provided username. If a user is found and the password hash matches the stored hash for that user, the user is considered authenticated. Their user ID is stored in the session, and they are redirected to the dashboard. If authentication fails, an error message is flashed, and the user is redirected back to the login page.

4. **Security Considerations:**
   - **Hashed Passwords:** The passwords are stored securely as hashed values using the `generate_password_hash` function. This ensures that even if the database is compromised, the passwords remain protected.
   - **Session Management:** User authentication is maintained using Flask's session management. Once a user logs in, their user ID is stored in the session, allowing them to access restricted routes without needing to log in again for subsequent requests.

Computational Thinking:
- **Decomposition:** The login system decomposes the task of user authentication into smaller, manageable steps: form submission handling, user authentication, and session management.
- **Abstraction:** The system abstracts away the complexities of password hashing and session management, allowing developers to focus on the higher-level logic of user authentication.
- **Algorithm Design:** The algorithm for user authentication involves querying the database, comparing password hashes, and managing sessions, all of which are well-defined steps in the authentication process.



## Signup System (SC1)

1. **Route Definition:**
   ```python
   @app.route('/signup', methods=['GET', 'POST'])
   def signup():
       # ...
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
