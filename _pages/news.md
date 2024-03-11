---
layout: page
title: News
permalink: /news/
nav: false
nav_order: 4
---

<style>
#news-timeline {
    position: relative;
    width: 100%;
    padding: 40px 0; /* Increased padding for visual clarity */
}

.timeline-spine {
    position: absolute;
    left: 50%;
    top: 0;
    bottom: 0;
    width: 2px; /* Adjusted for a slimmer spine */
    background-color: #333;
    z-index: 1; /* Ensure spine is above connectors but below news items */
}

.news-item {
    position: relative;
    display: flex;
    justify-content: center; /* Center content */
    width: 40%;
    margin: 20px auto; /* Auto margins for horizontal centering */
    padding: 10px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    background-color: #fff;
    border: 1px solid #ddd;
}

.news-content {
    padding: 10px;
}

.connector {
    position: absolute;
    width: 2px;
    background-color: #333;
    z-index: 0; /* Ensure connectors don't overlap news items */
}
</style>

{% assign news = site.news | reverse %}

<div id="news-timeline">
    <div class="timeline-spine"></div> <!-- Central spine of the timeline -->
    <!-- Placeholder loop: Replace with your template engine's loop syntax -->
    {% for item in news %}
    <div class="news-item" data-year="{{ item.date | date: '%Y' }}">
        <div class="news-content">
        <h2 color='black'> {{ item.date | date: '%Y' }} </h2> <br>
        <h4 color='black'> {{ item.date | date: '%b %d' }} </h4>:&nbsp;{{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
        </div>
    </div>
    {% endfor %}
</div>

<script type='text/javascript'>
document.addEventListener("DOMContentLoaded", function() {
    var newsItems = document.querySelectorAll('.news-item');

    newsItems.forEach(function(item) {
        var year = parseInt(item.getAttribute('data-year'), 10);
        if(year % 2 === 0) {
            // Even year, goes to the left
            item.style.right = "52%"; // Adjust based on the spine width
            item.style.transform = "translateX(50%)";
        } else {
            // Odd year, goes to the right
            item.style.left = "52%"; // Adjust based on the spine width
            item.style.transform = "translateX(-50%)";
        }
    });
});
</script> 

{% include news.liquid %}
