<style>
    html, body {
        margin: 0;
        padding: 0;
        height: 100%;
        font-family: 'Inter', sans-serif;
        background-color: #f0f2f5;
    }

    /* Clearfix fix */
    .clearfix::after {
        content: "";
        display: table;
        clear: both;
    }

    /* Sidebar */
    .admin-sidebar {
        float: left;
        width: 250px;
        height: 100vh;
        background: linear-gradient(135deg, #2c3e50, #34495e);
        color: #ecf0f1;
        padding: 30px 20px;
        box-shadow: 2px 0 10px rgba(0,0,0,0.1);
    }

    .admin-sidebar h3 {
        text-align: center;
        font-size: 24px;
        margin-bottom: 30px;
        font-weight: bold;
    }

    .admin-sidebar nav a {
        display: block;
        padding: 10px 15px;
        margin-bottom: 10px;
        background: transparent;
        text-decoration: none;
        color: #ecf0f1;
        border-radius: 5px;
    }

    .admin-sidebar nav a:hover {
        background-color: rgba(255,255,255,0.1);
    }

    /* Main Content */
    .admin-main-content {
        margin-left: 250px;
        padding: 40px;
        min-height: 100vh;
        overflow-y: auto;
    }

    .admin-main-content h2 {
        font-size: 28px;
        margin-bottom: 10px;
        color: #2c3e50;
    }

    .admin-main-content p {
        font-size: 16px;
        color: #555;
        margin-bottom: 20px;
    }

    .statistics-card {
        background-color: #ffffff;
        border-radius: 12px;
        padding: 30px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.05);
    }

    .statistics-card h3 {
        font-size: 22px;
        text-align: center;
        margin-bottom: 30px;
        border-bottom: 1px solid #ccc;
        padding-bottom: 10px;
        color: #333;
    }

    /* Bar Chart */
    .bar-chart-container {
        position: relative;
        height: 300px;
        padding-left: 50px;
        padding-bottom: 40px;
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

    .y-axis-labels .y-label-max { top: 0; transform: none; }
    .y-axis-labels .y-label-75 { top: 25%; }
    .y-axis-labels .y-label-50 { top: 50%; }
    .y-axis-labels .y-label-25 { top: 75%; }
    .y-axis-labels .y-label-0 { bottom: 0; top: auto; transform: none; }

    .y-axis-labels div::after {
        content: '';
        position: absolute;
        top: 50%;
        right: -10px;
        width: 10px;
        height: 1px;
        background-color: #ddd;
    }

    .chart-grid {
        position: absolute;
        top: 0;
        left: 50px;
        right: 0;
        bottom: 30px;
    }

    .chart-bar-wrapper {
        float: left;
        width: 13%;
        margin-right: 1.5%;
        position: relative;
        height: 100%;
        text-align: center;
    }

    .chart-bar-wrapper:last-child {
        margin-right: 0;
    }

    .chart-bar {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        background-color: #3498db;
        border-radius: 5px 5px 0 0;
        transition: height 0.3s ease-in-out;
    }

    .chart-bar:hover {
        background-color: #2980b9;
    }

    .bar-tooltip {
        position: absolute;
        bottom: 105%;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0,0,0,0.8);
        color: #fff;
        padding: 4px 8px;
        font-size: 12px;
        border-radius: 4px;
        white-space: nowrap;
        opacity: 0;
        transition: opacity 0.3s ease;
        pointer-events: none;
    }

    .chart-bar-wrapper:hover .bar-tooltip {
        opacity: 1;
    }

    .x-axis-label {
        position: absolute;
        bottom: -25px;
        left: 50%;
        transform: translateX(-50%);
        font-size: 12px;
        color: #555;
        width: 100%;
        text-align: center;
    }

    .no-data-message {
        text-align: center;
        padding: 40px;
        font-size: 18px;
        color: #777;
    }
</style>
