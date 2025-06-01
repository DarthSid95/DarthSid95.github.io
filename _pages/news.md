---
layout: page
title: News
permalink: /news/
description: Best viewed on a Desktop, or in landscape.
nav: true
nav_order: 5
---
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
<style>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background: #f4f4f4;
}

#news-timeline {
    position: relative;
    width: 90%;
    max-width: 1200px;
    margin: 50px auto;
    padding: 20px;
}

.timeline-spine {
    position: absolute;
    left: 50%;
    top: 0;
    bottom: 0;
    width: 4px;
    background: linear-gradient(180deg, #555, #000);
    transform: translateX(-50%);
}

.news-item {
    position: relative;
    width: 45%;
    margin-bottom: 40px;
    padding: 20px;
    background: white;
    border-radius: 10px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    opacity: 0;
    transform: translateY(30px);
}

.news-item.show {
    opacity: 1;
    transform: translateY(0);
}

.news-left {
    float: left;
    clear: both;
}

.news-right {
    float: right;
    clear: both;
}

.news-content h2 {
    margin-top: 0;
    font-size: 1.2em;
    background-color: #fff;
    color: #333;
}

.content-preview {
    font-weight: bold;
    margin-bottom: 10px;
}

.content-full {
    display: none;
    margin-top: 10px;
}

.expand-arrow {
    text-align: center;
    cursor: pointer;
    font-size: 1.2em;
    color: #007BFF;
    margin-top: 10px;
    transition: transform 0.3s ease;
}

.expanded .expand-arrow {
    transform: rotate(180deg);
}

@media (max-width: 768px) {
    .news-left, .news-right {
        float: none;
        width: 100%;
    }
}
</style>

<div id="news-timeline">
    <div class="timeline-spine"></div> <!-- Central spine of the timeline -->
    <!-- Placeholder loop: Replace with your template engine's loop syntax -->
    {% for item in news %}
    {% if item.inline %}
    <div class="news-item" data-year="{{ item.date | date: '%Y' }}">
        <div class="news-content">
        <h2> {{ item.date | date: '%Y' }} </h2> <br>
        <b> {{ item.date | date: '%b %d' }} </b>&nbsp;{{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
        </div>
    </div>
     {% else %}
     <div class="news-item" data-year="{{ item.date | date: '%Y' }}">
        <div class="news-content">
            <h2>{{ item.date | date: '%Y' }}</h2>
            <!-- The content before the delimiter goes here -->
            <div class="content-preview"><b>{{ item.date | date: '%b %d' }}&nbsp;{{ item.title }}</b></div>
            <!-- Hidden part of the content -->
            <div class="content-full" style="display: none;">{{ | item.content | remove: '<p>' | remove: '</p>' | emojify }}</div>
            <!-- Clickable arrow for expanding -->
            <div class="expand-arrow"><i class="fa-solid fa-chevron-down"></i></div>
        </div>
    </div>
     {% endif %}
    {% endfor %}
</div>

<script type='text/javascript'>
    document.addEventListener("DOMContentLoaded", function() {
    const timelineSpine = document.querySelector('.timeline-spine');
    // Event delegation on the #news-timeline for dynamic content
    document.getElementById('news-timeline').addEventListener('click', function(event) {
        // Check if the clicked element is the expand-arrow or its descendant
        if (event.target.closest('.expand-arrow')) {
            // Find the .content-full sibling of the clicked arrow
            const newsContent = event.target.closest('.news-content');
            const contentFull = newsContent.querySelector('.content-full');
            const contentPrev = newsContent.querySelector('.content-preview');
            const expandArrow = event.target.closest('.expand-arrow');
            let icon = expandArrow.querySelector('i');
            let InitnewBlockHeight = contentFull.offsetHeight;

            // var year = parseInt(item.getAttribute('data-year'), 10);
            // if(year % 2 === 0) {

            // }
            // Toggle visibility based on the current display style
            if (contentFull.style.display === 'none' || contentFull.style.display === '') {
                contentFull.style.display = 'block'; // Show the full content
                icon.className = 'fa-solid fa-chevron-up';// Optional: change the arrow direction
            } else {
                contentFull.style.display = 'none'; // Hide the full content
                icon.className = 'fa-solid fa-chevron-down';
            }
            // Adjust the timeline spine height
            let newBlockHeight = contentFull.offsetHeight;
            console.log('InitFullHeight:', InitnewBlockHeight, 'SmallHeight:', contentPrev.offsetHeight, 'FullHeight:', contentFull.offsetHeight);
            timelineSpine.style.height = (timelineSpine.style.height + 50 + newBlockHeight) + 'px'; // +20 for a little extra space
        }
    });

    const newsItems = document.querySelectorAll('.news-item');
    let maxHeight = 0;

    newsItems.forEach(item => {
        var year = parseInt(item.getAttribute('data-year'), 10);
        if(year % 2 === 0) {
            // Even year, goes to the left
            item.classList.add('news-left');
        } else {
            // Odd year, goes to the right
            item.classList.add('news-right');
        }
    });

    newsItems.forEach(function(item) {
        // Calculate the bottom position of each news item
        let itemBottom = item.offsetTop + item.offsetHeight;
        if (itemBottom > maxHeight) {
            maxHeight = itemBottom;
        }
    });

    // Adjust the timeline spine height
    timelineSpine.style.height = (maxHeight + 50) + 'px'; // +20 for a little extra space

});
</script>

