<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>BBK — Clean Launcher</title>
<style>
  :root {
    --bg-gradient-1: #0f1419;
    --bg-gradient-2: #1a2332;
    --bg-gradient-3: #0a0e14;
    --text: #ffffff;
    --text-muted: rgba(255,255,255,0.6);
    --card-bg: rgba(255,255,255,0.05);
    --card-border: rgba(255,255,255,0.1);
    --accent: #4fc3f7;
    --hero-bg-1: #1e3a5f;
    --hero-bg-2: #2d5a87;
  }

  [data-theme="light"] {
    --bg-gradient-1: #f5f7fa;
    --bg-gradient-2: #e8edf3;
    --bg-gradient-3: #dfe5ec;
    --text: #1a1a1a;
    --text-muted: rgba(0,0,0,0.6);
    --card-bg: rgba(0,0,0,0.04);
    --card-border: rgba(0,0,0,0.1);
    --accent: #0073a8;
    --hero-bg-1: #4a90c2;
    --hero-bg-2: #6ab0e0;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Roboto, sans-serif; }
  
  body {
    width: 1920px; height: 1080px;
    background: linear-gradient(135deg, var(--bg-gradient-1) 0%, var(--bg-gradient-2) 50%, var(--bg-gradient-3) 100%);
    color: var(--text); overflow: hidden;
    user-select: none;
    transition: background 0.4s, color 0.4s;
  }

  .screen { display: none; width: 100%; height: 100%; }
  .screen.active { display: block; }

  /* ===== ЭКРАН ПРИВЕТСТВИЯ ===== */
  #welcome {
    display: none;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 30px;
  }
  #welcome.active { display: flex; }
  
  .welcome-title {
    font-size: 64px; font-weight: 300;
    text-align: center;
  }
  .welcome-title b { font-weight: 600; color: var(--accent); }
  
  .welcome-input {
    width: 700px; padding: 24px 36px;
    font-size: 36px;
    background: var(--card-bg);
    border: 2px solid var(--card-border);
    border-radius: 20px;
    color: var(--text);
    text-align: center;
    outline: none;
    transition: border 0.3s;
  }
  .welcome-input:focus { border-color: var(--accent); }
  
  .color-picker { display: flex; gap: 20px; margin-top: 10px; }
  .color-swatch {
    width: 70px; height: 70px;
    border-radius: 50%;
    cursor: pointer;
    border: 4px solid transparent;
    transition: transform 0.2s, border 0.2s;
  }
  .color-swatch:hover { transform: scale(1.1); }
  .color-swatch.selected, .color-swatch.focused {
    border-color: var(--text);
    transform: scale(1.15);
  }
  
  .welcome-btn {
    padding: 18px 50px;
    font-size: 26px; font-weight: 600;
    background: var(--accent);
    color: #0f1419;
    border: none; border-radius: 50px;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .welcome-btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 30px rgba(79,195,247,0.4);
  }
  .welcome-btn:focus, .welcome-btn.focused {
    outline: 4px solid var(--text);
    outline-offset: 4px;
  }

  /* ===== ГЛАВНЫЙ ЭКРАН ===== */
  .header {
    display: flex; justify-content: space-between; align-items: center;
    padding: 30px 80px 15px;
  }
  .greeting { font-size: 44px; font-weight: 300; }
  .greeting b { font-weight: 600; }
  .header-right { display: flex; gap: 40px; align-items: center; font-size: 26px; }
  .clock { font-weight: 300; font-size: 52px; letter-spacing: 2px; }
  .weather { display: flex; align-items: center; gap: 12px; }
  .weather-icon { font-size: 36px; }

  .hero {
    margin: 20px 80px;
    height: 280px;
    border-radius: 24px;
    background: linear-gradient(90deg, rgba(20,25,35,0.95) 0%, rgba(20,25,35,0.4) 60%, transparent 100%),
                linear-gradient(135deg, var(--hero-bg-1) 0%, var(--hero-bg-2) 100%);
    padding: 40px 60px;
    display: flex; flex-direction: column; justify-content: space-between;
    position: relative; overflow: hidden;
    cursor: pointer;
    transition: transform 0.3s, box-shadow 0.3s;
  }
  .hero:focus, .hero.focused {
    outline: 4px solid var(--accent);
    outline-offset: 6px;
    transform: scale(1.01);
    box-shadow: 0 20px 60px rgba(79,195,247,0.3);
  }
  .hero-title { font-size: 48px; font-weight: 600; }
  .hero-sub { font-size: 24px; opacity: 0.8; max-width: 700px; }
  .hero-play {
    display: inline-flex; align-items: center; gap: 14px;
    background: var(--accent); color: #0f1419;
    padding: 14px 32px; border-radius: 50px;
    font-size: 22px; font-weight: 600;
    width: fit-content;
  }
  .no-ads {
    position: absolute; top: 20px; right: 20px;
    background: rgba(76, 175, 80, 0.9);
    padding: 8px 16px; border-radius: 20px;
    font-size: 16px; font-weight: 600;
    color: #fff;
  }

  .section { margin: 15px 80px 0; }
  .section-title {
    font-size: 24px; font-weight: 500; margin-bottom: 12px;
    display: flex; align-items: center; gap: 14px;
  }
  .section-title::before {
    content: ''; width: 6px; height: 20px; background: var(--accent); border-radius: 3px;
  }

  .row { display: flex; gap: 16px; flex-wrap: wrap; }
  .tile {
    width: 150px; height: 150px;
    border-radius: 18px;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    gap: 8px;
    font-size: 18px; font-weight: 500;
    cursor: pointer;
    transition: all 0.25s ease;
    position: relative;
    color: #fff;
    text-shadow: 0 2px 4px rgba(0,0,0,0.3);
  }
  .tile .num {
    position: absolute; top: 6px; left: 8px;
    font-size: 12px; opacity: 0.7;
    background: rgba(0,0,0,0.3);
    padding: 2px 6px; border-radius: 6px;
  }
  .tile .icon { font-size: 48px; }
  .tile:focus, .tile.focused {
    outline: 4px solid var(--accent);
    outline-offset: 6px;
    transform: scale(1.08);
    z-index: 10;
  }

  .tile.yt    { background: linear-gradient(135deg, #ff0000, #cc0000); }
  .tile.rt    { background: linear-gradient(135deg, #1a1a1a, #333); }
  .tile.kp    { background: linear-gradient(135deg, #ff5500, #ff8800); }
  .tile.okko  { background: linear-gradient(135deg, #6a0dad, #9b30ff); }
  .tile.wink  { background: linear-gradient(135deg, #e60000, #b30000); }
  .tile.more  { background: linear-gradient(135deg, #ff6b9d, #c44569); }
  .tile.nx    { background: linear-gradient(135deg, #b80000, #e50914); }
  .tile.prime { background: linear-gradient(135deg, #00a8e1, #0073a8); }
  .tile.ttv   { background: linear-gradient(135deg, #6441a5, #9146ff); }
  .tile.vk    { background: linear-gradient(135deg, #0077ff, #0055cc); }
  .tile.dzen  { background: linear-gradient(135deg, #000, #333); }
  .tile.ivi   { background: linear-gradient(135deg, #ff3366, #cc0044); }
  .tile.mg    { background: linear-gradient(135deg, #ffcc00, #ff9900); color: #000; }
  .tile.start { background: linear-gradient(135deg, #000, #222); }
  .tile.prem  { background: linear-gradient(135deg, #1a1a1a, #444); }
  .tile.spot  { background: linear-gradient(135deg, #1db954, #158f3f); }
  .tile.yam   { background: linear-gradient(135deg, #ffcc00, #ff9900); color: #000; }
  .tile.apple { background: linear-gradient(135deg, #fc3c44, #d12d2d); }
  .tile.tidal { background: linear-gradient(135deg, #000, #222); }
  .tile.sc    { background: linear-gradient(135deg, #ff5500, #ff8800); }
  .tile.brws  { background: linear-gradient(135deg, #4285f4, #1a73e8); }
  .tile.set   { background: linear-gradient(135deg, #3a3f47, #1e2228); }
  .tile.reset { background: linear-gradient(135deg, #666, #444); }

  .footer {
    position: absolute; bottom: 20px; left: 80px; right: 80px;
    display: flex; justify-content: space-between; align-items: center;
    font-size: 16px; color: var(--text-muted);
  }
  .hints { display: flex; gap: 20px; flex-wrap: wrap; }
  .hint { display: flex; align-items: center; gap: 8px; }
  .key {
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    padding: 4px 10px; border-radius: 6px;
    font-weight: 600; font-size: 13px;
    color: var(--text);
  }

  /* ===== НАСТРОЙКИ ===== */
  #settings { padding: 50px 80px; }
  .settings-title { font-size: 48px; font-weight: 300; margin-bottom: 30px; }
  .settings-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
    max-width: 1400px;
  }
  .setting-card {
    background: var(--card-bg);
    border: 2px solid var(--card-border);
    border-radius: 20px;
    padding: 24px 32px;
    cursor: pointer;
    transition: all 0.3s;
  }
  .setting-card:focus, .setting-card.focused {
    outline: 4px solid var(--accent);
    outline-offset: 4px;
    transform: scale(1.02);
  }
  .setting-card h3 { font-size: 26px; margin-bottom: 10px; }
  .setting-card p { font-size: 18px; color: var(--text-muted); }
  .theme-switch { display: flex; gap: 16px; margin-top: 16px; }
  .theme-btn {
    flex: 1; padding: 16px;
    border-radius: 14px;
    border: 2px solid var(--card-border);
    background: var(--card-bg);
    color: var(--text);
    font-size: 20px; font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
  }
  .theme-btn.active {
    border-color: var(--accent);
    background: var(--accent);
    color: #0f1419;
  }
  .back-btn {
    position: absolute; top: 50px; right: 80px;
    padding: 12px 26px;
    background: var(--card-bg);
    border: 2px solid var(--card-border);
    border-radius: 14px;
    color: var(--text);
    font-size: 20px;
    cursor: pointer;
  }
  .back-btn:focus { outline: 4px solid var(--accent); outline-offset: 4px; }

  /* ===== ВИРТУАЛЬНАЯ КЛАВИАТУРА ===== */
  #keyboard {
    position: fixed;
    bottom: 0; left: 0; right: 0;
    background: rgba(15, 20, 25, 0.97);
    backdrop-filter: blur(20px);
    border-top: 2px solid var(--accent);
    padding: 20px 40px 30px;
    display: none;
    z-index: 1000;
    box-shadow: 0 -10px 40px rgba(0,0,0,0.5);
  }
  #keyboard.active { display: block; }

  .kb-row {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin-bottom: 8px;
  }
  .kb-key {
    min-width: 70px; height: 64px;
    background: rgba(255,255,255,0.08);
    border: 2px solid rgba(255,255,255,0.15);
    border-radius: 12px;
    color: #fff;
    font-size: 24px; font-weight: 500;
    cursor: pointer;
    transition: all 0.15s;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .kb-key:hover { background: rgba(255,255,255,0.15); }
  .kb-key:focus, .kb-key.focused {
    background: var(--accent);
    color: #0f1419;
    border-color: var(--accent);
    transform: scale(1.08);
    z-index: 10;
  }
  .kb-key.wide { min-width: 140px; }
  .kb-key.xwide { min-width: 220px; }
  .kb-key.action { background: rgba(79,195,247,0.2); border-color: var(--accent); }

  /* ===== ИНДИКАТОР ГЕЙМПАДА ===== */
  #gamepadIndicator {
    position: fixed;
    top: 20px; right: 20px;
    background: rgba(76, 175, 80, 0.9);
    color: #fff;
    padding: 8px 16px;
    border-radius: 20px;
    font-size: 14px;
    display: none;
    z-index: 999;
  }
  #gamepadIndicator.active { display: block; }

  /* ===== TOAST ===== */
  #toast {
    position: fixed;
    bottom: 100px; left: 50%;
    transform: translateX(-50%) translateY(100px);
    background: rgba(0,0,0,0.9);
    color: #fff;
    padding: 16px 32px;
    border-radius: 14px;
    font-size: 20px;
    opacity: 0;
    transition: all 0.3s;
    z-index: 2000;
    pointer-events: none;
  }
  #toast.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }
</style>
</head>
<body>

<!-- ===== ЭКРАН ПРИВЕТСТВИЯ ===== -->
<div id="welcome" class="screen">
  <div class="welcome-title">Привет! <b>Как тебя зовут?</b></div>
  <input type="text" class="welcome-input" id="nameInput" placeholder="Нажми OK для ввода" maxlength="20" readonly>
  
  <div class="welcome-title" style="font-size: 32px;">Выбери <b>цвет имени</b></div>
  <div class="color-picker" id="colorPicker">
    <div class="color-swatch selected" style="background:#4fc3f7" data-color="#4fc3f7" tabindex="0"></div>
    <div class="color-swatch" style="background:#ff5500" data-color="#ff5500" tabindex="0"></div>
    <div class="color-swatch" style="background:#1db954" data-color="#1db954" tabindex="0"></div>
    <div class="color-swatch" style="background:#ff0000" data-color="#ff0000" tabindex="0"></div>
    <div class="color-swatch" style="background:#ffcc00" data-color="#ffcc00" tabindex="0"></div>
    <div class="color-swatch" style="background:#9b30ff" data-color="#9b30ff" tabindex="0"></div>
    <div class="color-swatch" style="background:#ff6b9d" data-color="#ff6b9d" tabindex="0"></div>
    <div class="color-swatch" style="background:#ffffff" data-color="#ffffff" tabindex="0"></div>
  </div>
  
  <button class="welcome-btn" id="saveBtn" tabindex="0">Продолжить →</button>
</div>

<!-- ===== ГЛАВНЫЙ ЭКРАН ===== -->
<div id="launcher" class="screen">
  <div class="header">
    <div class="greeting">Добрый <span id="timeOfDay">день</span>, <b id="userName">Гость</b></div>
    <div class="header-right">
      <div class="weather">
        <span class="weather-icon" id="weatherIcon">☁️</span>
        <span id="weatherText">Загрузка...</span>
      </div>
      <div class="clock" id="clock">--:--</div>
    </div>
  </div>

  <div class="hero" tabindex="0">
    <div class="no-ads">✓ Без рекламы</div>
    <div class="hero-title">Чистый интерфейс</div>
    <div class="hero-sub">Никакой слежки. Только твои приложения и твой контент.</div>
    <div class="hero-play">▶ Продолжить просмотр</div>
  </div>

  <div class="section">
    <div class="section-title">Видео</div>
    <div class="row" id="videoApps"></div>
  </div>

  <div class="section">
    <div class="section-title">Кинотеатры</div>
    <div class="row" id="cinemaApps"></div>
  </div>

  <div class="section">
    <div class="section-title">Музыка</div>
    <div class="row" id="musicApps"></div>
  </div>

  <div class="section">
    <div class="section-title">Инструменты</div>
    <div class="row" id="toolsApps"></div>
  </div>

  <div class="footer">
    <div class="hints">
      <div class="hint"><span class="key">🎮 D-Pad</span> навигация</div>
      <div class="hint"><span class="key">🎮 A</span> открыть</div>
      <div class="hint"><span class="key">🎮 B</span> назад</div>
      <div class="hint"><span class="key">1-9</span> быстрый выбор</div>
      <div class="hint"><span class="key">F11</span> полный экран</div>
    </div>
    <div id="deviceInfo">BBK • Clean Launcher v3.0</div>
  </div>
</div>

<!-- ===== НАСТРОЙКИ ===== -->
<div id="settings" class="screen">
  <button class="back-btn" id="backBtn" tabindex="0">← Назад</button>
  <div class="settings-title">Настройки</div>
  <div class="settings-grid">
    <div class="setting-card" tabindex="0">
      <h3>🎨 Тема оформления</h3>
      <p>Выбери светлую или тёмную тему</p>
      <div class="theme-switch">
        <button class="theme-btn" data-theme="light" tabindex="0">☀ Светлая</button>
        <button class="theme-btn active" data-theme="dark" tabindex="0">🌙 Тёмная</button>
      </div>
    </div>
    <div class="setting-card" tabindex="0">
      <h3>👤 Профиль</h3>
      <p>Имя: <b id="settingsName">—</b></p>
      <p style="margin-top:10px;">Цвет имени можно изменить через сброс профиля</p>
    </div>
    <div class="setting-card" tabindex="0">
      <h3>🌍 Погода</h3>
      <p id="settingsWeather">Загрузка...</p>
    </div>
    <div class="setting-card" tabindex="0">
      <h3>🎮 Устройства ввода</h3>
      <p>Клавиатура: ✓</p>
      <p id="gamepadStatus">Геймпад: не подключён</p>
      <p>Пульт: ✓ (если поддерживается)</p>
    </div>
  </div>
</div>

<!-- ===== ВИРТУАЛЬНАЯ КЛАВИАТУРА ===== -->
<div id="keyboard"></div>

<!-- ===== ИНДИКАТОР ГЕЙМПАДА ===== -->
<div id="gamepadIndicator">🎮 Геймпад подключён</div>

<!-- ===== TOAST ===== -->
<div id="toast"></div>

<script>
  // ===== ДАННЫЕ ПРИЛОЖЕНИЙ =====
  const APPS = {
    video: [
      { name: 'YouTube',   icon: '▶', url: 'https://youtube.com',       cls: 'yt' },
      { name: 'Rutube',    icon: 'R', url: 'https://rutube.ru',         cls: 'rt' },
      { name: 'VK Видео',  icon: 'VK',url: 'https://vk.com/video',      cls: 'vk' },
      { name: 'Twitch',    icon: '📺',url: 'https://twitch.tv',         cls: 'ttv' },
      { name: 'Дзен',      icon: 'Д', url: 'https://dzen.ru',           cls: 'dzen' }
    ],
    cinema: [
      { name: 'Кинопоиск', icon: '🎬', url: 'https://kinopoisk.ru',     cls: 'kp' },
      { name: 'Okko',      icon: 'O',  url: 'https://okko.tv',          cls: 'okko' },
      { name: 'Wink',      icon: 'W',  url: 'https://wink.ru',          cls: 'wink' },
      { name: 'More.tv',   icon: 'M',  url: 'https://more.tv',          cls: 'more' },
      { name: 'Netflix',   icon: 'N',  url: 'https://netflix.com',      cls: 'nx' },
      { name: 'Prime',     icon: 'P',  url: 'https://primevideo.com',   cls: 'prime' },
      { name: 'IVI',       icon: 'I',  url: 'https://ivi.ru',           cls: 'ivi' },
      { name: 'Megogo',    icon: 'M',  url: 'https://megogo.net',       cls: 'mg' },
      { name: 'START',     icon: 'S',  url: 'https://start.ru',         cls: 'start' },
      { name: 'Premier',   icon: 'P',  url: 'https://premier.one',      cls: 'prem' }
    ],
    music: [
      { name: 'Spotify',      icon: '🎵', url: 'https://open.spotify.com',  cls: 'spot' },
      { name: 'Яндекс Музыка',icon: 'Я',  url: 'https://music.yandex.ru',   cls: 'yam' },
      { name: 'Apple Music',  icon: '🎼', url: 'https://music.apple.com',   cls: 'apple' },
      { name: 'Tidal',        icon: 'T',  url: 'https://tidal.com',         cls: 'tidal' },
      { name: 'SoundCloud',   icon: '☁',  url: 'https://soundcloud.com',    cls: 'sc' }
    ],
    tools: [
      { name: 'Браузер',  icon: '🌐', action: 'browser',  cls: 'brws' },
      { name: 'Настройки',icon: '⚙',  action: 'settings', cls: 'set' },
      { name: 'Сброс',    icon: '↺',  action: 'reset',    cls: 'reset' }
    ]
  };

  // Рендер плиток с номерами
  function renderTiles() {
    let globalNum = 1;
    Object.entries(APPS).forEach(([key, apps]) => {
      const container = document.getElementById(key + 'Apps');
      container.innerHTML = '';
      apps.forEach(app => {
        const tile = document.createElement('div');
        tile.className = `tile ${app.cls}`;
        tile.tabIndex = 0;
        tile.dataset.num = globalNum;
        if (app.url) tile.dataset.url = app.url;
        if (app.action) tile.dataset.action = app.action;
        tile.innerHTML = `<span class="num">${globalNum}</span><div class="icon">${app.icon}</div>${app.name}`;
        container.appendChild(tile);
        globalNum++;
      });
    });
  }
  renderTiles();

  // ===== ХРАНИЛИЩЕ =====
  const STORAGE_KEY = 'bbk_launcher_user';
  let userData = JSON.parse(localStorage.getItem(STORAGE_KEY)) || null;

  // ===== ЭКРАНЫ =====
  const screens = {
    welcome: document.getElementById('welcome'),
    launcher: document.getElementById('launcher'),
    settings: document.getElementById('settings')
  };
  let currentScreen = 'welcome';

  function showScreen(name) {
    Object.values(screens).forEach(s => s.classList.remove('active'));
    screens[name].classList.add('active');
    currentScreen = name;
    hideKeyboard();
    resetFocus();
  }

  // ===== TOAST =====
  function showToast(msg) {
    const t = document.getElementById('toast');
    t.textContent = msg;
    t.classList.add('show');
    clearTimeout(t._timer);
    t._timer = setTimeout(() => t.classList.remove('show'), 2000);
  }

  // ===== ВИРТУАЛЬНАЯ КЛАВИАТУРА =====
  const RU_UPPER = 'АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ'.split('');
  const RU_LOWER = 'абвгдеёжзийклмнопрстуфхцчшщъыьэюя'.split('');
  let kbLayout = RU_UPPER;
  let kbActive = false;
  let kbFocusIdx = 0;

  function buildKeyboard() {
    const kb = document.getElementById('keyboard');
    kb.innerHTML = '';
    
    // 3 ряда по 11 букв
    for (let r = 0; r < 3; r++) {
      const row = document.createElement('div');
      row.className = 'kb-row';
      const letters = kbLayout.slice(r * 11, r * 11 + 11);
      letters.forEach(ch => {
        const key = document.createElement('button');
        key.className = 'kb-key';
        key.tabIndex = 0;
        key.textContent = ch;
        key.dataset.char = ch;
        row.appendChild(key);
      });
      kb.appendChild(row);
    }
    
    // Ряд управления
    const ctrlRow = document.createElement('div');
    ctrlRow.className = 'kb-row';
    
    const shiftBtn = document.createElement('button');
    shiftBtn.className = 'kb-key wide action';
    shiftBtn.tabIndex = 0;
    shiftBtn.textContent = kbLayout === RU_UPPER ? 'АБВ ⇧' : 'абв ⇩';
    shiftBtn.dataset.action = 'shift';
    ctrlRow.appendChild(shiftBtn);
    
    const spaceBtn = document.createElement('button');
    spaceBtn.className = 'kb-key xwide';
    spaceBtn.tabIndex = 0;
    spaceBtn.textContent = '␣ Пробел';
    spaceBtn.dataset.action = 'space';
    ctrlRow.appendChild(spaceBtn);
    
    const delBtn = document.createElement('button');
    delBtn.className = 'kb-key wide action';
    delBtn.tabIndex = 0;
    delBtn.textContent = '⌫ Удалить';
    delBtn.dataset.action = 'delete';
    ctrlRow.appendChild(delBtn);
    
    const doneBtn = document.createElement('button');
    doneBtn.className = 'kb-key wide action';
    doneBtn.tabIndex = 0;
    doneBtn.textContent = '✓ Готово';
    doneBtn.dataset.action = 'done';
    ctrlRow.appendChild(doneBtn);
    
    kb.appendChild(ctrlRow);
  }
  buildKeyboard();

  function showKeyboard() {
    if (kbActive) return;
    kbActive = true;
    document.getElementById('keyboard').classList.add('active');
    kbFocusIdx = 0;
    updateKbFocus();
  }

  function hideKeyboard() {
    kbActive = false;
    document.getElementById('keyboard').classList.remove('active');
  }

  function getKbKeys() {
    return Array.from(document.querySelectorAll('#keyboard .kb-key'));
  }

  function updateKbFocus() {
    const keys = getKbKeys();
    keys.forEach(k => k.classList.remove('focused'));
    if (keys[kbFocusIdx]) {
      keys[kbFocusIdx].classList.add('focused');
      keys[kbFocusIdx].focus();
    }
  }

  function kbPress(key) {
    const input = document.getElementById('nameInput');
    const action = key.dataset.action;
    const ch = key.dataset.char;
    
    if (ch) {
      if (input.value.length < 20) {
        input.value += ch;
      }
    } else if (action === 'space') {
      if (input.value.length < 20) input.value += ' ';
    } else if (action === 'delete') {
      input.value = input.value.slice(0, -1);
    } else if (action === 'shift') {
      kbLayout = kbLayout === RU_UPPER ? RU_LOWER : RU_UPPER;
      buildKeyboard();
      kbFocusIdx = 0;
      updateKbFocus();
    } else if (action === 'done') {
      hideKeyboard();
      resetFocus();
    }
  }

  function kbNavigate(dir) {
    const keys = getKbKeys();
    if (!keys.length) return;
    
    // Считаем сетку: первые 3 ряда по 11, последний ряд 4 кнопки
    const ROWS = [11, 11, 11, 4];
    let curRow = 0, curCol = 0, acc = 0;
    for (let r = 0; r < ROWS.length; r++) {
      if (kbFocusIdx < acc + ROWS[r]) {
        curRow = r;
        curCol = kbFocusIdx - acc;
        break;
      }
      acc += ROWS[r];
    }
    
    let newRow = curRow, newCol = curCol;
    if (dir === 'left')  newCol = Math.max(0, curCol - 1);
    if (dir === 'right') newCol = Math.min(ROWS[curRow] - 1, curCol + 1);
    if (dir === 'up')    newRow = Math.max(0, curRow - 1);
    if (dir === 'down')  newRow = Math.min(ROWS.length - 1, curRow + 1);
    
    let newIdx = 0;
    for (let r = 0; r < newRow; r++) newIdx += ROWS[r];
    newIdx += Math.min(newCol, ROWS[newRow] - 1);
    
    kbFocusIdx = newIdx;
    updateKbFocus();
  }

  // ===== ПРИВЕТСТВИЕ =====
  const nameInput = document.getElementById('nameInput');
  const colorPicker = document.getElementById('colorPicker');
  const saveBtn = document.getElementById('saveBtn');
  let selectedColor = '#4fc3f7';

  colorPicker.addEventListener('click', (e) => {
    if (e.target.classList.contains('color-swatch')) {
      document.querySelectorAll('.color-swatch').forEach(s => s.classList.remove('selected'));
      e.target.classList.add('selected');
      selectedColor = e.target.dataset.color;
    }
  });

  function saveProfile() {
    const name = nameInput.value.trim();
    if (!name) {
      showToast('Введи имя!');
      return;
    }
    userData = { name, color: selectedColor };
    localStorage.setItem(STORAGE_KEY, JSON.stringify(userData));
    applyUserData();
    showScreen('launcher');
    showToast('Добро пожаловать, ' + name + '!');
  }

  saveBtn.addEventListener('click', saveProfile);

  function applyUserData() {
    if (!userData) return;
    const userNameEl = document.getElementById('userName');
    userNameEl.textContent = userData.name;
    userNameEl.style.color = userData.color;
    document.getElementById('settingsName').textContent = userData.name;
  }

  // ===== ЧАСЫ =====
  function updateClock() {
    const d = new Date();
    const h = d.getHours();
    const m = String(d.getMinutes()).padStart(2,'0');
    const hStr = String(h).padStart(2,'0');
    document.getElementById('clock').textContent = `${hStr}:${m}`;
    
    let tod = 'ночи';
    if (h >= 5 && h < 12) tod = 'утро';
    else if (h >= 12 && h < 18) tod = 'день';
    else if (h >= 18 && h < 23) tod = 'вечер';
    document.getElementById('timeOfDay').textContent = tod;
  }
  updateClock();
  setInterval(updateClock, 1000);

  // ===== ПОГОДА =====
  async function loadWeather() {
    try {
      let lat = 55.75, lon = 37.62, city = 'Москва';
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(async (pos) => {
          await fetchWeather(pos.coords.latitude, pos.coords.longitude, 'Ваш город');
        }, async () => {
          await fetchWeather(lat, lon, city);
        }, { timeout: 3000 });
      } else {
        await fetchWeather(lat, lon, city);
      }
    } catch (e) {
      document.getElementById('weatherText').textContent = 'Нет данных';
    }
  }

  async function fetchWeather(lat, lon, defaultCity) {
    try {
      const url = `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m,weather_code,wind_speed_10m&timezone=auto`;
      const res = await fetch(url);
      const data = await res.json();
      const temp = Math.round(data.current.temperature_2m);
      const code = data.current.weather_code;
      const wind = Math.round(data.current.wind_speed_10m);
      
      const weatherMap = {
        0: { icon: '☀️', desc: 'Ясно' }, 1: { icon: '🌤', desc: 'Малооблачно' },
        2: { icon: '⛅', desc: 'Переменно' }, 3: { icon: '☁️', desc: 'Облачно' },
        45: { icon: '🌫', desc: 'Туман' }, 51: { icon: '🌦', desc: 'Морось' },
        61: { icon: '🌧', desc: 'Дождь' }, 63: { icon: '🌧', desc: 'Дождь' },
        71: { icon: '🌨', desc: 'Снег' }, 80: { icon: '🌦', desc: 'Ливень' },
        95: { icon: '⛈', desc: 'Гроза' }
      };
      const w = weatherMap[code] || { icon: '🌡', desc: 'Погода' };
      
      document.getElementById('weatherIcon').textContent = w.icon;
      document.getElementById('weatherText').textContent = `${temp}° ${defaultCity}`;
      document.getElementById('settingsWeather').innerHTML = 
        `${w.icon} ${w.desc}, <b>${temp}°C</b><br>Ветер: ${wind} м/с`;
    } catch (e) {
      document.getElementById('weatherText').textContent = 'Нет данных';
    }
  }
  loadWeather();

  // ===== КЛИКИ ПО ПЛИТКАМ =====
  document.addEventListener('click', (e) => {
    const tile = e.target.closest('.tile');
    if (tile) {
      const url = tile.dataset.url;
      const action = tile.dataset.action;
      if (url) window.open(url, '_blank');
      else if (action === 'browser') window.open('https://google.com', '_blank');
      else if (action === 'settings') showScreen('settings');
      else if (action === 'reset') {
        if (confirm('Сбросить профиль?')) {
          localStorage.removeItem(STORAGE_KEY);
          localStorage.removeItem('bbk_theme');
          location.reload();
        }
      }
    }
    
    // Клавиатура
    if (e.target.classList.contains('kb-key')) {
      kbPress(e.target);
    }
  });

  // Фокус на input → показать клавиатуру
  nameInput.addEventListener('focus', showKeyboard);
  nameInput.addEventListener('click', showKeyboard);

  // ===== НАСТРОЙКИ =====
  document.getElementById('backBtn').addEventListener('click', () => showScreen('launcher'));

  document.querySelectorAll('.theme-btn').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.theme-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const theme = btn.dataset.theme;
      document.documentElement.setAttribute('data-theme', theme);
      localStorage.setItem('bbk_theme', theme);
      showToast('Тема изменена');
    });
  });

  const savedTheme = localStorage.getItem('bbk_theme');
  if (savedTheme) {
    document.documentElement.setAttribute('data-theme', savedTheme);
    document.querySelectorAll('.theme-btn').forEach(b => {
      b.classList.toggle('active', b.dataset.theme === savedTheme);
    });
  }

  // ===== НАВИГАЦИЯ =====
  let focusable = [];
  let focusIdx = 0;

  function resetFocus() {
    focusable = Array.from(document.querySelectorAll(`#${currentScreen} [tabindex="0"]`));
    focusIdx = 0;
    document.querySelectorAll('.focused').forEach(el => el.classList.remove('focused'));
    if (focusable.length) {
      focusable[0].classList.add('focused');
      focusable[0].focus();
    }
  }

  function moveFocus(dir) {
    if (!focusable.length) return;
    focusable[focusIdx].classList.remove('focused');
    
    const cur = focusable[focusIdx];
    const rect = cur.getBoundingClientRect();
    const cx = rect.left + rect.width / 2;
    const cy = rect.top + rect.height / 2;
    
    let bestIdx = -1;
    let bestScore = Infinity;
    
    focusable.forEach((el, i) => {
      if (i === focusIdx) return;
      const r = el.getBoundingClientRect();
      const ex = r.left + r.width / 2;
      const ey = r.top + r.height / 2;
      const dx = ex - cx, dy = ey - cy;
      
      let valid = false;
      let score = 0;
      if (dir === 'left'  && dx < -10) { valid = true; score = Math.abs(dx) + Math.abs(dy) * 2; }
      if (dir === 'right' && dx > 10)  { valid = true; score = Math.abs(dx) + Math.abs(dy) * 2; }
      if (dir === 'up'    && dy < -10) { valid = true; score = Math.abs(dy) + Math.abs(dx) * 2; }
      if (dir === 'down'  && dy > 10)  { valid = true; score = Math.abs(dy) + Math.abs(dx) * 2; }
      
      if (valid && score < bestScore) {
        bestScore = score;
        bestIdx = i;
      }
    });
    
    if (bestIdx >= 0) focusIdx = bestIdx;
    focusable[focusIdx].classList.add('focused');
    focusable[focusIdx].focus();
  }

  // ===== ОБРАБОТКА КЛАВИАТУРЫ =====
  document.addEventListener('keydown', (e) => {
    // Глобальные горячие клавиши
    if (e.key === 'F11') {
      e.preventDefault();
      if (!document.fullscreenElement) document.documentElement.requestFullscreen();
      else document.exitFullscreen();
      return;
    }

    // Если активна клавиатура — рулим ей
    if (kbActive) {
      if (['ArrowLeft','ArrowRight','ArrowUp','ArrowDown'].includes(e.key)) {
        e.preventDefault();
        kbNavigate(e.key.replace('Arrow','').toLowerCase());
        return;
      }
      if (e.key === 'Enter') {
        e.preventDefault();
        const keys = getKbKeys();
        if (keys[kbFocusIdx]) kbPress(keys[kbFocusIdx]);
        return;
      }
      if (e.key === 'Escape' || e.key === 'Backspace') {
        e.preventDefault();
        if (e.key === 'Escape') hideKeyboard();
        else kbPress({ dataset: { action: 'delete' } });
        return;
      }
    }

    // Пульт: Back / BrowserBack
    if (e.key === 'GoBack' || e.key === 'BrowserBack' || (e.key === 'Backspace' && !kbActive && document.activeElement !== nameInput)) {
      e.preventDefault();
      if (currentScreen === 'settings') showScreen('launcher');
      else if (currentScreen === 'launcher') showToast('Нажми ещё раз для выхода');
      return;
    }

    // Пульт: Menu / ContextMenu
    if (e.key === 'ContextMenu' || e.key === 'BrowserRefresh' || e.key === 'F2') {
      e.preventDefault();
      if (currentScreen === 'launcher') showScreen('settings');
      return;
    }

    // Цветные кнопки пульта (красная/зелёная/жёлтая/синяя)
    if (e.key === 'ColorF0Red' || e.code === 'F6') { showToast('🔴 Красная кнопка'); return; }
    if (e.key === 'ColorF1Green' || e.code === 'F7') { showScreen('launcher'); return; }
    if (e.key === 'ColorF2Yellow' || e.code === 'F8') { showToast('🟡 Жёлтая кнопка'); return; }
    if (e.key === 'ColorF3Blue' || e.code === 'F9') { showScreen('settings'); return; }

    // Цифры 1-9 для быстрого выбора приложений
    if (/^[1-9]$/.test(e.key) && currentScreen === 'launcher') {
      const num = parseInt(e.key);
      const tile = document.querySelector(`.tile[data-num="${num}"]`);
      if (tile) {
        tile.click();
        showToast('Открыто: ' + tile.textContent.trim().split('\n').pop());
      }
      return;
    }

    // Навигация стрелками
    if (['ArrowLeft','ArrowRight','ArrowUp','ArrowDown'].includes(e.key)) {
      e.preventDefault();
      moveFocus(e.key.replace('Arrow','').toLowerCase());
      return;
    }

    // Enter — выбор
    if (e.key === 'Enter') {
      e.preventDefault();
      if (currentScreen === 'welcome') {
        if (document.activeElement === nameInput) showKeyboard();
        else if (document.activeElement === saveBtn) saveProfile();
        else document.activeElement?.click();
      } else {
        focusable[focusIdx]?.click();
      }
      return;
    }

    // Escape — назад
    if (e.key === 'Escape') {
      if (currentScreen === 'settings') showScreen('launcher');
      return;
    }
  });

  // ===== GAMEPAD API =====
  let gamepadIndex = null;
  let gpPrevButtons = {};
  let gpAxesDebounce = { x: 0, y: 0, t: 0 };

  window.addEventListener('gamepadconnected', (e) => {
    gamepadIndex = e.gamepad.index;
    document.getElementById('gamepadIndicator').classList.add('active');
    document.getElementById('gamepadStatus').textContent = 'Геймпад: ' + e.gamepad.id.slice(0, 40);
    showToast('🎮 Геймпад подключён: ' + e.gamepad.id.slice(0, 30));
  });

  window.addEventListener('gamepaddisconnected', () => {
    gamepadIndex = null;
    document.getElementById('gamepadIndicator').classList.remove('active');
    document.getElementById('gamepadStatus').textContent = 'Геймпад: не подключён';
    showToast('🎮 Геймпад отключён');
  });

  function gpButtonPressed(gp, idx) {
    return gp.buttons[idx] && gp.buttons[idx].pressed;
  }

  function gpPoll() {
    if (gamepadIndex === null) {
      requestAnimationFrame(gpPoll);
      return;
    }
    
    const gp = navigator.getGamepads()[gamepadIndex];
    if (!gp) { requestAnimationFrame(gpPoll); return; }
    
    const now = Date.now();
    
    // D-Pad (кнопки 12-15)
    const dpad = {
      up: gpButtonPressed(gp, 12),
      down: gpButtonPressed(gp, 13),
      left: gpButtonPressed(gp, 14),
      right: gpButtonPressed(gp, 15)
    };
    
    // Аналоговые стики как D-Pad
    const DEAD = 0.5;
    if (gp.axes[1] < -DEAD) dpad.up = true;
    if (gp.axes[1] > DEAD) dpad.down = true;
    if (gp.axes[0] < -DEAD) dpad.left = true;
    if (gp.axes[0] > DEAD) dpad.right = true;
    
    // Навигация с дебаунсом
    if (now - gpAxesDebounce.t > 180) {
      if (dpad.up) {
        if (kbActive) kbNavigate('up'); else moveFocus('up');
        gpAxesDebounce.t = now;
      } else if (dpad.down) {
        if (kbActive) kbNavigate('down'); else moveFocus('down');
        gpAxesDebounce.t = now;
      } else if (dpad.left) {
        if (kbActive) kbNavigate('left'); else moveFocus('left');
        gpAxesDebounce.t = now;
      } else if (dpad.right) {
        if (kbActive) kbNavigate('right'); else moveFocus('right');
        gpAxesDebounce.t = now;
      }
    }
    
    // A (кресток) — кнопка 0 — OK
    if (gpButtonPressed(gp, 0) && !gpPrevButtons[0]) {
      if (kbActive) {
        const keys = getKbKeys();
        if (keys[kbFocusIdx]) kbPress(keys[kbFocusIdx]);
      } else if (currentScreen === 'welcome') {
        if (document.activeElement === nameInput || document.activeElement?.classList.contains('color-swatch')) {
          if (document.activeElement === nameInput) showKeyboard();
          else document.activeElement.click();
        } else saveProfile();
      } else {
        focusable[focusIdx]?.click();
      }
    }
    
    // B (круг) — кнопка 1 — Back
    if (gpButtonPressed(gp, 1) && !gpPrevButtons[1]) {
      if (kbActive) hideKeyboard();
      else if (currentScreen === 'settings') showScreen('launcher');
    }
    
    // Start — кнопка 9 — меню
    if (gpButtonPressed(gp, 9) && !gpPrevButtons[9]) {
      if (currentScreen === 'launcher') showScreen('settings');
      else if (currentScreen === 'settings') showScreen('launcher');
    }
    
    // Y — кнопка 3 — сброс фокуса
    if (gpButtonPressed(gp, 3) && !gpPrevButtons[3]) {
      resetFocus();
    }
    
    // Сохраняем состояние кнопок
    for (let i = 0; i < gp.buttons.length; i++) {
      gpPrevButtons[i] = gpButtonPressed(gp, i);
    }
    
    requestAnimationFrame(gpPoll);
  }
  requestAnimationFrame(gpPoll);

  // ===== СТАРТ =====
  if (userData) {
    applyUserData();
    showScreen('launcher');
  } else {
    showScreen('welcome');
    setTimeout(() => {
      focusable = Array.from(document.querySelectorAll(`#welcome [tabindex="0"]`));
      if (focusable.length) {
        focusIdx = 0;
        focusable[0].classList.add('focused');
        focusable[0].focus();
      }
    }, 100);
  }
</script>
</body>
</html>
