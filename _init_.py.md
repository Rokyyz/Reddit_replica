```.py

import os
import secrets
from flask import Flask, session
from flask_sqlalchemy import SQLAlchemy

from flask_login import LoginManager

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your_secret_key'
db_file_path = os.path.join(os.path.abspath(os.path.dirname(__file__)), 'site.db')
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///' + db_file_path
app.config['UPLOAD_FOLDER'] = 'app/static/uploads'
db = SQLAlchemy(app)

login_manager = LoginManager()
login_manager.init_app(app)

@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))

@app.before_request
def add_csrf_token():
    if 'csrf_token' not in session:
        session['csrf_token'] = secrets.token_hex(16)

from app import routes, models

```
