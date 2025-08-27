<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>多狹縫干涉模擬</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 20px;
        }
        canvas {
            border: 1px solid #000;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>多狹縫干涉實驗模擬</h1>
    <p>調整以下參數來觀察多狹縫干涉條紋的變化：</p>
    
    <label for="numSlits">狹縫數量: </label>
    <input type="number" id="numSlits" value="2" min="2" max="10">
    <br>
    <label for="slitDistance">狹縫間距 (單位: nm): </label>
    <input type="number" id="slitDistance" value="500" min="100" max="1000">
    <br>
    <label for="wavelength">光波長 (單位: nm): </label>
    <input type="number" id="wavelength" value="500" min="200" max="700">
    <br>
    <label for="screenDistance">屏幕距離 (單位: m): </label>
    <input type="number" id="screenDistance" value="2" min="1" max="5">
    <br>
    <button onclick="updateInterference()">更新模擬</button>

    <canvas id="canvas" width="800" height="400"></canvas>

    <script>
        const canvas = document.getElementById("canvas");
        const ctx = canvas.getContext("2d");

        // 參數
        let numSlits = 2;
        let slitDistance = 500; // nm
        let wavelength = 500; // nm
        let screenDistance = 2; // m

        // 畫干涉條紋
        function drawInterference() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = "#000";
            const centerY = canvas.height / 2;
            const spacing = canvas.width / 20;

            // 計算干涉條紋的間距
            const d = slitDistance * 1e-9; // 狹縫間距轉為米
            const lambda = wavelength * 1e-9; // 波長轉為米
            const L = screenDistance; // 屏幕距離

            // 干涉條紋的距離公式
            const fringeSpacing = (lambda * L) / d;

            // 設置干涉條紋的位置
            for (let i = -Math.floor(canvas.width / (2 * fringeSpacing)); i <= Math.floor(canvas.width / (2 * fringeSpacing)); i++) {
                const positionX = i * fringeSpacing + canvas.width / 2;
                const brightness = Math.abs(Math.sin(Math.PI * i * numSlits / 2)); // 根據狹縫數量調整亮度
                ctx.fillStyle = `rgba(0, 0, 0, ${brightness})`;
                ctx.fillRect(positionX, centerY - 10, 2, 20); // 畫出干涉條紋
            }
        }

        // 更新參數並重繪
        function updateInterference() {
            numSlits = parseInt(document.getElementById("numSlits").value);
            slitDistance = parseInt(document.getElementById("slitDistance").value);
            wavelength = parseInt(document.getElementById("wavelength").value);
            screenDistance = parseFloat(document.getElementById("screenDistance").value);
            drawInterference();
        }

        // 初始化
        drawInterference();
    </script>
</body>
</html>
