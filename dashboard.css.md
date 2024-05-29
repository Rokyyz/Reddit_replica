```.css
/* dashboard.css */

body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    margin: 0;
    padding: 0;
}

/* .sidebar {
    width: 200px;
    background-color: #2c3e50;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 20px;
} */

.main-content {
    margin-left: 250px;
    padding: 20px;
}

.post {
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
    padding: 20px;
}

.post h3 {
    margin-top: 0;
}

.post p {
    margin-bottom: 10px;
}

.img {
    width: 300px;
    height: 300px;
    object-fit: cover; /* Add this line */
    border-radius: 5px; /* Add this line */
}

.comments {
    margin-top: 20px;
}

.comments p {
    margin-bottom: 5px;
}

.comment-form textarea {
    width: calc(100% - 5%);
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.comment-form button {
    width: 80px;
    padding: 10px;
    border: none;
    border-radius: 5px;
    background-color: #333;
    color: #fff;
    cursor: pointer;
}

.edit-comment,
.delete-comment {
    margin-left: 10px;
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    background-color: #333;
    color: #fff;
    cursor: pointer;
}

.edit-comment:hover,
.delete-comment:hover {
    background-color: #555;
}

.post-card {
    background-color: #555;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin: 20px;
    margin-bottom: 30px;
    padding-top: 30px;
    width: 47.5vw;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.post {
    margin-bottom: 30px;
    width: 43.5vw;
    
}

.comment{
    width: calc(100% - 1.7%);
    /* background-color: #555; */
}

.comment-divs{
    /* background-color: aqua; */
    display: flex;
    justify-content: space-between;
    margin-bottom: 5px;
}



```
