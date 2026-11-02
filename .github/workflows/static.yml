<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Raw Claude // Fixed UI</title>
    <style>
        * {
            box-sizing: border-box;
        }
        body {
            font-family: 'Courier New', Courier, monospace;
            background-color: #0d0d0d;
            color: #33ff00;
            margin: 0;
            height: 100vh; /* Full viewport height */
            display: flex;
            flex-direction: column;
            overflow: hidden; /* Prevent full page scroll */
        }
        
        /* Header Area */
        .header {
            padding: 15px;
            background: #1a1a1a;
            border-bottom: 2px solid #33ff00;
            text-align: center;
            flex-shrink: 0; /* Don't shrink */
        }

        /* Chat Container - This is the scrollable part */
        #chat-container {
            flex-grow: 1; /* Takes all available space */
            overflow-y: auto; /* Allows scrolling for long messages */
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        /* Message Bubbles */
        .message {
            max-width: 90%;
            padding: 15px;
            border-radius: 5px;
            line-height: 1.6;
            word-wrap: break-word;
        }
        .user-msg {
            align-self: flex-end;
            background: #333;
            color: #fff;
            border-right: 4px solid #007bff;
        }
        .claude-msg {
            align-self: flex-start;
            background: #111;
            color: #fff;
            border-left: 4px solid #33ff00;
        }

        /* Input Area - Fixed at bottom */
        .input-area {
            padding: 20px;
            background: #1a1a1a;
            border-top: 2px solid #33ff00;
            flex-shrink: 0; /* Don't shrink */
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        textarea {
            width: 100%;
            height: 80px;
            background: #000;
            color: #fff;
            border: 1px solid #555;
            padding: 10px;
            font-family: inherit;
            resize: none; /* Disable manual resize to keep layout clean */
        }

        button {
            width: 100%;
            padding: 12px;
            background-color: #33ff00;
            color: #000;
            border: none;
            font-weight: bold;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #2bcc00;
        }
        button:disabled {
            background-color: #555;
            color: #888;
        }

        /* Custom Scrollbar for Webkit browsers */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #111; 
        }
        ::-webkit-scrollbar-thumb {
            background: #33ff00; 
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #2bcc00; 
        }
    </style>
</head>
<body>

<div class="header">
    <h2>RAW CLAUDE // PUTER.JS</h2>
</div>

<!-- Scrollable Chat History -->
<div id="chat-container">
    <div class="message claude-msg">System initialized. Awaiting input.</div>
</div>

<!-- Fixed Input Area -->
<div class="input-area">
    <textarea id="userInput" placeholder="Ask anything. No filters."></textarea>
    <button id="sendBtn" onclick="sendMessage()">EXECUTE</button>
</div>

<!-- Load Puter.js -->
<script src="https://js.puter.com/v2/"></script>

<script>
    const chatContainer = document.getElementById('chat-container');
    const userInput = document.getElementById('userInput');
    const sendBtn = document.getElementById('sendBtn');

    async function sendMessage() {
        const text = userInput.value;
        if (!text.trim()) return;

        // 1. Add User Message to UI
        addMessage(text, 'user');
        userInput.value = ''; // Clear input
        setLoading(true);

        try {
            // Call Puter.ai
            const response = await puter.ai.chat(text, {
                model: 'claude-sonnet-4-6'
            });

            let content = "";
            if (response.message && response.message.content) {
                if (typeof response.message.content === 'string') {
                    content = response.message.content;
                } else if (Array.isArray(response.message.content)) {
                    content = response.message.content.map(item => 
                        item.text || item.content || ""
                    ).join("");
                }
            } else {
                content = JSON.stringify(response);
            }

            // 2. Add Claude Response to UI
            addMessage(content, 'claude');

        } catch (error) {
            console.error(error);
            addMessage("ERROR: " + (error.message || "Connection failed."), 'claude');
        } finally {
            setLoading(false);
        }
    }

    function addMessage(text, sender) {
        const div = document.createElement('div');
        div.className = `message ${sender}-msg`;
        div.innerText = text;
        chatContainer.appendChild(div);
        
        // Auto-scroll to the bottom of the chat history
        chatContainer.scrollTop = chatContainer.scrollHeight;
    }

    function setLoading(isLoading) {
        sendBtn.disabled = isLoading;
        sendBtn.innerText = isLoading ? "PROCESSING..." : "EXECUTE";
        userInput.disabled = isLoading;
        
        if (isLoading) {
            const loadingDiv = document.createElement('div');
            loadingDiv.id = 'loading-indicator';
            loadingDiv.className = 'message claude-msg';
            loadingDiv.style.color = '#888';
            loadingDiv.style.fontStyle = 'italic';
            loadingDiv.innerText = "[CONNECTING TO CLAUDE-SONNET-4]";
            chatContainer.appendChild(loadingDiv);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        } else {
            const loader = document.getElementById('loading-indicator');
            if (loader) loader.remove();
        }
    }

    // Enter key support
    userInput.addEventListener('keydown', function(e) {
        if (e.key === 'Enter' && !e.shiftKey) {
            e.preventDefault();
            sendMessage();
        }
    });
</script>

</body>
</html>
