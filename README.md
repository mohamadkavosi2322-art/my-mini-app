<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatGPT</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:system-ui,sans-serif;background:#fff;color:#000;min-height:100vh}
        .header{display:flex;align-items:center;justify-content:space-between;padding:16px 20px;background:#f7f7f8;border-bottom:1px solid #e0e0e0;position:sticky;top:0;z-index:100}
        .header-left{display:flex;align-items:center;gap:12px}
        .menu-btn{font-size:24px;background:none;border:none;cursor:pointer;padding:4px 8px;border-radius:8px}
        .menu-btn:hover{background:#e0e0e0}
        .logo{font-weight:700;font-size:18px}
        .header-right{display:flex;align-items:center;gap:8px}
        .header-btn{background:none;border:none;font-size:20px;cursor:pointer;padding:4px 8px;border-radius:8px}
        .header-btn:hover{background:#e0e0e0}
        .sidebar-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.4);z-index:200;display:none}
        .sidebar{position:fixed;top:0;left:0;bottom:0;width:320px;max-width:85%;background:#f7f7f8;z-index:300;transform:translateX(-100%);transition:transform .3s ease;padding:20px;overflow-y:auto}
        .sidebar.open{transform:translateX(0)}
        .sidebar-overlay.open{display:block}
        .sidebar-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:20px;padding-bottom:16px;border-bottom:1px solid #e0e0e0}
        .sidebar-header h2{font-size:20px}
        .close-sidebar{background:none;border:none;font-size:24px;cursor:pointer}
        .sidebar-item{display:flex;align-items:center;gap:14px;padding:12px 16px;border-radius:12px;cursor:pointer;transition:background .2s;width:100%;background:none;border:none;color:#000;font-size:15px;text-align:left;font-family:inherit}
        .sidebar-item:hover{background:#e0e0e0}
        .sidebar-item .icon{font-size:20px;width:28px;text-align:center}
        .sidebar-divider{height:1px;background:#e0e0e0;margin:8px 0}
        .main-content{padding:20px;min-height:calc(100vh - 70px)}
        .welcome{text-align:center;padding:40px 20px}
        .welcome h1{font-size:28px;margin-bottom:8px}
        .welcome p{color:#888;font-size:16px}
        .page{display:none}.page.active{display:block}
        .chat-container{display:flex;flex-direction:column;height:calc(100vh - 140px)}
        .chat-box{flex:1;overflow-y:auto;padding:16px 0;display:flex;flex-direction:column;gap:8px}
        .message{padding:12px 16px;border-radius:16px;max-width:85%;word-wrap:break-word;font-size:15px;line-height:1.5}
        .message.user{background:#0088cc;color:#fff;align-self:flex-end;border-bottom-right-radius:4px}
        .message.bot{background:#f0f0f0;color:#000;align-self:flex-start;border-bottom-left-radius:4px}
        .message.bot.thinking{color:#888}
        .chat-input-area{display:flex;gap:10px;padding:12px 0;border-top:1px solid #e0e0e0;background:#fff}
        .chat-input-area input{flex:1;padding:12px 16px;border-radius:24px;border:1px solid #e0e0e0;background:#f7f7f8;color:#000;font-size:15px;outline:none;font-family:inherit}
        .chat-input-area input:focus{border-color:#0088cc}
        .chat-input-area button{padding:12px 20px;border-radius:24px;border:none;background:#0088cc;color:#fff;font-size:15px;cursor:pointer;font-family:inherit}
        .chat-input-area button:disabled{opacity:.6}
        .settings-group{margin-bottom:24px}
        .settings-group-title{font-size:13px;font-weight:600;color:#888;text-transform:uppercase;letter-spacing:.5px;margin-bottom:8px;padding:0 4px}
        .settings-item{display:flex;justify-content:space-between;align-items:center;padding:12px 16px;background:#f7f7f8;border-radius:12px;margin-bottom:4px;cursor:pointer;transition:background .2s}
        .settings-item:hover{background:#e0e0e0}
        .settings-item-left{display:flex;align-items:center;gap:12px}
        .settings-item-left .icon{font-size:18px}
        .settings-item-left .label{font-size:15px}
        .settings-item .arrow{color:#888;font-size:14px}
        .settings-item .toggle{width:44px;height:24px;background:#ccc;border-radius:12px;position:relative;cursor:pointer;transition:background .3s}
        .settings-item .toggle.on{background:#0088cc}
        .settings-item .toggle .slider{width:20px;height:20px;background:#fff;border-radius:50%;position:absolute;top:2px;left:2px;transition:transform .3s;box-shadow:0 1px 3px rgba(0,0,0,.2)}
        .settings-item .toggle.on .slider{transform:translateX(20px)}
        .history-item{display:flex;align-items:center;justify-content:space-between;padding:12px 16px;background:#f7f7f8;border-radius:12px;margin-bottom:6px;cursor:pointer}
        .history-item:hover{background:#e0e0e0}
        .history-item .title{font-size:14px}
        .history-item .date{font-size:12px;color:#888}
        .model-item{display:flex;align-items:center;gap:12px;padding:12px 16px;background:#f7f7f8;border-radius:12px;margin-bottom:4px;cursor:pointer}
        .model-item:hover{background:#e0e0e0}
        .model-item .icon{font-size:18px}
        .model-item .name{font-size:15px;flex:1}
        .model-item .check{color:#0088cc;font-weight:700}
        .btn-13{background:#0088cc;color:#fff;border:none;padding:12px;border-radius:10px;width:100%;font-size:16px;margin-top:10px;cursor:pointer;font-family:inherit}
        .btn-13:hover{opacity:.85}
        ::-webkit-scrollbar{width:6px}
        ::-webkit-scrollbar-thumb{background:#ccc;border-radius:4px}
        @media(max-width:480px){.sidebar{width:280px}.header{padding:12px 16px}.main-content{padding:12px 16px}.welcome h1{font-size:22px}.settings-item{padding:10px 14px}}
    </style>
</head>
<body>
<div class="header">
    <div class="header-left">
        <button class="menu-btn" onclick="toggleSidebar()">☰</button>
        <span class="logo">🧠 ChatGPT</span>
    </div>
    <div class="header-right">
        <button class="header-btn" onclick="showPage('settings')">⚙️</button>
        <button class="header-btn" onclick="showPage('history')">📋</button>
    </div>
</div>
<div class="sidebar-overlay" id="sidebarOverlay" onclick="toggleSidebar()"></div>
<div class="sidebar" id="sidebar">
    <div class="sidebar-header"><h2>☰ منو</h2><button class="close-sidebar" onclick="toggleSidebar()">✕</button></div>
    <button class="sidebar-item" onclick="newChat()"><span class="icon">🆕</span> New Chat</button>
    <button class="sidebar-item" onclick="showPage('history');toggleSidebar();"><span class="icon">💬</span> تاریخچه چت‌ها</button>
    <button class="sidebar-item" onclick="showPage('search');toggleSidebar();"><span class="icon">🔍</span> Search</button>
    <button class="sidebar-item" onclick="showPage('projects');toggleSidebar();"><span class="icon">📁</span> Projects</button>
    <button class="sidebar-item" onclick="showPage('gpts');toggleSidebar();"><span class="icon">🧩</span> GPTs / Explore</button>
    <div class="sidebar-divider"></div>
    <button class="sidebar-item" onclick="showPage('files');toggleSidebar();"><span class="icon">📎</span> فایل و عکس</button>
    <button class="sidebar-item" onclick="showPage('voice');toggleSidebar();"><span class="icon">🎙️</span> Voice</button>
    <button class="sidebar-item" onclick="showPage('websearch');toggleSidebar();"><span class="icon">🌐</span> جست‌وجوی وب</button>
    <button class="sidebar-item" onclick="showPage('models');toggleSidebar();"><span class="icon">🤖</span> انتخاب مدل</button>
    <div class="sidebar-divider"></div>
    <button class="sidebar-item" onclick="showPage('settings');toggleSidebar();"><span class="icon">⚙️</span> تنظیمات</button>
    <button class="sidebar-item" onclick="showPage('support');toggleSidebar();"><span class="icon">🆘</span> پشتیبانی</button>
</div>
<div class="main-content">
    <div class="page active" id="page-home">
        <div class="welcome">
            <h1>🧠 ChatGPT</h1>
            <p>هر کاری که داری انجام بده.</p>
            <br>
            <button onclick="newChat()" class="btn-13">💬 شروع چت جدید</button>
        </div>
    </div>
    <div class="page" id="page-chat">
        <div class="chat-container">
            <div class="chat-box" id="chatBox"><div class="message bot">🤖 سلام! سوال خود را بپرسید.</div></div>
            <div class="chat-input-area">
                <input type="text" id="chatInput" placeholder="پیام خود را بنویسید..." onkeydown="if(event.key=='Enter') sendChat()">
                <button id="sendBtn" onclick="sendChat()">📤</button>
            </div>
        </div>
    </div>
    <div class="page" id="page-history">
        <h3 style="margin-bottom:16px;">📋 تاریخچه چت‌ها</h3>
        <div id="historyList">
            <div class="history-item" onclick="newChat()"><span class="title">🧠 چت جدید</span><span class="date">همین الان</span></div>
            <div style="color:#888;padding:12px;text-align:center;font-size:14px;">تاریخچه بیشتری وجود ندارد.</div>
        </div>
    </div>
    <div class="page" id="page-search">
        <h3 style="margin-bottom:16px;">🔍 جستجو</h3>
        <div style="display:flex;gap:10px;">
            <input type="text" id="searchInput" placeholder="جستجو در چت‌ها..." style="flex:1;padding:12px 16px;border-radius:24px;border:1px solid #e0e0e0;background:#f7f7f8;color:#000;font-size:15px;outline:none;font-family:inherit;">
            <button onclick="searchChats()" class="btn-13" style="width:auto;padding:12px 20px;">🔍</button>
        </div>
        <div id="searchResults" style="margin-top:16px;"><div style="color:#888;padding:12px;text-align:center;font-size:14px;">چیزی برای جستجو وارد کنید.</div></div>
    </div>
    <div class="page" id="page-projects"><h3 style="margin-bottom:16px;">📁 Projects</h3><div style="padding:16px;background:#f7f7f8;border-radius:12px;text-align:center;color:#888;">هنوز پروژه‌ای وجود ندارد.</div></div>
    <div class="page" id="page-gpts"><h3 style="margin-bottom:16px;">🧩 GPTs / Explore</h3><div style="padding:16px;background:#f7f7f8;border-radius:12px;text-align:center;color:#888;">لیست GPTهای تخصصی</div></div>
    <div class="page" id="page-files"><h3 style="margin-bottom:16px;">📎 فایل و عکس</h3><div style="padding:16px;background:#f7f7f8;border-radius:12px;text-align:center;color:#888;">فایلی انتخاب نشده است.</div></div>
    <div class="page" id="page-voice"><h3 style="margin-bottom:16px;">🎙️ Voice</h3><div style="padding:16px;background:#f7f7f8;border-radius:12px;text-align:center;color:#888;">برای استفاده از گفت‌وگوی صوتی، روی دکمه زیر کلیک کنید.</div></div>
    <div class="page" id="page-websearch">
        <h3 style="margin-bottom:16px;">🌐 جست‌وجوی وب</h3>
        <div style="display:flex;gap:10px;">
            <input type="text" id="webSearchInput" placeholder="عبارت جستجو..." style="flex:1;padding:12px 16px;border-radius:24px;border:1px solid #e0e0e0;background:#f7f7f8;color:#000;font-size:15px;outline:none;font-family:inherit;">
            <button onclick="webSearch()" class="btn-13" style="width:auto;padding:12px 20px;">🌐</button>
        </div>
        <div id="webSearchResults" style="margin-top:16px;"><div style="color:#888;padding:12px;text-align:center;font-size:14px;">منتظر عبارت جستجو هستم...</div></div>
    </div>
    <div class="page" id="page-models">
        <h3 style="margin-bottom:16px;">🤖 انتخاب مدل</h3>
        <div class="model-item" onclick="selectModel('GPT-3')"><span class="icon">🧠</span><span class="name">GPT-3</span><span class="check" id="model_gpt3">✓</span></div>
        <div class="model-item" onclick="selectModel('GPT-3.5')"><span class="icon">🧠</span><span class="name">GPT-3.5</span><span class="check" id="model_gpt35"></span></div>
        <div class="model-item" onclick="selectModel('GPT-4')"><span class="icon">🧠</span><span class="name">GPT-4</span><span class="check" id="model_gpt4"></span></div>
        <div class="model-item" onclick="selectModel('GPT-4o')"><span class="icon">🧠</span><span class="name">GPT-4o</span><span class="check" id="model_gpt4o"></span></div>
        <div class="model-item" onclick="selectModel('GPT-5')"><span class="icon">🧠</span><span class="name">GPT-5</span><span class="check" id="model_gpt5"></span></div>
        <div class="model-item" onclick="selectModel('o1')"><span class="icon">🧠</span><span class="name">o1</span><span class="check" id="model_o1"></span></div>
        <div style="margin-top:16px;padding:12px;background:#f7f7f8;border-radius:12px;text-align:center;color:#888;font-size:14px;" id="selectedModelDisplay">مدل فعلی: GPT-4o-mini</div>
    </div>
    <div class="page" id="page-settings">
        <h3 style="margin-bottom:16px;">⚙️ تنظیمات</h3>
        <div class="settings-group"><div class="settings-group-title">👤 Account</div><div class="settings-item" onclick="alert('حساب کاربری')"><div class="settings-item-left"><span class="icon">👤</span><span class="label">حساب کاربری</span></div><span class="arrow">›</span></div></div>
        <div class="settings-group"><div class="settings-group-title">🎨 Appearance</div><div class="settings-item" onclick="toggleTheme()"><div class="settings-item-left"><span class="icon">🌓</span><span class="label">حالت تاریک/روشن</span></div><div class="toggle" id="themeToggle"><div class="slider"></div></div></div></div>
        <div class="settings-group"><div class="settings-group-title">🗣️ Personality</div><div class="settings-item" onclick="alert('تغییر شخصیت')"><div class="settings-item-left"><span class="icon">🗣️</span><span class="label">شخصیت</span></div><span class="arrow">›</span></div></div>
        <div class="settings-group"><div class="settings-group-title">🧠 Memory</div><div class="settings-item" onclick="toggleMemory()"><div class="settings-item-left"><span class="icon">🧠</span><span class="label">حافظه</span></div><div class="toggle on" id="memoryToggle"><div class="slider"></div></div></div></div>
        <div class="settings-group"><div class="settings-group-title">🔒 Data Controls</div><div class="settings-item" onclick="alert('کنترل داده‌ها')"><div class="settings-item-left"><span class="icon">🔒</span><span class="label">کنترل داده‌ها</span></div><span class="arrow">›</span></div></div>
        <div class="settings-group"><div class="settings-group-title">🔔 Notifications</div><div class="settings-item" onclick="toggleNotifications()"><div class="settings-item-left"><span class="icon">🔔</span><span class="label">اعلان‌ها</span></div><div class="toggle on" id="notifToggle"><div class="slider"></div></div></div></div>
        <div class="settings-group"><div class="settings-group-title">🌐 Language</div><div class="settings-item" onclick="alert('تغییر زبان')"><div class="settings-item-left"><span class="icon">🌐</span><span class="label">زبان</span></div><span class="arrow">›</span></div></div>
        <div class="settings-group"><div class="settings-group-title">💳 Subscription</div><div class="settings-item" onclick="alert('مدیریت اشتراک')"><div class="settings-item-left"><span class="icon">💳</span><span class="label">اشتراک</span></div><span class="arrow">›</span></div></div>
        <div class="settings-group"><div class="settings-group-title">🛡️ Privacy</div><div class="settings-item" onclick="alert('حریم خصوصی')"><div class="settings-item-left"><span class="icon">🛡️</span><span class="label">حریم خصوصی</span></div><span class="arrow">›</span></div></div>
    </div>
    <div class="page" id="page-support">
        <h3 style="margin-bottom:16px;">🆘 پشتیبانی</h3>
        <div style="padding:16px;background:#f7f7f8;border-radius:12px;text-align:center;">
            <p style="margin-bottom:12px;">برای ارتباط با پشتیبانی روی لینک زیر کلیک کنید:</p>
            <a href="https://ble.ir/Pixeruser" target="_blank" style="display:inline-block;background:#0088cc;color:#fff;padding:12px 24px;border-radius:10px;text-decoration:none;font-family:inherit;">👤 ارتباط با پشتیبانی</a>
        </div>
    </div>
</div>
<script>
    const AVALAI_KEY = "aa-iuxSVnhoKQnYRRHavV6WnuohoCz1pzBUmJ2bJWRICxCXgejW";
    const AVALAI_URL = "https://api.avalai.ir/v1/chat/completions";

    let waiting = false;
    let currentModel = 'GPT-4o-mini';
    let chatHistory = [];

    function toggleSidebar(){
        document.getElementById('sidebar').classList.toggle('open');
        document.getElementById('sidebarOverlay').classList.toggle('open');
    }

    function showPage(p){
        document.querySelectorAll('.page').forEach(e=>e.classList.remove('active'));
        let t=document.getElementById('page-'+p);
        if(t) t.classList.add('active');
        document.getElementById('sidebar').classList.remove('open');
        document.getElementById('sidebarOverlay').classList.remove('open');
    }

    function newChat(){
        document.getElementById('chatBox').innerHTML='<div class="message bot">🤖 سلام! سوال خود را بپرسید.</div>';
        document.getElementById('chatInput').value='';
        document.getElementById('chatInput').focus();
        chatHistory = [];
        showPage('chat');
    }

    function selectModel(model){
        currentModel = model;
        document.querySelectorAll('.model-item .check').forEach(e=>e.textContent='');
        let id = 'model_'+model.toLowerCase().replace('.','').replace('-','');
        let el = document.getElementById(id);
        if(el) el.textContent='✓';
        document.getElementById('selectedModelDisplay').textContent='مدل فعلی: '+model;
    }

    async function askAvalai(prompt){
        try {
            const response = await fetch(AVALAI_URL, {
                method: 'POST',
                headers: {
                    'Authorization': `Bearer ${AVALAI_KEY}`,
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    model: 'gpt-4o-mini',
                    messages: [{"role": "user", "content": prompt}],
                    temperature: 0.7,
                    max_tokens: 800
                })
            });
            if(response.ok){
                const data = await response.json();
                return data.choices[0].message.content;
            }
            return null;
        } catch(e){
            console.error('خطا:', e);
            return null;
        }
    }

    async function sendChat(){
        let input = document.getElementById('chatInput');
        let msg = input.value.trim();
        if(!msg || waiting) return;

        let box = document.getElementById('chatBox');

        let userMsg = document.createElement('div');
        userMsg.className = 'message user';
        userMsg.textContent = msg;
        box.appendChild(userMsg);
        box.scrollTop = box.scrollHeight;

        input.value = '';
        waiting = true;
        document.getElementById('sendBtn').disabled = true;

        let botMsg = document.createElement('div');
        botMsg.className = 'message bot thinking';
        botMsg.textContent = '🤖 ⏳ در حال فکر کردن...';
        box.appendChild(botMsg);
        box.scrollTop = box.scrollHeight;

        chatHistory.push({role: 'user', content: msg});

        let response = await askAvalai(msg);

        if(response){
            botMsg.className = 'message bot';
            botMsg.textContent = '🤖 ' + response;
            chatHistory.push({role: 'assistant', content: response});
        } else {
            botMsg.className = 'message bot';
            botMsg.textContent = '🤖 متاسفم! خطایی رخ داد. لطفاً دوباره تلاش کنید.';
        }
        box.scrollTop = box.scrollHeight;
        waiting = false;
        document.getElementById('sendBtn').disabled = false;
    }

    function searchChats(){
        let q=document.getElementById('searchInput').value.trim();
        let r=document.getElementById('searchResults');
        if(!q){r.innerHTML='<div style="color:#888;padding:12px;text-align:
