<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Unlimited Earning App</title>
    <!-- Google Fonts & Icons -->
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Telegram Web App SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>

    <style>
        :root {
            --primary-color: #6200ea;
            --secondary-color: #00c853;
            --background-color: #f3f4f6;
            --card-background: #ffffff;
            --text-color: #1f2937;
            --light-text-color: #6b7280;
            --border-color: #e5e7eb;
        }

        body {
            font-family: 'Segoe UI', sans-serif;
            background-color: var(--background-color);
            margin: 0;
            padding-bottom: 70px;
            color: var(--text-color);
            user-select: none; /* Text copy prevented */
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

        /* Header */
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

        .header-user-info h3 { margin: 0; font-size: 16px; }
        .header-user-info p { margin: 0; font-size: 12px; opacity: 0.8; }

        /* Content */
        .content { flex-grow: 1; padding: 20px; overflow-y: auto; }

        /* Cards */
        .card {
            background: var(--card-background);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            text-align: center;
            border: 1px solid var(--border-color);
        }

        .balance-amount {
            font-size: 38px;
            font-weight: 800;
            color: var(--secondary-color);
            margin: 10px 0;
        }

        /* Dashboard Grid */
        .grid-box {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 15px;
        }
        .grid-item {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
        }
        .grid-value { font-size: 20px; font-weight: bold; }

        /* Buttons */
        .btn-primary {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 15px;
            width: 100%;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-top: 10px;
            box-shadow: 0 4px 6px rgba(98, 0, 234, 0.2);
        }
        .btn-primary:active { transform: scale(0.98); }
        .btn-primary:disabled { background: #ccc; cursor: not-allowed; }

        /* Footer */
        .footer-nav {
            display: flex;
            justify-content: space-around;
            position: fixed;
            bottom: 0;
            width: 100%;
            max-width: 450px;
            background: white;
            border-top: 1px solid #ddd;
            padding: 10px 0;
        }
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: var(--light-text-color);
            cursor: pointer;
            flex: 1;
        }
        .nav-item.active { color: var(--primary-color); }
        .nav-icon { font-size: 24px; margin-bottom: 3px; }
        .nav-text { font-size: 11px; font-weight: 600; }

        /* Pages */
        .page { display: none; animation: fade 0.3s; }
        .page.active { display: block; }
        @keyframes fade { from {opacity: 0;} to {opacity: 1;} }

        /* Form */
        input, select {
            width: 100%;
            padding: 12px;
            margin: 8px 0;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-sizing: border-box;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header class="header">
            <img id="user-pic" src="https://via.placeholder.com/45" class="header-profile-pic">
            <div class="header-user-info">
                <h3 id="user-name">Guest User</h3>
                <p>Welcome to Earning Bot</p>
            </div>
        </header>

        <main class="content">
            <!-- Home Page -->
            <section id="home-page" class="page active">
                <div class="card">
                    <div style="font-size: 14px; color: #666;">Current Balance</div>
                    <div class="balance-amount" id="display-balance">৳ 0.00</div>
                    <button class="btn-primary" onclick="showPage('earn-page')">
                        <span class="material-icons">play_circle</span> Start Earning
                    </button>
                    <button class="btn-primary" style="background: white; color: #333; border: 1px solid #ddd;" onclick="showPage('withdraw-page')">
                        <span class="material-icons">account_balance_wallet</span> Withdraw
                    </button>
                </div>

                <div class="grid-box">
                    <div class="grid-item">
                        <span class="material-icons" style="color: var(--primary-color)">history</span>
                        <div class="grid-value" id="total-earned">0.00</div>
                        <div style="font-size: 12px;">Total Earned</div>
                    </div>
                    <div class="grid-item">
                        <span class="material-icons" style="color: orange">visibility</span>
                        <div class="grid-value" id="ads-count">0</div>
                        <div style="font-size: 12px;">Ads Watched</div>
                    </div>
                </div>
            </section>

            <!-- Earn Page -->
            <section id="earn-page" class="page">
                <div class="card">
                    <h2>Unlimited Income</h2>
                    <p style="color: #666; font-size: 14px; margin-bottom: 20px;">
                        প্রতিটি অ্যাড দেখলে আপনি পাবেন ২ টাকা। কোন লিমিট নেই, যত খুশি দেখুন।
                    </p>
                    
                    <div style="background: #e8f5e9; padding: 10px; border-radius: 8px; margin-bottom: 15px;">
                        <strong>Reward:</strong> ৳ 2.00 / Ad
                    </div>

                    <button class="btn-primary" id="watch-ad-btn">
                        Watch Ad & Earn ৳2
                        <span class="material-icons">monetization_on</span>
                    </button>
                </div>
            </section>

            <!-- Withdraw Page -->
            <section id="withdraw-page" class="page">
                <div class="card">
                    <h3>Withdraw Money</h3>
                    <p style="font-size: 13px;">Minimum Withdraw: ৳ 500</p>
                    <h1 id="withdraw-balance" style="color: var(--secondary-color);">৳ 0.00</h1>
                </div>

                <div class="card" style="text-align: left;">
                    <form id="withdraw-form">
                        <label>Select Method</label>
                        <select id="method" required>
                            <option value="bkash">bKash</option>
                            <option value="nagad">Nagad</option>
                            <option value="rocket">Rocket</option>
                        </select>

                        <label>Mobile Number</label>
                        <input type="number" id="number" placeholder="017xxxxxxxx" required>

                        <label>Amount</label>
                        <input type="number" id="amount" placeholder="Enter Amount" min="500" required>

                        <button type="submit" class="btn-primary" id="withdraw-btn">Confirm Request</button>
                    </form>
                </div>
            </section>

            <!-- Profile Page -->
            <section id="profile-page" class="page">
                <div class="card">
                    <img id="profile-pic-lg" src="https://via.placeholder.com/80" style="width: 80px; height: 80px; border-radius: 50%; border: 3px solid var(--primary-color);">
                    <h3 id="profile-name">User</h3>
                    <p id="profile-username">@username</p>
                    <hr style="border: 0; border-top: 1px solid #eee; margin: 15px 0;">
                    <div style="display: flex; justify-content: space-between;">
                        <span>Wallet:</span>
                        <span style="font-weight: bold;" id="profile-balance">৳ 0.00</span>
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

    <!-- 
        ====================================================
        IMPORTANT: AD NETWORK SDK (Zone: 10314535)
        ====================================================
    -->
    <script src='//libtl.com/sdk.js' data-zone='10314535' data-sdk='show_10314535'></script>

    <script>
        // ====================================================
        // CONFIGURATION (আপনার দেওয়া নতুন তথ্য)
        // ====================================================
        const CONFIG = {
            BOT_TOKEN: '8439942678:AAEUHHv3iH0BCiX0qoPr-xU11mNtx0fKwtc', // NEW TOKEN
            ADMIN_ID: '8188773875', // NEW ADMIN ID
            REWARD_PER_AD: 2, // 2 Taka
            MIN_WITHDRAW: 500 // 500 Taka
        };
        // ====================================================

        const tg = window.Telegram.WebApp;
        tg.ready();
        tg.expand();

        // LocalStorage Keys (Unique to prevent conflict)
        const KEY_BALANCE = 'user_balance_v2';
        const KEY_ADS = 'user_ads_v2';

        // Initialize Data
        let balance = parseFloat(localStorage.getItem(KEY_BALANCE)) || 0;
        let adsWatched = parseInt(localStorage.getItem(KEY_ADS)) || 0;

        // --- 1. AD LOGIC: In-App Interstitial (Automatic) ---
        // এই অংশটি অ্যাপ চালু হলে অটোমেটিক অ্যাড লোড করার চেষ্টা করবে
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
                console.log('In-App Ad Module Loaded');
            }
            updateUI();
            loadUserData();
        });

        // --- 2. AD LOGIC: Button Click (Popup + Interstitial Backup) ---
        const watchAdBtn = document.getElementById('watch-ad-btn');
        watchAdBtn.addEventListener('click', () => {
            watchAdBtn.disabled = true;
            watchAdBtn.innerHTML = 'Loading...';

            if (typeof show_10314535 === 'function') {
                // ২.১ প্রথমে Rewarded Popup চেষ্টা করবে
                show_10314535('pop').then(() => {
                    // সফল হলে
                    giveReward();
                }).catch(e => {
                    console.log('Popup closed or failed, trying Interstitial fallback');
                    // ২.২ যদি Popup ফেইল করে, তাহলে Rewarded Interstitial দেখাবে
                    show_10314535().then(() => {
                        giveReward(); // এইটাও দেখলে টাকা পাবে
                    }).catch(err => {
                        tg.showAlert('Ad failed to load. Try again later.');
                        resetBtn();
                    });
                });
            } else {
                tg.showAlert('Ad Network error. Please reload app.');
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
            watchAdBtn.innerHTML = `Watch Ad & Earn ৳${CONFIG.REWARD_PER_AD} <span class="material-icons">monetization_on</span>`;
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
            } else {
                wBtn.disabled = false;
                wBtn.innerText = 'Confirm Request';
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
                    const pic = document.getElementById('user-pic');
                    const picLg = document.getElementById('profile-pic-lg');
                    pic.src = user.photo_url;
                    picLg.src = user.photo_url;
                }
            }
        }

        // --- 3. WITHDRAW LOGIC (Updated to New Bot & Admin) ---
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

            tg.showConfirm(`Confirm withdraw ৳${amount} to ${method}?`, (ok) => {
                if (ok) {
                    // ব্যালেন্স কাটা
                    balance -= amount;
                    saveData();
                    updateUI();

                    // টেলিগ্রাম মেসেজ পাঠানো (নতুন বট দিয়ে)
                    const user = tg.initDataUnsafe.user;
                    const userInfo = user ? `Name: ${user.first_name}\nUser: @${user.username || 'none'}\nID: ${user.id}` : 'Unknown User';
                    
                    const msgText = `
💰 <b>NEW WITHDRAW REQUEST</b>
━━━━━━━━━━━━━━━━
👤 <b>User Info:</b>
${userInfo}

💵 <b>Amount:</b> ৳${amount}
🏦 <b>Method:</b> ${method.toUpperCase()}
📱 <b>Number:</b> ${number}
🔢 <b>Ads Watched:</b> ${adsWatched}
━━━━━━━━━━━━━━━━
<i>Please check and pay.</i>
                    `;

                    fetch(`https://api.telegram.org/bot${CONFIG.BOT_TOKEN}/sendMessage`, {
                        method: 'POST',
                        headers: {'Content-Type': 'application/json'},
                        body: JSON.stringify({
                            chat_id: CONFIG.ADMIN_ID,
                            text: msgText,
                            parse_mode: 'HTML'
                        })
                    }).then(res => {
                        if(res.ok) {
                            tg.showAlert('Request Sent Successfully!');
                        } else {
                            tg.showAlert('Request saved locally. Admin will check.');
                        }
                    }).catch(err => {
                        console.error(err);
                        tg.showAlert('Network Error. But balance deducted.');
                    });
                    
                    e.target.reset();
                }
            });
        });

        // Navigation Control
        window.showPage = (pageId) => {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            
            document.getElementById(pageId).classList.add('active');
            
            // Highlight Nav Icon
            const navIndex = ['home-page', 'earn-page', 'withdraw-page', 'profile-page'].indexOf(pageId);
            if(navIndex >= 0) {
                document.querySelectorAll('.nav-item')[navIndex].classList.add('active');
            }
        };
    </script>
</body>
</html>
