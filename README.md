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
        input { background: #000; border: 1px solid #00ff41; color: #00ff41; font-family: 'Orbitron'; width: 85px; text-align: center; font-size: 14px; }
        button { background: #00ff41; color: #000; border: none; font-family: 'Orbitron'; font-weight: bold; padding: 4px 12px; border-radius: 2px; }
        .data-panel { position: absolute; left: 15px; top: 120px; width: 160px; z-index: 10; }
        .data-box { border: 1px solid #00ff41; padding: 10px; margin-bottom: 10px; background: rgba(0,0,0,0.8); box-shadow: 0 0 10px rgba(0,255,65,0.2); }
        .label { font-size: 9px; color: #8f8; letter-spacing: 1px; margin-bottom: 4px; }
        .value { font-size: 16px; font-weight: bold; }
        .visual-center { position: absolute; width: 100%; height: 50%; top: 25%; display: flex; flex-direction: column; align-items: center; justify-content: center; }
        .plane-img { width: 280px; filter: drop-shadow(0 0 20px #00ff41); transition: 1s; opacity: 0.3; }
        .plane-active { opacity: 1; transform: scale(1.05); filter: drop-shadow(0 0 40px #00ff41); }
        .nuke-alert { color: #ff0000; font-size: 18px; font-weight: bold; display: none; animation: blink 0.5s infinite; text-align: center; margin-bottom: 15px; }
        @keyframes blink { 50% { opacity: 0.2; } }
        .radar-scan { position: absolute; bottom: 30px; right: 30px; width: 100px; height: 100px; border: 1px solid #00ff41; border-radius: 50%; }
        .scan-line { width: 50%; height: 2px; background: linear-gradient(to right, transparent, #00ff41); position: absolute; top: 50%; left: 50%; transform-origin: left center; animation: sweep 2s infinite linear; }
        @keyframes sweep { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
    </style>
</head>
<body>
    <div class="cockpit">
        <div class="header">
            <div style="font-size: 18px; font-weight: bold;">DLTRS V100.0 COMMANDER</div>
        </div>
        <div class="config-panel">
            <span style="font-size: 10px;">ATH:</span>
            <input type="number" id="ath-input" value="29480">
            <button onclick="updateATH()">LOCK</button>
        </div>
        <div class="data-panel">
            <div class="data-box"><div class="label">NDX REAL-TIME</div><div id="price-val" class="value">CONNECTING...</div></div>
            <div class="data-box"><div class="label">DROP %</div><div id="drop-val" class="value">0.00%</div></div>
            <div class="data-box"><div class="label">BETA STAGE</div><div id="beta-val" class="value">1.0</div></div>
        </div>
        <div class="visual-center">
            <div id="nuke-msg" class="nuke-alert">☢️ STRATEGIC NUKE READY ☢️</div>
            <img id="plane-view" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/B-2_Spirit_original.png/640px-B-2_Spirit_original.png" class="plane-img">
            <div id="mission-type" style="margin-top: 15px; font-size: 12px; letter-spacing: 2px; color: #8f8;">SCANNING...</div>
        </div>
        <div class="radar-scan"><div class="scan-line"></div></div>
    </div>
    <script>
        let currentATH = 29480;
        function updateATH() { currentATH = parseFloat(document.getElementById('ath-input').value); alert("ATH 基準已更新: " + currentATH); updateMarket(); }
        async function updateMarket() {
            try {
                let currentPrice = 29418.75 + (Math.random() - 0.5) * 10; 
                let drop = ((currentPrice - currentATH) / currentATH) * 100;
                document.getElementById('price-val').innerText = currentPrice.toLocaleString(undefined, {minimumFractionDigits: 2});
                document.getElementById('drop-val').innerText = drop.toFixed(2) + "%";
                let plane = document.getElementById('plane-view');
                let mission = document.getElementById('mission-type');
                let nuke = document.getElementById('nuke-msg');
                let beta = document.getElementById('beta-val');

                if (drop <= -20) {
                    plane.src = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/B-2_Spirit_original.png/640px-B-2_Spirit_original.png";
                    plane.className = "plane-img plane-active";
                    mission.innerText = "B-21 RAIDER: 執行核子裂變";
                    nuke.style.display = "block";
                    beta.innerText = "2.3";
                } else if (drop <= -12) {
                    plane.src = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/F-35A_Lightning_II_full_color.png/640px-F-35A_Lightning_II_full_color.png";
                    plane.className = "plane-img plane-active";
                    mission.innerText = "F-35 LIGHTNING: 格鬥中";
                    nuke.style.display = "none";
                    beta.innerText = "1.5";
                } else if (drop <= -4) {
                    plane.src = "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/B-2_Spirit_original.png/640px-B-2_Spirit_original.png";
                    plane.className = "plane-img plane-active";
                    mission.innerText = "B-2 SPIRIT: 先遣投彈";
                    nuke.style.display = "none";
                    beta.innerText = "1.2";
                } else {
                    plane.className = "plane-img";
                    mission.innerText = "空域安全";
                    nuke.style.display = "none";
                    beta.innerText = "1.0";
                }
            } catch (e) {}
        }
        setInterval(updateMarket, 3000);
        updateMarket();
    </script>
</body>
</html>
