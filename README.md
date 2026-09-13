<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>立體圖形小偵探｜平面、曲面、球體與柱體</title>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        * { box-sizing: border-box; user-select: none; }
        body {
            margin: 0;
            font-family: "PingFang HK", "Noto Sans TC", "微軟正黑體", sans-serif;
            background: linear-gradient(180deg, #e0f2fe 0%, #fef3c7 50%, #dcfce7 100%);
            min-height: 100vh;
            color: #0f172a;
        }
        .wrap {
            max-width: 980px;
            margin: 0 auto;
            padding: 16px;
        }
        header.app {
            background: #fff;
            border: 5px solid #86efac;
            border-radius: 28px;
            padding: 16px 18px;
            box-shadow: 0 10px 24px rgba(0,0,0,.08);
            text-align: center;
            margin-bottom: 14px;
        }
        header.app h1 {
            margin: 0 0 6px;
            font-size: 26px;
            background: linear-gradient(45deg, #0284c7, #ea580c);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        header.app p { margin: 0; color: #475569; font-size: 15px; }
        .tabs {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            margin: 12px 0;
        }
        .tab {
            flex: 1;
            min-width: 140px;
            border: none;
            background: #fff;
            border-radius: 16px;
            padding: 10px 8px;
            font-weight: 800;
            font-size: 15px;
            cursor: pointer;
            box-shadow: 0 4px 0 #cbd5e1;
        }
        .tab.active { background: #fde047; box-shadow: 0 4px 0 #ca8a04; }
        .panel {
            display: none;
            background: #fff;
            border-radius: 28px;
            border: 5px solid #93c5fd;
            padding: 16px;
            box-shadow: 0 10px 24px rgba(0,0,0,.08);
        }
        .panel.show { display: block; }
        .goal-banner {
            background: #eff6ff;
            border-radius: 16px;
            padding: 10px 12px;
            margin-bottom: 12px;
            font-weight: 700;
            color: #1d4ed8;
        }
        .tips {
            background: #fff7ed;
            border: 2px dashed #fb923c;
            border-radius: 16px;
            padding: 10px 12px;
            margin-bottom: 14px;
            font-size: 15px;
            line-height: 1.6;
        }
        .q-head {
            display: flex;
            align-items: flex-start;
            gap: 10px;
            justify-content: space-between;
        }
        .q-title { font-size: 20px; font-weight: 900; margin: 0 0 10px; }
        .btn-icon {
            background: #fef08a;
            border: 3px solid #eab308;
            border-radius: 50%;
            width: 48px;
            height: 48px;
            font-size: 22px;
            cursor: pointer;
            box-shadow: 0 3px 0 #ca8a04;
            flex-shrink: 0;
        }
        .btn-icon:active { transform: translateY(3px); box-shadow: none; }
        .hint-box {
            display: none;
            background: #ecfeff;
            border: 2px solid #22d3ee;
            border-radius: 14px;
            padding: 10px 12px;
            margin: 8px 0 12px;
            line-height: 1.55;
        }
        .choices {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 10px;
            margin: 12px 0;
        }
        .choice {
            background: #f8fafc;
            border: 3px solid #cbd5e1;
            border-radius: 18px;
            padding: 12px 8px;
            text-align: center;
            cursor: pointer;
            font-weight: 800;
        }
        .choice .emoji { font-size: 52px; line-height: 1.1; }
        .choice.correct { border-color: #16a34a; background: #dcfce7; }
        .choice.wrong { border-color: #dc2626; background: #fee2e2; }
        .nav-row {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 12px;
            flex-wrap: wrap;
        }
        .btn {
            border: none;
            border-radius: 999px;
            padding: 10px 22px;
            font-size: 17px;
            font-weight: 800;
            cursor: pointer;
            color: #fff;
        }
        .btn-green { background: #22c55e; box-shadow: 0 4px 0 #15803d; }
        .btn-blue { background: #3b82f6; box-shadow: 0 4px 0 #1d4ed8; }
        .btn-orange { background: #f97316; box-shadow: 0 4px 0 #c2410c; }
        .feedback { min-height: 28px; text-align: center; font-weight: 800; margin-top: 6px; }
        .progress { text-align: center; color: #64748b; margin-bottom: 8px; font-weight: 700; }

        /* Goal 1 shape cards */
        .shape-lab {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }
        @media (max-width: 640px) { .shape-lab { grid-template-columns: 1fr; } }
        .shape-preview {
            background: #f1f5f9;
            border-radius: 18px;
            min-height: 180px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            gap: 8px;
        }
        .face-btns { display: flex; flex-direction: column; gap: 8px; }
        .face-btn {
            border: 3px solid #94a3b8;
            background: #fff;
            border-radius: 14px;
            padding: 12px;
            font-weight: 800;
            cursor: pointer;
            text-align: left;
        }
        .face-btn.on-flat { border-color: #2563eb; background: #dbeafe; }
        .face-btn.on-curve { border-color: #c026d3; background: #fae8ff; }

        .cube3d {
            width: 90px; height: 90px;
            background: #93c5fd;
            transform: rotateX(-18deg) rotateY(28deg);
            box-shadow: 18px -12px 0 #60a5fa, 0 16px 0 #3b82f6;
            border: 3px solid #1d4ed8;
        }
        .sphere3d {
            width: 110px; height: 110px;
            border-radius: 50%;
            background: radial-gradient(circle at 35% 30%, #fda4af, #e11d48 70%);
            box-shadow: inset -12px -10px 0 rgba(0,0,0,.12);
        }
        .cyl3d {
            width: 70px; height: 100px;
            background: linear-gradient(#38bdf8, #0284c7);
            border-radius: 12px;
            position: relative;
            box-shadow: 0 0 0 3px #0369a1;
        }
        .cyl3d::before, .cyl3d::after {
            content: "";
            position: absolute;
            left: -3px; right: -3px;
            height: 22px;
            background: #7dd3fc;
            border: 3px solid #0369a1;
            border-radius: 50%;
        }
        .cyl3d::before { top: -12px; }
        .cyl3d::after { bottom: -12px; background: #0369a1; }

        .cone3d {
            width: 0; height: 0;
            border-left: 55px solid transparent;
            border-right: 55px solid transparent;
            border-bottom: 110px solid #fb923c;
            position: relative;
        }
        .cone3d::after {
            content: "";
            position: absolute;
            left: -46px; bottom: -16px;
            width: 92px; height: 24px;
            background: #fdba74;
            border: 3px solid #c2410c;
            border-radius: 50%;
        }

        /* Ramp */
        .lab {
            margin-top: 12px;
            background: #f8fafc;
            border-radius: 18px;
            padding: 12px;
        }
        .lab h3 { margin: 0 0 8px; }
        .objects {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            margin-bottom: 10px;
        }
        .obj {
            border: 3px solid #cbd5e1;
            background: #fff;
            border-radius: 14px;
            padding: 8px 10px;
            cursor: pointer;
            font-weight: 800;
            min-width: 86px;
            text-align: center;
        }
        .obj.picked { border-color: #2563eb; background: #dbeafe; }
        .ramp-wrap {
            position: relative;
            height: 170px;
            overflow: hidden;
            border-radius: 14px;
            background: linear-gradient(#e0f2fe, #bbf7d0);
        }
        .ramp {
            position: absolute;
            left: 20px; right: 20px; bottom: 18px;
            height: 18px;
            background: #78716c;
            transform: rotate(-18deg);
            transform-origin: left center;
            border-radius: 8px;
        }
        .roller {
            position: absolute;
            left: 28px;
            top: 28px;
            font-size: 42px;
            transition: left 1.1s ease-in, top 1.1s ease-in, transform 1.1s linear;
        }
        .roller.roll {
            left: calc(100% - 70px);
            top: 108px;
            transform: rotate(420deg);
        }
        .roller.stuck {
            left: 70px;
            top: 46px;
            transform: rotate(12deg);
        }

        /* Table stand */
        .table-wrap {
            position: relative;
            height: 170px;
            background: linear-gradient(#e0f2fe, #fef9c3);
            border-radius: 14px;
            overflow: hidden;
        }
        .tabletop {
            position: absolute;
            left: 10%; right: 10%;
            bottom: 36px;
            height: 16px;
            background: #92400e;
            border-radius: 6px;
        }
        .table-leg {
            position: absolute;
            bottom: 10px;
            width: 12px; height: 30px;
            background: #78350f;
        }
        .table-leg.l { left: 18%; }
        .table-leg.r { right: 18%; }
        .stander {
            position: absolute;
            left: 50%;
            bottom: 52px;
            transform: translateX(-50%);
            font-size: 54px;
            transition: transform .5s ease, bottom .5s ease;
        }
        .stander.fall {
            transform: translateX(-20%) rotate(78deg);
            bottom: 20px;
        }
        .highlight-parts {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            margin-top: 8px;
        }
        .part {
            background: #fff;
            border: 2px dashed #64748b;
            border-radius: 12px;
            padding: 8px 12px;
            cursor: pointer;
            font-weight: 800;
        }
        .part.found { background: #dcfce7; border-color: #16a34a; }
        .teacher {
            margin-top: 14px;
            background: #f1f5f9;
            border-radius: 16px;
            padding: 10px 12px;
            font-size: 14px;
            color: #334155;
            line-height: 1.55;
        }
        details summary { cursor: pointer; font-weight: 800; }
    </style>
</head>
<body>
<div class="wrap">
    <header class="app">
        <h1>立體圖形小偵探</h1>
        <p>平面同曲面 · 球體 · 柱體　（小一至小三探究學習）</p>
    </header>

    <div class="tabs">
        <button class="tab active" onclick="showTab('g1', this)">目標一　平面／曲面</button>
        <button class="tab" onclick="showTab('g2', this)">目標二　球體</button>
        <button class="tab" onclick="showTab('g3', this)">目標三　柱體</button>
    </div>

    <!-- GOAL 1 -->
    <section id="g1" class="panel show">
        <div class="goal-banner">目標一：找出立體圖形中的「平面」同「曲面」</div>
        <div class="tips">
            <strong>小貼士：</strong><br>
            🖐️ 用手「摸」：摸落去平平哋、可以貼實枱面 → <b>平面</b><br>
            🖐️ 摸落去彎彎哋、會滑走 → <b>曲面</b><br>
            👀 用眼「睇」：有直邊圍住嘅面多數係平面；圓滾滾、冇直邊嘅面多數係曲面。
        </div>
        <div class="progress" id="g1-progress"></div>
        <div class="q-head">
            <h2 class="q-title" id="g1-title"></h2>
            <button class="btn-icon" title="提示" onclick="toggleHint('g1-hint')">💡</button>
        </div>
        <div class="hint-box" id="g1-hint"></div>
        <div id="g1-body"></div>
        <div class="feedback" id="g1-fb"></div>
        <div class="nav-row">
            <button class="btn btn-green" onclick="checkG1()">核對答案</button>
            <button class="btn btn-blue" onclick="nextG1()">下一題</button>
            <button class="btn btn-orange" onclick="speakNow(g1Data[g1Index].speak)">🗣️ 讀題</button>
        </div>
        <div class="teacher">
            <details>
                <summary>教師備註（課堂建議）</summary>
                <p>建議先用實物：積木、罐、波、雪糕筒。請學生閉眼摸一摸，再講「平定彎」。</p>
                <p>常見迷思：圓柱側面「望落好似長方形」，但其實係曲面；圓形底先係平面。</p>
                <p>延伸：數一數每個立體有幾多個平面、幾多個曲面。</p>
            </details>
        </div>
    </section>

    <!-- GOAL 2 -->
    <section id="g2" class="panel">
        <div class="goal-banner">目標二：指出球體特性——可滾動、無邊亦無角</div>
        <div class="tips">
            <strong>小貼士：</strong>球體四面都係曲面，冇邊冇角，放斜道會滾走。橙、足球似球體；雪糕筒、紙巾盒都唔係。
        </div>
        <div class="progress" id="g2-progress"></div>
        <div class="q-head">
            <h2 class="q-title" id="g2-title"></h2>
            <button class="btn-icon" title="提示" onclick="showSphereHint()">💡</button>
        </div>
        <div class="hint-box" id="g2-hint">
            球體特性：① 可以滾動　② 無邊　③ 無角。<br>
            唔肯定？將物件放到下面斜道試吓，會滾落去嘅先至似球體。
        </div>
        <div class="choices" id="g2-choices"></div>
        <div class="feedback" id="g2-fb"></div>
        <div class="nav-row">
            <button class="btn btn-blue" onclick="nextG2()">下一題</button>
            <button class="btn btn-orange" onclick="speakNow(g2Data[g2Index].speak)">🗣️ 讀題</button>
        </div>

        <div class="lab" id="ramp-lab" style="display:none;">
            <h3>🔬 斜道實驗：邊樣會滾落去？</h3>
            <p>先揀一件物件，再撳「放上斜道」。</p>
            <div class="objects" id="ramp-objects"></div>
            <div class="nav-row" style="margin:8px 0;">
                <button class="btn btn-green" onclick="releaseRamp()">放上斜道</button>
                <button class="btn btn-blue" onclick="resetRamp()">重設</button>
            </div>
            <div class="ramp-wrap">
                <div class="ramp"></div>
                <div class="roller" id="roller">❓</div>
            </div>
            <div class="feedback" id="ramp-fb"></div>
        </div>
    </section>

    <!-- GOAL 3 -->
    <section id="g3" class="panel">
        <div class="goal-banner">目標三：指出柱體特性——可豎立，頂同底都係平嘅</div>
        <div class="tips">
            <strong>小貼士：</strong>柱體可以穩穩企喺枱上。頂同底都係平面，而且形狀一樣。紙巾盒、罐、積木柱都係柱體；波同雪糕筒就唔係（雪糕筒係錐體）。
        </div>
        <div class="progress" id="g3-progress"></div>
        <div class="q-head">
            <h2 class="q-title" id="g3-title"></h2>
            <button class="btn-icon" title="提示" onclick="showPrismHint()">💡</button>
        </div>
        <div class="hint-box" id="g3-hint">
            柱體特性：① 可以豎立　② 底部係平　③ 頂部都係平。<br>
            試吓將物件「企」喺枱上，再指出頂同底。
        </div>
        <div class="choices" id="g3-choices"></div>
        <div class="feedback" id="g3-fb"></div>
        <div class="nav-row">
            <button class="btn btn-blue" onclick="nextG3()">下一題</button>
            <button class="btn btn-orange" onclick="speakNow(g3Data[g3Index].speak)">🗣️ 讀題</button>
        </div>

        <div class="lab" id="table-lab" style="display:none;">
            <h3>🔬 枱面實驗：邊樣可以豎立？</h3>
            <p>揀一件物件，撳「放到枱上」。之後再指出柱體嘅頂同底。</p>
            <div class="objects" id="table-objects"></div>
            <div class="nav-row" style="margin:8px 0;">
                <button class="btn btn-green" onclick="placeOnTable()">放到枱上</button>
                <button class="btn btn-blue" onclick="resetTable()">重設</button>
            </div>
            <div class="table-wrap">
                <div class="stander" id="stander">❓</div>
                <div class="tabletop"></div>
                <div class="table-leg l"></div>
                <div class="table-leg r"></div>
            </div>
            <div class="feedback" id="table-fb"></div>
            <div class="highlight-parts">
                <button class="part" id="part-top" onclick="markPart('top')">指出頂部（平面）</button>
                <button class="part" id="part-bottom" onclick="markPart('bottom')">指出底部（平面）</button>
            </div>
        </div>
    </section>
</div>

<script>
    function speakNow(text) {
        if (!('speechSynthesis' in window)) return;
        speechSynthesis.cancel();
        const u = new SpeechSynthesisUtterance(text);
        u.lang = 'zh-HK';
        u.rate = 0.88;
        u.pitch = 1.08;
        speechSynthesis.speak(u);
    }
    function showTab(id, btn) {
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('show'));
        document.getElementById(id).classList.add('show');
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        btn.classList.add('active');
    }
    function toggleHint(id) {
        const el = document.getElementById(id);
        el.style.display = el.style.display === 'block' ? 'none' : 'block';
    }
    function celebrate() {
        if (window.confetti) confetti({ particleCount: 90, spread: 70, origin: { y: 0.7 } });
    }

    /* ===== 目標一 ===== */
    const g1Data = [
        {
            title: '立方體（積木）有邊啲面係平面？邊啲係曲面？',
            speak: '立方體有邊啲面係平面？邊啲係曲面？',
            hint: '立方體好似骰仔。六個面都摸落去平平哋，可以貼實枱面。',
            preview: '<div class="cube3d"></div><div>立方體</div>',
            faces: [
                { name: '上面', type: 'flat' },
                { name: '前面', type: 'flat' },
                { name: '側面', type: 'flat' }
            ],
            ask: '撳一撳每個面，揀「平面」定「曲面」。其實三個面全部都係平面。',
            answerNote: '立方體全部都係平面，冇曲面。'
        },
        {
            title: '球體（波）嘅表面係平面定曲面？',
            speak: '球體嘅表面係平面定曲面？',
            hint: '波摸落去四面都彎，冇一塊可以完全貼實枱面。',
            preview: '<div class="sphere3d"></div><div>球體</div>',
            faces: [{ name: '整個表面', type: 'curve' }],
            ask: '揀一揀：整個表面係平面定曲面？',
            answerNote: '球體只有曲面，冇平面、冇邊、冇角。'
        },
        {
            title: '圓柱體（罐）邊度平？邊度彎？',
            speak: '圓柱體邊度平？邊度彎？',
            hint: '頂同底可以企穩，係圓形平面；中間一圈摸落去彎彎哋，係曲面。',
            preview: '<div class="cyl3d"></div><div>圓柱體</div>',
            faces: [
                { name: '頂部圓面', type: 'flat' },
                { name: '底部圓面', type: 'flat' },
                { name: '側面一圈', type: 'curve' }
            ],
            ask: '為每個部分揀平面或曲面。',
            answerNote: '圓柱有 2 個平面（頂、底）同 1 個曲面（側面）。'
        },
        {
            title: '圓錐體（派對帽／雪糕筒）邊度平？邊度彎？',
            speak: '圓錐體邊度平？邊度彎？',
            hint: '底部圓面係平面，可以放平；尖尖嗰面一圈係曲面。',
            preview: '<div class="cone3d"></div><div style="margin-top:18px">圓錐體</div>',
            faces: [
                { name: '底部圓面', type: 'flat' },
                { name: '斜斜嘅側面', type: 'curve' }
            ],
            ask: '為每個部分揀平面或曲面。',
            answerNote: '圓錐有 1 個平面（底）同 1 個曲面（側面）。'
        }
    ];
    let g1Index = 0;
    let g1Picks = {};

    function renderG1() {
        const q = g1Data[g1Index];
        document.getElementById('g1-progress').textContent = `第 ${g1Index + 1} / ${g1Data.length} 題`;
        document.getElementById('g1-title').textContent = q.title;
        document.getElementById('g1-hint').innerHTML = q.hint;
        document.getElementById('g1-hint').style.display = 'none';
        document.getElementById('g1-fb').textContent = '';
        g1Picks = {};
        let facesHtml = '';
        q.faces.forEach((f, i) => {
            facesHtml += `<div class="face-btn" id="face-${i}">
                ${f.name}
                <div style="margin-top:6px;">
                    <button class="btn btn-blue" style="padding:6px 12px;font-size:14px;" onclick="pickFace(${i},'flat')">平面</button>
                    <button class="btn btn-orange" style="padding:6px 12px;font-size:14px;" onclick="pickFace(${i},'curve')">曲面</button>
                </div>
            </div>`;
        });
        document.getElementById('g1-body').innerHTML = `
            <p>${q.ask}</p>
            <div class="shape-lab">
                <div class="shape-preview">${q.preview}</div>
                <div class="face-btns">${facesHtml}</div>
            </div>`;
    }
    function pickFace(i, type) {
        g1Picks[i] = type;
        const el = document.getElementById('face-' + i);
        el.classList.remove('on-flat', 'on-curve');
        el.classList.add(type === 'flat' ? 'on-flat' : 'on-curve');
        speakNow(type === 'flat' ? '平面' : '曲面');
    }
    function checkG1() {
        const q = g1Data[g1Index];
        let ok = true;
        q.faces.forEach((f, i) => {
            if (g1Picks[i] !== f.type) ok = false;
        });
        const fb = document.getElementById('g1-fb');
        if (Object.keys(g1Picks).length < q.faces.length) {
            fb.textContent = '請先為每一個面作出選擇。';
            speakNow('請先為每一個面作出選擇');
            return;
        }
        if (ok) {
            fb.textContent = '✅ 答得啱！' + q.answerNote;
            speakNow('答得啱！');
            celebrate();
        } else {
            fb.textContent = '再摸一摸、想一想：邊啲可以貼實枱面？';
            speakNow('再試一次');
        }
    }
    function nextG1() {
        g1Index = (g1Index + 1) % g1Data.length;
        renderG1();
    }

    /* ===== 目標二 ===== */
    const g2Data = [
        { title: '以下邊一樣係球體？', speak: '橙、雪糕同紙巾盒，邊一樣係球體？',
          items: [
            { name: '橙', emoji: '🍊', sphere: true },
            { name: '雪糕', emoji: '🍦', sphere: false },
            { name: '紙巾盒', emoji: '📦', sphere: false }
          ]},
        { title: '以下邊一樣係球體？', speak: '足球、書本同水杯，邊一樣係球體？',
          items: [
            { name: '足球', emoji: '⚽', sphere: true },
            { name: '書本', emoji: '📘', sphere: false },
            { name: '水杯', emoji: '🥤', sphere: false }
          ]},
        { title: '以下邊一樣係球體？', speak: '西瓜、金字塔同積木，邊一樣係球體？',
          items: [
            { name: '西瓜', emoji: '🍉', sphere: true },
            { name: '金字塔', emoji: '🔺', sphere: false },
            { name: '積木', emoji: '🧊', sphere: false }
          ]},
        { title: '以下邊一樣係球體？', speak: '玻璃珠、罐同派對帽，邊一樣係球體？',
          items: [
            { name: '玻璃珠', emoji: '🔵', sphere: true },
            { name: '罐', emoji: '🥫', sphere: false },
            { name: '派對帽', emoji: '🎉', sphere: false }
          ]}
    ];
    let g2Index = 0;
    let rampPick = null;

    function renderG2() {
        const q = g2Data[g2Index];
        document.getElementById('g2-progress').textContent = `第 ${g2Index + 1} / ${g2Data.length} 題`;
        document.getElementById('g2-title').textContent = q.title;
        document.getElementById('g2-hint').style.display = 'none';
        document.getElementById('g2-fb').textContent = '';
        document.getElementById('g2-choices').innerHTML = q.items.map((it, i) =>
            `<button class="choice" onclick="pickSphere(${i})"><div class="emoji">${it.emoji}</div>${it.name}</button>`
        ).join('');
        setupRamp(q.items);
    }
    function pickSphere(i) {
        const q = g2Data[g2Index];
        const buttons = document.querySelectorAll('#g2-choices .choice');
        buttons.forEach(b => b.classList.remove('correct', 'wrong'));
        if (q.items[i].sphere) {
            buttons[i].classList.add('correct');
            document.getElementById('g2-fb').textContent = '✅ 啱啦！球體可以滾動，而且無邊無角。';
            speakNow('答得啱，呢樣係球體');
            celebrate();
        } else {
            buttons[i].classList.add('wrong');
            document.getElementById('g2-fb').textContent = '再諗諗：佢有冇邊有冇角？放斜道會唔會滾？撳💡試實驗。';
            speakNow('再試一次，可以用斜道實驗');
        }
    }
    function showSphereHint() {
        const box = document.getElementById('g2-hint');
        box.style.display = box.style.display === 'block' ? 'none' : 'block';
        document.getElementById('ramp-lab').style.display = 'block';
        speakNow('球體可以滾動，無邊亦無角。試吓放到斜道上。');
    }
    function setupRamp(items) {
        rampPick = null;
        document.getElementById('ramp-objects').innerHTML = items.map((it, i) =>
            `<button class="obj" id="ramp-obj-${i}" onclick="chooseRamp(${i})">${it.emoji}<br>${it.name}</button>`
        ).join('');
        resetRamp();
    }
    function chooseRamp(i) {
        rampPick = g2Data[g2Index].items[i];
        document.querySelectorAll('#ramp-objects .obj').forEach(el => el.classList.remove('picked'));
        document.getElementById('ramp-obj-' + i).classList.add('picked');
        document.getElementById('roller').textContent = rampPick.emoji;
        document.getElementById('roller').className = 'roller';
        document.getElementById('ramp-fb').textContent = '已揀：' + rampPick.name + '，可以放上斜道。';
    }
    function releaseRamp() {
        if (!rampPick) {
            document.getElementById('ramp-fb').textContent = '請先揀一件物件。';
            return;
        }
        const roller = document.getElementById('roller');
        roller.className = 'roller';
        void roller.offsetWidth;
        if (rampPick.sphere) {
            roller.classList.add('roll');
            document.getElementById('ramp-fb').textContent = rampPick.name + '滾落斜道喇！所以佢係球體。';
            speakNow(rampPick.name + '滾落去，係球體');
        } else {
            roller.classList.add('stuck');
            document.getElementById('ramp-fb').textContent = rampPick.name + '滾唔到（或者只係滑一下），因為佢唔係球體。';
            speakNow(rampPick.name + '滾唔到，唔係球體');
        }
    }
    function resetRamp() {
        document.getElementById('roller').className = 'roller';
        document.getElementById('roller').textContent = rampPick ? rampPick.emoji : '❓';
        document.getElementById('ramp-fb').textContent = '';
    }
    function nextG2() {
        g2Index = (g2Index + 1) % g2Data.length;
        document.getElementById('ramp-lab').style.display = 'none';
        renderG2();
    }

    /* ===== 目標三 ===== */
    const g3Data = [
        { title: '以下邊一樣係柱體？', speak: '圓柱罐、波同雪糕，邊一樣係柱體？',
          items: [
            { name: '圓柱罐', emoji: '🥫', prism: true },
            { name: '波', emoji: '🏀', prism: false },
            { name: '雪糕', emoji: '🍦', prism: false }
          ]},
        { title: '紙巾盒同橙，邊一樣係柱體？', speak: '紙巾盒同橙，邊一樣係柱體？',
          items: [
            { name: '紙巾盒', emoji: '📦', prism: true },
            { name: '橙', emoji: '🍊', prism: false }
          ]},
        { title: '以下邊一樣係柱體？', speak: '積木柱、玻璃珠同派對帽，邊一樣係柱體？',
          items: [
            { name: '積木柱', emoji: '🧱', prism: true },
            { name: '玻璃珠', emoji: '🔵', prism: false },
            { name: '派對帽', emoji: '🎉', prism: false }
          ]},
        { title: '以下邊一樣係柱體？', speak: '水杯、足球同雪糕筒，邊一樣係柱體？',
          items: [
            { name: '水杯', emoji: '🥛', prism: true },
            { name: '足球', emoji: '⚽', prism: false },
            { name: '雪糕筒', emoji: '🍦', prism: false }
          ]}
    ];
    let g3Index = 0;
    let tablePick = null;
    let foundTop = false, foundBottom = false;

    function renderG3() {
        const q = g3Data[g3Index];
        document.getElementById('g3-progress').textContent = `第 ${g3Index + 1} / ${g3Data.length} 題`;
        document.getElementById('g3-title').textContent = q.title;
        document.getElementById('g3-hint').style.display = 'none';
        document.getElementById('g3-fb').textContent = '';
        document.getElementById('g3-choices').innerHTML = q.items.map((it, i) =>
            `<button class="choice" onclick="pickPrism(${i})"><div class="emoji">${it.emoji}</div>${it.name}</button>`
        ).join('');
        setupTable(q.items);
    }
    function pickPrism(i) {
        const q = g3Data[g3Index];
        const buttons = document.querySelectorAll('#g3-choices .choice');
        buttons.forEach(b => b.classList.remove('correct', 'wrong'));
        if (q.items[i].prism) {
            buttons[i].classList.add('correct');
            document.getElementById('g3-fb').textContent = '✅ 啱啦！柱體可以豎立，頂同底都係平嘅。';
            speakNow('答得啱，呢樣係柱體');
            celebrate();
        } else {
            buttons[i].classList.add('wrong');
            document.getElementById('g3-fb').textContent = '再諗諗：佢可唔可以穩穩企喺枱上？頂同底平唔平？撳💡試實驗。';
            speakNow('再試一次，可以用枱面實驗');
        }
    }
    function showPrismHint() {
        const box = document.getElementById('g3-hint');
        box.style.display = box.style.display === 'block' ? 'none' : 'block';
        document.getElementById('table-lab').style.display = 'block';
        speakNow('柱體可以豎立，底部同頂部都係平嘅。試吓放到枱上。');
    }
    function setupTable(items) {
        tablePick = null;
        foundTop = foundBottom = false;
        document.getElementById('part-top').classList.remove('found');
        document.getElementById('part-bottom').classList.remove('found');
        document.getElementById('table-objects').innerHTML = items.map((it, i) =>
            `<button class="obj" id="table-obj-${i}" onclick="chooseTable(${i})">${it.emoji}<br>${it.name}</button>`
        ).join('');
        resetTable();
    }
    function chooseTable(i) {
        tablePick = g3Data[g3Index].items[i];
        document.querySelectorAll('#table-objects .obj').forEach(el => el.classList.remove('picked'));
        document.getElementById('table-obj-' + i).classList.add('picked');
        document.getElementById('stander').textContent = tablePick.emoji;
        document.getElementById('stander').className = 'stander';
        document.getElementById('table-fb').textContent = '已揀：' + tablePick.name + '，可以放到枱上。';
    }
    function placeOnTable() {
        if (!tablePick) {
            document.getElementById('table-fb').textContent = '請先揀一件物件。';
            return;
        }
        const el = document.getElementById('stander');
        el.className = 'stander';
        void el.offsetWidth;
        if (tablePick.prism) {
            el.classList.remove('fall');
            document.getElementById('table-fb').textContent = tablePick.name + '可以穩穩豎立！因為頂同底都係平面。而家試指出頂部同底部。';
            speakNow(tablePick.name + '可以豎立，係柱體');
        } else {
            el.classList.add('fall');
            document.getElementById('table-fb').textContent = tablePick.name + '企唔穩，所以唔係柱體。';
            speakNow(tablePick.name + '企唔穩，唔係柱體');
        }
    }
    function resetTable() {
        const el = document.getElementById('stander');
        el.className = 'stander';
        el.textContent = tablePick ? tablePick.emoji : '❓';
        document.getElementById('table-fb').textContent = '';
        foundTop = foundBottom = false;
        document.getElementById('part-top').classList.remove('found');
        document.getElementById('part-bottom').classList.remove('found');
    }
    function markPart(which) {
        if (!tablePick || !tablePick.prism) {
            document.getElementById('table-fb').textContent = '請先揀一件可以豎立嘅柱體，再指出頂同底。';
            speakNow('請先揀柱體');
            return;
        }
        if (which === 'top') {
            foundTop = true;
            document.getElementById('part-top').classList.add('found');
            speakNow('頂部係平面');
        } else {
            foundBottom = true;
            document.getElementById('part-bottom').classList.add('found');
            speakNow('底部係平面');
        }
        if (foundTop && foundBottom) {
            document.getElementById('table-fb').textContent = '做得好！你已指出柱體嘅頂同底都係平面，所以方形盒、罐、積木柱都係柱體。';
            celebrate();
        }
    }
    function nextG3() {
        g3Index = (g3Index + 1) % g3Data.length;
        document.getElementById('table-lab').style.display = 'none';
        renderG3();
    }

    renderG1();
    renderG2();
    renderG3();
</script>
</body>
</html>
