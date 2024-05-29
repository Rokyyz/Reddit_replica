```.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Dashboard</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='sidebar.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='dashboard.css') }}">
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        $(document).ready(function(){

            $('.like-btn').click(function(){
                var postId = $(this).data('post-id');
                $.post("{{ url_for('like_post') }}", { post_id: postId }, function(response){
                    if(response.success){
                        location.reload();
                    } else {
                        alert('Failed to like post');
                    }
                });
            });

            $('.unlike-btn').click(function(){
                var postId = $(this).data('post-id');
                $.post("{{ url_for('unlike_post') }}", { post_id: postId }, function(response){
                    if(response.success){
                        location.reload();
                    } else {
                        alert('Failed to unlike post');
                    }
                });
            });
            
            $('.follow-btn').click(function(){
                var userId = $(this).data('id');
                var action = 'follow';
                $.post("{{ url_for('follow_unfollow_user') }}", { user_id: userId, action: action }, function(response){
                    if(response.success){
                        location.reload();
                    }
                });
            });

            $('.unfollow-btn').click(function(){
                var userId = $(this).data('id');
                var action = 'unfollow';
                $.post("{{ url_for('follow_unfollow_user') }}", { user_id: userId, action: action }, function(response){
                    if(response.success){
                        location.reload();
                    }
                });
            });

            $('.comment-form').submit(function(event){
                event.preventDefault();
                var form = $(this);
                $.post(form.attr('action'), form.serialize(), function(response){
                    if(response.success){
                        var commentHtml = `<div class="comment">
                                            <div class="comment-divs">
                                                <div>
                                                    <strong>${response.comment.user.username}</strong>: ${response.comment.content}
                                                </div>
                                                <div>
                                                    {% if user.id %}
                                                        <button class="edit-comment" data-comment-id="${response.comment.id}">Edit</button>
                                                        <button class="delete-comment" data-comment-id="${response.comment.id}">Delete</button>
                                                    {% endif %}
                                                </div>
                                            </div>
                                        </div>`;
                        form.siblings('.comments').append(commentHtml);
                        form.find('textarea[name="content"]').val('');
                    }
                });
            });

            $(document).on('click', '.edit-comment', function(){
                var commentId = $(this).data('comment-id');
                var content = prompt('Enter new comment:');
                if(content !== null) {
                    $.ajax({
                        url: "{{ url_for('edit_comment', comment_id=0) }}".replace('0', commentId),
                        method: 'POST',
                        data: { content: content },
                        success: function(response) {
                            if(response.success) {
                                location.reload();
                            } else {
                                alert('Failed to edit comment');
                            }
                        }
                    });
                }
            });

            $(document).on('click', '.delete-comment', function(){
                var commentId = $(this).data('comment-id');
                if(confirm('Are you sure you want to delete this comment?')) {
                    $.ajax({
                        url: "{{ url_for('delete_comment', comment_id=0) }}".replace('0', commentId),
                        method: 'POST',
                        success: function(response) {
                            if(response.success) {
                                location.reload();
                            } else {
                                alert('Failed to delete comment');
                            }
                        }
                    });
                }
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
        <h1>Dashboard</h1>

        {% if user_posts %}
            <h2>Your Posts</h2>
            <div class="post-card">
                {% for post in user_posts %}
                    <div class="post">
                        <p>Posted by: <a href="{{ url_for('view_profile', user_id=post.user.id) }}">{{ post.user.username }}</a></p>
                        <p>Posted on: {{ post.date_posted.strftime('%Y, %d %B at: %H:%M') }}</p>
                        <h3>{{ post.title }}</h3>
                        <p>{{ post.content }}</p>
                        {% if post.image_url %}
                            <img style="width: 43vw; aspect-ratio: 1 / 1; object-fit: cover;" src="{{ post.image_url }}" alt="Post Image">
                        {% endif %}
                        
                        <button class="like-btn" data-post-id="{{ post.id }}">Like</button>
                        <button class="unlike-btn" data-post-id="{{ post.id }}">Unlike</button>
                        <p>{{ post.like_count() }} Likes</p>

                        <div class="comments">
                            {% for comment in post.comments %}
                                <div class="comment">
                                    <div class="comment-divs">
                                        <div>
                                            <strong>{{ comment.user.username }}</strong>: {{ comment.content }}
                                        </div>
                                        <div>
                                            {% if comment.user_id == user.id %}
                                                <button class="edit-comment" data-comment-id="{{ comment.id }}">Edit</button>
                                                <button class="delete-comment" data-comment-id="{{ comment.id }}">Delete</button>
                                            {% endif %}
                                        </div>
                                        
                                    </div>
                                </div>
                            {% endfor %}
                        </div>
                        
                        <form class="comment-form" action="{{ url_for('add_comment', post_id=post.id) }}" method="POST">
                            <textarea name="content" placeholder="Add a comment"></textarea>
                            <button type="submit">Comment</button>
                        </form>
                    </div>
                {% endfor %}
            </div>
        {% endif %}

        {% if followed_posts %}
            <h2>Followed Users' Posts</h2>
            <div class="post-card">
                {% for post in followed_posts %}
                    <div class="post">
                        <p>Posted by: <a href="{{ url_for('view_profile', user_id=post.user.id) }}">{{ post.user.username }}</a></p>
                        <h3>{{ post.title }}</h3>
                        <p>{{ post.content }}</p>
                        {% if post.image_url %}
                            <img style="width: 43vw; aspect-ratio: 1 / 1; object-fit: cover;" src="{{ post.image_url }}" alt="Post Image">
                        {% endif %}
                     
                        <button class="like-btn" data-post-id="{{ post.id }}">Like</button>
                        <button class="unlike-btn" data-post-id="{{ post.id }}">Unlike</button>
                        <p>{{ post.like_count() }} Likes</p>

                        <div class="comments">
                            {% for comment in post.comments %}
                                <div class="comment">
                                    <div class="comment-divs">
                                        <div>
                                            <strong>{{ comment.user.username }}</strong>: {{ comment.content }}
                                        </div>
                                        <div>
                                            {% if comment.user_id == user.id %}
                                                <button class="edit-comment" data-comment-id="{{ comment.id }}">Edit</button>
                                                <button class="delete-comment" data-comment-id="{{ comment.id }}">Delete</button>
                                            {% endif %}
                                        </div>
                                        
                                    </div>
                                </div>
                            {% endfor %}
                        </div>
                        
                        <form class="comment-form" action="{{ url_for('add_comment', post_id=post.id) }}" method="POST">
                            <textarea name="content" placeholder="Add a comment"></textarea>
                            <button type="submit">Comment</button>
                        </form>
                    </div>
                {% endfor %}
            </div>
        {% endif %}
    </div>
</body>
</html>

```
