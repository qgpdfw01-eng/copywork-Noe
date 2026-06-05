```
index.html

```
```

<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تطبيق المحاكاة</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Amiri+Quran&display=swap" rel="stylesheet">

    <style>
        * { box-sizing: border-box; }

        body {
            font-family: 'Amiri Quran', serif;
            background-color: #f4f6f9;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }

        .container {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.04);
            width: 100%;
            max-width: 750px;
            text-align: center;
        }

        .editor-wrapper {
            position: relative;
            width: 100%;
            min-height: 200px;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            background: #ffffff;
            padding: 20px;
            overflow: hidden;
        }

        .layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            padding: 20px; 
            font-family: 'Amiri Quran', serif;
            font-size: 32px;
            line-height: 2.5;
            text-align: right;
            white-space: pre-wrap;
            margin: 0;
            border: none;
        }

        .background-text { color: #cbd5e1; z-index: 1; pointer-events: none; }
        .overlay-text { z-index: 2; pointer-events: none; }
        
        .input-textarea {
            z-index: 3;
            background: transparent;
            color: transparent;
            caret-color: #3b82f6;
            outline: none;
            resize: none;
            display: block;
        }

        .correct-char { color: #000000 !important; }
        .wrong-char { color: #94a3b8 !important; }

        .bottom-bar { display: flex; justify-content: space-between; align-items: center; margin-top: 15px; }
        .stats { font-size: 16px; color: #94a3b8; font-weight: 600; }
        .toggle-btn { font-family: 'Amiri Quran', serif; font-size: 16px; font-weight: 600; background: none; border: none; color: #64748b; cursor: pointer; }
    </style>
</head>
<body>

<div class="container">
    <div class="editor-wrapper">
        <div class="layer background-text" id="bgText">بَراءَةٌ مِنَ اللَّهِ وَرَسولِهِ إِلَى الَّذينَ عاهَدتُم مِنَ المُشرِكينَ</div>
        <div class="layer overlay-text" id="overlayText"></div>
        <textarea class="layer input-textarea" id="userInput" spellcheck="false" autocomplete="off"></textarea>
    </div>

    <div class="bottom-bar">
        <div class="stats" id="wordCount">عدد الكلمات: 0</div>
        <button id="toggleBtn" class="toggle-btn">تغيير النص ⚙️</button>
    </div>
</div>

<script>
    const bgText = document.getElementById('bgText');
    const overlayText = document.getElementById('overlayText');
    const userInput = document.getElementById('userInput');
    const wordCount = document.getElementById('wordCount');

    const diacritics = ['\u064B', '\u064C', '\u064D', '\u064E', '\u064F', '\u0650', '\u0651', '\u0652', '\u0670'];

    userInput.addEventListener('input', () => {
        const inputText = userInput.value;
        const originalText = bgText.innerText;
        const originalWords = originalText.split(" ");
        const inputWords = inputText.split(" ");
        let htmlContent = "";

        for (let w = 0; w < inputWords.length; w++) {
            const currentInWord = inputWords[w];
            const currentOrigWord = originalWords[w] || "";
            let inIdx = 0;
            let origIdx = 0;

            while (inIdx < currentInWord.length) {
                const inChar = currentInWord[inIdx];
                const origChar = currentOrigWord[origIdx] || "";
                if (inChar === origChar || (origChar === "" && inChar !== "")) {
                    htmlContent += `<span class="correct-char">${inChar}</span>`;
                    inIdx++; origIdx++;
                } else if (diacritics.includes(inChar)) {
                    htmlContent += `<span class="wrong-char">${inChar}</span>`;
                    inIdx++;
                } else if (diacritics.includes(origChar)) {
                    origIdx++;
                } else {
                    htmlContent += `<span class="wrong-char">${inChar}</span>`;
                    inIdx++; origIdx++;
                }
            }
            if (w < inputWords.length - 1) htmlContent += " ";
        }
        overlayText.innerHTML = htmlContent;
        wordCount.innerText = `عدد الكلمات: ${inputText.trim() === "" ? 0 : inputText.trim().split(/\s+/).length}`;
    });
</script>
</body>
</html>

```
