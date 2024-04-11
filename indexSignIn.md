---
permalink: /signin
title: Sign In
---

<html>
    <head>
    <style>
            .darkmode {
                background: #252525;
                color: #ffffff;
            }
            .lightmode {
                background: #ffffff;
                color: #000000;
            }
        </style>
        <link id="theme-style" rel="stylesheet" type="text/css" href="assets/css/style.css">
    </head>
    <body class="lightmode">
        <script>
            // import { uri, options } from '/static/js/api/config.js';
            window.sign_in = function signin(){
                const authurl = 'http://localhost:8086/api/users/authenticate';
                const body = {
                    name: document.getElementById('name').value,
                    uid: document.getElementById('uid').value,
                    password: document.getElementById('password').value
                };
                const authOptions = {
                    // ...options,
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(body),
                    cache: 'no-cache',
                    credentials: 'include'
                };
                // Fetch the JWT
                fetch(authurl, authOptions)
                .then(response => response.json())
                .then(data => {
                    console.log(data);
                    // Console log the success
                    var users = document.getElementById('users');
                    if(users) {
                        users.innerHTML = JSON.stringify(data);
                    }
                    alert("User Doesnt Exist");
                    // window.location.href = "/register";
                })
                .catch(err => {
                    console.error(err);
                    alert("User Authenticated Successfully");
                    userBody = "{" + "uid: " + document.getElementById('uid').value + "," + " password: " + document.getElementById('password').value + "}";
                    console.log(userBody);
                    window.localStorage.setItem('uid', document.getElementById('uid').value);
                    window.localStorage.setItem('userBody', userBody);
                    console.log(window.localStorage.getItem('userBody'));
                    console.log("User Auth Token Stored Successfully");
                    console.log(document.getElementById('uid').value);
                    if (document.getElementById('uid').value == 'admin' || document.getElementById('uid').value == 'Admin'){
                        window.location.href = "/AtlasIndex/display";
                        return;
                    }
                    else{
                        window.location.href = "/AtlasIndex/";
                    }             
                });
            }
        </script>
        <script>
            var darkMode = false;
            window.onload = function() {
                var themeStyle = document.getElementById('theme-style');
                var body = document.body;
                var storedTheme = localStorage.getItem('theme');
                if (storedTheme === 'dark') {
                    themeStyle.href = "assets/css/dark.css";
                    body.classList.remove('lightmode');
                    body.classList.add('darkmode');
                } else {
                    themeStyle.href = "assets/css/style.css";
                    body.classList.remove('darkmode');
                    body.classList.add('lightmode');
                }
            }
        </script>
        <form action="javascript:sign_in()">
            <p><label>
                    Username:
                    <input type="text" name="name" id="name" required>
            </label></p>
            <p><label>
                User ID:
                <input type="text" name="uid" id="uid" required>
            </label></p>
            <p><label>
                Password:
                <input type="password" name="password" id="password" required>
            </label></p>
            <p>
                <button>Login</button>
            </p>
        </form>
    </body>
</html>