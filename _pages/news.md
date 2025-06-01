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
    background: linear-gradient(180deg, #555, #999);
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
</head>
<body>

<div id="news-timeline">
    <div class="timeline-spine"></div>
    <!-- Replace below loop with template engine loop -->
    <!-- Example item -->
    <div class="news-item news-left">
        <div class="news-content">
            <h2>2025</h2>
            <div class="content-preview">Jun 01 Breaking News Headline</div>
            <div class="content-full">Full details of the breaking news. Lorem ipsum dolor sit amet.</div>
            <div class="expand-arrow"><i class="fas fa-chevron-down"></i></div>
        </div>
    </div>
    <!-- Duplicate this block for each news item, alternating left/right -->
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
    const newsItems = document.querySelectorAll('.news-item');
    const timelineSpine = document.querySelector('.timeline-spine');

    // Animate items on load
    newsItems.forEach((item, index) => {
        setTimeout(() => item.classList.add('show'), index * 100);
    });

    // Setup expand toggles
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
        });
    });

    // Alternate left/right based on year (even/odd)
    newsItems.forEach((item) => {
        const year = parseInt(item.getAttribute('data-year'), 10);
        if (year % 2 === 0) {
            item.classList.add('news-left');
        } else {
            item.classList.add('news-right');
        }
    });

    // Adjust spine height
    const maxHeight = Array.from(newsItems).reduce((max, item) => Math.max(max, item.offsetTop + item.offsetHeight), 0);
    timelineSpine.style.height = maxHeight + 'px';
});
</script>
