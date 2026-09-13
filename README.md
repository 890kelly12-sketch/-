<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>立體圖形 AI 探險家</title>
    <!-- Canvas Confetti 答對特效庫 -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        * {
            box-sizing: border-box;
            font-family: "PingFang HK", "Chalkboard SE", "微軟正黑體", sans-serif;
            user-select: none;
        }
        body {
            background: linear-gradient(135deg, #e0f2fe, #fef3c7);
            margin: 0;
            padding: 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            background: #ffffff;
            border-radius: 25px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.12);
            max-width: 850px;
            width: 100%;
            text-align: center;
            border: 5px solid #38bdf8;
            position: relative;
        }

        /* 標題與分頁 */
        h1 { font-size: 28px; color: #0284c7; margin-top: 0; }
        .nav-tabs {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 20px;
        }
        .tab-btn {
            background-color: #f1f5f9;
            border: 2px solid #cbd5e1;
            padding: 10px 18px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 20px;
            cursor: pointer;
            color: #475569;
        }
        .tab-btn.active {
            background-color: #0284c7;
            color: white;
            border-color: #0284c7;
            box-shadow: 0 4px 10px rgba(2, 132, 199, 0.3);
        }

        /* 主展示區 */
        .card-box {
            background-color: #fafafa;
            border: 3px dashed #cbd5e1;
            border-radius: 20px;
            padding: 20px;
            min-height: 380px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;
        }

        .shape-display {
            width: 160px;
            height: 160px;
            margin: 10px 0;
        }
        svg { width: 100%; height: 100%; }

        .audio-btn {
            background-color: #fef08a;
            border: 2px solid #eab308;
            border-radius: 50px;
            padding: 6px 16px;
            font-size: 16px;
            font-weight: bold;
            color: #854d0e;
            cursor: pointer;
            margin-bottom: 10px;
        }

        .btn-group {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 12px;
            margin-top: 15px;
        }
        .option-btn {
            background-color: #3b82f6;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 12px;
            cursor: pointer;
            box-shadow: 0 4px 0 #1d4ed8;
        }
        .option-btn:active { transform: translateY(4px); box-shadow: none; }

        .feature-list {
            text-align: left;
            background: #f0fdf4;
            border: 2px solid #86efac;
            padding: 15px 20px;
            border-radius: 15px;
            font-size: 17px;
            line-height: 1.6;
            color: #166534;
            max-width: 480px;
        }

        .next-btn {
            background-color: #22c55e;
            color: white;
            border: none;
            padding: 10px 25px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 0 #15803d;
            margin-top: 15px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📦 立體圖形 AI 探險家</h1>
    <div class="nav-tabs">
        <button class="tab-btn active" onclick="switchTab('surface', this)">1. 平面與曲面</button>
        <button class="tab-btn" onclick="switchTab('sphere', this)">2. 探索球體</button>
        <button class="tab-btn" onclick="switchTab('prism', this)">3. 識別柱體</button>
    </div>

    <div class="card-box" id="card-box">
        <!-- JavaScript 動態注入內容 -->
    </div>

    <button class="next-btn" onclick="nextQuestion()">➡️ 下一題 / 切換</button>
</div>

<script>
    let currentTab = 'surface';
    let currentIndex = 0;

    // 語音功能（預設廣東話 zh-HK）
    function speak(text) {
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'zh-HK';
            utterance.rate = 0.85;
            window.speechSynthesis.speak(utterance);
        }
    }

    // 音效
    function playSound() {
        const ctx = new (window.AudioContext || window.webkitAudioContext)();
        const osc = ctx.createOscillator();
        const gain = ctx.createGain();
        osc.frequency.setValueAtTime(600, ctx.currentTime);
        gain.gain.setValueAtTime(0.2, ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime + 0.3);
        osc.connect(gain);
        gain.connect(ctx.destination);
        osc.start();
        osc.stop(ctx.currentTime + 0.3);
    }

    function switchTab(tab, btn) {
        currentTab = tab;
        currentIndex = 0;
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        renderContent();
    }

    const surfaceData = [
        { name: "足球（球體）", type: "只有曲面", desc: "球體整個表面都是彎曲的曲面，沒有平面喔！", svg: `<circle cx="80" cy="80" r="60" fill="#38bdf8" stroke="#0284c7" stroke-width="4"/><ellipse cx="80" cy="80" rx="60" ry="20" fill="none" stroke="#fff" stroke-width="3" stroke-dasharray="4 4"/>` },
        { name: "正方體積木", type: "只有平面", desc: "正方體由6個平平的正方形組成，全部都是平面！", svg: `<rect x="40" y="40" width="80" height="80" fill="#facc15" stroke="#ca8a04" stroke-width="4"/><path d="M40,40 L65,15 L145,15 L120,40 M145,15 L145,95 L120,120" fill="none" stroke="#ca8a04" stroke-width="4"/>` },
        { name: "可樂罐（圓柱體）", type: "兩者都有", desc: "圓柱體的頂部和底部是平面，側面是曲面！", svg: `<ellipse cx="80" cy="35" rx="45" ry="18" fill="#f87171" stroke="#dc2626" stroke-width="4"/><rect x="35" y="35" width="90" height="90" fill="#f87171"/><path d="M35,35 L35,125 A45,18 0 0,0 125,125 L125,35" fill="none" stroke="#dc2626" stroke-width="4"/>` }
    ];

    const prismData = [
        { name: "圓柱體", correct: "圓柱體", options: ["圓柱體", "三角柱體", "長方體"], info: "上下兩個底面都是相等的圓形！", svg: `<ellipse cx="80" cy="35" rx="45" ry="18" fill="#a7f3d0" stroke="#059669" stroke-width="4"/><rect x="35" y="35" width="90" height="90" fill="#a7f3d0"/><path d="M35,35 L35,125 A45,18 0 0,0 125,125 L125,35" fill="none" stroke="#059669" stroke-width="4"/>` },
        { name: "三角柱體", correct: "三角柱體", options: ["四角柱體", "三角柱體", "圓柱體"], info: "上下兩個底面都是三角形！", svg: `<polygon points="80,20 35,65 125,65" fill="#c084fc" stroke="#7e22ce" stroke-width="4"/><path d="M35,65 L35,135 L125,135 L125,65 M80,20 L80,90 L35,135 M80,90 L125,135" fill="none" stroke="#7e22ce" stroke-width="4"/>` },
        { name: "長方體", correct: "長方體", options: ["正方體", "長方體", "三角柱體"], info: "有6個長方形的面，相對的面大小相同！", svg: `<rect x="25" y="50" width="90" height="60" fill="#fed7aa" stroke="#ea580c" stroke-width="4"/><path d="M25,50 L50,18 L140,18 L115,50 M140,18 L140,78 L115,110" fill="none" stroke="#ea580c" stroke-width="4"/>` }
    ];

    function renderContent() {
        const box = document.getElementById('card-box');

        if (currentTab === 'surface') {
            let item = surfaceData[currentIndex % surfaceData.length];
            box.innerHTML = `
                <button class="audio-btn" onclick="speak('${item.name}包含平面還是曲面？')">🔊 聽題目</button>
                <h3>${item.name}</h3>
                <div class="shape-display"><svg>${item.svg}</svg></div>
                <div class="btn-group">
                    <button class="option-btn" onclick="checkAns('${item.type}', '只有平面', '${item.desc}')">只有平面</button>
                    <button class="option-btn" onclick="checkAns('${item.type}', '只有曲面', '${item.desc}')">只有曲面</button>
                    <button class="option-btn" onclick="checkAns('${item.type}', '兩者都有', '${item.desc}')">兩者都有</button>
                </div>
                <p id="feedback" style="font-size:18px; font-weight:bold; color:#0284c7; min-height:24px; margin-top:10px;"></p>
            `;
            speak(`${item.name}包含平面還是曲面？`);

        } else if (currentTab === 'sphere') {
            box.innerHTML = `
                <button class="audio-btn" onclick="speak('探索球體的特性')">🔊 聽說明</button>
                <h3>⚽ 球體 (Sphere) 的三大特性</h3>
                <div class="shape-display">
                    <svg><circle cx="80" cy="80" r="60" fill="#38bdf8" stroke="#0284c7" stroke-width="4"/><ellipse cx="80" cy="80" rx="60" ry="20" fill="none" stroke="#fff" stroke-width="3" stroke-dasharray="4 4"/></svg>
                </div>
                <div class="feature-list">
                    📍 <strong>全曲面：</strong> 表面全部都是彎曲的，沒有任何平面。<br>
                    📍 <strong>無邊無角：</strong> 圓滑沒有尖角，也沒有直直的邊。<br>
                    📍 <strong>自由滾動：</strong> 放在桌上，向任何方向都可以滾動。
                </div>
            `;
            speak("球體的表面全部都是曲面，沒有邊和角，向任何方向都可以自由滾動！");

        } else if (currentTab === 'prism') {
            let item = prismData[currentIndex % prismData.length];
            box.innerHTML = `
                <button class="audio-btn" onclick="speak('請問這個柱體的名稱是什麼？')">🔊 聽題目</button>
                <h3>認一認，這是什麼柱體？</h3>
                <div class="shape-display"><svg>${item.svg}</svg></div>
                <div class="btn-group">
                    ${item.options.map(opt => `<button class="option-btn" onclick="checkAns('${item.correct}', '${opt}', '${item.info}')">${opt}</button>`).join('')}
                </div>
                <p id="feedback" style="font-size:18px; font-weight:bold; color:#0284c7; min-height:24px; margin-top:10px;"></p>
            `;
            speak("請問這個柱體的名稱是什麼？");
        }
    }

    function checkAns(target, user, info) {
        let fb = document.getElementById('feedback');
        if (target === user) {
            playSound();
            confetti({ particleCount: 80, spread: 60 });
            fb.innerText = `🎉 答對了！${info}`;
            speak(`答對了！${info}`);
        } else {
            fb.innerText = `🤔 再想一想喔！仔細看清楚形狀。`;
            speak("再想一想喔！");
        }
    }

    function nextQuestion() {
        currentIndex++;
        renderContent();
    }

    window.onload = renderContent;
</script>

</body>
</html>
