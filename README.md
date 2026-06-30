<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>中姸院數據驗證系統</title>
    <style>
        body {
            background-color: #0a0a0a;
            color: #00ff66;
            font-family: 'Courier New', Courier, monospace, "Microsoft JhengHei";
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }
        .terminal {
            background-color: #111;
            border: 2px solid #00ff66;
            box-shadow: 0 0 20px rgba(0, 255, 102, 0.2);
            width: 90%;
            max-width: 600px;
            padding: 30px;
            box-sizing: border-box;
            border-radius: 8px;
            position: relative;
        }
        h2 {
            text-align: center;
            color: #00ff66;
            margin-top: 0;
            border-bottom: 1px solid #00ff66;
            padding-bottom: 15px;
            font-size: 1.4rem;
            text-shadow: 0 0 5px rgba(0, 255, 102, 0.5);
        }
        .input-group {
            margin: 30px 0;
            text-align: center;
        }
        label {
            display: block;
            margin-bottom: 15px;
            font-size: 1.1rem;
        }
        input[type="text"] {
            background-color: #000;
            border: 1px solid #00ff66;
            color: #00ff66;
            padding: 12px 20px;
            font-size: 1.2rem;
            width: 80%;
            text-align: center;
            outline: none;
            border-radius: 4px;
            box-sizing: border-box;
        }
        input[type="text"]:focus {
            box-shadow: 0 0 10px rgba(0, 255, 102, 0.5);
        }
        button {
            background-color: #00ff66;
            color: #000;
            border: none;
            padding: 12px 30px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 20px;
            border-radius: 4px;
            transition: all 0.2s;
        }
        button:hover {
            background-color: #00ff99;
            box-shadow: 0 0 15px rgba(0, 255, 102, 0.6);
        }
        #error-msg {
            color: #ff3333;
            text-align: center;
            margin-top: 15px;
            min-height: 20px;
            font-weight: bold;
        }
        .loader-container {
            display: none;
            margin: 30px 0;
        }
        .progress-bar {
            width: 100%;
            background-color: #222;
            border: 1px solid #00ff66;
            height: 25px;
            position: relative;
            border-radius: 4px;
            overflow: hidden;
        }
        .progress-fill {
            width: 0%;
            height: 100%;
            background-color: #00ff66;
            transition: width 0.1s linear;
        }
        .progress-text {
            position: absolute;
            width: 100%;
            text-align: center;
            top: 3px;
            color: #000;
            font-weight: bold;
        }
        .log-output {
            background-color: #000;
            border: 1px solid #222;
            height: 120px;
            padding: 10px;
            overflow-y: auto;
            font-size: 0.9rem;
            color: #00ff66;
            text-align: left;
            border-radius: 4px;
        }
        .result-screen {
            display: none;
            text-align: center;
            animation: flash 0.5s ease-in-out;
        }
        .result-box {
            background-color: #000;
            border: 2px dashed #ff3333;
            padding: 25px;
            margin: 20px 0;
            color: #ff3333;
            font-size: 1.3rem;
            font-weight: bold;
            line-height: 1.6;
            box-shadow: 0 0 15px rgba(255, 51, 51, 0.3);
            border-radius: 4px;
        }
        @keyframes flash {
            0% { background-color: rgba(255, 51, 51, 0.4); }
            100% { background-color: transparent; }
        }
    </style>
</head>
<body>

<div class="terminal">
    <div id="setup-screen">
        <h2>CORE DATA VERIFICATION SYSTEM</h2>
        <div class="input-group">
            <label for="username">> 請輸入受測姓名：</label>
            <input type="text" id="username" autocomplete="off" placeholder="輸入姓名...">
            <br>
            <button onclick="startAnalysis()">啟動邏輯運算</button>
            <div id="error-msg"></div>
        </div>
    </div>

    <div id="loading-screen" class="loader-container">
        <h2>系統分析中...</h2>
        <div class="progress-bar">
            <div id="fill" class="progress-fill"></div>
            <div id="percent" class="progress-text">0%</div>
        </div>
        <div id="log" class="log-output"></div>
    </div>

    <div id="result-screen" class="result-screen">
        <h2 style="color: #ff3333; border-color: #ff3333; text-shadow: 0 0 5px rgba(255, 51, 51, 0.5);">[CRITICAL] 驗證運算完成</h2>
        <div class="result-box">
            [SYSTEM INFO] <br>
            經國家級編譯器邏輯運算確認：<br>
            <span style="font-size: 1.8rem; text-decoration: underline;">陳昱凱是gay。</span>
        </div>
        <button onclick="resetSystem()" style="background-color: #ff3333; color: #000;">重新鎖定系統</button>
    </div>
</div>

<script>
    const logs = [
        "[FETCHING] 正在串接低層生物識別資料庫...",
        "[PARSING] 娘化邏輯架構解析中...",
        "[CONNECTING] 正在向 C++ 編譯器請求底層哥布林反映...",
        "[ANALYZING] 正在分析其量子性傾向波動狀態...",
        "[RUNNING] 執行 油水 模組...",
        "[SUCCESS] 女裝校正成功，排除所有正常人狀態..."
    ];

    function startAnalysis() {
        const nameInput = document.getElementById('username').value.trim();
        const errorMsg = document.getElementById('error-msg');
        
        if (!nameInput) {
            errorMsg.innerText = "[ERROR] 目標欄位不可為空。";
            return;
        }
        if (nameInput !== "陳昱凱") {
            errorMsg.innerText = "[WARN] 權限不足，該人員不屬於受測範圍。";
            return;
        }

        errorMsg.innerText = "";
        document.getElementById('setup-screen').style.display = 'none';
        document.getElementById('loading-screen').style.display = 'block';

        let progress = 0;
        const fill = document.getElementById('fill');
        const percent = document.getElementById('percent');
        const logBox = document.getElementById('log');

        let logIndex = 0;
        const logInterval = setInterval(() => {
            if (logIndex < logs.length) {
                logBox.innerHTML += logs[logIndex] + "<br>";
                logBox.scrollTop = logBox.scrollHeight;
                logIndex++;
            }
        }, 600);

        const progressInterval = setInterval(() => {
            progress += 1;
            fill.style.width = progress + '%';
            percent.innerText = progress + '%';

            if (progress >= 100) {
                clearInterval(progressInterval);
                clearInterval(logInterval);
                setTimeout(() => {
                    document.getElementById('loading-screen').style.display = 'none';
                    document.getElementById('result-screen').style.display = 'block';
                }, 500);
            }
        }, 40);
    }

    function resetSystem() {
        document.getElementById('username').value = "";
        document.getElementById('log').innerHTML = "";
        document.getElementById('result-screen').style.display = 'none';
        document.getElementById('setup-screen').style.display = 'block';
    }
</script>

</body>
</html>
