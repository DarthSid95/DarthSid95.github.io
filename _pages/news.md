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
    color: black;
}

.responsive-img {
width: 100%;
height: auto; /_ Maintains aspect ratio _/
}

.video-container iframe{
width=100%;
aspect-ratio: 16 / 9; /_ Adjust aspect ratio as needed _/
border: none;
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
    width: 4px;
    background: linear-gradient(180deg, #555, #999);
    transform: translateX(-50%);
    transition: height 0.4s ease;
}


.news-item {
    position: relative;
    width: 45%;
    margin-bottom: 60px;
    padding: 20px;
    background: white;
    border-radius: 10px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    opacity: 0;
    transform: translateY(30px);
    color: black;
}

.news-item.show {
opacity: 1;
transform: translateY(0);
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
clear: both;
margin-right: 10%;
}

.news-right {
float: right;
clear: both;
margin-left: 10%;
}

.news-content {
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

/* Styles when content is expanded */
.expanded .content-full {
    display: block; /* Show full content */
}


@media (max-width: 768px) {
.news-left, .news-right {
float: none;
width: 100%;
}
}
</style>

{% assign news = site.news | reverse %}

<div id="news-timeline">
    <div class="timeline-spine"></div>
    {% for item in news %}
    <div class="news-item" data-year="{{ item.date | date: '%Y' }}">
        <div class="news-content">
            <h2>{{ item.date | date: '%Y' }}</h2>
            {% if item.inline %}
                <b>{{ item.date | date: '%b %d' }}</b>&nbsp;{{ item.content | remove: '<p>' | remove: '</p>' | emojify }}
            {% else %}
                <div class="content-preview"><b>{{ item.date | date: '%b %d' }}&nbsp;{{ item.title }}</b></div>
                <div class="content-full" style="display: none;">{{ item.content | remove: '<p>' | remove: '</p>' | emojify }}</div>
                <div class="expand-arrow"><i class="fa-solid fa-chevron-down"></i></div>
            {% endif %}
        </div>
    </div>
    {% endfor %}
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
    const newsItems = document.querySelectorAll('.news-item');
    const timelineSpine = document.querySelector('.timeline-spine');

    function updateSpineHeight() {
        const maxHeight = Array.from(newsItems).reduce((max, item) => Math.max(max, item.offsetTop + item.offsetHeight), 0);
        timelineSpine.style.height = maxHeight + 'px';
    }

    newsItems.forEach((item, index) => {
        setTimeout(() => item.classList.add('show'), index * 100);
    });

    document.querySelectorAll('.expand-arrow').forEach(arrow => {
        arrow.addEventListener('click', function() {
            const content = this.parentElement;
            content.classList.toggle('expanded');
            const fullContent = content.querySelector('.content-full');
            if (fullContent.style.display === 'block') {
                fullContent.style.display = 'none';
                this.querySelector('i').classList.replace('fa-chevron-up', 'fa-chevron-down');
            } else {
                fullContent.style.display = 'block';
                this.querySelector('i').classList.replace('fa-chevron-down', 'fa-chevron-up');
            }
            setTimeout(updateSpineHeight, 300); // allow content expand to finish before recalculating
        });
    });

    newsItems.forEach((item) => {
        const year = parseInt(item.getAttribute('data-year'), 10);
        if (year % 2 === 0) {
            item.classList.add('news-left');
        } else {
            item.classList.add('news-right');
        }
    });

    updateSpineHeight();
});
</script>
