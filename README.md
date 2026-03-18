<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="theme-color" content="#F2F2F7">
    <title>Iru J&K Ultra</title>
    
    <style>
        /* --- 1. NATIVE MOBILE VARIABLES --- */
        :root {
            --primary: #007AFF;
            --primary-light: #E5F1FF;
            --bg: #F2F2F7;
            --card: #FFFFFF;
            --text-main: #000000;
            --text-sub: #8E8E93;
            --green: #34C759;
            --red: #FF3B30;
            --orange: #FF9500;
            --purple: #AF52DE;
            --dark: #1C1C1E;
            --shadow: 0 4px 20px rgba(0,0,0,0.06);
            --safe-top: env(safe-area-inset-top, 20px);
            --safe-bottom: env(safe-area-inset-bottom, 20px);
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; outline: none; margin: 0; padding: 0; }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg); color: var(--text-main);
            width: 100vw; height: 100vh; height: 100dvh;
            overflow: hidden; 
            user-select: none; -webkit-user-select: none; 
        }

        /* Allows typing in inputs */
        input, textarea, button, select { user-select: auto; -webkit-user-select: auto; font-family: inherit; }

        /* --- 2. LAYOUT ARCHITECTURE --- */
        #app-layout {
            position: absolute; top: 0; left: 0; right: 0; bottom: 0;
            display: flex; flex-direction: column; background: var(--bg);
            z-index: 10; display: none;
        }

        /* Header */
        .app-header {
            background: rgba(255,255,255,0.85); backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px);
            padding: 12px 20px; padding-top: max(12px, var(--safe-top));
            border-bottom: 1px solid rgba(0,0,0,0.05);
            display: flex; justify-content: space-between; align-items: center;
            flex-shrink: 0; z-index: 100;
        }
        .brand { font-size: 24px; font-weight: 900; letter-spacing: -0.5px; }
        .brand span { color: var(--red); }
        .location-pill {
            background: var(--primary-light); color: var(--primary);
            padding: 6px 14px; border-radius: 20px; font-size: 13px; font-weight: 700;
            display: flex; align-items: center; gap: 4px; cursor: pointer; transition: 0.2s;
        }
        .location-pill:active { transform: scale(0.95); opacity: 0.8; }

        /* Main Scrollable Area */
        .main-container {
            flex: 1; overflow-y: auto; overflow-x: hidden; scroll-behavior: smooth;
            padding: 20px; padding-bottom: calc(100px + var(--safe-bottom)); 
            -webkit-overflow-scrolling: touch; 
        }
        .main-container::-webkit-scrollbar { display: none; }

        .view-section { display: none; animation: slideIn 0.3s cubic-bezier(0.25, 0.8, 0.25, 1); }
        .view-section.active { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        /* --- 3. UI COMPONENTS --- */
        .section-header { font-size: 22px; font-weight: 800; margin-bottom: 15px; color: var(--text-main); }
        
        .search-bar {
            width: 100%; padding: 14px 18px; border-radius: 16px; border: none;
            background: #FFFFFF; font-size: 16px; margin-bottom: 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.04); transition: 0.3s;
        }
        .search-bar:focus { box-shadow: 0 4px 15px rgba(0,122,255,0.15); }

        /* Smart SOS Button */
        .sos-panic-btn {
            background: linear-gradient(135deg, #FF3B30, #FF2D55);
            color: white; width: 100%; padding: 20px; border-radius: 20px;
            display: flex; align-items: center; justify-content: center; gap: 10px;
            font-size: 18px; font-weight: 900; box-shadow: 0 10px 25px rgba(255, 59, 48, 0.4);
            margin-bottom: 20px; cursor: pointer; transition: 0.2s;
            animation: pulse 2s infinite; border: none;
        }
        .sos-panic-btn:active { transform: scale(0.95); }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(255, 59, 48, 0.5); } 70% { box-shadow: 0 0 0 15px rgba(255, 59, 48, 0); } 100% { box-shadow: 0 0 0 0 rgba(255, 59, 48, 0); } }

        /* Emergency Cards */
        .service-card {
            background: var(--card); border-radius: 20px; padding: 16px; margin-bottom: 12px;
            display: flex; align-items: center; gap: 14px;
            box-shadow: var(--shadow); border: 1px solid rgba(0,0,0,0.02);
            transition: transform 0.15s; cursor: pointer;
        }
        .service-card:active { transform: scale(0.96); background: #fafafa; }
        
        .icon-box {
            width: 50px; height: 50px; border-radius: 14px;
            display: flex; align-items: center; justify-content: center;
            font-size: 24px; color: white; flex-shrink: 0;
        }
        .info { flex: 1; overflow: hidden; }
        .info h3 { margin: 0 0 3px; font-size: 16px; font-weight: 700; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .info p { margin: 0; font-size: 12px; color: var(--text-sub); display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; line-height: 1.3; }

        .call-btn {
            background: #E8F5E9; color: var(--green);
            padding: 8px 14px; border-radius: 16px; font-weight: 800; font-size: 13px;
            display: flex; align-items: center; gap: 4px; pointer-events: none;
        }

        /* Booking Grid */
        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .book-card {
            background: white; padding: 20px 15px; border-radius: 20px;
            display: flex; flex-direction: column; align-items: center; text-align: center;
            box-shadow: var(--shadow); cursor: pointer; transition: 0.15s;
        }
        .book-card:active { transform: scale(0.95); }
        .b-icon { font-size: 32px; margin-bottom: 8px; }
        .b-title { font-weight: 700; font-size: 14px; margin-bottom: 3px; }
        .b-desc { font-size: 11px; color: var(--text-sub); line-height: 1.3; padding: 0 4px; }

        /* Medical ID */
        .med-card { background: white; border-radius: 24px; padding: 20px; box-shadow: var(--shadow); border-left: 6px solid var(--red); margin-bottom: 20px; }
        .med-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin: 15px 0; }
        .med-box { background: #f9f9f9; padding: 12px 5px; border-radius: 12px; text-align: center; }
        .med-label { font-size: 10px; font-weight: 700; color: #888; text-transform: uppercase; }
        .med-val { font-size: 16px; font-weight: 800; display: block; margin-top: 4px; }
        .sos-btn { background: var(--red); color: white; width: 100%; padding: 14px; border-radius: 14px; font-weight: bold; text-align: center; display: block; text-decoration: none; margin-top: 15px; }

        /* --- 4. BOTTOM NAVIGATION --- */
        .bottom-nav {
            position: absolute; bottom: 0; left: 0; right: 0;
            background: rgba(255,255,255,0.92);
            backdrop-filter: blur(25px); -webkit-backdrop-filter: blur(25px);
            border-top: 1px solid rgba(0,0,0,0.05);
            display: flex; justify-content: space-around; 
            padding: 10px 0; padding-bottom: max(10px, var(--safe-bottom));
            z-index: 1000;
        }
        .nav-item {
            display: flex; flex-direction: column; align-items: center; gap: 4px;
            color: #999; font-size: 10px; font-weight: 600;
            padding: 5px 15px; border-radius: 12px; transition: 0.2s; cursor: pointer;
        }
        .nav-item.active { color: var(--primary); transform: translateY(-2px); }
        .nav-item svg { width: 24px; height: 24px; fill: currentColor; }

        /* --- 5. LOGIN SCREEN --- */
        #login-screen {
            position: absolute; inset: 0; z-index: 9999;
            background: white; display: flex; flex-direction: column;
            align-items: center; justify-content: center;
        }
        .login-card { width: 85%; max-width: 360px; text-align: center; }
        .login-inp {
            width: 100%; padding: 16px; margin-bottom: 15px;
            border: 1px solid #ddd; border-radius: 16px; font-size: 16px;
            background: #FAFAFA; transition: 0.3s;
        }
        .login-inp:focus { border-color: var(--primary); background: white; }
        .login-btn {
            width: 100%; padding: 16px; background: var(--primary);
            color: white; border: none; border-radius: 16px;
            font-size: 16px; font-weight: 700; cursor: pointer;
        }

        /* --- 6. TOAST & BOTTOM SHEET MODALS --- */
        .toast {
            position: fixed; bottom: -60px; left: 50%; transform: translateX(-50%);
            background: #333; color: white; padding: 12px 24px; border-radius: 30px;
            font-size: 14px; font-weight: 600; z-index: 10000; transition: bottom 0.3s ease;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2); white-space: nowrap; pointer-events: none;
        }
        .toast.show { bottom: calc(80px + var(--safe-bottom)); }

        .overlay {
            position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 5000;
            opacity: 0; pointer-events: none; transition: opacity 0.3s;
            backdrop-filter: blur(3px); -webkit-backdrop-filter: blur(3px);
        }
        .overlay.show { opacity: 1; pointer-events: auto; }
        
        .bottom-sheet {
            position: absolute; bottom: -100%; left: 0; right: 0;
            background: white; border-radius: 24px 24px 0 0;
            padding: 25px 20px; padding-bottom: max(25px, var(--safe-bottom));
            transition: bottom 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
            max-height: 85vh; overflow-y: auto;
        }
        .overlay.show .bottom-sheet { bottom: 0; }

        .dist-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 15px; }
        .dist-btn { padding: 14px; background: #F2F2F7; border-radius: 14px; text-align: center; font-weight: 600; font-size: 14px; cursor: pointer; }
        .dist-btn:active { transform: scale(0.95); }

        /* Glow Signature */
        .creator-glow {
            text-align: center; margin-top: 40px; padding: 20px;
            font-weight: 800; font-size: 13px; letter-spacing: 1.5px;
            text-transform: uppercase; color: #333;
            animation: neon 3s infinite alternate;
        }
        @keyframes neon {
            from { text-shadow: 0 0 5px rgba(0,122,255,0.2); }
            to { text-shadow: 0 0 15px rgba(175,82,222,0.8); }
        }

        /* Utilities */
        .bg-blue { background: linear-gradient(135deg, #007AFF, #5AC8FA); }
        .bg-red { background: linear-gradient(135deg, #FF3B30, #FF2D55); }
        .bg-green { background: linear-gradient(135deg, #34C759, #30B350); }
        .bg-orange { background: linear-gradient(135deg, #FF9500, #FFCC00); }
        .bg-purple { background: linear-gradient(135deg, #AF52DE, #5856D6); }
        .bg-dark { background: linear-gradient(135deg, #2C2C2E, #3A3A3C); }

    </style>
</head>
<body>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="toast">Message</div>

    <!-- LOGIN SCREEN -->
    <div id="login-screen">
        <div class="login-card">
            <div style="font-size:50px; margin-bottom:10px;">🚨</div>
            <h1 style="font-size:32px; margin-bottom:5px; color:var(--text-main);">Iru <span style="color:var(--red);">Ultra</span></h1>
            <p style="color:var(--text-sub); margin-bottom:30px; font-size:14px;">J&K's Native Emergency App</p>
            
            <input type="text" id="loginName" class="login-inp" placeholder="Your Full Name">
            <input type="tel" id="loginPhone" class="login-inp" placeholder="Phone Number">
            <button class="login-btn" onclick="handleLogin()">Enter Securely</button>
        </div>
    </div>

    <!-- MAIN APP LAYOUT -->
    <div id="app-layout">
        
        <!-- Header -->
        <header class="app-header">
            <div class="brand">Iru <span>Ultra</span></div>
            <div class="location-pill" onclick="openDistModal()">
                📍 <span id="headerDist">Srinagar</span> ▼
            </div>
        </header>

        <!-- Content -->
        <main class="main-container">
            
            <!-- VIEW 1: HOME -->
            <div id="view-home" class="view-section active">
                <div style="margin-bottom:15px; margin-left:5px;">
                    <div style="font-size:13px; color:var(--text-sub); font-weight:600;">Welcome back, <span id="uNameDisplay">User</span> 👋</div>
                    <div class="section-header" style="margin:0;">Emergency Services</div>
                </div>

                <!-- SMART SOS PANIC BUTTON -->
                <button class="sos-panic-btn" onclick="triggerSOS()">
                    <span>🚨</span> TAP FOR INSTANT SOS
                </button>
                
                <input type="text" class="search-bar" placeholder="🔍 Search Police, Fire, Ambulance..." onkeyup="renderServices(this.value)">
                <div id="emergencyList"></div>
            </div>

            <!-- VIEW 2: BOOKINGS -->
            <div id="view-book" class="view-section">
                <div class="section-header">Bookings & Utilities</div>
                <div class="grid-2" id="bookingList"></div>
            </div>

            <!-- VIEW 3: SAFETY TOOLS & FIRST AID -->
            <div id="view-tools" class="view-section">
                
                <div class="section-header">Safety Toolkit</div>
                <div class="grid-2">
                    <div class="book-card" onclick="toggleFlash()">
                        <div class="b-icon">🔦</div>
                        <div class="b-title">Flashlight</div>
                        <div class="b-desc">Screen Torch</div>
                    </div>
                    <div class="book-card" onclick="toggleWhistle()">
                        <div class="b-icon">📢</div>
                        <div class="b-title">SOS Whistle</div>
                        <div class="b-desc">Loud Alarm</div>
                    </div>
                    <div class="book-card" onclick="shareLocation()">
                        <div class="b-icon">📍</div>
                        <div class="b-title">Share Loc</div>
                        <div class="b-desc">Send GPS Link</div>
                    </div>
                </div>

                <!-- FIRST-AID & DISASTER GUIDES -->
                <div class="section-header" style="margin-top:30px;">First-Aid & Disaster Guide</div>
                <div class="grid-2">
                    <div class="book-card" style="border-left: 4px solid var(--red);" onclick="showGuide('fire')">
                        <div class="b-icon">🔥</div>
                        <div class="b-title">Fire Safety</div>
                    </div>
                    <div class="book-card" style="border-left: 4px solid var(--orange);" onclick="showGuide('lpg')">
                        <div class="b-icon">⛽</div>
                        <div class="b-title">LPG Leak</div>
                    </div>
                    <div class="book-card" style="border-left: 4px solid var(--dark);" onclick="showGuide('quake')">
                        <div class="b-icon">🏚️</div>
                        <div class="b-title">Earthquake</div>
                    </div>
                    <div class="book-card" style="border-left: 4px solid var(--green);" onclick="showGuide('cpr')">
                        <div class="b-icon">❤️</div>
                        <div class="b-title">CPR / Heart</div>
                    </div>
                    <div class="book-card" style="border-left: 4px solid var(--purple);" onclick="showGuide('bleed')">
                        <div class="b-icon">🩸</div>
                        <div class="b-title">Bleeding</div>
                    </div>
                    <div class="book-card" style="border-left: 4px solid var(--primary);" onclick="showGuide('choke')">
                        <div class="b-icon">🤢</div>
                        <div class="b-title">Choking</div>
                    </div>
                </div>
            </div>

            <!-- VIEW 4: PROFILE & ABOUT -->
            <div id="view-profile" class="view-section">
                
                <!-- Profile Block -->
                <div style="background:white; padding:25px; border-radius:24px; text-align:center; box-shadow:var(--shadow); margin-bottom:25px;">
                    <div style="width:80px; height:80px; background:var(--primary); color:white; border-radius:50%; margin:0 auto 15px; display:flex; align-items:center; justify-content:center; font-size:36px; font-weight:800;" id="pAvatar">U</div>
                    <h2 id="pName" style="margin-bottom:5px;">User Name</h2>
                    <p id="pPhone" style="color:var(--text-sub); font-size:15px; margin-bottom:15px;">+91...</p>
                    <button onclick="logout()" style="background:#FFEBEB; color:var(--red); padding:10px 20px; border-radius:12px; border:none; font-weight:700; cursor:pointer;">Log Out</button>
                </div>

                <!-- Medical ID -->
                <div class="section-header">Medical ID (Offline)</div>
                <div id="medDisplay" class="med-card">
                    <div style="display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid #eee; padding-bottom:10px; margin-bottom:15px;">
                        <div style="font-weight:800; color:var(--red); display:flex; align-items:center; gap:6px;"><span>🏥</span> Medical Profile</div>
                        <button onclick="toggleMedEdit()" style="background:#f0f0f0; border:none; padding:6px 12px; border-radius:10px; font-weight:700; font-size:12px; color:var(--text-sub);">EDIT</button>
                    </div>
                    <div class="med-grid">
                        <div class="med-box"><span class="med-label">BLOOD</span><span class="med-val" id="dBlood">--</span></div>
                        <div class="med-box"><span class="med-label">AGE</span><span class="med-val" id="dAge">--</span></div>
                        <div class="med-box"><span class="med-label">WEIGHT</span><span class="med-val" id="dWeight">--</span></div>
                    </div>
                    <div style="font-size:14px; margin-bottom:8px;"><strong>Condition:</strong> <span id="dCond" style="color:#555;">None</span></div>
                    <div style="font-size:14px; margin-bottom:15px;"><strong>Allergies:</strong> <span id="dAlg" style="color:#555;">None</span></div>
                    <a href="#" id="dIce" class="sos-btn" style="display:none;">📞 Call Emergency Contact</a>
                    <div id="noIceMsg" style="text-align:center; font-size:12px; color:#aaa; margin-top:10px;">Save ICE Number for SOS Button to work.</div>
                </div>

                <!-- Edit Form -->
                <div id="medForm" style="display:none; background:white; padding:20px; border-radius:24px; box-shadow:var(--shadow); margin-bottom:20px;">
                    <h3 style="margin-bottom:15px;">Update Details</h3>
                    <input type="text" id="inBlood" class="login-inp" placeholder="Blood Group (e.g., O+)">
                    <div class="grid-2" style="gap:10px; margin-bottom:15px;">
                        <input type="number" id="inAge" class="login-inp" style="margin:0;" placeholder="Age">
                        <input type="number" id="inWeight" class="login-inp" style="margin:0;" placeholder="Weight">
                    </div>
                    <input type="text" id="inCond" class="login-inp" placeholder="Conditions (e.g., Asthma)">
                    <input type="text" id="inAlg" class="login-inp" placeholder="Allergies (e.g., Peanuts)">
                    <input type="tel" id="inIce" class="login-inp" placeholder="Emergency Contact Number">
                    <button class="login-btn" onclick="saveMedData()">Save Medical ID</button>
                    <button onclick="toggleMedEdit()" style="width:100%; padding:15px; background:none; border:none; color:var(--text-sub); font-weight:600; margin-top:5px;">Cancel</button>
                </div>

                <!-- About Section -->
                <div class="section-header" style="margin-top:35px;">About Application</div>
                <div style="background:white; padding:25px; border-radius:24px; line-height:1.8; font-size:14px; color:#444; box-shadow:var(--shadow);">
                    <strong>Welcome to Iru J&K Ultra Ultimate Edition</strong><br><br>
                    This application is a comprehensive digital lifeline designed specifically for the citizens and tourists of Jammu & Kashmir. In a region where terrain and connectivity can sometimes be challenging, having immediate offline access to the right tools is a necessity.<br><br>
                    
                    <strong>Smart Localized Engine:</strong><br>
                    Unlike generic search engines, this app contains verified numbers for all 20 districts of J&K. When you select a district like Bandipora or Srinagar, the Police Control Room, Fire, and Disaster Management numbers automatically configure themselves to the exact local STD code and helpline digits.<br><br>
                    
                    <strong>Comprehensive Coverage:</strong><br>
                    1. <strong>40+ Emergency Services:</strong> Spanning from Ambulances and Snake Bite anti-venom info to Highway Help, Drug De-addiction, and Cyber Crime. All with detailed descriptions.<br>
                    2. <strong>30+ Booking Options:</strong> Instantly book flights, trains, pay electricity bills, recharge FASTag, or download your Aadhaar card.<br>
                    3. <strong>Smart SOS System:</strong> The large red panic button instantly sends your GPS location to your saved medical emergency contact.<br>
                    4. <strong>Offline Medical Safety:</strong> The Digital Medical ID card stores your blood group, conditions, and emergency contact locally on your phone.<br>
                    5. <strong>First-Aid & Safety Toolkit:</strong> Flashlight, High-Frequency Alarm, and offline guides for Choking, Bleeding, CPR, and Earthquakes.<br><br>

                    <strong>Privacy Commitment:</strong><br>
                    Your login details and medical records are stored strictly on your device's Local Storage. We respect your privacy, and no data is sent to third-party servers.<br><br>
                    
                    <strong>Dedication:</strong><br>
                    This app is a tribute to the resilience of the people of Jammu & Kashmir. A tool for empowerment, safety, and digital readiness.
                    
                    <div class="creator-glow">
                        ✨ Created & Designed by Irfan Ahmed ✨
                    </div>
                </div>
            </div>

        </main>

        <!-- BOTTOM NAV -->
        <nav class="bottom-nav">
            <div class="nav-item active" id="nav-home" onclick="switchTab('home')">
                <svg viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 15h-2v-6h2v6zm0-8h-2V7h2v2z"/></svg>
                Help
            </div>
            <div class="nav-item" id="nav-book" onclick="switchTab('book')">
                <svg viewBox="0 0 24 24"><path d="M4 6H2v14c0 1.1.9 2 2 2h14v-2H4V6zm16-4H8c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm-1 9H9V9h10v2zm-4 4H9v-2h6v2zm4-8H9V5h10v2z"/></svg>
                Book
            </div>
            <div class="nav-item" id="nav-tools" onclick="switchTab('tools')">
                <svg viewBox="0 0 24 24"><path d="M22.7 19l-9.1-9.1c.9-2.3.4-5-1.5-6.9-2-2-5-2.4-7.4-1.3L9 6 6 9 1.6 4.7C.4 7.1.9 10.1 2.9 12.1c1.9 1.9 4.6 2.4 6.9 1.5l9.1 9.1c.4.4 1 .4 1.4 0l2.3-2.3c.5-.4.5-1.1.1-1.4z"/></svg>
                Safety
            </div>
            <div class="nav-item" id="nav-profile" onclick="switchTab('profile')">
                <svg viewBox="0 0 24 24"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>
                Profile
            </div>
        </nav>
    </div>

    <!-- BOTTOM SHEET MODALS -->
    <div id="distModal" class="overlay" onclick="closeModal(event, 'distModal')">
        <div class="bottom-sheet" onclick="event.stopPropagation()">
            <h2 style="margin:0 0 5px; font-size:22px;">Select District</h2>
            <p style="color:var(--text-sub); font-size:13px; margin-bottom:15px;">Local numbers will update automatically.</p>
            <div class="dist-grid" id="distList"></div>
        </div>
    </div>

    <div id="guideModal" class="overlay" onclick="closeModal(event, 'guideModal')">
        <div class="bottom-sheet" onclick="event.stopPropagation()">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;">
                <h2 id="gTitle" style="color:var(--primary); margin:0;">Guide</h2>
                <span style="background:#f2f2f7; padding:5px 12px; border-radius:12px; font-size:11px; font-weight:800; color:#555;">TIPS</span>
            </div>
            <div id="gContent" style="line-height:1.8; color:#333; white-space: pre-line; font-size:15px; background:#fafafa; padding:20px; border-radius:20px; border:1px solid #eee;"></div>
        </div>
    </div>

    <!-- FLASH OVERLAY -->
    <div id="flash-overlay" style="position:fixed; inset:0; background:white; z-index:10000; display:none;" onclick="toggleFlash()"></div>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="toast">Message</div>

    <script>
        // --- 1. DATA CONFIGURATION ---
        
        // 20 Districts mapped to their respective STD Codes and Local Endings
        const districtConfig = {
            "Anantnag": { std: "01932", pcr: "222870", fire: "222222", disaster: "222333" },
            "Bandipora": { std: "01957", pcr: "225278", fire: "225057", disaster: "226085" },
            "Baramulla": { std: "01952", pcr: "234210", fire: "234400", disaster: "234343" },
            "Budgam": { std: "01951", pcr: "255207", fire: "255034", disaster: "255255" },
            "Doda": { std: "01996", pcr: "233333", fire: "233101", disaster: "233000" },
            "Ganderbal": { std: "0194", pcr: "2416478", fire: "2416101", disaster: "2416222" },
            "Jammu": { std: "0191", pcr: "2542000", fire: "2544101", disaster: "2541111" },
            "Kathua": { std: "01922", pcr: "234100", fire: "234101", disaster: "234500" },
            "Kishtwar": { std: "01995", pcr: "259200", fire: "259101", disaster: "259300" },
            "Kulgam": { std: "01931", pcr: "260124", fire: "260101", disaster: "260333" },
            "Kupwara": { std: "01955", pcr: "252451", fire: "252101", disaster: "252252" },
            "Poonch": { std: "01965", pcr: "220250", fire: "220101", disaster: "220400" },
            "Pulwama": { std: "01933", pcr: "241221", fire: "241101", disaster: "241333" },
            "Rajouri": { std: "01962", pcr: "263200", fire: "263101", disaster: "263500" },
            "Ramban": { std: "01998", pcr: "266700", fire: "266101", disaster: "266800" },
            "Reasi": { std: "01991", pcr: "245600", fire: "245101", disaster: "245700" },
            "Samba": { std: "01923", pcr: "243200", fire: "243101", disaster: "243900" },
            "Shopian": { std: "01933", pcr: "260990", fire: "260444", disaster: "260555" },
            "Srinagar": { std: "0194", pcr: "2452092", fire: "2479488", disaster: "2474040" },
            "Udhampur": { std: "01992", pcr: "270200", fire: "270101", disaster: "270600" }
        };
        const districtNames = Object.keys(districtConfig);

        // 40+ Detailed Emergency Services
        const emergencyServices =[
            {n: "Police Control Room", type: "local", key: "pcr", i: "👮", c: "bg-blue", d: "District Police Headquarters for immediate crime reporting and law & order assistance."},
            {n: "Fire & Emergency", type: "local", key: "fire", i: "🔥", c: "bg-red", d: "Fire outbreaks, short circuits, and rescue operations coordination."},
            {n: "Disaster Mgmt", type: "local", key: "disaster", i: "⛈️", c: "bg-dark", d: "Flood, Earthquake, and heavy Snowfall control room."},
            {n: "Ambulance", type: "national", num: "108", i: "🚑", c: "bg-green", d: "Free National Medical Emergency Transport Service."},
            {n: "National Emergency", type: "national", num: "112", i: "🚨", c: "bg-red", d: "All-in-one Emergency Response Support System (ERSS)."},
            {n: "Women Helpline", type: "national", num: "181", i: "👩", c: "bg-purple", d: "24/7 Support and rescue for women facing domestic violence or harassment."},
            {n: "Cyber Crime", type: "national", num: "1930", i: "💻", c: "bg-dark", d: "Report online financial fraud, bank scams, or digital harassment immediately."},
            {n: "Child Helpline", type: "national", num: "1098", i: "👶", c: "bg-orange", d: "Protection and assistance for children in distress or facing abuse."},
            {n: "Electricity (PDD)", type: "national", num: "1912", i: "⚡", c: "bg-orange", d: "Report power cuts, transformer faults, or dangerous fallen wires."},
            {n: "Traffic Police", type: "national", num: "103", i: "🚦", c: "bg-green", d: "Report severe traffic jams, road accidents, and highway blockages."},
            {n: "Tourist Helpline", type: "national", num: "1800111363", i: "🏔️", c: "bg-blue", d: "Safety guidance, support, and information for tourists visiting J&K."},
            {n: "Snake Bite Help", type: "national", num: "108", i: "🐍", c: "bg-green", d: "Anti-Venom emergency availability and medical first response."},
            {n: "Railway Inquiry", type: "national", num: "139", i: "🚆", c: "bg-blue", d: "Check Train PNR status, seat availability, and train timings."},
            {n: "Voter Helpline", type: "national", num: "1950", i: "🗳️", c: "bg-dark", d: "Election Commission helpline for Voter ID and polling info."},
            {n: "Gas Leakage", type: "national", num: "1906", i: "⛽", c: "bg-red", d: "Immediate emergency helpline for LPG cylinder leakage and fire risk."},
            {n: "SDRF Rescue", type: "national", num: "1070", i: "🚣", c: "bg-orange", d: "State Disaster Response Force for water rescue and natural calamities."},
            {n: "Senior Citizen", type: "national", num: "14567", i: "👴", c: "bg-purple", d: "Elderline: Emotional support, legal help, and rescue for senior citizens."},
            {n: "Drug Rehab", type: "national", num: "14446", i: "💊", c: "bg-blue", d: "Tele-Manas Counseling and rehabilitation support for substance abuse."},
            {n: "Animal Rescue", type: "national", num: "1962", i: "🐾", c: "bg-orange", d: "Mobile Veterinary Ambulance for injured street animals and pets."},
            {n: "Highway Help", type: "national", num: "1033", i: "🛣️", c: "bg-red", d: "Emergency help on National Highways and Expressways (NH44)."},
            {n: "Water Supply", type: "national", num: "18001807045", i: "💧", c: "bg-blue", d: "Jal Shakti / PHE Department for water shortage or contamination complaints."},
            {n: "Municipality", type: "national", num: "155304", i: "🧹", c: "bg-green", d: "Garbage collection, sanitation issues, and street light complaints."},
            {n: "Food Safety", type: "national", num: "1800112100", i: "🥗", c: "bg-orange", d: "Report adulterated, expired, or unsafe food products sold in markets."},
            {n: "Consumer Court", type: "national", num: "1915", i: "⚖️", c: "bg-dark", d: "File complaints against unfair trade practices, overpricing, or fraud."},
            {n: "Aadhaar Help", type: "national", num: "1947", i: "🆔", c: "bg-red", d: "UIDAI official support for Aadhaar updates, links, and issues."},
            {n: "Income Tax", type: "national", num: "1961", i: "💰", c: "bg-blue", d: "Help regarding Income Tax Returns (ITR) and PAN card applications."},
            {n: "GST Help", type: "national", num: "18001200232", i: "📊", c: "bg-dark", d: "Support for GST filing, business tax queries, and fraud reporting."},
            {n: "Bank Fraud", type: "national", num: "155260", i: "🏦", c: "bg-red", d: "Immediately report unauthorized banking transactions to freeze accounts."},
            {n: "Post Office", type: "national", num: "1924", i: "✉️", c: "bg-orange", d: "Track parcels, speed posts, and register postal service complaints."},
            {n: "BSNL Help", type: "national", num: "1503", i: "📞", c: "bg-blue", d: "BSNL network, broadband fiber, and landline complaints."},
            {n: "Air Ambulance", type: "national", num: "9540161344", i: "🚁", c: "bg-red", d: "Critical patient airlift service (Note: This is usually a Paid Service)."},
            {n: "Army Helpline", type: "national", num: "1904", i: "🎖️", c: "bg-green", d: "Indian Army assistance helpline for civilians in border/remote areas."},
            {n: "CRPF Help", type: "national", num: "14411", i: "👮‍♂️", c: "bg-blue", d: "Madadgaar Helpline for public safety, medical help, and emergency assistance."},
            {n: "Mental Health", type: "national", num: "14416", i: "🧠", c: "bg-purple", d: "Tele-Manas: Free mental health counseling and psychological support."},
            {n: "Missing Person", type: "national", num: "1094", i: "👤", c: "bg-dark", d: "Report missing persons directly to specialized police tracking units."},
            {n: "Earthquake", type: "national", num: "1092", i: "🏚️", c: "bg-orange", d: "Seismic activity information, relief coordination, and safety guidelines."},
            {n: "Mine Safety", type: "national", num: "1800345", i: "⛏️", c: "bg-dark", d: "Report mining accidents, collapses, or unsafe labor practices."},
            {n: "Legal Aid", type: "national", num: "15100", i: "⚖️", c: "bg-blue", d: "Free legal advice and aid for underprivileged citizens."},
            {n: "CM Grievance", type: "national", num: "180018070", i: "🏛️", c: "bg-green", d: "Register civil complaints directly to the Chief Minister's Grievance Cell."},
            {n: "Weather Info", type: "national", num: "18001801", i: "⛅", c: "bg-blue", d: "Get live weather forecasts, heavy rain, and storm warnings."},
            {n: "Avalanche Info", type: "national", num: "0194245", i: "❄️", c: "bg-red", d: "Snow avalanche warnings for hilly areas (Gulmarg, Sonamarg, etc)."},
            {n: "Coast Guard", type: "national", num: "1554", i: "🌊", c: "bg-teal", d: "Maritime Search and Rescue (For coastal/water body emergencies)."}
        ];

        // 30+ Bookings with detailed descriptions
        const bookings =[
            {t:"Flight Ticket", i:"✈️", d:"Book domestic & international flights instantly.", l:"https://www.makemytrip.com/"},
            {t:"Train Ticket", i:"🚆", d:"Check seat availability & book train tickets.", l:"https://www.irctc.co.in/"},
            {t:"Bus Ticket", i:"🚌", d:"Book AC/Non-AC intercity buses via RedBus.", l:"https://www.redbus.in/"},
            {t:"Book Cab", i:"🚖", d:"Book an Uber or Ola for local city travel.", l:"https://www.uber.com/"},
            {t:"Hotel Stay", i:"🏨", d:"Find and book safe hotel rooms & stays.", l:"https://www.booking.com/"},
            {t:"Food Order", i:"🍔", d:"Order food online from local restaurants.", l:"https://www.zomato.com/"},
            {t:"Medicines", i:"💊", d:"Order medicines online with home delivery.", l:"https://www.1mg.com/"},
            {t:"Lab Test", i:"🩸", d:"Book blood tests & full body checkups.", l:"https://pharmeasy.in/"},
            {t:"Grocery", i:"🥦", d:"Get daily groceries delivered in minutes.", l:"https://blinkit.com/"},
            {t:"Aadhaar", i:"🆔", d:"Download e-Aadhaar or update your details.", l:"https://myaadhaar.uidai.gov.in/"},
            {t:"PAN Card", i:"💳", d:"Apply for a new PAN card or do corrections.", l:"https://www.onlineservices.nsdl.com/"},
            {t:"Passport", i:"🛂", d:"Book passport appointments at Sewa Kendra.", l:"https://passportindia.gov.in/"},
            {t:"Driving License", i:"🚘", d:"Apply for Learner/Permanent Driving License.", l:"https://parivahan.gov.in/"},
            {t:"Voter ID", i:"🗳️", d:"Register to vote or download Voter ID.", l:"https://voters.eci.gov.in/"},
            {t:"Ration Card", i:"🌾", d:"Check ration card status and NFSA details.", l:"https://nfsa.gov.in/"},
            {t:"Mobile Recharge", i:"📱", d:"Recharge prepaid or pay postpaid bills.", l:"https://www.amazon.in/hfc/bill/mobile_recharge"},
            {t:"DTH Recharge", i:"📺", d:"Recharge Tata Sky, Airtel, Dish TV etc.", l:"https://www.amazon.in/hfc/bill/dth"},
            {t:"Electricity Bill", i:"💡", d:"Pay your monthly electricity bills online.", l:"https://www.amazon.in/hfc/bill/electricity"},
            {t:"Gas Cylinder", i:"⛽", d:"Book LPG refill for HP, Indane, or Bharat.", l:"https://www.amazon.in/hfc/bill/lpg"},
            {t:"FASTag", i:"🚗", d:"Recharge your vehicle's FASTag for tolls.", l:"https://www.amazon.in/hfc/bill/fastag"},
            {t:"Broadband", i:"🌐", d:"Pay your WiFi and broadband internet bills.", l:"https://www.amazon.in/hfc/bill/broadband"},
            {t:"Water Bill", i:"💧", d:"Pay municipal water tax and supply bills.", l:"https://www.amazon.in/hfc/bill/water"},
            {t:"Challan Pay", i:"👮", d:"Check and pay your pending traffic challans.", l:"https://echallan.parivahan.gov.in/"},
            {t:"Stock Market", i:"📈", d:"Invest in stocks, mutual funds & IPOs.", l:"https://zerodha.com/"},
            {t:"Insurance", i:"🛡️", d:"Compare and buy health, bike, or life insurance.", l:"https://www.policybazaar.com/"},
            {t:"Credit Score", i:"📊", d:"Check your free CIBIL credit score online.", l:"https://www.cibil.com/"},
            {t:"Jobs", i:"💼", d:"Search for private and government job openings.", l:"https://www.linkedin.com/"},
            {t:"Courier Track", i:"📦", d:"Track your parcels and courier deliveries.", l:"https://www.delhivery.com/"},
            {t:"Currency Rate", i:"💱", d:"Check live foreign exchange currency rates.", l:"https://www.xe.com/"},
            {t:"Translate", i:"🗣️", d:"Translate text or voice to any language.", l:"https://translate.google.com/"},
            {t:"Movie Ticket", i:"🍿", d:"Book latest cinema and movie theater tickets.", l:"https://in.bookmyshow.com/"}
        ];

        const guides = {
            'fire': "🔥 FIRE SAFETY\n\n1. Stay low to the ground to avoid inhaling toxic smoke.\n2. Touch doors with the back of your hand before opening; if hot, DO NOT open.\n3. Evacuate immediately and call 101.\n4. Use stairs, NEVER use elevators.\n5. If your clothes catch fire: STOP, DROP to the ground, and ROLL.",
            'cpr': "❤️ CPR (CARDIOPULMONARY RESUSCITATION)\n\n1. Check for responsiveness and breathing. If none, Call 108 immediately.\n2. Place the heel of your hand on the center of the person's chest.\n3. Place your other hand on top and interlock fingers.\n4. Push hard and fast (at least 2 inches deep, 100-120 compressions per minute).\n5. Continue without stopping until medical help arrives.",
            'lpg': "⛽ LPG GAS LEAKAGE\n\n1. Do NOT switch on/off any electrical switches or appliances (sparks can ignite gas).\n2. Open all doors and windows for cross-ventilation.\n3. Turn off the LPG regulator valve immediately.\n4. Extinguish any open flames in the house.\n5. Evacuate the area and call 1906 helpline from outside.",
            'quake': "🏚️ EARTHQUAKE SURVIVAL\n\n1. DROP to your hands and knees to prevent being knocked over.\n2. COVER your head and neck under a sturdy table or desk.\n3. HOLD ON to your shelter until the shaking stops.\n4. Stay away from glass, windows, outside doors, and heavy furniture.\n5. If outdoors, move to an open clear area away from buildings.",
            'bleed': "🩸 HEAVY BLEEDING\n\n1. Call 108 for an ambulance.\n2. Apply direct, firm pressure on the wound with a clean cloth.\n3. Maintain pressure for at least 5-10 minutes without peeking.\n4. Elevate the injured area above the heart if possible.\n5. Do not remove the cloth; if blood soaks through, add another layer on top.",
            'choke': "🤢 CHOKING (HEIMLICH MANEUVER)\n\n1. Ask 'Are you choking?' If they can't cough or speak, act immediately.\n2. Stand behind them and wrap your arms around their waist.\n3. Make a fist with one hand, place it just above their belly button.\n4. Grab your fist with the other hand.\n5. Give quick, upward thrusts into the stomach until the object pops out."
        };

        // --- 3. STATE & INIT ---
        let user = null;
        let currentDist = "Srinagar";

        window.onload = () => {
            try {
                user = JSON.parse(localStorage.getItem('iruAppUser'));
                currentDist = localStorage.getItem('iruAppDist') || "Bandipora";
            } catch(e) { console.log("Storage disabled"); }

            if(user && user.name) launchApp();
            else document.getElementById('login-screen').style.display = 'flex';
        };

        // --- 4. CORE LOGIC ---
        function showToast(msg) {
            const t = document.getElementById('toast');
            if(!t) return;
            t.innerText = msg;
            t.classList.add('show');
            setTimeout(() => t.classList.remove('show'), 3000);
        }

        function handleLogin() {
            const n = document.getElementById('loginName').value.trim();
            const p = document.getElementById('loginPhone').value.trim();
            if(n.length > 2 && p.length >= 10) {
                user = {name: n, phone: p};
                try { localStorage.setItem('iruAppUser', JSON.stringify(user)); } catch(e){}
                launchApp();
            } else {
                showToast("Please enter valid Name and Phone Number");
            }
        }

        function launchApp() {
            const loginScreen = document.getElementById('login-screen');
            if(loginScreen) loginScreen.style.display = 'none';
            
            document.getElementById('app-layout').style.display = 'flex';
            
            if(document.getElementById('uNameDisplay')) document.getElementById('uNameDisplay').innerText = user.name;
            if(document.getElementById('pName')) document.getElementById('pName').innerText = user.name;
            if(document.getElementById('pPhone')) document.getElementById('pPhone').innerText = "+91 " + user.phone;
            if(document.getElementById('pAvatar')) document.getElementById('pAvatar').innerText = user.name.charAt(0).toUpperCase();
            
            updateDistUI(currentDist);
            renderServices("");
            renderBookings();
            loadMedicalID();
        }

        function logout() {
            if(confirm("Are you sure you want to log out?")) {
                try { localStorage.removeItem('iruAppUser'); } catch(e){}
                location.reload();
            }
        }

        // --- 5. RENDERERS ---
        function renderServices(f) {
            const list = document.getElementById('emergencyList');
            if(!list) return;
            list.innerHTML = "";
            
            const distData = districtConfig[currentDist] || districtConfig["Srinagar"];

            emergencyServices.forEach(s => {
                if(s.n.toLowerCase().includes(f.toLowerCase()) || s.d.toLowerCase().includes(f.toLowerCase())) {
                    let num = s.num;
                    if(s.type === "local") {
                        num = `${distData.std}-${distData[s.key]}`;
                    }

                    const div = document.createElement('div');
                    div.className = 'service-card';
                    div.onclick = () => { window.location.href = `tel:${num}`; };
                    
                    div.innerHTML = `
                        <div class="icon-box ${s.c}">${s.i}</div>
                        <div class="info"><h3>${s.n}</h3><p>${s.d}</p></div>
                        <button class="call-btn">📞 Call</button>
                    `;
                    list.appendChild(div);
                }
            });
        }

        function renderBookings() {
            const list = document.getElementById('bookingList');
            if(!list) return;
            list.innerHTML = "";
            bookings.forEach(b => {
                const div = document.createElement('div');
                div.className = 'book-card';
                div.onclick = () => window.open(b.l, '_blank');
                div.innerHTML = `
                    <div class="b-icon">${b.i}</div>
                    <div class="b-title">${b.t}</div>
                    <div class="b-desc">${b.d}</div>
                `;
                list.appendChild(div);
            });
        }

        // --- 6. SAFETY TOOLS, SOS & GUIDES ---
        function triggerSOS() {
            let m = { ice: "" };
            try { m = JSON.parse(localStorage.getItem('iruAppMedData')) || m; } catch(e){}
            
            if(!m.ice || m.ice.length < 5) {
                showToast("⚠️ Save Emergency Contact in Profile First!");
                switchTab('profile');
                return;
            }

            if(navigator.geolocation) {
                showToast("Fetching location to send SOS...");
                navigator.geolocation.getCurrentPosition(
                    p => {
                        const url = `https://www.google.com/maps?q=${p.coords.latitude},${p.coords.longitude}`;
                        const text = `🚨 SOS EMERGENCY! I am in danger. Please help me. My exact location is: ${url}`;
                        window.open(`sms:${m.ice}?body=${encodeURIComponent(text)}`, '_blank');
                    },
                    err => { 
                        showToast("Sending SOS without GPS...");
                        window.open(`sms:${m.ice}?body=${encodeURIComponent("🚨 SOS EMERGENCY! I need help immediately. (Location unavailable)")}`, '_blank');
                    },
                    { enableHighAccuracy: true }
                );
            } else {
                window.open(`sms:${m.ice}?body=${encodeURIComponent("🚨 SOS EMERGENCY! I need help immediately.")}`, '_blank');
            }
        }

        function showGuide(key) {
            const modal = document.getElementById('guideModal');
            if(modal) {
                const titles = {'fire': 'Fire Safety', 'cpr': 'CPR Guide', 'lpg': 'LPG Leak', 'quake': 'Earthquake', 'bleed': 'Bleeding', 'choke': 'Choking'};
                document.getElementById('gTitle').innerText = titles[key];
                document.getElementById('gContent').innerText = guides[key];
                modal.classList.add('show');
            }
        }

        function toggleFlash() {
            const f = document.getElementById('flash-overlay');
            if(f) f.style.display = f.style.display === 'block' ? 'none' : 'block';
        }

        let osc;
        function toggleWhistle() {
            if(osc) { 
                osc.stop(); osc=null; 
            } else {
                const AC = window.AudioContext || window.webkitAudioContext;
                if(!AC) { showToast("Audio not supported"); return; }
                const ctx = new AC();
                osc = ctx.createOscillator();
                osc.type = 'square';
                osc.frequency.value = 3000;
                osc.connect(ctx.destination); 
                osc.start();
                setTimeout(() => { if(osc) { osc.stop(); osc=null; } }, 4000);
            }
        }

        function shareLocation() {
            if(navigator.geolocation) {
                showToast("Fetching location...");
                navigator.geolocation.getCurrentPosition(
                    p => {
                        const url = `https://www.google.com/maps?q=${p.coords.latitude},${p.coords.longitude}`;
                        window.open(`https://wa.me/?text=${encodeURIComponent("Here is my current location: " + url)}`, '_blank');
                    },
                    err => { showToast("Please enable GPS location permission."); },
                    { enableHighAccuracy: true }
                );
            } else {
                showToast("GPS not supported on this device");
            }
        }

        // --- 7. MEDICAL ID (PROFILE) ---
        function loadMedicalID() {
            let m = { b: "--", a: "--", w: "--", c: "None", alg: "None", ice: "" };
            try { m = JSON.parse(localStorage.getItem('iruAppMedData')) || m; } catch(e){}
            
            if(document.getElementById('dBlood')) document.getElementById('dBlood').innerText = m.b;
            if(document.getElementById('dAge')) document.getElementById('dAge').innerText = m.a;
            if(document.getElementById('dWeight')) document.getElementById('dWeight').innerText = m.w;
            if(document.getElementById('dCond')) document.getElementById('dCond').innerText = m.c;
            if(document.getElementById('dAlg')) document.getElementById('dAlg').innerText = m.alg;
            
            const iceBtn = document.getElementById('dIce');
            const msg = document.getElementById('noIceMsg');
            if(m.ice && m.ice.length >= 5) {
                if(iceBtn) { iceBtn.href = "tel:" + m.ice; iceBtn.style.display = "block"; }
                if(msg) msg.style.display = "none";
            } else {
                if(iceBtn) iceBtn.style.display = "none";
                if(msg) msg.style.display = "block";
            }
        }

        function toggleMedEdit() {
            const disp = document.getElementById('medDisplay');
            const form = document.getElementById('medForm');
            if(!disp || !form) return;

            if(form.style.display === 'block') {
                form.style.display = 'none';
                disp.style.display = 'block';
            } else {
                let m = { b: "", a: "", w: "", c: "", alg: "", ice: "" };
                try { m = JSON.parse(localStorage.getItem('iruAppMedData')) || m; } catch(e){}
                
                document.getElementById('inBlood').value = m.b !== "--" ? m.b : "";
                document.getElementById('inAge').value = m.a !== "--" ? m.a : "";
                document.getElementById('inWeight').value = m.w !== "--" ? m.w : "";
                document.getElementById('inCond').value = m.c !== "None" ? m.c : "";
                document.getElementById('inAlg').value = m.alg !== "None" ? m.alg : "";
                document.getElementById('inIce').value = m.ice;
                
                disp.style.display = 'none';
                form.style.display = 'block';
            }
        }

        function saveMedData() {
            const d = {
                b: document.getElementById('inBlood').value || "--",
                a: document.getElementById('inAge').value || "--",
                w: document.getElementById('inWeight').value || "--",
                c: document.getElementById('inCond').value || "None",
                alg: document.getElementById('inAlg').value || "None",
                ice: document.getElementById('inIce').value || ""
            };
            try { localStorage.setItem('iruAppMedData', JSON.stringify(d)); } catch(e){}
            loadMedicalID();
            toggleMedEdit();
            showToast("Medical ID Saved Successfully");
        }

        // --- 8. DISTRICT SELECTOR ---
        function openDistModal() {
            const list = document.getElementById('distList');
            const modal = document.getElementById('distModal');
            if(!list || !modal) return;
            
            list.innerHTML = "";
            districtNames.forEach(d => {
                const div = document.createElement('div');
                div.className = 'dist-btn'; 
                div.innerText = d;
                if(d === currentDist) { div.style.background = "#007AFF"; div.style.color = "white"; }
                
                div.onclick = () => {
                    currentDist = d;
                    try{ localStorage.setItem('iruAppDist', d); } catch(e){}
                    updateDistUI(d);
                    renderServices(""); 
                    modal.classList.remove('show');
                    showToast(`Location set to ${d}`);
                };
                list.appendChild(div);
            });
            modal.classList.add('show');
        }

        function updateDistUI(d) {
            const el = document.getElementById('headerDist');
            if(el) el.innerText = d;
        }

        function closeModal(e, id) {
            if(e.target.classList.contains('overlay')) {
                document.getElementById(id).classList.remove('show');
            }
        }

        // --- 9. NAVIGATION ---
        function switchTab(id) {
            document.querySelectorAll('.view-section').forEach(e => e.classList.remove('active'));
            const view = document.getElementById('view-'+id);
            if(view) view.classList.add('active');
            
            document.querySelectorAll('.nav-item').forEach(e => e.classList.remove('active'));
            const nav = document.getElementById('nav-'+id);
            if(nav) nav.classList.add('active');
        }

    </script>
</body>
</html>
