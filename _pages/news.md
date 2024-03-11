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
    padding: 20px 0;
}

.timeline-spine {
    position: absolute;
    left: 50%;
    top: 0;
    bottom: 0;
    width: 4px;
    background-color: #333;
}

.news-item {
    position: absolute; /* Updated to absolute */
    width: 40%; /* Adjust based on your preference */
    padding: 10px;
    margin-bottom: 20px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    background-color: #fff;
    border: 1px solid #ddd;
}

.news-content {
    padding: 10px;
}

.connector {
    position: absolute;
    width: 2px; /* Width of the connecting line */
    background-color: #333; /* Color of the connecting line */
    top: 50%;
}
</style>


<div id="news-timeline">
    <div class="timeline-spine"></div> <!-- Central spine of the timeline -->
    <!-- Placeholder loop: Replace with your template engine's loop syntax -->
    {% for item in site.news %}
    <div class="news-item" data-year="{{ item.date | date: '%Y' }}">
        <div class="news-content">
        <h2> {{ item.date | date: '%Y' }} </h2> <br>
        <h3> {{ item.date | date: '%b %d' }} </h3>:&nbsp;{{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
        </div>
    </div>
    {% endfor %}
</div>

<script type='text/javascript'>
document.addEventListener("DOMContentLoaded", function() {
    var newsItems = document.querySelectorAll('.news-item');
    var timelineSpine = document.querySelector('.timeline-spine');
    var spineLeftOffset = timelineSpine.offsetLeft + timelineSpine.offsetWidth / 2; // Center of the spine

    newsItems.forEach(function(item, index) {
        var year = parseInt(item.getAttribute('data-year'), 10);
        var connector = document.createElement('div');
        connector.classList.add('connector');

        if(year % 2 === 0) {
            // Even year, goes to the left
            item.style.right = (window.innerWidth - spineLeftOffset) + "px";
            connector.style.right = "50%";
            connector.style.height = "2px"; // Horizontal connector
            connector.style.width = (window.innerWidth - spineLeftOffset - item.offsetWidth / 2) + "px"; // Adjust connector width
        } else {
            // Odd year, goes to the right
            item.style.left = spineLeftOffset + "px";
            connector.style.left = "50%";
            connector.style.height = "2px"; // Horizontal connector
            connector.style.width = (spineLeftOffset - item.offsetWidth / 2) + "px"; // Adjust connector width
        }

        item.appendChild(connector);
    });
});
</script>

{% include news.liquid %}