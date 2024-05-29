```.py
from functools import wraps
from flask import redirect, url_for, session, jsonify, request, flash, render_template
import os
from werkzeug.utils import secure_filename
from werkzeug.security import generate_password_hash, check_password_hash
from app import app, db
from app.models import User, Post, Comment, Like
from datetime import datetime
import time


def login_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if 'user_id' not in session:
            return redirect(url_for('login'))
        return f(*args, **kwargs)
    return decorated_function

ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif'}

def allowed_file(filename):
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS

@app.route('/')
@app.route('/dashboard')
@login_required
def dashboard():
    if User.query.count() == 0: 
        return redirect(url_for('signup'))
    user = User.query.get(session['user_id'])
    user_posts = Post.query.filter_by(user_id=user.id).all()
    followed_posts = Post.query.filter(Post.user_id.in_([u.id for u in user.followed])).all()
    return render_template('dashboard.html', user=user, user_posts=user_posts, followed_posts=followed_posts)



@app.route('/login', methods=['GET', 'POST'])
def login():
    if User.query.count() == 0:  
        return redirect(url_for('signup'))
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        user = User.query.filter_by(username=username).first()
        if user and check_password_hash(user.password, password):
            session['user_id'] = user.id
            return redirect(url_for('dashboard'))
        else:
            flash('Login Unsuccessful. Please check email and password', 'danger')
            return redirect(url_for('login'))
    return render_template('login.html')

@app.route('/signup', methods=['GET', 'POST'])
def signup():
    if request.method == 'POST':
        username = request.form['username']
        email = request.form['email']
        password = request.form['password']

        if not email.endswith('@gmail.com'):
            flash('Incorrect email format. Please use an email that ends with @gmail.com', 'danger')
            return redirect(url_for('signup'))

        existing_user = User.query.filter((User.username == username) | (User.email == email)).first()
        if existing_user:
            flash('Username or email already exists. Please choose another.', 'danger')
            return redirect(url_for('signup'))

        hashed_password = generate_password_hash(password, method='pbkdf2:sha256')
        new_user = User(username=username, email=email, password=hashed_password)
        db.session.add(new_user)
        db.session.commit()
        session['user_id'] = new_user.id
        return redirect(url_for('follow_users'))
    return render_template('signup.html')


@app.route('/upload', methods=['GET', 'POST'])
@login_required
def upload():
    if request.method == 'POST':
        title = request.form['title']
        content = request.form['content']
        file = request.files['image']
        if file and allowed_file(file.filename):
            filename = secure_filename(file.filename)
            file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
            image_url = url_for('static', filename=f'uploads/{filename}')
            new_post = Post(title=title, content=content, image_url=image_url, user_id=session['user_id'])
            db.session.add(new_post)
            db.session.commit()
            return redirect(url_for('dashboard'))
        else:
            flash('Invalid file format. Please upload an image file.', "danger")
    return render_template('upload.html')

@app.route('/follow_users', methods=['GET', 'POST'])
@login_required
def follow_users():
    user = User.query.get(session['user_id'])
    followed_users = user.followed.all()
    unfollowed_users = User.query.filter(User.id != user.id, ~User.id.in_([u.id for u in followed_users])).all()
    return render_template('followed_users.html', user=user, followed_users=followed_users, unfollowed_users=unfollowed_users)

@app.route('/follow_unfollow_user', methods=['POST'])
@login_required
def follow_unfollow_user():
    user = User.query.get(session['user_id'])
    target_user = User.query.get(request.form['user_id'])
    action = request.form['action']
    
    if action == 'follow':
        user.followed.append(target_user)
    elif action == 'unfollow':
        user.followed.remove(target_user)
    
    db.session.commit()
    return jsonify({'success': 'Action completed'}), 200

@app.route('/add_comment/<int:post_id>', methods=['POST'])
@login_required
def add_comment(post_id):
    content = request.form['content']
    user_id = session['user_id']
    new_comment = Comment(content=content, user_id=user_id, post_id=post_id)
    db.session.add(new_comment)
    db.session.commit()
    comment = {
        'user': {'username': new_comment.user.username},
        'content': new_comment.content
    }
    return jsonify({'success': True, 'comment': comment})

@app.route('/edit_comment/<int:comment_id>', methods=['POST'])
@login_required
def edit_comment(comment_id):
    content = request.form['content']
    comment = Comment.query.get(comment_id)
    if comment.user_id == session['user_id']:
        comment.content = content
        db.session.commit()
        return jsonify({'success': True, 'message': 'Comment edited successfully'})
    else:
        return jsonify({'success': False, 'message': 'You are not authorized to edit this comment'})

@app.route('/delete_comment/<int:comment_id>', methods=['POST'])
@login_required
def delete_comment(comment_id):
    comment = Comment.query.get(comment_id)
    if comment.user_id == session['user_id']:
        db.session.delete(comment)
        db.session.commit()
        return jsonify({'success': True, 'message': 'Comment deleted successfully'})
    else:
        return jsonify({'success': False, 'message': 'You are not authorized to delete this comment'})

@app.route('/logout')
def logout():
    session.pop('user_id', None)
    return redirect(url_for('login'))

@app.route('/like_post', methods=['POST'])
@login_required
def like_post():
    post_id = request.form.get('post_id')
    user_id = session['user_id']
    like = Like.query.filter_by(user_id=user_id, post_id=post_id).first()
    if not like:
        new_like = Like(user_id=user_id, post_id=post_id)
        db.session.add(new_like)
        db.session.commit()
        return jsonify({'success': True})
    return jsonify({'success': False, 'message': 'You have already liked this post'})

@app.route('/unlike_post', methods=['POST'])
@login_required
def unlike_post():
    post_id = request.form.get('post_id')
    user_id = session['user_id']
    like = Like.query.filter_by(user_id=user_id, post_id=post_id).first()
    if like:
        db.session.delete(like)
        db.session.commit()
        return jsonify({'success': True})
    return jsonify({'success': False, 'message': 'You have not liked this post'})


@app.route('/profile', methods=['GET', 'POST'])
@login_required
def profile():
    if 'user_id' not in session:
        return redirect(url_for('login'))

    user = User.query.get(session['user_id'])

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
                    user.profile_pic = filename#os.path.join('uploads', filename)  # Update user profile pic path
                    db.session.commit()
                    flash('Profile picture updated successfully', 'success')
        date_posted = request.form.get('date_posted', None)
        return redirect(url_for('profile'))

    return render_template('profile.html', user=user, cache_bust=time.time())

@app.route('/send_email', methods=['POST'])
@login_required
def send_email():
    user_id = request.form.get('user_id')
    subject = request.form.get('subject')
    cc = request.form.get('cc')
    description = request.form.get('description')

    user = User.query.get(user_id)
    if not user:
        flash('User not found', 'danger')
        return redirect(url_for('profile'))

    recipient = user.email

    message = f"""
    To: {recipient}
    CC: {cc}
    Subject: {subject}
    
    {description}
    """

    print(message) 

    flash('Email sent (printed to terminal) successfully', 'success')
    return redirect(url_for('profile'))


@app.route('/user/<int:user_id>')
@login_required
def view_profile(user_id):
    user = User.query.get_or_404(user_id)
    if user.id == session['user_id']:
        return redirect(url_for('profile'))
    return render_template('view_profile.html', user=user, cache_bust=time.time())


if __name__ == "__main__":
    app.run(debug=True)


```
