<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>MiniGPT</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:system-ui,-apple-system,sans-serif;background:#0a0a0a;color:#e5e7eb;min-height:100vh;padding:20px;direction:rtl}
        .app{max-width:800px;margin:0 auto;display:flex;flex-direction:column;height:95vh}
        .header{display:flex;justify-content:space-between;align-items:center;padding:16px 20px;background:#141414;border-radius:16px;border:1px solid #2a2a2a;margin-bottom:16px;flex-shrink:0}
        .header-left{display:flex;align-items:center;gap:10px}
        .header-left .logo-wrapper{width:36px;height:36px;border-radius:50%;overflow:hidden;border:1.5px solid #2a2a2a;flex-shrink:0;background:#fff;display:flex;align-items:center;justify-content:center}
        .header-left .logo-wrapper img{width:100%;height:100%;object-fit:cover}
        .header h1{font-size:18px;font-weight:600}
        .header-actions{display:flex;align-items:center;gap:10px;position:relative}
        .header-actions .menu-btn{background:transparent;border:1.5px solid #2a2a2a;color:#9ca3af;width:38px;height:38px;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;transition:all .2s}
        .header-actions .menu-btn:hover{border-color:#3b82f6;color:#e5e7eb;background:#1a1a1a}
        .header-actions .menu-btn svg{width:20px;height:20px;stroke:currentColor;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
        .dropdown-menu{display:none;position:absolute;top:45px;left:0;background:#141414;border:1px solid #2a2a2a;border-radius:12px;padding:8px;min-width:180px;z-index:100;box-shadow:0 8px 30px rgba(0,0,0,0.6)}
        .dropdown-menu.open{display:block}
        .dropdown-menu .model-item{display:flex;align-items:center;gap:10px;padding:8px 12px;border-radius:8px;cursor:pointer;transition:all .2s;color:#9ca3af;font-size:13px}
        .dropdown-menu .model-item:hover{background:#1a1a1a;color:#e5e7eb}
        .dropdown-menu .model-item.active{background:#3b82f6;color:#fff}
        .dropdown-menu .model-item img{width:24px;height:24px;border-radius:4px;object-fit:cover;border:1px solid #2a2a2a}
        .main{background:#141414;border-radius:16px;border:1px solid #2a2a2a;padding:20px;flex:1;display:flex;flex-direction:column}
        .chat-box{background:#1a1a1a;border-radius:12px;padding:16px;overflow-y:auto;border:1px solid #2a2a2a;flex:1;min-height:0}
        .chat-box::-webkit-scrollbar{width:4px}
        .chat-box::-webkit-scrollbar-thumb{background:#3b82f6;border-radius:4px}
        .msg-wrapper{display:flex;flex-direction:column;margin-bottom:8px}
        .msg{padding:10px 16px;border-radius:12px;max-width:85%;word-wrap:break-word;font-size:14px;line-height:1.8}
        .msg.user{background:#3b82f6;color:#fff;margin-right:auto;text-align:right}
        .msg.bot{background:#1f1f1f;color:#e5e7eb;margin-left:auto;text-align:left;border:1px solid #2a2a2a}
        .msg.bot.typing{color:#6b7280}
        .msg-actions{display:flex;gap:6px;margin-top:4px;margin-right:4px;opacity:0.5;transition:opacity .2s}
        .msg-wrapper:hover .msg-actions{opacity:1}
        .msg-actions button{background:transparent;border:none;color:#9ca3af;padding:4px 8px;border-radius:6px;cursor:pointer;transition:all .2s;display:flex;align-items:center;gap:4px;font-size:12px}
        .msg-actions button:hover{background:#1a1a1a;color:#e5e7eb}
        .msg-actions button svg{width:16px;height:16px;stroke:currentColor;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
        .msg-actions button.liked{color:#22c55e}
        .msg-actions button.disliked{color:#ef4444}
        .input-area{display:flex;gap:10px;margin-top:12px;flex-shrink:0;align-items:center}
        .input-area input{flex:1;padding:12px 16px;border-radius:12px;border:1px solid #2a2a2a;font-size:14px;outline:none;font-family:inherit;background:#1a1a1a;color:#e5e7eb;transition:border .2s}
        .input-area input::placeholder{color:#6b7280}
        .input-area input:focus{border-color:#3b82f6}
        .input-area .send-btn{width:44px;height:44px;border-radius:50%;border:none;background:#3b82f6;color:#fff;cursor:pointer;transition:all .2s;display:flex;align-items:center;justify-content:center;flex-shrink:0}
        .input-area .send-btn:hover{transform:scale(1.04);background:#2563eb}
        .input-area .send-btn:disabled{opacity:0.5;cursor:not-allowed;transform:none}
        .input-area .send-btn svg{width:22px;height:22px;stroke:currentColor;fill:none;stroke-width:2.5;stroke-linecap:round;stroke-linejoin:round}
        .input-area .stop-btn{width:44px;height:44px;border-radius:50%;border:1.5px solid #ef4444;background:transparent;color:#ef4444;cursor:pointer;transition:all .2s;display:flex;align-items:center;justify-content:center;flex-shrink:0}
        .input-area .stop-btn:hover{background:#ef4444;color:#fff}
        .input-area .stop-btn svg{width:18px;height:18px;stroke:currentColor;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
        .status{font-size:12px;color:#6b7280;text-align:center;margin-top:10px;flex-shrink:0;display:flex;align-items:center;justify-content:center;gap:6px}
        .status svg{width:12px;height:12px;fill:#22c55e}
        @media(max-width:600px){body{padding:12px}.header{padding:12px 16px}.main{padding:16px}.dropdown-menu{left:-10px;min-width:160px}.input-area .send-btn,.input-area .stop-btn{width:38px;height:38px}.input-area .send-btn svg{width:18px;height:18px}}
    </style>
</head>
<body>
    <div class="app">
        <!-- هدر -->
        <div class="header">
            <div class="header-left">
                <div class="logo-wrapper">
                    <img src="https://raw.githubusercontent.com/mohamadkavosi2322-art/my-mini-app/main/file_000000009dc481f4beac7bb7a39278af.png" alt="MiniGPT Logo" />
                </div>
                <h1>ChatGPT</h1>
            </div>
            <div class="header-actions">
                <button class="menu-btn" id="menuBtn" onclick="toggleDropdown()">
                    <svg viewBox="0 0 24 24"><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/></svg>
                </button>
                <div class="dropdown-menu" id="dropdownMenu">
                    <div class="model-item active" data-model="gpt-4o-mini" onclick="selectModel(this)">
                        <img src="https://raw.githubusercontent.com/mohamadkavosi2322-art/my-mini-app/main/file_000000009dc481f4beac7bb7a39278af.png" alt="GPT-4o Mini" />
                        GPT-4o Mini
                    </div>
                    <div class="model-item" data-model="gpt-4o" onclick="selectModel(this)">
                        <img src="https://raw.githubusercontent.com/mohamadkavosi2322-art/my-mini-app/main/file_000000009dc481f4beac7bb7a39278af.png" alt="GPT-4o" />
                        GPT-4o
                    </div>
                    <div class="model-item" data-model="gpt-4" onclick="selectModel(this)">
                        <img src="https://raw.githubusercontent.com/mohamadkavosi2322-art/my-mini-app/main/file_000000009dc481f4beac7bb7a39278af.png" alt="GPT-4" />
                        GPT-4
                    </div>
                    <div class="model-item" data-model="gpt-3.5-turbo" onclick="selectModel(this)">
                        <img src="https://raw.githubusercontent.com/mohamadkavosi2322-art/my-mini-app/main/file_000000009dc481f4beac7bb7a39278af.png" alt="GPT-3.5" />
                        GPT-3.5
                    </div>
                    <div class="model-item" data-model="o1-mini" onclick="selectModel(this)">
                        <img src="https://raw.githubusercontent.com/mohamadkavosi2322-art/my-mini-app/main/file_000000009dc481f4beac7bb7a39278af.png" alt="O1 Mini" />
                        O1 Mini
                    </div>
                </div>
            </div>
        </div>

        <!-- بخش اصلی -->
        <div class="main">
            <div class="chat-box" id="chatBox">
                <div class="msg-wrapper">
                    <div class="msg bot">سلام. سوال خود را بپرسید.</div>
                    <div class="msg-actions">
                        <button onclick="copyMessage(this)"><svg viewBox="0 0 24 24"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/></svg></button>
                        <button onclick="likeMessage(this)"><svg viewBox="0 0 24 24"><path d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 1.98-1.72l1.38-9A2 2 0 0 0 19.66 9H14z"/><path d="M7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3"/></svg></button>
                        <button onclick="dislikeMessage(this)"><svg viewBox="0 0 24 24"><path d="M10 15v4a3 3 0 0 0 3 3l4-9V2H5.72a2 2 0 0 0-1.98 1.72l-1.38 9A2 2 0 0 0 4.34 15H10z"/><path d="M17 2h3a2 2 0 0 1 2 2v7a2 2 0 0 1-2 2h-3"/></svg></button>
                        <button onclick="feedbackMessage(this)"><svg viewBox="0 0 24 24"><path d="M21 15a4 4 0 0 1-4 4H8l-5 3V7a4 4 0 0 1 4-4h10a4 4 0 0 1 4 4z"/><path d="M8 9h8"/><path d="M8 12h5"/></svg></button>
                        <button onclick="shareMessage(this)"><svg viewBox="0 0 24 24"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/></svg></button>
                    </div>
                </div>
            </div>

            <div class="input-area">
                <input type="text" id="chatInput" placeholder="پیام خود را بنویسید..." />
                <button class="send-btn" id="sendBtn" onclick="sendMessage()">
                    <svg viewBox="0 0 24 24"><path d="M22 2L11 13M22 2l-7 20-4-9-9-4 20-7z"/></svg>
                </button>
            </div>

            <div class="status">
                <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/></svg>
                وضعیت: فعال
            </div>
        </div>
    </div>

    <script>
        const AVALAI_KEY = "aa-7Z6scGydDdrtJA67WKgoj8cX1ztH9rgeULRwEUB1L7sZeKId";
        const AVALAI_URL = "https://api.avalai.ir/v1/chat/completions";

        let currentModel = "gpt-4o-mini";
        let isWaiting = false;
        let lastBotResponse = '';
        let chatHistory = [];
        let typingInterval = null;

        // ===== منوی مدل =====
        function toggleDropdown() {
            document.getElementById('dropdownMenu').classList.toggle('open');
        }

        function selectModel(element) {
            document.querySelectorAll('.dropdown-menu .model-item').forEach(i => i.classList.remove('active'));
            element.classList.add('active');
            currentModel = element.dataset.model;
            document.querySelector('.status').innerHTML = `
                <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/></svg>
                وضعیت: فعال · مدل: ${element.textContent.trim()}
            `;
            document.getElementById('dropdownMenu').classList.remove('open');
        }

        // ===== دکمه‌های زیر پیام =====
        function copyMessage(btn) {
            const msgWrapper = btn.closest('.msg-wrapper');
            const msg = msgWrapper.querySelector('.msg');
            if (msg) {
                navigator.clipboard.writeText(msg.textContent).then(() => {
                    btn.style.color = '#22c55e';
                    setTimeout(() => btn.style.color = '', 1500);
                });
            }
        }

        function likeMessage(btn) {
            btn.classList.toggle('liked');
        }

        function dislikeMessage(btn) {
            btn.classList.toggle('disliked');
        }

        function feedbackMessage(btn) {
            const feedback = prompt('نظر خود را بنویسید:');
            if (feedback) {
                alert('بازخورد شما ثبت شد: ' + feedback);
            }
        }

        function shareMessage(btn) {
            const msgWrapper = btn.closest('.msg-wrapper');
            const msg = msgWrapper.querySelector('.msg');
            if (msg && navigator.share) {
                navigator.share({ text: msg.textContent }).catch(() => {});
            } else if (msg) {
                navigator.clipboard.writeText(msg.textContent).then(() => {
                    btn.style.color = '#22c55e';
                    setTimeout(() => btn.style.color = '', 1500);
                });
            }
        }

        // ===== تایپ روان =====
        function typeMessage(element, text) {
            let index = 0;
            element.textContent = '';
            clearInterval(typingInterval);
            typingInterval = setInterval(() => {
                if (index < text.length) {
                    element.textContent += text.charAt(index);
                    index++;
                    const chatBox = document.getElementById('chatBox');
                    chatBox.scrollTop = chatBox.scrollHeight;
                } else {
                    clearInterval(typingInterval);
                }
            }, 15);
        }

        // ===== ارسال پیام =====
        function updateStatus(text, isError = false) {
            const status = document.querySelector('.status');
            if (isError) {
                status.innerHTML = `
                    <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10" fill="#ef4444"/></svg>
                    وضعیت: ${text}
                `;
            } else {
                status.innerHTML = `
                    <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/></svg>
                    وضعیت: ${text}
                `;
            }
        }

        async function sendMessage() {
            const input = document.getElementById('chatInput');
            const msg = input.value.trim();
            if (!msg || isWaiting) return;

            const box = document.getElementById('chatBox');

            const userWrapper = document.createElement('div');
            userWrapper.className = 'msg-wrapper';
            const userMsg = document.createElement('div');
            userMsg.className = 'msg user';
            userMsg.textContent = msg;
            userWrapper.appendChild(userMsg);
            box.appendChild(userWrapper);
            box.scrollTop = box.scrollHeight;

            chatHistory.push({ role: 'user', content: msg });

            input.value = '';
            isWaiting = true;
            const sendBtn = document.getElementById('sendBtn');
            sendBtn.disabled = true;

            // دکمه توقف
            const stopBtn = document.createElement('button');
            stopBtn.className = 'stop-btn';
            stopBtn.innerHTML = `<svg viewBox="0 0 24 24"><rect x="6" y="6" width="12" height="12"/></svg>`;
            stopBtn.onclick = function() {
                clearInterval(typingInterval);
                isWaiting = false;
                sendBtn.disabled = false;
                this.remove();
                sendBtn.style.display = 'flex';
                updateStatus('متوقف شد');
                const typingMsg = botWrapper.querySelector('.typing');
                if (typingMsg) typingMsg.textContent = '⏹️ تولید متوقف شد';
            };
            const inputArea = document.querySelector('.input-area');
            sendBtn.style.display = 'none';
            inputArea.appendChild(stopBtn);
            updateStatus('در حال فکر کردن...');

            const botWrapper = document.createElement('div');
            botWrapper.className = 'msg-wrapper';
            const botMsg = document.createElement('div');
            botMsg.className = 'msg bot typing';
            botMsg.textContent = 'در حال نوشتن...';
            botWrapper.appendChild(botMsg);
            box.appendChild(botWrapper);
            box.scrollTop = box.scrollHeight;

            try {
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
                    })
                });

                const data = await response.json();
                if (data.choices && data.choices[0]) {
                    const reply = data.choices[0].message.content;
                    botMsg.className = 'msg bot';
                    botMsg.textContent = '';
                    typeMessage(botMsg, reply);
                    lastBotResponse = reply;
                    chatHistory.push({ role: 'assistant', content: reply });
                    updateStatus('فعال · پاسخ دریافت شد');

                    const actions = document.createElement('div');
                    actions.className = 'msg-actions';
                    actions.innerHTML = `
                        <button onclick="copyMessage(this)"><svg viewBox="0 0 24 24"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/></svg></button>
                        <button onclick="likeMessage(this)"><svg viewBox="0 0 24 24"><path d="M14 9V5a3 3 0 0 0-3-3l-4 9v11h11.28a2 2 0 0 0 1.98-1.72l1.38-9A2 2 0 0 0 19.66 9H14z"/><path d="M7 22H4a2 2 0 0 1-2-2v-7a2 2 0 0 1 2-2h3"/></svg></button>
                        <button onclick="dislikeMessage(this)"><svg viewBox="0 0 24 24"><path d="M10 15v4a3 3 0 0 0 3 3l4-9V2H5.72a2 2 0 0 0-1.98 1.72l-1.38 9A2 2 0 0 0 4.34 15H10z"/><path d="M17 2h3a2 2 0 0 1 2 2v7a2 2 0 0 1-2 2h-3"/></svg></button>
                        <button onclick="feedbackMessage(this)"><svg viewBox="0 0 24 24"><path d="M21 15a4 4 0 0 1-4 4H8l-5 3V7a4 4 0 0 1 4-4h10a4 4 0 0 1 4 4z"/><path d="M8 9h8"/><path d="M8 12h5"/></svg></button>
                        <button onclick="shareMessage(this)"><svg viewBox="0 0 24 24"><circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/></svg></button>
                    `;
                    botWrapper.appendChild(actions);
                } else {
                    botMsg.className = 'msg bot';
                    botMsg.textContent = 'خطا در دریافت پاسخ';
                    updateStatus('خطا', true);
                }
            } catch (e) {
                botMsg.className = 'msg bot';
                botMsg.textContent = 'خطا در ارتباط با سرور';
                updateStatus('خطا', true);
            }

            // برگرداندن دکمه ارسال
            stopBtn.remove();
            sendBtn.style.display = 'flex';
            isWaiting = false;
            sendBtn.disabled = false;
            box.scrollTop = box.scrollHeight;
        }

        // ===== ارسال با Enter =====
        document.getElementById('chatInput').addEventListener('keydown', function(e) {
            if (e.key === 'Enter') {
                e.preventDefault();
                sendMessage();
            }
        });
    </script>
</body>
</html>
