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
    max-width: 1024px; /* Adjust as needed */
    overflow: hidden; /* Ensure nothing spills outside the container */
}

.image-compare-image {
    height: 100%; /* Ensure images always take up 100% of the container's height */
    width: auto; /* Adjust width automatically to maintain aspect ratio */
    display: block;
}

.image-compare-overlay {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%; /* Overlay takes full height of the container */
    overflow: hidden; /* Crop the overlay image based on the container's width */
}

.image-compare-slider {
    position: absolute;
    z-index: 9;
    cursor: pointer;
    width: 10px;
    height: 100%;
    background-color: #2196F3;
    left: 50%; /* Initial position */
    box-shadow: 0 0 5px #000;
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

    container.addEventListener('click', function(e) {
        var rect = container.getBoundingClientRect();
        var xPos = e.clientX - rect.left; // Calculate click position within the container

        overlay.style.width = xPos + "px"; // Adjust overlay width, cropping the image
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
