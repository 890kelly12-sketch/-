<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>立體圖形 AI 互動學習工作紙</title>
    <!-- Canvas Confetti 特效庫 -->
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

        /* 頂部標題與分頁 */
        h1 { font-size: 26px; color: #0284c7; margin: 0 0 10px 0; }
        .nav-tabs {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        .tab-btn {
            background-color: #f1f5f9;
            border: 2px solid #cbd5e1;
            padding: 8px 14px;
            font-size: 15px;
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

        /* 主卡片區域 */
        .card-box {
            background-color: #fafafa;
            border: 3px dashed #cbd5e1;
            border-radius: 20px;
            padding: 15px;
            min-height: 420px;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
        }

        /* 標題列與燈泡按鈕 */
        .question-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 10px;
        }
        .q-title { font-size: 20px; font-weight: bold; color: #1e293b; }
        .btn-bulb {
            background: #fef08a;
            border: 2px solid #eab308;
            border-radius: 50%;
            width: 42px;
            height: 42px;
            font-size: 22px;
            cursor: pointer;
            box-shadow: 0 3px 0 #ca8a04;
        }
        .btn-bulb:active { transform: translateY(3px); box-shadow: none; }

        /* 提示資訊框 */
        .hint-box {
            display: none;
            background: #fef3c7;
            border: 2px solid #f59e0b;
            border-radius: 12px;
            padding: 10px;
            margin-bottom: 15px;
            color: #78350f;
            font-size: 16px;
            width: 100%;
            text-align: left;
        }

        /* 模擬測試區（斜道與桌面） */
        .simulation-area {
            display: none;
            width: 100%;
            height: 140px;
            background: #e2e8f0;
            border-radius: 15px;
            position: relative;
            margin-bottom: 15px;
            overflow: hidden;
            border: 2px solid #94a3b8;
        }
        /* 斜道樣式 */
        .ramp {
            position: absolute;
            width: 80%;
            height: 10px;
            background: #854d0e;
            top: 40px;
            left: 10%;
            transform: rotate(15deg);
            border-radius: 5px;
        }
        /* 桌子樣式 */
        .table-top {
            position: absolute;
            width: 80%;
            height: 15px;
            background: #b45309;
            bottom: 30px;
            left: 10%;
            border-radius: 5px;
        }
        /* 測試物件動畫 */
        .sim-object {
            position: absolute;
            font-size: 35px;
            transition: all 1.2s ease-in-out;
        }

        /* 物件選項按鈕列 */
        .options-grid {
            display: flex;
            justify-content: center;
            gap: 15px;
            width: 100%;
            margin-top: 10px;
            flex-wrap: wrap;
        }
        .item-card {
            background: white;
            border: 3px solid #cbd5e1;
            border-radius: 15px;
            padding: 10px 15px;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 110px;
            box-shadow: 0 4px 0 #94a3b8;
            transition: 0.1s;
        }
        .item-card:active { transform: translateY(4px); box-shadow: none; }
        .item-card .icon { font-size: 40px; }
        .item-card .name { font-weight: bold; margin-top: 5px; color: #334155; }

        /* 回饋與控制按鈕 */
        .feedback {
            font-size: 18px;
            font-weight: bold;
            color: #0284c7;
            min-height: 30px;
            margin-top: 10px;
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
            margin-top: 10px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📦 立體圖形 AI 互動學習工作紙</h1>

    <!-- 頁籤導覽 -->
    <div class="nav-tabs">
        <button class="tab-btn active" onclick="switchGoal(1, this)">目標一：平面與曲面</button>
        <button class="tab-btn" onclick="switchGoal(2, this)">目標二：球體特性</button>
        <button class="tab-btn" onclick="switchGoal(3, this)">目標三：柱體特性</button>
    </div>

    <!-- 主展示區 -->
    <div class="card-box">
        <div class="question-header">
            <div class="q-title" id="q-title">題目載入中...</div>
            <button class="btn-bulb" onclick="toggleHint()" title="按我拿提示！">💡</button>
        </div>

        <!-- 提示文字框 -->
        <div class="hint-box" id="hint-box"></div>

        <!-- 物理模擬測試區（斜道/桌面） -->
        <div class="simulation-area" id="sim-area">
            <div id="sim-stage"></div>
            <div id="sim-obj" class="sim-object"></div>
        </div>

        <!-- 選項按鈕區 -->
        <div class="options-grid" id="options-grid"></div>

        <!-- 反饋訊息 -->
        <div class="feedback" id="feedback"></div>
    </div>

    <button class="next-btn" onclick="nextQuestion()">➡️ 下一題</button>
</div>

<script>
    let currentGoal = 1;
    let currentQIndex = 0;

    // 廣東話語音朗讀
    function speak(text) {
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'zh-HK';
            utterance.rate = 0.85;
            window.speechSynthesis.speak(utterance);
        }
    }

    // 題庫資料結構
    const quizData = {
        goal1: [
            {
                title: "以下哪一個物件同時有「平面」和「曲面」？",
                hint: "小貼士：平面像鏡子一樣平平的；曲面是彎彎的。請點擊物件放入斜道測試！",
                options: [
                    { name: "罐頭", icon: "🥫", isCorrect: true, type: "both", desc: "答對咗！罐頭頂部和底部是平面，側面是曲面！" },
                    { name: "足球", icon: "⚽", isCorrect: false, type: "curved", desc: "足球只有曲面喔！" },
                    { name: "紙巾盒", icon: "🧻", isCorrect: false, type: "flat", desc: "紙巾盒只有平面喔！" }
                ]
            },
            {
                title: "魔術方塊（正方體）有沒有「曲面」？",
                hint: "小貼士：放在斜道上試試看，看看它會滾動還是只能滑動？",
                options: [
                    { name: "只有平面", icon: "🧊", isCorrect: true, type: "flat", desc: "你答對咗啦！魔術方塊全部都是平面，沒有曲面。" },
                    { name: "有曲面", icon: "🏀", isCorrect: false, type: "curved", desc: "再想想看，正方體有彎彎的面嗎？" }
                ]
            }
        ],
        goal2: [
            {
                title: "以下哪一個是「球體」？",
                hint: "💡 球體特性：可自由滾動，而且「無邊亦無角」。按下方物件放在斜道上測試看看！",
                options: [
                    { name: "橙", icon: "🍊", isCorrect: true, isSphere: true, desc: "答對咗！橙是球體，可以順暢滾動，而且沒有邊和角！" },
                    { name: "雪糕筒", icon: "🍦", isCorrect: false, isSphere: false, desc: "雪糕筒有尖尖的角，不是球體喔！" },
                    { name: "紙巾盒", icon: "🧻", isCorrect: false, isSphere: false, desc: "紙巾盒有直直的邊和尖角，不是球體！" }
                ]
            },
            {
                title: "哪一個物件放在斜道上可以向任何方向滾動？",
                hint: "💡 球體沒有尖角限制，所以向哪裡推都能滾動！",
                options: [
                    { name: "乒乓球", icon: "🏓", isCorrect: true, isSphere: true, desc: "答對咗！乒乓球是球體，無邊無角，可以自由滾動！" },
                    { name: "骰子", icon: "🎲", isCorrect: false, isSphere: false, desc: "骰子有角和邊，不能自由滾動喔！" }
                ]
            }
        ],
        goal3: [
            {
                title: "以下哪一個是「柱體」？",
                hint: "💡 柱體特性：可以豎立在桌上，且「頂部和底部是平平的形狀」。按物件放在桌上測試！",
                options: [
                    { name: "雪糕筒", icon: "🍦", isCorrect: false, isPrism: false, desc: "雪糕筒頂部是尖的，不能兩端都平放豎立！" },
                    { name: "紙巾盒", icon: "🧻", isCorrect: true, isPrism: true, desc: "答對咗！紙巾盒可以豎立，頂和底都是平平的長方形，是柱體！" }
                ]
            },
            {
                title: "以下哪一個物件可以豎立，且頂部與底部形狀完全一樣？",
                hint: "💡 柱體可以站得穩穩的，上下兩個面平平且形狀相同。",
                options: [
                    { name: "罐頭", icon: "🥫", isCorrect: true, isPrism: true, desc: "答對咗！罐頭是圓柱體，頂部和底部都是平平的圓形！" },
                    { name: "金字塔", icon: "🎪", isCorrect: false, isPrism: false, desc: "金字塔頂部是尖角，不是柱體喔！" }
                ]
            }
        ]
    };

    // 切換學習目標
    function switchGoal(goalNum, btn) {
        currentGoal = goalNum;
        currentQIndex = 0;
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        loadQuestion();
    }

    // 載入當前題目
    function loadQuestion() {
        const goalKey = `goal${currentGoal}`;
        const qList = quizData[goalKey];
        const q = qList[currentQIndex % qList.length];

        document.getElementById('q-title').innerText = q.title;
        document.getElementById('hint-box').innerText = q.hint;
        document.getElementById('hint-box').style.display = 'none';
        document.getElementById('sim-area').style.display = 'none';
        document.getElementById('feedback').innerText = '';

        // 渲染選項按鈕
        const grid = document.getElementById('options-grid');
        grid.innerHTML = '';
        q.options.forEach(opt => {
            const card = document.createElement('div');
            card.className = 'item-card';
            card.onclick = () => handleSelect(opt);
            card.innerHTML = `<div class="icon">${opt.icon}</div><div class="name">${opt.name}</div>`;
            grid.appendChild(card);
        });

        speak(q.title);
    }

    // 切換提示顯示並執行模擬
    function toggleHint() {
        const hintBox = document.getElementById('hint-box');
        const simArea = document.getElementById('sim-area');
        
        if (hintBox.style.display === 'none') {
            hintBox.style.display = 'block';
            simArea.style.display = 'block';
            speak(hintBox.innerText);
            setupSimulationStage();
        } else {
            hintBox.style.display = 'none';
            simArea.style.display = 'none';
        }
    }

    // 設定斜道或桌面模擬舞台
    function setupSimulationStage() {
        const stage = document.getElementById('sim-stage');
        if (currentGoal === 1 || currentGoal === 2) {
            // 斜道場景
            stage.innerHTML = '<div class="ramp"></div>';
        } else {
            // 桌面豎立場景
            stage.innerHTML = '<div class="table-top"></div>';
        }
    }

    // 點擊選項處理與動態模擬演示
    function handleSelect(opt) {
        const simArea = document.getElementById('sim-area');
        const simObj = document.getElementById('sim-obj');
        simArea.style.display = 'block';
        setupSimulationStage();

        simObj.innerText = opt.icon;

        if (currentGoal === 1 || currentGoal === 2) {
            // 斜道模擬動畫
            simObj.style.top = '10px';
            simObj.style.left = '15%';
            simObj.style.transform = 'rotate(0deg)';

            setTimeout(() => {
                if (opt.type === 'curved' || opt.isSphere || opt.type === 'both') {
                    // 滾動動畫
                    simObj.style.top = '70px';
                    simObj.style.left = '75%';
                    simObj.style.transform = 'rotate(360deg)';
                } else {
                    // 滑動或卡住（非球體/非曲面）
                    simObj.style.top = '30px';
                    simObj.style.left = '30%';
                }
            }, 100);

        } else {
            // 柱體豎立桌面模擬動畫
            simObj.style.top = '10px';
            simObj.style.left = '45%';

            setTimeout(() => {
                simObj.style.top = '55px'; // 放到桌上
            }, 100);
        }

        // 回饋訊息與聲音
        const fb = document.getElementById('feedback');
        fb.innerText = opt.desc;
        speak(opt.desc);

        if (opt.isCorrect) {
            confetti({ particleCount: 80, spread: 60 });
        }
    }

    function nextQuestion() {
        currentQIndex++;
        loadQuestion();
    }

    window.onload = loadQuestion;
</script>

</body>
</html>
