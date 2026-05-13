<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>DLTRS V100.0 - 終極核武終端</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body { background: #000; color: #00ff41; font-family: 'Orbitron', sans-serif; margin: 0; overflow: hidden; }
        .cockpit { width: 100vw; height: 100vh; border: 4px solid #1a331a; box-sizing: border-box; position: relative; background: radial-gradient(circle, #050a05 0%, #000 100%); }
        .header { text-align: center; border-bottom: 2px solid #00ff41; padding: 10px; background: rgba(0, 255, 65, 0.1); }
        .config-panel { padding: 8px; background: rgba(0, 255, 65, 0.05); display: flex; justify-content: center; align-items: center; gap: 8px; border-bottom: 1px solid #1a331a; }
        input { background: #000; border: 1px solid #00ff41; color: #00ff41; font-family: 'Orbitron'; width: 90px; text-align: center; font-size: 14px; }
        button { background: #00ff41; color: #000; border: none; font-family: 'Orbitron'; font-weight: bold; padding: 4px 12px; border-radius: 2px; }
        .data-panel { position: absolute; left: 15px; top: 120px; width: 180px; z-index: 10; }
        .data-box { border: 1px solid #00ff41; padding: 10px; margin-bottom: 10px; background: rgba(0,0,0,0.8); box-shadow: 0 0 10px rgba(0,255,65,0.2); }
        .label { font-size: 10px; color: #8f8; letter-spacing: 1px; margin-bottom: 4px; }
        .value { font-size: 18px; font-weight: bold; }
        .visual-center { position: absolute; width: 100%; height: 50%; top: 25%; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        /* 戰機視覺修正：改用形狀模擬，避免圖片失效 */
        .plane-silhouette { width: 200px; height: 100px; background: #00ff41; clip-path: polygon(50% 0%, 100% 100%, 80% 100%, 50% 70%, 20% 100%, 0% 100%); opacity: 0.1; transition: 0.5s; box-shadow: 0 0 30px #00ff41; }
        .active-mode { opacity: 1; transform: scale(1.1); filter: drop-shadow(0 0 15px #00ff41); }
        .radar-scan { position: absolute; bottom: 30px; right: 30px; width: 80px; height: 80px; border: 1px solid #00ff41; border-radius: 50%; }
        .scan-line { width: 50%; height: 2px; background: linear-gradient(to right, transparent, #00ff41); position: absolute; top: 50%; left: 50%; transform-origin: left center; animation: sweep 2s infinite linear; }
        @keyframes sweep { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
    </style>
</head>
<body>
    <div class="cockpit">
        <div class="header"><div style="font-size: 20px; font-weight: bold;">DLTRS V100.0 COMMANDER</div></div>
        <div class="config-panel"><span>1YR ATH:</span><input type="number" id="ath-input" value="30418"><button onclick="updateATH()">LOCK</button></div>
        <div class="data-panel">
            <div class="data-box"><div class="label">NDX REAL-TIME</div><div id="price-val" class="value">29,416.75</div></div>
            <div class="data-box"><div class="label">DROP % (偏離度)</div><div id="drop-val" class="value">0.00%</div></div>
        </div>
        <div class="visual-center">
            <div id="plane-viz" class="plane-silhouette"></div>
            <div id="mission-status" style="margin-top: 30px; font-size: 14px; letter-spacing: 2px; color: #00ff41; text-shadow: 0 0 5px #00ff41;">SCANNING...</div>
        </div>
        <div class="radar-scan"><div class="scan-line"></div></div>
    </div>
    <script>
        let currentATH = 30418;
        function updateATH() { currentATH = parseFloat(document.getElementById('ath-input').value); updateMarket(); }
        function updateMarket() {
            let currentPrice = 29416.75; 
            let drop = ((currentPrice - currentATH) / currentATH) * 100;
            document.getElementById('drop-val').innerText = drop.toFixed(2) + "%";
            let plane = document.getElementById('plane-viz');
            let mission = document.getElementById('mission-status');
            // 當跌幅超過 -4% 時啟動戰機模式
            if (drop <= -4) { plane.classList.add("active-mode"); mission.innerText = "B-2 SPIRIT: 戰鬥部署中"; }
            else { plane.classList.remove("active-mode"); mission.innerText = "空域安全：監視中"; }
        }
        updateMarket();
    </script>
</body>
</html>
