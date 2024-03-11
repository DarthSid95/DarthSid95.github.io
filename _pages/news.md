---
layout: page
title: News
permalink: /news/
nav: false
nav_order: 4
---
<style>
.timeline {
    position: relative;
    max-width: 1200px;
    margin: 40px auto;
}

.timeline::after {
    content: '';
    position: absolute;
    width: 2px; /* Slimmer line for a more refined look */
    background-color: #ddd;
    top: 0;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
}

.container {
    padding: 10px 40px;
    position: relative;
    background-color: inherit;
    width: 50%;
}

.left {
    left: 0;
}

.right {
    left: 50%;
}

.content {
    padding: 20px 30px;
    background-color: white;
    position: relative;
    border-radius: 6px;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2); /* Add shadow for depth */
    color: black;
}

/* Adjust the position of the content boxes */
.left .content {
    margin-left: 0px;
    margin-right: auto;
}

.right .content {
    margin-left: auto;
    margin-right: 0;
}

/* Position the timeline dots correctly */
.container::after {
    content: '';
    position: absolute;
    width: 20px;
    height: 20px;
    background-color: white;
    border: 4px solid #ddd;
    top: 15px;
    border-radius: 50%;
    z-index: 1;
    transform: translateX(-50%);
}

.left::after {
    left: 50%;
}

.right::after {
    left: 50%;
}

@media screen and (max-width: 600px) {
    .container {
        width: 100%;
        padding-left: 70px; /* Adjust as needed for smaller screens */
        padding-right: 25px; /* Adjust as needed for smaller screens */
    }
    .left::after, .right::after {
        left: 0;
    }
    .left .content, .right .content {
        margin-left: 0;
        margin-right: 0;
    }
}
    /* Add more styling according to your preference */
</style>

<!-- <div class="timeline">
    <div class="container left">
        <div class="content">
            <h2>2024</h2>
            <p>This is some text about the news article. This can be expanded to show more details.</p>
        </div>
    </div>
    {% for item in site.news limit: news_limit %}
        <div class="container right">
            <div class="content">
                <h2> {{ item.date | date: '%Y' }} </h2> <br>
                <h3> {{ item.date | date: '%b %d, %Y' }} </h3> <br>
                {{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
            </div>
    {% endfor %}
</div> -->

<div id="timeline"></div>

<script type='text/javascript'>
    var newsData = {{ site.news | jsonify }};
    document.addEventListener('DOMContentLoaded', () => {
        const timeline = document.getElementById('timeline');
        
        // Use the `newsData` variable directly in your JavaScript
        newsData.forEach(item => {
            const year = parseInt(item.date.split('-')[0]); // Extract the year
            const isEvenYear = year % 2 === 0;

            const timelineItem = document.createElement('div');
            timelineItem.classList.add('timeline-item', isEvenYear ? 'right' : 'left');
            
            const timelineContent = document.createElement('div');
            timelineContent.classList.add('timeline-content');
            
            timelineContent.innerHTML = `<h3>${item.date}</h3><p>${item.content}</p>`;
            
            timelineItem.appendChild(timelineContent);
            timeline.appendChild(timelineItem);
        });
    });
</script>


{% include news.liquid %}