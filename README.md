
<style>
    html, body {
        margin: 0;
        padding: 0;
        font-family: 'Inter', sans-serif;
        background-color: #f0f2f5;
        height: 100%;
    }

    /* Sidebar (left fixed) */
    .admin-sidebar {
        width: 250px;
        height: 100vh;
        position: fixed;
        top: 0;
        left: 0;
        background: linear-gradient(135deg, #2c3e50, #34495e);
        color: #ecf0f1;
        padding: 30px 20px;
        box-shadow: 2px 0 10px rgba(0,0,0,0.1);
    }

    .admin-sidebar h3 {
        font-size: 24px;
        font-weight: 600;
        margin-bottom: 40px;
        text-align: center;
    }

    .admin-sidebar nav a {
        display: block;
        padding: 10px 15px;
        margin-bottom: 10px;
        color: #ecf0f1;
        text-decoration: none;
        border-radius: 6px;
    }

    .admin-sidebar nav a:hover {
        background-color: rgba(255,255,255,0.1);
    }

    /* Main Content (next to sidebar) */
    .admin-main-content {
        margin-left: 250px; /* To the right of the sidebar */
        padding: 40px;
        min-height: 100vh;
    }

    .admin-main-content h2 {
        font-size: 28px;
        color: #2c3e50;
        margin-bottom: 10px;
    }

    .admin-main-content p {
        font-size: 16px;
        color: #555;
        margin-bottom: 20px;
    }

    .statistics-card {
        background-color: #fff;
        border-radius: 10px;
        padding: 30px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.05);
    }

    .statistics-card h3 {
        font-size: 22px;
        text-align: center;
        margin-bottom: 30px;
        color: #333;
        border-bottom: 1px solid #ccc;
        padding-bottom: 10px;
    }

    /* Chart Container */
    .bar-chart-container {
        position: relative;
        height: 320px;
        padding-left: 50px;
        border-bottom: 2px solid #ccc;
    }

    .y-axis-labels {
        position: absolute;
        left: 0;
        top: 0;
        bottom: 30px;
        width: 40px;
        font-size: 12px;
        color: #777;
        text-align: right;
        padding-right: 8px;
    }

    .y-axis-labels div {
        position: absolute;
        right: 0;
        transform: translateY(-50%);
    }

    .y-label-max { top: 0; transform: none; }
    .y-label-75 { top: 25%; }
    .y-label-50 { top: 50%; }
    .y-label-25 { top: 75%; }
    .y-label-0 { bottom: 0; top: auto; transform: none; }

    .y-axis-labels div::after {
        content: '';
        position: absolute;
        top: 50%;
        right: -10px;
        width: 10px;
        height: 1px;
        background-color: #ccc;
    }

    .chart-grid {
        margin-left: 50px; /* to avoid y-axis */
        height: 300px;
        overflow: hidden;
    }

    .chart-bar-wrapper {
        float: left;
        width: 12%; /* adjust based on number of bars */
        text-align: center;
        position: relative;
        height: 100%;
    }

    .chart-bar {
        position: absolute;
        bottom: 25px; /* leave space for label */
        left: 20%;
        width: 60%;
        background-color: #3498db;
        border-radius: 5px 5px 0 0;
        transition: height 0.3s ease;
    }

    .chart-bar:hover {
        background-color: #2980b9;
    }

    .bar-tooltip {
        position: absolute;
        bottom: 100%;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0,0,0,0.8);
        color: #fff;
        font-size: 12px;
        padding: 4px 8px;
        border-radius: 4px;
        white-space: nowrap;
        opacity: 0;
        pointer-events: none;
        transition: opacity 0.3s ease;
    }

    .chart-bar-wrapper:hover .bar-tooltip {
        opacity: 1;
    }

    .x-axis-label {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        font-size: 12px;
        color: #555;
        white-space: nowrap;
    }

    .no-data-message {
        text-align: center;
        padding: 40px;
        font-size: 18px;
        color: #777;
    }
</style>
