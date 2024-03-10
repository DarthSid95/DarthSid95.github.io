---
layout: page
title: News
permalink: /news/
nav: true
nav_order: 4
---

{% include news.liquid %}

<style>
    .timeline {
        position: relative;
        max-width: 1200px;
        margin: 0 auto;
    }

    .timeline::after {
        content: '';
        position: absolute;
        width: 6px;
        background-color: #ddd;
        top: 0;
        bottom: 0;
        left: 50%;
        margin-left: -3px;
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
    }

    .left .content {
        margin-left: 15px;
    }

    .right .content {
        margin-right: 15px;
    }

    .container::after {
        content: '';
        position: absolute;
        width: 25px;
        height: 25px;
        right: -17px;
        background-color: white;
        border: 4px solid #ddd;
        top: 15px;
        border-radius: 50%;
        z-index: 1;
    }

    .right::after {
        left: -17px;
    }

    /* Add shadows to create the "card" effect */
    .content {
        box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
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
</body>
</html>
