---
layout: page
title: <i class="fa-solid fa-brain"></i>
permalink: /compare/
description: Competing against our AI Overlord
nav: true
nav_order: 8
---

<style>
.image-compare-container {
    position: relative;
    width: 100%;
    max-width: 500px; /* Adjust as needed */
}

.image-compare-image {
    display: block;
    width: 100%;
}

.image-compare-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
}

.image-compare-slider {
    position: absolute;
    z-index: 9;
    cursor: ew-resize;
    width: 5px; /* Adjust as needed */
    height: 100%;
    background-color: #2196F3; /* Slider color */
    left: 50%; /* Initial position */
}
</style>

<script type="text/javascript">
document.addEventListener('DOMContentLoaded', function() {
    var container = document.getElementById('image-compare-container');
    var slider = container.querySelector('.image-compare-slider');
    var overlay = container.querySelector('.image-compare-overlay');
    var isDown = false;

    var drag = function(e) {
        if (!isDown) return;
        e.preventDefault();
        var rect = container.getBoundingClientRect();
        var x = (e.pageX || e.touches[0].pageX) - rect.left;
        var width = Math.min(Math.max(0, x), rect.width);
        overlay.style.width = width + "px";
        slider.style.left = width + "px";
    };

    slider.addEventListener('mousedown', function(e) {
        isDown = true;
        drag(e);
    });

    window.addEventListener('mouseup', function() {
        isDown = false;
    });

    container.addEventListener('mousemove', drag);
    container.addEventListener('touchstart', function(e) {
        isDown = true;
        drag(e);
    }, {passive: true});

    container.addEventListener('touchend', function() {
        isDown = false;
    });

    container.addEventListener('touchmove', drag, {passive: true});
});
</script>

<div id="image-compare-container" class="image-compare-container">
    <img src="assets/img/Left/L2.png" alt="Image 1" class="image-compare-image">
    <div class="image-compare-overlay" style="width: 50%;">
        <img src="assets/img/Right/R2.png" alt="Image 2" class="image-compare-image">
        <div class="image-compare-slider"></div>
    </div>
</div>
