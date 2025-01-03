---
layout: page
title: <i class="fa-solid fa-brain"></i> AI
permalink: /compare/
description: <h1> Me vs. The Machine </h1> <br> The image juxtaposes my photography againt one generated by GPT-4 using a caption that GPT-4 itself generated, when prompted with my photography. <br> Click/Tap anywhere on the image to change the percentage of how much of each image is shown. <br> Click on the \Random\ button to generate a random image, or cycle thru them all sequentyally (Current count - 26 Pairs) <br> Fun Fact -- GPT4 coded all the logic (JS, CSS, HTML) for this page!
nav: true
nav_order: 9
---

<style>
.image-comparison-wrapper {
    position: relative;
    display: flex; /* Change to inline-flex to wrap content more tightly */
    flex-direction: column;
    align-items: center; /* Center children horizontally */
    justify-content: center; /* Center the entire wrapper vertically, if you're using it for full page */
    min-height: 100vh; /* Optional: For full-page height alignment */
    padding: 10px; /* Adds some space around the content */
}

.image-caption {
    background-color: rgba(255, 255, 255, 0.7); /* Semi-transparent white background for legibility */
    color: #333; /* Dark text for contrast */
    padding: 5px 10px;
    border-radius: 5px;
    font-size: 16px;
    margin-bottom: 5px; /* Space between caption and image comparison tool */
}

.image-compare-container {
    position: relative;
    width: 512px; /* Set to the specific width of your images */
    margin: auto; /* Centers the container horizontally */
}

.image-compare-image {
    display: block;
    width: 512px; /* Width of the images */
    height: auto; /* Maintain aspect ratio */
}

.image-compare-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 256px; /* Starting point of the slider at halfway */
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
    left: 256px; /* Initial position at the halfway point */
    box-shadow: 0 0 5px #000;
}

/* Style adjustments for arrow buttons to overlap the image edges */
.arrow-button {
    position: absolute;
    top: 50%;
    transform: translateY(-50%) translateX(10%); /* Adjust translation if needed */
    cursor: pointer;
    padding: 10px;
    background-color: rgba(100, 100, 100, 0.75); /* Adjust color/transparency as desired */
    color: white;
    border: none;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    font-size: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    z-index: 5; /* Ensure they are above the image */
}

#arrow-left {
    left: 0; /* Adjust for overlap */
}

#arrow-right {
    right: 0; /* Adjust for overlap */
}

/* Reverting "Random Image" button style */
#random-image {
    cursor: pointer;
    padding: 10px 15px;
    background-color: #2196F3; /* Solid color for a standard look */
    color: white;
    border: 1px solid #ddd; /* Optional: adds a slight border */
    border-radius: 4px; /* Rounded corners for a classic button look */
    font-size: 16px;
    margin: 10px 0; /* Space above the button */
}

</style>

