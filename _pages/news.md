---
layout: page
title: News
permalink: /news/
description: Best viewed on a Desktop, or in landscape.
nav: true
nav_order: 4
---

<style>
#news-timeline {
    color: black;
    position: relative;
    width: 100%;
    min-height: 1500px; /* Ensure container has a minimum height */
    padding: 20px 0;
}

#news-timeline,
#news-timeline * { /* Targets #news-timeline and all its child elements */
    color: black; /* Sets text color to black */
}


.timeline-spine {
    color: rgba(100, 100, 100, 0.75);
    position: absolute;
    left: 50%;
    top: 0;
    bottom: 0;
    width: 2px;
    background-color: rgba(100, 100, 100, 0.75);
    z-index: 1; /* Ensure it's above connecting lines */
}

.news-item {
    color: rgba(50, 50, 50, 0.5);
    position: relative;
    width: 40%;
    margin-bottom: 20px;
    padding: 10px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.news-left, .news-right {
    color: rgba(50, 50, 50, 0.5);
    clear: both;
}

.news-left::before, .news-right::before {
    content: '';
    position: absolute;
    top: 50%;
    width: 25%; /* Adjust if needed */
    height: 2px;
    background-color: #333;
    z-index: 0;
}

.news-left::before {
    left: 100%;
}

.news-right::before {
    right: 100%;
}

.news-left {
    float: left;
    margin-right: 10%;
}

.news-right {
    float: right;
    margin-left: 10%;
}

.news-content {
    background-color: #fff;
    padding: 10px;
}
.content-full {
    display: none; /* Initially hidden */
}

.expand-arrow {
    cursor: pointer;
    color: black; /* Arrow color, ensure it's visible against the background */
    text-align: center; /* Center the arrow below the content */
}

/* Styles when content is expanded */
.expanded .content-full {
    display: block; /* Show full content */
}

.expanded .expand-arrow {
    transform: rotate(180deg); /* Flip arrow to indicate collapsibility */
}
</style>

{% assign news = site.news | reverse %}

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
            <div class="content-preview"><b>{{ item.date | date: '%b %d' }}</b>&nbsp;{{ item.title }}</div>
            <!-- Hidden part of the content -->
            <div class="content-full" style="display: none;">{{ item.content | remove: '<p>' | remove: '</p>' | emojify }}</div>
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
