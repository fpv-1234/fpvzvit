<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Майно підрозділу</title>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Майно V10">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/5741/5741369.png">

    <link rel="manifest" href="data:application/json;base64,ewogICJuYW1lIjogIk1ham5vIFBpdHByb3pkdWx1IiwKICAic2hvcnRfbmFtZSI6ICJNYWpubyIsCiAgInN0YXJ0X3VybCI6ICIuIiwKICAiZGlzcGxheSI6ICJzdGFuZGFsb25lIiwKICAiYmFja2dyb3VuZF9jb2xvciI6ICIjMGQxMTE3IiwKICAidGhlbWVfY29sb3IiOiAiIzBkMTExNyIsCiAgImljb25zIjogWwogICAgewogICAgICAic3JjIjogImh0dHBzOi8vY2RuLWljb25zLXBuZy5mbGF0aWNvbi5jb20vNTEyLzU3NDEvNTc0MTM2OS5wbmciLAogICAgICAic2l6ZXMiOiAiNTEyeDUxMiIsCiAgICAgICJ0eXBlIjogImltYWdlL3BuZyIsCiAgICAgICJwdXJwb3NlIjogImFueSBtYXNreWJsZSIKICAgIH0KICBdCn0=">

    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif; user-select: none; }
        body { background-color: #0d1117; color: #c9d1d9; padding: 12px; padding-top: calc(12px + env(safe-area-inset-top)); display: flex; flex-direction: column; align-items: center; min-height: 100vh; }
        .container { width: 100%; max-width: 480px; background-color: #161b22; border-radius: 14px; padding: 20px; border: 1px solid #30363d; }
        
        .notification { position: fixed; top: -100px; left: 50%; transform: translateX(-50%); background-color: #238636; color: #ffffff; padding: 14px 20px; border-radius: 10px; box-shadow: 0 8px 20px rgba(0,0,0,0.4); z-index: 1000; font-weight: 600; font-size: 14px; text-align: center; width: calc(100% - 34px); max-width: 440px; transition: top 0.4s ease; border: 1px solid #2ea043; }
        .notification.show { top: 15px; }
        .notification.error { background-color: #da3633; border-color: #f85149; }

        h2, h3 { text-align: center; margin-bottom: 20px; color: #f0f6fc; font-weight: 700; }
        h2 { font-size: 20px; color: #58a6ff; }
        h3 { font-size: 16px; }
        .screen { display: none; }
        .screen.active { display: block; }
        .form-group { margin-bottom: 16px; }
        label { display: block; margin-bottom: 6px; font-size: 13px; color: #8b949e; font-weight: 500; }
        
        input { width: 100%; padding: 12px 14px; background-color: #0d1117; border: 1px solid #30363d; border-radius: 8px; color: #f0f6fc; font-size: 15px; outline: none; }
        input:focus { border-color: #58a6ff; }

        button { width: 100%; padding: 14px; background-color: #1f6feb; border: none; border-radius: 8px; color: white; font-size: 15px; font-weight: 600; cursor: pointer; margin-top: 10px; }
        button:disabled { background-color: #21262d; color: #8b949e; border: 1px solid #30363d; }

        .btn-save-online { background-color: #238636; font-size: 16px; padding: 16px; margin-top: 20px; border: 1px solid #2ea043; }
        button.secondary { background-color: #da3633; }

        .category-title { font-size: 11px; text-transform: uppercase; letter-spacing: 0.05em; color: #58a6ff; margin: 20px 0 8px 4px; font-weight: 700; border-bottom: 1px dashed #30363d; padding-bottom: 4px; }
        .item-card { background-color: #21262d; border: 1px solid #30363d; border-radius: 10px; padding: 12px 14px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; }
        .item-card.has-changes { border-color: #f0883e; background-color: rgba(240, 136, 62, 0.03); }
        .item-name { font-weight: 600; font-size: 14px; color: #c9d1d9; }
        .counter-controls { display: flex; align-items: center; gap: 10px; }

        .btn-counter { width: 44px; height: 44px; font-size: 24px; display: flex; justify-content: center; align-items: center; background-color: #30363d; border-radius: 6px; color: #c9d1d9; border: 1px solid #8b949e; -webkit-tap-highlight-color: transparent; }
        .counter-value { width: 35px; text-align: center; font-size: 18px; font-weight: 700; color: #f0f6fc; }
        
        .header-bar { display: flex; flex-direction: column; gap: 6px; margin-bottom: 16px; border-bottom: 1px solid #30363d; padding-bottom: 10px; }
        .badge-container { display: flex; gap: 6px; flex-wrap: wrap; }
        .badge { padding: 4px 10px; border-radius: 4px; font-size: 11px; font-weight: 600; }
        .badge-incoming { background-color: rgba(56, 139, 253, 0.15); color: #58a6ff; border: 1px solid rgba(56, 139, 253, 0.3); }
        .badge-status { background-color: rgba(46, 160, 67, 0.15); color: #2ea043; border: 1px solid rgba(46, 160, 67, 0.3); }
        .live-indicator { display: flex; align-items: center; gap: 6px; font-size: 12px; color: #8b949e; }
        .pulse-dot { width: 8px; height: 8px; background-color: #238636; border-radius: 50%; }
        .pulse-dot.syncing { background-color: #58a6ff; animation: pulse 1.2s infinite; }

        @keyframes pulse { 0% { transform: scale(0.95); opacity: 0.5; } 70% { transform: scale(1); opacity: 1; } 100% { transform: scale(0.95); opacity: 0.5; } }
        .monitor-box { background-color: #0d1117; border: 1px solid #30363d; border-radius: 10px; margin-top: 16px; }
        .monitor-header { background-color: #161b22; padding: 8px 12px; border-top-left-radius: 9px; border-top-right-radius: 9px; font-size: 11px; font-weight: 700; color: #8b949e; display: flex; justify-content: space-between; }
        .log-box { padding: 10px; font-size: 12px; max-height: 120px; overflow-y: auto; }
        .log-entry { margin-bottom: 6px; border-left: 3px solid #1f6feb; padding-left: 6px; }
        .log-time { color: #8b949e; margin-right: 4px; }
    </style>
</head>
<body>

<div id="toast-notification" class="notification"><span id="toast-text">Синхронізація...</span></div>

<div class="container">
    <div id="screen-login" class="screen active">
        <h2>База майна підрозділу V10</h2>
        <div class="form-group">
            <label>Назва вашого екіпажу:</label>
            <input type="text" id="crew-incoming" placeholder="Наприклад: Сапсан, Грім-2">
        </div>
        <div class="form-group">
            <label>Пін-код доступу:</label>
            <input type="password" id="pin-code" inputmode="numeric" placeholder="••••" value="1111">
        </div>
        <button id="btn-login" onclick="loginToDashboard()" disabled>Завантаження хмари...</button>
    </div>

    <div id="screen-main-dashboard" class="screen">
        <div class="header-bar">
            <div class="badge-container">
                <span class="badge badge-incoming">Екіпаж: <span id="lbl-active-crew"></span></span>
                <span class="badge badge-status">Мережа активна</span>
            </div>
            <div class="live-indicator">
                <div class="pulse-dot" id="sync-dot"></div>
                <span id="sync-status-text">Дані синхронізовано з Google.</span>
            </div>
        </div>
        
        <h3>Поточний баланс на позиції</h3>
        <div id="active-inventory-list"></div>

        <button class="btn-save-online" id="btn-save" onclick="saveChangesOnline()">
            💾 Зберегти зміни в хмару
        </button>

        <div class="monitor-box">
            <div class="monitor-header"><span>ЖУРНАЛ ДІЙ</span><span style="color:#58a6ff;">LIVE</span></div>
            <div class="log-box" id="log-container"></div>
        </div>
        <button class="secondary" style="margin-top: 20px; font-size: 13px; padding: 10px;" onclick="resetApp()">Вийти</button>
    </div>
</div>

<script>
    // ==========================================
    // ВСТАВ СВОЄ ПОСИЛАННЯ НА GOOGLE APPS SCRIPT СЮДИ:
    // ==========================================
    const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbwD04y73UvGFvZYJDW7bP-Z5XRLgfrdJmidXohLvkuSgy34q27UbI8Y1CMZ_20sw4nG3Q/exec";
    // ==========================================P

    let activeCrew = ""; let localChanges = {}; let items = [];

    window.onload = function() {
        if(localStorage.getItem('savedCrew_v10')) {
            document.getElementById('crew-incoming').value = localStorage.getItem('savedCrew_v10');
        }
        loadDataFromSheets();
        
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('data:text/javascript;base64,c2VsZi5hZGRFdmVudExpc3RlbmVyKCdmZXRjaCcsIGZ1bmN0aW9uKGUpIHt9KTs=');
        }
    };

    function loadDataFromSheets() {
        fetch(SCRIPT_URL)
        .then(res => res.json())
        .then(data => {
            items = data;
            document.getElementById('btn-login').innerText = "Відкрити панель";
            document.getElementById('btn-login').disabled = false;
            showToast("✓ Дані оновлено.");
        })
        .catch(err => {
            showToast("❌ Помилка з'єднання з хмарою.", true);
            document.getElementById('btn-login').innerText = "Мережа відсутня. Спробуйте ще";
        });
    }

    function showToast(text, isError = false) {
        const toast = document.getElementById('toast-notification');
        document.getElementById('toast-text').innerText = text;
        if(isError) toast.classList.add('error'); else toast.classList.remove('error');
        toast.classList.add('show');
        setTimeout(() => toast.classList.remove('show'), 3000);
    }

    function loginToDashboard() {
        const crewInput = document.getElementById('crew-incoming').value.trim();
        if(!crewInput) { alert("Введіть назву екіпажу!"); return; }
        activeCrew = crewInput;
        localStorage.setItem('savedCrew_v10', activeCrew);
        document.getElementById('lbl-active-crew').innerText = activeCrew;
        renderList(); loadLogs();
        document.getElementById('screen-login').classList.remove('active');
        document.getElementById('screen-main-dashboard').classList.add('active');
    }

    function renderList() {
        const container = document.getElementById('active-inventory-list');
        container.innerHTML = "";
        const categories = [...new Set(items.map(i => i.cat))];
        categories.forEach(cat => {
            const catDiv = document.createElement('div'); catDiv.className = 'category-title'; catDiv.innerText = cat; container.appendChild(catDiv);
            items.filter(i => i.cat === cat).forEach(item => {
                const card = document.createElement('div'); card.className = 'item-card'; card.id = `card-active-${item.id}`;
                if (localChanges[item.id]) card.classList.add('has-changes');
                card.innerHTML = `<div class="item-name">${item.name}</div><div class="counter-controls"><button class="btn-counter" onclick="changeVal('${item.id}', -1)">-</button><div class="counter-value" id="active-val-${item.id}">${item.val}</div><button class="btn-counter" onclick="changeVal('${item.id}', 1)">+</button></div>`;
                container.appendChild(card);
            });
        });
    }

    function changeVal(id, diff) {
        const item = items.find(i => i.id === id);
        if (item) {
            item.val = Math.max(0, item.val + diff);
            document.getElementById(`active-val-${id}`).innerText = item.val;
            localChanges[id] = true;
            document.getElementById(`card-active-${id}`).classList.add('has-changes');
            document.getElementById('sync-dot').style.backgroundColor = "#f0883e";
            document.getElementById('sync-status-text').innerText = "Є незбережені зміни.";
            document.getElementById('sync-status-text').style.color = "#f0883e";
        }
    }

    function saveChangesOnline() {
        const btn = document.getElementById('btn-save'); const syncDot = document.getElementById('sync-dot'); const syncText = document.getElementById('sync-status-text');
        btn.disabled = true; syncDot.className = "pulse-dot syncing"; syncText.innerText = "Збереження..."; syncText.style.color = "#58a6ff";

        fetch(SCRIPT_URL, { method: "POST", body: JSON.stringify(items) })
        .then(res => res.json())
        .then(resData => {
            for (let id in localChanges) { const card = document.getElementById(`card-active-${id}`); if (card) card.classList.remove('has-changes'); }
            localChanges = {};
            const time = new Date().toLocaleTimeString('uk-UA', {hour: '2-digit', minute:'2-digit'});
            addLog(time, `Екіпаж "${activeCrew}" оновив базу.`);
            syncDot.className = "pulse-dot"; syncDot.style.backgroundColor = "#238636"; syncText.innerText = "Збережено в Google."; syncText.style.color = "#2ea043";
            showToast("✓ Успішно збережено.");
        })
        .catch(err => {
            syncDot.className = "pulse-dot"; syncDot.style.backgroundColor = "#da3633"; syncText.innerText = "Помилка мережі."; syncText.style.color = "#da3633";
            showToast("❌ Помилка інтернету.", true);
        })
        .finally(() => { btn.disabled = false; });
    }

    function addLog(time, msg) {
        let logs = []; if(localStorage.getItem('appLogs_v10')) logs = JSON.parse(localStorage.getItem('appLogs_v10'));
        logs.unshift({time: time, msg: msg}); localStorage.setItem('appLogs_v10', JSON.stringify(logs.slice(0, 20))); loadLogs();
    }

    function loadLogs() {
        const logBox = document.getElementById('log-container'); logBox.innerHTML = "";
        let logs = []; if(localStorage.getItem('appLogs_v10')) logs = JSON.parse(localStorage.getItem('appLogs_v10'));
        if(logs.length === 0) { logBox.innerHTML = `<div style="color:#8b949e; text-align:center;">Журнал порожній</div>`; return; }
        logs.forEach(l => { const entry = document.createElement('div'); entry.className = 'log-entry'; entry.innerHTML = `<span class="log-time">${l.time}</span> ${l.msg}`; logBox.appendChild(entry); });
    }

    function resetApp() { if(confirm("Вийти з екіпажу?")) { localChanges = {}; document.getElementById('screen-main-dashboard').classList.remove('active'); document.getElementById('screen-login').classList.add('active'); loadDataFromSheets(); } }
</script>
</body>
</html>[index.html](https://github.com/user-attachments/files/29392983/index.html)
