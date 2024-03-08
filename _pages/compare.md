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
    width: 50%; /* Initial overlay width to show both images */
    height: 100%;
    overflow: hidden;
}

.image-compare-slider {
    position: absolute;
    z-index: 9;
    cursor: pointer; /* Change to pointer to indicate clickable */
    width: 10px; /* Adjust for visibility */
    height: 100%;
    background-color: #2196F3; /* Slider color */
    left: 50%; /* Initial position */
    box-shadow: 0 0 5px #000; /* Improved visibility */
}
</style>

<script type="text/javascript">
document.addEventListener('DOMContentLoaded', function() {
    // Arrays of image sources for base and overlay images
    var baseImages = [
        '/assets/img/left/L1.png',
        '/assets/img/left/L2.png'
    ];
    var overlayImages = [
        '/assets/img/right/R1.png',
        '/assets/img/right/R2.png'
    ];

    // Function to select a random index
    function selectRandomIndex(imageArray) {
        return Math.floor(Math.random() * imageArray.length);
    }

    // Select a random index for both images
    var index = selectRandomIndex(baseImages); // This index is used for both arrays

    // Set the source for the base and overlay images using the same index
    document.getElementById('base-image').src = baseImages[index];
    document.getElementById('overlay-image').src = overlayImages[index];

    // Reference to the container, overlay, and slider elements
    var container = document.getElementById('image-compare-container');
    var overlay = container.querySelector('.image-compare-overlay');
    var slider = container.querySelector('.image-compare-slider');

    // Event listener for click events on the container
    container.addEventListener('click', function(e) {
        var rect = container.getBoundingClientRect();
        var xPos = e.clientX - rect.left; // Calculate click position within the container

        overlay.style.width = xPos + "px"; // Adjust overlay width
        slider.style.left = xPos + "px"; // Move slider to click position
    });
});
</script>

<div id="image-compare-container" class="image-compare-container">
    <!-- Placeholder for base image -->
    <img id="base-image" alt="Base Image" class="image-compare-image">
    <div class="image-compare-overlay" style="width: 50%;">
        <!-- Placeholder for overlay image -->
        <img id="overlay-image" alt="Overlay Image" class="image-compare-image">
        <div class="image-compare-slider"></div>
    </div>
</div>


<div id="image-compare-container" class="image-compare-container">
    <img src="/assets/img/left/L2.png" alt="Image 1" class="image-compare-image">
    <div class="image-compare-overlay" style="width: 50%;">
        <img src="/assets/img/right/R2.png" alt="Image 2" class="image-compare-image">
        <div class="image-compare-slider"></div>
    </div>
</div>
