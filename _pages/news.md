---
layout: page
title: News
permalink: /news/
nav: true
nav_order: 4
---
<style>
body, html {
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
}

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

<div class="timeline">
    <div class="container left">
        <div class="content">
            <h2>2024</h2>
            <p>This is some text about the news article. This can be expanded to show more details.</p>
        </div>
    </div>
    <div class="container right">
        <div class="content">
            <h2>2023</h2>
            <p>This is some text about the news article. This can be expanded to show more details.</p>
        </div>
    </div>
    <!-- Repeat the container divs for each news article -->
</div>

<script type='text/javascript'>
    document.addEventListener('DOMContentLoaded', (event) => {
    const expandables = document.querySelectorAll('.content');
    expandables.forEach((item) => {
        item.addEventListener('click', function() {
            // Toggle expanded class to change height or show more content
            this.classList.toggle('expanded');
            // You might want to change this to adjust the display of expanded content
        });
    });
});
</script>


{% include news.liquid %}