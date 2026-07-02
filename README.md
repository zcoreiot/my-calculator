# 🧮 Scientific Calculator (เว็บเครื่องคิดเลขวิทยาศาสตร์)

เว็บแอปพลิเคชันเครื่องคิดเลขวิทยาศาสตร์แบบตอบสนอง (Responsive) พัฒนาด้วย HTML, CSS และ JavaScript บริสุทธิ์ (Vanilla JS) สามารถใช้งานคำนวณพื้นฐานและฟังก์ชันทางคณิตศาสตร์ระดับสูงได้

---

## ✨ ฟีเจอร์เด่น (Features)
*   **ฟังก์ชันวิทยาศาสตร์พื้นฐาน:** รองรับการคำนวณ `sin`, `cos`, `tan`, `log`, รากที่สอง (`√`) และการยกกำลัง (`xʸ`)
*   **รองรับแป้นพิมพ์ภายนอก (External Keypad Support):** สามารถพิมพ์ตัวเลข เครื่องหมายบวกลบคูณหาร กด `Enter` เพื่อคำนวณ หรือกด `Backspace` / `Escape` จากคีย์บอร์ดคอมพิวเตอร์ของคุณได้โดยตรง
*   **การออกแบบที่ทันสมัย:** อินเตอร์เฟสธีมมืด (Dark Mode) สไตล์มินิมอล สบายตา และรองรับการแสดงผลบนมือถือ (Responsive Design)

---

## 🛠️ เทคโนโลยีที่ใช้ (Technologies Used)
*   **HTML5** - โครงสร้างปุ่มและหน้าจอแสดงผล
*   **CSS3** - การจัดหน้าด้วย Grid และสไตล์ธีมมืด
*   **JavaScript (ES6)** - ระบบคำนวณคณิตศาสตร์และการดักจับ Event แป้นพิมพ์

---

## 📦 ซอร์สโค้ดโปรเจกต์ (Source Code)

โค้ดทั้งหมดถูกรวมไว้ในไฟล์เดียวเพื่อความสะดวกในการจัดการ คุณสามารถสร้างไฟล์ชื่อ `index.html` แล้วคัดลอกโค้ดด้านล่างนี้ไปรันได้เลย:

```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scientific Calculator</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .calculator {
            background-color: #202124;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            width: 100%;
            max-width: 400px;
        }

        .display {
            background-color: #303134;
            color: #ffffff;
            font-size: 2.5rem;
            text-align: right;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            min-height: 70px;
            word-wrap: break-word;
            word-break: break-all;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            padding: 18px;
            font-size: 1.25rem;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            background-color: #3c4043;
            color: #e8eaed;
            transition: background-color 0.1s;
        }

        button:hover {
            background-color: #4f5357;
        }

        button.operator {
            background-color: #dadce0;
            color: #202124;
        }

        button.operator:hover {
            background-color: #e8eaed;
        }

        button.equals {
            background-color: #8ab4f8;
            color: #202124;
            grid-column: span 2;
        }

        button.equals:hover {
            background-color: #aecbfa;
        }

        button.clear {
            background-color: #f28b82;
            color: #202124;
        }

        button.clear:hover {
            background-color: #f6aea9;
        }
    </style>
</head>
<body>

<div class="calculator">
    <div class="display" id="display">0</div>
    <div class="buttons">
        <!-- แถววิทยาศาสตร์ -->
        <button onclick="appendFunction('Math.sin')">sin</button>
        <button onclick="appendFunction('Math.cos')">cos</button>
        <button onclick="appendFunction('Math.tan')">tan</button>
        <button class="clear" onclick="clearDisplay()">C</button>

        <button onclick="appendFunction('Math.sqrt')">√</button>
        <button onclick="appendValue('**')">xʸ</button>
        <button onclick="appendFunction('Math.log10')">log</button>
        <button class="operator" onclick="appendValue('/')">÷</button>

        <!-- ตัวเลขและตัวดำเนินการหลัก -->
        <button onclick="appendValue('7')">7</button>
        <button onclick="appendValue('8')">8</button>
        <button onclick="appendValue('9')">9</button>
        <button class="operator" onclick="appendValue('*')">×</button>

        <button onclick="appendValue('4')">4</button>
        <button onclick="appendValue('5')">5</button>
        <button onclick="appendValue('6')">6</button>
        <button class="operator" onclick="appendValue('-')">-</button>

        <button onclick="appendValue('1')">1</button>
        <button onclick="appendValue('2')">2</button>
        <button onclick="appendValue('3')">3</button>
        <button class="operator" onclick="appendValue('+')">+</button>

        <button onclick="appendValue('0')">0</button>
        <button onclick="appendValue('.')">.</button>
        <button class="equals" onclick="calculate()">=</button>
    </div>
</div>

<script>
    const display = document.getElementById('display');
    let currentInput = '';

    function appendValue(value) {
        if (display.innerText === '0' || display.innerText === 'Error') {
            currentInput = value;
        } else {
            currentInput += value;
        }
        updateDisplay();
    }

    function appendFunction(func) {
        if (display.innerText === '0' || display.innerText === 'Error') {
            currentInput = func + '(';
        } else {
            currentInput += func + '(';
        }
        updateDisplay();
    }

    function clearDisplay() {
        currentInput = '';
        display.innerText = '0';
    }

    function updateDisplay() {
        let formatted = currentInput
            .replace(/Math\.sin\(/g, 'sin(')
            .replace(/Math\.cos\(/g, 'cos(')
            .replace(/Math\.tan\(/g, 'tan(')
            .replace(/Math\.sqrt\(/g, '√(')
            .replace(/Math\.log10\(/g, 'log(')
            .replace(/\*\*/g, '^')
            .replace(/\*/g, '×')
            .replace(/\//g, '÷');
        
        display.innerText = formatted || '0';
    }

    function calculate() {
        try {
            let expression = currentInput;
            let openBrackets = (expression.match(/\(/g) || []).length;
            let closeBrackets = (expression.match(/\)/g) || []).length;
            while (openBrackets > closeBrackets) {
                expression += ')';
                closeBrackets++;
            }

            let result = eval(expression);
            if (result === undefined || isNaN(result)) {
                display.innerText = 'Error';
            } else {
                display.innerText = Number(result.toFixed(8));
            }
            currentInput = display.innerText;
        } catch (error) {
            display.innerText = 'Error';
            currentInput = '';
        }
    }

    // ระบบแป้นพิมพ์ภายนอก
    document.addEventListener('keydown', (event) => {
        const key = event.key;
        if ((key >= '0' && key <= '9') || key === '.') {
            appendValue(key);
        } else if (key === '+' || key === '-' || key === '*' || key === '/') {
            appendValue(key);
        } else if (key === 'Enter' || key === '=') {
            event.preventDefault();
            calculate();
        } else if (key === 'Escape' || key === 'c' || key === 'C') {
            clearDisplay();
        } else if (key === 'Backspace') {
            currentInput = currentInput.slice(0, -1);
            updateDisplay();
        }
    });
</script>

</body>
</html>