<script type="text/javascript">
document.addEventListener('DOMContentLoaded', function() {
    // Arrays of image sources for base and overlay images
    var overlayImages = [
        '/assets/img/left/L1.png',
        '/assets/img/left/L2.png',
        '/assets/img/left/L3.png',
        '/assets/img/left/L4.png',
        '/assets/img/left/L5.png',
        '/assets/img/left/L6.png',
        '/assets/img/left/L7.png',
        '/assets/img/left/L8.png',
        '/assets/img/left/L9.png',
        '/assets/img/left/L10.png',
        '/assets/img/left/L11.png',
        '/assets/img/left/L12.png',
        '/assets/img/left/L13.png',
        '/assets/img/left/L14.png',
        '/assets/img/left/L15.png',
        '/assets/img/left/L16.png',
        '/assets/img/left/L17.png',
        '/assets/img/left/L18.png',
        '/assets/img/left/L19.png',
        '/assets/img/left/L20.png',
        '/assets/img/left/L21.png',
        '/assets/img/left/L22.png',
        '/assets/img/left/L23.png',
        '/assets/img/left/L24.png',
        '/assets/img/left/L25.png',
        '/assets/img/left/L26.png'
    ];
    var baseImages = [
        '/assets/img/right/R1.png',
        '/assets/img/right/R2.png',
        '/assets/img/right/R3.png',
        '/assets/img/right/R4.png',
        '/assets/img/right/R5.png',
        '/assets/img/right/R6.png',
        '/assets/img/right/R7.png',
        '/assets/img/right/R8.png',
        '/assets/img/right/R9.png',
        '/assets/img/right/R10.png',
        '/assets/img/right/R11.png',
        '/assets/img/right/R12.png',
        '/assets/img/right/R13.png',
        '/assets/img/right/R14.png',
        '/assets/img/right/R15.png',
        '/assets/img/right/R16.png',
        '/assets/img/right/R17.png',
        '/assets/img/right/R18.png',
        '/assets/img/right/R19.png',
        '/assets/img/right/R20.png',
        '/assets/img/right/R21.png',
        '/assets/img/right/R22.png',
        '/assets/img/right/R23.png',
        '/assets/img/right/R24.png',
        '/assets/img/right/R25.png',
        '/assets/img/right/R26.png'
    ];

    var captions = [
        'The image shows a gibbon with a gentle expression, cradling a baby in its arms. Both are positioned behind the diamond-shaped links of a chain-link fence, which partially obscures them from view. The adult gibbon has a soft grey and white fur that contrasts sharply with the dark color of the metal fence. The infant is nestled closely to the adult, suggesting a sense of nurturing and protection. The fur of both gibbons looks plush, and the delicate facial features of the adult, especially around the eyes, are expressive and poignant. The scene is both touching and saddening, as it highlights the bond between parent and child while also drawing attention to their captivity.',
        'The image displays a textured surface composed of a mottled pattern of black, white, and varying shades of brown. This pattern suggests a natural material, such as a type of rock or mineral, possibly granite or a similar igneous rock which commonly has a granular and phaneritic texture. Additionally, there is a dragonfly resting on the surface, camouflaged against the mottled pattern. Its presence adds a biological element to the scene, contrasting with the inanimate background. The dragonfly has prominent wings with visible venation patterns, typical of dragonfly morphology.',
        'The image presents a cityscape viewed from a high vantage point. It is a cloudy day, and the clouds appear thick and textured, allowing only some sunlight to filter through, creating a dramatic sky. Below, a river cuts through the city, with buildings flanking either side. To the left stands a large white building with a unique, sloped architectural design. Across the river, there are more buildings, possibly residential or office spaces, with a standard blocky appearance typical of urban structures. The streets are organized in a grid layout, with a few cars visible, implying moderate traffic. The scene captures a balance between natural grandeur and the human-made environment.',
        'The image depicts a wooden boardwalk meandering through a natural landscape under an overcast sky. The boardwalk is constructed of weathered wooden planks and stretches straight into the distance before curving to the left. On either side of the boardwalk, the ground is covered with low-lying vegetation, shrubs, and scattered trees that appear to be in a state of early autumnal transition or perhaps a post-summer dormancy. The sky is dramatic, filled with thick, textured clouds that allow some sunlight to filter through, creating a moody atmosphere. The lighting is subdued, with the clouds casting a soft shadow over the entire scene, giving it a serene yet slightly ominous feel.',
        'The image shows a happy, fluffy white dog sitting atop a large log in a sunny garden. The log is cut at an angle, with the fresh, lighter wood visible at the cut end, indicating it may have been recently chopped. The bark is rugged and dark, offering a textural contrast to the dogs soft fur. The dog appears to be mid-sized, with long, wavy fur, and it looks content, with its mouth slightly open as if panting or smiling. Behind the dog, there is a neat lawn, a white picket fence, some bushes, and a variety of other green plants, suggesting a well-maintained backyard. The scene is bathed in natural light, creating a warm and peaceful atmosphere.',
        'The image features a close-up of an owls face, showcasing its detailed plumage and captivating gaze. The owl has a rounded head with a mix of brown and beige feathers that create a mottled pattern, which serves as excellent camouflage in woodland habitats. Its eyes are large, round, and dark, reflecting the environment and giving the owl a wise and intense look. The beak is short, curved, and a light yellow color, contrasting with the darker feathers surrounding it. The feathers around the beak and eyes are slightly fluffed, adding texture to the image. The owls direct stare gives it a sense of curiosity and intelligence, typical of these nocturnal birds of prey.',
        'The image captures an atmospheric scene of a blue vintage steam locomotive in the midst of operation, emitting a dense plume of dark smoke into the air. The smoke billows from the locomotives chimney, filling the grey sky and obscuring part of the surrounding area. The train, labeled with the number 804 and the word "VALIANT," appears to be stationary or moving slowly, with steam and smoke suggesting it is working hard. Snow or ash lightly dusts the ground, and the weather appears cold and damp, enhancing the steam effect. The surroundings are dimly lit, implying either dusk, dawn, or overcast weather conditions. There is a faint hint of trees in the background, which, along with the trains vintage design, evokes a bygone era of rail travel.',
        'The image captures a silhouette of a tugboat on the water during what appears to be either sunrise or sunset. The sun hangs low in the hazy sky, casting a soft golden glow and reflecting on the waters surface. The boat is facing towards the left, and its details are mostly obscured by backlighting, but its outline suggests a sturdy, utilitarian design characteristic of a working vessel. In the background, there is a silhouette of a landscape that includes palm trees and a building with a prominent spire, possibly a mosque or church, contributing to a serene maritime scene. The atmosphere is peaceful, and the water is relatively calm, with gentle ripples.',
        'The image is a close-up portrait of a peacocks head, showcasing its vibrant and intricate features. The birds face is a rich blue with a pattern of black and white stripes around its eyes, which are dark and attentive. A crown of delicate, wispy feathers fans out from the top of its head, and behind, the suggestion of the peacocks iconic iridescent train can be seen. The peacocks beak is short, curved, and a pale ivory color. The background is a soft, blurred green, indicating a natural environment.',
        'The image shows a langur monkey standing upright on its hind legs in a grassy area. Its fur is a mix of light grey and white, with a darker grey or black face surrounded by a mane of white fur. The monkeys eyes are wide and dark, and it has a black snout. Its right arm is raised as if it is reaching for something or gesturing, and its tail hangs down behind its legs. The langur appears to be in a natural habitat with green grass and some foliage in the background, which is slightly blurred. The overall mood of the image is playful and curious, capturing the langur in a human-like pose.',
        'The image depicts an expansive view of a large steel truss bridge spanning across a broad body of water. The perspective is taken from a distance, showing the bridge at an angle where it creates a diagonal line across the frame. The water is shimmering with reflections of sunlight, indicating it is a bright day. The sky is partly cloudy with scattered white clouds, but there is enough sunlight to create a sparkling effect on the waters surface. The bridges architecture features a series of interconnected steel beams forming a repeating pattern of triangles, typical of truss construction. The silhouette of the bridge is bold against the sky, illustrating an impressive feat of engineering blending with the natural environment.',
        'The image captures the striking silhouette of a traditional fishing net against a dusky sky, with the setting or rising sun positioned directly above it. The sun is a vibrant orange sphere, diffusing a warm light that fills the sky and casts a hazy glow around its outline. The net structure consists of a series of poles and cables that create a triangular shape, with the net hanging loosely between them. The composition of the nets silhouette and the sun creates a harmonious blend of natural beauty and human ingenuity, often associated with the iconic Chinese fishing nets found in places like Cochin, India.',
        'The image features a single brown leaf lying on a red brick pavement. The leaf is dry and appears to have fallen recently, as it is still whole without signs of decay. Its pointed tips and lobed edges suggest it is from a deciduous tree, commonly associated with autumn. The brick surface is in a herringbone pattern, and the mortar lines create a grid that frames the leaf. The sun casts a shadow diagonally across the image, intersecting the leaf and contrasting with the bright light illuminating the rest of the scene.',
        'The image displays a graceful egret standing amidst a bed of green lily pads. The birds plumage is a striking, pure white, which stands out against the vibrant greenery. It has a slender, black bill and long, thin legs that are partially hidden by the foliage. The egret appears to be looking off to the side, its neck curved elegantly. The lily pads have broad, round leaves that float on the waters surface, creating a dense carpet that the egret navigates. The scene is peaceful, with the egret poised delicately, suggesting it may be hunting for fish or insects.',
        'Railway tracks leading towards a small, quaint railway station nestled in a dense, green forest. The station is a simple, single-story building with yellow walls and a sloping red roof. To the right, an old, weathered, yellow cart rests by the tracks. The forest envelops the scene with a variety of trees, some with trunks close to the tracks, and the ground is covered with fallen leaves and foliage. A signal post stands to the left of the tracks, and a number placard showing 48 is visible. The atmosphere is serene and slightly misty, typical of a secluded woodland area.',
        'A little grebe, also known as a dabchick, floats on the surface of a calm body of water. Its plumage is a rich combination of chestnut-brown and grey, with a distinctive bright yellow eye and a small white patch on the cheek. The birds feathered body is round and its beak is pointed, hinting at its adept diving abilities. The water ripples gently around the grebe, reflecting its natural habitat.',
        'Two cormorants are perched on a tree branch that extends over a body of water. The cormorant in the foreground has its wings fully spread, showcasing the intricate feather patterns and the impressive wingspan, while it suns itself. The second cormorant is at rest, with its head turned towards the first, possibly observing its companion. Behind them, lush green foliage provides a natural backdrop, indicative of a rich, aquatic habitat.',
        'A man leans out of a train carriage, gazing forward, as the train awaits departure at a rural station. The station platform runs parallel to the train, lined with other passengers and railway staff in the distance. The focus on the man creates a shallow depth of field, blurring the background and imparting a sense of anticipation. The lush greenery and misty mountains in the distance suggest a cooler climate, possibly early morning.',
        'Three monkeys huddle together atop a stone structure, their fur dampened by the rain. The monkey in the foreground clings to a juvenile, offering comfort or warmth, while the third monkey, partially obscured, appears to groom the juveniles back. Their expressions are serene, evoking a sense of familial closeness and care. The backdrop is a soft focus of greenery, likely a forest, suggesting their natural habitat.',
        'In the dim light of dusk, a steam locomotives headlamp glows brightly, cutting through the gloom. Black smoke billows from the locomotives stack, while a train crew member stands on the side, looking ahead. The surrounding area is shadowy, suggesting the train is either departing or arriving during the early evening or on a heavily overcast day.',
        'A close-up of frothy water, capturing the dynamic movement of bubbles and foam. The water is alive with energy, each droplet caught in a moment of suspension, reflecting the light. The detail of the waters turbulence is crisp, highlighting the chaotic beauty of splashing water.',
        'A road and railway track run parallel, bordered by lush greenery. In the foreground, a weathered pole stands beside the track, with a road sign perched at the top, warning of a nearby crossing. The railway sleepers and metal rails reflect a history of journeys past, while the empty road curves gently into the distance, flanked by tall trees that speak to the tranquility of the location.',
        'Vivid flames engulf a pile of wooden logs, their dance casting a warm, dynamic glow. The fires intensity is palpable, with the flames twisting and curling in various shades of yellow, orange, and hints of blue at the base, suggesting a high-temperature burn. Embers float upward, carried by the heat, while the background remains shadowy, highlighting the fires brightness in contrast to its surroundings.',
        'A small bird nestles within the protective embrace of a meticulously constructed nest. The nest, woven from twigs and grass, is cradled in the foliage of a garden. The bird, with soft brown feathers and a watchful eye, seems to be resting or guarding its domain. Surrounding green leaves frame this intimate moment in nature.',
        'A green-lit wall lantern emits a soft glow in a dark environment. The lantern, with a protective metal cage around the glass, casts a green hue on the wall it is mounted to, creating a feeling of ambiance. The light appears bright within the lantern, suggesting it is the sole source of illumination in the immediate vicinity.',
        'A moody and atmospheric landscape where skeletal trees stand stark against a heavy mist. The foreground features a gnarled tree stump and a few patches of snow, evidence of a recent thaw. The desolate trees, bereft of leaves, reach into the foggy backdrop, conveying a sense of desolation and the quietude of winters end. Rocks and withered grass pepper the ground, and the subdued light suggests an overcast day, possibly dusk or dawn.'
    ]

    let currentIndex = 0; // Start at the first image
    let PageLoad = -1

    function updateImageAndCaption() {
        document.getElementById('base-image').src = baseImages[currentIndex];
        document.getElementById('overlay-image').src = overlayImages[currentIndex];
        document.getElementById('caption').textContent = captions[currentIndex];
    }

    // Random image selection
    document.getElementById('random-image').addEventListener('click', function() {
        currentIndex = Math.floor(Math.random() * baseImages.length);
        updateImageAndCaption();
    });

    // Sequential navigation
    document.getElementById('arrow-right').addEventListener('click', function() {
        currentIndex = (currentIndex + 1) % baseImages.length;
        updateImageAndCaption();
    });

    document.getElementById('arrow-left').addEventListener('click', function() {
        currentIndex = (currentIndex - 1 + baseImages.length) % baseImages.length; // Ensure positive index
        updateImageAndCaption();
    });

    if (PageLoad = -1) {
        let PageLoad = 0
        updateImageAndCaption();
    }

    // Implementing the click/touch interface for comparison
    const container = document.getElementById('image-compare-container');
    container.addEventListener('click', function(e) {
        const rect = container.getBoundingClientRect();
        const xPos = e.clientX - rect.left;
        const width = container.offsetWidth;

        const overlayWidth = Math.max(0, Math.min(xPos, width));
        container.querySelector('.image-compare-overlay').style.width = overlayWidth + 'px';
    });

});



