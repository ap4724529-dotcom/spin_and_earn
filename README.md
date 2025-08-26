<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no" />
  <title>Spin & Earn — Professional</title>

  <!-- Telegram Web App script -->
  <script src="https://telegram.org/js/telegram-web-app.js" async></script>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>

  <!-- Fonts & Icons -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css" />

  <style>
    :root {
      --bg1: #0f172a; 
      --bg2: #0b1220;
      --accent: #6a11cb; 
      --accent-2: #2575fc;
      --accent-glow: rgba(106, 17, 203, 0.3);
      --card: rgba(255, 255, 255, 0.05);
      --card-border: rgba(255, 255, 255, 0.08);
      --glass: rgba(255, 255, 255, 0.05);
      --success: #10b981; 
      --danger: #ef4444;
      --warning: #f59e0b;
      --text-primary: #f8fafc;
      --text-secondary: #cbd5e1;
      --text-muted: #94a3b8;
      font-family: 'Poppins', system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    
    * {
      box-sizing: border-box;
    }
    
    html, body {
      height: 100%; 
      margin: 0; 
      background: linear-gradient(135deg, var(--bg1), var(--bg2)); 
      color: var(--text-primary); 
      -webkit-font-smoothing: antialiased;
      overflow-x: hidden;
    }
    
    .app {
      max-width: 460px; 
      margin: 0 auto; 
      min-height: 100vh; 
      display: flex; 
      flex-direction: column; 
      padding-bottom: 72px;
      position: relative;
    }
    
    header.app-head {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 16px 20px;
      background: rgba(15, 23, 42, 0.8);
      backdrop-filter: blur(10px);
      position: sticky;
      top: 0;
      z-index: 100;
      border-bottom: 1px solid var(--card-border);
    }
    
    .brand {
      display: flex;
      gap: 12px;
      align-items: center;
    }
    
    .logo {
      width: 44px;
      height: 44px;
      border-radius: 12px;
      background: linear-gradient(135deg, var(--accent), var(--accent-2));
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      box-shadow: 0 4px 15px var(--accent-glow);
      font-size: 18px;
    }
    
    .brand h1 {
      font-size: 18px;
      margin: 0;
      font-weight: 700;
      background: linear-gradient(to right, #fff, #cbd5e1);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    
    .brand .subtitle {
      font-size: 11px;
      color: var(--text-muted);
      margin: 0;
      line-height: 1.2;
    }
    
    .balance {
      background: var(--glass);
      padding: 8px 16px;
      border-radius: 12px;
      font-weight: 600;
      font-size: 14px;
      border: 1px solid var(--card-border);
      display: flex;
      align-items: center;
      gap: 6px;
    }
    
    .balance i {
      color: var(--accent-2);
    }
    
    main.content {
      flex: 1;
      padding: 16px;
      overflow: auto;
    }
    
    .card {
      background: var(--card);
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 16px;
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.25);
      border: 1px solid var(--card-border);
      backdrop-filter: blur(10px);
    }
    
    .center {
      text-align: center;
    }
    
    h2 {
      margin: 0 0 8px 0;
      font-size: 18px;
      font-weight: 600;
      color: var(--text-primary);
    }
    
    h3 {
      font-size: 16px;
      font-weight: 600;
      margin: 0 0 12px 0;
      color: var(--text-primary);
    }
    
    p.muted {
      color: var(--text-muted);
      font-size: 13px;
      margin: 0;
      line-height: 1.5;
    }
    
    /* Wheel */
    .wheel-container {
      position: relative;
      margin: 20px auto;
      width: 300px;
      height: 300px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    
    .wheel-wrap {
      position: relative;
      width: 100%;
      height: 100%;
    }
    
    .wheel-pointer {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      top: -20px;
      width: 0;
      height: 0;
      border-left: 20px solid transparent;
      border-right: 20px solid transparent;
      border-top: 30px solid #ffd54a;
      z-index: 30;
      filter: drop-shadow(0 4px 6px rgba(0, 0, 0, .5));
    }
    
    .wheel {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      overflow: hidden;
      transition: transform 4s cubic-bezier(0.17, 0.67, 0.21, 0.96);
      will-change: transform;
      box-shadow: 0 0 0 6px rgba(255, 255, 255, 0.05), 
                  0 0 30px rgba(0, 0, 0, 0.3);
    }
    
    .slice-text {
      font-size: 12px;
      fill: #fff;
      font-weight: 700;
    }
    
    .spin-btn {
      display: block;
      width: 100%;
      padding: 16px;
      border-radius: 12px;
      border: 0;
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      color: #fff;
      font-weight: 600;
      font-size: 16px;
      cursor: pointer;
      box-shadow: 0 10px 30px rgba(37, 117, 252, 0.25);
      transition: all 0.3s ease;
      margin-top: 20px;
      position: relative;
      overflow: hidden;
    }
    
    .spin-btn::before {
      content: '';
      position: absolute;
      top: 0;
      left: -100%;
      width: 100%;
      height: 100%;
      background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
      transition: 0.5s;
    }
    
    .spin-btn:hover::before {
      left: 100%;
    }
    
    .spin-btn:disabled {
      opacity: .6;
      cursor: not-allowed;
      transform: none;
    }
    
    .spins-counter {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 6px 12px;
      background: rgba(255, 213, 74, 0.1);
      border-radius: 20px;
      color: #ffd54a;
      font-size: 13px;
      font-weight: 500;
      margin-top: 8px;
    }
    
    /* bottom nav */
    nav.bottom {
      position: fixed;
      left: 0;
      right: 0;
      bottom: 0;
      max-width: 460px;
      margin: 0 auto;
      padding: 12px 16px;
      background: rgba(7, 10, 20, 0.8);
      box-shadow: 0 -5px 25px rgba(0, 0, 0, 0.2);
      border-radius: 20px 20px 0 0;
      display: flex;
      gap: 8px;
      z-index: 40;
      backdrop-filter: blur(10px);
      border-top: 1px solid var(--card-border);
    }
    
    .tab {
      flex: 1;
      text-align: center;
      padding: 10px 6px;
      border-radius: 12px;
      color: var(--text-muted);
      font-weight: 500;
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
    }
    
    .tab i {
      font-size: 18px;
      transition: all 0.3s ease;
    }
    
    .tab.active {
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      color: #fff;
      box-shadow: 0 5px 15px rgba(106, 17, 203, 0.3);
    }
    
    .tab.active i {
      transform: translateY(-2px);
    }
    
    .tab-text {
      font-size: 12px;
      font-weight: 500;
    }
    
    /* form */
    label {
      display: block;
      color: var(--text-secondary);
      font-size: 13px;
      margin-bottom: 8px;
      font-weight: 500;
    }
    
    input[type="text"], input[type="number"] {
      width: 100%;
      padding: 14px;
      border-radius: 12px;
      border: 1px solid var(--card-border);
      background: rgba(15, 23, 42, 0.4);
      color: var(--text-primary);
      outline: none;
      font-family: 'Poppins', sans-serif;
      font-size: 14px;
      transition: all 0.3s ease;
    }
    
    input[type="text"]:focus, input[type="number"]:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 2px rgba(106, 17, 203, 0.2);
    }
    
    .small {
      font-size: 13px;
      color: var(--text-muted);
      line-height: 1.5;
    }
    
    /* flushbar */
    .toast {
      position: fixed;
      left: 50%;
      transform: translateX(-50%);
      top: 20px;
      padding: 14px 20px;
      border-radius: 12px;
      z-index: 9999;
      color: #fff;
      display: none;
      font-weight: 500;
      box-shadow: 0 5px 20px rgba(0, 0, 0, 0.15);
      backdrop-filter: blur(10px);
      max-width: 90%;
      text-align: center;
    }
    
    .toast.show {
      display: block;
      animation: toastIn 0.4s ease;
    }
    
    .toast.success {
      background: rgba(16, 185, 129, 0.9);
      border: 1px solid rgba(16, 185, 129, 0.3);
    }
    
    .toast.error {
      background: rgba(239, 68, 68, 0.9);
      border: 1px solid rgba(239, 68, 68, 0.3);
    }
    
    .toast.warning {
      background: rgba(245, 158, 11, 0.9);
      border: 1px solid rgba(245, 158, 11, 0.3);
    }
    
    @keyframes toastIn {
      from { 
        opacity: 0; 
        transform: translate(-50%, -10px); 
      }
      to { 
        opacity: 1; 
        transform: translate(-50%, 0); 
      }
    }
    
    /* loading */
    .loading-full {
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 240px;
    }
    
    .spinner {
      border: 3px solid rgba(255, 255, 255, 0.1);
      border-top: 3px solid var(--accent-2);
      width: 42px;
      height: 42px;
      border-radius: 50%;
      animation: spin 1s linear infinite;
    }
    
    @keyframes spin {
      to { transform: rotate(360deg); }
    }
    
    /* Stats cards */
    .stats-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      margin: 16px 0;
    }
    
    .stat-card {
      background: rgba(255, 255, 255, 0.03);
      border-radius: 12px;
      padding: 14px;
      text-align: center;
      border: 1px solid var(--card-border);
    }
    
    .stat-value {
      font-size: 20px;
      font-weight: 700;
      margin: 6px 0;
      color: var(--text-primary);
    }
    
    .stat-label {
      font-size: 12px;
      color: var(--text-muted);
    }
    
    /* Referral section */
    .referral-box {
      background: linear-gradient(135deg, rgba(106, 17, 203, 0.1), rgba(37, 117, 252, 0.1));
      border-radius: 12px;
      padding: 16px;
      text-align: center;
      margin: 16px 0;
      border: 1px solid var(--card-border);
    }
    
    .referral-code {
      font-size: 22px;
      font-weight: 700;
      color: #ffd54a;
      letter-spacing: 1px;
      margin: 10px 0;
      padding: 10px;
      background: rgba(0, 0, 0, 0.2);
      border-radius: 8px;
      font-family: monospace;
    }
    
    /* Ad placeholder */
    .ad-container {
      background: linear-gradient(135deg, rgba(255, 255, 255, 0.03), rgba(255, 255, 255, 0.05));
      border-radius: 12px;
      padding: 20px;
      text-align: center;
      margin: 20px 0;
      border: 1px dashed var(--card-border);
      color: var(--text-muted);
    }
    
    /* Divider */
    .divider {
      height: 1px;
      background: var(--card-border);
      margin: 20px 0;
      position: relative;
    }
    
    .divider-text {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: var(--card);
      padding: 0 12px;
      font-size: 12px;
      color: var(--text-muted);
    }
    
    /* Animation for wheel win */
    @keyframes confetti {
      0% { transform: translateY(0) rotate(0); opacity: 1; }
      100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
    }
    
    .confetti {
      position: fixed;
      width: 10px;
      height: 10px;
      background: #ffd54a;
      opacity: 0;
      z-index: 1000;
      pointer-events: none;
    }
    
    /* small screens */
    @media (max-width: 420px) {
      header.app-head {
        padding: 14px 16px;
      }
      
      .logo {
        width: 40px;
        height: 40px;
        font-size: 16px;
      }
      
      .wheel-container {
        width: 260px;
        height: 260px;
      }
      
      .card {
        padding: 16px;
      }
      
      nav.bottom {
        padding: 10px 14px;
      }
      
      .tab i {
        font-size: 16px;
      }
      
      .tab-text {
        font-size: 11px;
      }
    }
  </style>
</head>
<body>
  <div class="app" role="application" aria-labelledby="app-title">
    <header class="app-head">
      <div class="brand">
        <div class="logo" aria-hidden="true">
          <i class="fas fa-coins"></i>
        </div>
        <div>
          <h1 id="app-title">Spin & Earn</h1>
          <p class="subtitle">Play daily • Withdraw via UPI</p>
        </div>
      </div>
      <div class="balance" aria-live="polite">
        <i class="fas fa-coins"></i>
        <span id="points-balance">0</span> pts
      </div>
    </header>

    <main class="content" id="main-content">
      <!-- Loading placeholder -->
      <div id="loading" class="card loading-full">
        <div>
          <div class="spinner" role="status" aria-hidden="true"></div>
          <p class="center small" style="margin-top:16px">Loading... please open inside Telegram</p>
        </div>
      </div>

      <!-- Spin page -->
      <section id="page-spin" class="card" hidden>
        <div class="center">
          <h2>Spin the Wheel</h2>
          <p class="muted">Spin daily to earn rewards and points</p>
          <div class="spins-counter">
            <i class="fas fa-sync-alt"></i>
            <span id="spins-left">0</span> / 5 spins today
          </div>
        </div>

        <div class="wheel-container">
          <div class="wheel-wrap" aria-hidden="false">
            <div class="wheel-pointer" aria-hidden="true"></div>
            <div id="wheel" class="wheel" role="img" aria-label="Prize wheel"></div>
          </div>
        </div>

        <button id="btn-spin" class="spin-btn" aria-label="Spin the wheel">
          <i class="fas fa-play-circle" style="margin-right: 8px;"></i> SPIN TO EARN
        </button>
        
        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-label">Today's Earnings</div>
            <div class="stat-value" id="today-earnings">₹0.00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">Total Earnings</div>
            <div class="stat-value" id="total-earnings">₹0.00</div>
          </div>
        </div>
      </section>

      <!-- Wallet page -->
      <section id="page-wallet" class="card" hidden>
        <h2><i class="fas fa-wallet" style="margin-right: 10px;"></i> Wallet</h2>
        
        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-label">Total Points</div>
            <div class="stat-value" id="wallet-points">0</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">Balance</div>
            <div class="stat-value" id="wallet-balance">₹0.00</div>
          </div>
        </div>

        <div class="divider"><span class="divider-text">Withdraw Funds</span></div>

        <h3>Request Withdrawal</h3>
        <div>
          <label for="upi"><i class="fas fa-id-card"></i> UPI ID</label>
          <input id="upi" type="text" inputmode="email" placeholder="yourname@bank" autocomplete="off" />
        </div>
        <div style="margin-top:16px">
          <label for="amount"><i class="fas fa-indian-rupee-sign"></i> Amount (Min ₹100)</label>
          <input id="amount" type="number" min="100" placeholder="100" />
        </div>
        <button id="btn-withdraw" class="spin-btn" style="margin-top:16px">
          <i class="fas fa-paper-plane" style="margin-right: 8px;"></i> Request Withdrawal
        </button>
        <p class="small muted" style="margin-top:12px">(1000 points = ₹1). Withdrawals are reviewed by admin.</p>

        <!-- Banner ad placeholder -->
        <div class="ad-container">
          <i class="fas fa-ad" style="font-size: 24px; margin-bottom: 8px;"></i>
          <p class="small">Advertisement</p>
        </div>
      </section>

      <!-- Profile page -->
      <section id="page-profile" class="card" hidden>
        <h2><i class="fas fa-user" style="margin-right: 10px;"></i> Profile</h2>
        
        <div style="margin-top:16px">
          <div class="small">Name</div>
          <div id="profile-name" style="font-weight:600; font-size: 16px;">-</div>
        </div>
        <div style="margin-top:16px">
          <div class="small">User ID</div>
          <div id="profile-id" style="font-weight:600; font-size: 14px; color: var(--text-muted);">-</div>
        </div>

        <div class="divider"><span class="divider-text">Referral Program</span></div>

        <h3>Invite Friends & Earn</h3>
        <div class="referral-box">
          <p class="small muted">Share your code with friends and earn ₹2 when they sign up</p>
          <div class="referral-code" id="ref-code">-</div>
          <p class="small muted">Tap to copy</p>
        </div>

        <div style="margin-top:20px">
          <label for="friend-code"><i class="fas fa-user-plus"></i> Apply a friend's code (one-time)</label>
          <input id="friend-code" type="text" placeholder="Friend's code" />
          <button id="btn-apply-code" class="spin-btn" style="margin-top:12px">
            <i class="fas fa-check-circle" style="margin-right: 8px;"></i> Apply Code
          </button>
        </div>

        <!-- small banner ad -->
        <div class="ad-container">
          <i class="fas fa-ad" style="font-size: 24px; margin-bottom: 8px;"></i>
          <p class="small">Advertisement</p>
        </div>
      </section>
    </main>

    <!-- bottom navigation -->
    <nav class="bottom" role="navigation" aria-label="Main Navigation">
      <div class="tab active" data-target="page-spin" role="button" tabindex="0" aria-pressed="true">
        <i class="fas fa-circle-dot"></i>
        <div class="tab-text">Spin</div>
      </div>
      <div class="tab" data-target="page-wallet" role="button" tabindex="0" aria-pressed="false">
        <i class="fas fa-wallet"></i>
        <div class="tab-text">Wallet</div>
      </div>
      <div class="tab" data-target="page-profile" role="button" tabindex="0" aria-pressed="false">
        <i class="fas fa-user"></i>
        <div class="tab-text">Profile</div>
      </div>
    </nav>

    <!-- toasts -->
    <div id="toast" class="toast" role="status" aria-live="polite"></div>
  </div>

  <script>
  (function () {
    'use strict';

    /* ============================
       CONFIG: Replace with your Firebase config if needed
       ============================ */
    const firebaseConfig = {
      apiKey: "AIzaSyAVnYewj5yds3gLBOJYlRhkW5dSlsa4xM8",
      authDomain: "spintoearn247.firebaseapp.com",
      databaseURL: "https://spintoearn247-default-rtdb.firebaseio.com",
      projectId: "spintoearn247",
      storageBucket: "spintoearn247.firebasestorage.app",
      messagingSenderId: "75167281143",
      appId: "1:75167281143:web:255976358947a657cf08f2"
    };

    /* ============================
       Initialize Firebase & Firestore
       ============================ */
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    const FieldValue = firebase.firestore.FieldValue;

    /* ============================
       App state & constants
       ============================ */
    const MAX_SPINS_PER_DAY = 5;
    const POINTS_PER_RUPEE = 1000;
    // Wheel segments (titles, point values, colors)
    const SEGMENTS = [
      { title: 'Nothing', value: 0, color: '#64748b' },
      { title: 'Mini', value: 10, color: '#f87171' },
      { title: 'Mega', value: 15, color: '#fb7185' },
      { title: 'Lucky', value: 20, color: '#e879f9' },
      { title: 'Jackpot', value: 25, color: '#a78bfa' },
      { title: 'Medium', value: 12, color: '#818cf8' },
      { title: 'Grand', value: 30, color: '#60a5fa' },
      { title: 'Winner', value: 100, color: '#38bdf8' }
    ];

    // DOM
    const loadingEl = document.getElementById('loading');
    const pageSpin = document.getElementById('page-spin');
    const pageWallet = document.getElementById('page-wallet');
    const pageProfile = document.getElementById('page-profile');
    const wheelEl = document.getElementById('wheel');
    const btnSpin = document.getElementById('btn-spin');
    const spinsLeftEl = document.getElementById('spins-left');
    const pointsBalanceEl = document.getElementById('points-balance');
    const walletPointsEl = document.getElementById('wallet-points');
    const walletBalanceEl = document.getElementById('wallet-balance');
    const todayEarningsEl = document.getElementById('today-earnings');
    const totalEarningsEl = document.getElementById('total-earnings');
    const upiInput = document.getElementById('upi');
    const amountInput = document.getElementById('amount');
    const withdrawBtn = document.getElementById('btn-withdraw');
    const refCodeEl = document.getElementById('ref-code');
    const applyCodeBtn = document.getElementById('btn-apply-code');
    const friendCodeInput = document.getElementById('friend-code');
    const toastEl = document.getElementById('toast');

    // Telegram WebApp
    const tg = window.Telegram?.WebApp;
    if (tg && tg.expand) {
      try { tg.expand(); } catch (e) { /* ignore */ }
    }

    // Local runtime state
    let currentUser = null;
    let userDoc = null;
    let isSpinning = false;
    let currentRotation = 0;

    /* ============================
       Helper UI functions
       ============================ */
    function showToast(message, type = 'success') {
      toastEl.textContent = message;
      toastEl.className = `toast ${type} show`;
      setTimeout(() => { toastEl.className = 'toast'; }, 3000);
    }

    function showLoading(state) {
      loadingEl.hidden = !state;
      if (state) {
        pageSpin.hidden = true;
        pageWallet.hidden = true;
        pageProfile.hidden = true;
      }
    }

    function updateUIFromUserData() {
      if (!userDoc) return;
      const data = userDoc;
      pointsBalanceEl.textContent = data.points ?? 0;
      walletPointsEl.textContent = data.points ?? 0;
      walletBalanceEl.textContent = `₹${((data.points ?? 0) / POINTS_PER_RUPEE).toFixed(2)}`;
      todayEarningsEl.textContent = `₹${(data.todayEarning || 0).toFixed(2)}`;
      totalEarningsEl.textContent = `₹${(data.totalEarnings || 0).toFixed(2)}`;
      spinsLeftEl.textContent = Math.max(0, MAX_SPINS_PER_DAY - (data.spinsToday ?? 0));
      refCodeEl.textContent = data.myReferralCode ?? '—';
      document.getElementById('profile-name').textContent = data.name ?? '-';
      document.getElementById('profile-id').textContent = (currentUser && currentUser.id) ? currentUser.id.toString() : '-';
      
      // Make referral code copyable
      if (data.myReferralCode) {
        refCodeEl.style.cursor = 'pointer';
        refCodeEl.onclick = function() {
          navigator.clipboard.writeText(data.myReferralCode);
          showToast('Referral code copied!');
        };
      }
      
      // disable apply code if already referred
      if (data.referredBy) {
        friendCodeInput.value = 'ALREADY APPLIED';
        friendCodeInput.disabled = true;
        applyCodeBtn.disabled = true;
      } else {
        friendCodeInput.disabled = false;
        applyCodeBtn.disabled = false;
      }
      
      // enable/disable spin button by spins left and state
      const spinsDoneToday = data.spinsToday ?? 0;
      btnSpin.disabled = isSpinning || (spinsDoneToday >= MAX_SPINS_PER_DAY);
    }

    /* ============================
       Utility: date string for "day" uniqueness (YYYY-MM-DD)
       ============================ */
    function getDateKey(d = new Date()) {
      const y = d.getFullYear();
      const m = String(d.getMonth() + 1).padStart(2, '0');
      const day = String(d.getDate()).padStart(2, '0');
      return `${y}-${m}-${day}`;
    }

    /* ============================
       Render wheel as SVG (segments)
       ============================ */
    function renderWheel() {
      const svgNS = "http://www.w3.org/2000/svg";
      const size = 300;
      const center = size / 2;
      const radius = size / 2;
      const segCount = SEGMENTS.length;
      const segAngle = 360 / segCount;

      const svg = document.createElementNS(svgNS, "svg");
      svg.setAttribute("viewBox", `0 0 ${size} ${size}`);
      svg.setAttribute("width", "100%");
      svg.setAttribute("height", "100%");

      // background circle
      const bg = document.createElementNS(svgNS, "circle");
      bg.setAttribute("cx", center);
      bg.setAttribute("cy", center);
      bg.setAttribute("r", radius);
      bg.setAttribute("fill", "transparent");
      svg.appendChild(bg);

      // draw each segment
      for (let i = 0; i < segCount; i++) {
        const startAngle = i * segAngle;
        const endAngle = startAngle + segAngle;
        const startRad = (startAngle - 90) * Math.PI / 180;
        const endRad = (endAngle - 90) * Math.PI / 180;
        const x1 = center + radius * Math.cos(startRad);
        const y1 = center + radius * Math.sin(startRad);
        const x2 = center + radius * Math.cos(endRad);
        const y2 = center + radius * Math.sin(endRad);
        const largeArc = segAngle > 180 ? 1 : 0;
        const path = document.createElementNS(svgNS, "path");
        path.setAttribute("d", `M ${center},${center} L ${x1},${y1} A ${radius},${radius} 0 ${largeArc} 1 ${x2},${y2} Z`);
        path.setAttribute("fill", SEGMENTS[i].color);
        svg.appendChild(path);

        // label (midpoint)
        const midAngle = startAngle + segAngle / 2;
        const midRad = (midAngle - 90) * Math.PI / 180;
        const labelRadius = radius * 0.62;
        const lx = center + labelRadius * Math.cos(midRad);
        const ly = center + labelRadius * Math.sin(midRad);
        const text = document.createElementNS(svgNS, "text");
        text.setAttribute("x", lx);
        text.setAttribute("y", ly);
        text.setAttribute("fill", "#fff");
        text.setAttribute("font-size", "11");
        text.setAttribute("font-weight", "700");
        text.setAttribute("text-anchor", "middle");
        text.setAttribute("alignment-baseline", "middle");
        text.textContent = SEGMENTS[i].value > 0 ? `${SEGMENTS[i].title} ${SEGMENTS[i].value}` : SEGMENTS[i].title;
        svg.appendChild(text);
      }

      // center circle
      const centerCircle = document.createElementNS(svgNS, "circle");
      centerCircle.setAttribute("cx", center);
      centerCircle.setAttribute("cy", center);
      centerCircle.setAttribute("r", radius * 0.15);
      centerCircle.setAttribute("fill", "rgba(0, 0, 0, 0.3)");
      centerCircle.setAttribute("stroke", "rgba(255, 255, 255, 0.1)");
      centerCircle.setAttribute("stroke-width", "2");
      svg.appendChild(centerCircle);

      // outer ring
      const ring = document.createElementNS(svgNS, "circle");
      ring.setAttribute("cx", center);
      ring.setAttribute("cy", center);
      ring.setAttribute("r", radius - 2);
      ring.setAttribute("fill", "transparent");
      ring.setAttribute("stroke", "rgba(255, 255, 255, 0.08)");
      ring.setAttribute("stroke-width", "2");
      svg.appendChild(ring);

      wheelEl.innerHTML = "";
      wheelEl.appendChild(svg);
    }

    /* ============================
       Load Telegram user & initialize Firestore user doc
       ============================ */
    async function initApp() {
      // Ensure Telegram provided user data
      if (!window.Telegram?.WebApp || !Telegram.WebApp.initDataUnsafe || !Telegram.WebApp.initDataUnsafe.user) {
        loadingEl.querySelector('.small').textContent = 'Open this inside Telegram (Web App).';
        return;
      }
      currentUser = Telegram.WebApp.initDataUnsafe.user;
      // Build userId string
      const userId = String(currentUser.id);

      // Ensure user doc exists (atomic create if missing)
      const userRef = db.collection('users').doc(userId);
      await db.runTransaction(async (tx) => {
        const docSnap = await tx.get(userRef);
        if (!docSnap.exists) {
          const referral = generateReferralCode(userId);
          const initial = {
            uid: userId,
            name: currentUser.first_name || (currentUser.username || 'Player'),
            username: currentUser.username || '',
            points: 100,                // welcome bonus
            totalEarnings: 0,
            todayEarning: 0,
            referredBy: null,
            myReferralCode: referral,
            upiId: '',
            spinsToday: 0,
            lastSpinDate: null,
            lastLoginDate: getDateKey(new Date()),
            createdAt: FieldValue.serverTimestamp(),
            updatedAt: FieldValue.serverTimestamp()
          };
          tx.set(userRef, initial);
          // store referral code mapping
          tx.set(db.collection('referralCodes').doc(referral), { uid: userId });
        }
      });

      // attach real-time listener
      userRef.onSnapshot(snap => {
        userDoc = snap.exists ? snap.data() : null;
        // If lastLoginDate isn't today, give daily bonus once
        checkAndApplyDailyBonus(userId).catch(console.error);
        showInitialUI();
        updateUIFromUserData();
      });
    }

    function showInitialUI() {
      // show pages & hide loading
      loadingEl.hidden = true;
      pageSpin.hidden = false;
      // show default tab
      showPage('page-spin');
    }

    /* ============================
       Referral code generation (deterministic-ish)
       ============================ */
    function generateReferralCode(uid) {
      // keep short & readable
      const short = String(uid).slice(-5);
      const rand = Math.floor(10 + Math.random() * 90);
      return `REF${short}${rand}`;
    }

    /* ============================
       Daily bonus: only once per day
       ============================ */
    async function checkAndApplyDailyBonus(uid) {
      if (!userDoc) return;
      try {
        const todayKey = getDateKey(new Date());
        if (userDoc.lastLoginDate === todayKey) return; // already awarded today
        // award random daily bonus 1000-3000 pts (i.e., ₹1-₹3)
        const bonusPts = (Math.floor(Math.random() * 3) + 1) * 1000;
        const userRef = db.collection('users').doc(uid);
        await db.runTransaction(async (tx) => {
          const snap = await tx.get(userRef);
          if (!snap.exists) return;
          const data = snap.data();
          // double-check lastLoginDate inside transaction
          if (data.lastLoginDate === todayKey) return;
          tx.update(userRef, {
            points: FieldValue.increment(bonusPts),
            totalEarnings: (data.totalEarnings || 0) + (bonusPts / POINTS_PER_RUPEE),
            todayEarning: (data.lastLoginDate === todayKey ? data.todayEarning || 0 : 0) + (bonusPts / POINTS_PER_RUPEE),
            lastLoginDate: todayKey,
            updatedAt: FieldValue.serverTimestamp()
          });
        });
        showToast(`Daily bonus: ${(bonusPts / POINTS_PER_RUPEE).toFixed(2)} ₹ credited!`);
      } catch (err) {
        console.error('Daily bonus error', err);
      }
    }

    /* ============================
       Confetti animation for wins
       ============================ */
    function createConfetti() {
      const colors = ['#ffd54a', '#6a11cb', '#2575fc', '#10b981', '#ef4444'];
      for (let i = 0; i < 30; i++) {
        const confetti = document.createElement('div');
        confetti.className = 'confetti';
        confetti.style.left = Math.random() * 100 + 'vw';
        confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
        confetti.style.width = (5 + Math.random() * 10) + 'px';
        confetti.style.height = (5 + Math.random() * 10) + 'px';
        confetti.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
        document.body.appendChild(confetti);
        
        const animation = confetti.animate([
          { transform: `translateY(0) rotate(0)`, opacity: 1 },
          { transform: `translateY(100vh) rotate(${720 + Math.random() * 360}deg)`, opacity: 0 }
        ], {
          duration: 2000 + Math.random() * 3000,
          easing: 'cubic-bezier(0.1, 0.8, 0.3, 1)'
        });
        
        animation.onfinish = () => confetti.remove();
      }
    }

    /* ============================
       Spin flow (safe with runTransaction)
       ============================ */
    btnSpin.addEventListener('click', async () => {
      if (isSpinning) return;
      if (!currentUser) { showToast('Not opened inside Telegram', 'error'); return; }
      const userId = String(currentUser.id);

      try {
        // 1) attempt to reserve a spin in a transaction (prevents two simultaneous spins)
        const userRef = db.collection('users').doc(userId);
        const reserved = await db.runTransaction(async (tx) => {
          const snap = await tx.get(userRef);
          if (!snap.exists) throw new Error('User not found');
          const data = snap.data();
          const lastSpinDate = data.lastSpinDate || null;
          const todayKey = getDateKey(new Date());
          let spinsToday = data.spinsToday || 0;
          // reset spins if lastSpinDate is not today
          if (lastSpinDate !== todayKey) spinsToday = 0;
          if (spinsToday >= MAX_SPINS_PER_DAY) {
            return { ok: false, reason: 'Daily spin limit reached' };
          }
          // reserve spin: increment spinsToday and set provisional lastSpinDate to today
          tx.update(userRef, {
            spinsToday: FieldValue.increment(1),
            lastSpinDate: todayKey,
            updatedAt: FieldValue.serverTimestamp()
          });
          return { ok: true };
        });

        if (!reserved.ok) {
          showToast(reserved.reason || 'Cannot spin', 'error');
          return;
        }

        // 2) Show (mock) rewarded ad.
        isSpinning = true;
        btnSpin.disabled = true;
        await playRewardedAdMock();

        // 3) Determine prize fairly
        const prize = pickRandomPrize();
        // animate wheel (deterministic final segment)
        await animateWheelToPrize(prize.index);

        // 4) Credit prize in transaction (ensure we add points atomically)
        await db.runTransaction(async (tx) => {
          const uRef = db.collection('users').doc(userId);
          const s = await tx.get(uRef);
          if (!s.exists) throw new Error('User missing during credit');
          tx.update(uRef, {
            points: FieldValue.increment(prize.value),
            totalEarnings: (s.data().totalEarnings || 0) + (prize.value / POINTS_PER_RUPEE),
            todayEarning: (s.data().todayEarning || 0) + (prize.value / POINTS_PER_RUPEE),
            updatedAt: FieldValue.serverTimestamp()
          });
        });

        // Show confetti for wins over 0
        if (prize.value > 0) {
          createConfetti();
        }
        
        showToast(`You won ${prize.value} points!`);
      } catch (err) {
        console.error('Spin error', err);
        showToast(err.message || 'Spin failed', 'error');
      } finally {
        isSpinning = false;
        // small delay to allow UI update via snapshot listener
        setTimeout(() => {
          if (userDoc) updateUIFromUserData();
          btnSpin.disabled = false;
        }, 300);
      }
    });

    /* Pick random prize index & value based on equal sectors */
    function pickRandomPrize() {
      // uniform random index
      const idx = Math.floor(Math.random() * SEGMENTS.length);
      return { index: idx, value: SEGMENTS[idx].value };
    }

    /* Animate wheel rotation so pointer lands at target segment index */
    function animateWheelToPrize(targetIndex) {
      return new Promise((resolve) => {
        const segments = SEGMENTS.length;
        const anglePerSeg = 360 / segments;
        // randomize exact stopping within the target slice to look natural
        const minAngle = targetIndex * anglePerSeg;
        const maxAngle = minAngle + anglePerSeg;
        // We want the wheel to rotate multiple full turns plus the target offset.
        // Because pointer is at top (0deg), we compute rotation so that the target slice centers at top.
        const midAngle = (minAngle + maxAngle) / 2;
        // compute the needed rotation so that wheel's rotation % 360 == midAngle
        const rounds = 6; // number of complete rotations (visual)
        const targetRotation = rounds * 360 + midAngle;
        // animate from currentRotation to new rotation
        currentRotation = (currentRotation % 360);
        const finalRotation = currentRotation + (targetRotation - (currentRotation % 360));
        // set transform
        wheelEl.style.transition = 'transform 4.8s cubic-bezier(.17,.67,.21,.96)';
        requestAnimationFrame(() => {
          wheelEl.style.transform = `rotate(${finalRotation}deg)`;
        });
        // cleanup after animation
        setTimeout(() => {
          // keep currentRotation in sync
          currentRotation = finalRotation % 360;
          // remove inline transition to allow next quick changes
          wheelEl.style.transition = '';
          resolve();
        }, 5000);
      });
    }

    /* Mocked rewarded ad flow */
    function playRewardedAdMock() {
      return new Promise((resolve) => {
        showToast('Playing rewarded ad (mock) — please wait...', 'warning');
        setTimeout(() => resolve(), 2500);
      });
    }

    /* ============================
       Withdraw request (atomic)
       ============================ */
    withdrawBtn.addEventListener('click', async () => {
      if (!currentUser || !userDoc) return showToast('User not ready', 'error');
      const userId = String(currentUser.id);
      const upi = upiInput.value.trim();
      const amount = parseInt(amountInput.value, 10);

      if (!upi) return showToast('Enter valid UPI ID', 'error');
      if (!amount || amount < 100) return showToast('Minimum withdrawal is ₹100', 'error');

      const pointsNeeded = amount * POINTS_PER_RUPEE;
      try {
        const userRef = db.collection('users').doc(userId);
        await db.runTransaction(async (tx) => {
          const snap = await tx.get(userRef);
          if (!snap.exists) throw new Error('User missing');
          const pts = snap.data().points || 0;
          if (pts < pointsNeeded) throw new Error('Not enough points');
          // create withdrawal request and deduct points atomically via batch-like updates inside transaction
          const withdrawRef = db.collection('withdrawalRequests').doc(); // auto id
          tx.set(withdrawRef, {
            uid: userId,
            upiId: upi,
            amount: amount,
            status: 'pending',
            createdAt: FieldValue.serverTimestamp()
          });
          tx.update(userRef, { points: FieldValue.increment(-pointsNeeded), updatedAt: FieldValue.serverTimestamp() });
        });
        showToast('Withdrawal requested — admin will review.');
        upiInput.value = '';
        amountInput.value = '';
      } catch (err) {
        console.error('Withdraw error', err);
        showToast(err.message || 'Withdraw failed', 'error');
      }
    });

    /* ============================
       Apply referral code (safe)
       ============================ */
    applyCodeBtn.addEventListener('click', async () => {
      if (!currentUser || !userDoc) return showToast('User not ready', 'error');
      const selfUid = String(currentUser.id);
      const code = friendCodeInput.value.trim();
      if (!code) return showToast('Enter referral code', 'error');

      try {
        const codeRef = db.collection('referralCodes').doc(code);
        // We will run a transaction to ensure referer exists and user hasn't already used a code
        await db.runTransaction(async (tx) => {
          const codeSnap = await tx.get(codeRef);
          if (!codeSnap.exists) throw new Error('Invalid code');
          const refUid = codeSnap.data().uid;
          if (String(refUid) === selfUid) throw new Error("You can't use your own code");

          const meRef = db.collection('users').doc(selfUid);
          const meSnap = await tx.get(meRef);
          if (!meSnap.exists) throw new Error('User not found');
          const me = meSnap.data();
          if (me.referredBy) throw new Error('Referral already used');

          const refRef = db.collection('users').doc(String(refUid));
          const refSnap = await tx.get(refRef);
          if (!refSnap.exists) throw new Error('Referrer not found');

          const bonusPoints = 2000; // 2000 = ₹2
          // update both
          tx.update(meRef, { referredBy: code, points: FieldValue.increment(bonusPoints), updatedAt: FieldValue.serverTimestamp() });
          tx.update(refRef, { points: FieldValue.increment(bonusPoints), updatedAt: FieldValue.serverTimestamp() });
        });

        showToast('Referral applied — both received ₹2!');
        friendCodeInput.value = '';
      } catch (err) {
        console.error('Referral error', err);
        showToast(err.message || 'Referral failed', 'error');
      }
    });

    /* ============================
       Page navigation (bottom tabs)
       ============================ */
    document.querySelectorAll('.tab').forEach(tab => {
      tab.addEventListener('click', () => {
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        tab.classList.add('active');
        const target = tab.dataset.target;
        showPage(target);
      });
      tab.addEventListener('keydown', (ev) => {
        if (ev.key === 'Enter' || ev.key === ' ') { tab.click(); ev.preventDefault(); }
      });
    });

    function showPage(id) {
      pageSpin.hidden = (id !== 'page-spin');
      pageWallet.hidden = (id !== 'page-wallet');
      pageProfile.hidden = (id !== 'page-profile');
    }

    /* ============================
       Render wheel initially & start app
       ============================ */
    renderWheel();
    initApp().catch(err => {
      console.error('init error', err);
      showToast('Initialization error', 'error');
    });

  })();
  </script>
</body>
</html>
