# Lawww
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>116法律修煉 17.5：職涯藍圖</title>
    <style id="dynamic-font">
        :root { --main-font: 'Noto Serif TC', serif; }
    </style>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Serif+TC:wght@900&family=Noto+Sans+TC:wght@400;700&family=Zhi+Mang+Xing&display=swap');
        
        :root { --law-red: #800000; --gold: #d4af37; --bg: #0a0a0a; --card: #151515; --text: #ffffff; }
        
        body { font-family: var(--main-font), 'Noto Sans TC', sans-serif; background-color: var(--bg); color: var(--text); margin: 0; padding: 0; display: flex; flex-direction: column; align-items: center; min-height: 100vh; }
        
        /* 導航 */
        nav { width: 100%; background: linear-gradient(to bottom, var(--law-red), #500000); padding: 15px; position: sticky; top: 0; z-index: 1000; border-bottom: 2px solid var(--gold); }
        .stats-grid { display: flex; justify-content: space-around; align-items: center; max-width: 500px; margin: 0 auto; cursor: pointer; }
        .exp-bar-bg { background: rgba(0,0,0,0.6); border-radius: 10px; height: 12px; width: 120px; overflow: hidden; border: 1px solid var(--gold); }
        .exp-bar-fill { background: linear-gradient(90deg, #d4af37, #fff, #d4af37); background-size: 200% 100%; animation: shine 2s infinite linear; height: 100%; width: 0%; transition: width 0.8s ease; }
        @keyframes shine { 0% { background-position: 200% 0; } 100% { background-position: -200% 0; } }

        /* 通用遮罩 */
        .overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.95); z-index: 3000; flex-direction: column; justify-content: center; align-items: center; text-align: center; }

        /* 內容區 */
        .content { display: none; width: 95%; max-width: 500px; margin: 20px 0 120px 0; animation: fadeIn 0.4s; }
        .content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .card { background: var(--card); border-radius: 15px; padding: 20px; margin-bottom: 15px; border: 1px solid #2a2a2a; }
        .btn { background: var(--law-red); color: white; border: 1px solid var(--gold); padding: 12px; border-radius: 10px; width: 100%; font-weight: bold; cursor: pointer; transition: 0.2s; }
        .btn:active { transform: scale(0.98); opacity: 0.8; }

        /* 課表專用 CSS */
        #timetable-table { width:100%; border-collapse: collapse; font-size: 0.75rem; min-width: 450px; }
        #timetable-table td { border: 1px solid #333; height: 45px; text-align: center; cursor: pointer; padding: 2px; }
        #timetable-table td:hover { background: #1a1a1a; }
        #timetable-table th { color: var(--gold); padding: 8px 0; border-bottom: 2px solid var(--gold); }

        /* 筆記專用 CSS */
        .note-card { background: #1a1a1a; border-left: 4px solid var(--law-red); padding: 15px; margin-bottom: 15px; border-radius: 0 10px 10px 0; position: relative; }
        .note-card h4 { color: var(--gold); margin: 0 0 8px 0; font-size: 1.1rem; }
        .note-card p { font-size: 0.9rem; color: #ccc; line-height: 1.5; white-space: pre-wrap; margin: 0; }
        .note-card img { max-width: 100%; border-radius: 5px; margin-top: 10px; border: 1px solid #333; }
        .note-date { font-size: 0.65rem; color: #555; margin-top: 10px; text-align: right; }

        /* 選單欄 */
        .menu-bar { display: grid; grid-template-columns: repeat(5, 1fr); gap: 5px; width: 100%; max-width: 500px; padding: 12px; background: #111; position: fixed; bottom: 0; border-top: 1px solid var(--gold); z-index: 1000; }
        .menu-item { text-align: center; color: #666; font-size: 0.65rem; cursor: pointer; }
        .menu-item.active { color: var(--gold); font-weight: bold; }

        input[type="text"], input[type="date"], select, textarea { 
            width: 100%; padding: 10px; background: #000; border: 1px solid #333; color: #fff; border-radius: 8px; box-sizing: border-box; margin-top: 5px;
        }
    </style>
</head>
<body>

<div id="level-up-overlay" class="overlay">
    <div style="font-family:'Noto Serif TC'; font-size:1.5rem; color:var(--gold);">LEVEL UP</div>
    <div id="pop-rank" style="font-family:'Noto Serif TC'; font-size:3.5rem; color:#fff; text-shadow:0 0 20px var(--gold); margin:20px 0;">法律素人</div>
    <button class="btn" style="width:200px;" onclick="closeOverlay('level-up-overlay')">確認晉升</button>
</div>

<div id="rank-list-overlay" class="overlay" onclick="closeOverlay('rank-list-overlay')">
    <div class="card" style="width: 85%; max-width: 400px;" onclick="event.stopPropagation()">
        <h2 style="color:var(--gold); margin-top:0;">⚖️ 法律修煉體系</h2>
        <div id="rank-items-container"></div>
        <button class="btn" style="margin-top:20px;" onclick="closeOverlay('rank-list-overlay')">關閉</button>
    </div>
</div>

<nav>
    <div class="stats-grid" onclick="showRankList()">
        <div style="text-align:center;">
            <div id="rank-tag" style="font-size:0.6rem; color:var(--gold);">法律素人</div>
            <div id="lv-num" style="font-size:1.4rem; font-weight:900;">LV.01</div>
        </div>
        <div style="flex:1; margin-left:15px;">
            <div id="exp-label" style="font-size:0.7rem; text-align:right; color:var(--gold);">0 / 100</div>
            <div class="exp-bar-bg"><div id="exp-fill" class="exp-bar-fill"></div></div>
        </div>
    </div>
</nav>

<div id="p1" class="content active">
    <div class="card" style="text-align:center;">
        <h3 style="margin:0; color:#888;">修煉目標：<span id="display-goal">台大法律系</span></h3>
        <h1 style="color:var(--gold); font-size:3.5rem; margin:15px 0;">D-<span id="dday-num"></span></h1>
        <p style="font-size:0.8rem;">決戰日期：<span id="display-date">2027-01-16</span></p>
    </div>
    <div class="card">
        <h3>🏛️ 今日核心條文</h3>
        <div id="case-display" style="font-size:1rem; line-height:1.6; color:#ddd; margin:15px 0; background:#000; padding:12px; border-radius:8px;">尚未領取...</div>
        <button id="case-btn" class="btn" onclick="getDailyCase()">領取條文 (+5 EXP)</button>
    </div>
</div>

<div id="p2" class="content">
    <div class="card">
        <h3 style="color:var(--gold);">📌 每日鋼鐵任務 (各 +10)</h3>
        <div id="fixed-task-list"></div>
    </div>
    <div class="card">
        <h3>⚔️ 額外加成任務 (+5)</h3>
        <div id="task-list"></div>
        <input type="text" id="t-in" placeholder="新增臨時任務...">
        <button class="btn" style="margin-top:10px;" onclick="addTask()">新增</button>
    </div>
</div>

<div id="p3" class="content">
    <div class="card" style="padding: 10px; overflow-x: auto;">
        <h3 style="color:var(--gold); text-align:center;">📅 每週修煉課表</h3>
        <table id="timetable-table">
            <thead>
                <tr>
                    <th style="width: 50px;">時段</th>
                    <th>一</th><th>二</th><th>三</th><th>四</th><th>五</th><th>六</th><th>日</th>
                </tr>
            </thead>
            <tbody id="timetable-body"></tbody>
        </table>
        <p style="font-size: 0.7rem; color: #666; margin-top: 10px;">* 點擊儲存格編輯內容</p>
    </div>
</div>

<div id="p4" class="content">
    <div class="card">
        <h3 style="color:var(--gold);">📜 數位卷宗</h3>
        <input type="text" id="note-title" placeholder="卷宗標題...">
        <textarea id="note-content" placeholder="輸入修煉心得..."></textarea>
        <div style="display: flex; gap: 10px; margin-top: 10px;">
            <label class="btn" style="flex:1; background:#333; text-align:center; font-size:0.8rem;">
                📷 附上證據
                <input type="file" id="note-image" accept="image/*" style="display:none;" onchange="previewImage(this)">
            </label>
            <button class="btn" style="flex:1;" onclick="addNote()">歸檔 (+5 EXP)</button>
        </div>
        <div id="img-preview-container" style="display:none; margin-top:10px; text-align:center;">
            <img id="img-preview" src="" style="max-width:100px; border:1px solid var(--gold);">
        </div>
    </div>
    <div id="notes-display-list"></div>
</div>

<div id="p5" class="content">
    <div class="card">
        <h3 style="color:var(--gold);">⚙️ 個人偏好</h3>
        <label>字體選擇：</label>
        <select id="font-select" onchange="updateSettings()">
            <option value="'Noto Serif TC', serif">⚖️ 經典法典</option>
            <option value="'Noto Sans TC', sans-serif">📱 俐落黑體</option>
            <option value="'Zhi Mang Xing', cursive">✍️ 隨筆手寫</option>
        </select>
        <label style="display:block; margin-top:15px;">目標志願：</label>
        <input type="text" id="set-goal" oninput="updateSettings()">
        <label style="display:block; margin-top:15px;">日期設定：</label>
        <input type="date" id="set-date" onchange="updateSettings()">
        <button class="btn" style="background:#444; margin-top:30px;" onclick="resetAll()">格式化所有數據</button>
    </div>
</div>

<div class="menu-bar">
    <div class="menu-item active" onclick="tab('p1')">🏠<br>主頁</div>
    <div class="menu-item" onclick="tab('p2')">⚔️<br>任務</div>
    <div class="menu-item" onclick="tab('p3')">📅<br>課表</div>
    <div class="menu-item" onclick="tab('p4')">✍️<br>筆記</div>
    <div class="menu-item" onclick="tab('p5')">⚙️<br>設定</div>
</div>

<script>
    let state = { 
        lv: 1, exp: 0, tasks: [], tt: {}, notes: [], 
        lastCaseDate: "", lastResetDate: "", caseIndex: 0, currentLaw: null,
        goal: "台大法律系", date: "2027-01-16", font: "'Noto Serif TC', serif",
        fixedTasks: [
            { id: 'f1', t: "【法條】精讀今日條文", done: false },
            { id: 'f2', t: "【英數】完成每日練習", done: false },
            { id: 'f3', t: "【課業】複習學校重點", done: false },
            { id: 'f4', t: "【錯題】分析三道錯題", done: false },
            { id: 'f5', t: "【自律】遠離手機 2 小時", done: false }
        ]
    };

    const rankSystem = [
        { icon: "🌱", name: "法律素人", req: "LV.1" },
        { icon: "📜", name: "法學新秀", req: "LV.20" },
        { icon: "⚖️", name: "法典守護者", req: "LV.50" },
        { icon: "🛡️", name: "正義辯護人", req: "LV.70" },
        { icon: "🏛️", name: "傳奇大法官", req: "LV.98" }
    ];

    const lawPedia = [
        { t: "憲法 第1條", d: "中華民國基於三民主義，為民有民治民享之民主共和國。" },
        { t: "刑法 第1條", d: "罪刑法定：行為之處罰，以行為時之法律有明文規定者為限。" },
        { t: "民法 第6條", d: "人之權利能力，始於出生，終於死亡。" },
        { t: "民法 第12條", d: "滿十八歲為成年。" }
    ];

    let currentImageData = "";

    function init() {
        const saved = localStorage.getItem("LAW_V17_FINAL");
        if(saved) state = JSON.parse(saved);
        
        // 介面初始化
        document.getElementById('font-select').value = state.font;
        document.getElementById('set-goal').value = state.goal;
        document.getElementById('set-date').value = state.date;
        applyFont(state.font);
        
        checkDailyReset(); 
        renderUI(); 
        renderTasks(); 
        renderTimetable();
        renderNotes();
        updateDDay();
    }

    // 核心邏輯
    function tab(id) {
        document.querySelectorAll('.content').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.menu-item').forEach(m => m.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        event.currentTarget.classList.add('active');
    }

    function applyFont(f) { document.getElementById('dynamic-font').innerHTML = `:root { --main-font: ${f}; }`; }

    function saveData() { localStorage.setItem("LAW_V17_FINAL", JSON.stringify(state)); }

    function gainExp(amt) {
        state.exp += amt;
        let need = Math.floor(100 * Math.pow(1.2, state.lv-1));
        if(state.exp >= need) {
            state.exp -= need; state.lv++;
            showLevelUp();
        }
        renderUI(); saveData();
    }

    function showLevelUp() {
        const idx = Math.min(Math.floor(state.lv / 10), 4);
        document.getElementById('pop-rank').innerText = rankSystem[idx].name;
        document.getElementById('level-up-overlay').style.display = 'flex';
    }

    function closeOverlay(id) { document.getElementById(id).style.display = 'none'; }

    // 條文與任務
    function getDailyCase() {
        state.currentLaw = lawPedia[state.caseIndex % lawPedia.length];
        state.lastCaseDate = new Date().toDateString();
        state.caseIndex++;
        gainExp(5); checkDailyReset();
    }

    function checkDailyReset() {
        const today = new Date().toDateString();
        const btn = document.getElementById('case-btn');
        if (state.lastCaseDate === today) {
            btn.disabled = true; btn.innerText = "今日修煉完畢";
            if (state.currentLaw) document.getElementById('case-display').innerHTML = `<b style="color:var(--gold);">${state.currentLaw.t}</b><br>${state.currentLaw.d}`;
        }
        if (state.lastResetDate !== today) {
            state.fixedTasks.forEach(t => t.done = false);
            state.lastResetDate = today; saveData();
        }
    }

    function finishFixed(id) {
        const t = state.fixedTasks.find(x => x.id === id);
        if(!t.done) { t.done = true; gainExp(10); renderTasks(); }
    }

    function addTask() {
        const t = document.getElementById('t-in').value;
        if(t){ state.tasks.push({id:Date.now(), t}); renderTasks(); saveData(); document.getElementById('t-in').value=""; }
    }

    function finishTask(id) { state.tasks = state.tasks.filter(x=>x.id!==id); gainExp(5); renderTasks(); }

    function renderTasks() {
        document.getElementById('fixed-task-list').innerHTML = state.fixedTasks.map(t=>`
            <div style="display:flex; align-items:center; padding:10px 0; border-bottom:1px solid #222; opacity:${t.done?0.3:1}">
                <input type="checkbox" ${t.done?'checked disabled':''} onclick="finishFixed('${t.id}')" style="width:18px; height:18px; margin-right:10px;">
                <span style="flex:1;">${t.t}</span><b style="color:var(--gold);">+10</b>
            </div>
        `).join('');
        document.getElementById('task-list').innerHTML = state.tasks.map(t=>`
            <div style="display:flex; align-items:center; padding:10px 0; border-bottom:1px solid #222;">
                <input type="checkbox" onclick="finishTask(${t.id})" style="width:18px; height:18px; margin-right:10px;">
                <span style="flex:1;">${t.t}</span><b style="color:var(--gold);">+5</b>
            </div>
        `).join('');
    }

    // 課表系統
    function renderTimetable() {
        const hours = ["早讀", "01", "02", "03", "04", "午休", "05", "06", "07", "08", "晚自習"];
        let html = "";
        hours.forEach((h, rIdx) => {
            html += `<tr><td style="color:var(--gold); font-weight:bold;">${h}</td>`;
            for (let cIdx = 0; cIdx < 7; cIdx++) {
                const id = `${rIdx}-${cIdx}`;
                html += `<td onclick="editCell('${id}')" id="cell-${id}">${state.tt[id] || ""}</td>`;
            }
            html += `</tr>`;
        });
        document.getElementById('timetable-body').innerHTML = html;
    }

    function editCell(id) {
        const val = prompt("輸入科目/任務：", state.tt[id] || "");
        if (val !== null) { state.tt[id] = val; renderTimetable(); saveData(); }
    }

    // 筆記系統
    function previewImage(input) {
        if (input.files && input.files[0]) {
            const reader = new FileReader();
            reader.onload = e => {
                currentImageData = e.target.result;
                document.getElementById('img-preview').src = currentImageData;
                document.getElementById('img-preview-container').style.display = "block";
            };
            reader.readAsDataURL(input.files[0]);
        }
    }

    function addNote() {
        const title = document.getElementById('note-title').value;
        const content = document.getElementById('note-content').value;
        if (!title || !content) return alert("請填寫標題與內容");

        state.notes.unshift({ id: Date.now(), title, content, image: currentImageData, date: new Date().toLocaleString() });
        currentImageData = ""; 
        document.getElementById('note-title').value = "";
        document.getElementById('note-content').value = "";
        document.getElementById('img-preview-container').style.display = "none";
        renderNotes(); gainExp(5);
    }

    function renderNotes() {
        document.getElementById('notes-display-list').innerHTML = state.notes.map(n => `
            <div class="note-card">
                <span onclick="deleteNote(${n.id})" style="position:absolute; right:10px; top:10px; cursor:pointer;">🗑️</span>
                <h4>${n.title}</h4>
                <p>${n.content}</p>
                ${n.image ? `<img src="${n.image}">` : ''}
                <div class="note-date">${n.date}</div>
            </div>
        `).join('');
    }

    function deleteNote(id) { if(confirm("銷毀此卷宗？")) { state.notes = state.notes.filter(n=>n.id!==id); renderNotes(); saveData(); } }

    // UI 更新
    function renderUI() {
        let need = Math.floor(100 * Math.pow(1.2, state.lv-1));
        document.getElementById("lv-num").innerText = "LV." + (state.lv<10?'0'+state.lv:state.lv);
        document.getElementById("exp-fill").style.width = (state.exp/need*100) + "%";
        document.getElementById("exp-label").innerText = `${state.exp} / ${need}`;
        document.getElementById("display-goal").innerText = state.goal;
        const idx = Math.min(Math.floor(state.lv / 10), 4);
        document.getElementById("rank-tag").innerText = rankSystem[idx].name;
    }

    function updateSettings() {
        state.goal = document.getElementById('set-goal').value || "台大法律系";
        state.date = document.getElementById('set-date').value || "2027-01-16";
        state.font = document.getElementById('font-select').value;
        applyFont(state.font); saveData(); renderUI(); updateDDay();
    }

    function updateDDay() {
        const diff = new Date(state.date) - new Date();
        document.getElementById("dday-num").innerText = Math.max(0, Math.ceil(diff/86400000));
        document.getElementById("display-date").innerText = state.date;
    }

    function showRankList() {
        const cur = Math.min(Math.floor(state.lv / 10), 4);
        document.getElementById('rank-items-container').innerHTML = rankSystem.map((r, i) => `
            <div style="display:flex; padding:10px; border-bottom:1px solid #333; ${i===cur?'background:rgba(212,175,55,0.1); border-left:4px solid var(--gold);':''}">
                <div style="font-size:1.5rem; margin-right:15px;">${r.icon}</div>
                <div><div style="color:var(--gold); font-weight:bold;">${r.name}</div><div style="font-size:0.7rem; color:#888;">解鎖條件：${r.req}</div></div>
            </div>
        `).join('');
        document.getElementById('rank-list-overlay').style.display = 'flex';
    }

    function resetAll() { if(confirm("確定重置所有修煉數據？")) { localStorage.clear(); location.reload(); } }
    
    init();
</script>
</body>
</html>
