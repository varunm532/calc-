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
        <button type="button" onclick="checkInput()">Submit</button>
    </form>
    <form id="myFormxin">
        <label for="textInputxin">Enter the initial x-value:</label>
        <input type="text" id="textInputxin" name="textInputxin"  onfocus="highlightdx()" onblur="highlightdx()">
        <button type="button" onclick="checkInputxin() ">Submit</button>
    </form>
    <form id="myFormdx">
        <label for="textInputdx">Enter the number of steps:</label>
        <input type="text" id="textInputdx" name="textInputdx"  onfocus="highlightdx()" onblur="highlightdx()">
        <button type="button" onclick="checkInputdx()">Submit</button>
    </form>
     <form id="myFormdy">
        <label for="textInputdy">Enter the differential equation:</label>
        <input type="text" id="textInputdy" name="textInputdy"  onfocus="highlightdy()" onblur="highlightdy()">
        <button type="button" onclick="checkInputdy()">Submit</button>
    </form>
     <form id="myFormxed">
        <label for="textInputxed">Enter the x-value for approximation:</label>
        <input type="text" id="textInputxed" name="textInputxed">
        <button type="button" onclick="checkInputxed()">Submit</button>
    </form>
     
</html>
<script>
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
    
</script>
