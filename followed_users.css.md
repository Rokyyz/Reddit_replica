```.css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #ecf0f1;
}

.main-content {
    margin-left: 250px; 
    padding: 20px;
    box-sizing: border-box;
}

h1, h2 {
    text-align: center;
    color: #34495e;
}

.users-list {
    display: flex;
    justify-content: flex-start; 
    flex-wrap: wrap;
}

.users-list > h2 {
    flex-basis: 100%;
    margin-top: 20px;
}
.users-list > ul {
    flex-basis: 50%; 
    padding: 0;
    margin: 0;
}

.users-list > ul li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
}

.follow-button,
.unfollow-button {
    padding: 5px 10px;
    background-color: #3498db;
    color: #fff;
    border: none;
    cursor: pointer;
    margin-left: 10px;
}

```
