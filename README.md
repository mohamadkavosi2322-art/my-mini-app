<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ChatGPT</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:system-ui,-apple-system,sans-serif;min-height:100vh;padding:20px;direction:rtl;transition:all .3s}
        .app{max-width:900px;margin:0 auto}
        .header{display:flex;justify-content:space-between;align-items:center;padding:16px 20px;border-radius:16px;border:1px solid;margin-bottom:16px;transition:all .3s}
        .header h1{font-size:20px;font-weight:700}
        .header h1 span{color:#3b82f6}
        .header .badge{font-size:11px;padding:4px 12px;border-radius:20px;background:rgba(255,255,255,0.1)}
        .main{background:rgba(255,255,255,0.05);border-radius:16px;border:1px solid;padding:20px;margin-bottom:16px;transition:all .3s}
        .top-bar{display:flex;gap:8px;margin-bottom:12px;flex-wrap:wrap}
        .top-bar button{background:transparent;border:1px solid;padding:5px 12px;border-radius:8px;font-size:11px;cursor:pointer;transition:all .2s;font-family:inherit}
        .top-bar button.active{border-color:#3b82f6}
        .models{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:12px;padding-bottom:12px;border-bottom:1px solid}
        .models button{background:transparent;border:1px solid;padding:5px 12px;border-radius:8px;font-size:11px;cursor:pointer;transition:all .2s;font-family:inherit}
        .models button.active{background:#3b82f6;color:#fff;border-color:#3b82f6}
        .history-area{display:flex;gap:16px;margin-bottom:12px;padding-bottom:12px;border-bottom:1px solid}
        .history-list{width:200px;flex-shrink:0;border-radius:12px;padding:8px;border:1px solid;max-height:200px;overflow-y:auto}
        .history-list .h-item{padding:6px 10px;border-radius:6px;cursor:pointer;font-size:12px;transition:all .2s;border-bottom:1px solid rgba(255,255,255,0.05)}
        .history-list .h-item:hover{background:rgba(255,255,255,0.05)}
        .history-list .h-item.active{background:rgba(59,130,246,0.2)}
        .history-list .h-item .h-time{font-size:9px;opacity:0.5;display:block;margin-top:2px}
        .chat-box{flex:1;border-radius:12px;padding:16px;border:1px solid;min-height:250px;max-height:350px;overflow-y:auto}
        .chat-box::-webkit-scrollbar{width:4px}
        .chat-box::-webkit-scrollbar-thumb{background:#3b82f6;border-radius:4px}
        .msg{padding:10px 16px;border-radius:12px;margin-bottom:6px;max-width:85%;word-wrap:break-word;font-size:14px;line-height:1.8}
        .msg.user{background:#3b82f6;color:#fff;margin-right:auto;text-align:right}
        .msg.bot{background:rgba(255,255,255,0.08);margin-left:auto;text-align:left;border:1px solid rgba(255,255,255,0.1)}
        .msg.bot.typing{opacity:0.6}
        .msg.bot .cursor{display:inline-block;width:2px;height:18px;background:#3b82f6;animation:blink 1s step-end infinite;vertical-align:text-bottom;margin-right:2px}
        @keyframes blink{50%{opacity:0}}
        .input-area{display:flex;gap:10px;margin-top:12px}
        .input-area input{flex:1;padding:12px 16px;border-radius:12px;border:1px solid;font-size:14px;outline:none;font-family:inherit;background:transparent;transition:all .3s}
        .input-area input:focus{border-color:#3b82f6}
        .input-area .send-btn{padding:12px 20px;border-radius:12px;border:none;font-size:18px;cursor:pointer;font-weight:500;transition:all .2s;font-family:inherit;display:flex;align-items:center;justify-content:center;min-width:50px}
        .input-area .send-btn:hover{transform:scale(1.05)}
        .input-area .send-btn:disabled{opacity:0.5;cursor:not-allowed;transform:none}
        .input-area .stop-btn{padding:12px 20px;border-radius:12px;border:none;font-size:16px;cursor:pointer;font-weight:500;transition:all .2s;font-family:inherit;display:none;align-items:center;justify-content:center;min-width:50px;background:#ef4444;color:#fff}
        .input-area .stop-btn:hover{transform:scale(1.05)}
        .actions{display:flex;gap:6px;margin-top:12px;justify-content:center;padding-top:12px;border-top:1px solid;flex-wrap:wrap}
        .actions button{background:transparent;border:1px solid;padding:5px 14px;border-radius:8px;font-size:11px;cursor:pointer;transition:all .2s;font-family:inherit}
        .actions button:hover{background:rgba(255,255,255,0.05)}
        .footer{text-align:center;font-size:12px;padding-top:16px;border-top:1px solid}
        .footer span{color:#3b82f6}
        .status{font-size:12px;text-align:center;margin-top:10px;opacity:0.6}
        .color-picker{display:flex;gap:6px;flex-wrap:wrap;margin-top:8px}
        .color-picker .cbtn{width:28px;height:28px;border-radius:50%;border:2px solid transparent;cursor:pointer;transition:all .2s}
        .color-picker .cbtn:hover{transform:scale(1.15)}
        .color-picker .cbtn.active{border-color:#fff;box-shadow:0 0 10px rgba(255,255,255,0.3)}
        .empty-history{text-align:center;padding:20px 0;font-size:13px;opacity:0.5}
        .theme-toggle{display:flex;gap:6px;margin-top:8px}
        .theme-toggle button{padding:4px 14px;border-radius:8px;border:1px solid;cursor:pointer;font-size:12px;font-family:inherit;background:transparent;transition:all .2s}
        .theme-toggle button:hover{background:rgba(255,255,255,0.05)}
        .theme-toggle button.active{background:rgba(59,130,246,0.2)}
        @media(max-width:700px){.history-area{flex-direction:column}.history-list{width:100%;max-height:120px}}
    </style>
</head>
<body>
<div class="app" id="app">
    <!-- هدر -->
    <div class="header" id="header">
        <h1><span>Chat</span>GPT</h1>
        <span class="badge">نسخه 3.0</span>
    </div>

    <!-- بخش اصلی -->
    <div class="main" id="main">
        <!-- انتخاب رنگ -->
        <div class="top-bar">
            <span style="font-size:12px;opacity:0.6;margin-left:8px;">رنگ: </span>
            <div class="color-picker" id="colorPicker">
                <button class="cbtn active" style="background:#3b82f6" data-color="#3b82f6" title="آبی"></button>
                <button class="cbtn" style="background:#ef4444" data-color="#ef4444" title="قرمز"></button>
                <button class="cbtn" style="background:#ffffff" data-color="#ffffff" title="سفید"></button>
                <button class="cbtn" style="background:#1a1a1a" data-color="#1a1a1a" title="مشکی"></button>
                <button class="cbtn" style="background:#22c55e" data-color="#22c55e" title="سبز"></button>
                <button class="cbtn" style="background:#ec4899" data-color="#ec4899" title="صورتی"></button>
                <button class="cbtn" style="background:#8b5cf6" data-color="#8b5cf6" title="بنفش"></button>
                <button class="cbtn" style="background:#eab308" data-color="#eab308" title="زرد"></button>
                <button class="cbtn" style="background:#f97316" data-color="#f97316" title="نارنجی"></button>
            </div>
            <span style="font-size:12px;opacity:0.6;margin-right:12px;margin-left:8px;">تم: </span>
            <div class="theme-toggle" id="themeToggle">
                <button class="active" data-theme="dark">تاریک</button>
                <button data-theme="light">روشن</button>
            </div>
        </div>

        <!-- انتخاب مدل -->
        <div class="models" id="modelContainer">
            <button class="active" data-model="gpt-4o-mini">GPT-4o Mini</button>
            <button data-model="gpt-4o">GPT-4o</button>
            <button data-model="gpt-4">GPT-4</button>
            <button data-model="gpt-3.5-turbo">GPT-3.5</button>
            <button data-model="o1-mini">O1 Mini</button>
        </div>

        <!-- تاریخچه + چت -->
        <div class="history-area">
            <div class="history-list" id="historyList">
                <div class="empty-history">تاریخچه خالی است</div>
            </div>
            <div class="chat-box" id="chatBox">
                <div class="msg bot">سلام. سوال خود را بپرسید.</div>
            </div>
        </div>

        <!-- ورودی -->
        <div class="input-area">
            <input type="text" id="chatInput" placeholder="پیام خود را بنویسید..." onkeydown="if(event.key=='Enter') sendMessage()">
            <button class="send-btn" id="sendBtn" onclick="sendMessage()">↑</button>
            <button class="stop-btn" id="stopBtn" onclick="stopGeneration()">■</button>
        </div>

        <!-- دکمه‌های عملیات -->
        <div class="actions">
            <button onclick="copyLastResponse()">کپی پاسخ</button>
            <button onclick="clearChat()">پاک کردن چت</button>
            <button onclick="exportChat()">خروجی چت</button>
            <button onclick="newChat()">گفتگوی جدید</button>
        </div>

        <div class="status" id="statusText">وضعیت: آماده</div>
    </div>

    <!-- فوتر -->
    <div class="footer" id="footer">ساخته شده توسط <span>Bale-OpenAI</span></div>
</div>

<script>
    const AVALAI_KEY = "aa-iuxSVnhoKQnYRRHavV6WnuohoCz1pzBUmJ2bJWRICxCXgejW";
    const AVALAI_URL = "https://api.avalai.ir/v1/chat/completions";

    let currentModel = "gpt-4o-mini";
    let isWaiting = false;
    let isStreaming = false;
    let lastBotResponse = '';
    let chatHistory = [];
    let currentChatId = null;
    let chatSessions = JSON.parse(localStorage.getItem('chatSessions')) || [];
    let currentColor = "#3b82f6";
    let currentTheme = "dark";

    // ===== تایپ کلمه به کلمه =====
    async function typeMessage(element, text, speed = 15) {
        element.textContent = '';
        const chars = text.split('');
        for (let i = 0; i < chars.length; i++) {
            if (!isStreaming) break;
            element.textContent += chars[i];
            element.parentElement.scrollTop = element.parentElement.scrollHeight;
            await new Promise(r => setTimeout(r, speed));
        }
        return element.textContent;
    }

    // ===== ذخیره و تاریخچه =====
    function saveSessions() {
        localStorage.setItem('chatSessions', JSON.stringify(chatSessions));
    }

    function renderHistory() {
        const list = document.getElementById('historyList');
        if (chatSessions.length === 0) {
            list.innerHTML = '<div class="empty-history">تاریخچه خالی است</div>';
            return;
        }
        list.innerHTML = '';
        chatSessions.forEach((session, index) => {
            const div = document.createElement('div');
            div.className = 'h-item' + (session.id === currentChatId ? ' active' : '');
            div.innerHTML = `<span>${session.title}</span><span class="h-time">${session.time}</span>`;
            div.onclick = () => loadSession(index);
            list.appendChild(div);
        });
    }

    function saveCurrentChat() {
        if (chatHistory.length === 0) return;
        const firstMsg = chatHistory.find(m => m.role === 'user');
        const title = firstMsg ? firstMsg.content.slice(0, 30) + (firstMsg.content.length > 30 ? '...' : '') : 'گفتگو';
        const existing = chatSessions.findIndex(s => s.id === currentChatId);
        const session = {
            id: currentChatId || 'chat_' + Date.now(),
            title: title,
            time: new Date().toLocaleString('fa-IR'),
            messages: chatHistory
        };
        if (existing !== -1) {
            chatSessions[existing] = session;
        } else {
            chatSessions.push(session);
        }
        if (chatSessions.length > 50) chatSessions.shift();
        saveSessions();
        renderHistory();
    }

    function loadSession(index) {
        const session = chatSessions[index];
        if (!session) return;
        currentChatId = session.id;
        chatHistory = JSON.parse(JSON.stringify(session.messages));
        const box = document.getElementById('chatBox');
        box.innerHTML = '';
        chatHistory.forEach(msg => {
            const div = document.createElement('div');
            div.className = 'msg ' + (msg.role === 'user' ? 'user' : 'bot');
            div.textContent = msg.content;
            box.appendChild(div);
        });
        if (chatHistory.length === 0) {
            box.innerHTML = '<div class="msg bot">سلام. سوال خود را بپرسید.</div>';
        }
        box.scrollTop = box.scrollHeight;
        renderHistory();
        updateStatus('بارگذاری شد');
        setTimeout(() => updateStatus('آماده'), 1000);
    }

    function newChat() {
        currentChatId = 'chat_' + Date.now();
        chatHistory = [];
        document.getElementById('chatBox').innerHTML = '<div class="msg bot">سلام. سوال خود را بپرسید.</div>';
        lastBotResponse = '';
        updateStatus('گفتگوی جدید');
        setTimeout(() => updateStatus('آماده'), 1000);
        renderHistory();
    }

    // ===== رنگ و تم =====
    function applyColor(color) {
        currentColor = color;
        document.documentElement.style.setProperty('--accent', color);
        document.querySelectorAll('.msg.user').forEach(el => {
            el.style.background = color;
        });
        document.querySelectorAll('.models button.active').forEach(el => {
            el.style.background = color;
            el.style.borderColor = color;
        });
        document.querySelectorAll('.send-btn').forEach(el => {
            el.style.background = color;
        });
        document.querySelectorAll('.header h1 span').forEach(el => {
            el.style.color = color;
        });
        document.querySelectorAll('.footer span').forEach(el => {
            el.style.color = color;
        });
        document.querySelectorAll('.chat-box::-webkit-scrollbar-thumb').forEach(el => {
            el.style.background = color;
        });
        document.querySelectorAll('.msg.bot .cursor').forEach(el => {
            el.style.background = color;
        });
        document.querySelectorAll('.top-bar button.active').forEach(el => {
            el.style.borderColor = color;
        });
        document.querySelectorAll('.input-area input:focus').forEach(el => {
            el.style.borderColor = color;
        });
        document.querySelectorAll('.color-picker .cbtn').forEach(btn => {
            btn.classList.toggle('active', btn.dataset.color === color);
        });
        localStorage.setItem('chatColor', color);
    }

    function applyTheme(theme) {
        currentTheme = theme;
        const body = document.body;
        const header = document.getElementById('header');
        const main = document.getElementById('main');
        const footer = document.getElementById('footer');
        const isDark = theme === 'dark';
        
        body.style.background = isDark ? '#0a0a0a' : '#f5f7fa';
        body.style.color = isDark ? '#e5e7eb' : '#1a1a1a';
        
        const borderColor = isDark ? '#2a2a2a' : '#d1d5db';
        const bgColor = isDark ? '#141414' : '#ffffff';
        const mainBg = isDark ? 'rgba(255,255,255,0.05)' : 'rgba(0,0,0,0.03)';
        
        header.style.background = bgColor;
        header.style.borderColor = borderColor;
        header.style.color = body.style.color;
        
        main.style.background = mainBg;
        main.style.borderColor = borderColor;
        
        footer.style.borderColor = borderColor;
        footer.style.color = body.style.color;
        
        document.querySelectorAll('.models').forEach(el => el.style.borderColor = borderColor);
        document.querySelectorAll('.history-area').forEach(el => el.style.borderColor = borderColor);
        document.querySelectorAll('.history-list').forEach(el => {
            el.style.background = isDark ? '#1a1a1a' : '#f9fafb';
            el.style.borderColor = borderColor;
        });
        document.querySelectorAll('.chat-box').forEach(el => {
            el.style.background = isDark ? '#1a1a1a' : '#f9fafb';
            el.style.borderColor = borderColor;
        });
        document.querySelectorAll('.input-area input').forEach(el => {
            el.style.background = isDark ? '#1a1a1a' : '#f9fafb';
            el.style.borderColor = borderColor;
            el.style.color = body.style.color;
        });
        document.querySelectorAll('.actions').forEach(el => el.style.borderColor = borderColor);
        document.querySelectorAll('.top-bar button').forEach(el => {
            el.style.borderColor = borderColor;
            el.style.color = body.style.color;
        });
        document.querySelectorAll('.models button').forEach(el => {
            el.style.borderColor = borderColor;
            el.style.color = body.style.color;
        });
        document.querySelectorAll('.color-picker .cbtn').forEach(el => {
            el.style.borderColor = borderColor;
        });
        document.querySelectorAll('.theme-toggle button').forEach(el => {
            el.style.borderColor = borderColor;
            el.style.color = body.style.color;
        });
        document.querySelectorAll('.theme-toggle button.active').forEach(el => {
            el.style.background = isDark ? 'rgba(59,130,246,0.2)' : 'rgba(59,130,246,0.1)';
        });
        
        document.querySelectorAll('.msg.bot').forEach(el => {
            el.style.borderColor = borderColor;
            el.style.color = body.style.color;
        });
        
        localStorage.setItem('chatTheme', theme);
    }

    // ===== انتخاب رنگ =====
    document.querySelectorAll('.color-picker .cbtn').forEach(btn => {
        btn.addEventListener('click', function() {
            applyColor(this.dataset.color);
        });
    });

    // ===== انتخاب تم =====
    document.querySelectorAll('.theme-toggle button').forEach(btn => {
        btn.addEventListener('click', function() {
            document.querySelectorAll('.theme-toggle button').forEach(b => b.classList.remove('active'));
            this.classList.add('active');
            applyTheme(this.dataset.theme);
        });
    });

    // ===== انتخاب مدل =====
    document.querySelectorAll('#modelContainer button').forEach(btn => {
        btn.addEventListener('click', function() {
            document.querySelectorAll('#modelContainer button').forEach(b => b.classList.remove('active'));
            this.classList.add('active');
            currentModel = this.dataset.model;
            updateStatus('مدل: ' + this.textContent);
            // اعمال رنگ روی دکمه فعال
            this.style.background = currentColor;
            this.style.borderColor = currentColor;
        });
    });

    function updateStatus(text) {
        document.getElementById('statusText').textContent = 'وضعیت: ' + text;
    }

    // ===== چت با تایپ کلمه به کلمه =====
    async function sendMessage() {
        const input = document.getElementById('chatInput');
        const msg = input.value.trim();
        if (!msg || isWaiting) return;

        const box = document.getElementById('chatBox');
        if (box.querySelector('.msg.bot')?.textContent === 'سلام. سوال خود را بپرسید.') {
            box.innerHTML = '';
        }

        const userMsg = document.createElement('div');
        userMsg.className = 'msg user';
        userMsg.textContent = msg;
        userMsg.style.background = currentColor;
        box.appendChild(userMsg);
        box.scrollTop = box.scrollHeight;

        chatHistory.push({role: 'user', content: msg});
        input.value = '';
        isWaiting = true;
        isStreaming = true;
        document.getElementById('sendBtn').disabled = true;
        document.getElementById('sendBtn').style.display = 'none';
        document.getElementById('stopBtn').style.display = 'flex';
        updateStatus('در حال فکر کردن...');

        const botMsg = document.createElement('div');
        botMsg.className = 'msg bot typing';
        const cursor = document.createElement('span');
        cursor.className = 'cursor';
        botMsg.textContent = '';
        botMsg.appendChild(cursor);
        box.appendChild(botMsg);
        box.scrollTop = box.scrollHeight;

       
