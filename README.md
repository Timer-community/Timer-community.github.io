# Timer-community.github.io
<html lang="ru">
<head>
    <title>Полный Python на русском</title>
    <style>
        body { font-family: sans-serif; margin: 20px; }
        .tabs { display: flex; margin-bottom: 10px; }
        .tab-btn { padding: 10px; cursor: pointer; background: #eee; border: 1px solid #ccc; }
        .tab-btn.active { background: white; border-bottom: none; }
        textarea, #output, .docs { width: 100%; margin: 10px 0; padding: 10px; font-family: monospace; }
        button { padding: 10px; margin: 5px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
        #output { background: #f0f0f0; white-space: pre-wrap; }
        .docs { background: #f9f9f9; border: 1px solid #ddd; }
        .syntax-example { background: #e7f3ff; padding: 8px; margin: 5px 0; }
    </style>
</head>
<body>
    <h1>Полный редактор Python на русском</h1>

    <div class="tabs">
        <button class="tab-btn active" onclick="showTab('editor')">Редактор кода</button>
        <button class="tab-btn" onclick="showTab('docs')">Документация</button>
    </div>

    <div id="editor">
        <textarea id="code" placeholder="вывести('Привет, мир!')
число = ввести_число('Введите число: ')
если число > 10:
    вывести('Число больше 10')
иначе:
    вывести('Число меньше или равно 10')"></textarea>
        <button onclick="run()">Запустить</button>
        <button onclick="clearCode()">Очистить</button>
        <pre id="output">Готов к выполнению...</pre>
    </div>

    <div id="docs" style="display:none">
        <div class="docs">
            <h2>Документация: Python на русском</h2>

            <div class="doc-section">
                <h3>Базовые команды</h3>
                <p><strong>вывести()</strong> — вывод текста (<code>print()</code>)</p>
                <div class="syntax-example">вывести('Текст для вывода')</div>
                <p><strong>ввести()</strong> — ввод строки (<code>input()</code>)</p>
                <p><strong>ввести_число()</strong> — ввод числа (<code>float(input())</code>)</p>
            </div>

            <div class="doc-section">
                <h3>Условные конструкции</h3>
                <p><strong>если</strong> — <code>if</code></p>
                <p><strong>иначе_если</strong> — <code>elif</code></p>
                <p><strong>иначе</strong> — <code>else</code></p>
                <div class="syntax-example">
если x > 10:<br>
&nbsp;&nbsp;&nbsp;&nbsp;вывести('x больше 10')<br>
иначе_если x > 5:<br>
&nbsp;&nbsp;&nbsp;&nbsp;вывести('x больше 5, но меньше или равно 10')<br>
иначе:<br>
&nbsp;&nbsp;&nbsp;&nbsp;вывести('x меньше или равно 5')
                </div>
            </div>

            <div class="doc-section">
                <h3>Циклы</h3>
                <p><strong>для</strong> — <code>for</code></p>
                <p><strong>в</strong> — <code>in</code></p>
                <p><strong>диапазоне</strong> — <code>range()</code></p>
                <p><strong>пока</strong> — <code>while</code></p>
                <div class="syntax-example">
для i в диапазоне(5):<br>
&nbsp;&nbsp;&nbsp;&nbsp;вывести(f'Итерация {i}')
                </div>
            </div>

            <div class="doc-section">
                <h3>Функции и управление</h3>
                <p><strong>определить</strong> — <code>def</code></p>
                <p><strong>вернуть</strong> — <code>return</code></p>
                <p><strong>прервать</strong> — <code>break</code></p>
                <p><strong>продолжить</strong> — <code>continue</code></p>
                <p><strong>остановить</strong> — остановка выполнения</p>
            </div>

            <div class="doc-section">
                <h3>Логические операторы</h3>
                <ul>
                    <li><strong>и</strong> — <code>and</code></li>
            <li><strong>или</strong> — <code>or</code></li>
            <li><strong>не</strong> — <code>not</code></li>
        </ul>
            </div>

            <div class="doc-section">
                <h3>Импорт модулей</h3>
                <p><strong>импортировать</strong> — <code>import</code></p>
                <p><strong>как</strong> — <code>as</code></p>
                <div class="syntax-example">импортировать математику как math</div>
            </div>

            <div class="doc-section">
                <h3>Важные замечания</h3>
                <ul>
            <li>Отступы — 4 пробела для блоков кода</li>
            <li>Переменные и имена функций — на английском языке</li>
            <li>Для математических операций: <code>импортировать математику как math</code></li>
            <li>Команда <code>остановить</code> немедленно прерывает выполнение</li>
        </ul>
            </div>
        </div>
    </div>

    <script>
        function showTab(tabName) {
            document.getElementById('editor').style.display = 'none';
            document.getElementById('docs').style.display = 'none';
            document.querySelector('.tab-btn.active').classList.remove('active');
            event.target.classList.add('active');
            document.getElementById(tabName).style.display = 'block';
        }

        function run() {
            const code = document.getElementById('code').value;
            const output = document.getElementById('output');

            // Полная среда с русскими аналогами всех основных команд Python
            const safeEnv = {
                вывести: (...args) => output.textContent += args.join(' ') + '\n',
                ввести: prompt,
                ввести_число: (promptText) => parseFloat(prompt(promptText)),
                прервать: () => { throw new Error('Цикл прерван'); },
                продолжить: () => { throw new Error('Итерация пропущена'); },
                остановить: () => { throw new Error('Выполнение остановлено'); }
            };

            output.textContent = 'Выполняется...\n';

            try {
                // Создаём функцию с русской средой
                const func = new Function(
                    Object.keys(safeEnv).filter(key => typeof safeEnv[key] === 'function'),
            `
                try {
                    ${code}
                } catch (e) {
                    if (!e.message.includes('Цикл') && !e.message.includes('остановлено')) {
                        output.textContent += 'Ошибка: ' + e.message;
                    }
                }`
                );

                // Выполняем с передачей функций
                func(...Object.values(safeEnv).filter(value => typeof value === 'function'));
            } catch (e) {
                if (!e.message.includes('Цикл') && !e.message.includes('остановлено')) {
                    output.textContent += 'Критическая ошибка: ' + e.message;
                }
            }
        }

        function clearCode() {
            document.getElementById('code').value = '';
            document.getElementById('output').textContent = 'Готов к выполнению...';
        }
    </script>
</body>
</html>
