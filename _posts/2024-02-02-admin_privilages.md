---
toc: true
comments: false
layout: categories
title: admin
description: Example Blog!!!  This shows planning and notes from hacks.
type: plans
courses: { compsci: {week: 1} }
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Display Users</title>
</head>
<body>
<h2>User List</h2>

<div id="userList"></div>
<button id="delete" onClick = "user_delete()" >Click here to delete your account<button>
<div id="userList"></div>
<button id="delete" onClick = "logout()" >Click here to logout<button>
<button id="viewall" onClick = "viewall()" >Click here to view all users<button>
<script>
    // Function to fetch and display user data
    function displayUsers() {
        //fetch('/api/players/', {
            fetch('/api/user/display', {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json'
            }
        })
        .then(response => response.json())
        .then(users => {
            // Build HTML for user list
            var userListHTML = '<ul>';
            users.forEach(user => {
                userListHTML += '<li>';
                userListHTML += '<strong>Name:</strong> ' + user.name + '<br>';
                userListHTML += '<strong>User ID:</strong> ' + user.uid + '<br>';
                userListHTML += '<strong>Date of Birth:</strong> ' + user.dob + '<br>';
                userListHTML += '<strong>Zipcode:</strong> ' + user.zipcode + '<br>';
                userListHTML += '<strong>Posts:</strong> <ul>';
                   user.posts.forEach(post => {
                    userListHTML += '<li>' + post.note + '</li>';
                });
                
                userListHTML += '</ul>';
                userListHTML += '</li>';
            });
            userListHTML += '</ul>';
            
            // Update the HTML content
            document.getElementById('userList').innerHTML = userListHTML;
        })
        .catch(error => {
            console.error('Error fetching users:', error);
            alert('You need admin privilages, 403');
            window.location.href = "http://127.0.0.1:8086/display/"
        });
    }
    function user_delete() {
        window.location.href = "http://127.0.0.1:8086/delete"
    }
    function logout() {
        // Clear the JWT token from local storage
        window.location.href = 'http://127.0.0.1:8086/logout';
    }
    function viewall() {
        // Clear the JWT token from local storage
        window.location.href = 'http://127.0.0.1:8086/logout';
    }
    // Call the displayUsers function on page load
    window.onload = function() {
        displayUsers();
    };
</script>

</body>
</html>