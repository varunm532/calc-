---
permalink: /logout
title: Logout
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
    <body>
        <div class="index">
            <div class="jumbotron jumbotron-fluid" style="text-align: center; ">
                <h2>Thank you for using our service</h2>
                <!-- Prompt user to logout with button -->
                <button id="logoutButton">Logout</button>
                <p>Want To Go Back?</p>
                <a href="javascript:history.back()">Go Back</a>
            </div>
        </div>
        <script>
            document.getElementById('logoutButton').addEventListener('click', function() {
                document.cookie = 'jwt=; Max-Age=0; path=/; samesite=None; secure';
                window.localStorage.removeItem('userBody');
                window.localStorage.removeItem('uid');
                window.location.href = "/AtlasIndex/";
            });
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
    </body>
</html>