<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI 行銷文案產生器</title>
  <!-- 載入 Google Fonts: Shippori Mincho (明朝體) & Noto Sans TC (無襯線體) -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&family=Shippori+Mincho:wght@500;700;800&display=swap" rel="stylesheet">
  
  <style>
    :root {
      --bg-color: #f5f3ee;
      --card-bg: #ffffff;
      --border-color: #d8d4cb;
      --primary-color: #b5703a; /* 陶土橘 */
      --primary-hover: #9c5c2d;
      --dark-bg: #2c2b28;
      --dark-text: #f5f3ee;
      --text-main: #3d3b36;
      --text-muted: #8c867a;
      --accent-green: #6e8b75; /* 抹茶綠 */
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-main);
      font-family: 'Noto Sans TC', sans-serif;
      line-height: 1.6;
      padding: 40px 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      width: 100%;
      max-width: 1200px; /* 拓寬容器以利電腦版雙邊排版 */
    }

    header {
      text-align: center;
      margin-bottom: 32px;
    }

    h1 {
      font-family: 'Shippori Mincho', serif;
      font-size: 2.2rem;
      font-weight: 800;
      color: var(--text-main);
      margin-bottom: 8px;
      letter-spacing: 0.05em;
    }

    .subtitle {
      font-size: 1rem;
      color: var(--text-muted);
      letter-spacing: 0.02em;
      margin-bottom: 16px;
    }

    /* 卡片主體 */
    .card {
      background-color: var(--card-bg);
      border: 1px solid var(--border-color);
      border-radius: 6px;
      padding: 32px;
      margin-bottom: 24px;
    }

    /* 控制面板與 API 設定 */
    .control-panel {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 24px;
      border-bottom: 1px dashed var(--border-color);
      padding-bottom: 16px;
      flex-wrap: wrap;
      gap: 12px;
    }

    .api-toggle-container {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    /* 電腦版左右分割版面，行動版垂直堆疊 */
    .workspace {
      display: grid;
      grid-template-columns: 1fr;
      gap: 32px;
      align-items: start;
    }

    @media (min-width: 992px) {
      .workspace {
        grid-template-columns: 1.1fr 0.9fr; /* 左右黃金分割比例 */
      }
    }

    /* 左右兩側面板樣式 */
    .input-panel {
      display: flex;
      flex-direction: column;
    }

    .output-panel {
      background-color: #fbfaf7;
      border: 1px dashed var(--border-color);
      border-radius: 6px;
      padding: 24px;
      min-height: 580px; /* 保持與左側輸入面板的大致平衡 */
      display: flex;
      flex-direction: column;
      justify-content: center; /* 無內容時將空狀態居中 */
    }

    /* 2x2 格狀輸入版面（在左側面板中改為直列，在電腦上更好閱讀與輸入） */
    .grid-layout {
      display: flex;
      flex-direction: column;
      gap: 20px;
      margin-bottom: 24px;
    }

    .input-group {
      display: flex;
      flex-direction: column;
    }

    .label-container {
      display: flex;
      align-items: center;
      margin-bottom: 8px;
    }

    .label-badge {
      background-color: var(--primary-color);
      color: #ffffff;
      font-size: 0.75rem;
      padding: 2px 6px;
      border-radius: 4px;
      margin-right: 8px;
      font-weight: 700;
    }

    label {
      font-family: 'Shippori Mincho', serif;
      font-weight: 700;
      font-size: 1.05rem;
      color: var(--text-main);
    }

    textarea, select, input[type="text"] {
      width: 100%;
      border: 1px solid var(--border-color);
      border-radius: 6px;
      padding: 12px;
      font-family: 'Noto Sans TC', sans-serif;
      font-size: 0.95rem;
      color: var(--text-main);
      outline: none;
      transition: border-color 0.2s ease;
      background-color: #fcfbf9;
    }

    textarea {
      height: 90px;
      resize: vertical;
    }

    textarea:focus, select:focus, input[type="text"]:focus {
      border-color: var(--primary-color);
      background-color: #ffffff;
    }

    .placeholder-tip {
      font-size: 0.8rem;
      color: var(--text-muted);
      margin-top: 6px;
      padding-left: 2px;
    }

    /* API 密鑰設定區 */
    .api-config-box {
      width: 100%;
      background-color: #fbfaf7;
      border: 1px solid var(--border-color);
      border-radius: 6px;
      padding: 16px;
      margin-bottom: 24px;
      display: none;
    }

    .api-config-title {
      font-family: 'Shippori Mincho', serif;
      font-weight: 700;
      font-size: 0.95rem;
      margin-bottom: 8px;
      color: var(--text-main);
    }

    /* 按鈕樣式 */
    .btn-outline {
      background: transparent;
      border: 1px solid var(--border-color);
      color: var(--text-muted);
      padding: 6px 12px;
      font-size: 0.85rem;
      border-radius: 6px;
      cursor: pointer;
      font-family: 'Noto Sans TC', sans-serif;
      transition: all 0.2s ease;
    }

    .btn-outline:hover, .btn-outline.active {
      border-color: var(--primary-color);
      color: var(--primary-color);
    }

    .btn-primary {
      background-color: var(--primary-color);
      color: #ffffff;
      border: none;
      border-radius: 6px;
      padding: 14px 28px;
      font-size: 1.05rem;
      font-weight: 500;
      cursor: pointer;
      font-family: 'Noto Sans TC', sans-serif;
      letter-spacing: 0.05em;
      width: 100%;
      transition: background-color 0.2s ease;
    }

    .btn-primary:hover {
      background-color: var(--primary-hover);
    }

    .btn-primary:disabled {
      background-color: var(--border-color);
      cursor: not-allowed;
    }

    /* 右側空狀態提示 */
    .empty-state {
      text-align: center;
      padding: 40px 20px;
      color: var(--text-muted);
    }

    .empty-icon {
      font-size: 3rem;
      margin-bottom: 16px;
      color: var(--primary-color);
    }

    .empty-title {
      font-family: 'Shippori Mincho', serif;
      font-weight: 700;
      font-size: 1.25rem;
      color: var(--text-main);
      margin-bottom: 12px;
    }

    .empty-state p {
      font-size: 0.9rem;
      max-width: 320px;
      margin: 0 auto;
      line-height: 1.6;
    }

    .result-section {
      display: none; /* 預設隱藏，生成後顯示 */
      width: 100%;
    }

    .result-title {
      font-family: 'Shippori Mincho', serif;
      font-weight: 700;
      font-size: 1.2rem;
      margin-bottom: 16px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    /* 文案切換 Tab */
    .tabs-header {
      display: flex;
      gap: 8px;
      margin-bottom: 16px;
      overflow-x: auto;
    }

    .tab-btn {
      background: #e8e5dd;
      border: none;
      color: var(--text-main);
      padding: 8px 12px;
      font-size: 0.85rem;
      border-radius: 6px 6px 0 0;
      cursor: pointer;
      font-family: 'Noto Sans TC', sans-serif;
      transition: all 0.2s ease;
      white-space: nowrap;
    }

    .tab-btn.active {
      background-color: var(--dark-bg);
      color: var(--dark-text);
      font-weight: 500;
    }

    /* 模擬程式碼區塊 */
    .code-wrapper {
      background-color: var(--dark-bg);
      border-radius: 6px;
      padding: 24px;
      position: relative;
      min-height: 320px;
    }

    .copy-btn-container {
      display: flex;
      justify-content: flex-end;
      margin-bottom: 12px;
    }

    pre {
      white-space: pre-wrap;
      word-wrap: break-word;
      font-family: 'Noto Sans TC', sans-serif;
      font-size: 0.95rem;
      color: var(--dark-text);
      margin: 0;
      line-height: 1.7;
    }

    /* 載入中動畫 */
    .loader {
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 16px;
      color: var(--primary-color);
      font-weight: 500;
      padding: 40px 20px;
    }

    .spinner {
      width: 32px;
      height: 32px;
      border: 3px solid #e8e5dd;
      border-top: 3px solid var(--primary-color);
      border-radius: 50%;
      animation: spin 1s linear infinite;
    }

    /* 複製提示泡泡 */
    .toast-msg {
      position: absolute;
      top: -40px;
      right: 0;
      background-color: var(--primary-color);
      color: #ffffff;
      font-size: 0.75rem;
      padding: 4px 8px;
      border-radius: 4px;
      white-space: nowrap;
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.2s ease, visibility 0.2s ease;
    }

    .toast-msg.show {
      opacity: 1;
      visibility: visible;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    /* 響應式調整 */
    @media (max-width: 991px) {
      body {
        padding: 20px 12px;
      }

      h1 {
        font-size: 1.8rem;
      }

      .card {
        padding: 20px;
      }

      .output-panel {
        min-height: auto;
      }
    }
  </style>
</head>
<body>

  <div class="container">
    <header>
      <h1>AI 行銷文案產生器</h1>
      <div class="subtitle">將您的產品特色，一鍵幻化為多平台瘋傳的爆紅文案</div>
    </header>

    <main class="card">
      <!-- 頂部控制與設定區 -->
      <div class="control-panel">
        <div>
          <button type="button" class="btn-outline" onclick="loadExample()">帶入午睡枕範例</button>
          <button type="button" class="btn-outline" onclick="loadTeaExample()">帶入冷萃茶範例</button>
        </div>
        <div class="api-toggle-container">
          <label style="font-size:0.85rem; cursor:pointer;" for="api-toggle">啟用真實 Gemini AI 模式</label>
          <input type="checkbox" id="api-toggle" onchange="toggleApiMode()">
        </div>
      </div>

      <!-- Real AI API Key 配置區 -->
      <div class="api-config-box" id="api-config-box">
        <div class="api-config-title">🔑 設定您的 Google Gemini API 金鑰</div>
        <div style="display:flex; gap:10px;">
          <input type="text" id="api-key-input" placeholder="在此貼上您的 AI Studio API Key (AIzaSy...)" style="flex: 1;">
          <a href="https://aistudio.google.com/" target="_blank" class="btn-outline" style="text-decoration:none; display:flex; align-items:center;">獲取免費 Key</a>
        </div>
        <p class="placeholder-tip" style="color:var(--accent-green); font-weight:500; margin-top:8px;">💡 啟用此模式將使用真實的 Gemini 2.5 進行動態生成，不填寫則預設使用離線智能模板引擎。</p>
      </div>

      <div class="workspace">
        
        <!-- 左側面板：輸入與參數設定 -->
        <div class="input-panel">
          <form id="generator-form" onsubmit="event.preventDefault(); handleGeneration();">
            <div class="grid-layout">
              <!-- 1. 產品名稱與特色 -->
              <div class="input-group">
                <div class="label-container">
                  <span class="label-badge">① 產品</span>
                  <label for="input-product">產品名稱與特色</label>
                </div>
                <textarea id="input-product" required placeholder="例如：好眠人體工學午睡枕。特色為 3D 支撐護頸、100% 記憶棉、中空透氣設計不壓迫眼球。"></textarea>
                <div class="placeholder-tip">填入產品核心名稱、關鍵特色或規格。</div>
              </div>

              <!-- 2. 目標受眾 -->
              <div class="input-group">
                <div class="label-container">
                  <span class="label-badge">② 受眾</span>
                  <label for="input-audience">目標受眾 (TA)</label>
                </div>
                <textarea id="input-audience" required placeholder="例如：長期肩頸酸痛、中午只能趴在桌上睡覺的辦公室白領上班族。"></textarea>
                <div class="placeholder-tip">描繪出客戶畫像、痛點與使用場景。</div>
              </div>

              <!-- 3. 語氣風格 -->
              <div class="input-group">
                <div class="label-container">
                  <span class="label-badge">③ 風格</span>
                  <label for="input-tone">品牌語氣風格</label>
                </div>
                <select id="input-tone">
                  <option value="empathy">溫暖同理（溫柔傾聽、解決上班族的痛點）</option>
                  <option value="humorous">幽默風趣（社群爆點、令人會心一笑）</option>
                  <option value="urgent">限時催單（促銷感強、刺激立即行動）</option>
                  <option value="professional">專業客觀（科學數據、專家背書感）</option>
                </select>
                <div class="placeholder-tip">設定文案與受眾對話時的面貌與氣氛。</div>
              </div>

              <!-- 4. 平台格式 -->
              <div class="input-group">
                <div class="label-container">
                  <span class="label-badge">④ 平台</span>
                  <label for="input-platform">發佈平台與格式</label>
                </div>
                <select id="input-platform">
                  <option value="facebook">Facebook 長文案（著重痛點引導與完整權益）</option>
                  <option value="instagram">Instagram 貼文（著重視覺引導與條列式亮點）</option>
                  <option value="threads">Threads 幽默短文（字數短、高互動、時事共鳴）</option>
                  <option value="edm">EDM 電子報（含推薦信件主旨與行動呼籲）</option>
                </select>
                <div class="placeholder-tip">針對不同社群媒介的閱讀習慣，調整排版長短。</div>
              </div>

              <!-- 5. 外部參考網址 -->
              <div class="input-group">
                <div class="label-container">
                  <span class="label-badge">⑤ 參考</span>
                  <label for="input-reference">外部參考資訊網址</label>
                </div>
                <input type="text" id="input-reference" placeholder="例如：https://example.com/product-details（選填）">
                <div class="placeholder-tip">貼上官方規格、新聞報導等連結，降低 AI 資訊幻覺與錯誤。</div>
              </div>
            </div>

            <!-- 觸發按鈕 -->
            <div class="action-container">
              <button type="submit" class="btn-primary" id="submit-btn">立即生成黃金行銷文案</button>
            </div>
          </form>
        </div>

        <div class="output-panel">
          <!-- 預設空狀態提示 -->
          <div class="empty-state" id="empty-state">
            <div class="empty-icon">✨</div>
            <div class="empty-title">等待文案生成</div>
            <p>請在左側填寫您的產品細節與受眾定位，點擊「立即生成黃金行銷文案」後，大師級文案將即時呈現在此處。</p>
          </div>

          <!-- 載入中動畫 -->
          <div class="loader" id="loading-spinner">
            <div class="spinner"></div>
            <span id="loading-text" style="font-size: 0.95rem;">文案大師正在腦力激盪中...</span>
          </div>

          <!-- 結果展示 -->
          <div class="result-section" id="result-section">
            <div class="result-title">
              <span>✨ 獨家生成的行銷切角文案</span>
            </div>
            
            <!-- 3 個不同切角的 Tabs 切換 -->
            <div class="tabs-header" id="tabs-header">
              <button type="button" class="tab-btn active" onclick="switchTab(0)" id="tab-0">切角一：黃金 AIDA 法</button>
              <button type="button" class="tab-btn" onclick="switchTab(1)" id="tab-1">切角二：PAS 痛點突擊</button>
              <button type="button" class="tab-btn" onclick="switchTab(2)" id="tab-2">切角三：情境催淚故事</button>
            </div>

            <div class="code-wrapper">
              <div class="copy-btn-container">
                <div style="position: relative;">
                  <div class="toast-msg" id="copy-toast">已複製文案！</div>
                  <button type="button" class="btn-outline" onclick="copyCurrentCopy()" style="color: var(--dark-text); border-color: var(--text-muted);">複製此版文案</button>
                </div>
              </div>
              <!-- 實際存放產出文案 -->
              <pre id="output-copy-text"></pre>
            </div>
          </div>
        </div>

      </div>
    </main>
  </div>

  <script>
    const examples = {
      napPillow: {
        product: "「好眠人體工學午睡枕」。特色：3D 弧度立體護頸、100% 慢回彈竹炭記憶棉、專利中空透氣設計，趴睡時眼球與呼吸不壓迫。",
        audience: "一到中午就只能趴在堅硬桌子上睡覺、醒來手麻眼花、肩頸痠痛長達數小時的辦公室上班族與久坐工程師。",
        tone: "empathy",
        platform: "threads",
        reference: "https://www.example.com/nap-pillow-specs"
      },
      coldTea: {
        product: "「日曬森林・冷萃綠茶」。特色：零卡無糖、12小時極低溫冷萃、毫無苦澀感，保留高濃度茶氨酸與滿滿的茶葉清香。",
        audience: "不愛喝白開水、想戒掉含糖手搖飲，但下午又需要健康提神、維持身體清爽的現代健康意識族群。",
        tone: "humorous",
        platform: "facebook",
        reference: ""
      }
    };

    // 存放當前生成的 3 個切角文案
    let generatedCopies = ["", "", ""];
    let activeTabIndex = 0;

    // 頁面初次載入
    window.addEventListener('DOMContentLoaded', () => {
      loadExample();
    });

    function loadExample() {
      document.getElementById('input-product').value = examples.napPillow.product;
      document.getElementById('input-audience').value = examples.napPillow.audience;
      document.getElementById('input-tone').value = examples.napPillow.tone;
      document.getElementById('input-platform').value = examples.napPillow.platform;
      document.getElementById('input-reference').value = examples.napPillow.reference;
    }

    function loadTeaExample() {
      document.getElementById('input-product').value = examples.coldTea.product;
      document.getElementById('input-audience').value = examples.coldTea.audience;
      document.getElementById('input-tone').value = examples.coldTea.tone;
      document.getElementById('input-platform').value = examples.coldTea.platform;
      document.getElementById('input-reference').value = examples.coldTea.reference;
    }

    // 切換 API Key 顯示
    function toggleApiMode() {
      const isChecked = document.getElementById('api-toggle').checked;
      const apiBox = document.getElementById('api-config-box');
      apiBox.style.display = isChecked ? 'block' : 'none';
    }

    function runSimulatedGenerator(product, audience, tone, platform, reference) {
      // 提取核心簡短詞彙
      const shortProd = product.split(/[。，、]/)[0].replace(/「|」/g, '');
      const shortAud = audience.split(/[。，、]/)[0];
      
      // 根據平台與風格客製的開頭/內文模塊
      let emoji = "💡";
      if (platform === 'threads') emoji = "💬";
      if (platform === 'instagram') emoji = "📸";
      if (platform === 'edm') emoji = "✉️";

      // AIDA 模版
      let copyAIDA = "";
      // PAS 模版
      let copyPAS = "";
      // 情境故事 模版
      let copyStory = "";

      // 1. AIDA (Attention, Interest, Desire, Action)
      if (platform === 'threads') {
        copyAIDA = `⚠️ 【脆友注意】你也是 ${shortAud} 嗎？\n\n每天中午用錯誤的姿勢休息，根本是慢性自殘。介紹你這個社群瘋傳的救星：${shortProd}！\n\n✨ 3D支撐＋極致透氣，再也不用體驗手腳發麻、眼睛被壓扁的痛苦。今天起，午休15分鐘，精神飽滿一整天。現在就點擊主頁連結了解更多，拯救你的午休時光！`;
        
        copyPAS = `有沒有人跟我一樣是 ${shortAud}？\n\n（痛點突擊）每次趴睡醒來，手麻到像不是自己的，眼睛還會視線模糊，真的超絕望…\n\n（解決方案）直到我用了「${shortProd}」，中空透氣跟3D立體支撐完全是黑科技。現在午睡醒來神清氣爽，下午再也不用狂灌咖啡。限時優惠中，快留言你的午睡痛點，我私訊你傳送門！`;
        
        copyStory = `剛剛午睡醒來手麻到差點把水杯摔了... 身為 ${shortAud} 我真的受夠了 😭\n\n聽朋友推薦換了「${shortProd}」，一開始還半信疑，結果昨天一用，整張臉放上去直接完美貼合，呼吸順暢，直接熟睡到打呼！\n\n現在每天中午都是我的高光時刻，懂的都懂。連結放留言處了，手麻救星不謝！`;
      } 
      else if (platform === 'instagram') {
        copyAIDA = `📣 辦公室美學必備！致所有努力奮鬥的 ${shortAud} ✨\n\n你是不是每天中午都在與硬梆梆的辦公桌搏鬥？\n👉 救星降臨：【${shortProd}】\n\n💡 為什麼你需要 it？\n- 專利3D支撐：完美貼合人體工學，保護你的頸椎\n- 100% 竹炭記憶棉：透氣不悶熱，不壓迫眼球\n- 美型外觀：午睡也可以很優雅\n\n👉 點擊首頁 Link 搶先看最新優惠 🛒\n#辦公室必備 #午睡枕 #上班族日常`;
        
        copyPAS = `😵 你也是「趴睡手麻」俱樂部的會員嗎？\n\n每次中午想補個眠，醒來卻是：\n❌ 肩頸比睡前更酸痛\n❌ 雙手麻痺到動彈不得\n❌ 臉上印著深深的紅印子\n\n這不是休息，這是身體的微型職災！\n你需要的是專為 ${shortAud} 設計的【${shortProd}】。\n\n中空專利透氣、極致回彈，徹底釋放你頭頸的壓力。即日起享限時體驗特惠，告別狼狽，優雅午睡！點擊圖片或連結看詳情👇`;
        
        copyStory = `「以前中午趴睡就像在受刑，現在卻是每天最期待的 15 分鐘。」\n\n這是一位 ${shortAud} 對【${shortProd}】的真實反饋。\n\n從前在桌上克難趴睡，下午上班總是偏頭痛、精神更差。自從把桌上的雜物換成了這個高質感午睡枕，中午短暫充電，下午腦袋清晰度直接拉滿！\n\n送給自己（或辦公室隔壁那位）一份最好的健康禮物吧 🎁 點擊 Bio 連結探索極致好眠！`;
      }
      else if (platform === 'edm') {
        copyAIDA = `主旨：【專屬信】給 ${shortAud}：中午15分鐘，決定你下午的生產力！\n\n親愛的夥伴，\n\n你是不是也發現，每次中午趴在桌上睡完覺，下午下午反而更加昏沉，甚至肩頸僵硬、手麻眼花？\n\n這不是你的問題，是你的「午睡姿勢」缺乏對的支撐。\n\n我們專為您推出了【${shortProd}】。\n採用人體工學 3D 設計，讓您即使在辦公桌前，也能享受如同躺在床上的無壓放鬆。經測試，能有效減輕頸椎負擔 80% 以上！\n\n【限時早鳥 8 折點此領取專屬優惠】`;
        
        copyPAS = `主旨：別再折磨你的頸椎了！辦公室裡的「隱形健康殺手」\n\n您好，\n\n身為 ${shortAud}，您是否每天都面臨這個惡性循環？\n- 中午疲憊，趴桌小憩\n- 醒來手腳發麻、眼壓過高、肩頸酸痛\n- 下午工作效率低落，只能拼命灌黑咖啡\n\n這不是小事，長期壓迫眼球與頸椎，可能會對身體造成永久性傷害。\n\n【${shortProd}】正是為解決此痛點而生。中空無壓設計，讓您呼吸順暢不壓眼，慢回彈記憶棉完美托護頭部。\n\n【點此立即拯救您的午休，享免運優惠】`;
        
        copyStory = `主旨：一場由 15 分鐘午睡引發的「辦公室革命」...\n\n您好，\n\n「以前我以為，中午趴著睡手麻是正常的。」\n\n這是一位跟您一樣的 ${shortAud} 寫給我們的分享信。他曾試過用各種抱枕，但總是覺得胸悶、呼吸不順、脖子酸痛。直到他遇見了【${shortProd}】。\n\n現在，他不只告別了手麻，下午的提案效率更是翻倍，甚至連隔壁同事都來問他在哪裡買的。\n\n這不僅僅是一顆枕頭，更是您對自己健康的投資。\n\n【點此觀看更多真實見證與體驗好康】`;
      }
      else { // Facebook 長文案
        copyAIDA = `🔥 【好物推薦】中午不睡，下午崩潰！致所有辛苦奮鬥的 ${shortAud} 👩‍💻👨‍💻\n\n你是否每天中午都面臨以下困擾：\n1️⃣ 趴在硬邦邦的桌面上，睡不到10分鐘就手麻腳發麻？\n2️⃣ 睡醒後眼睛因為長期被手臂壓迫，視線模糊好久才能恢復？\n3️⃣ 下午剛開始開會，脖子和肩膀就酸痛得無法集中注意力？\n\n這都是原因你缺少了【${shortProd}】！\n\n這款午睡枕專為現代高壓上班族研發，採用頂級人體工學3D支撐科技，完美貼合您的頸椎，中空專利設計更能徹底釋放眼球與呼吸道壓力，讓您趴睡也能深呼吸、零負擔！\n\n現在就提升您的午休品質，點選下方網址，一鍵搶購早鳥特惠！`;
        
        copyPAS = `🛑 警告：你每天中午的「趴睡姿勢」，可能正在摧毀你的頸椎健康！\n\n致所有親愛的 ${shortAud}：\n你以為中午趴在桌上睡覺是在休息嗎？事實上，錯誤的趴睡姿勢會讓頸椎承受正常站立時的 3 倍壓力！這也是為什麼你總是越睡越累、醒來手麻腳麻、甚至下午偏頭痛的原因。\n\n別再用手臂當枕頭了！\n\n你需要的是專為護頸設計的【${shortProd}】。透過3D立體包覆與特製記憶棉，它能精準分擔頭部重量，維持脊椎自然曲線。中午只需睡15分鐘，就能換來下午高效率的黃金狀態！\n\n點擊下方連結，為自己或心愛的人挑選一個健康的禮物👇`;
        
        copyStory = `「以前中午趴睡醒來像被打了一頓，現在我終於體驗到什麼叫『秒睡與深眠』！」\n\n這是一位身為 ${shortAud} 的資深會員，上週跟我們分享的真心話。過去他總是中午趴在桌上胡亂睡個10分鐘，下午開會就開始打瞌睡，肩頸貼滿了貼布。\n\n直到他決定試試【${shortProd}】。\n第一天用，他就被那種完美的頸部托護感震驚了！中空的設計讓呼吸無比順暢，手臂再也不會被壓到失去知覺。這不僅是一顆枕頭，更是他工作效率倍增的秘密武器！\n\n你值得擁有更好的午休品質。點擊連結，看更多辦公室神器的秘密開箱！`;
      }

      // 風格適度調整微調
      if (tone === 'humorous') {
        copyAIDA = "【幽默警報 🚨】" + copyAIDA.replace("特別介紹", "強烈推薦這款拯救靈魂的魔法道具").replace("你地址", "你是不是也被桌子家暴的");
        copyPAS = "【搞笑吐槽 🤪】" + copyPAS.replace("這不是休息", "再不改變，你中午午睡的姿勢看起來就像是在桌上超渡一樣");
      } else if (tone === 'urgent') {
        copyAIDA = "⚡【限時瘋搶・僅此一檔】⚡\n\n" + copyAIDA + "\n\n⚠️ 注意：首批早鳥名額有限，售完即恢復原價！";
        copyPAS = "⏰【倒數24小時・手慢無】⏰\n\n" + copyPAS + "\n\n🔥 庫存告急！現在下單現省500元！";
      } else if (tone === 'professional') {
        copyAIDA = "🔬【符合人體工學之科學實證研究】\n\n" + copyAIDA.replace("救星", "符合物理學骨骼支撐設計之工具");
        copyPAS = "📊【臨床專家指出：錯誤姿勢會增加頸椎病風險】\n\n" + copyPAS;
      }

      // 模擬附加參考資料查證提示
      if (reference) {
        copyAIDA += `\n\n*(ℹ️ 本文案已模擬參考外部資料網址進行資訊查證與校對: ${reference})*`;
        copyPAS += `\n\n*(ℹ️ 本文案已模擬參考外部資料網址進行資訊查證與校對: ${reference})*`;
        copyStory += `\n\n*(ℹ️ 本文案已模擬參考外部資料網址進行資訊查證與校對: ${reference})*`;
      }

      return [copyAIDA, copyPAS, copyStory];
    }

    async function runRealGeminiGenerator(product, audience, tone, platform, reference, apiKey) {
      const prompt = `你是一位精通轉換率優化（CRO）、神經行銷學與各大社群媒體特性（FB, IG, Threads, EDM）的資深行銷大師。
請根據以下輸入資料，為我撰寫 3 種完全不同行銷切角（Angle）的精彩行銷文案：

【產品資訊】：${product}
【目標受眾 (TA)】：${audience}
【品牌語氣風格】：${tone} (請完全融會貫通並反映在字裡行銷)
【發佈平台格式】：${platform}
${reference ? `【外部參考網址】：${reference} (請優先查證此網址之事實與規格資訊，以降低文案資訊錯誤率。若連結內容與輸入有出入，以此連結內容為準。)` : ''}

請產出以下 3 種切角的文案，並在文案中完美嵌入產品特色、解決痛點與強烈的呼籲行動(CTA)：
1. 切角一（黃金 AIDA 架構）：著重於注意力 Attraction -> 興趣 Interest -> 渴望 Desire -> 行動 Action 的經典遞進。
2. 切角二（PAS 痛點突擊法）：著重於先描述受眾遭遇的極端痛點 Problem -> 傷口撒鹽/情緒煽動 Agitate -> 帶出產品作為完美解決方案 Solve。
3. 切角三（情境式溫慢故事法）：用第一人稱或精美的情境故事，讓受眾產生強烈共鳴、感同身受，進而轉化。

請【嚴格遵守】以下輸出格式，並用繁體中文 (台灣) 撰寫，使用適當的 emoji 增加閱讀快感。
回覆格式：
===ANGLE_1===
(在此填入切角一的文案內容，不要有任何其他標記)
===ANGLE_2===
(在此填入切角二的文案內容，不要有任何其他標記)
===ANGLE_3===
(在此填入切角三的文案內容，不要有任何其他標記)`;

      try {
        const payload = {
          contents: [{ parts: [{ text: prompt }] }]
        };

        // 啟用 Google 搜尋 Grounding 以動態擷取/核實使用者輸入之網址
        if (reference) {
          payload.tools = [{ "google_search": {} }];
        }

        const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${apiKey}`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload)
        });

        if (!response.ok) {
          throw new Error(`API 請求失敗，狀態碼：${response.status}。請確認您的 API Key 是否有效。`);
        }

        const data = await response.json();
        const textResult = data.candidates[0].content.parts[0].text;
        
        let angle1 = "";
        let angle2 = "";
        let angle3 = "";

        if (textResult.includes("===ANGLE_1===")) {
          const parts = textResult.split(/===ANGLE_\d===/);
          angle1 = parts[1] ? parts[1].trim() : "（生成異常，請重試）";
          angle2 = parts[2] ? parts[2].trim() : "（生成異常，請重試）";
          angle3 = parts[3] ? parts[3].trim() : "（生成異常，請重試）";
        } else {
          angle1 = textResult;
          angle2 = "（真實 AI 已生成全部內容，請查閱切角一）";
          angle3 = "（真實 AI 已生成全部內容，請查閱切角一）";
        }

        return [angle1, angle2, angle3];

      } catch (err) {
        console.error(err);
        alert(`Gemini API 呼叫發生錯誤：${err.message}\n\n系統已自動為您切換至「離線智能模擬模式」！`);
        return runSimulatedGenerator(product, audience, tone, platform, reference);
      }
    }

    async function handleGeneration() {
      const product = document.getElementById('input-product').value.trim();
      const audience = document.getElementById('input-audience').value.trim();
      const tone = document.getElementById('input-tone').value;
      const platform = document.getElementById('input-platform').value;
      const reference = document.getElementById('input-reference').value.trim();
      
      const isApiMode = document.getElementById('api-toggle').checked;
      const apiKey = document.getElementById('api-key-input').value.trim();

      // UI 狀態切換
      const submitBtn = document.getElementById('submit-btn');
      const loader = document.getElementById('loading-spinner');
      const resultSection = document.getElementById('result-section');
      const emptyState = document.getElementById('empty-state');
      
      submitBtn.disabled = true;
      emptyState.style.display = 'none'; // 隱藏空狀態
      loader.style.display = 'flex';
      resultSection.style.display = 'none';

      let results = [];

      if (isApiMode && apiKey) {
        document.getElementById('loading-text').textContent = "正在透過真實 Gemini 2.5 進行大腦創作並連網核實...";
        results = await runRealGeminiGenerator(product, audience, tone, platform, reference, apiKey);
      } else {
        document.getElementById('loading-text').textContent = "行銷文案引擎正在就地套用黃金行銷模型...";
        // 模擬 0.8 秒大腦思考延遲，體現儀式感
        await new Promise(resolve => setTimeout(resolve, 800));
        results = runSimulatedGenerator(product, audience, tone, platform, reference);
      }

      // 將結果寫入狀態
      generatedCopies = results;
      
      // 更新 Tab 標題（根據平台美化）
      const platformLabels = {
        facebook: "FB 格式",
        instagram: "IG 格式",
        threads: "Threads 格式",
        edm: "EDM 格式"
      };
      const label = platformLabels[platform] || "社群格式";
      document.getElementById('tab-0').textContent = `切角一：AIDA 法 (${label})`;
      document.getElementById('tab-1').textContent = `切角二：PAS 痛點 (${label})`;
      document.getElementById('tab-2').textContent = `切角三：情境故事 (${label})`;

      // 還原/切換 UI
      submitBtn.disabled = false;
      loader.style.display = 'none';
      
      // 顯示最前切角
      switchTab(0);
      resultSection.style.display = 'block';
    }

    function switchTab(index) {
      activeTabIndex = index;
      
      // 切換按鈕狀態
      for (let i = 0; i < 3; i++) {
        const tab = document.getElementById(`tab-${i}`);
        if (i === index) {
          tab.classList.add('active');
        } else {
          tab.classList.remove('active');
        }
      }

      // 更新程式碼區塊內容
      const textOutput = document.getElementById('output-copy-text');
      textOutput.textContent = generatedCopies[index] || "尚無產出文案";
    }

    function copyCurrentCopy() {
      const copyText = document.getElementById('output-copy-text').textContent;
      if (!copyText || copyText === "尚無產出文案") return;

      // 使用安全相容的 iframe 內置備用複製方案（由 document.execCommand 驅動）
      const tempTextArea = document.createElement('textarea');
      tempTextArea.value = copyText;
      tempTextArea.style.position = 'fixed';
      tempTextArea.style.opacity = '0';
      document.body.appendChild(tempTextArea);
      tempTextArea.select();
      
      try {
        const successful = document.execCommand('copy');
        if (successful) {
          showToast();
        } else {
          console.error('複製失敗');
        }
      } catch (err) {
        console.error('無法執行複製指令', err);
      }
      
      document.body.removeChild(tempTextArea);
    }

    function showToast() {
      const toast = document.getElementById('copy-toast');
      toast.classList.add('show');
      
      setTimeout(() => {
        toast.classList.remove('show');
      }, 2000);
    }
  </script>
</body>
</html>
