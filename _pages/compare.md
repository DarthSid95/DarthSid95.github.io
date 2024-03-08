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
    width: 10px; /* Make the slider thicker for easier interaction */
    height: 100%;
    background-color: #2196F3; /* Slider color */
    left: 50%; /* Initial position */
    /* Add a shadow or border for better visibility */
    box-shadow: 0 0 5px #000;
}
</style>

<script type="text/javascript">
document.addEventListener('DOMContentLoaded', function() {
    // Initialize image selection
    var baseImages = ['image1_base.jpg', 'image2_base.jpg', 'image3_base.jpg', 'image4_base.jpg'];
    var overlayImages = ['image1_overlay.jpg', 'image2_overlay.jpg', 'image3_overlay.jpg', 'image4_overlay.jpg'];

    function selectRandomImage(imageArray) {
        var index = Math.floor(Math.random() * imageArray.length);
        return imageArray[index];
    }

    document.getElementById('base-image').src = selectRandomImage(baseImages);
    document.getElementById('overlay-image').src = selectRandomImage(overlayImages);

    // Slider functionality
    var container = document.getElementById('image-compare-container');
    var slider = container.querySelector('.image-compare-slider');
    var overlay = container.querySelector('.image-compare-overlay');
    var isDragging = false;

    var dragStart = function(e) {
        isDragging = true;
        // Use preventDefault to avoid selecting text/images on drag
        e.preventDefault();
    };

    var dragMove = function(e) {
        if (!isDragging) return;
        var rect = container.getBoundingClientRect();
        var xPos = e.pageX - rect.left; // Get the x position within the container
        // Adjust for touch devices
        if (e.touches) {
            xPos = e.touches[0].pageX - rect.left;
        }

        // Clamp the position of the slider within the container
        var position = Math.max(0, Math.min(xPos, rect.width));

        overlay.style.width = position + "px";
        slider.style.left = position + "px";
    };

    var dragEnd = function() {
        isDragging = false;
    };

    // Attach mouse events
    slider.addEventListener('mousedown', dragStart);
    window.addEventListener('mousemove', dragMove);
    window.addEventListener('mouseup', dragEnd);

    // Attach touch events
    container.addEventListener('touchstart', dragStart, {passive: true});
    container.addEventListener('touchmove', dragMove, {passive: true});
    container.addEventListener('touchend', dragEnd);
});

</script>

<div id="image-compare-container" class="image-compare-container">
    <img src="/assets/img/left/L2.png" alt="Image 1" class="image-compare-image">
    <div class="image-compare-overlay" style="width: 50%;">
        <img src="/assets/img/right/R2.png" alt="Image 2" class="image-compare-image">
        <div class="image-compare-slider"></div>
    </div>
</div>
