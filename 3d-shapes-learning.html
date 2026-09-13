<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>立體圖形學習：球體與柱體</title>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        * { box-sizing: border-box; user-select: none; }
        body {
            margin: 0;
            font-family: "PingFang HK", "Noto Sans TC", "微軟正黑體", sans-serif;
            background: linear-gradient(180deg, #e0f2fe, #fef9c3);
            min-height: 100vh;
            color: #0f172a;
        }
        .wrap { max-width: 880px; margin: 0 auto; padding: 16px; }
        header.app {
            background: #fff;
            border: 5px solid #86efac;
            border-radius: 24px;
            padding: 14px;
            text-align: center;
            margin-bottom: 12px;
        }
        header.app h1 {
            margin: 0;
            font-size: 24px;
            background: linear-gradient(45deg, #0284c7, #ea580c);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        .tabs { display: flex; gap: 8px; margin: 10px 0; }
        .tab {
            flex: 1;
            border: none;
            background: #fff;
            border-radius: 14px;
            padding: 12px;
            font-weight: 800;
            font-size: 18px;
            cursor: pointer;
            box-shadow: 0 4px 0 #cbd5e1;
        }
        .tab.active { background: #fde047; box-shadow: 0 4px 0 #ca8a04; }
        .panel {
            display: none;
            background: #fff;
            border-radius: 24px;
            border: 5px solid #93c5fd;
            padding: 16px;
        }
        .panel.show { display: block; }
        .goal-banner {
            background: #eff6ff;
            border-radius: 14px;
            padding: 10px 12px;
            margin-bottom: 10px;
            font-weight: 800;
            color: #1d4ed8;
            font-size: 16px;
        }
        .progress { text-align: center; color: #64748b; margin-bottom: 8px; font-weight: 700; }
        .q-head {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
        }
        .q-title { font-size: 22px; font-weight: 900; margin: 0; }
        .bulb-row { display: flex; gap: 10px; }
        .bulb-item { display: flex; flex-direction: column; align-items: center; gap: 2px; }
        .bulb-label { font-size: 13px; font-weight: 800; color: #475569; }
        .btn-icon {
            background: #fef08a;
            border: 3px solid #eab308;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            font-size: 22px;
            cursor: pointer;
            box-shadow: 0 3px 0 #ca8a04;
        }
        .btn-icon:active { transform: translateY(3px); box-shadow: none; }
        .btn-icon.locked {
            background: #e2e8f0;
            border-color: #94a3b8;
            box-shadow: 0 3px 0 #64748b;
            opacity: 0.5;
            cursor: not-allowed;
        }
        .hint-box {
            display: none;
            background: #ecfeff;
            border: 2px solid #22d3ee;
            border-radius: 14px;
            padding: 10px 12px;
            margin: 10px 0 0;
            font-size: 18px;
            font-weight: 700;
        }
        .choices {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 10px;
            margin: 14px 0;
        }
        .choice {
            background: #f8fafc;
            border: 3px solid #cbd5e1;
            border-radius: 18px;
            padding: 12px 8px;
            text-align: center;
            cursor: pointer;
            font-weight: 800;
            font-size: 18px;
        }
        .choice .emoji { font-size: 52px; line-height: 1.1; }
        .choice.correct { border-color: #16a34a; background: #dcfce7; }
        .choice.wrong { border-color: #dc2626; background: #fee2e2; }
        .nav-row { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }
        .btn {
            border: none;
            border-radius: 999px;
            padding: 10px 22px;
            font-size: 16px;
            font-weight: 800;
            cursor: pointer;
            color: #fff;
        }
        .btn-green { background: #22c55e; box-shadow: 0 4px 0 #15803d; }
        .btn-blue { background: #3b82f6; box-shadow: 0 4px 0 #1d4ed8; }
        .feedback { min-height: 26px; text-align: center; font-weight: 800; margin: 8px 0; font-size: 18px; }
        .lab {
            display: none;
            margin-top: 12px;
            background: #f8fafc;
            border-radius: 16px;
            padding: 12px;
        }
        .lab h3 { margin: 0 0 8px; font-size: 18px; }
        .lab p { margin: 0 0 8px; }
        .objects { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 10px; }
        .obj {
            border: 3px solid #cbd5e1;
            background: #fff;
            border-radius: 14px;
            padding: 8px 10px;
            cursor: pointer;
            font-weight: 800;
            min-width: 84px;
            text-align: center;
        }
        .obj.picked { border-color: #2563eb; background: #dbeafe; }

        .ramp-stage {
            position: relative;
            height: 220px;
            border-radius: 14px;
            overflow: hidden;
            background: linear-gradient(#dbeafe, #bbf7d0);
        }
        .ramp-board {
            position: absolute;
            left: 8%;
            top: 18%;
            width: 84%;
            height: 22px;
            background: #78716c;
            border-radius: 8px;
            transform: rotate(18deg);
            transform-origin: left center;
            box-shadow: 0 4px 0 #44403c;
        }
        .ramp-post {
            position: absolute;
            left: 8%;
            top: 18%;
            width: 14px;
            height: 62%;
            background: #57534e;
            border-radius: 4px;
        }
        .roller {
            position: absolute;
            left: 10%;
            top: 4%;
            font-size: 44px;
            z-index: 2;
            transition: left 1.2s linear, top 1.2s linear, transform 1.2s linear;
        }
        .roller.roll { left: 78%; top: 58%; transform: rotate(480deg); }
        .roller.stuck { left: 18%; top: 12%; transform: rotate(10deg); }

        .table-wrap {
            position: relative;
            height: 170px;
            background: linear-gradient(#e0f2fe, #fef9c3);
            border-radius: 14px;
            overflow: hidden;
        }
        .tabletop {
            position: absolute;
            left: 12%;
            right: 12%;
            bottom: 36px;
            height: 16px;
            background: #92400e;
            border-radius: 6px;
        }
        .table-leg {
            position: absolute;
            bottom: 10px;
            width: 12px;
            height: 30px;
            background: #78350f;
        }
        .table-leg.l { left: 20%; }
        .table-leg.r { right: 20%; }
        .stander {
            position: absolute;
            left: 50%;
            bottom: 52px;
            transform: translateX(-50%);
            font-size: 54px;
            transition: transform .45s ease, bottom .45s ease;
        }
        .stander.fall { transform: translateX(-20%) rotate(78deg); bottom: 20px; }
        .highlight-parts { display: flex; gap: 8px; flex-wrap: wrap; margin-top: 8px; }
        .part {
            background: #fff;
            border: 2px dashed #64748b;
            border-radius: 12px;
            padding: 8px 12px;
            cursor: pointer;
            font-weight: 800;
        }
        .part.found { background: #dcfce7; border-color: #16a34a; }
    </style>
</head>
<body>
<div class="wrap">
    <header class="app">
        <h1>立體圖形學習：球體與柱體</h1>
    </header>

    <div class="tabs">
        <button class="tab active" onclick="showTab('g2', this)">球體</button>
        <button class="tab" onclick="showTab('g3', this)">柱體</button>
    </div>

    <section id="g2" class="panel show">
        <div class="goal-banner">目標：指出球體的特性</div>
        <div class="progress" id="g2-progress"></div>
        <div class="q-head">
            <h2 class="q-title" id="g2-title"></h2>
            <div class="bulb-row">
                <div class="bulb-item">
                    <button class="btn-icon locked" id="bulb-tip" onclick="openSphereTip()">💡</button>
                    <div class="bulb-label">特性</div>
                </div>
                <div class="bulb-item">
                    <button class="btn-icon" id="bulb-exp" onclick="openExperiment()">💡</button>
                    <div class="bulb-label">實驗</div>
                </div>
            </div>
        </div>
        <div class="hint-box" id="g2-hint">球體可以滾動，沒有邊，也沒有角。</div>
        <div class="lab" id="ramp-lab">
            <h3>斜道實驗</h3>
            <p>先選物件，再按「放到斜道」。</p>
            <div class="objects" id="ramp-objects"></div>
            <div class="nav-row" style="margin:8px 0;">
                <button class="btn btn-green" onclick="releaseRamp()">放到斜道</button>
                <button class="btn btn-blue" onclick="resetRamp()">重設</button>
            </div>
            <div class="ramp-stage">
                <div class="ramp-post"></div>
                <div class="ramp-board"></div>
                <div class="roller" id="roller">❓</div>
            </div>
        </div>
        <div class="choices" id="g2-choices"></div>
        <div class="feedback" id="g2-fb"></div>
        <div class="nav-row">
            <button class="btn btn-blue" onclick="nextG2()">下一題</button>
        </div>
    </section>

    <section id="g3" class="panel">
        <div class="goal-banner">目標：指出柱體的特性</div>
        <div class="progress" id="g3-progress"></div>
        <div class="q-head">
            <h2 class="q-title" id="g3-title"></h2>
            <div class="bulb-row">
                <div class="bulb-item">
                    <button class="btn-icon locked" id="bulb-tip-p" onclick="openPrismTip()">💡</button>
                    <div class="bulb-label">特性</div>
                </div>
                <div class="bulb-item">
                    <button class="btn-icon" id="bulb-exp-p" onclick="openTableExperiment()">💡</button>
                    <div class="bulb-label">實驗</div>
                </div>
            </div>
        </div>
        <div class="hint-box" id="g3-hint">柱體可以豎立，頂和底都是平的。</div>
        <div class="lab" id="table-lab">
            <h3>桌面實驗</h3>
            <p>先選物件，再按「放到桌上」。</p>
            <div class="objects" id="table-objects"></div>
            <div class="nav-row" style="margin:8px 0;">
                <button class="btn btn-green" onclick="placeOnTable()">放到桌上</button>
                <button class="btn btn-blue" onclick="resetTable()">重設</button>
            </div>
            <div class="table-wrap">
                <div class="stander" id="stander">❓</div>
                <div class="tabletop"></div>
                <div class="table-leg l"></div>
                <div class="table-leg r"></div>
            </div>
            <div class="highlight-parts">
                <button class="part" id="part-top" onclick="markPart('top')">頂</button>
                <button class="part" id="part-bottom" onclick="markPart('bottom')">底</button>
            </div>
        </div>
        <div class="choices" id="g3-choices"></div>
        <div class="feedback" id="g3-fb"></div>
        <div class="nav-row">
            <button class="btn btn-blue" onclick="nextG3()">下一題</button>
        </div>
    </section>
</div>

<script>
    function speakNow(text) {
        if (!('speechSynthesis' in window)) return;
        speechSynthesis.cancel();
        const u = new SpeechSynthesisUtterance(text);
        u.lang = 'zh-HK';
        u.rate = 0.8;
        speechSynthesis.speak(u);
    }
    function showTab(id, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('show'));
        document.getElementById(id).classList.add('show');
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        btn.classList.add('active');
    }
    function celebrate() {
        if (window.confetti) confetti({ particleCount: 80, spread: 70, origin: { y: 0.7 } });
    }

    const g2Data = [
        { title: '哪一個是球體？', items: [
            { name: '橙', emoji: '🍊', sphere: true },
            { name: '雪糕', emoji: '🍦', sphere: false },
            { name: '紙巾盒', emoji: '📦', sphere: false }
        ]},
        { title: '哪一個是球體？', items: [
            { name: '足球', emoji: '⚽', sphere: true },
            { name: '書本', emoji: '📘', sphere: false },
            { name: '水杯', emoji: '🥤', sphere: false }
        ]},
        { title: '哪一個是球體？', items: [
            { name: '西瓜', emoji: '🍉', sphere: true },
            { name: '金字塔', emoji: '🔺', sphere: false },
            { name: '積木', emoji: '🧊', sphere: false }
        ]},
        { title: '哪一個是球體？', items: [
            { name: '玻璃珠', emoji: '🔵', sphere: true },
            { name: '罐', emoji: '🥫', sphere: false },
            { name: '派對帽', emoji: '🎉', sphere: false }
        ]}
    ];
    let g2Index = 0, rampPick = null, openedExperiment = false;

    function renderG2() {
        const q = g2Data[g2Index];
        document.getElementById('g2-progress').textContent = '第 ' + (g2Index + 1) + ' / ' + g2Data.length + ' 題';
        document.getElementById('g2-title').textContent = q.title;
        document.getElementById('g2-hint').style.display = 'none';
        document.getElementById('g2-fb').textContent = '';
        document.getElementById('g2-choices').innerHTML = q.items.map((it, i) =>
            '<button class="choice" onclick="pickSphere(' + i + ')"><div class="emoji">' + it.emoji + '</div>' + it.name + '</button>'
        ).join('');
        setupRamp(q.items);
    }
    function pickSphere(i) {
        const q = g2Data[g2Index];
        const buttons = document.querySelectorAll('#g2-choices .choice');
        buttons.forEach(b => b.classList.remove('correct', 'wrong'));
        if (q.items[i].sphere) {
            buttons[i].classList.add('correct');
            document.getElementById('g2-fb').textContent = '正確。';
            speakNow('正確。');
            celebrate();
        } else {
            buttons[i].classList.add('wrong');
            document.getElementById('g2-fb').textContent = '請再想一想。';
            speakNow('請再想一想。');
        }
    }
    function openExperiment() {
        document.getElementById('ramp-lab').style.display = 'block';
        openedExperiment = true;
        document.getElementById('bulb-tip').classList.remove('locked');
    }
    function openSphereTip() {
        if (!openedExperiment) {
            document.getElementById('g2-fb').textContent = '請先按「實驗」。';
            return;
        }
        const box = document.getElementById('g2-hint');
        box.style.display = box.style.display === 'block' ? 'none' : 'block';
    }
    function setupRamp(items) {
        rampPick = null;
        document.getElementById('ramp-objects').innerHTML = items.map((it, i) =>
            '<button class="obj" id="ramp-obj-' + i + '" onclick="chooseRamp(' + i + ')">' + it.emoji + '<br>' + it.name + '</button>'
        ).join('');
        resetRamp();
    }
    function chooseRamp(i) {
        rampPick = g2Data[g2Index].items[i];
        document.querySelectorAll('#ramp-objects .obj').forEach(el => el.classList.remove('picked'));
        document.getElementById('ramp-obj-' + i).classList.add('picked');
        const roller = document.getElementById('roller');
        roller.textContent = rampPick.emoji;
        roller.className = 'roller';
    }
    function releaseRamp() {
        if (!rampPick) return;
        const roller = document.getElementById('roller');
        roller.className = 'roller';
        void roller.offsetWidth;
        roller.classList.add(rampPick.sphere ? 'roll' : 'stuck');
    }
    function resetRamp() {
        const roller = document.getElementById('roller');
        roller.className = 'roller';
        roller.textContent = rampPick ? rampPick.emoji : '❓';
    }
    function nextG2() {
        g2Index = (g2Index + 1) % g2Data.length;
        document.getElementById('ramp-lab').style.display = 'none';
        document.getElementById('g2-hint').style.display = 'none';
        openedExperiment = false;
        document.getElementById('bulb-tip').classList.add('locked');
        renderG2();
    }

    const g3Data = [
        { title: '哪一個是柱體？', items: [
            { name: '圓柱罐', emoji: '🥫', prism: true },
            { name: '波', emoji: '🏀', prism: false },
            { name: '雪糕', emoji: '🍦', prism: false }
        ]},
        { title: '哪一個是柱體？', items: [
            { name: '紙巾盒', emoji: '📦', prism: true },
            { name: '橙', emoji: '🍊', prism: false }
        ]},
        { title: '哪一個是柱體？', items: [
            { name: '積木', emoji: '🧱', prism: true },
            { name: '玻璃珠', emoji: '🔵', prism: false },
            { name: '派對帽', emoji: '🎉', prism: false }
        ]},
        { title: '哪一個是柱體？', items: [
            { name: '水杯', emoji: '🥛', prism: true },
            { name: '足球', emoji: '⚽', prism: false },
            { name: '雪糕筒', emoji: '🍦', prism: false }
        ]}
    ];
    let g3Index = 0, tablePick = null, openedTableExperiment = false;

    function renderG3() {
        const q = g3Data[g3Index];
        document.getElementById('g3-progress').textContent = '第 ' + (g3Index + 1) + ' / ' + g3Data.length + ' 題';
        document.getElementById('g3-title').textContent = q.title;
        document.getElementById('g3-hint').style.display = 'none';
        document.getElementById('g3-fb').textContent = '';
        document.getElementById('g3-choices').innerHTML = q.items.map((it, i) =>
            '<button class="choice" onclick="pickPrism(' + i + ')"><div class="emoji">' + it.emoji + '</div>' + it.name + '</button>'
        ).join('');
        setupTable(q.items);
    }
    function pickPrism(i) {
        const q = g3Data[g3Index];
        const buttons = document.querySelectorAll('#g3-choices .choice');
        buttons.forEach(b => b.classList.remove('correct', 'wrong'));
        if (q.items[i].prism) {
            buttons[i].classList.add('correct');
            document.getElementById('g3-fb').textContent = '正確。';
            speakNow('正確。');
            celebrate();
        } else {
            buttons[i].classList.add('wrong');
            document.getElementById('g3-fb').textContent = '請再想一想。';
            speakNow('請再想一想。');
        }
    }
    function openTableExperiment() {
        document.getElementById('table-lab').style.display = 'block';
        openedTableExperiment = true;
        document.getElementById('bulb-tip-p').classList.remove('locked');
    }
    function openPrismTip() {
        if (!openedTableExperiment) {
            document.getElementById('g3-fb').textContent = '請先按「實驗」。';
            return;
        }
        const box = document.getElementById('g3-hint');
        box.style.display = box.style.display === 'block' ? 'none' : 'block';
    }
    function setupTable(items) {
        tablePick = null;
        document.getElementById('part-top').classList.remove('found');
        document.getElementById('part-bottom').classList.remove('found');
        document.getElementById('table-objects').innerHTML = items.map((it, i) =>
            '<button class="obj" id="table-obj-' + i + '" onclick="chooseTable(' + i + ')">' + it.emoji + '<br>' + it.name + '</button>'
        ).join('');
        resetTable();
    }
    function chooseTable(i) {
        tablePick = g3Data[g3Index].items[i];
        document.querySelectorAll('#table-objects .obj').forEach(el => el.classList.remove('picked'));
        document.getElementById('table-obj-' + i).classList.add('picked');
        const el = document.getElementById('stander');
        el.textContent = tablePick.emoji;
        el.className = 'stander';
    }
    function placeOnTable() {
        if (!tablePick) return;
        const el = document.getElementById('stander');
        el.className = 'stander';
        void el.offsetWidth;
        if (!tablePick.prism) el.classList.add('fall');
    }
    function resetTable() {
        const el = document.getElementById('stander');
        el.className = 'stander';
        el.textContent = tablePick ? tablePick.emoji : '❓';
        document.getElementById('part-top').classList.remove('found');
        document.getElementById('part-bottom').classList.remove('found');
    }
    function markPart(which) {
        if (!tablePick) return;
        document.getElementById(which === 'top' ? 'part-top' : 'part-bottom').classList.add('found');
    }
    function nextG3() {
        g3Index = (g3Index + 1) % g3Data.length;
        document.getElementById('table-lab').style.display = 'none';
        document.getElementById('g3-hint').style.display = 'none';
        openedTableExperiment = false;
        document.getElementById('bulb-tip-p').classList.add('locked');
        renderG3();
    }

    renderG2();
    renderG3();
</script>
</body>
</html>
