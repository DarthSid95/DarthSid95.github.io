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
    position: relative;
    width: 40%; /* Adjust based on your preference */
    padding: 10px;
    margin-bottom: 20px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.news-content {
    padding: 10px;
    color: black;
    background-color: #fff;
    border: 1px solid #ddd;
}   /* Add more styling according to your preference */
</style>


<div id="news-timeline">
    <div class="timeline-spine"></div> <!-- Central spine of the timeline -->
    <!-- Placeholder loop: Replace with your template engine's loop syntax -->
    {% for item in site.news %}
    <div class="news-item" data-year="{{ item.date | date: '%Y' }}">
        <div class="news-content">
        <h2> {{ item.date | date: '%Y' }} </h2> <br>
        <h3> {{ item.date | date: '%b %d, %Y' }} </h3> <br>
        {{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
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