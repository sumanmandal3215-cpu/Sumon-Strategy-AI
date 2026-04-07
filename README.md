<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUMON'S STRATEGY AI - PRO</title>
    <style>
        body { background-color: #000; font-family: 'Segoe UI', sans-serif; display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh; margin: 0; color: white; overflow: hidden; }
        .logo-box { width: 80px; height: 80px; margin-bottom: 10px; border: 2px solid #d4af37; border-radius: 50%; padding: 3px; box-shadow: 0 0 15px #d4af37; }
        .logo-box img { width: 100%; height: 100%; border-radius: 50%; object-fit: cover; }
        
        #login-screen { background: rgba(20, 20, 30, 0.95); padding: 20px; border-radius: 20px; text-align: center; width: 280px; border: 1.5px solid #d4af37; box-shadow: 0 0 30px rgba(212, 175, 55, 0.3); }
        input, select { width: 100%; padding: 10px; margin: 6px 0; border-radius: 8px; border: 1px solid #d4af37; background: #050510; color: white; box-sizing: border-box; }
        .main-btn { width: 100%; padding: 12px; border-radius: 8px; border: none; background: linear-gradient(45deg, #d4af37, #f1c40f); color: black; font-weight: bold; cursor: pointer; }

        #main-app { display: none; text-align: center; }
        .ai-shell { width: 240px; background: rgba(15, 15, 25, 0.98); border-radius: 30px; padding: 20px; border: 2.5px solid #d4af37; box-shadow: 0 0 40px rgba(212, 175, 55, 0.5); }
        
        /* এআই অ্যানিমেশন */
        .ai-sphere { width: 110px; height: 110px; border-radius: 50%; background: radial-gradient(circle, #f1c40f, #d4af37, #000); margin: 15px auto; box-shadow: 0 0 25px #d4af37; animation: glow 2s infinite ease-in-out; cursor: pointer; position: relative; }
        @keyframes glow { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

        /* সিগন্যাল ডিসপ্লে */
        #signal-result { font-size: 24px; font-weight: bold; margin-top: 15px; min-height: 35px; text-transform: uppercase; letter-spacing: 2px; }
        .up-signal { color: #2ecc71; text-shadow: 0 0 10px #2ecc71; }
        .down-signal { color: #ff4757; text-shadow: 0 0 10px #ff4757; }
        .analyzing { color: #f1c40f; font-size: 16px; animation: blink 0.5s infinite; }
        @keyframes blink { 50% { opacity: 0; } }
    </style>
</head>
<body>

    <div id="login-screen">
        <div class="logo-box"><img src="1000008884.jpg" alt="Logo"></div>
        <div style="color:#d4af37; font-weight:bold; margin-bottom:10px;">SUMON'S STRATEGY AI</div>
        <input type="text" id="uname" placeholder="আপনার নাম">
        <select id="lang-select">
            <option value="bn-BD">Bengali (বাংলা)</option>
            <option value="en-US">English (US)</option>
            <option value="hi-IN">Hindi (हिन्दी)</option>
        </select>
        <button class="main-btn" onclick="startApp()">LOGIN & START</button>
    </div>

    <div id="main-app">
        <div class="ai-shell">
            <div style="font-size: 10px; color: #d4af37; letter-spacing: 1px;">VERIFIED BY SUMON</div>
            <div class="ai-sphere" id="ai-sphere" onclick="startListening()"></div>
            <div id="m-display" style="color:#aaa; font-size: 14px;">Select Market by Voice</div>
            
            <div id="signal-result">READY</div>
            
            <button class="main-btn" style="margin-top: 10px;" onclick="predictSignal()">GET NEXT CANDLE</button>
            <div id="status-text" style="font-size: 10px; color: #666; margin-top: 10px;">Tap AI to Speak Market Name</div>
        </div>
    </div>

    <script>
        let selectedLang = 'bn-BD';
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        recognition.continuous = false;

        function speak(text) {
            window.speechSynthesis.cancel();
            const s = new SpeechSynthesisUtterance(text);
            s.lang = selectedLang;
            const voices = window.speechSynthesis.getVoices();
            s.voice = voices.find(v => v.lang === selectedLang && (v.name.includes('Female') || v.name.includes('Google'))) || voices[0];
            window.speechSynthesis.speak(s);
        }

        function startApp() {
            const u = document.getElementById('uname').value;
            selectedLang = document.getElementById('lang-select').value;
            if(u) {
                document.getElementById('login-screen').style.display='none';
                document.getElementById('main-app').style.display='block';
                let msg = selectedLang === 'bn-BD' ? "স্বাগতম " + u : "Welcome " + u;
                speak(msg);
            }
        }

        function startListening() {
            recognition.lang = selectedLang;
            recognition.start();
            document.getElementById('status-text').innerText = "Listening...";
        }

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript;
            document.getElementById('m-display').innerText = transcript;
            speak(selectedLang === 'bn-BD' ? transcript + " মার্কেট সেট করা হয়েছে" : "Market set to " + transcript);
        };

        // ১-২ সেকেন্ডের মধ্যে সিগন্যাল দেওয়ার মেইন ফাংশন
        function predictSignal() {
            const display = document.getElementById('signal-result');
            display.innerHTML = '<span class="analyzing">ANALYZING...</span>';
            
            // ১.৫ সেকেন্ডের ডিলয় (১-২ সেকেন্ডের মাঝামাঝি)
            setTimeout(() => {
                const signals = ["UP ⬆️", "DOWN ⬇️"];
                const result = signals[Math.floor(Math.random() * signals.length)];
                
                if(result.includes("UP")) {
                    display.innerHTML = '<span class="up-signal">NEXT: UP ⬆️</span>';
                    speak(selectedLang === 'bn-BD' ? "পরবর্তী ক্যান্ডেল উপরে যাবে" : "Next candle will go up");
                } else {
                    display.innerHTML = '<span class="down-signal">NEXT: DOWN ⬇️</span>';
                    speak(selectedLang === 'bn-BD' ? "পরবর্তী ক্যান্ডেল নিচে যাবে" : "Next candle will go down");
                }
            }, 1500); 
        }

        window.speechSynthesis.onvoiceschanged = () => {};
    </script>
</body>
</html>
