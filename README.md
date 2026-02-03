<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Индекс Рафика</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        :root {
            --tg-theme-bg-color: #ffffff;
            --tg-theme-text-color: #000000;
            --tg-theme-hint-color: #999999;
            --tg-theme-link-color: #2481cc;
            --tg-theme-button-color: #2481cc;
            --tg-theme-button-text-color: #ffffff;
            --tg-theme-secondary-bg-color: #f0f0f0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }

        html, body {
            height: 100%;
            overflow-x: hidden;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--tg-theme-secondary-bg-color);
            color: var(--tg-theme-text-color);
            padding: 0;
            padding-bottom: calc(env(safe-area-inset-bottom, 0) + 80px);
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 20px 16px;
            padding-top: calc(env(safe-area-inset-top, 0) + 20px);
            text-align: center;
        }

        .header h1 {
            color: white;
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .header p {
            color: rgba(255, 255, 255, 0.85);
            font-size: 0.85rem;
        }

        .container {
            padding: 12px;
        }

        .card {
            background-color: var(--tg-theme-bg-color);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 12px;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
        }

        .employee-name input {
            width: 100%;
            padding: 14px 16px;
            font-size: 16px;
            border: none;
            border-radius: 10px;
            background-color: var(--tg-theme-secondary-bg-color);
            color: var(--tg-theme-text-color);
            outline: none;
            transition: box-shadow 0.2s;
        }

        .employee-name input:focus {
            box-shadow: 0 0 0 2px var(--tg-theme-button-color);
        }

        .employee-name input::placeholder {
            color: var(--tg-theme-hint-color);
        }

        .parameter {
            padding: 16px 0;
            border-bottom: 1px solid var(--tg-theme-secondary-bg-color);
        }

        .parameter:last-of-type {
            border-bottom: none;
            padding-bottom: 0;
        }

        .param-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 12px;
            gap: 8px;
        }

        .param-title {
            font-weight: 600;
            font-size: 0.95rem;
            color: var(--tg-theme-text-color);
            line-height: 1.3;
        }

        .param-weight {
            background: var(--tg-theme-button-color);
            color: var(--tg-theme-button-text-color);
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 0.7rem;
            font-weight: 600;
            white-space: nowrap;
            flex-shrink: 0;
        }

        .labels {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 0.7rem;
            font-weight: 500;
        }

        .label-left { color: #e53935; }
        .label-center { color: #43a047; }
        .label-right { color: #fb8c00; }

        .slider-container {
            position: relative;
            padding: 4px 0;
        }

        input[type="range"] {
            width: 100%;
            height: 6px;
            -webkit-appearance: none;
            appearance: none;
            background: linear-gradient(to right, #e53935, #43a047 50%, #fb8c00);
            border-radius: 3px;
            outline: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 28px;
            height: 28px;
            background: var(--tg-theme-bg-color);
            border: 3px solid var(--tg-theme-button-color);
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
            transition: transform 0.1s;
        }

        input[type="range"]::-webkit-slider-thumb:active {
            transform: scale(1.15);
        }

        input[type="range"]::-moz-range-thumb {
            width: 28px;
            height: 28px;
            background: var(--tg-theme-bg-color);
            border: 3px solid var(--tg-theme-button-color);
            border-radius: 50%;
            cursor: pointer;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
        }

        .value-display {
            text-align: center;
            margin-top: 8px;
            font-weight: 700;
            font-size: 1rem;
            color: var(--tg-theme-button-color);
        }

        .description {
            margin-top: 10px;
            padding: 10px 12px;
            background: var(--tg-theme-secondary-bg-color);
            border-radius: 8px;
            font-size: 0.8rem;
            color: var(--tg-theme-text-color);
            line-height: 1.4;
        }

        .result {
            display: none;
            background-color: var(--tg-theme-bg-color);
            border-radius: 12px;
            padding: 24px 16px;
            margin-bottom: 12px;
            text-align: center;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
        }

        .result.show {
            display: block;
            animation: slideUp 0.4s ease;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .emoji {
            font-size: 4rem;
            margin-bottom: 12px;
        }

        .result-name {
            font-size: 1.1rem;
            color: var(--tg-theme-hint-color);
            margin-bottom: 8px;
        }

        .result-value {
            font-size: 3rem;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .result-type {
            font-size: 1.3rem;
            font-weight: 600;
            margin-bottom: 16px;
        }

        .meter {
            width: 100%;
            height: 12px;
            background: linear-gradient(to right, #e53935, #43a047 40%, #43a047 60%, #fb8c00);
            border-radius: 6px;
            margin: 16px 0 8px;
            position: relative;
        }

        .meter-marker {
            position: absolute;
            top: -4px;
            width: 4px;
            height: 20px;
            background: var(--tg-theme-text-color);
            border-radius: 2px;
            transform: translateX(-50%);
            transition: left 0.5s ease;
            box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3);
        }

        .meter-labels {
            display: flex;
            justify-content: space-between;
            font-size: 0.65rem;
            color: var(--tg-theme-hint-color);
            margin-bottom: 16px;
        }

        .result-description {
            font-size: 0.85rem;
            color: var(--tg-theme-text-color);
            line-height: 1.5;
            text-align: left;
            padding: 12px;
            background: var(--tg-theme-secondary-bg-color);
            border-radius: 8px;
        }

        .share-btn {
            width: 100%;
            padding: 14px;
            margin-top: 16px;
            font-size: 0.95rem;
            font-weight: 600;
            color: var(--tg-theme-button-color);
            background: transparent;
            border: 2px solid var(--tg-theme-button-color);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .share-btn:active {
            background: var(--tg-theme-button-color);
            color: var(--tg-theme-button-text-color);
        }

        .reset-btn {
            width: 100%;
            padding: 12px;
            margin-top: 8px;
            font-size: 0.85rem;
            color: var(--tg-theme-hint-color);
            background: transparent;
            border: none;
            cursor: pointer;
        }

        /* Скрыть кнопку, если используется MainButton */
        .calculate-btn-container {
            display: none;
        }

        .calculate-btn-container.show {
            display: block;
            padding: 12px;
            padding-bottom: calc(env(safe-area-inset-bottom, 0) + 12px);
            background: var(--tg-theme-secondary-bg-color);
            position: sticky;
            bottom: 0;
        }

        .calculate-btn {
            width: 100%;
            padding: 16px;
            font-size: 1rem;
            font-weight: 600;
            color: var(--tg-theme-button-text-color);
            background: var(--tg-theme-button-color);
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: opacity 0.2s;
        }

        .calculate-btn:active {
            opacity: 0.8;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Индекс Рафика</h1>
        <p>Баланс между смузихлебом и фанатиком</p>
    </div>

    <div class="container">
        <div class="card">
            <div class="employee-name">
                <input type="text" id="employeeName" placeholder="Имя сотрудника">
            </div>
        </div>

        <div class="card" id="parametersCard">
            <div class="parameter">
                <div class="param-header">
                    <span class="param-title">1. Ворк-Лайф Баланс</span>
                    <span class="param-weight">Вес: 3</span>
                </div>
                <div class="labels">
                    <span class="label-left">Смузихлеб</span>
                    <span class="label-center">Рафик</span>
                    <span class="label-right">Фанатик</span>
                </div>
                <div class="slider-container">
                    <input type="range" id="param1" min="-1" max="1" step="0.2" value="0" data-weight="3">
                    <div class="value-display" id="value1">0.0</div>
                </div>
                <div class="description" id="desc1"></div>
            </div>

            <div class="parameter">
                <div class="param-header">
                    <span class="param-title">2. Объём ответственности</span>
                    <span class="param-weight">Вес: 2</span>
                </div>
                <div class="labels">
                    <span class="label-left">Смузихлеб</span>
                    <span class="label-center">Рафик</span>
                    <span class="label-right">Фанатик</span>
                </div>
                <div class="slider-container">
                    <input type="range" id="param2" min="-1" max="1" step="0.2" value="0" data-weight="2">
                    <div class="value-display" id="value2">0.0</div>
                </div>
                <div class="description" id="desc2"></div>
            </div>

            <div class="parameter">
                <div class="param-header">
                    <span class="param-title">3. Отношение к командировкам</span>
                    <span class="param-weight">Вес: 1</span>
                </div>
                <div class="labels">
                    <span class="label-left">Смузихлеб</span>
                    <span class="label-center">Рафик</span>
                    <span class="label-right">Фанатик</span>
                </div>
                <div class="slider-container">
                    <input type="range" id="param3" min="-1" max="1" step="0.2" value="0" data-weight="1">
                    <div class="value-display" id="value3">0.0</div>
                </div>
                <div class="description" id="desc3"></div>
            </div>

            <div class="parameter">
                <div class="param-header">
                    <span class="param-title">4. Сверхурочная работа</span>
                    <span class="param-weight">Вес: 2</span>
                </div>
                <div class="labels">
                    <span class="label-left">Смузихлеб</span>
                    <span class="label-center">Рафик</span>
                    <span class="label-right">Фанатик</span>
                </div>
                <div class="slider-container">
                    <input type="range" id="param4" min="-1" max="1" step="0.2" value="0" data-weight="2">
                    <div class="value-display" id="value4">0.0</div>
                </div>
                <div class="description" id="desc4"></div>
            </div>

            <div class="parameter">
                <div class="param-header">
                    <span class="param-title">5. Работа по процессам</span>
                    <span class="param-weight">Вес: 2</span>
                </div>
                <div class="labels">
                    <span class="label-left">Смузихлеб</span>
                    <span class="label-center">Рафик</span>
                    <span class="label-right">Фанатик</span>
                </div>
                <div class="slider-container">
                    <input type="range" id="param5" min="-1" max="1" step="0.2" value="0" data-weight="2">
                    <div class="value-display" id="value5">0.0</div>
                </div>
                <div class="description" id="desc5"></div>
            </div>

            <div class="parameter">
                <div class="param-header">
                    <span class="param-title">6. Ответственность за других</span>
                    <span class="param-weight">Вес: 1</span>
                </div>
                <div class="labels">
                    <span class="label-left">Смузихлеб</span>
                    <span class="label-center">Рафик</span>
                    <span class="label-right">Фанатик</span>
                </div>
                <div class="slider-container">
                    <input type="range" id="param6" min="-1" max="1" step="0.2" value="0" data-weight="1">
                    <div class="value-display" id="value6">0.0</div>
                </div>
                <div class="description" id="desc6"></div>
            </div>
        </div>

        <div class="result" id="result">
            <div class="emoji" id="resultEmoji"></div>
            <div class="result-name" id="resultName"></div>
            <div class="result-value" id="resultValue"></div>
            <div class="result-type" id="resultType"></div>
            <div class="meter">
                <div class="meter-marker" id="meterMarker"></div>
            </div>
            <div class="meter-labels">
                <span>0.0</span>
                <span>0.5</span>
                <span>1.0</span>
            </div>
            <p class="result-description" id="resultDescription"></p>
            <button class="share-btn" onclick="shareResult()">Поделиться результатом</button>
            <button class="reset-btn" onclick="resetForm()">Рассчитать заново</button>
        </div>
    </div>

    <div class="calculate-btn-container" id="calcBtnContainer">
        <button class="calculate-btn" onclick="calculate()">Рассчитать Индекс Рафика</button>
    </div>

    <script>
        // Telegram WebApp init
        const tg = window.Telegram?.WebApp;
        let isInTelegram = false;

        if (tg) {
            tg.ready();
            tg.expand();
            isInTelegram = true;

            // Настройка MainButton
            tg.MainButton.setText('Рассчитать Индекс Рафика');
            tg.MainButton.show();
            tg.MainButton.onClick(calculate);

            // Применяем тему
            document.documentElement.style.setProperty('--tg-theme-bg-color', tg.themeParams.bg_color || '#ffffff');
            document.documentElement.style.setProperty('--tg-theme-text-color', tg.themeParams.text_color || '#000000');
            document.documentElement.style.setProperty('--tg-theme-hint-color', tg.themeParams.hint_color || '#999999');
            document.documentElement.style.setProperty('--tg-theme-link-color', tg.themeParams.link_color || '#2481cc');
            document.documentElement.style.setProperty('--tg-theme-button-color', tg.themeParams.button_color || '#2481cc');
            document.documentElement.style.setProperty('--tg-theme-button-text-color', tg.themeParams.button_text_color || '#ffffff');
            document.documentElement.style.setProperty('--tg-theme-secondary-bg-color', tg.themeParams.secondary_bg_color || '#f0f0f0');
        } else {
            // Показать fallback кнопку если не в Telegram
            document.getElementById('calcBtnContainer').classList.add('show');
        }

        // Haptic feedback
        function haptic(type = 'light') {
            if (tg?.HapticFeedback) {
                if (type === 'light') tg.HapticFeedback.impactOccurred('light');
                else if (type === 'medium') tg.HapticFeedback.impactOccurred('medium');
                else if (type === 'success') tg.HapticFeedback.notificationOccurred('success');
                else if (type === 'error') tg.HapticFeedback.notificationOccurred('error');
            }
        }

        const descriptions = {
            1: {
                "-1.0": { type: "Ультра-Смузихлеб", text: "Работает 4-5 часов максимум. Имитация деятельности — основной навык.", color: "#c62828" },
                "-0.8": { type: "Яркий Смузихлеб", text: "Регулярно недорабатывает. Часто 'болеет' в понедельник и пятницу.", color: "#d32f2f" },
                "-0.6": { type: "Смузихлеб", text: "Систематически недорабатывает. Ищет способы сократить рабочий день.", color: "#e53935" },
                "-0.4": { type: "Склонность к Смузихлебу", text: "Иногда уходит пораньше. Не готов задержаться даже на 10 минут.", color: "#f44336" },
                "-0.2": { type: "Лёгкий Смузихлеб", text: "В целом работает нормально, но оптимизирует время в свою пользу.", color: "#ef5350" },
                "0.0": { type: "Идеальный Рафик", text: "Чётко работает 8 часов. Границы соблюдает, но гибок.", color: "#43a047" },
                "0.2": { type: "Рафик+", text: "Иногда задерживается, если задача интересная. Компенсирует отгулами.", color: "#66bb6a" },
                "0.4": { type: "Склонность к Фанатизму", text: "Периодически работает сверхурочно. Забывает про личные дела.", color: "#fb8c00" },
                "0.6": { type: "Фанатик", text: "Регулярно задерживается. Выходные тратит на рабочие задачи.", color: "#f57c00" },
                "0.8": { type: "Яркий Фанатик", text: "Работает по 10-12 часов. Личная жизнь страдает.", color: "#ef6c00" },
                "1.0": { type: "Ультра-Фанатик", text: "Работает ночами и выходными. 'Работа — это жизнь'.", color: "#e65100" }
            },
            2: {
                "-1.0": { type: "Ультра-Смузихлеб", text: "Саботирует любую ответственность. Перекладывает всё на других.", color: "#c62828" },
                "-0.8": { type: "Яркий Смузихлеб", text: "'Это не моя работа' — любимая фраза.", color: "#d32f2f" },
                "-0.6": { type: "Смузихлеб", text: "Активно уходит от ответственности. 'Это не моя зона'.", color: "#e53935" },
                "-0.4": { type: "Склонность к Смузихлебу", text: "Берёт минимум ответственности. Предпочитает простые задачи.", color: "#f44336" },
                "-0.2": { type: "Лёгкий Смузихлеб", text: "Немного сужает зону ответственности.", color: "#ef5350" },
                "0.0": { type: "Идеальный Рафик", text: "Отвечает за свой функционал. Глубоко знает свою зону.", color: "#43a047" },
                "0.2": { type: "Рафик+", text: "Знает свою зону и соседние. Готов помочь.", color: "#66bb6a" },
                "0.4": { type: "Склонность к Фанатизму", text: "Берёт задачи из смежных зон. Переживает за чужие модули.", color: "#fb8c00" },
                "0.6": { type: "Фанатик", text: "Отвечает за смежные зоны и часть команды.", color: "#f57c00" },
                "0.8": { type: "Яркий Фанатик", text: "Чувствует ответственность за весь проект.", color: "#ef6c00" },
                "1.0": { type: "Ультра-Фанатик", text: "Несёт на себе весь продукт. Не может делегировать.", color: "#e65100" }
            },
            3: {
                "-1.0": { type: "Ультра-Смузихлеб", text: "Готов уволиться, лишь бы не ехать. Справка уже готова.", color: "#c62828" },
                "-0.8": { type: "Яркий Смузихлеб", text: "Категорически против. Семья, здоровье, кот заболел...", color: "#d32f2f" },
                "-0.6": { type: "Смузихлеб", text: "Командировки — зло. Ищет причину отказаться.", color: "#e53935" },
                "-0.4": { type: "Склонность к Смузихлебу", text: "Едет только под давлением руководства.", color: "#f44336" },
                "-0.2": { type: "Лёгкий Смузихлеб", text: "Не любит, но иногда соглашается. Минимизирует срок.", color: "#ef5350" },
                "0.0": { type: "Идеальный Рафик", text: "Готов ездить 1-2 раза в год, когда нужно для дела.", color: "#43a047" },
                "0.2": { type: "Рафик+", text: "Нормально относится. Видит возможность решить задачи на месте.", color: "#66bb6a" },
                "0.4": { type: "Склонность к Фанатизму", text: "Часто вызывается. На месте всё эффективнее.", color: "#fb8c00" },
                "0.6": { type: "Фанатик", text: "Регулярно в разъездах. Дома бывает редко.", color: "#f57c00" },
                "0.8": { type: "Яркий Фанатик", text: "Почти живёт в командировках. Семья — по видеосвязи.", color: "#ef6c00" },
                "1.0": { type: "Ультра-Фанатик", text: "Вечно в поле. Где сложнее — туда и едет.", color: "#e65100" }
            },
            4: {
                "-1.0": { type: "Ультра-Смузихлеб", text: "Уходит в 18:00, даже если сервер горит.", color: "#c62828" },
                "-0.8": { type: "Яркий Смузихлеб", text: "5 минут сверхурочно — нарушение прав.", color: "#d32f2f" },
                "-0.6": { type: "Смузихлеб", text: "Сверхурочные — никогда. 'Мои права'.", color: "#e53935" },
                "-0.4": { type: "Склонность к Смузихлебу", text: "Только если угрожают увольнением.", color: "#f44336" },
                "-0.2": { type: "Лёгкий Смузихлеб", text: "В аварии может задержаться. С недовольством.", color: "#ef5350" },
                "0.0": { type: "Идеальный Рафик", text: "Только 'по кайфу' или краткая необходимость. Не система.", color: "#43a047" },
                "0.2": { type: "Рафик+", text: "Иногда задерживается из интереса. Следит за балансом.", color: "#66bb6a" },
                "0.4": { type: "Склонность к Фанатизму", text: "Регулярно перерабатывает 'потому что интересно'.", color: "#fb8c00" },
                "0.6": { type: "Фанатик", text: "Сверхурочные — норма. 'Горю задачей'.", color: "#f57c00" },
                "0.8": { type: "Яркий Фанатик", text: "Переработки — основной источник кайфа.", color: "#ef6c00" },
                "1.0": { type: "Ультра-Фанатик", text: "Работает всегда. Живёт в офисе.", color: "#e65100" }
            },
            5: {
                "-1.0": { type: "Ультра-Смузихлеб", text: "Создаёт хаос намеренно. В мутной воде легче.", color: "#c62828" },
                "-0.8": { type: "Яркий Смузихлеб", text: "Любит хаос. Саботирует порядок.", color: "#d32f2f" },
                "-0.6": { type: "Смузихлеб", text: "Делает вид, что не понимает процессов.", color: "#e53935" },
                "-0.4": { type: "Склонность к Смузихлебу", text: "Не любит процессы — они обнажают неэффективность.", color: "#f44336" },
                "-0.2": { type: "Лёгкий Смузихлеб", text: "Иногда игнорирует процессы, когда удобно.", color: "#ef5350" },
                "0.0": { type: "Идеальный Рафик", text: "Работает в отлаженном процессе. Авралы — редкость.", color: "#43a047" },
                "0.2": { type: "Рафик+", text: "Ценит процессы и помогает их улучшать.", color: "#66bb6a" },
                "0.4": { type: "Склонность к Фанатизму", text: "Создаёт избыточные процессы.", color: "#fb8c00" },
                "0.6": { type: "Фанатик", text: "Создаёт хаос активностью. Вечный аврал.", color: "#f57c00" },
                "0.8": { type: "Яркий Фанатик", text: "Режим тушения пожаров 24/7.", color: "#ef6c00" },
                "1.0": { type: "Ультра-Фанатик", text: "Живёт в аврале. Спокойная работа = скука.", color: "#e65100" }
            },
            6: {
                "-1.0": { type: "Ультра-Смузихлеб", text: "Игнорирует всех. Токсичен для команды.", color: "#c62828" },
                "-0.8": { type: "Яркий Смузихлеб", text: "Не отвечает на вопросы. 'Разбирайтесь сами'.", color: "#d32f2f" },
                "-0.6": { type: "Смузихлеб", text: "Игнорирует запросы. 'Моё — моё, ваше — ваше'.", color: "#e53935" },
                "-0.4": { type: "Склонность к Смузихлебу", text: "Неохотно помогает. Отвечает односложно.", color: "#f44336" },
                "-0.2": { type: "Лёгкий Смузихлеб", text: "Помогает, если настойчиво попросят.", color: "#ef5350" },
                "0.0": { type: "Идеальный Рафик", text: "Профессионально отвечает за код и качество.", color: "#43a047" },
                "0.2": { type: "Рафик+", text: "Охотно помогает, делится знаниями.", color: "#66bb6a" },
                "0.4": { type: "Склонность к Фанатизму", text: "Помогает в ущерб своим задачам.", color: "#fb8c00" },
                "0.6": { type: "Фанатик", text: "Неформально опекает команду. Ментор.", color: "#f57c00" },
                "0.8": { type: "Яркий Фанатик", text: "Тратит всё время на помощь другим.", color: "#ef6c00" },
                "1.0": { type: "Ультра-Фанатик", text: "Погружён в проблемы других. Свои откладывает.", color: "#e65100" }
            }
        };

        const results = [
            { max: 0.05, type: "Абсолютный Смузихлеб", emoji: "🧘", color: "#b71c1c", text: "Критический уровень отстранённости. Присутствует только физически. Нужен серьёзный разговор." },
            { max: 0.15, type: "Ультра-Смузихлеб", emoji: "🥤", color: "#c62828", text: "Крайне низкая вовлечённость. Негативно влияет на команду." },
            { max: 0.25, type: "Яркий Смузихлеб", emoji: "🍹", color: "#d32f2f", text: "Деструктивная пассивность. Делает минимум. Нужно выяснить причины." },
            { max: 0.35, type: "Смузихлеб", emoji: "🥱", color: "#e53935", text: "Заметная недовольечённость. Инициатива отсутствует." },
            { max: 0.42, type: "Склонность к Смузихлебу", emoji: "😐", color: "#fb8c00", text: "Лёгкий дисбаланс в сторону пассивности. Нужна мягкая коррекция." },
            { max: 0.48, type: "Почти Рафик", emoji: "🙂", color: "#8bc34a", text: "Близко к балансу. Хороший сотрудник, иногда перестраховывается." },
            { max: 0.52, type: "Идеальный Рафик", emoji: "⚖️", color: "#43a047", text: "Золотая середина! Здоровый баланс. Эталон." },
            { max: 0.58, type: "Рафик+", emoji: "💪", color: "#8bc34a", text: "Отличный сотрудник с хорошей мотивацией. Следить за балансом." },
            { max: 0.65, type: "Склонность к Фанатизму", emoji: "🔥", color: "#fb8c00", text: "Лёгкий дисбаланс в сторону работы. Следить за усталостью." },
            { max: 0.75, type: "Фанатик", emoji: "🚀", color: "#f57c00", text: "Заметный трудоголизм. Риск выгорания в течение года." },
            { max: 0.85, type: "Яркий Фанатик", emoji: "💥", color: "#ef6c00", text: "Работает на износ. Высокий риск выгорания." },
            { max: 0.95, type: "Ультра-Фанатик", emoji: "🌋", color: "#e65100", text: "Критический трудоголизм. Выгорание неизбежно." },
            { max: 1.01, type: "Абсолютный Фанатик", emoji: "☠️", color: "#bf360c", text: "Экстремальный уровень. Нужна профессиональная помощь." }
        ];

        let lastResult = null;

        function getDescription(paramNum, value) {
            const val = parseFloat(value).toFixed(1);
            return descriptions[paramNum][val] || descriptions[paramNum]["0.0"];
        }

        function updateSlider(i) {
            const slider = document.getElementById(`param${i}`);
            const valueDisplay = document.getElementById(`value${i}`);
            const descDisplay = document.getElementById(`desc${i}`);

            const val = parseFloat(slider.value);
            valueDisplay.textContent = val.toFixed(1);

            const desc = getDescription(i, val);
            descDisplay.innerHTML = `<strong style="color: ${desc.color}">${desc.type}:</strong> ${desc.text}`;
        }

        // Init sliders
        for (let i = 1; i <= 6; i++) {
            const slider = document.getElementById(`param${i}`);

            slider.addEventListener('input', function() {
                haptic('light');
                updateSlider(i);
            });

            slider.addEventListener('change', function() {
                haptic('medium');
            });

            updateSlider(i);
        }

        function calculate() {
            haptic('medium');

            const name = document.getElementById('employeeName').value || 'Сотрудник';
            let weightedSum = 0;
            const totalWeight = 22;

            for (let i = 1; i <= 6; i++) {
                const slider = document.getElementById(`param${i}`);
                const value = parseFloat(slider.value);
                const weight = parseInt(slider.dataset.weight);
                weightedSum += value * weight;
            }

            const ri = 0.5 + (weightedSum / totalWeight);
            const riClamped = Math.max(0, Math.min(1, ri));

            const result = results.find(r => riClamped < r.max) || results[results.length - 1];
            lastResult = { name, ri: riClamped, ...result };

            document.getElementById('resultEmoji').textContent = result.emoji;
            document.getElementById('resultName').textContent = name;
            document.getElementById('resultValue').textContent = riClamped.toFixed(2);
            document.getElementById('resultValue').style.color = result.color;
            document.getElementById('resultType').textContent = result.type;
            document.getElementById('resultType').style.color = result.color;
            document.getElementById('resultDescription').textContent = result.text;
            document.getElementById('meterMarker').style.left = `${riClamped * 100}%`;

            const resultDiv = document.getElementById('result');
            resultDiv.classList.add('show');

            // Скрываем параметры и показываем результат
            document.getElementById('parametersCard').style.display = 'none';

            if (tg) {
                tg.MainButton.hide();
            }

            haptic('success');

            setTimeout(() => {
                resultDiv.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }, 100);
        }

        function shareResult() {
            haptic('medium');

            if (!lastResult) return;

            const text = `${lastResult.emoji} ${lastResult.name}\nИндекс Рафика: ${lastResult.ri.toFixed(2)}\n${lastResult.type}\n\n${lastResult.text}`;

            if (tg) {
                // Отправить в чат
                tg.sendData(JSON.stringify(lastResult));
            } else {
                // Fallback: копировать в буфер
                navigator.clipboard?.writeText(text).then(() => {
                    alert('Скопировано в буфер обмена!');
                });
            }
        }

        function resetForm() {
            haptic('medium');

            // Сбрасываем слайдеры
            for (let i = 1; i <= 6; i++) {
                document.getElementById(`param${i}`).value = 0;
                updateSlider(i);
            }
            document.getElementById('employeeName').value = '';

            // Скрываем результат, показываем параметры
            document.getElementById('result').classList.remove('show');
            document.getElementById('parametersCard').style.display = 'block';

            if (tg) {
                tg.MainButton.show();
            }

            window.scrollTo({ top: 0, behavior: 'smooth' });
        }
    </script>
</body>
</html>
