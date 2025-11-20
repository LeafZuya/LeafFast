<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LeafFast - Klik Cepat!</title>
    
    <!-- Firebase -->
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            min-height: 100vh;
            background: linear-gradient(135deg, #00b4db 0%, #0083b0 25%, #00c853 50%, #7cb342 75%, #43a047 100%);
            background-size: 400% 400%;
            animation: gradientShift 8s ease infinite;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .container {
            width: 100%;
            max-width: 1200px;
            display: grid;
            grid-template-columns: 1fr 400px;
            gap: 30px;
            align-items: start;
        }

        .main-content {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
        }

        .sidebar {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            backdrop-filter: blur(10px);
        }

        .screen {
            display: none;
        }

        .active {
            display: block;
        }

        /* Header Styles */
        .header {
            text-align: center;
            margin-bottom: 40px;
        }

        .logo {
            font-size: 4rem;
            margin-bottom: 10px;
        }

        .title {
            font-size: 3rem;
            background: linear-gradient(135deg, #00c853, #0077b6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 10px;
            font-weight: 800;
        }

        .subtitle {
            color: #666;
            font-size: 1.2rem;
            margin-bottom: 30px;
        }

        /* Buttons */
        .btn-primary {
            background: linear-gradient(135deg, #00c853, #0077b6);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.1rem;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            width: 100%;
            box-shadow: 0 5px 15px rgba(0, 200, 83, 0.4);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(0, 200, 83, 0.6);
        }

        .btn-primary:active {
            transform: translateY(0);
        }

        .btn-secondary {
            background: #6c757d;
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1rem;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            margin-top: 10px;
        }

        .btn-secondary:hover {
            background: #5a6268;
            transform: translateY(-2px);
        }

        /* Stats Cards */
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: linear-gradient(135deg, #00c853, #0077b6);
            color: white;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
        }

        .stat-value {
            font-size: 2rem;
            font-weight: 800;
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        /* Leaderboard Preview */
        .leaderboard-preview {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
        }

        .preview-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 15px;
            color: #333;
            text-align: center;
        }

        .preview-item {
            display: flex;
            align-items: center;
            padding: 10px;
            border-bottom: 1px solid #e9ecef;
            position: relative;
        }

        .preview-item:last-child {
            border-bottom: none;
        }

        .preview-rank {
            font-weight: 700;
            width: 30px;
            color: #00c853;
            font-size: 0.9rem;
        }

        .preview-avatar-container {
            position: relative;
            margin: 0 10px;
        }

        .preview-avatar {
            font-size: 1.2rem;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
            display: flex;
            align-items: center;
            justify-content: center;
            background: white;
        }

        .preview-frame {
            position: absolute;
            top: -3px;
            left: -3px;
            right: -3px;
            bottom: -3px;
            border-radius: 50%;
            pointer-events: none;
        }

        .preview-info {
            flex: 1;
        }

        .preview-name {
            font-weight: 600;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .preview-score {
            color: #666;
            font-size: 0.8rem;
        }

        .frame-badge {
            font-size: 0.7rem;
            padding: 2px 6px;
            border-radius: 8px;
            background: #f8f9fa;
            border: 1px solid #dee2e6;
        }

        /* Registration & Profile Edit */
        .avatar-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            margin: 25px 0;
        }

        .avatar-option {
            width: 70px;
            height: 70px;
            font-size: 2rem;
            border: 3px solid #e9ecef;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            background: white;
            transition: all 0.3s ease;
        }

        .avatar-option:hover {
            transform: scale(1.1);
            border-color: #00c853;
        }

        .avatar-option.selected {
            border-color: #00c853;
            background: #f0fff4;
            transform: scale(1.1);
        }

        .custom-avatar-option {
            position: relative;
            overflow: hidden;
        }

        .custom-avatar-option input {
            position: absolute;
            width: 100%;
            height: 100%;
            opacity: 0;
            cursor: pointer;
        }

        .custom-avatar-preview {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            object-fit: cover;
            background: #f8f9fa;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            color: #666;
        }

        .form-group {
            margin-bottom: 25px;
        }

        .form-input {
            width: 100%;
            padding: 15px 20px;
            font-size: 1rem;
            border: 2px solid #e9ecef;
            border-radius: 12px;
            transition: all 0.3s ease;
        }

        .form-input:focus {
            outline: none;
            border-color: #00c853;
            box-shadow: 0 0 0 3px rgba(0, 200, 83, 0.1);
        }

        /* Game Screen */
        .game-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .click-area {
            text-align: center;
            margin: 40px 0;
        }

        .click-button {
            background: none;
            border: none;
            width: 200px;
            height: 200px;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.1s ease;
            padding: 0;
            overflow: hidden;
        }

        .click-button img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 50%;
            transition: transform 0.1s ease;
        }

        .click-button:hover img {
            transform: scale(1.05);
        }

        .click-button:active img {
            transform: scale(0.95);
        }

        .click-button:disabled {
            cursor: not-allowed;
            opacity: 0.7;
        }

        .game-stats {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin: 30px 0;
        }

        .game-stat {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
        }

        .game-stat-value {
            font-size: 2.5rem;
            font-weight: 800;
            color: #333;
        }

        .game-stat-label {
            color: #666;
            font-size: 0.9rem;
            margin-top: 5px;
        }

        #timer {
            font-size: 3rem;
            font-weight: 800;
            color: #00c853;
            text-align: center;
            margin: 20px 0;
        }

        /* Real-time Updates */
        .score-update {
            animation: scorePop 0.3s ease;
        }

        @keyframes scorePop {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        /* Auto-save Indicator */
        .auto-save-indicator {
            background: #d4edda;
            border: 1px solid #c3e6cb;
            border-radius: 10px;
            padding: 10px 15px;
            margin: 15px 0;
            text-align: center;
            color: #155724;
            font-size: 0.9rem;
        }

        /* Profile Section */
        .profile-section {
            background: #e8f5e8;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
        }

        .profile-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 15px;
        }

        .profile-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            object-fit: cover;
            font-size: 2rem;
            display: flex;
            align-items: center;
            justify-content: center;
            background: white;
            position: relative;
        }

        /* Frame Styles */
        .frame-basic {
            border: 3px solid #95a5a6;
        }

        .frame-bronze {
            border: 4px solid #cd7f32;
            box-shadow: 0 0 10px #cd7f32;
        }

        .frame-silver {
            border: 5px solid #c0c0c0;
            box-shadow: 0 0 15px #c0c0c0;
        }

        .frame-gold {
            border: 6px solid #ffd700;
            box-shadow: 0 0 20px #ffd700;
        }

        .frame-platinum {
            border: 7px solid #e5e4e2;
            box-shadow: 0 0 25px #e5e4e2, 0 0 15px #b9f2ff;
        }

        .frame-diamond {
            border: 8px solid #b9f2ff;
            box-shadow: 0 0 30px #b9f2ff, 0 0 20px #7fffd4;
            animation: diamondGlow 2s infinite;
        }

        .frame-master {
            border: 9px solid #ff6b6b;
            box-shadow: 0 0 35px #ff6b6b, 0 0 25px #ffa500;
            animation: masterGlow 1.5s infinite;
        }

        .frame-legend {
            border: 10px solid #9b59b6;
            box-shadow: 0 0 40px #9b59b6, 0 0 30px #e74c3c;
            animation: legendGlow 1s infinite;
        }

        .frame-god {
            border: 12px solid #f1c40f;
            box-shadow: 0 0 50px #f1c40f, 0 0 35px #e74c3c, 0 0 25px #9b59b6;
            animation: godGlow 0.8s infinite;
        }

        .frame-titan {
            border: 15px solid #e74c3c;
            box-shadow: 0 0 60px #e74c3c, 0 0 45px #f1c40f, 0 0 30px #9b59b6;
            animation: titanGlow 0.5s infinite;
            background: linear-gradient(45deg, #e74c3c, #f1c40f, #9b59b6);
            background-size: 300% 300%;
            animation: titanBackground 3s infinite;
        }

        @keyframes diamondGlow {
            0%, 100% { box-shadow: 0 0 30px #b9f2ff, 0 0 20px #7fffd4; }
            50% { box-shadow: 0 0 40px #b9f2ff, 0 0 30px #7fffd4, 0 0 20px #00ffff; }
        }

        @keyframes masterGlow {
            0%, 100% { box-shadow: 0 0 35px #ff6b6b, 0 0 25px #ffa500; }
            50% { box-shadow: 0 0 45px #ff6b6b, 0 0 35px #ffa500, 0 0 25px #ff0000; }
        }

        @keyframes legendGlow {
            0%, 100% { box-shadow: 0 0 40px #9b59b6, 0 0 30px #e74c3c; }
            50% { box-shadow: 0 0 50px #9b59b6, 0 0 40px #e74c3c, 0 0 30px #ff0000; }
        }

        @keyframes godGlow {
            0%, 100% { box-shadow: 0 0 50px #f1c40f, 0 0 35px #e74c3c, 0 0 25px #9b59b6; }
            50% { box-shadow: 0 0 60px #f1c40f, 0 0 45px #e74c3c, 0 0 35px #9b59b6, 0 0 25px #ffffff; }
        }

        @keyframes titanGlow {
            0%, 100% { box-shadow: 0 0 60px #e74c3c, 0 0 45px #f1c40f, 0 0 30px #9b59b6; }
            50% { box-shadow: 0 0 70px #e74c3c, 0 0 55px #f1c40f, 0 0 40px #9b59b6, 0 0 30px #ffffff; }
        }

        @keyframes titanBackground {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        .profile-info {
            flex: 1;
        }

        .profile-name {
            font-weight: 700;
            font-size: 1.2rem;
            color: #2e7d32;
        }

        .profile-id {
            font-size: 0.8rem;
            color: #666;
        }

        /* Welcome Message */
        .welcome-message {
            background: linear-gradient(135deg, #e8f5e8, #e6f7ff);
            border-radius: 15px;
            padding: 25px;
            text-align: center;
            margin-bottom: 30px;
            border: 2px solid #00c853;
        }

        .welcome-icon {
            font-size: 3rem;
            margin-bottom: 15px;
        }

        .welcome-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: #2e7d32;
            margin-bottom: 10px;
        }

        .welcome-desc {
            color: #666;
            font-size: 1rem;
            line-height: 1.5;
        }

        /* Achievement Notification */
        .achievement-notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: linear-gradient(135deg, #ffd700, #ff6b6b);
            color: white;
            padding: 20px 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            z-index: 1000;
            text-align: center;
            animation: achievementPop 0.5s ease;
            display: none;
        }

        @keyframes achievementPop {
            0% { transform: translateX(-50%) translateY(-100px); opacity: 0; }
            70% { transform: translateX(-50%) translateY(10px); opacity: 1; }
            100% { transform: translateX(-50%) translateY(0); opacity: 1; }
        }

        .achievement-icon {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        .achievement-title {
            font-size: 1.3rem;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .achievement-desc {
            font-size: 1rem;
            opacity: 0.9;
        }

        /* Frames Gallery */
        .frames-gallery {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin: 20px 0;
        }

        .frame-item {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            border: 2px solid #e9ecef;
        }

        .frame-preview {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            margin: 0 auto 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            background: white;
        }

        .frame-name {
            font-weight: 600;
            font-size: 0.9rem;
            margin-bottom: 5px;
        }

        .frame-requirement {
            font-size: 0.8rem;
            color: #666;
        }

        .frame-locked {
            opacity: 0.6;
            position: relative;
        }

        .frame-locked::after {
            content: "🔒";
            position: absolute;
            top: 5px;
            right: 5px;
            font-size: 1rem;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .container {
                grid-template-columns: 1fr;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
            }

            .frames-gallery {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <!-- Achievement Notification -->
    <div id="achievementNotification" class="achievement-notification">
        <div class="achievement-icon">🏆</div>
        <div class="achievement-title" id="achievementTitle">Achievement Unlocked!</div>
        <div class="achievement-desc" id="achievementDesc">You reached a new milestone!</div>
    </div>

    <div class="container">
        <!-- Main Content Area -->
        <div class="main-content">
            <!-- Screen 1: Main Menu -->
            <div id="modeScreen" class="screen active">
                <div class="header">
                    <div class="logo">🍃</div>
                    <h1 class="title">LeafFast</h1>
                    <p class="subtitle">Klik sebanyak-banyaknya dan kumpulkan bingkai legendaris!</p>
                </div>

                <!-- Welcome Message -->
                <div class="welcome-message">
                    <div class="welcome-icon">🌍</div>
                    <div class="welcome-title">Mode Global + Achievement System</div>
                    <div class="welcome-desc">
                        Setiap milestone yang kamu capai akan membuka bingkai eksklusif! 
                        Dari Basic sampai Titan Frame yang berapi-api!
                    </div>
                </div>

                <button class="btn-primary" id="playButton">
                    🎮 MULAI KLIK!
                </button>

                <!-- Edit Profile Button -->
                <button class="btn-secondary" id="editProfileButton" style="display: none;">
                    ✏️ EDIT PROFIL & BINGKAI
                </button>

                <!-- Auto-save Status -->
                <div id="autoSaveStatus" class="auto-save-indicator" style="display: none;">
                    🔄 Data tersimpan otomatis - Aman untuk refresh/close browser
                </div>
            </div>

            <!-- Screen 2: Registration & Profile Edit -->
            <div id="registerScreen" class="screen">
                <div class="header">
                    <div class="logo">🌍</div>
                    <h1 class="title" id="registerTitle">Buat Profil</h1>
                    <p class="subtitle" id="registerSubtitle">Daftar dan kumpulkan bingkai legendaris!</p>
                </div>

                <div class="form-group">
                    <input type="text" class="form-input" id="username" placeholder="Masukkan nama panggilan kamu" maxlength="15">
                </div>

                <h3 class="preview-title">Pilih Avatar Kamu</h3>
                <div class="avatar-grid" id="avatarSelection">
                    <!-- Avatars will be generated by JavaScript -->
                </div>

                <!-- Frames Gallery -->
                <h3 class="preview-title" style="margin-top: 30px;">🎨 Gallery Bingkai</h3>
                <div class="frames-gallery" id="framesGallery">
                    <!-- Frames will be generated by JavaScript -->
                </div>

                <button class="btn-primary" id="registerButton">
                    🚀 SIMPAN PROFIL & MULAI
                </button>
                <button class="btn-primary" style="background: #6c757d; margin-top: 10px;" id="backToMode">
                    ↩ KEMBALI
                </button>
            </div>

            <!-- Screen 3: Game -->
            <div id="gameScreen" class="screen">
                <div class="game-header">
                    <h2 class="title" style="font-size: 2rem;">🍃 LeafFast</h2>
                    <div style="font-size: 1.2rem; color: #0077b6; font-weight: 600;">Mode Global</div>
                </div>

                <div id="timer">∞</div>

                <div class="click-area">
                    <button class="click-button" id="clickButton">
                        <img id="clickButtonImage" src="https://via.placeholder.com/200x200/4CAF50/FFFFFF?text=KLIK" alt="Klik Tombol">
                    </button>
                </div>

                <div class="game-stats">
                    <div class="game-stat">
                        <div class="game-stat-value" id="score">0</div>
                        <div class="game-stat-label">SCORE SAAT INI</div>
                    </div>
                    <div class="game-stat">
                        <div class="game-stat-value" id="globalRank">-</div>
                        <div class="game-stat-label">RANK GLOBAL</div>
                    </div>
                </div>

                <!-- Current Frame Display -->
                <div id="currentFrameDisplay" style="text-align: center; margin: 20px 0; padding: 15px; background: #f8f9fa; border-radius: 10px;">
                    <div style="font-size: 0.9rem; color: #666; margin-bottom: 5px;">Bingkai Saat Ini:</div>
                    <div id="currentFrameName" style="font-weight: 700; font-size: 1.1rem; color: #2e7d32;">Basic Frame</div>
                    <div id="nextFrameInfo" style="font-size: 0.8rem; color: #888;">Next: Bronze Frame at 100 klik</div>
                </div>

                <!-- Auto-save Status in Game -->
                <div id="gameAutoSaveStatus" class="auto-save-indicator">
                    💾 Auto-save aktif - Score terus bertambah!
                </div>

                <div id="message" style="text-align: center; font-size: 1.1rem; color: #666; margin: 20px 0;">
                    Klik tombol hijau untuk mulai menambah score!
                </div>

                <button class="btn-primary" id="backToMenuButton" style="background: #6c757d;">
                    🏠 KEMBALI KE MENU
                </button>
            </div>
        </div>

        <!-- Sidebar -->
        <div class="sidebar">
            <!-- Player Profile -->
            <div id="playerProfile" class="profile-section" style="display: none;">
                <div class="profile-header">
                    <div id="profileAvatar" class="profile-avatar frame-basic">😊</div>
                    <div class="profile-info">
                        <div id="profileName" class="profile-name">Nama Player</div>
                        <div id="profileId" class="profile-id">ID: loading...</div>
                    </div>
                </div>
                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-value" id="globalScore">0</div>
                        <div class="stat-label">TOTAL SCORE</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="sessionClicks">0</div>
                        <div class="stat-label">KLIK SESSION</div>
                    </div>
                </div>
            </div>

            <!-- Leaderboard Preview -->
            <div class="leaderboard-preview">
                <h3 class="preview-title">🏆 TOP 5 GLOBAL</h3>
                <div id="leaderboardPreview">
                    <div class="preview-item">
                        <div class="preview-rank">1</div>
                        <div class="preview-avatar-container">
                            <div class="preview-avatar">😊</div>
                            <div class="preview-frame frame-basic"></div>
                        </div>
                        <div class="preview-info">
                            <div class="preview-name">
                                Pemain Top 1
                                <span class="frame-badge">Basic</span>
                            </div>
                            <div class="preview-score">Loading...</div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Real-time Updates -->
            <div id="realtimeUpdates" style="margin-top: 20px; padding: 15px; background: #fff3cd; border-radius: 10px;">
                <h4 style="color: #856404; margin-bottom: 10px;">🔄 Update Real-time</h4>
                <div id="updateMessage" style="font-size: 0.9rem; color: #856404;">
                    Siap untuk mulai klik!
                </div>
            </div>

            <!-- Quick Tips -->
            <div style="margin-top: 25px; padding: 20px; background: #e8f5e8; border-radius: 12px;">
                <h4 style="color: #2e7d32; margin-bottom: 10px;">💡 Tips Cepat</h4>
                <ul style="color: #666; font-size: 0.9rem; padding-left: 20px;">
                    <li>Gunakan dua jari secara bergantian</li>
                    <li>Fokus dan konsisten</li>
                    <li>Dapatkan bingkai eksklusif setiap milestone!</li>
                    <li>Data tersimpan otomatis</li>
                </ul>
            </div>
        </div>
    </div>

    <script>
        // Firebase Configuration
        const firebaseConfig = {
            apiKey: "AIzaSyAmD8WEyyBVbWAq4cEOJQ8AyUpspPNeosM",
            authDomain: "leaffast-79a4a.firebaseapp.com",
            databaseURL: "https://leaffast-79a4a-default-rtdb.asia-southeast1.firebasedatabase.app",
            projectId: "leaffast-79a4a",
            storageBucket: "leaffast-79a4a.firebasestorage.app",
            messagingSenderId: "668436977384",
            appId: "1:668436977384:web:6024f3c99a17b178515b5d"
        };

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        // Frame System
        const frames = [
            { id: 'basic', name: 'Basic Frame', requirement: 0, class: 'frame-basic', color: '#95a5a6', icon: '⚪', shortName: 'Basic' },
            { id: 'bronze', name: 'Bronze Frame', requirement: 100, class: 'frame-bronze', color: '#cd7f32', icon: '🟤', shortName: 'Bronze' },
            { id: 'silver', name: 'Silver Frame', requirement: 1000, class: 'frame-silver', color: '#c0c0c0', icon: '⚪', shortName: 'Silver' },
            { id: 'gold', name: 'Gold Frame', requirement: 10000, class: 'frame-gold', color: '#ffd700', icon: '🟡', shortName: 'Gold' },
            { id: 'platinum', name: 'Platinum Frame', requirement: 100000, class: 'frame-platinum', color: '#e5e4e2', icon: '⚪', shortName: 'Platinum' },
            { id: 'diamond', name: 'Diamond Frame', requirement: 1000000, class: 'frame-diamond', color: '#b9f2ff', icon: '💎', shortName: 'Diamond' },
            { id: 'master', name: 'Master Frame', requirement: 10000000, class: 'frame-master', color: '#ff6b6b', icon: '🔥', shortName: 'Master' },
            { id: 'legend', name: 'Legend Frame', requirement: 100000000, class: 'frame-legend', color: '#9b59b6', icon: '⚡', shortName: 'Legend' },
            { id: 'god', name: 'God Frame', requirement: 1000000000, class: 'frame-god', color: '#f1c40f', icon: '👑', shortName: 'God' },
            { id: 'titan', name: 'Titan Frame', requirement: 1000000000000, class: 'frame-titan', color: '#e74c3c', icon: '🌋', shortName: 'Titan' }
        ];

        // DOM Elements
        const screens = {
            mode: document.getElementById('modeScreen'),
            register: document.getElementById('registerScreen'),
            game: document.getElementById('gameScreen')
        };

        const clickButton = document.getElementById('clickButton');
        const clickButtonImage = document.getElementById('clickButtonImage');
        const playButton = document.getElementById('playButton');
        const registerButton = document.getElementById('registerButton');
        const editProfileButton = document.getElementById('editProfileButton');
        const backToMenuButton = document.getElementById('backToMenuButton');
        const backToMode = document.getElementById('backToMode');
        const registerTitle = document.getElementById('registerTitle');
        const registerSubtitle = document.getElementById('registerSubtitle');
        const achievementNotification = document.getElementById('achievementNotification');
        const achievementTitle = document.getElementById('achievementTitle');
        const achievementDesc = document.getElementById('achievementDesc');
        const framesGallery = document.getElementById('framesGallery');
        const currentFrameDisplay = document.getElementById('currentFrameDisplay');
        const currentFrameName = document.getElementById('currentFrameName');
        const nextFrameInfo = document.getElementById('nextFrameInfo');
        
        const scoreDisplay = document.getElementById('score');
        const globalRank = document.getElementById('globalRank');
        const messageDisplay = document.getElementById('message');
        const usernameInput = document.getElementById('username');
        const avatarSelection = document.getElementById('avatarSelection');
        const realtimeUpdates = document.getElementById('realtimeUpdates');
        const updateMessage = document.getElementById('updateMessage');
        const autoSaveStatus = document.getElementById('autoSaveStatus');
        const gameAutoSaveStatus = document.getElementById('gameAutoSaveStatus');
        const playerProfile = document.getElementById('playerProfile');
        const profileAvatar = document.getElementById('profileAvatar');
        const profileName = document.getElementById('profileName');
        const profileId = document.getElementById('profileId');
        const globalScore = document.getElementById('globalScore');
        const sessionClicks = document.getElementById('sessionClicks');

        // Game Variables
        let score = 0;
        let sessionScore = 0;
        let gameActive = false;
        let selectedAvatar = '😊';
        let customAvatar = null;
        let playerId = localStorage.getItem('playerId') || generatePlayerId();
        let playerName = localStorage.getItem('playerName') || '';
        let lastUpdateTime = 0;
        const UPDATE_INTERVAL = 1000; // Lebih cepat untuk responsivitas
        let autoSaveInterval;
        let isEditingProfile = false;
        let unlockedFrames = JSON.parse(localStorage.getItem('unlockedFrames') || '["basic"]');
        let currentFrame = localStorage.getItem('currentFrame') || 'basic';
        let lastSaveTime = 0;
        const QUICK_SAVE_INTERVAL = 500; // Save setiap 500ms untuk leaderboard

        // Avatar Options
        const avatars = ['😊', '😎', '🤠', '🧐', '🤩', '😍', '🥳', '🤖', '🐱', '🦊', '🐶', '🐼'];

        // Sound Effect
        const clickSound = new Audio("Hey.mp3");
        
        // Tombol gambar (gunakan placeholder, ganti dengan URL gambar kamu)
        const buttonImages = {
            normal: "Bahlil.jpg",
            pressed: "BahlilB.jpg"
        };

        // Initialize - Load saved data dengan lebih baik
        function initializeGameData() {
            playerId = localStorage.getItem('playerId') || generatePlayerId();
            playerName = localStorage.getItem('playerName') || '';
            selectedAvatar = localStorage.getItem('playerAvatar') || '😊';
            customAvatar = localStorage.getItem('customAvatar');
            unlockedFrames = JSON.parse(localStorage.getItem('unlockedFrames') || '["basic"]');
            currentFrame = localStorage.getItem('currentFrame') || 'basic';
            
            // Load score dari localStorage dengan fallback ke Firebase
            const savedScore = localStorage.getItem('playerScore');
            if (savedScore !== null) {
                score = parseInt(savedScore) || 0;
            }

            // Load global score dari Firebase sebagai backup
            loadGlobalScore();

            // Jika user sudah terdaftar, show profile
            if (playerName) {
                showPlayerProfile();
                autoSaveStatus.style.display = 'block';
                autoSaveStatus.textContent = `✅ Profil tersimpan: ${playerName} - Score: ${formatNumber(score)}`;
                editProfileButton.style.display = 'block';
            }

            generateAvatarOptions();
            generateFramesGallery();
            updateCurrentFrameDisplay();
            updateAllDisplays();
            
            // Set tombol gambar normal
            clickButtonImage.src = buttonImages.normal;
            
            console.log('Game initialized - Score:', score, 'Player:', playerName);
        }

        // Update semua display sekaligus
        function updateAllDisplays() {
            scoreDisplay.textContent = formatNumber(score);
            globalScore.textContent = formatNumber(score);
            sessionClicks.textContent = formatNumber(sessionScore);
            updateCurrentFrameDisplay();
        }

        // Generate frames gallery
        function generateFramesGallery() {
            framesGallery.innerHTML = '';
            frames.forEach(frame => {
                const isUnlocked = unlockedFrames.includes(frame.id);
                const frameItem = document.createElement('div');
                frameItem.className = `frame-item ${isUnlocked ? '' : 'frame-locked'}`;
                frameItem.innerHTML = `
                    <div class="frame-preview ${frame.class}">${frame.icon}</div>
                    <div class="frame-name">${frame.name}</div>
                    <div class="frame-requirement">${formatNumber(frame.requirement)} klik</div>
                `;
                
                if (isUnlocked) {
                    frameItem.style.cursor = 'pointer';
                    frameItem.addEventListener('click', () => {
                        if (frame.id !== currentFrame) {
                            currentFrame = frame.id;
                            localStorage.setItem('currentFrame', currentFrame);
                            updateCurrentFrameDisplay();
                            showPlayerProfile();
                            // Save perubahan frame ke Firebase
                            quickSaveToLeaderboard();
                        }
                    });
                }
                
                framesGallery.appendChild(frameItem);
            });
        }

        // Update current frame display
        function updateCurrentFrameDisplay() {
            const currentFrameData = frames.find(f => f.id === currentFrame);
            const nextFrame = frames.find(f => f.requirement > score && f.requirement > currentFrameData.requirement);
            
            currentFrameName.textContent = currentFrameData.name;
            currentFrameName.style.color = currentFrameData.color;
            
            if (nextFrame) {
                nextFrameInfo.textContent = `Next: ${nextFrame.name} at ${formatNumber(nextFrame.requirement)} klik`;
                nextFrameInfo.style.color = nextFrame.color;
            } else {
                nextFrameInfo.textContent = '🎉 Semua bingkai sudah terbuka!';
                nextFrameInfo.style.color = '#00c853';
            }
        }

        // Check and unlock frames based on score
        function checkFrameUnlocks(newScore) {
            let unlockedNewFrame = false;
            
            frames.forEach(frame => {
                if (newScore >= frame.requirement && !unlockedFrames.includes(frame.id)) {
                    unlockedFrames.push(frame.id);
                    localStorage.setItem('unlockedFrames', JSON.stringify(unlockedFrames));
                    unlockedNewFrame = true;
                    
                    // Auto-equip the highest unlocked frame
                    const highestFrame = frames
                        .filter(f => unlockedFrames.includes(f.id))
                        .reduce((highest, current) => current.requirement > highest.requirement ? current : highest);
                    
                    currentFrame = highestFrame.id;
                    localStorage.setItem('currentFrame', currentFrame);
                    
                    // Show achievement notification
                    showAchievementNotification(frame);
                    
                    // Regenerate gallery
                    generateFramesGallery();
                }
            });
            
            return unlockedNewFrame;
        }

        // Show achievement notification
        function showAchievementNotification(frame) {
            achievementTitle.textContent = `Bingkai Terbuka!`;
            achievementDesc.textContent = `${frame.name} - ${formatNumber(frame.requirement)} klik`;
            achievementNotification.style.display = 'block';
            
            setTimeout(() => {
                achievementNotification.style.display = 'none';
            }, 4000);
        }

        // Load global score dari Firebase
        function loadGlobalScore() {
            if (playerId && playerName) {
                database.ref('leaderboard/' + playerId).once('value')
                    .then((snapshot) => {
                        if (snapshot.exists()) {
                            const playerData = snapshot.val();
                            const firebaseScore = playerData.score || 0;
                            
                            // Gunakan score yang lebih tinggi antara localStorage dan Firebase
                            if (firebaseScore > score) {
                                score = firebaseScore;
                                localStorage.setItem('playerScore', score.toString());
                            }
                            
                            globalScore.textContent = formatNumber(score);
                            
                            // Check frame unlocks
                            checkFrameUnlocks(score);
                            
                            // Update rank
                            updateGlobalRank();
                        }
                    })
                    .catch((error) => {
                        console.log('Firebase load error:', error);
                    });
            }
        }

        // Format number dengan separator
        function formatNumber(num) {
            if (num >= 1000000000) {
                return (num / 1000000000).toFixed(1) + 'M';
            }
            if (num >= 1000000) {
                return (num / 1000000).toFixed(1) + 'Jt';
            }
            if (num >= 1000) {
                return (num / 1000).toFixed(1) + 'K';
            }
            return num.toString();
        }

        // Update global rank - lebih responsif
        function updateGlobalRank() {
            if (!playerName) return;
            
            database.ref('leaderboard').orderByChild('score').once('value')
                .then((snapshot) => {
                    const leaders = [];
                    snapshot.forEach((childSnapshot) => {
                        leaders.push({
                            id: childSnapshot.key,
                            ...childSnapshot.val()
                        });
                    });
                    
                    // Sort by score descending
                    leaders.sort((a, b) => b.score - a.score);
                    
                    // Find current player rank
                    const rank = leaders.findIndex(player => player.id === playerId) + 1;
                    globalRank.textContent = rank > 0 ? `#${rank}` : '-';
                    
                    // Update leaderboard preview
                    updateLeaderboardPreview(leaders.slice(0, 5));
                })
                .catch((error) => {
                    console.log('Rank update error:', error);
                });
        }

        // Update leaderboard preview
        function updateLeaderboardPreview(leaders) {
            const previewElement = document.getElementById('leaderboardPreview');
            previewElement.innerHTML = '';
            
            if (leaders.length === 0) {
                previewElement.innerHTML = '<div style="text-align: center; color: #666; padding: 20px;">Belum ada pemain</div>';
                return;
            }
            
            leaders.forEach((player, index) => {
                const item = document.createElement('div');
                item.className = `preview-item ${player.id === playerId ? 'current-player' : ''}`;
                item.style.background = player.id === playerId ? '#e8f5e8' : 'transparent';
                
                // Get player's frame
                const playerFrame = player.currentFrame || 'basic';
                const frameData = frames.find(f => f.id === playerFrame) || frames[0];
                
                let avatarContent = player.avatar;
                if (player.avatar && player.avatar.startsWith('data:image')) {
                    avatarContent = `<img src="${player.avatar}" style="width: 100%; height: 100%; border-radius: 50%; object-fit: cover;">`;
                } else if (typeof player.avatar === 'string' && player.avatar.length === 2) {
                    // Emoji avatar
                    avatarContent = player.avatar;
                }
                
                item.innerHTML = `
                    <div class="preview-rank">#${index + 1}</div>
                    <div class="preview-avatar-container">
                        <div class="preview-avatar ${frameData.class}">
                            ${avatarContent}
                        </div>
                        <div class="preview-frame ${frameData.class}"></div>
                    </div>
                    <div class="preview-info">
                        <div class="preview-name">
                            ${player.name}
                            <span class="frame-badge" style="background: ${frameData.color}20; color: ${frameData.color}; border-color: ${frameData.color};">${frameData.shortName}</span>
                        </div>
                        <div class="preview-score">${formatNumber(player.score)} klik</div>
                    </div>
                `;
                previewElement.appendChild(item);
            });
        }

        // Show player profile in sidebar
        function showPlayerProfile() {
            playerProfile.style.display = 'block';
            
            // Apply current frame
            const currentFrameData = frames.find(f => f.id === currentFrame);
            profileAvatar.className = `profile-avatar ${currentFrameData.class}`;
            
            if (customAvatar) {
                profileAvatar.innerHTML = `<img src="${customAvatar}" style="width: 100%; height: 100%; border-radius: 50%; object-fit: cover;">`;
            } else {
                profileAvatar.textContent = selectedAvatar;
            }
            
            profileName.textContent = playerName;
            profileId.textContent = `ID: ${playerId.substring(0, 8)}...`;
            globalScore.textContent = formatNumber(score);
        }

        // Generate unique player ID
        function generatePlayerId() {
            const id = 'player_' + Math.random().toString(36).substr(2, 9);
            localStorage.setItem('playerId', id);
            return id;
        }

        // Generate avatar selection dengan custom option
        function generateAvatarOptions() {
            avatarSelection.innerHTML = '';
            
            // Custom avatar option
            const customOption = document.createElement('div');
            customOption.className = 'avatar-option custom-avatar-option';
            customOption.innerHTML = `
                <div class="custom-avatar-preview" id="customAvatarPreview">
                    📷
                </div>
                <input type="file" id="customAvatarInput" accept="image/*">
            `;
            customOption.addEventListener('click', () => {
                document.querySelectorAll('.avatar-option').forEach(opt => {
                    opt.classList.remove('selected');
                });
                customOption.classList.add('selected');
                selectedAvatar = 'custom';
            });
            avatarSelection.appendChild(customOption);

            // Setup custom avatar upload
            const customAvatarInput = document.getElementById('customAvatarInput');
            const customAvatarPreview = document.getElementById('customAvatarPreview');
            
            customAvatarInput.addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        customAvatar = e.target.result;
                        customAvatarPreview.innerHTML = `<img src="${customAvatar}" style="width: 100%; height: 100%; border-radius: 50%; object-fit: cover;">`;
                        localStorage.setItem('customAvatar', customAvatar);
                    };
                    reader.readAsDataURL(file);
                }
            });

            // Load existing custom avatar
            if (customAvatar) {
                customAvatarPreview.innerHTML = `<img src="${customAvatar}" style="width: 100%; height: 100%; border-radius: 50%; object-fit: cover;">`;
            }

            // Emoji avatars
            avatars.forEach(avatar => {
                const avatarDiv = document.createElement('div');
                avatarDiv.className = 'avatar-option';
                if (avatar === selectedAvatar && !customAvatar) {
                    avatarDiv.classList.add('selected');
                }
                avatarDiv.textContent = avatar;
                avatarDiv.addEventListener('click', () => {
                    document.querySelectorAll('.avatar-option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    avatarDiv.classList.add('selected');
                    selectedAvatar = avatar;
                    customAvatar = null;
                    localStorage.setItem('playerAvatar', avatar);
                    localStorage.removeItem('customAvatar');
                });
                avatarSelection.appendChild(avatarDiv);
            });
        }

        // Screen management
        function showScreen(screenName) {
            Object.values(screens).forEach(screen => {
                screen.classList.remove('active');
            });
            screens[screenName].classList.add('active');
        }

        // Play button click - langsung ke game
        playButton.addEventListener('click', () => {
            if (playerName) {
                // Langsung ke game dengan continue score
                showScreen('game');
                initializeGame();
            } else {
                showScreen('register');
            }
        });

        // Edit profile button
        editProfileButton.addEventListener('click', () => {
            isEditingProfile = true;
            registerTitle.textContent = '✏️ Edit Profil & Bingkai';
            registerSubtitle.textContent = 'Ubah nama, avatar, dan pilih bingkai favorit';
            usernameInput.value = playerName;
            showScreen('register');
        });

        // Register/Save profile button
        registerButton.addEventListener('click', () => {
            const username = usernameInput.value.trim();
            if (username.length < 2) {
                alert('Nama harus minimal 2 karakter!');
                return;
            }
            playerName = username;
            
            // Save to localStorage
            localStorage.setItem('playerName', playerName);
            if (selectedAvatar !== 'custom') {
                localStorage.setItem('playerAvatar', selectedAvatar);
            }
            
            showPlayerProfile();
            autoSaveStatus.style.display = 'block';
            autoSaveStatus.textContent = `✅ Profil tersimpan: ${playerName} - Score: ${formatNumber(score)}`;
            editProfileButton.style.display = 'block';
            
            // Save initial data ke Firebase
            quickSaveToLeaderboard();
            
            if (isEditingProfile) {
                showScreen('mode');
                isEditingProfile = false;
            } else {
                showScreen('game');
                initializeGame();
            }
        });

        // Back buttons
        backToMode.addEventListener('click', () => {
            showScreen('mode');
            isEditingProfile = false;
        });
        
        backToMenuButton.addEventListener('click', () => {
            if (gameActive) {
                // Save final score sebelum kembali
                saveToLeaderboard(true);
                gameActive = false;
                if (autoSaveInterval) {
                    clearInterval(autoSaveInterval);
                }
            }
            showScreen('mode');
        });

        // Initialize game - AUTO START
        function initializeGame() {
            gameActive = true;
            sessionScore = 0;
            clickButton.disabled = false;
            messageDisplay.textContent = "Klik tombol hijau untuk menambah score!";
            updateMessage.textContent = 'Score otomatis tersimpan...';
            
            // Update display
            updateAllDisplays();
            
            // Auto-save interval yang lebih cepat
            autoSaveInterval = setInterval(() => {
                if (gameActive && score > 0) {
                    quickSaveToLeaderboard();
                }
            }, QUICK_SAVE_INTERVAL);
            
            // Update rank periodically
            setInterval(() => {
                if (gameActive) {
                    updateGlobalRank();
                }
            }, 3000);
        }

        // Handle click dengan sound effect dan gambar berubah
        clickButton.addEventListener('mousedown', () => {
            if (!gameActive) return;
            
            // Ganti gambar ke pressed state
            clickButtonImage.src = buttonImages.pressed;
            
            // Play sound effect
            playClickSound();
        });

        clickButton.addEventListener('mouseup', () => {
            // Kembali ke gambar normal
            clickButtonImage.src = buttonImages.normal;
        });

        clickButton.addEventListener('touchstart', () => {
            if (!gameActive) return;
            
            // Ganti gambar ke pressed state
            clickButtonImage.src = buttonImages.pressed;
            
            // Play sound effect
            playClickSound();
        });

        clickButton.addEventListener('touchend', () => {
            // Kembali ke gambar normal
            clickButtonImage.src = buttonImages.normal;
        });

        // Function untuk play sound effect dan update score
        function playClickSound() {
            // Reset sound ke awal
            clickSound.currentTime = 0;
            
            // Play sound dengan volume rendah
            clickSound.volume = 0.1;
            
            // Coba play sound, tangani error jika ada
            clickSound.play().catch(error => {
                console.log('Sound effect tidak bisa diputar:', error);
            });
            
            // Update score
            score++;
            sessionScore++;
            
            // Update localStorage immediately
            localStorage.setItem('playerScore', score.toString());
            
            updateAllDisplays();
            scoreDisplay.classList.add('score-update');
            setTimeout(() => scoreDisplay.classList.remove('score-update'), 300);
            
            // Check for frame unlocks
            const unlockedNewFrame = checkFrameUnlocks(score);
            
            // Quick save ke Firebase jika perlu
            const now = Date.now();
            if (unlockedNewFrame || now - lastSaveTime > QUICK_SAVE_INTERVAL) {
                quickSaveToLeaderboard();
                lastSaveTime = now;
            }
            
            // Update rank setiap 10 klik
            if (score % 10 === 0) {
                updateGlobalRank();
            }
        }

        // Quick save ke Firebase (tanpa delay)
        function quickSaveToLeaderboard() {
            if (playerName && score > 0) {
                const playerData = {
                    name: playerName,
                    avatar: customAvatar || selectedAvatar,
                    score: score,
                    timestamp: Date.now(),
                    lastUpdated: new Date().toLocaleString('id-ID'),
                    playerId: playerId,
                    currentFrame: currentFrame,
                    unlockedFrames: unlockedFrames
                };
                
                database.ref('leaderboard/' + playerId).set(playerData)
                    .then(() => {
                        updateMessage.textContent = `✅ Score ${formatNumber(score)} tersimpan!`;
                        setTimeout(() => {
                            if (gameActive) {
                                updateMessage.textContent = 'Score terus bertambah...';
                            }
                        }, 1000);
                    })
                    .catch((error) => {
                        updateMessage.textContent = '❌ Gagal menyimpan score';
                        console.error('Firebase error:', error);
                    });
            }
        }

        // Save score to Firebase (dengan frame data) - untuk final save
        function saveToLeaderboard(isFinal) {
            if (playerName && score > 0) {
                const playerData = {
                    name: playerName,
                    avatar: customAvatar || selectedAvatar,
                    score: score,
                    timestamp: Date.now(),
                    lastUpdated: new Date().toLocaleString('id-ID'),
                    playerId: playerId,
                    currentFrame: currentFrame,
                    unlockedFrames: unlockedFrames
                };
                
                database.ref('leaderboard/' + playerId).set(playerData)
                    .then(() => {
                        if (isFinal) {
                            updateMessage.textContent = `✅ Score final ${formatNumber(score)} tersimpan!`;
                        }
                        // Update rank setelah save
                        updateGlobalRank();
                    })
                    .catch((error) => {
                        updateMessage.textContent = '❌ Gagal menyimpan score';
                        console.error('Firebase error:', error);
                    });
            }
        }

        // Load leaderboard preview dengan frame support
        function loadLeaderboardPreview() {
            database.ref('leaderboard').orderByChild('score').limitToLast(5).on('value', (snapshot) => {
                const leaders = [];
                snapshot.forEach((childSnapshot) => {
                    leaders.push({
                        id: childSnapshot.key,
                        ...childSnapshot.val()
                    });
                });
                
                // Sort by score descending
                leaders.sort((a, b) => b.score - a.score);
                updateLeaderboardPreview(leaders.slice(0, 5));
            });
        }

        // Handle page refresh/close - save data
        window.addEventListener('beforeunload', (event) => {
            if (gameActive && score > 0) {
                // Save ke localStorage
                localStorage.setItem('playerScore', score.toString());
                // Quick save ke Firebase
                quickSaveToLeaderboard();
            }
        });

        // Initialize game data saat page load
        initializeGameData();
        
        // Load leaderboard preview
        loadLeaderboardPreview();

        console.log('LeafFast Global Mode + Frame System Ready!');
    </script>
</body>
</html>
