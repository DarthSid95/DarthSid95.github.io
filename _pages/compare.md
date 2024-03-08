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
    var isDragging = false;

    var dragStart = function(e) {
        isDragging = true;
        // Prevent any text selection during the drag
        e.preventDefault();
    };

    var dragEnd = function() {
        isDragging = false;
    };

    var dragMove = function(e) {
        if (!isDragging) return;
        e.preventDefault();
        var rect = container.getBoundingClientRect();
        var x = 0;
        // Determine if it's a touch event
        if (e.type === 'touchmove') {
            x = e.touches[0].clientX - rect.left;
        } else {
            x = e.clientX - rect.left;
        }
        var width = Math.max(0, Math.min(x, rect.width));
        overlay.style.width = width + 'px';
        slider.style.left = width + 'px';
    };

    // Event listeners for mouse
    slider.addEventListener('mousedown', dragStart);
    window.addEventListener('mouseup', dragEnd);
    window.addEventListener('mousemove', dragMove);

    // Event listeners for touch
    container.addEventListener('touchstart', dragStart, { passive: true });
    container.addEventListener('touchend', dragEnd);
    container.addEventListener('touchmove', dragMove, { passive: true });
});
</script>

<div id="image-compare-container" class="image-compare-container">
    <img src="/assets/img/left/L2.png" alt="Image 1" class="image-compare-image">
    <div class="image-compare-overlay" style="width: 50%;">
        <img src="/assets/img/right/R2.png" alt="Image 2" class="image-compare-image">
        <div class="image-compare-slider"></div>
    </div>
</div>
