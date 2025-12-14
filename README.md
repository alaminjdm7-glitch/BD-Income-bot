<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BD Income Bot</title>
    <!-- Icons -->
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Telegram Web App SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>

    <style>
        :root {
            --primary-color: #6200ea; /* Purple Theme from Screenshot */
            --secondary-color: #00c853;
            --background-color: #f3f4f6;
            --card-background: #ffffff;
            --text-color: #1f2937;
        }

        body {
            font-family: 'Segoe UI', sans-serif;
            background-color: var(--background-color);
            margin: 0;
            padding-bottom: 70px;
            color: var(--text-color);
            user-select: none;
        }

        .container {
            max-width: 450px;
            margin: 0 auto;
            background-color: var(--card-background);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        /* Header without unwanted text */
        .header {
            display: flex;
            align-items: center;
            padding: 15px;
            background: linear-gradient(135deg, var(--primary-color), #3700b3);
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
            background: #eee;
        }

        .header-user-info h3 { margin: 0; font-size: 16px; font-weight: 600; }
        .header-user-info p { margin: 0; font-size: 12px; opacity: 0.9; }

        /* Main Content */
        .content { flex-grow: 1; padding: 20px; overflow-y: auto; }

        /* Cards */
        .card {
            background: var(--card-background);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
            text-align: center;
            border: 1px solid #eee;
        }

        .balance-label { font-size: 14px; color: #666; }
        .balance-amount {
            font-size: 42px;
            font-weight: 800;
            color: var(--secondary-color);
            margin: 5px 0 15px 0;
        }

        /* Stats Grid */
        .grid-box {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 15px;
        }
        .grid-item {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 12px;
            text-align: center;
        }
        .grid-value { font-size: 22px; font-weight: bold; color: #333; }
        .grid-label { font-size: 12px; color: #777; margin-top: 5px; }

        /* Buttons */
        .btn-primary {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 16px;
            width: 100%;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 10px;
            box-shadow: 0 4px 6px rgba(98, 0, 234, 0.2);
            transition: transform 0.2s;
        }
        .btn-primary:active { transform: scale(0.98); }
        
        .btn-outline {
            background: white;
            color: #333;
            border: 2px solid #eee;
            box-shadow: none;
        }

        /* Form Inputs */
        input, select {
            width: 100%;
            padding: 14px;
            margin: 8px 0 15px 0;
            border: 1px solid #ddd;
            border-radius: 10px;
            box-sizing: border-box;
            font-size: 16px;
            background: #f9f9f9;
        }
        input:focus { outline: none; border-color: var(--primary-color); background: #fff; }

        /* Footer */
        .footer-nav {
            display: flex;
            justify-content: space-around;
            position: fixed;
            bottom: 0;
            width: 100%;
            max-width: 450px;
            background: white;
            border-top: 1px solid #eee;
            padding: 10px 0;
            z-index: 99;
        }
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: #999;
            cursor: pointer;
            flex: 1;
        }
        .nav-item.active { color: var(--primary-color); }
        .nav-icon { font-size: 24px; margin-bottom: 4px; }
        .nav-text { font-size: 11px; font-weight: 600; }

        /* Page Toggle */
        .page { display: none; animation: fadeIn 0.3s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from {opacity: 0; transform: translateY(10px);} to {opacity: 1; transform: translateY(0);} }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header class="header">
            <img id="user-pic" src="https://via.placeholder.com/45" class="header-profile-pic">
            <div class="header-user-info">
                <h3 id="user-name">Guest</h3>
                <p>Welcome to BD Income Bot</p>
            </div>
        </header>

        <main class="content">
            <!-- Home Page -->
            <section id="home-page" class="page active">
                <div class="card">
                    <div class="balance-label">Current Balance</div>
                    <div class="balance-amount" id="display-balance">৳ 0.00</div>
                    
                    <button class="btn-primary" onclick="showPage('earn-page')">
                        <span class="material-icons">play_circle</span> Start Earning
                    </button>
                    <button class="btn-primary btn-outline" onclick="showPage('withdraw-page')">
                        <span class="material-icons">account_balance_wallet</span> Withdraw
                    </button>
                </div>

                <div class="grid-box">
                    <div class="grid-item">
                        <span class="material-icons" style="color: var(--primary-color)">history</span>
                        <div class="grid-value" id="total-earned">0.00</div>
                        <div class="grid-label">Total Earned</div>
                    </div>
                    <div class="grid-item">
                        <span class="material-icons" style="color: orange">visibility</span>
                        <div class="grid-value" id="ads-count">0</div>
                        <div class="grid-label">Ads Watched</div>
                    </div>
                </div>
            </section>

            <!-- Earn Page -->
            <section id="earn-page" class="page">
                <div class="card">
                    <h2>Watch & Earn</h2>
                    <p style="color: #666; margin-bottom: 20px;">
                        Unlimited Income! Get ৳10 per ad.
                    </p>
                    
                    <div style="background: #eef2ff; padding: 15px; border-radius: 10px; margin-bottom: 20px; border: 1px solid #e0e7ff;">
                        <h1 style="margin: 0; color: var(--primary-color);">৳ 10.00</h1>
                        <small>Reward Per Ad</small>
                    </div>

                    <button class="btn-primary" id="watch-ad-btn">
                        Watch Ad Now
                        <span class="material-icons">monetization_on</span>
                    </button>
                </div>
            </section>

            <!-- Withdraw Page -->
            <section id="withdraw-page" class="page">
                <div class="card">
                    <h3>Withdraw Funds</h3>
                    <p style="font-size: 13px; color: #666;">Minimum Withdraw: ৳ 100</p>
                    <h1 id="withdraw-balance" style="color: var(--secondary-color); margin: 10px 0;">৳ 0.00</h1>
                </div>

                <div class="card" style="text-align: left;">
                    <form id="withdraw-form">
                        <label style="font-weight: 600; font-size: 14px;">Select Method</label>
                        <select id="method" required>
                            <option value="bkash">bKash</option>
                            <option value="nagad">Nagad</option>
                            <option value="rocket">Rocket</option>
                        </select>

                        <label style="font-weight: 600; font-size: 14px;">Mobile Number</label>
                        <input type="number" id="number" placeholder="017xxxxxxxx" required>

                        <label style="font-weight: 600; font-size: 14px;">Amount (Min 100)</label>
                        <input type="number" id="amount" placeholder="Enter Amount" min="100" required>

                        <button type="submit" class="btn-primary" id="withdraw-btn">Confirm Request</button>
                    </form>
                </div>
            </section>

            <!-- Profile Page -->
            <section id="profile-page" class="page">
                <div class="card">
                    <img id="profile-pic-lg" src="https://via.placeholder.com/80" style="width: 80px; height: 80px; border-radius: 50%; border: 3px solid var(--primary-color);">
                    <h3 id="profile-name">User</h3>
                    <p id="profile-username" style="color: #888;">@username</p>
                    
                    <div style="margin-top: 20px; text-align: left;">
                        <div style="padding: 10px 0; border-bottom: 1px solid #eee; display: flex; justify-content: space-between;">
                            <span>Wallet Balance</span>
                            <span style="font-weight: bold; color: var(--secondary-color);" id="profile-balance">৳ 0.00</span>
                        </div>
                        <div style="padding: 10px 0; display: flex; justify-content: space-between;">
                            <span>Account Status</span>
                            <span style="font-weight: bold; color: var(--primary-color);">Active</span>
                        </div>
                    </div>
                </div>
            </section>
        </main>

        <!-- Navigation -->
        <nav class="footer-nav">
            <div class="nav-item active" onclick="showPage('home-page')">
                <span class="material-icons nav-icon">home</span>
                <span class="nav-text">Home</span>
            </div>
            <div class="nav-item" onclick="showPage('earn-page')">
                <span class="material-icons nav-icon">play_circle_filled</span>
                <span class="nav-text">Earn</span>
            </div>
            <div class="nav-item" onclick="showPage('withdraw-page')">
                <span class="material-icons nav-icon">payments</span>
                <span class="nav-text">Withdraw</span>
            </div>
            <div class="nav-item" onclick="showPage('profile-page')">
                <span class="material-icons nav-icon">person</span>
                <span class="nav-text">Profile</span>
            </div>
        </nav>
    </div>

    <!-- SDK -->
    <script src='//libtl.com/sdk.js' data-zone='10314535' data-sdk='show_10314535'></script>

    <script>
        // ====================================================
        // UPDATED CONFIGURATION
        // ====================================================
        const CONFIG = {
            BOT_TOKEN: '8439942678:AAEUHHv3iH0BCiX0qoPr-xU11mNtx0fKwtc', // টোকেন ঠিক আছে
            ADMIN_ID: '8188773875', // এডমিন আইডি ঠিক আছে
            REWARD_PER_AD: 10, // প্রতি এড ১০ টাকা
            MIN_WITHDRAW: 100 // মিনিমাম উইথড্র ১০০ টাকা
        };
        // ====================================================

        const tg = window.Telegram.WebApp;
        tg.ready();
        tg.expand();

        // LocalStorage Keys
        const KEY_BALANCE = 'user_balance_v3'; // Version 3 to ensure fresh start/clean logic
        const KEY_ADS = 'user_ads_v3';

        let balance = parseFloat(localStorage.getItem(KEY_BALANCE)) || 0;
        let adsWatched = parseInt(localStorage.getItem(KEY_ADS)) || 0;

        // --- 1. Automatic In-App Ad ---
        document.addEventListener('DOMContentLoaded', () => {
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
            }
            updateUI();
            loadUserData();
        });

        // --- 2. Button Ad Logic ---
        const watchAdBtn = document.getElementById('watch-ad-btn');
        watchAdBtn.addEventListener('click', () => {
            watchAdBtn.disabled = true;
            watchAdBtn.innerHTML = 'Loading Ad...';

            if (typeof show_10314535 === 'function') {
                show_10314535('pop').then(() => {
                    giveReward();
                }).catch(e => {
                    // Fallback to Interstitial
                    show_10314535().then(() => {
                        giveReward();
                    }).catch(err => {
                        tg.showAlert('Ad failed to load. Check internet connection.');
                        resetBtn();
                    });
                });
            } else {
                tg.showAlert('Ad Network Error. Reload App.');
                resetBtn();
            }
        });

        function giveReward() {
            balance += CONFIG.REWARD_PER_AD;
            adsWatched++;
            saveData();
            tg.showAlert(`Congratulations! You earned ৳${CONFIG.REWARD_PER_AD}`);
            resetBtn();
            updateUI();
        }

        function resetBtn() {
            watchAdBtn.disabled = false;
            watchAdBtn.innerHTML = `Watch Ad Now <span class="material-icons">monetization_on</span>`;
        }

        function saveData() {
            localStorage.setItem(KEY_BALANCE, balance);
            localStorage.setItem(KEY_ADS, adsWatched);
        }

        function updateUI() {
            const formattedBalance = balance.toFixed(2);
            document.getElementById('display-balance').innerText = `৳ ${formattedBalance}`;
            document.getElementById('total-earned').innerText = formattedBalance;
            document.getElementById('profile-balance').innerText = `৳ ${formattedBalance}`;
            document.getElementById('withdraw-balance').innerText = `৳ ${formattedBalance}`;
            document.getElementById('ads-count').innerText = adsWatched;

            const wBtn = document.getElementById('withdraw-btn');
            if(balance < CONFIG.MIN_WITHDRAW) {
                wBtn.disabled = true;
                wBtn.innerText = `Need ৳${CONFIG.MIN_WITHDRAW} to Withdraw`;
                wBtn.style.opacity = "0.6";
            } else {
                wBtn.disabled = false;
                wBtn.innerText = 'Confirm Request';
                wBtn.style.opacity = "1";
            }
        }

        function loadUserData() {
            const user = tg.initDataUnsafe.user;
            if (user) {
                const fullName = `${user.first_name} ${user.last_name || ''}`;
                document.getElementById('user-name').innerText = fullName;
                document.getElementById('profile-name').innerText = fullName;
                document.getElementById('profile-username').innerText = user.username ? `@${user.username}` : '';
                
                if(user.photo_url) {
                    // Proxy for image to work in webview
                    const picUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(user.photo_url)}`;
                    document.getElementById('user-pic').src = picUrl;
                    document.getElementById('profile-pic-lg').src = picUrl;
                }
            }
        }

        // --- 3. Withdraw Logic ---
        document.getElementById('withdraw-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const method = document.getElementById('method').value;
            const number = document.getElementById('number').value;
            const amount = parseFloat(document.getElementById('amount').value);

            if (amount > balance) {
                tg.showAlert('Insufficient Balance!');
                return;
            }

            if (amount < CONFIG.MIN_WITHDRAW) {
                tg.showAlert(`Minimum withdraw is ৳${CONFIG.MIN_WITHDRAW}`);
                return;
            }

            tg.showConfirm(`Withdraw ৳${amount} via ${method}?`, (ok) => {
                if (ok) {
                    balance -= amount;
                    saveData();
                    updateUI();

                    const user = tg.initDataUnsafe.user;
                    const userInfo = user ? `Name: ${user.first_name}\nUser: @${user.username || 'none'}\nID: ${user.id}` : 'Unknown User';
                    
                    const msgText = `
💰 <b>WITHDRAW REQUEST</b>
━━━━━━━━━━━━━━━━
👤 <b>User:</b> ${userInfo}
💵 <b>Amount:</b> ৳${amount}
🏦 <b>Method:</b> ${method.toUpperCase()}
📱 <b>Number:</b> ${number}
━━━━━━━━━━━━━━━━
`;

                    fetch(`https://api.telegram.org/bot${CONFIG.BOT_TOKEN}/sendMessage`, {
                        method: 'POST',
                        headers: {'Content-Type': 'application/json'},
                        body: JSON.stringify({
                            chat_id: CONFIG.ADMIN_ID,
                            text: msgText,
                            parse_mode: 'HTML'
                        })
                    }).then(() => {
                        tg.showAlert('Withdraw request sent successfully!');
                        document.getElementById('withdraw-form').reset();
                    }).catch(() => {
                        tg.showAlert('Request saved locally.');
                    });
                }
            });
        });

        window.showPage = (pageId) => {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            
            const navIndex = ['home-page', 'earn-page', 'withdraw-page', 'profile-page'].indexOf(pageId);
            if(navIndex >= 0) document.querySelectorAll('.nav-item')[navIndex].classList.add('active');
        };
    </script>
</body>
</html>
