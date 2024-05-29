# Criteria C: Development
## Signup system (SC1)

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
