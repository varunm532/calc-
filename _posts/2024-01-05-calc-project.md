---
toc: true
comments: false
layout: post
title: Calc project 1
description: Example Blog!!!  This shows planning and notes from hacks.
type: plans
courses: { compsci: {week: 0} }
---
<style>
    .p1{
        display: inline-block;
        font-size: 20px;
    }
    .p2{
        display: inline-block;
        font-size: 15px
    }
    .color{
        color: green;
    }
    .nocolor{
        color: black;
    }
</style>
<html>
    <h1>Euler's method</h1>
    <h4>What is it?</h4>
    <p>Euler's method is used to approximate a point using a differential equation and a know point. </p>
    <h1> Euler's method calculator </h1>
    <div>
        <p class="p1 nocolor" >f(x) </p><p class="p1">=</p><p class="p1 nocolor" id="y">y </p><p class="p1"> + </p><p class="p1 nocolor" id="dx"> dx </p><p class="p1 nocolor" id="dy"> (dy/dx) </p>
    </div>
    <form id="myForm">
        <label for="textInput">Enter the initial y-value:</label>
        <input type="text" id="textInput" name="textInput"  onfocus="highlighty()" onblur="highlighty()">
        <button type="button" onclick="checkInput()">Submit</button><p class="p2" id ="return yval"></p>
    </form>
    <br>
    <form id="myFormxin">
        <label for="textInputxin">Enter the initial x-value:</label>
        <input type="text" id="textInputxin" name="textInputxin"  onfocus="highlightdx()" onblur="highlightdx()">
        <button type="button" onclick="checkInputxin() ">Submit</button><p class="p2" id ="return xval"></p>
    </form>
    <br>
    <form id="myFormdx">
        <label for="textInputstep">Enter the number of steps:</label>
        <input type="text" id="textInputstep" name="textInputstep"  onfocus="highlightdx()" onblur="highlightdx()">
        <button type="button" onclick="checkInputstep()">Submit</button><p class="p2" id ="return stepval"></p>
    </form>
    <br>
     <form id="myFormdy">
        <label for="textInputdy">Enter the differential equation:</label>
        <input type="text" id="textInputdy" name="textInputdy"  onfocus="highlightdy()" onblur="highlightdy()">
        <button type="button" onclick="checkInputdy()">Submit</button>
    </form>
    <br>
     <form id="myFormxed">
        <label for="textInputxfinal">Enter the x-value for approximation:</label>
        <input type="text" id="textInputxfinal" name="textInputxfinal">
        <button type="button" onclick="checkInputxfinal()">Submit</button><p class="p2" id ="return xfinal"></p>
    </form>
     
</html>
<script>
    // general variables for user inputed values
    let yval = ""
    let xval = ""
    let stepval = ""
    let difeq = ""
    let xfinal = ""
    // code for highlighting the equation
    function highlighty() {
        var y = document.getElementById('y');
        y.classList.toggle('nocolor');
        y.classList.toggle('color');
    }
    function highlightdx() {
        var dx = document.getElementById('dx');
        dx.classList.toggle('nocolor');
        dx.classList.toggle('color');
    }
    function highlightdy() {
        var dy = document.getElementById('dy');
        dy.classList.toggle('nocolor');
        dy.classList.toggle('color');
    }
    function checkInput() {
        yval = document.getElementById('textInput').value;
        var returnElement = document.getElementById('return yval');
        if (isNaN(yval) || yval === "") {
            returnElement.innerHTML = " Please enter a valid number";
        } else {
            returnElement.innerHTML = " Value stored";
        }
    }
    function checkInputxin() {
        xval = document.getElementById('textInputxin').value;
        var returnElement = document.getElementById('return xval');
        if (isNaN(xval) || xval === "") {
            returnElement.innerHTML = " Please enter a valid number";
        } else {
            returnElement.innerHTML = " Value stored";
        }
    }
    function checkInputstep() {
        stepval = document.getElementById('textInputstep').value;
        var returnElement = document.getElementById('return stepval');
        if (isNaN(stepval) || stepval === "") {
            returnElement.innerHTML = " Please enter a valid number";
        } else {
            returnElement.innerHTML = " Value stored";
        }
    }
    function checkInputxfinal() {
        xfinal = document.getElementById('textInputxfinal').value;
        var returnElement = document.getElementById('return xfinal');
        if (isNaN(xfinal) || xfinal === "") {
            returnElement.innerHTML = " Please enter a valid number";
        } else {
            returnElement.innerHTML = " Value stored";
        }
    }
</script>
