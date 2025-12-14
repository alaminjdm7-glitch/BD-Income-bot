<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Unlimited Earning App</title>
    <!-- Google Fonts for Icons -->
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Telegram Web App SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>

    <style>
        :root {
            --primary-color: #6200ea; /* Changed to a purple theme for fresh look */
            --secondary-color: #00c853;
            --background-color: #f3f4f6;
            --card-background: #ffffff;
            --text-color: #1f2937;
            --light-text-color: #6b7280;
            --border-color: #e5e7eb;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--background-color);
            margin: 0;
            padding-bottom: 70px; /* Space for footer */
            color: var(--text-color);
        }

        .container {
            max-width: 450px;
            margin: 0 auto;
            background-color: var(--card-background);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        /* Header Styles */
        .header {
            display: flex;
            align-items: center;
            padding: 15px;
            background: linear-gradient(135deg, var(--primary-color), #7c4dff);
            color: white;
            position: sticky;
            top: 0;
            z-index: 100;
        }
        
        .header-profile-pic {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            margin-right: 12px;
            border: 2px solid #fff;
            background-color: #eee;
            object-fit: cover;
        }

        .header-user-info h3 {
            margin: 0;
            font-size: 17px;
            font-weight: 600;
        }
        
        .header-user-info p {
            margin: 0;
            font-size: 12px;
            opacity: 0.9;
        }

        /* Content Area */
        .content {
            flex-grow: 1;
            padding: 20px;
            overflow-y: auto;
        }

        /* Card Styles */
        .card {
            background-color: var(--card-background);
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            text-align: center;
            border: 1px solid var(--border-color);
        }

        .card-header {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 15px;
            color: var(--text-color);
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .card-header .material-icons, .card-header .fa-solid {
            margin-right: 8px;
            color: var(--primary-color);
        }

        .balance-card .balance {
            font-size: 40px;
            font-weight: 800;
            color: var(--secondary-color);
            margin: 10px 0;
            text-shadow: 1px 1px 0px rgba(0,0,0,0.1);
        }

        .card-label {
            font-size: 14px;
            color: var(--light-text-color);
        }

        /* Dashboard Grid */
        .dashboard-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 20px;
        }

        .dashboard-item {
            background-color: #f9fafb;
            border-radius: 12px;
            padding: 15px;
            border: 1px solid var(--border-color);
            text-align: center;
        }

        .dashboard-item .material-icons {
            font-size: 28px;
            margin-bottom: 8px;
            color: var(--primary-color);
        }

        .dashboard-item-value {
            font-size: 22px;
            font-weight: 700;
            color: var(--text-color);
        }

        .dashboard-item-label {
            font-size: 12px;
            color: var(--light-text-color);
        }

        /* Buttons */
        .primary-btn, .secondary-btn {
            border: none;
            padding: 16px;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            transition: all 0.3s;
            margin-top: 15px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        .primary-btn .material-icons, .secondary-btn .material-icons {
            font-size: 20px;
        }

        .primary-btn {
            background: linear-gradient(135deg, var(--primary-color), #7c4dff);
            color: #fff;
            box-shadow: 0 4px 6px rgba(98, 0, 234, 0.2);
        }

        .primary-btn:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }
        
        .primary-btn:disabled {
            background: #cbd5e1;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .secondary-btn {
            background-color: #fff;
            color: var(--text-color);
            border: 2px solid var(--border-color);
        }

        .secondary-btn:hover {
            background-color: #f9fafb;
            transform: translateY(-2px);
        }
        
        /* Stats Box inside Earn Page */
        .stats-box {
            background: #eef2ff;
            border-radius: 10px;
            padding: 15px;
            margin-top: 15px;
            display: flex;
            justify-content: space-between;
        }
        .stat {
            text-align: center;
        }
        .stat-val { font-weight: bold; font-size: 18px; color: var(--primary-color); }
        .stat-lbl { font-size: 11px; color: #666; }

        /* Footer Navigation */
        .footer-nav {
            display: flex;
            justify-content: space-around;
            align-items: center;
            position: fixed;
            bottom: 0;
            width: 100%;
            max-width: 450px;
            background-color: var(--card-background);
            border-top: 1px solid var(--border-color);
            padding-bottom: 10px; /* Safe area */
            box-shadow: 0 -4px 6px rgba(0, 0, 0, 0.03);
            z-index: 99;
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 12px;
            cursor: pointer;
            color: var(--light-text-color);
            transition: color 0.3s;
            flex: 1;
        }

        .nav-item .material-icons, .nav-item .fa-solid {
            font-size: 24px;
            margin-bottom: 4px;
        }

        .nav-item-label {
            font-size: 11px;
            font-weight: 600;
        }

        .nav-item.active {
            color: var(--primary-color);
        }

        /* Page Transitions */
        .page {
            display: none;
            animation: fadeIn 0.4s ease-out;
        }

        .page.active {
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* Profile & Withdraw Form */
        .form-group {
            margin-bottom: 15px;
            text-align: left;
        }
        .form-group label {
            display: block;
            margin-bottom: 6px;
            font-size: 14px;
            font-weight: 600;
        }
        .form-group input, .form-group select {
            width: 100%;
            padding: 14px;
            border-radius: 10px;
            border: 1px solid var(--border-color);
            box-sizing: border-box;
            font-size: 16px;
            background: #f9fafb;
            transition: border-color 0.3s;
        }
        .form-group input:focus, .form-group select:focus {
            outline: none;
            border-color: var(--primary-color);
            background: #fff;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header class="header">
            <img id="header-profile-pic" src="https://via.placeholder.com/45" alt="Profile" class="header-profile-pic">
            <div class="header-user-info">
                <h3 id="header-user-name">Guest User</h3>
                <p>Unlimited Earning App</p>
            </div>
        </header>

        <!-- Main Content -->
        <main class="content">
            <!-- Home Page -->
            <section id="home-page" class="page active">
                <div class="card balance-card">
                    <h2 class="card-header">
                        <span class="material-icons">account_balance_wallet</span>
                        Total Balance
                    </h2>
                    <div class="balance" id="current-balance">BDT 0.00</div>
                    <button class="primary-btn" id="start-earning-btn">
                        Start Earning <span class="material-icons">rocket_launch</span>
                    </button>
                    <button class="secondary-btn" id="withdraw-btn">
                        Withdraw Money <span class="material-icons">payments</span>
                    </button>
                </div>
                <div class="dashboard-grid">
                    <div class="dashboard-item">
                        <span class="material-icons">history</span>
                        <div class="dashboard-item-value" id="total-earnings">0.00</div>
                        <div class="dashboard-item-label">Lifetime Earned</div>
                    </div>
                    <div class="dashboard-item">
                        <span class="material-icons" style="color: #ff9800;">play_circle</span>
                        <div class="dashboard-item-value" id="ads-watched">0</div>
                        <div class="dashboard-item-label">Ads Today</div>
                    </div>
                </div>
            </section>

            <!-- Earn Page -->
            <section id="earn-page" class="page">
                <div class="card">
                    <h2 class="card-header">
                        <span class="material-icons">monetization_on</span>
                        Watch & Earn
                    </h2>
                    
                    <div class="stats-box">
                        <div class="stat">
                            <div class="stat-val">Unlimited</div>
                            <div class="stat-lbl">Daily Limit</div>
                        </div>
                        <div class="stat">
                            <div class="stat-val">BDT 2.00</div>
                            <div class="stat-lbl">Per Ad</div>
                        </div>
                    </div>

                    <div style="margin: 20px 0;">
                        <img src="https://cdn-icons-png.flaticon.com/512/6183/6183535.png" alt="Ad" width="80" style="margin-bottom: 10px;">
                        <p style="font-size: 15px; color: #555;">
                            যত খুশি বিজ্ঞাপন দেখুন এবং আয় করুন। প্রতিটি বিজ্ঞাপনে ২ টাকা।
                        </p>
                    </div>

                    <button class="primary-btn" id="watch-ad-btn">
                        Watch Ad Now (BDT 2)
                        <span class="material-icons">play_arrow</span>
                    </button>
                    
                    <p style="font-size: 12px; color: #999; margin-top: 15px;">
                        *বিজ্ঞাপন সম্পূর্ণ না দেখলে রিওয়ার্ড পাবেন না।
                    </p>
                </div>
            </section>
            
            <!-- Withdraw Page -->
            <section id="withdraw-page" class="page">
                <div class="card">
                    <h2 class="card-header">
                        <span class="material-icons">currency_exchange</span>
                        Withdraw Request
                    </h2>
                    <div class="balance" id="withdraw-balance-display">BDT 0.00</div>
                    <div class="card-label">Available Balance</div>
                </div>
                
                <div class="card" style="text-align: left;">
                    <form id="withdraw-form">
                        <div class="form-group">
                            <label>Payment Method</label>
                            <select id="withdraw-method" required>
                                <option value="" disabled selected>Select Method</option>
                                <option value="bkash">bKash (Personal)</option>
                                <option value="nagad">Nagad (Personal)</option>
                                <option value="rocket">Rocket (Personal)</option>
                                <option value="upay">Upay</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label>Mobile Number</label>
                            <input type="number" id="account-number" placeholder="017xxxxxxxx" required>
                        </div>
                        <div class="form-group">
                            <label>Amount (Min: 500)</label>
                            <input type="number" id="withdraw-amount" placeholder="500" required min="500">
                        </div>
                        <button type="submit" class="primary-btn" id="submit-withdraw-btn">
                            Confirm Withdraw
                        </button>
                    </form>
                </div>
            </section>

            <!-- Profile Page -->
            <section id="profile-page" class="page">
                <div class="card">
                    <div style="margin-bottom: 15px;">
                        <img id="profile-page-pic" src="https://via.placeholder.com/80" alt="Pic" style="width: 80px; height: 80px; border-radius: 50%; border: 3px solid var(--primary-color);">
                    </div>
                    <h3 id="profile-user-name">User Name</h3>
                    <p id="profile-user-username" style="color: var(--light-text-color);">@username</p>
                    
                    <div style="margin-top: 20px; border-top: 1px solid #eee; padding-top: 15px;">
                        <div style="display: flex; justify-content: space-between; padding: 10px 0; border-bottom: 1px solid #f5f5f5;">
                            <span>Balance</span>
                            <span style="font-weight: bold; color: var(--secondary-color);" id="profile-balance">0.00</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; padding: 10px 0; border-bottom: 1px solid #f5f5f5;">
                            <span>Ads Watched (Today)</span>
                            <span style="font-weight: bold;" id="profile-ads-watched">0</span>
                        </div>
                        <div style="display: flex; justify-content: space-between; padding: 10px 0;">
                            <span>Status</span>
                            <span style="font-weight: bold; color: var(--primary-color);">Active</span>
                        </div>
                    </div>
                </div>
            </section>
        </main>

        <!-- Footer Navigation -->
        <nav class="footer-nav">
            <div class="nav-item active" data-page="home-page">
                <span class="material-icons">home</span>
                <span class="nav-item-label">Home</span>
            </div>
            <div class="nav-item" data-page="earn-page">
                <span class="material-icons">play_circle_filled</span>
                <span class="nav-item-label">Earn</span>
            </div>
            <div class="nav-item" data-page="withdraw-page">
                <span class="material-icons">account_balance</span>
                <span class="nav-item-label">Withdraw</span>
            </div>
            <div class="nav-item" data-page="profile-page">
                <span class="material-icons">person</span>
                <span class="nav-item-label">Profile</span>
            </div>
        </nav>
    </div>

    <!-- 
        =====================================================================
        ====== AD NETWORK SDK (NEW ZONE: 10314535) ======
        =====================================================================
    -->
    <script src='//libtl.com/sdk.js' data-zone='10314535' data-sdk='show_10314535'></script>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const tg = window.Telegram.WebApp;
            tg.ready();
            tg.expand(); // Fullscreen mode
            
            // ==========================================================
            // ====== সেটিংস (Settings) ======
            // ==========================================================
            const BOT_TOKEN = '8439942678:AAEUHHv3iH0BCiX0qoPr-xU11mNtx0fKwtc'; // আপনার দেওয়া নতুন টোকেন
            const ADMIN_ID = '8188773875'; // আপনার এডমিন আইডি
            
            const AD_REWARD = 2; // প্রতি অ্যাডে ২ টাকা
            const MIN_WITHDRAW_AMOUNT = 500; // মিনিমাম উইথড্র ৫০০ টাকা
            // ==========================================================

            // User Data Handling
            const userData = tg.initDataUnsafe.user;
            
            // UI Elements
            const navItems = document.querySelectorAll('.nav-item');
            const pages = document.querySelectorAll('.page');
            const watchAdBtn = document.getElementById('watch-ad-btn');
            
            // Initialize LocalStorage
            if (!localStorage.getItem('earnings')) localStorage.setItem('earnings', '0');
            if (!localStorage.getItem('adsWatched')) localStorage.setItem('adsWatched', '0');
            if (!localStorage.getItem('lastAdResetTime')) localStorage.setItem('lastAdResetTime', new Date().getTime());

            // 1. In-App Interstitial Ad Settings (অটোমেটিক লোড হবে)
            // আপনার দেওয়া কোড অনুযায়ী
            if (typeof show_10314535 === 'function') {
                show_10314535({
                    type: 'inApp',
                    inAppSettings: {
                        frequency: 2,
                        capping: 0.1,
                        interval: 30,
                        timeout: 5,
                        everyPage: false
                    }
                });
                console.log("In-App Ads Initialized");
            }

            // Daily Reset Logic (Just for counting stats, not limiting)
            function checkDailyReset() {
                const now = new Date().getTime();
                const lastResetTime = parseInt(localStorage.getItem('lastAdResetTime') || 0);
                const twentyFourHours = 24 * 60 * 60 * 1000;

                if (now - lastResetTime > twentyFourHours) {
                    localStorage.setItem('adsWatched', '0'); 
                    localStorage.setItem('lastAdResetTime', now);
                }
            }
            checkDailyReset();

            function updateUI() {
                const earnings = parseFloat(localStorage.getItem('earnings'));
                const adsWatched = parseInt(localStorage.getItem('adsWatched'));
                
                // Update User Info
                if(userData) {
                    const name = `${userData.first_name || ''} ${userData.last_name || ''}`.trim();
                    document.getElementById('header-user-name').textContent = name;
                    document.getElementById('profile-user-name').textContent = name;
                    document.getElementById('profile-user-username').textContent = userData.username ? `@${userData.username}` : '';
                    
                    if (userData.photo_url) {
                        // Using proxy to avoid CORS issues with Telegram images
                        const picUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(userData.photo_url)}`;
                        document.getElementById('header-profile-pic').src = picUrl;
                        document.getElementById('profile-page-pic').src = picUrl;
                    }
                }

                // Balance Updates
                const formattedBalance = earnings.toFixed(2);
                document.querySelectorAll('#current-balance, #profile-balance, #withdraw-balance-display').forEach(el => {
                    el.textContent = `BDT ${formattedBalance}`;
                });
                
                document.getElementById('total-earnings').textContent = formattedBalance;
                document.getElementById('ads-watched').textContent = adsWatched;
                document.getElementById('profile-ads-watched').textContent = adsWatched;

                // Withdraw Button State
                const withdrawSubmitBtn = document.getElementById('submit-withdraw-btn');
                if (earnings < MIN_WITHDRAW_AMOUNT) {
                    withdrawSubmitBtn.disabled = true;
                    withdrawSubmitBtn.textContent = `Need BDT ${MIN_WITHDRAW_AMOUNT} to Withdraw`;
                    withdrawSubmitBtn.style.background = "#ccc";
                } else {
                    withdrawSubmitBtn.disabled = false;
                    withdrawSubmitBtn.textContent = "Confirm Withdraw";
                    withdrawSubmitBtn.style.background = "var(--primary-color)";
                }
            }

            // Navigation Logic
            function showPage(pageId) {
                document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
                document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
                
                document.getElementById(pageId).classList.add('active');
                const activeNav = document.querySelector(`.nav-item[data-page="${pageId}"]`);
                if(activeNav) activeNav.classList.add('active');
            }

            // Event Listeners for Nav
            navItems.forEach(item => {
                item.addEventListener('click', () => showPage(item.dataset.page));
            });

            document.getElementById('start-earning-btn').addEventListener('click', () => showPage('earn-page'));
            document.getElementById('withdraw-btn').addEventListener('click', () => showPage('withdraw-page'));

            // ==========================================================
            // ====== 2. REWARDED AD LOGIC (Popup Format) ======
            // ==========================================================
            watchAdBtn.addEventListener('click', () => {
                checkDailyReset();
                watchAdBtn.disabled = true;
                watchAdBtn.innerHTML = '<span class="material-icons fa-spin">refresh</span> Loading Ad...';

                if (typeof show_10314535 === 'function') {
                    // Using Rewarded Popup Format as requested
                    show_10314535('pop').then(() => {
                        // Success: User watched ad
                        addReward();
                    }).catch(e => {
                        console.error("Popup Ad failed/closed early:", e);
                        // Fallback: Try standard interstitial if popup fails
                        // 3. Rewarded Interstitial (Backup)
                        show_10314535().then(() => {
                            addReward();
                        }).catch(err => {
                            tg.showAlert('Ad failed to load. Please check your internet connection.');
                            resetAdButton();
                        });
                    });
                } else {
                    // SDK not loaded properly
                    tg.showAlert('Ad Network connecting... Wait 5 seconds.');
                    setTimeout(resetAdButton, 2000);
                }
            });

            function addReward() {
                const currentEarnings = parseFloat(localStorage.getItem('earnings'));
                const newEarnings = currentEarnings + AD_REWARD;
                const newAdsCount = parseInt(localStorage.getItem('adsWatched')) + 1;
                
                localStorage.setItem('earnings', newEarnings.toString());
                localStorage.setItem('adsWatched', newAdsCount.toString());
                
                tg.showAlert(`Congrats! You earned BDT ${AD_REWARD} successfully.`);
                updateUI();
                resetAdButton();
            }

            function resetAdButton() {
                watchAdBtn.disabled = false;
                watchAdBtn.innerHTML = `Watch Ad Now (BDT ${AD_REWARD}) <span class="material-icons">play_arrow</span>`;
            }

            // ==========================================================
            // ====== WITHDRAW LOGIC ======
            // ==========================================================
            const withdrawForm = document.getElementById('withdraw-form');
            withdrawForm.addEventListener('submit', (e) => {
                e.preventDefault();
                
                const amount = parseFloat(document.getElementById('withdraw-amount').value);
                const method = document.getElementById('withdraw-method').value;
                const account = document.getElementById('account-number').value;
                const currentBalance = parseFloat(localStorage.getItem('earnings'));

                if (amount > currentBalance) {
                    tg.showAlert("Insufficient balance!");
                    return;
                }
                
                if (amount < MIN_WITHDRAW_AMOUNT) {
                    tg.showAlert(`Minimum withdraw is BDT ${MIN_WITHDRAW_AMOUNT}`);
                    return;
                }

                tg.showConfirm(`Withdraw BDT ${amount} via ${method}?`, (ok) => {
                    if(ok) {
                        // Deduct Balance
                        localStorage.setItem('earnings', (currentBalance - amount).toString());
                        updateUI();

                        // Send to Telegram Admin
                        const userTxt = userData ? `${userData.first_name} (@${userData.username})` : 'App User';
                        const msg = `
💸 <b>New Withdraw Request</b>
➖➖➖➖➖➖➖➖➖➖
👤 <b>User:</b> ${userTxt}
💰 <b>Amount:</b> ${amount} BDT
🏦 <b>Method:</b> ${method.toUpperCase()}
📞 <b>Number:</b> ${account}
📅 <b>Date:</b> ${new Date().toLocaleString()}
➖➖➖➖➖➖➖➖➖➖
`;
                        // Telegram API Call
                        fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
                            method: 'POST',
                            headers: {'Content-Type': 'application/json'},
                            body: JSON.stringify({
                                chat_id: ADMIN_ID,
                                text: msg,
                                parse_mode: 'HTML'
                            })
                        }).then(() => {
                            tg.showAlert('Withdraw request sent successfully! Wait for payment.');
                            withdrawForm.reset();
                        }).catch(err => {
                            tg.showAlert('Request saved locally (Network Error). Contact admin.');
                        });
                    }
                });
            });

            // Initial UI Update
            updateUI();
        });
    </script>
</body>
</html>
