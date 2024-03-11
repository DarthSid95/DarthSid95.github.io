---
layout: page
title: News
permalink: /news/
nav: false
nav_order: 4
---

<!-- <style>
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
    justify-content: left; /* Center content */
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
</style> -->

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
    color: rgba(100, 100, 100, 0.75);
    position: relative;
    width: 40%;
    margin-bottom: 20px;
    padding: 10px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.news-left, .news-right {
    color: rgba(100, 100, 100, 0.75);
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

</style>

{% assign news = site.news | reverse %}

<div id="news-timeline">
    <div class="timeline-spine"></div> <!-- Central spine of the timeline -->
    <!-- Placeholder loop: Replace with your template engine's loop syntax -->
    {% for item in news %}
    <div class="news-item" data-year="{{ item.date | date: '%Y' }}">
        <div class="news-content">
        <h2> {{ item.date | date: '%Y' }} </h2> <br>
        <b> {{ item.date | date: '%b %d' }} </b>&nbsp;{{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
        </div>
    </div>
    {% endfor %}
</div>

<!-- <script type='text/javascript'>
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
</script>  -->

<script type='text/javascript'>
    document.addEventListener("DOMContentLoaded", function() {
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
    const timelineSpine = document.querySelector('.timeline-spine');
    timelineSpine.style.height = (maxHeight + 20) + 'px'; // +20 for a little extra space

});
</script>

<!-- {% include news.liquid %} -->