//     // Function to select a random index
//     function selectRandomIndex(imageArray) {
//         return Math.floor(Math.random() * imageArray.length);
//     }

//     // Select a random index for both images
//     var index = selectRandomIndex(baseImages); // This index is used for both arrays

//     // Set the source for the base and overlay images using the same index
//     document.getElementById('base-image').src = baseImages[index];
//     document.getElementById('overlay-image').src = overlayImages[index];
//     document.getElementById('caption').textContent = captions[index]; // Set caption

//     // try {
//     //     const response = await fetch(captions[index]);
//     //     const text = await response.text();
//     //     document.getElementById('caption').textContent = text;
//     // } catch (error) {
//     //     console.error('Error fetching caption:', error);
//     //     document.getElementById('caption').textContent = 'Images generated By Me vs. ChatGPT';
//     // }

//     // Reference to the container, overlay, and slider elements
//     var container = document.getElementById('image-compare-container');
//     var overlay = container.querySelector('.image-compare-overlay');
//     var slider = container.querySelector('.image-compare-slider');

//     container.addEventListener('click', function(e) {
//         var rect = container.getBoundingClientRect();
//         var xPos = e.clientX - rect.left; // Calculate click position within the container

//         overlay.style.width = xPos + "px"; // Adjust overlay width, cropping the image
//         slider.style.left = xPos + "px"; // Move slider to click position
//     });
// });
</script>

<!-- <div id="caption" class="image-caption">Caption goes here</div>
<div id="image-compare-container" class="image-compare-container">
    <img id="base-image" alt="Base Image" class="image-compare-image">
    <div class="image-compare-overlay" style="width: 50%;">
        <img id="overlay-image" alt="Overlay Image" class="image-compare-image">
        <div class="image-compare-slider"></div>
    </div>
</div> -->

<div class="image-comparison-wrapper">
    <button id="random-image">Random Image</button>
    <div id="arrow-left" class="arrow-button">&lt;</div> <!-- Left arrow for sequential navigation -->
    <div id="caption" class="image-caption">Caption goes here</div>
    <div id="image-compare-container" class="image-compare-container">
        <img id="base-image" alt="Base Image" class="image-compare-image">
        <div class="image-compare-overlay" style="width: 50%;">
            <img id="overlay-image" alt="Overlay Image" class="image-compare-image">
            <div class="image-compare-slider"></div>
        </div>
    </div>
    <div id="arrow-right" class="arrow-button">&gt;</div> <!-- Right arrow for sequential navigation -->
</div>
