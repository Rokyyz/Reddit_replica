```.css
/* profile.css */

body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}

.main-content {
    margin-left: 250px;
    padding: 20px;
    box-sizing: border-box;
    width: calc(100% - 250px);
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    /* align-items: center; */
}

h1 {
    text-align: center;
    margin-bottom: 20px;
    color: #333;
}

p {
    font-size: 18px;
    color: #666;
    margin: 10px 0;
}

h2 {
    margin-top: 40px;
    margin-bottom: 20px;
    color: #333;
}


.user-info{
    color: rgb(83, 90, 101);
}

.profile-pic-upload-section{
    margin-top: 50px;
}

.bio-section{
    margin: 20px 0px;
}

.bold{
    font-weight: bold;
}

.user-info{
    font-size: large;
    margin-top: 8vh;
}

.profile-pic {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    object-fit: cover;
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
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    padding-top: 60px;
}
.modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
}
.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}
.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}
```
