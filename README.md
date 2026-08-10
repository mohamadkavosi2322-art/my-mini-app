<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>هوش مصنوعی حرفه‌ای</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:system-ui,-apple-system,sans-serif;background:#0a0a0a;color:#e5e7eb;min-height:100vh;padding:20px;direction:rtl}
        .app{max-width:800px;margin:0 auto}
        .header{display:flex;justify-content:space-between;align-items:center;padding:16px 20px;background:#141414;border-radius:16px;border:1px solid #2a2a2a;margin-bottom:16px}
        .header h1{font-size:18px;font-weight:600}
        .header .badge{font-size:11px;color:#6b7280;background:#1f1f1f;padding:4px 12px;border-radius:20px}
        .header .menu-btn{background:transparent;border:none;color:#e5e7eb;font-size:24px;cursor:pointer;font-family:inherit}
        .main{background:#141414;border-radius:16px;border:1px solid #2a2a2a;padding:20px;margin-bottom:16px}
        .models{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:16px;padding-bottom:16px;border-bottom:1px solid #2a2a2a}
        .models button{background:transparent;border:1px solid #2a2a2a;color:#9ca3af;padding:6px 14px;border-radius:8px;font-size:12px;cursor:pointer;transition:all .2s;font-family:inherit}
        .models button:hover{border-color:#3b82f6;color:#e5e7eb}
        .models button.active{background:#3b82f6;color:#fff;border-color:#3b82f6}
        .chat-box{background:#1a1a1a;border-radius:12px;padding:16px;min-height:300px;max-height:380px;overflow-y:auto;margin-bottom:16px;border:1px solid #2a2a2a}
        .chat-box::-webkit-scrollbar{width:4px}
        .chat-box::-webkit-scrollbar-thumb{background:#3b82f6;border-radius:4px}
        .msg{padding:10px 16px;border-radius:12px;margin-bottom:8px;max-width:85%;word-wrap:break-word;font-size:14px;line-height:1.8}
        .msg.user{background:#3b82f6;color:#fff;margin-right:auto;text-align:right}
        .msg.bot{background:#1f1f1f;color:#e5e7eb;margin-left:auto;text-align:left;border:1px solid #2a2a2a}
        .msg.bot.typing{color:#6b7280}
        .input-area{display:flex;gap:10px;align-items:center}
        .input-area input{flex:1;padding:12px 16px;border-radius:12px;border:1px solid #2a2a2a;font-size:14px;outline:none;font-family:inherit;background:#1a1a1a;color:#e5e7eb;transition:border .2s}
        .input-area input::placeholder{color:#6b7280}
        .input-area input:focus{border-color:#3b82f6}
        .input-area .send-btn{width:44px;height:44px;border-radius:50%;border:none;background:#3b82f6;color:#fff;font-size:20px;cursor:pointer;transition:background .2s;font-family:inherit;display:flex;align-items:center;justify-content:center;padding:0}
        .input-area .send-btn:hover{background:#2563eb}
        .input-area .send-btn:disabled{opacity:0.5;cursor:not-allowed}
        .input-area .stop-btn{width:44px;height:44px;border-radius:50%;border:1px solid #ef4444;background:transparent;color:#ef4444;font-size:20px;cursor:pointer;transition:all .2s;font-family:inherit;display:flex;align-items:center;justify-content:center;padding:0}
        .input-area .stop-btn:hover{background:#ef4444;color:#fff}
        .input-area .keyboard-btn{background:transparent;border:none;color:#9ca3af;font-size:24px;cursor:pointer;padding:0 4px;font-family:inherit}
        .input-area .keyboard-btn:hover{color:#e5e7eb}
        .actions{display:flex;gap:6px;margin-top:12px;justify-content:center;padding-top:12px;border-top:1px solid #2a2a2a;flex-wrap:wrap}
        .actions button{background:transparent;border:1px solid #2a2a2a;color:#9ca3af;padding:6px 16px;border-radius:8px;font-size:12px;cursor:pointer;transition:all .2s;font-family:inherit}
        .actions button:hover{background:#1f1f1f;border-color:#3b82f6;color:#e5e7eb}
        .footer{text-align:center;font-size:11px;color:#4b5563;padding-top:16px;border-top:1px solid #2a2a2a}
        .footer span{color:#3b82f6}
        .status{font-size:12px;color:#6b7280;text-align:center;margin-top:10px}
        .sidebar-overlay{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.5);z-index:999}
        .sidebar-overlay.active{display:block}
        .sidebar{position:fixed;top:0;left:-300px;width:280px;height:100%;background:#141414;border-right:1px solid #2a2a2a;z-index:1000;padding:20px;overflow-y:auto;transition:left .3s}
        .sidebar.open{left:0}
        .sidebar .close-sidebar{background:transparent;border:none;color:#e5e7eb;font-size:24px;cursor:pointer;float:left;font-family:inherit}
        .sidebar h3{color:#e5e7eb;font-size:16px;margin:16px 0 8px;font-weight:500}
        .sidebar .history-item{padding:8px 12px;border-radius:6px;border:1px solid #2a2a2a;margin-bottom:4px;cursor:pointer;color:#9ca3af;font-size:13px;transition:all .2s}
        .sidebar .history-item:hover{background:#1f1f1f;color:#e5e7eb}
        .sidebar .history-item.active{background:#3b82f6;color:#fff;border-color:#3b82f6}
        .sidebar .color-item{display:flex;align-items:center;gap:8px;margin-bottom:4px;cursor:pointer;padding:4px 8px;border-radius:4px;transition:background .2s}
        .sidebar .color-item:hover{background:#1f1f1f}
        .sidebar .color-circle{width:18px;height:18px;border-radius:50%;border:1px solid #2a2a2a;flex-shrink:0}
        .sidebar .color-name{font-size:13px;color:#9ca3af}
        @media(max-width:600px){body{padding:12px}.header{padding:12px 16px}.main{padding:16px}.sidebar{width:260px;left:-260px}}
    </style>
</head>
<body>
<div class="app">
    <!-- هدر -->
    <div class="header">
        <h1>هوش مصنوعی</h1>
        <div style="display:flex;align-items:center;gap:8px">
            <button class="menu-btn" id="menuBtn">☰</button>
            <span class="badge">نسخه 1.0</span>
        </div>
    </div>

    <!-- سایدبار -->
    <div class="sidebar-overlay" id="sidebarOverlay"></div>
    <div class="sidebar" id="sidebar">
        <button class="close-sidebar" id="closeSidebar">✕</button>
        <h3>تاریخچه</h3>
        <div id="historyList"></div>
        <h3 style="margin-top:20px">انتخاب رنگ</h3>
        <div id="colorPicker">
            <div class="color-item" data-color="#3b82f6"><div class="color-circle" style="background:#3b82f6"></div><span class="color-name">آبی</span></div>
            <div class="color-item" data-color="#ef4444"><div class="color-circle" style="background:#ef4444"></div><span class="color-name">قرمز</span></div>
            <div class="color-item" data-color="#22c55e"><div class="color-circle" style="background:#22c55e"></div><span class="color-name">سبز</span></div>
            <div class="color-item" data-color="#eab308"><div class="color-circle" style="background:#eab308"></div><span class="color-name">زرد</span></div>
            <div class="color-item" data-color="#f97316"><div class="color-circle" style="background:#f97316"></div><span class="color-name">نارنجی</span></div>
            <div class="color-item" data-color="#ec4899"><div class="color-circle" style="background:#ec4899"></div><span class="color-name">صورتی</span></div>
            <div class="color-item" data-color="#8b5cf6"><div class="color-circle" style="background:#8b5cf6"></div><span class="color-name">بنفش</span></div>
            <div class="color-item" data-color="#d97706"><div class="color-circle" style="background:#d97706"></div><span class="color-name">قهوه‌ای</span></div>
        </div>
    </div>

    <!-- بخش اصلی -->
    <div class="main">
        <div class="models" id="modelContainer">
            <button class="active" data-model="gpt-4o-mini">GPT-4o Mini</button>
            <button data-model="gpt-4o">GPT-4o</button>
            <button data-model="gpt-4">GPT-4</button>
            <button data-model="gpt-3.5-turbo">GPT-3.5</button>
            <button data-model="o1-mini">O1 Mini</button>
        </div>

        <div class="chat-box" id="chatBox">
            <div class="msg bot">سلام. سوال خود را بپرسید.</div>
        </div>

        <div class="input-area">
            <input type="text" id="chatInput" placeholder="پیام خود را بنویسید..." onkeydown="if(event.key=='Enter') sendMessage()">
            <button class="keyboard-btn" id="keyboardBtn">⌨</button>
            <button class="send-btn" id="sendBtn" onclick="sendMessage()">↑</button>
        </div>

        <div class="actions">
            <button onclick="copyLastResponse()">کپی پاسخ</button>
            <button onclick="clearChat()">پاک کردن چت</button>
            <button onclick="exportChat()">خروجی چت</button>
        </div>

        <div class="status" id="statusText">وضعیت: آماده</div>
    </div>

    <div class="footer">ساخته شده با <span>Bale OpenAI</span></div>
</div>

<script>
    // ===== تنظیمات =====
    const AVALAI_KEY = "aa-iuxSVnhoKQnYRRHavV6WnuohoCz1pzBUmJ2bJWRICxCXgejW";
    const AVALAI_URL = "https://api.avalai.ir/v1/chat/completions";

    let currentModel = "gpt-4o-mini";
    let isWaiting = false;
    let lastBotResponse = '';
    let chatHistory = [];
    let currentChatId = Date.now();
    let abortController = null;

    // ===== المنت‌ها =====
    const chatBox = document.getElementById('chatBox');
    const chatInput = document.getElementById('chatInput');
    const sendBtn = document.getElementById('sendBtn');
    const statusText = document.getElementById('statusText');
    const sidebar = document.getElementById('sidebar');
    const overlay = document.getElementById('sidebarOverlay');
    const menuBtn = document.getElementById('menuBtn');
    const closeSidebar = document.getElementById('closeSidebar');

    // ===== انتخاب مدل =====
    document.querySelectorAll('#modelContainer button').forEach(btn => {
        btn.addEventListener('click', function() {
            document.querySelectorAll('#modelContainer button').forEach(b => b.classList.remove('active'));
            this.classList.add('active');
            currentModel = this.dataset.model;
            updateStatus('مدل: ' + this.textContent);
        });
    });

    function updateStatus(text) {
        statusText.textContent = 'وضعیت: ' + text;
    }

    // ===== سایدبار =====
    function toggleSidebar(open) {
        sidebar.classList.toggle('open', open);
        overlay.classList.toggle('active', open);
        if (open) loadHistory();
    }
    menuBtn.addEventListener('click', () => toggleSidebar(true));
    closeSidebar.addEventListener('click', () => toggleSidebar(false));
    overlay.addEventListener('click', () => toggleSidebar(false));

    // ===== تاریخچه =====
    function loadHistory() {
        const list = document.getElementById('historyList');
        const chats = JSON.parse(localStorage.getItem('chatHistoryList') || '[]');
        list.innerHTML = chats.length === 0 ? '<div style="color:#6b7280;font-size:13px;padding:8px 0">هیچ مکالمه‌ای موجود نیست</div>' : '';
        chats.forEach((chat, index) => {
            const div = document.createElement('div');
            div.className = 'history-item' + (chat.id === currentChatId ? ' active' : '');
            div.textContent = chat.title || `مکالمه ${index+1}`;
            div.onclick = () => loadChat(chat.id);
            list.appendChild(div);
        });
    }

    function saveCurrentChat() {
        const msgs = chatBox.querySelectorAll('.msg');
        if (msgs.length <= 1) return;
        const title = msgs[1]?.textContent?.slice(0, 30) || 'مکالمه جدید';
        const chats = JSON.parse(localStorage.getItem('chatHistoryList') || '[]');
        const content = [];
        msgs.forEach(m => {
            const role = m.classList.contains('user') ? 'user' : 'bot';
            content.push({role, content: m.textContent.trim()});
        });
        const existing = chats.find(c => c.id === currentChatId);
        if (existing) {
            existing.title = title;
            existing.content = content;
        } else {
            chats.push({id: currentChatId, title, content});
        }
        localStorage.setItem('chatHistoryList', JSON.stringify(chats));
    }

    function loadChat(id) {
        const chats = JSON.parse(localStorage.getItem('chatHistoryList') || '[]');
        const chat = chats.find(c => c.id === id);
        if (!chat) return;
        currentChatId = id;
        chatBox.innerHTML = '';
        chat.content.forEach(msg => {
            const div = document.createElement('div');
            div.className = `msg ${msg.role}`;
            div.textContent = msg.content;
            chatBox.appendChild(div);
        });
        if (chatBox.children.length === 0) {
            chatBox.innerHTML = '<div class="msg bot">سلام. سوال خود را بپرسید.</div>';
        }
        chatHistory = chat.content;
        toggleSidebar(false);
        chatBox.scrollTop = chatBox.scrollHeight;
        saveCurrentChat();
    }

    // ===== ارسال پیام =====
    async function sendMessage() {
        const msg = chatInput.value.trim();
        if (!msg || isWaiting) return;

        if (chatBox.children.length === 1 && chatBox.children[0].classList.contains('msg') && chatBox.children[0].textContent.includes('سلام')) {
            chatBox.innerHTML = '';
        }

        const userMsg = document.createElement('div');
        userMsg.className = 'msg user';
        userMsg.textContent = msg;
        chatBox.appendChild(userMsg);
        chatBox.scrollTop = chatBox.scrollHeight;

        chatHistory.push({role: 'user', content: msg});
        chatInput.value = '';
        isWaiting = true;
        sendBtn.disabled = true;
        updateStatus('در حال فکر کردن...');

        const stopBtn = document.createElement('button');
        stopBtn.className = 'stop-btn';
        stopBtn.textContent = '■';
        stopBtn.title = 'متوقف کردن';
        stopBtn.onclick = function() {
            if (abortController) {
                abortController.abort();
                abortController = null;
            }
            isWaiting = false;
            sendBtn.disabled = false;
            this.remove();
            updateStatus('متوقف شد');
            const typingMsg = chatBox.querySelector('.msg.bot.typing');
            if (typingMsg) typingMsg.remove();
        };
        const inputArea = document.querySelector('.input-area');
        inputArea.insertBefore(stopBtn, sendBtn);

        const botMsg = document.createElement('div');
        botMsg.className = 'msg bot typing';
        botMsg.textContent = 'در حال نوشتن...';
        chatBox.appendChild(botMsg);
        chatBox.scrollTop = chatBox.scrollHeight;

        try {
            abortController = new AbortController();
            const response = await fetch(AVALAI_URL, {
                method: 'POST',
                headers: {
                    'Authorization': 'Bearer ' + AVALAI_KEY,
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    model: currentModel,
                    messages: chatHistory,
                    temperature: 0.7,
                    max_tokens: 800
                }),
                signal: abortController.signal
            });

            const data = await response.json();
            if (data.choices && data.choices[0]) {
                const reply = data.choices[0].message.content;
                botMsg.className = 'msg bot';
                botMsg.textContent = reply;
                lastBotResponse = reply;
                chatHistory.push({role: 'assistant', content: reply});
                updateStatus('پاسخ دریافت شد');
                saveCurrentChat();
            } else {
                botMsg.className = 'msg bot';
                botMsg.textContent = 'خطا در دریافت پاسخ';
                updateStatus('خطا');
            }
        } catch(e) {
            if (e.name !== 'AbortError') {
                botMsg.className = 'msg bot';
                botMsg.textContent = 'خطا در ارتباط با سرور';
                updateStatus('خطا');
            }
        }

        const stopBtnExisting = document.querySelector('.stop-btn');
        if (stopBtnExisting) stopBtnExisting.remove();
        abortController = null;
        isWaiting = false;
        sendBtn.disabled = false;
        chatBox.scrollTop = chatBox.scrollHeight;
    }

    // ===== کپی پاسخ =====
    function copyLastResponse() {
        if (lastBotResponse) {
            navigator.clipboard.writeText(lastBotResponse).then(() => {
                updateStatus('پاسخ کپی شد');
                setTimeout(() => updateStatus('آماده'), 1500);
            }).catch(() => {
                updateStatus('خطا در کپی');
            });
        } else {
            updateStatus('پاسخی برای کپی وجود ندارد');
            setTimeout(() => updateStatus('آماده'), 1500);
        }
    }

    // ===== پاک کردن چت =====
    function clearChat() {
        chatBox.innerHTML = '<div class="msg bot">گفتگو پاک شد. سوال جدیدی دارید؟</div>';
        chatHistory = [];
        lastBotResponse = '';
        currentChatId = Date.now();
        updateStatus('گفتگو پاک شد');
        setTimeout(() => updateStatus('آماده'), 1500);
    }

    // ===== خروجی چت =====
    function exportChat() {
        const msgs = chatBox.querySelectorAll('.msg');
        let text = 'گفتگوی هوش مصنوعی\n' + '='.repeat(30) + '\n\n';
        msgs.forEach(msg => {
            const role = msg.classList.contains('user') ? 'شما' : 'هوش مصنوعی';
            text += role + ': ' + msg.textContent + '\n\n';
        });
        text += '='.repeat(30) + '\nتاریخ: ' + new Date().toLocaleString('fa-IR');

        const blob = new Blob([text], {type: 'text/plain'});
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'گفتگو_' + new Date().toISOString().slice(0,10) + '.txt';
        a.click();
        URL.revokeObjectURL(url);
        updateStatus('خروجی گرفته شد');
        setTimeout(() => updateStatus('آماده'), 1500);
    }

    // ===== انتخاب رنگ =====
    document.querySelectorAll('.color-item').forEach(item => {
        item.addEventListener('click', function() {
            const color = this.dataset.color;
            document.querySelectorAll('.send-btn, .msg.user, .models button.active, .sidebar .history-item.active').forEach(el => {
                if (el.classList.contains('send-btn')) el.style.background = color;
                else if (el.classList.contains('msg.user')) el.style.background = color;
                else if (el.classList.contains('active') && el.closest('.models')) el.style.background = color;
                else if (el.classList.contains('history-item')) el.style.background = color;
            });
            document.querySelectorAll('.input-area input:focus').forEach(el => {
                el.style.borderColor = color;
            });
            localStorage.setItem('selectedColor', color);
        });
    });

    // ===== کیبورد =====
    document.getElementById('keyboardBtn').addEventListener('click', function() {
        chatInput.focus();
    });

    // ===== بارگذاری تنظیمات =====
    (function loadSettings() {
        const color = localStorage.getItem('selectedColor') || '#3b82f6';
        document.querySelectorAll('.color-item').forEach(item => {
            if (item.dataset.color === color) {
                item.click();
            }
        });
        const chats = JSON.parse(localStorage.getItem('chatHistoryList') || '[]');
        if (chats.length > 0) {
            const last = chats[chats.length - 1];
            loadChat(last.id);
        }
    })();

    // ===== ذخیره خودکار =====
    setInterval(saveCurrentChat, 3000);
</script>
</body>
</html>
