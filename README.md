<吃飽太閒做的網站，能幫到你我覺得很開心>
<html lang="zh-Hant">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>147測驗｜單一檔案版</title>

    <!-- 預先套用主題避免閃爍 -->
    <script>
      (function () {
        try {
          var saved = localStorage.getItem('theme');
          if (saved === 'dark' || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('preload-dark');
            document.documentElement.setAttribute('data-theme','dark');
          }
        } catch(e){}
      })();
    </script>

    <style>
      :root{
        --bg:#f0f4f8; --txt:#000; --card:#fff; --rule:#e9ecef; --muted:#666;
        --primary:#007bff; --primary-800:#0056b3; --danger:#dc3545;
        --green:#28a745; --green-800:#218838; --teal:#17a2b8; --teal-800:#138496;
        --table-bd:#ccc;

        /* 正解強調（淺色） */
        --ans-chip-bg:#e7f6ec; --ans-chip-fg:#136d3a;
        --ans-row-bg:#f5fbf7; --ans-row-bd:#cce8d5;
      }
      body.dark {
        --bg:#121212; --txt:#fff; --card:#1e1e1e; --rule:#333; --table-bd:#444; --muted:#aaa;

        /* 正解強調（深色） */
        --ans-chip-bg:#1f3a29; --ans-chip-fg:#b9f6ca;
        --ans-row-bg:#12301f; --ans-row-bd:#2c5a3f;
      }

      html, body { height: 100%; }
      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 40px;
        /* 基礎自適應字級（桌機普遍上限 24px） */
        font-size: clamp(16px, 1.1vw + 12px, 24px);
        background: var(--bg);
        color: var(--txt);
        transition: background-color .4s, color .4s;
      }

      #container {
        max-width: 1320px; margin: auto; background: var(--card); padding: 40px;
        border-radius: 20px; box-shadow: 0 0 10px rgba(0,0,0,.1);
        transition: background-color .4s, color .4s;
      }

      .hidden { display: none; }
      h1, h2 { text-align: center; font-size: 1.6em; margin: .2em 0 .6em; }
      #rules { background: var(--rule); padding: 16px; margin-bottom: 28px; border-radius: 10px; font-size: .95em; }

      .btn {
        background: var(--primary); color: #fff; border: none; padding: 14px 24px;
        margin: 8px; border-radius: 10px; cursor: pointer; font-size: .95em;
        transition: background-color .2s, transform .1s;
        white-space: nowrap;
      }
      .btn:hover { background: var(--primary-800); }
      #leaveBtn { background: var(--danger); }
      #openBankBtn { background: var(--teal); }
      #openBankBtn:hover { background: var(--teal-800); }

      /* 進度條 */
      #progress, #timer { font-weight: bold; font-size: 1em; }
      .progress-container { background: #ddd; height: 10px; border-radius: 5px; margin-top: 14px; overflow: hidden; }
      body.dark .progress-container { background: #555; }
      .progress-bar { height: 100%; width: 0; background: var(--primary); transition: width .6s ease; }

      .question { margin: 24px 0 12px; font-size: 1.2em; }
      .options label { display: block; margin-bottom: 12px; font-size: 1em; }

      /* 表格與響應式容器 */
      .table-responsive { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }
      table { width: 100%; border-collapse: collapse; margin-top: 16px; font-size: 1em; min-width: 460px; }
      th, td {
        border: 1px solid var(--table-bd); padding: 12px 14px; text-align: left; vertical-align: top;
        color: var(--txt); word-break: break-word;
      }

      /* 錯題列色（深色模式改亮字） */
      tr.wrong { background-color: #ffe6e6; }
      body.dark tr.wrong { background-color: #5a1a1a; }
      body.dark tr.wrong td { color: #fff; }

      /* 正解強調（提高優先度，避免被錯題底色吃掉） */
      .ans-chip { display:inline-block; padding:2px 8px; border-radius:999px; background: var(--ans-chip-bg); color: var(--ans-chip-fg); font-size:.9em; margin-right:6px; }
      .ans-cell  { background: var(--ans-row-bg) !important; border-left: 3px solid var(--ans-row-bd) !important; color: var(--txt) !important; }
      .opt-row   { display:block; padding:4px 6px; margin:2px 0; border-radius:6px; }
      .opt-row.is-correct { background: var(--ans-row-bg) !important; border-left: 3px solid var(--ans-row-bd) !important; color: var(--txt) !important; }
      tr.wrong td.ans-cell { background: var(--ans-row-bg) !important; color: var(--txt) !important; }

      /* 右上角按鈕固定 */
      #darkModeToggle, #homeBtn {
        position: fixed; right: 20px; padding: 10px 16px; border: none; border-radius: 8px;
        cursor: pointer; font-size: .95em; z-index: 2000; box-shadow: 0 4px 12px rgba(0,0,0,.15);
      }
      #darkModeToggle { top: 20px; background: #333; color: #fff; }
      #darkModeToggle:hover { background: #555; }
      #homeBtn { top: 68px; background: var(--green); color: #fff; text-decoration: none; }
      #homeBtn:hover { background: var(--green-800); }

      .mono { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Courier New", monospace; }
      .muted { opacity: .8; color: var(--muted); }

      /* 小尺寸優化 */
      @media (max-width: 768px) {
        body { padding: 20px; font-size: clamp(14px, 2.2vw + 8px, 17px); }
        #container { padding: 20px; border-radius: 16px; }
        .btn { padding: 12px 16px; margin: 6px; font-size: .95em; }
        #darkModeToggle, #homeBtn { right: 12px; }
        #darkModeToggle { top: 12px; }
        #homeBtn { top: 56px; }
      }
      @media (max-width: 540px) {
        /* 手機上把「您的答案」隱藏，保留題目/正解/OX */
        #results table th:nth-child(2), #results table td:nth-child(2) { display:none; }
      }
      @media (max-width: 420px) {
        #bank .controls { display: grid; grid-template-columns: 1fr; gap: 8px; }
      }

      /* —— 大螢幕加碼放大（到 4K 前） —— */
      @media (min-width: 1200px) {
        body       { font-size: clamp(18px, 0.9vw + 12px, 26px); }
        #container { max-width: 1440px; }
        h1, h2     { font-size: 2rem; }
      }
      @media (min-width: 1800px) {
        body       { font-size: clamp(20px, 0.7vw + 12px, 28px); }
        #container { max-width: 1680px; }
        h1, h2     { font-size: 2.2rem; }
      }
      @media (min-width: 2400px) {
        body       { font-size: clamp(22px, 0.6vw + 14px, 32px); }
        #container { max-width: 1920px; }
        h1, h2     { font-size: 2.4rem; }
      }

      /* —— 2560×1440（含以上）專用上限 —— */
      @media (min-width: 2560px) and (min-height: 1400px) {
        body       { font-size: clamp(22px, 0.55vw + 16px, 34px); } /* 上限 34px */
        #container { max-width: 2000px; }
        h1, h2     { font-size: 2.6rem; }
      }

      /* 預載深色（head 腳本使用），load 後會移除 */
      .preload-dark body, .preload-dark { background:#121212; color:#fff; }
    </style>
  </head>
  <body>
    <button id="darkModeToggle" aria-pressed="false">深色模式 / Dark Mode</button>
    <a id="homeBtn" href="https://hisausage7.github.io/147test/" title="回首頁">🏠 回首頁</a>

    <div id="container">
      <!-- 歡迎頁 -->
      <div id="welcome">
        <h1>147測驗M1-CAA</h1>
        <div id="rules">
          <p><strong>考試注意事項 / Exam Rules:</strong></p>
          <p>1. 請輸入姓名後才能開始作答。 / You must enter your name to start the quiz.</p>
          <!-- 這行改成可自訂 -->
          <p>2. 考試限時可自訂（預設 80 分鐘,可設定至999分鐘），開始後自動倒數。 / You can set the time limit (default 80 minutes); countdown starts on begin.</p>
          <p>3. 作答途中可隨時點擊「離開考試」提前結束。 / You can click "Leave Quiz" anytime to finish early.</p>
          <p>4. 完成後會自動顯示所有答題結果與成績。 / Results and scores will be displayed after completion.</p>
          <p>5. 答對題目顯示O，答錯題目顯示X。 / Correct answers will show O, incorrect answers will show X.</p>
          <p>6. 測驗過程為亂序出題。 / The test process was chaotic and disordered in setting questions.</p>
          <p>!版權及源代碼所有-航機系008沈崑宸!</p>
          <p>!僅作為自我測驗使用!</p>
          <p>!此章節不含圖片題</p>
        </div>
        <input type="text" id="nameInput" placeholder="輸入姓名 / Enter your name"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <input type="number" id="questionLimit" placeholder="輸入題數,至多346題 / Enter number of questions"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <!-- 新增：作答時間（分鐘），預設 80，限制 1~300 -->
        <input type="number" id="durationInput" value="80" min="1" max="300"
               placeholder="作答時間（分鐘，預設 80,可設定置999分鐘） / Duration in minutes"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <div style="text-align:center; display:flex; flex-wrap:wrap; justify-content:center;">
          <button id="startBtn" class="btn">開始測驗 / Start Quiz</button>
          <button id="openBankBtn" class="btn">題庫瀏覽 / Browse Bank</button>
        </div>
      </div>

      <!-- 測驗頁 -->
      <div id="quiz" class="hidden">
        <div style="display:flex; align-items:center; gap:8px;">
          <span id="welcomeName"></span>
          <span id="timer" style="margin-left:auto">80:00</span>
        </div>
        <div class="mono" id="progress">題數: <span id="current">0</span> / <span id="total">0</span></div>
        <div class="progress-container"><div id="progressBar" class="progress-bar"></div></div>
        <div class="question" id="questionText"></div>
        <div class="options" id="options"></div>
        <div style="display:flex;gap:10px;flex-wrap:wrap">
          <button id="prevBtn" class="btn" style="background:#6c757d">上一題 / Previous</button>
          <button id="leaveBtn" class="btn">離開考試 / Leave Quiz</button>
        </div>
      </div>

      <!-- 結果頁 -->
      <div id="results" class="hidden">
        <h2>測驗結果 / Results</h2>
        <div style="display:flex; flex-wrap:wrap; justify-content:center;">
          <button id="retryBtn" class="btn">重新開始 / Retry</button>
          <button id="showWrongBtn" class="btn" style="background:#ffc107">只看錯題 / Wrong Only</button>
          <button id="showAllBtn" class="btn">看全部結果 / Show All</button>
          <button id="toBankFromResults" class="btn" style="background:#17a2b8">題庫瀏覽 / Browse Bank</button>
        </div>
        <div class="table-responsive">
          <table>
            <thead>
              <tr><th>題目</th><th>您的答案</th><th>正確答案</th><th>結果</th></tr>
            </thead>
            <tbody id="resultsBody"></tbody>
          </table>
        </div>
        <div id="scoreSummary" style="text-align:center;margin-top:12px;font-size:1em"></div>
      </div>

      <!-- 題庫瀏覽頁 -->
      <div id="bank" class="hidden">
        <h2>題庫瀏覽 / Question Bank</h2>
        <div class="controls">
          <input id="bankSearch" type="text" placeholder="關鍵字搜尋（會搜尋題目與選項）" />
          <label>每頁顯示
            <select id="perPage">
              <option value="5">5</option>
              <option value="10" selected>10</option>
              <option value="20">20</option>
              <option value="50">50</option>
              <option value="all">全部</option>
            </select>
            題
          </label>
          <button id="backToWelcome" class="btn" style="padding:12px 18px">返回首頁 / Home</button>
        </div>
        <div class="table-responsive">
          <table>
            <thead>
              <tr>
                <th style="width:70px">#</th>
                <th>題目</th>
                <th style="width:34%">選項</th>
                <th style="width:160px">正確答案</th>
              </tr>
            </thead>
            <tbody id="bankBody"></tbody>
          </table>
        </div>
        <div class="pagination" style="display:flex;justify-content:center;align-items:center;gap:10px;margin-top:10px;">
          <button id="prevPage" class="btn" style="padding:10px 16px">上一頁</button>
          <span id="pageInfo" class="mono"></span>
          <button id="nextPage" class="btn" style="padding:10px 16px">下一頁</button>
        </div>
        <div style="text-align:center;margin-top:8px">
          <small class="muted">提示：可用右上角「深色模式」切換外觀；搜尋支援中英文與數字。</small>
        </div>
      </div>
    </div>
    

    <script>
            document.addEventListener("DOMContentLoaded", function () {
              const questions = [
  { question: "0,0,1,3,8,9 What is the mode?", 
    options: ["A. 0", "B. 3.5", "C. 4.5"], 
    answer: "A" },

  { question: "180 degrees in terms of radian is", 
    options: ["A. π/2", "B. π", "C. 2π"], 
    answer: "B" },

  { question: "19/31 as a decimal is", 
    options: ["A. 0.6123", "B. 0.6129", "C. 0.5467"], 
    answer: "B" },

  { question: "200 kilovolts can be expressed as", 
    options: ["A. 2 x 10^3 volts", "B. 2 x 10^-4 volts", "C. 2 x 10^5 volts"], 
    answer: "C" },

  { question: "31/8 - 11/5 =", 
    options: ["A. 137/40", "B. 13/40", "C. 23/40"], 
    answer: null }, // 正確值應為 67/40

  { question: "4(4 (4 - 1) - 1) - 1 =", 
    options: ["A. 15", "B. 31", "C. 43"], 
    answer: "C" },

  { question: "5% of 0.76 is", 
    options: ["A. 0.0038", "B. 0.38", "C. 0.038"], 
    answer: "C" },

  { question: "5.645 written to one decimal place would be", 
    options: ["A. 5.6", "B. 6.0", "C. 5.7"], 
    answer: "A" },

  { question: "6 mm is equal to", 
    options: ["A. 0.625 inches", "B. 0.375 inches", "C. 0.236 inches"], 
    answer: "C" },

  { question: "70 percent of 220 is", 
    options: ["A. 66", "B. 132", "C. 154"], 
    answer: "C" },

  { question: "3/4 multiplied by 0.82 is equal to", 
    options: ["A. 1.23", "B. 0.615", "C. 2.46"], 
    answer: "B" },

  { question: "A copper pipe has a radius of 7/32 inch. What is this in decimal?", 
    options: ["A. 0.15625", "B. 0.21875", "C. 0.28125"], 
    answer: "B" },

  { question: "A right-angled triangle with two shortest sides 3 inches and 4 inches. What is the area?", 
    options: ["A. 5 square inches", "B. 6 square inches", "C. 7 square inches"], 
    answer: "B" },

  { question: "A shop keeper sold his car for £120. If this is 80% of the buying price, how much loss did he make?", 
    options: ["A. £150", "B. £50", "C. £30"], 
    answer: "C" },

  { question: "A trapezium has parallel sides of 9cm and 5cm. The perpendicular height between them is 3cm. The length of one of the non-parallel sides is 6cm. What is the area?", 
    options: ["A. 27 cm2", "B. 42 cm2", "C. 21 cm2"], 
    answer: "C" },

  { question: "Add together; 3/4, 5/16, 7/8 and 0.375", 
    options: ["A. 21/8", "B. 21/4", "C. 25/16"], 
    answer: null }, // 正確值應為 37/16

  { question: "An aircraft has 1800 kilos of fuel, 45% is in the centre tank, 25% is in the left wing. How much fuel is in the right wing?", 
    options: ["A. 540 kilos", "B. 450 kilos", "C. 940 kilos"], 
    answer: "A" },

  { question: "Can you take the cube root of a negative number?", 
    options: ["A. No", "B. Only certain numbers", "C. Yes"], 
    answer: "C" },

  { question: "Convert into decimal the fraction 5/8 of 60", 
    options: ["A. 40", "B. 37.5", "C. 37"], 
    answer: "B" },

  { question: "Determine the following 11/16 + 5/8", 
    options: ["A. 55/128", "B. 15/16", "C. 11/10"], 
    answer: null }, // 正確值應為 21/16

  { question: "Determine the following: 9/4 + 5/12 + 1 3/8", 
    options: ["A. 41/24", "B. 225/24", "C. 41/12"], 
    answer: null }, // 正確值應為 97/24

  { question: "Determine10 x 23 + 10 x 25", 
    options: ["A. 32,008,000", "B. 520", "C. 400"], 
    answer: "C" }, // 這題應該是 10×2^3 + 10×2^5

  { question: "Evaluate 15.4/2 - 2(4.6 - 15.7)", 
    options: ["A. 29.9", "B. 26.5", "C. -14.5"], 
    answer: "A" },

  { question: "Express decimally: four and two tenths", 
    options: ["A. 0.042", "B. 4.2", "C. 0.42"], 
    answer: "B" },

  { question: "How many centimetres is in an inch?", 
    options: ["A. 25.4", "B. 0.254", "C. 2.54"], 
    answer: "C" },

  { question: "How many inches are in 3½ yards?", 
    options: ["A. 226", "B. 126", "C. 98"], 
    answer: "B" },

  { question: "If you bought a second hand car worth £4500 after getting 15% discount. How much did the car cost originally?", 
    options: ["A. £5300", "B. £3800", "C. £6000"], 
    answer: "A" }, // 原價約 5294，最接近 5300

  { question: "One of the square roots of a positive number is positive. What is the other one?", 
    options: ["A. positive", "B. negative", "C. positive or negative"], 
    answer: "B" },

  { question: "One radian is equal to", 
    options: ["A. 57.5°", "B. 90°", "C. 75°"], 
    answer: "A" },

  { question: "Solve the following:27.65 + 4.012", 
    options: ["A. 14.006", "B. 31.662", "C. 55.338"], 
    answer: "B" },

  { question: "The answer to 10 + 6/2 - 2 - 2(8+6) is", 
    options: ["A. -26", "B. -17", "C. 58"], 
    answer: "B" },

  { question: "The area of a circle whose circumference is given as 12cm is approximately", 
    options: ["A. 11.3 sq.cm", "B. 3.8 sq.cm", "C. 38 sq cm"], 
    answer: "A" },

  { question: "The area of the curved surface area of a cone is (where r = radius; h = vertical height and l = slant height)", 
    options: ["A. 1/3πr2h", "B. πrh", "C. πrl"], 
    answer: "C" },

  { question: "The average of the following values 16.5, 42.6, 67.3, 56.9 and 14.2 is", 
    options: ["A. 37.4", "B. 38.5", "C. 39.5"], 
    answer: "C" },

  { question: "The diameter of a cylinder is 200 cm and the height is 20 cm, what is the volume?", 
    options: ["A. 628000 cm3", "B. 8000 cm3", "C. 62800 cm3"], 
    answer: "A" },

  { question: "The diameter of a pipe is 100mm. What is the cross-sectional area of the pipe in m2?", 
    options: ["A. 0.01π", "B. 0.0025π", "C. 0.2π"], 
    answer: "B" },

  { question: "The formula for calculating the area of a right angled triangle is", 
    options: ["A. ½ height + base", "B. ½ base / height", "C. ½ (base x height)"], 
    answer: "C" },

  { question: "The fraction 1/4 expressed as a ratio would be", 
    options: ["A. 1:4", "B. 4%", "C. 1:0.25"], 
    answer: "A" },

  { question: "The Lowest Common Denominator for the problem below is 1/6 + 1/5 + 1/17 + 1/2", 
    options: ["A. 1020", "B. 17", "C. 510"], 
    answer: "C" },

  { question: "The marks of eight students are as follows:95,87,60,73,45,82,65 and 52.What is the number of students below average?", 
    options: ["A. two", "B. three", "C. four"], 
    answer: "C" },

  { question: "The normal units for angular dimensions are", 
    options: ["A. degrees, tenths of a degree, hundredths of a degree", "B. degrees, minutes, seconds", "C. degrees, bars, millibars"], 
    answer: "B" },

  { question: "The ratio of the resistance of A to that of B is 5:8 If the resistance of A = 160 ohms then element B must be", 
    options: ["A. 256 ohms", "B. 100 ohms", "C. 108 ohms"], 
    answer: "A" },

  { question: "The S.I unit of angle is the", 
    options: ["A. degree", "B. radian", "C. gradient"], 
    answer: "B" },

  { question: "The SUM of the internal angles of a polygon is given by", 
    options: ["A. (n - 4) Right Angles", "B. (2n - 4) Right Angles", "C. (2n - 2) Right Angles"], 
    answer: "B" },

  { question: "The supplement of 13 degrees is", 
    options: ["A. 76", "B. 243", "C. 167"], 
    answer: "C" },

  { question: "The surface area of a cylinder of diameter 10 cm and height 10 cm, is", 
    options: ["A. 100π", "B. 50π", "C. 80π"], 
    answer: "A" }, // 這裡用的是側面+上下兩面嗎？若只算側面則 100π

  { question: "The temperature at midday was 8 degree Centigrade. Overnight it reached a low of -3 degrees Centigrade. What was the temperature drop?", 
    options: ["A. 11 degrees C", "B. 8 degrees C", "C. 5 degrees C"], 
    answer: "A" },

  { question: "To convert 1 inch to centimetres", 
    options: ["A. divide by 25.4", "B. multiply by 2.54", "C. divide by 2.54"], 
    answer: "B" },

  { question: "To convert gallons to litres", 
    options: ["A. multiply by 0.568", "B. multiply by 4.55", "C. multiply by 0.00455"], 
    answer: "B" },
    {
    question: "To find the area of a circle use the formula",
    options: ["A. πr2", "B. 2πr", "C. 2πd"],
    answer: "A"
  },
  {
    question: "What are the next two prime numbers in the following list? 2 3 5 7 11 ?",
    options: ["A. 13 19", "B. 13 17", "C. 13 15"],
    answer: "B"
  },
  {
    question: "What are the SI units for measurements?",
    options: ["A. Mile, Kilogram, Second", "B. Degree, Minute, Litre", "C. Metre, Candela, Kelvin"],
    answer: "C"
  },
  {
    question: "What is 3% of 0.001?",
    options: ["A. 0.3", "B. 0.003", "C. 0.00003"],
    answer: "C"
  },
  {
    question: "What is 467.1933 rewritten to 2 decimal places?",
    options: ["A. 467.19", "B. 467.20", "C. 470"],
    answer: "A"
  },
  {
    question: "What is the area of a rectangle when its height is 11cm and the width 120cm?",
    options: ["A. 1320 m2", "B. 0.132 m2", "C. 1.32 m2"],
    answer: "B"
  },
  {
    question: "What is the lowest common multiple of 10 and 5 and 8?",
    options: ["A. 20", "B. 40", "C. 320"],
    answer: "B"
  },
  {
    question: "What is the Lowest Common Multiple of 5; 12; 20",
    options: ["A. 60", "B. 5", "C. 120"],
    answer: "A"
  },
  {
    question: "What is the supplement of an angle of 37°?",
    options: ["A. 143°", "B. 8°", "C. 53°"],
    answer: "A"
  },
  {
    question: "Work out the following sum: 4 {2 (5 - 1) - 3} + 8",
    options: ["A. 37", "B. 28", "C. 54"],
    answer: "B"
  },
  {
    question: "Written correct to four significant figures, 16.0524 is",
    options: ["A. 16.05", "B. 16.052", "C. 16.0524"],
    answer: "A"
  },
  {
    question: "(4/x)-4=24, solve for x",
    options: ["A. 7", "B. -7", "C. 0.143"],
    answer: "C"
  },
  {
    question: "(a + b)(a - b) =",
    options: ["A. a2 - b2", "B. a2 + 2ab - b2", "C. a2 + b2"],
    answer: "A"
  },
  {
    question: "(a + b)2 =",
    options: ["A. a2+b2", "B. 2ab", "C. a2+b2 + 2ab"],
    answer: "C"
  },
  {
    question: "(x - 3)(x + 5) =",
    options: ["A. x2 - 15", "B. x2 + 2x - 15", "C. x2 + 2x"],
    answer: "B"
  },
  {
    question: "21 = 43 - X, X is equal to",
    options: ["A. 43 - 21", "B. 43 + 21", "C. 21 - 43"],
    answer: "A"
  },
  {
    question: "27y = 3so y is equal to:",
    options: ["A. 1/9", "B. 1/3", "C. 9/1"],
    answer: "A"
  },
  {
    question: "2x2z2(3x - z2) =",
    options: ["A. 6x3z2 - 2x2z4", "B. 6x2z2 - 2x2z2", "C. 6x2z2 + 3x - z2"],
    answer: "A"
  },
  {
    question: "64y = 64 what does y =",
    options: ["A. 0", "B. 1", "C. 0.5"],
    answer: "B"
  },
  {
    question: "By solving the equation 5x-5=30, x is equal to",
    options: ["A. 7", "B. 5", "C. 30"],
    answer: "A"
  },
  {
    question: "Given 43 - x = 21, find the value of x",
    options: ["A. 43 - 21", "B. 43 + 21", "C. 43 / 21"],
    answer: "A"
  },
  {
    question: "Given that A = X + BY, what is Y equal to?",
    options: ["A. A - X minus B", "B. A - X add B", "C. A - X divided by B"],
    answer: "C"
  },
  {
    question: "If y/x = 4 and x = 5 then y =",
    options: ["A. 20", "B. 1 ¼", "C. 4/5"],
    answer: "A"
  },
  {
    question: "If -3(x + 5) = 3 then x =",
    options: ["A. -6", "B. +4", "C. +6"],
    answer: "A"
  },
  {
    question: "If 3Y + 18 = 30 - Y, then Y =",
    options: ["A. 24", "B. 3", "C. 0.5"],
    answer: "B"
  },
  {
    question: "If 6y = 18x - 24, find the value of y",
    options: ["A. y = 3x - 4", "B. y = -3x + 4", "C. y = 3x - 24"],
    answer: "A"
  },
  {
    question: "If v = u + at, then t =",
    options: ["A. v - u - a", "B. (v - u )/a", "C. (v - a)/u"],
    answer: "B"
  },
  {
    question: "Simplify 3a -2b +6a -3b -2a",
    options: ["A. 7a +5b", "B. 7a -5b", "C. 7a +b"],
    answer: "B"
  },
  {
    question: "Simplify 3x - 2xy - 3y +5xy -2x +2y",
    options: ["A. x +3xy -y", "B. 5x +3xy -y", "C. x -3xy +y"],
    answer: "A"
  },
  {
    question: "Simplify (3x2 - 6xy)/(x - 2y)",
    options: ["A. 3x - 3y", "B. cannot be simplified further", "C. 3x"],
    answer: "C"
  },
  {
    question: "Solve 8 + 4[3 + 2(1 - 2/3)]",
    options: ["A. 222/3", "B. 20", "C. 36"],
    answer: "A"
  },
  {
    question: "Solve the following equation for x; -4(1-x)=24",
    options: ["A. x = 7", "B. x = 6", "C. x = -7"],
    answer: "A"
  },
  {
    question: "Solve the following equation: 5x = 3x + 2",
    options: ["A. 5", "B. 1", "C. 3"],
    answer: "B"
  },
  {
    question: "Work out 4 x 2(5-1) - (3+8)",
    options: ["A. 21", "B. 37", "C. 54"],
    answer: "A"
  },
  {
    question: "2y = 5x + 3 What is the gradient?",
    options: ["A. 5/2 + 3", "B. 5/2", "C. 2/5"],
    answer: "B"
  },
  {
    question: "What is the gradient of the line 5x-y=2?",
    options: ["A. -5", "B. 5", "C. 2"],
    answer: "B"
  },
  {
    question: "What is the slope of 5x-3y=2?",
    options: ["A. 5/3", "B. 3/5", "C. -2"],
    answer: "A"
  },
  {
    question: "3y = -21 + 24y , y=",
    options: ["A. -1", "B. 1", "C. -21"],
    answer: "B"
  },
  {
    question: "A parabola has the equation y = 2x2 + 6x - 20. At what point or points does it cut the vertical axis?",
    options: ["A. At y=-20", "B. When x=-5 and when x=2", "C. At x=6"],
    answer: "A"
  },
  {
    question: "A straight line graph has the equation 3y = 12x - 3 What is the gradient?",
    options: ["A. 1/4", "B. 3/4", "C. 4/1"],
    answer: "C"
  },
  {
    question: "A straight line has the equation y = -2x + 5. At what point does it cut the vertical axis?",
    options: ["A. 5", "B. -5", "C. -2"],
    answer: "A"
  },
  {
    question: "A straight line passes through the two points (1,4) and (6,1). What is the gradient of the line?",
    options: ["A. 3/5", "B. 2/5", "C. -3/5"],
    answer: "C"
  },
  {
    question: "Find the y-intercept of the line y=5x+c, given point (10,20)",
    options: ["A. -30", "B. -10", "C. 10"],
    answer: "A"
  },
  {
    question: "For an equation 2y = 5x + 3 what is the gradient?",
    options: ["A. 5x+3/2", "B. 3/5x", "C. 5/2"],
    answer: "C"
  },
  {
    question: "Given the cartesian coordinates of (15,4), what is the value of the abscissa?",
    options: ["A. 15", "B. 4", "C. 19"],
    answer: "A"
  },
  {
    question: "How many times does the x-axis get crossed when y = x2 - 3",
    options: ["A. 1", "B. 2", "C. 3"],
    answer: "B"
  },
  {
    question: "In the following equation what is the y-intercept? 4y = 2x + 8",
    options: ["A. 4", "B. 2", "C. 8"],
    answer: "B"
  },
  {
    question: "On a graph the y intercept is -2. Which equation is correct?",
    options: ["A. 4y=2x-2", "B. 2y=x+2", "C. y+2=-x"],
    answer: "C"
  },
  {
    question: "On a graph what is the intercept of y when 4y = x + 8",
    options: ["A. 2", "B. 4", "C. 8"],
    answer: "A"
  },
  {
    question: "Solving graphically the equation 1.2x - y = 1.80 will give which of the following type of graph?",
    options: ["A. Parabola", "B. Sine", "C. Straight Line"],
    answer: "C"
  },
  {
    question: "The best type of graph for showing fuel use in an aircraft, especially in different phases of flight, is a",
    options: ['A. " pie chart\n圓餅圖"', 'B. " bar graph\n柱狀圖"', "C. continuous line graph"],
    answer: "B"
  },
  {
    question: "The best type of graph for showing pressure in a system is a",
    options: ["A. continuous line graph", "B. pie chart", "C. bar graph"],
    answer: "A"
  },
  {
    question: "The gradient of the line 2y + 4x = 15 is",
    options: ["A. -2", "B. 15", "C. -4"],
    answer: "A"
  },
  {
    question: "The gradient of the line 5 = 2x - y is",
    options: ["A. 2", "B. -2", "C. -5"],
    answer: "A"
  },
  {
    question: "The graph of the equation 2y + 3x = 4 intercepts the y axis at y =",
    options: ["A. 4", "B. -1.5", "C. 2"],
    answer: "C"
  },
  {
    question: "The graph points (9, 3) and (3, 1) what is the slope?",
    options: ["A. 1/3", "B. 3/1", "C. 9/5"],
    answer: "A"
  },
  {
    question: "The line with equation x = 3 is",
    options: ["A. parallel to the x axis", "B. parallel to the y axis", "C. at 45° to both axes"],
    answer: "B"
  },
  {
    question: "The most common use of graphs during aircraft maintenance is for",
    options: [
      "A. the performance of aircraft and their associated systems\n飛機及系統的 Performance",
      "B. torque loading fasteners\n扣件的扭力負載",
      "C. the preparation of major modifications\n重大修改"
    ],
    answer: "A"
  },
  {
    question: "The plot of the equation y = 1/x is a",
    options: ["A. straight line", "B. curve with one turning point", "C. curve with two turning points"],
    answer: "C"
  },
  {
    question: "The shape of the graph that would result from the equation 5x - 3y = 12 would be a",
    options: ["A. parabola", "B. hyperbola", "C. straight line"],
    answer: "C"
  },
  {
    question: "The sine of 45 degree is",
    options: ["A. 1/√2", "B. √2/1", "C. 1"],
    answer: "A"
  },
  {
    question: "The slope or gradient of a velocity-time graph represents the",
    options: ["A. speed of the body", "B. acceleration of the body", "C. mass of the body"],
    answer: "B"
  },
  {
    question: "The y intercept of 4y = 4x + 8 is",
    options: ["A. 8", "B. 4", "C. 2"],
    answer: "C"
  },
  {
    question: "Two lines with equations y = 3x - 6 and y = 3x + 4 ?",
    options: ["A. are parallel", "B. are at right angles", "C. meet when y = 3"],
    answer: "A"
  },
  {
    question: "Using cosine to find the angle of a triangle, which statement is true?",
    options: ["A. Adj/Hyp", "B. Opp/Adj", "C. Opp/Hyp"],
    answer: "A"
  },
  {
    question: "What is commonly referred to as the law of a straight line?",
    options: [
      "A. The line must pass through the 180 degree datum",
      "B. y = x2 plus 180",
      "C. y = mx + c"
    ],
    answer: "C"
  },
  {
    question: "What is the equation of a straight line that passes through the two points (0,0) and (3,2)",
    options: ["A. y = 3x + 2", "B. y = 2x + 3", "C. y =2x/3"],
    answer: "C"
  },
  {
    question: "What is the equation of a straight line with gradient m and intercept on the y axis c?",
    options: ["A. y = cx + m", "B. y = mx + c", "C. x = y + mc"],
    answer: "B"
  },
  {
    question: "What is the gradient of the following straight line graph equation: 3y=24x+24",
    options: ["A. 8", "B. 12", "C. 9"],
    answer: "A"
  },
  {
    question: "What is the gradient of the straight line whose equation is 2y + 3x = 6?",
    options: ["A. 3", "B. 3/2", "C. -3/2"],
    answer: "C"
  },
  {
    question: "What is the slope between the points (3,1) and (9,3)?",
    options: ["A. 1/3", "B. 3/1", "C. 2"],
    answer: "A"
  },
  {
    question: "What type of equation is y = x2 + 9x + 14?",
    options: [
      "A. Quadratic\n二元一次方程式",
      "B. Exponential\n指數",
      "C. Circular\n圓"
    ],
    answer: "A"
  },
  {
    question: "When the Cartesian co-ordinates (4,7) are converted to the polar form, the value of 'r' is approximately",
    options: ["A. 11", "B. 8", "C. 5.5"],
    answer: "B"
  },
  {
    question: "How many centimetres is in an inch？",
    options: ["A. 25.4", "B. 2.54", "C. 0.254"],
    answer: "B"
  },
  {
    question: "Simplify 3x-2xy-3y+5xy-2x+2y.",
    options: ["A. x+3xy-y", "B. 5x+3xy-y", "C. x-3xy+y"],
    answer: "A"
  },
  {
    question: "What is the answer of linear equation 2x+1=x+5",
    options: ["A. 4", "B. 0", "C. 2"],
    answer: "A"
  },
  {
    question: "A triangle has internal angles 67° and 48°. The third angle is？",
    options: ["A. 115°", "B. 75°", "C. 65°"],
    answer: "C"
  },
  {
    question: "What type of equation is y = x^2 + 9x + 14？",
    options: [
      "A. Quadratic\n二元一次方程式",
      "B. Circular\n圓方程式",
      "C. Line\n直線方程式"
    ],
    answer: "A"
  },
  {
    question: "log100+cos60=?",
    options: ["A. 2.5", "B. 0.25", "C. 25"],
    answer: "A"
  },
  {
    question: "y/x=4, x=5, y=?",
    options: ["A. 4/5", "B. 1", "C. 20"],
    answer: "C"
  },
  {
    question: "(a+b)(a-c)(b-c) / (b+a)(c-a)(c-b) =?",
    options: ["A. 1", "B. (a+b)(a-c)(b-c)", "C. -1"],
    answer: "A"
  },
  {
    question: "1 英吋等於多少公分？",
    options: ["A. 25.4", "B. 2.54", "C. 0.254"],
    answer: "B"
  },
  {
    question: "簡化 3x-2xy-3y+5xy-2x+2y.",
    options: ["A. x+3xy-y", "B. 5x+3xy-y", "C. x-3xy+y"],
    answer: "A"
  },
  {
    question: "三角形ABC，∠A= 60°、內心為I，∠BIC= 1/2∠A = 30°",
    options: [],
    answer: ""
  },
  {
    question: "一球體直徑為4，面積為?",
    options: ["A. 4π"],
    answer: ""
  },
  {
    question: "一球半徑4公分,球體面積為何?",
    options: ["A. 8π", "B. 64π", "C. 16π"],
    answer: "C"
  },
  {
    question: "求二進位10111 - 1001 =?",
    options: ["A. 1010", "B. 1100", "C. 1110"],
    answer: "C"
  },
  {
    question: "方程式Y=1/X的圖形是?",
    options: ["A. 直線", "B. 有兩個轉折點的曲線", "C. "],
    answer: "B"
  },
  {
    question: "圖上兩點(9, 3) and (3, 1)求斜率? (註:y=mx+c  m=斜率、c=與y軸相交時的數值)",
    options: ["A. 3/1", "B. 1/3", "C. 9/5"],
    answer: "B"
  },
  {
    question: "I=PRT/100轉換為P的表示?",
    options: ["A. P=100RT/I", "B. P=IRT/100", "C. P=100I/RT"],
    answer: "C"
  },
  {
    question: "一線性方程式X=3？",
    options: ["A. 平行Y軸", "B. 平行X軸", "C. 與兩軸夾45度角"],
    answer: "A"
  },
  {
    question: "求直角三角型,內角餘弦值.",
    options: ["A. 對邊除以斜邊", "B. 對邊除以臨邊", "C. 臨邊除以斜邊"],
    answer: "C"
  },
  {
    question: "圓柱體底面半徑0.5公尺，高2公尺,圓柱體積為多少立方公尺?",
    options: ["A. 0.5π", "B. 5π", "C. π"],
    answer: "A"
  },
  {
    question: "直角三角形銳角30度,另一銳角：",
    options: ["A. 60", "B. 90", "C. 30"],
    answer: "A"
  },
  {
    question: "101012+110012=?",
    options: ["A. 462", "B. 482", "C. 4610"],
    answer: "C"
  },
  {
    question: "(a＋b)(a－b) =?",
    options: ["A. a2－b2", "B. a2＋2ab－b2", "C. a2＋b2"],
    answer: "A"
  },
  {
    question: "2X-3=4, X=?",
    options: ["A. 3.5", "B. -3", "C. 7"],
    answer: "A"
  },
  {
    question: "y=2x＋4，當x=-1，y=?",
    options: ["A. 2", "B. 4", "C. 0.5"],
    answer: "A"
  },
  {
    question: "4又3/8 - 2又1/4 +1/8=?",
    options: ["A. 2 1/8", "B. 2 1/2", "C. 2 1/4"],
    answer: "C"
  },
  {
    question: "39和104最小公倍數為",
    options: ["A. 109", "B. 312", "C. 208"],
    answer: "B"
  },
  {
    question: "(3a+2b)(2a-3b)=?",
    options: ["A. 6a^2+5ab-6b^2", "B. 6a^2-5ab-6b^2", "C. 6a-5ab-6b"],
    answer: "B"
  },
  {
    question: "內角68度和32度求第三角?",
    options: ["A. 114", "B. 32", "C. 80"],
    answer: "C"
  },
  {
    question: "39和104 最小公倍數多少?",
    options: ["A. 208", "B. 104", "C. 312"],
    answer: "C"
  },
  {
    question: "Log100+cos60°=",
    options: ["A. 2 + 0.5 = 2.5"],
    answer: ""
  },
  {
    question: "正弦90°為?",
    options: ["A. 1"],
    answer: "A"
  },
  {
    question: "求函數x=y^2+2y+1的圖形開口向?",
    options: ["A. 下", "B. 上", "C. 右"],
    answer: "C"
  },
  {
    question: "求直線2y+3x=6斜率?",
    options: ["A. 3/2", "B. 1/3", "C. -3/2"],
    answer: "C"
  },
  {
    question: "Find the area of the triangle shown 1-41.jpg",
    options: ["A. 12 cm2", "B. 9 cm2", "C. 6 cm2"],
    answer: "C"
  },
  {
    question: "The area of the shape is calculated by 1-4.gif",
    options: ["A. ½ height x base", "B. ½ base x ½ height", "C. ½ height + twice base"],
    answer: "A"
  },
  {
    question: "What is the area of the shape below? 1-12.gif",
    options: ["A. 220 square inches", "B. 200 square inches", "C. 196 square inches"],
    answer: "C"
  },
  {
    question: "What is the area of the shape shown, in square centimetres? 1-12.gif",
    options: ["A. 1130", "B. 1000", "C. 1225"],
    answer: "C"
  },
  {
    question: "What is the area of the trapezium shown? 1-39.jpg",
    options: ["A. 27 cm2", "B. 24 cm2", "C. Area cannot be calculated from information given"],
    answer: "B"
  },
  {
    question: "Make L the subject of the formula 2nfL = x",
    options: ["A. 1-23.jpg", "B. 1-24.jpg", "C. 1-25.jpg"],
    answer: "C"
  },
  {
    question: "The graph shown below is for the equation m20.jpg",
    options: ["A. y = 3x - 2", "B. 2y - 3x = 6", "C. 2y - 3x = 3"],
    answer: "B"
  },
  {
    question: "Use the graphs to solve the simultaneous equations y = x + 2 and 3y + 2x = 6 1-32.jpg",
    options: ["A. x = 0, y = 0", "B. x = 2, y = 0", "C. x = 0, y = 2"],
    answer: "C"
  },
  {
    question: "What is the equation of the graph shown? 1-33.jpg",
    options: ["A. x = 2", "B. y = 2", "C. y = 2x"],
    answer: "B"
  },
  {
    question: "Which of the graphs shown is given by the equation y = x2 + 3?",
    options: ["A. 1-34.jpg", "B. 1-35.jpg", "C. 1-36.jpg"],
    answer: "B"
  },
  {
    question: "Which of the graphs shown is given by the equation; 2y = 4x +6 ?",
    options: ["A. 1-34.jpg", "B. 1-35.jpg", "C. 1-36.jpg"],
    answer: "A"
  },
  {
    question: "Which of the graphs shown is given by the equation; y = 2x3 + 4x + 3 ?",
    options: ["A. 1-34.jpg", "B. 1-35.jpg", "C. 1-36.jpg"],
    answer: "C"
  },
  {
    question: "A car travels 24 miles in 45 minutes. What is its average speed? 一輛車在 45 分鐘內行駛 24 英里，它的平均速度是多少？",
    options: [
      "A. 32 mph（32 英里/小時）",
      "B. 36 mph（36 英里/小時）",
      "C. 18 mph（18 英里/小時）"
    ],
    answer: "A"
  },
  {
    question: "Evaluate. Yes（計算下列式子：Yes；※原題敘述如此）",
    options: [
      "A. 4 ½（4 又 1/2）",
      "B. 3 ½（3 又 1/2）",
      "C. 4"
    ],
    answer: "B"
  },
  {
    question: "15.4/2 - 2*(6.2 - 15.6). 計算：15.4 ÷ 2 − 2 × (6.2 − 15.6)",
    options: [
      "A. 11.1",
      "B. 26.5",
      "C. -11.1"
    ],
    answer: "B"
  },
  {
    question: "A cuboid has dimensions of 4 cm, 6 cm and 12 cm. What is its volume? 一個長方體尺寸為 4 cm × 6 cm × 12 cm，它的體積是？（單位：m³）",
    options: [
      "A. 0.028 m³",
      "B. 2.88 m³",
      "C. 0.000288 m³"
    ],
    answer: "B"
  },
  {
    question: "What is a scalene triangle? 什麼是不等邊三角形（scalene triangle）？",
    options: [
      "A. 2 sides unequal（2 邊不相等）",
      "B. All 3 sides unequal（三邊皆不相等）",
      "C. 2 sides equal（2 邊相等）"
    ],
    answer: "B"
  },
  {
    question: "Work out the following sum: 4 {2 (5-1) -3} + 8 計算：4 {2(5 − 1) − 3} + 8",
    options: [
      "A. 28",
      "B. 37",
      "C. 54"
    ],
    answer: "A"
  },
  {
    question: "A rectangle is 11 cm × 120 cm. What is its area in m²? 一個長方形邊長為 11 cm 和 120 cm，它的面積是多少平方公尺？",
    options: [
      "A. 0.132 m²",
      "B. 1320 m²",
      "C. 13.2 m²"
    ],
    answer: "A"
  },
  {
    question: "The surface area of a cone whose height is 10 cm and diameter is 8 cm is: 高 10 cm、直徑 8 cm 的圓錐，其側面面積為？",
    options: [
      "A. 40π cm²",
      "B. 120π cm²",
      "C. 80π cm²"
    ],
    answer: "A"
  },
  {
    question: "4 3/8 – 2 1/4 + 1/8 = ？（4 又 3/8 − 2 又 1/4 ＋ 1/8）",
    options: [
      "A. 2.25（2 又 1/4）",
      "B. 2.5（2 又 1/2）",
      "C. 2 1/8（2 又 1/8）"
    ],
    answer: "A"  // 正確值為 2 又 1/4 = 2.25
  },
  {
    question: "11/16 + 5/8 = ？",
    options: [
      "A. 55/128",
      "B. 21/16（1 又 5/16）",
      "C. 10/11"
    ],
    answer: "B"
  },
  {
    question: "Q11. 3/4 multiplied by 0.82 is equal to.／3/4 乘以 0.82 等於多少？",
    options: [
      "A. 1.23／1.23",
      "B. 0.615／0.615",
      "C. 2.46／2.46"
    ],
    answer: "B"
  },
  {
    question: "Q12. 7/6 can be expressed as.／分數 7/6 可表示為多少小數？",
    options: [
      "A. 1.166／1.166",
      "B. 2.6／2.6",
      "C. 1.6／1.6"
    ],
    answer: "A"
  },
  {
    question: "Q13. The ratio of 6:5 can be expressed as.／比值 6:5 可以改寫成哪一個？",
    options: [
      "A. 24:20／24：20",
      "B. 20:25／20：25",
      "C. 10:16／10：16"
    ],
    answer: "A"
  },
  {
    question: "Q14. 14³ can be expressed as.／14 的三次方可以寫成？",
    options: [
      "A. 14 * 14 * 14／14 × 14 × 14",
      "B. 14 x³／14 乘以 3 次方？（寫法錯誤的選項）",
      "C. 14 + 14 + 14／14 + 14 + 14"
    ],
    answer: "A"
  },
  {
    question: "Q15. 0.0000413 can be written as.／0.0000413 可以寫成科學記號：",
    options: [
      "A. 0.413 x 10⁻⁷／0.413 × 10⁻⁷",
      "B. 413 x 10⁻⁷／413 × 10⁻⁷",
      "C. 4.13 x 10⁻⁷／4.13 × 10⁻⁷"
    ],
    answer: "B"
  },
  {
    question: "Q16. 5/8 + 3/4 =.／5/8 + 3/4 等於？",
    options: [
      "A. 11/4／11/4",
      "B. 8/8／8/8",
      "C. 11/8／11/8"
    ],
    answer: "C"
  },
  {
    question: "Q17. The Lowest Common Denominator for 1/6 + 1/5 + 1/17 + 1/2 is.／1/6、1/5、1/17、1/2 的最小公分母為？",
    options: [
      "A. 510／510",
      "B. 1020／1020",
      "C. 17／17"
    ],
    answer: "A"
  },
  {
    question: "Q18. The formula for calculating the area of a right angled triangle is.／計算直角三角形面積的公式為：",
    options: [
      "A. ½ height + base／1/2 高度 ＋ 底邊",
      "B. ½ (base * height)／1/2 × (底 × 高)",
      "C. ½ base / height／1/2 × 底 ÷ 高"
    ],
    answer: "B"
  },
  {
    question: "Q19. The area of a circle whose circumference is 12 cm is approximately.／一個圓周長為 12 cm，其面積約為：",
    options: [
      "A. 3.8 sq.cm／3.8 平方公分",
      "B. 11.3 sq.cm／11.3 平方公分",
      "C. 38 sq.cm／38 平方公分"
    ],
    answer: "B"
  },
  {
    question: "Q20. Area of a right circular cone of base radius r, and height l, is.／底半徑為 r、高為 l 的直圓錐，其側面積公式為：",
    options: [
      "A. 2/3 (π * r * l)／2/3 π r l",
      "B. (π * r * l) + 2π * r * r／π r l + 2π r²",
      "C. π * r * l／π r l"
    ],
    answer: "C"
  },
  {
    question: "Q21. Determine (+3) - (−4).／計算 (+3) − (−4) 的結果。",
    options: [
      "A. −1／負 1",
      "B. −7／負 7",
      "C. +7／正 7"
    ],
    answer: "C"
  },
  {
    question: "Q22. To convert imperial gallons to litres, multiply by.／將英制加侖換算成公升，需要乘以：",
    options: [
      "A. 4.5／4.5",
      "B. 5.4／5.4",
      "C. 4.7／4.7"
    ],
    answer: "A"
  },
  {
    question: "Q23. Express 9/20 as a percentage.／將 9/20 表示為百分比。",
    options: [
      "A. 45%／45％",
      "B. 40%／40％",
      "C. 47%／47％"
    ],
    answer: "A"
  },
  {
    question: "Q24. To find the area of a circle, multiply.／要求一個圓的面積，應該：",
    options: [
      "A. twice the radius by π／半徑乘 2 再乘以 π",
      "B. the square of the circumference by the radius／周長平方再乘以半徑",
      "C. the square of the radius by π／半徑平方乘以 π"
    ],
    answer: "C"
  },
  {
    question: "Q25. How many centimetres is in an inch?／1 英吋等於多少公分？",
    options: [
      "A. 25.4／25.4 公分",
      "B. 2.54／2.54 公分",
      "C. 0.254／0.254 公分"
    ],
    answer: "B"
  },
  {
    question: "Q26. Find the lowest common multiple of 6, 7, 8.／6、7、8 的最小公倍數為？",
    options: [
      "A. 84／84",
      "B. 336／336",
      "C. 168／168"
    ],
    answer: "C"
  },
  {
    question: "Q27. What torque loading would you apply to a nut if the force is 50 lbs, exerted 2 feet from its axis?／若施力為 50 磅，作用點距離螺帽中心 2 英尺，扭矩為？",
    options: [
      "A. 600 lbs.ft／600 磅·呎",
      "B. 100 lbs.ft／100 磅·呎",
      "C. 251 lbs.ft／251 磅·呎"
    ],
    answer: "B"
  },
  {
    question: "Q28. The formula for calculating the torque loading on a nut or bolt is.／計算螺帽或螺栓扭矩的公式為：",
    options: [
      "A. Force used x lever length of the spanner／施力 × 板手作用臂長",
      "B. Lever length of the spanner / Threads per inch／板手長度 ÷ 每吋牙數",
      "C. Force used x diameter of the bolt／施力 × 螺栓直徑"
    ],
    answer: "A"
  },
  {
    question: "Q29. How is the area of a circle calculated? (r=radius, d=diameter).／圓的面積如何計算？(r = 半徑, d = 直徑)",
    options: [
      "A. 2 x 3.142 x r／2 × 3.142 × r",
      "B. d² x 3.142／d² × 3.142",
      "C. r² x 3.142／r² × 3.142"
    ],
    answer: "C"
  },
  {
    question: "Q30. Determine: 9/4 + 5/12 + 5 1/8.（原題解釋算出 7 19/24，原文答案文字有誤）／計算 9/4 + 5/12 + 5 又 1/8。",
    options: [
      "A. 2 25/24／2 又 25/24",
      "B. 4 1/12／4 又 1/12",
      "C. 7 19/24／7 又 19/24"
    ],
    answer: "C"
  },
  {
    question: "Q31. The specific torque loading for a bolt is 50 lbs.ins but an extension of 2 in. is needed in addition to the 8 in. torque wrench. What will the actual reading be?／規定扭矩為 50 lb·in，扭力扳手長度 8 吋，再加 2 吋延長桿，扭力扳手上應讀多少？",
    options: [
      "A. 60 lb.ins／60 磅·吋",
      "B. 54 lb.ins／54 磅·吋",
      "C. 40 lb.ins／40 磅·吋"
    ],
    answer: "C"
  },
  {
    question: "Q32. Express the fraction 7/8 as a decimal.／將 7/8 寫成小數。",
    options: [
      "A. 0.785／0.785",
      "B. 0.878／0.878",
      "C. 0.875／0.875"
    ],
    answer: "C"
  },
  {
    question: "Q33. Determine 0.75 x 0.003.／計算 0.75 × 0.003。",
    options: [
      "A. 0.225／0.225",
      "B. 0.00225／0.00225",
      "C. 0.0225／0.0225"
    ],
    answer: "B"
  },
  {
    question: "Q34. Convert 162 knots to MPH.／將 162 節換算為英里/小時。",
    options: [
      "A. 186 mph／186 英里/小時",
      "B. 176 mph／176 英里/小時",
      "C. 196 mph／196 英里/小時"
    ],
    answer: "A"
  },
  {
    question: "Q35. To convert inches to millimetres, it is necessary to.／把英吋轉成毫米，需要：",
    options: [
      "A. divide by 25.4／除以 25.4",
      "B. multiply by 25.4／乘以 25.4",
      "C. multiply by 2.54／乘以 2.54"
    ],
    answer: "B"
  },
  {
    question: "Q36. 3/4 x 82 =.／3/4 × 82 等於？",
    options: [
      "A. 123／123",
      "B. 61.5／61.5",
      "C. 81.5／81.5"
    ],
    answer: "B"
  },
  {
    question: "Q37. A circular patch is held together by seven equally spaced rivets. What is their angular spacing?／一圓形補片用七顆平均分布的鉚釘固定，每顆鉚釘的角度間距為？",
    options: [
      "A. 51.50°／51.50 度",
      "B. 52°／52 度",
      "C. 51.43°／51.43 度"
    ],
    answer: "C"
  },
  {
    question: "Q38. Add together: 3/4, 5/16, 7/8 and 0.375.／將 3/4、5/16、7/8 和 0.375 相加。",
    options: [
      "A. 2 5/16／2 又 5/16",
      "B. 2 1/8／2 又 1/8",
      "C. 2 1/4／2 又 1/4"
    ],
    answer: "A"
  },
  {
    question: "Q39. To convert pounds of fuel into kilograms, it is necessary to.／把燃油磅數換算為公斤，應該：",
    options: [
      "A. divide by 0.4536／除以 0.4536",
      "B. multiply by 4536／乘以 4536",
      "C. multiply by 0.4536／乘以 0.4536"
    ],
    answer: "C"
  },
  {
    question: "Q40. If resin to hardener is used in the ratio of 1000:45, how much hardener is used with 60 grams of resin?／樹脂：硬化劑比例為 1000:45，若使用 60 g 樹脂，需要多少 g 硬化劑？",
    options: [
      "A. 145 grams／145 公克",
      "B. 47 grams／47 公克",
      "C. 2.7 grams／2.7 公克"
    ],
    answer: "C"
  },
  {
    question: "Q41. Determine the following: 11/16 + 5/8.／計算 11/16 + 5/8。",
    options: [
      "A. 11/10／11/10",
      "B. 1 55/128／1 又 55/128",
      "C. 1 5/16／1 又 5/16"
    ],
    answer: "C"
  },
  {
    question: "Q42. 6 mm is equal to.／6 mm 約等於多少英吋？",
    options: [
      "A. 0.625 inches／0.625 英吋",
      "B. 0.236 inches／0.236 英吋",
      "C. 0.375 inches／0.375 英吋"
    ],
    answer: "B"
  },
  {
    question: "Q43. Weight is equal to.／重量（Weight）的公式為：",
    options: [
      "A. volume x gravity／體積 × 重力",
      "B. mass x acceleration／質量 × 加速度",
      "C. mass x gravity／質量 × 重力加速度"
    ],
    answer: "C"
  },
  {
    question: "Q44. 8 + 4[5 x 2 (5 - 9/3)] =.／計算：8 + 4[5 × 2 (5 − 9/3)]。",
    options: [
      "A. 88／88",
      "B. 12／12",
      "C. 48／48"
    ],
    answer: "A"
  },
  {
    question: "Q45. To convert gallons to litres.／將加侖換算為公升：",
    options: [
      "A. multiply by 4.55／乘以 4.55",
      "B. multiply by 0.00455／乘以 0.00455",
      "C. multiply by 0.568／乘以 0.568"
    ],
    answer: "A"
  },
  {
    question: "Q46. A cylinder has a diameter of 20 cm and a length of 20 cm, what is its volume?／一圓柱直徑 20 cm、長度 20 cm，體積約為？",
    options: [
      "A. 1240 cm³／1240 立方公分",
      "B. 6200 cm³／6200 立方公分",
      "C. 400 cm³／400 立方公分"
    ],
    answer: "B"
  },
  {
    question: "Q47. 31/8 − 11/5 =.／計算 31/8 − 11/5。",
    options: [
      "A. 23/40／23/40",
      "B. 13/40／13/40",
      "C. 67/40／67/40"
    ],
    answer: "C"
  },
  {
    question: "Q48. What is the formula for calculating the curved area of a cone?／圓錐側面積的公式為：",
    options: [
      "A. π × radius² × height／π r² h",
      "B. π × radius × height／π r h（實際上是 π r l，題目把 l 寫成 height）",
      "C. 2/3 × π × radius × height／2/3 π r h"
    ],
    answer: "B"
  },
  {
    question: "Q49. Determine 10 (23) + 10 (25).／計算 10(23) + 10(25)（注意這裡是 10×23 和 10×25 的和）。",
    options: [
      "A. 520／520",
      "B. 320／320",
      "C. 480／480"
    ],
    answer: "C"
  },
  {
    question: "Q50. One radian is equal to.／1 弧度約等於多少度？",
    options: [
      "A. 90°／90 度",
      "B. 75°／75 度",
      "C. 57.5°／57.5 度"
    ],
    answer: "C"
  },
  {
    question: "Q51. The surface area of a cylinder of diameter 10 cm and height 10 cm, is.（只算側面積）／直徑 10 cm、高 10 cm 圓柱的側面積為：",
    options: [
      "A. 80π／80π",
      "B. 50π／50π",
      "C. 100π／100π"
    ],
    answer: "C"
  },
  {
    question: "Q52. A parallelogram has a base 120 cm and height 11 cm. What is the area?【原題選項數值疑似打錯，正確應為 0.132 m²】／一個平行四邊形底邊 120 cm、高 11 cm，面積為？",
    options: [
      "A. 0.0132 m²／0.0132 平方公尺",
      "B. 1.32 m²／1.32 平方公尺",
      "C. 1.32 m²／1.32 平方公尺"
    ],
    answer: "A" // 原文給的正解數值是 0.132 m²，最接近的是 A，題目本身有誤
  },
  {
    question: "Q53. The area of this shape is calculated by.／此圖形（矩形）的面積計算方式為：",
    options: [
      "A. Perimeter squared／周長平方",
      "B. ½ Base * Height／1/2 × 底 × 高",
      "C. Base * Height／底 × 高"
    ],
    answer: "C"
  },
  {
    question: "Q54. The area of the shape is calculated by.（三角形）／此圖形（三角形）的面積為：",
    options: [
      "A. ½ height * base／1/2 × 高 × 底",
      "B. ½ base * ½ height／(1/2 底) × (1/2 高)",
      "C. ½ base * ½ height／(1/2 底) × (1/2 高)"
    ],
    answer: "A"
  },
  {
    question: "Q55. The area of the curved surface area of a cone is (where r = radius; h = vertical height and l = slant height).／圓錐側面積（r 為半徑，h 垂直高，l 斜高）為：",
    options: [
      "A. π r h／π r h",
      "B. π r² h／π r² h",
      "C. 1/3 π r² h／1/3 π r² h"
    ],
    answer: "A"
  },
  {
    question: "Q56. What is the volume of a cuboid?／長方體體積為：",
    options: [
      "A. height * length * width／高 × 長 × 寬",
      "B. height * ½ base * height／高 × 1/2 底 × 高",
      "C. height * ½ base * length／高 × 1/2 底 × 長"
    ],
    answer: "A"
  },
  {
    question: "Q57. (4−6) − (9/−3) + (−3) =.／計算 (4−6) − (9/−3) + (−3)。",
    options: [
      "A. −2／負 2",
      "B. 4.5／4.5",
      "C. 4／4"
    ],
    answer: "A"
  },
  {
    question: "Q58. An aircraft travels 2150 nautical miles in 2 hours 30 minutes. What is the average speed of the aircraft?／飛機在 2 小時 30 分內飛行 2150 海浬，其平均速度為？",
    options: [
      "A. 550 knots／550 節",
      "B. 600 knots／600 節",
      "C. 860 knots／860 節"
    ],
    answer: "C"
  },
  {
    question: "Q59. What is the surface area of a cylinder whose diameter is 20 cm and height is 15 cm?（只算側面積）／直徑 20 cm、高 15 cm 圓柱的側面積為：",
    options: [
      "A. 300π／300π",
      "B. 942π／942π",
      "C. 350π／350π"
    ],
    answer: "A"
  },
  {
    question: "Q60. Four percent of 0.01 is.／0.01 的 4% 為多少？",
    options: [
      "A. 0.0004／0.0004",
      "B. 0.004／0.004",
      "C. 0.04／0.04"
    ],
    answer: "A"
  },
  {
    question: "Q61. (6 + 2)² * 2 − (2 * 45) =.／計算 (6+2)² × 2 − (2 × 45)。",
    options: [
      "A. 218／218",
      "B. 38／38",
      "C. 128／128"
    ],
    answer: "B"
  },
  {
    question: "Q62. 17°49′10″ + 22°22′59″ equals.／17 度 49 分 10 秒 加上 22 度 22 分 59 秒等於？",
    options: [
      "A. 40°11′69″／40 度 11 分 69 秒",
      "B. 40°12′09″／40 度 12 分 9 秒",
      "C. 39°11′09″／39 度 11 分 9 秒"
    ],
    answer: "B"
  },
  {
    question: "Q63. The diameter of a cylinder is 200 cm and the height is 20 cm, what is the volume?／直徑 200 cm、高 20 cm 的圓柱體積為？",
    options: [
      "A. 628000 cm³／628000 立方公分",
      "B. 62800 cm³／62800 立方公分",
      "C. 8000 cm³／8000 立方公分"
    ],
    answer: "A"
  },
  {
    question: "Q64. The comparison of the power input to the power output of an inverter is expressed as a.／比較變流器輸入功率與輸出功率時，以什麼形式表示？",
    options: [
      "A. ratio／比值",
      "B. gain／增益",
      "C. loss／損失"
    ],
    answer: "A"
  },
  {
    question: "Q65. The ratio of 6:4 can also be expressed as.／6:4 可以表示為幾百分比？",
    options: [
      "A. 64%／64％",
      "B. 66%／66％",
      "C. 150%／150％"
    ],
    answer: "C"
  },
  {
    question: "Q66. 200 kilovolts can be expressed as.／200 kV 可以寫成：",
    options: [
      "A. 2 × 10³ volts／2 × 10³ 伏特",
      "B. 2 × 10⁵ volts／2 × 10⁵ 伏特",
      "C. 2 × 10⁻⁴ volts／2 × 10⁻⁴ 伏特"
    ],
    answer: "B"
  },
  {
    question: "Q67. What is the surface area of a cone if the base is 8 cm diameter and the height is 10 cm?（取斜高 l ≈ 10）／底直徑 8 cm、高約 10 cm 圓錐的側面積為：",
    options: [
      "A. 40π／40π",
      "B. 80π／80π",
      "C. 120π／120π"
    ],
    answer: "A"
  },
  {
    question: "Q68. What is the area of a rectangle when its height is 11 cm and the width 120 cm?／一長方形高 11 cm、寬 120 cm，其面積為？",
    options: [
      "A. 0.132 m²／0.132 平方公尺",
      "B. 1.32 m²／1.32 平方公尺",
      "C. 1320 m²／1320 平方公尺"
    ],
    answer: "A"
  },
  {
    question: "Q69. 4 3/8 − 2 1/4 + 1/5 =.／計算 4 又 3/8 − 2 又 1/4 + 1/5。",
    options: [
      "A. 2 1/4／2 又 1/4",
      "B. 2 13/40／2 又 13/40",
      "C. 3 3/10／3 又 3/10"
    ],
    answer: "B"
  },
  {
    question: "Q70. 4 * (4 * (4 − 1) − 1) − 1 =.／計算 4 × (4 × (4 − 1) − 1) − 1。",
    options: [
      "A. 31／31",
      "B. 15／15",
      "C. 43／43"
    ],
    answer: "C"
  },
  {
    question: "Which number is the lowest common factor of 36, 66 and 126? 36、66 和 126 的「最小共同因數」是下列哪一個？",
    options: [
      "A. 23",
      "B. 12（12）",
      "C. 6（6）"
    ],
    answer: "C"
  },
  {
    question: "What is 3% of 0.001? 0.001 的 3% 是多少？",
    options: [
      "A. 0.00003",
      "B. 0.003",
      "C. 0.3"
    ],
    answer: "A"
  },
  {
    question: "11/16 divided by 5/8 is… 11/16 ÷ 5/8 等於多少？",
    options: [
      "A. 55/128",
      "B. 11/10（1 又 1/10）",
      "C. 10/11"
    ],
    answer: "B"
  },
  {
    question: "An aircraft uses 1680 gallons of fuel. The left tank uses 45%, the right tank uses 32.5%. How much was used by the centre tank? 一架飛機總共使用 1680 加侖燃油，左油箱用掉 45%，右油箱用掉 32.5%，中央油箱用了多少加侖？",
    options: [
      "A. 210 gallons（210 加侖）",
      "B. 21 gallons（21 加侖）",
      "C. 378 gallons（378 加侖）"
    ],
    answer: "C"
  },
  {
    question: "What is the fraction 1/7 in decimal? 分數 1/7 轉成小數約為多少？",
    options: [
      "A. 0.14295",
      "B. 0.14286",
      "C. 1.429"
    ],
    answer: "B"
  },
  {
    question: "The supplement of 13 degrees is… 13 度的補角為多少？（補角 = 兩角和為 180°）",
    options: [
      "A. 243°",
      "B. 76°",
      "C. 167°"
    ],
    answer: "C"
  },
  {
    question: "What is the area of a ring with an outer diameter of 90 inches and an inner diameter of 80 inches? 一個環形，其外徑 90 吋、內徑 80 吋，它的面積為多少？",
    options: [
      "A. 325π",
      "B. 435π",
      "C. 425π"
    ],
    answer: "C"
  },
  {
    question: "What is the area of the shape shown, in centimetres?（題目圖形單位為吋，問換算成平方公分的面積）",
    options: [
      "A. 1000",
      "B. 1225",
      "C. 1130"
    ],
    answer: "B"
  },
  {
    question: "What is the area of a rectangle with base 160 cm and height 12 cm? 底 160 cm、高 12 cm 的長方形，面積是多少平方公尺？",
    options: [
      "A. 0.0192 m²",
      "B. 0.192 m²",
      "C. 0.00192 m²"
    ],
    answer: "B"
  },
  {
    question: "Calculate the area of the shape shown.（兩同心圓形成的環形，R1 = 1.5, R2 = 3）",
    options: [
      "A. 6.75π",
      "B. 6.75π",
      "C. 17.5π"
    ],
    answer: "A"
  },
  {
    question: "An aircraft flies 1350 nm in 2 hours 15 minutes. What is the average speed? 一架飛機在 2 小時 15 分鐘內飛行 1350 海里，平均速度為何？",
    options: [
      "A. 850 kts",
      "B. 600 kts",
      "C. 650 kts"
    ],
    answer: "B"
  },
  {
    question: "What is the supplement of 13 degrees 13 minutes 13 seconds? 13 度 13 分 13 秒的補角為多少？",
    options: [
      "A. 167°46′47″",
      "B. 266°87′87″",
      "C. 166°46′47″"
    ],
    answer: "C"
  },
  {
    question: "Determine 15.4/2 − 2(6.2 − 15.6). 計算：15.4 ÷ 2 − 2(6.2 − 15.6)",
    options: [
      "A. 11.1",
      "B. 4.5",
      "C. 26.5"
    ],
    answer: "C"
  },
  {
    question: "Calculate the area of the shape shown.（由幾個矩形組成的小平面圖，單位 sq.ins）",
    options: [
      "A. 12 sq.ins（12 平方英吋）",
      "B. 16 sq.ins（16 平方英吋）",
      "C. 14 sq.ins（14 平方英吋）"
    ],
    answer: "C"
  },
  {
    question: "A mound of soil is piled into a cone of base diameter 1.8 m and height 0.6 m. What is the volume of soil? 土堆堆成一個底徑 1.8 m、高 0.6 m 的圓錐，土的體積約為多少？",
    options: [
      "A. 0.5 m³",
      "B. 1.0 m³",
      "C. 1.5 m³"
    ],
    answer: "A"
  },
  {
    question: "What is the area of the shape below?（不規則矩形組合，單位 square inches）",
    options: [
      "A. 220 square inches（220 平方英吋）",
      "B. 196 square inches（196 平方英吋）",
      "C. 200 square inches（200 平方英吋）"
    ],
    answer: "B"
  },
  {
    question: "What is 1 radian in degrees? 1 弧度約等於多少度？",
    options: [
      "A. 57°",
      "B. 270°",
      "C. 66°"
    ],
    answer: "A"
  },
  {
    question: "(5² × 5³)² is… 表達式 (5² × 5³)² 的值為多少的 10 的次方？",
    options: [
      "A. 5⁷",
      "B. 5¹²",
      "C. 5¹⁰"
    ],
    answer: "C"
  },
  {
    question: "What is 30% of 0.01? 0.01 的 30% 是多少？",
    options: [
      "A. 0.03",
      "B. 0.003",
      "C. 0.0003"
    ],
    answer: "B"
  },
  {
    question: "Evaluate 15.4/2 − 2(4.6 − 15.7). 計算：15.4 ÷ 2 − 2(4.6 − 15.7)",
    options: [
      "A. 26.5",
      "B. 29.9",
      "C. -14.5"
    ],
    answer: "B"
  },
  {
    question: "How many radians are in 360°? 360 度等於多少弧度？",
    options: [
      "A. 2π",
      "B. 6π",
      "C. 4π"
    ],
    answer: "A"
  },
  {
    question: "What is the area (including the ends) of a cylinder of diameter 10 cm and height 10 cm? 直徑 10 cm、高 10 cm 的圓柱（含兩個底面）的總表面積為？",
    options: [
      "A. 50π cm²",
      "B. 150π cm²",
      "C. 100π cm²"
    ],
    answer: "B"
  },
  {
    question: "What is the highest factor of 153? 153 的最大因數（大於 1 的約數）為何？",
    options: [
      "A. 6",
      "B. 3",
      "C. 9"
    ],
    answer: "C"
  },
  {
    question: "Convert into decimal the fraction 5/8 of 60. 60 的 5/8 寫成小數為多少？",
    options: [
      "A. 40",
      "B. 37.5",
      "C. 37"
    ],
    answer: "B"
  },
  {
    question: "An aeroplane has 1800 gallons of fuel on board. 35% is in the left wing, 42.5% in the right wing. How much fuel is in the centre tank? 一架飛機共有 1800 加侖燃油，左翼 35%、右翼 42.5%，中央油箱有多少加侖？",
    options: [
      "A. 405 gallons",
      "B. 545 gallons",
      "C. 183 gallons"
    ],
    answer: "A"
  },
  {
    question: "In the common fraction 2/5, the number 5 is known as… 在分數 2/5 中，數字 5 稱為？",
    options: [
      "A. the quotient（商）",
      "B. the numerator（分子）",
      "C. the denominator（分母）"
    ],
    answer: "C"
  },
  {
    question: "If 42% = 15,000, what is 100%? 若某數的 42% 為 15,000，則此數的 100% 約為多少？",
    options: [
      "A. 21,300",
      "B. 35,714",
      "C. 6,300"
    ],
    answer: "B"
  },
  {
    question: "What is 12.75 × 26.1 to two significant figures? 12.75 × 26.1，取兩位有效數字的近似值為？",
    options: [
      "A. 332.775",
      "B. 332.78",
      "C. 330"
    ],
    answer: "C"
  },
  {
    question: "The fraction 17/11 is classed as… 分數 17/11 屬於哪一類？",
    options: [
      "A. a mixed fraction（帶分數）",
      "B. an improper fraction（假分數）",
      "C. a proper fraction（真分數）"
    ],
    answer: "B"
  },
  {
    question: "To convert 1 inch to centimetres… 要把 1 英吋換算成公分，應該？",
    options: [
      "A. divide by 2.54（除以 2.54）",
      "B. multiply by 2.54（乘以 2.54）",
      "C. divide by 25.4（除以 25.4）"
    ],
    answer: "B"
  },
  {
    question: "0.000006 volts can be written as… 0.000006 伏特可寫成？",
    options: [
      "A. 60 nanovolts（60 奈伏特）",
      "B. 6 microvolts（6 微伏特）",
      "C. 6 millivolts（6 毫伏特）"
    ],
    answer: "B"
  },
  {
    question: "The median of the values 20, 28, 17, 34, 40, 11, 34, 26 is… 數列 20, 28, 17, 34, 40, 11, 34, 26 的中位數為？",
    options: [
      "A. 34.0",
      "B. 27.0",
      "C. 26.25"
    ],
    answer: "B"
  },
  {
    question: "The mode of the following 28, 17, 34, 28, 34, 35, 28, 40 is… 數列 28, 17, 34, 28, 34, 35, 28, 40 的眾數為？",
    options: [
      "A. 28.0",
      "B. 30.5",
      "C. 31.0"
    ],
    answer: "A"
  },
  {
    question: "0.004 amperes can be written as… 0.004 安培可寫成？",
    options: [
      "A. 0.4 mA",
      "B. 4 kA",
      "C. 4 mA"
    ],
    answer: "C"
  },
  {
    question: "A sphere with a radius of 2 cm has a surface area of… 半徑 2 cm 的球體，其表面積為？",
    options: [
      "A. 16π cm²",
      "B. 64π cm²",
      "C. 8π cm²"
    ],
    answer: "A"
  },
  {
    question: "The sum of an odd and an even number is… 一個奇數加上一個偶數，其結果是？",
    options: [
      "A. sometimes odd, sometimes even（有時奇、有時偶）",
      "B. always odd（一定是奇數）",
      "C. always even（一定是偶數）"
    ],
    answer: "B"
  },
  {
    question: "A copper pipe has a radius of 7/32 inch. What is this in decimal? 一根銅管半徑為 7/32 吋，換成十進位小數為？",
    options: [
      "A. 0.28125",
      "B. 0.15625",
      "C. 0.21875"
    ],
    answer: "C"
  },
  {
    question: "Millibar is the unit of… 毫巴（millibar）是什麼物理量的單位？",
    options: [
      "A. temperature（溫度）",
      "B. pressure（壓力）",
      "C. density（密度）"
    ],
    answer: "B"
  },
  {
    question: "A ball rolls down a hill initially at 60 ft/s. It slows down at a rate of 5 ft/s² for 7 seconds. What will its final speed be? 一顆球初速 60 ft/s，下坡時以 5 ft/s² 的減速度減速 7 秒後，末速為？",
    options: [
      "A. 15 ft/s",
      "B. 35 ft/s",
      "C. 25 ft/s"
    ],
    answer: "C"
  },
  {
    question: "A dial gauge is calibrated to an accuracy of 0.001 inch. When using the dial gauge, you should… 某指針量規刻度精度為 0.001 吋，使用時讀值應該？",
    options: [
      "A. round off the answer to calibrated value（四捨五入到 0.001 的精度）",
      "B. read the true value to 4 decimal places（讀到小數四位）",
      "C. read five significant figures（讀五位有效數字）"
    ],
    answer: "A"
  },
  {
    question: "In a flight control system, the control cable is allowed an elongation of 3% due to wear. The length from the manufacturer is 78 cm. What is its maximum used length? 某飛控鋼索因磨損允許延長 3%，原長度 78 cm，最大可用長度為？",
    options: [
      "A. 80.34 cm",
      "B. 78.34 cm",
      "C. 2.34 cm"
    ],
    answer: "A"
  },
  {
    question: "You have made 20% profit. Your balance is now £900. What was your pre-profit balance? 你獲利 20% 之後餘額為 900 英鎊，獲利前原本的金額約是多少？",
    options: [
      "A. £700",
      "B. £800",
      "C. £750"
    ],
    answer: "C"
  },
  {
    question: "One of the square roots of a positive number is positive. What is the other one? 一個正數的平方根其中一個是正的，另一個是？",
    options: [
      "A. positive or negative（可能正或負）",
      "B. negative（負）",
      "C. positive（正）"
    ],
    answer: "B"
  },
  {
    question: "A cylinder has a radius of 20 cm and a length of 40 cm. What is its volume? (Take π as 3.1). 半徑 20 cm、長度 40 cm 的圓柱，其體積為何？（π 取 3.1）",
    options: [
      "A. 49600 cm³",
      "B. 50270 cm³",
      "C. 800 cm³"
    ],
    answer: "A"
  },
  {
    question: "Can you take the cube root of a negative number? 負數可以開立方根嗎？",
    options: [
      "A. No（不行）",
      "B. Yes（可以）",
      "C. Only certain numbers（只有某些可以）"
    ],
    answer: "B"
  },
  {
    question: "The process of removing roots from the denominator of fractions is called what? 把分母中的根號消除，這個過程稱為？",
    options: [
      "A. Rationalizing the denominator（分母有理化）",
      "B. Squaring the denominator（分母平方化）",
      "C. Derooting the denominator（分母去根）"
    ],
    answer: "A"
  },
  {
    question: "Find the curved surface area of a cylinder of diameter 20 cm and length 10 cm. 直徑 20 cm、長度 10 cm 的圓柱，其側面面積為多少？",
    options: [
      "A. 1256 cm²",
      "B. 2512 cm²",
      "C. 400 cm²"
    ],
    answer: "A"
  },
  {
    question: "The conversion factor of litres to pints is… 公升換算成品脫的換算係數為？",
    options: [
      "A. 2.2",
      "B. 1.76",
      "C. 0.57"
    ],
    answer: "B"
  },
  {
    question: "The volume of a pyramid is ____ times b times h. 棱錐的體積等於 ____ × 底面積 b × 高 h。",
    options: [
      "A. 1/4",
      "B. 1/3",
      "C. 1/2"
    ],
    answer: "B"
  },
  {
    question: "What is the square root of 0.0289? 0.0289 的平方根為？",
    options: [
      "A. 0.17",
      "B. 1.017",
      "C. 1.7"
    ],
    answer: "A"
  },
  {
    question: "A car travelling at 72 km/hour is travelling at what speed (m/s)? 一輛車速 72 km/h，換算成公尺/秒約為多少？",
    options: [
      "A. 30 m/s",
      "B. 20 m/s",
      "C. 10 m/s"
    ],
    answer: "B"
  },
  {
    question: "If you bought a TV set worth £30 after getting 15% discount. How much discount did you get? 一台原價某數的電視，打 85 折後付了 30 英鎊，你得到的折扣金額約是多少？",
    options: [
      "A. £15",
      "B. £5",
      "C. £35"
    ],
    answer: "B"
  },
  {
    question: "If you bought a second hand car worth £4500 after getting 15% discount. How much did the car cost originally? 一台二手車打 85 折後價格為 4500 英鎊，請問原價約為多少？",
    options: [
      "A. £3800",
      "B. £5300",
      "C. £6000"
    ],
    answer: "B"
  },
  {
    question: "31 × 91 × 23 × 52 = ?（估算量級）",
    options: [
      "A. 3,373",
      "B. 33,739",
      "C. 3,373,916"
    ],
    answer: "C"
  },
  {
    question: "1/5 + 2.5 − 6 = ?",
    options: [
      "A. 3.3",
      "B. 2.0",
      "C. -3.3"
    ],
    answer: "C"
  },
  {
    question: "Express 173942 in standard form. 將 173942 寫成科學記號（標準形）？",
    options: [
      "A. 17.3942 × 10⁴",
      "B. 173.942 × 10³",
      "C. 1.73942 × 10⁵"
    ],
    answer: "C"
  },
  {
    question: "The sum of an odd number + an odd number is a… 一個奇數加上一個奇數，結果為？",
    options: [
      "A. either odd or even（可能奇或偶）",
      "B. odd number（奇數）",
      "C. even number（偶數）"
    ],
    answer: "C"
  },
  {
    question: "Express 750 milligrams in grams. 750 毫克換算成公克為？",
    options: [
      "A. 0.0000075",
      "B. 0.075",
      "C. 0.75"
    ],
    answer: "B"
  },
  {
    question: "There is 1800 pounds of fuel in an aircraft, 25% in the left tank and 45% in the right. How much fuel is in the centre tank? 飛機共有 1800 磅燃油，左油箱 25%、右油箱 45%，中間油箱有多少磅？",
    options: [
      "A. 810 pounds",
      "B. 450 pounds",
      "C. 540 pounds"
    ],
    answer: "C"
  },
  {
    question: "What is the supplement of an angle of 37°? 37° 的補角為多少？",
    options: [
      "A. 8°",
      "B. 53°",
      "C. 143°"
    ],
    answer: "C"
  },
  {
    question: "To what power must 10 be raised to equal 100,000? 10 要升到幾次方才會得到 100,000？",
    options: [
      "A. 6",
      "B. 4",
      "C. 5"
    ],
    answer: "C"
  },
  {
    question: "Find the square root of 1600. 1600 的平方根為？",
    options: [
      "A. 80",
      "B. 40",
      "C. 800"
    ],
    answer: "B"
  },
  {
    question: "What is the ratio of 5 feet to 30 inches? 5 英尺比 30 英吋的比值為？",
    options: [
      "A. 2 : 1",
      "B. 5 : 3",
      "C. 1 : 6"
    ],
    answer: "A"
  },
  {
    question: "Evaluate 5[3 + 6(7 − 4) − 2]. 計算：5[3 + 6(7 − 4) − 2]",
    options: [
      "A. 31",
      "B. 395",
      "C. 95"
    ],
    answer: "C"
  },
  {
    question: "Find the value of 3[5 − 2(4 − 7)]. 計算：3[5 − 2(4 − 7)]",
    options: [
      "A. 9",
      "B. -3",
      "C. 33"
    ],
    answer: "C"
  },
  {
    question: "What is the cube root of -64? -64 的立方根是多少？",
    options: [
      "A. 4",
      "B. -8",
      "C. -4"
    ],
    answer: "C"
  },
  {
    question: "What is the cube root of 8²? 8 的平方 (8²) 的立方根是多少？",
    options: [
      "A. 2",
      "B. 4",
      "C. 8"
    ],
    answer: "B"
  },
  {
    question: "An engine of 96 horsepower is running at 75% power. What horsepower is being developed? 一具 96 匹馬力的引擎以 75% 功率運轉，實際輸出馬力為多少？",
    options: [
      "A. 72 hp",
      "B. 168 hp",
      "C. 62 hp"
    ],
    answer: "A"
  },
  {
    question: "A blueprint shows a hole of 0.3751 to be drilled. What fraction size drill bit is most nearly equal? 圖紙上標示鑽孔直徑為 0.3751 吋，最接近的鑽頭分數尺寸為？",
    options: [
      "A. 5/16",
      "B. 3/8",
      "C. 3/16"
    ],
    answer: "B"
  },
  {
    question: "120 out of 125 bolts produced are of an acceptable tolerance. What percentage of the bolts are not acceptable? 125 顆螺栓中有 120 顆在可接受公差內，請問不合格比例為多少百分比？",
    options: [
      "A. 5%",
      "B. 4%",
      "C. 25%"
    ],
    answer: "B"
  },
  {
    question: "Evaluate 1/4 + 3/8 − 1/2. 計算：1/4 + 3/8 − 1/2",
    options: [
      "A. 1/8",
      "B. 1/14",
      "C. 1/2"
    ],
    answer: "A"
  },
  {
    question: "3 3/4 + 4 2/3 = ? 計算：3 又 3/4 加 4 又 2/3 等於多少？",
    options: [
      "A. 8 5/12",
      "B. 7 5/12",
      "C. 7 5/7"
    ],
    answer: "A"
  },
  {
    question: "An aircraft travels 1400 nautical miles in 1 hour 45 minutes. What is the average speed of the aircraft? 飛機在 1 小時 45 分（1.75 小時）飛行 1400 海里，平均速度為？",
    options: [
      "A. 750 knots",
      "B. 2450 knots",
      "C. 800 knots"
    ],
    answer: "C"
  },
  {
    question: "Evaluate 0.8 × 0.004. 計算：0.8 × 0.004",
    options: [
      "A. 0.32",
      "B. 0.0032",
      "C. 0.032"
    ],
    answer: "B"
  },
  {
    question: "Convert 10 inches to millimetres. 將 10 英吋換算成毫米。1 inch = 25.4 mm",
    options: [
      "A. 2540 mm",
      "B. 254 mm",
      "C. 25.4 mm"
    ],
    answer: "B"
  },
  {
    question: "What number is the highest common factor of 24, 84, 120? 24、84、120 的最大公因數為？",
    options: [
      "A. 8",
      "B. 12",
      "C. 24"
    ],
    answer: "B"
  },
  {
    question: "0.0000314 can be written as… 0.0000314 可寫成科學記號為？",
    options: [
      "A. 3.14 × 10⁻⁵",
      "B. 3.14 × 10⁵",
      "C. 3.14 × 10⁻⁴"
    ],
    answer: "A"
  },
  {
    question: "What is the Lowest Common Multiple (LCM) of 5, 12, 20? 5、12、20 的最小公倍數為？",
    options: [
      "A. 60",
      "B. 120",
      "C. 5"
    ],
    answer: "A"
  },
  {
    question: "Evaluate 1/4{(4 − 6) − (2 − 8)}. 計算：1/4{(4 − 6) − (2 − 8)}",
    options: [
      "A. 3/4",
      "B. -2",
      "C. 1"
    ],
    answer: "C"
  },
  {
    question: "What is the average of the following numbers: 5, 13, 23, 12, 17? 數列 5, 13, 23, 12, 17 的平均值為？",
    options: [
      "A. 14",
      "B. 15",
      "C. 23"
    ],
    answer: "A"
  },
  {
    question: "What is the volume of a rectangular tank 5 m by 4 m by 150 cm? 一個長方體水槽，尺寸 5 m × 4 m × 150 cm，其體積為？",
    options: [
      "A. 3000 cu.m（3000 立方公尺）",
      "B. 30 sq.m（30 平方公尺）",
      "C. 30 cu.m（30 立方公尺）"
    ],
    answer: "C"
  },
  {
    question: "What is the depth of a rectangular tank whose volume is 40 cu.m and has a base 5 m by 10 m? 一個底面 5 m × 10 m、體積 40 立方公尺的水槽，其深度為？",
    options: [
      "A. 8 m",
      "B. 80 cm",
      "C. 0.08 m"
    ],
    answer: "B"
  },
  {
    question: "Convert 20 imperial gallons to litres. 將 20 英制加侖轉換為公升。",
    options: [
      "A. 909.2 litres",
      "B. 9.092 litres",
      "C. 90.92 litres"
    ],
    answer: "C"
  },
  {
    question: "To find the area of a circle use the formula… 求圓面積應使用下列哪個公式？",
    options: [
      "A. 2πd",
      "B. πr²",
      "C. 2πr"
    ],
    answer: "B"
  },
  {
    question: "What is the circumference of the top of a cylindrical tank whose radius is 3 metres? 半徑 3 公尺的圓柱形水槽頂面，其圓周長為？",
    options: [
      "A. 3π metres",
      "B. 6π metres",
      "C. 9π metres"
    ],
    answer: "B"
  },
  {
    question: "The volume of a certain cylinder is: （題目給定一圓柱，計算其體積）",
    options: [
      "A. 67.5 cu.m",
      "B. 675,000 cu.cm",
      "C. 6.75 cu.m"
    ],
    answer: "C"
  },
  {
    question: "What is the surface area of a cylindrical pipe of length 150 cm and diameter 5 cm? 長 150 cm、直徑 5 cm 的圓柱管，其側表面積為？",
    options: [
      "A. 1500π sq.cm",
      "B. 750π sq.cm",
      "C. 3750π sq.cm"
    ],
    answer: "B"
  },
  {
    question: "Find the value of 5/8 of 4/5. 求 4/5 的 5/8 為多少？",
    options: [
      "A. 1/2",
      "B. 3/4",
      "C. 25/32"
    ],
    answer: "A"
  },
  {
    question: "What is the square root of 4 raised to the fifth power? 4⁵ 的平方根為？",
    options: [
      "A. 32",
      "B. 128",
      "C. 64"
    ],
    answer: "A"
  },
  {
    question: "-3[8 − 3(5 + √9) − (7 − 9)] 的值為？",
    options: [
      "A. 60",
      "B. -42",
      "C. 42"
    ],
    answer: "C"
  },
  {
    question: "Which of the fractions is equivalent to 0.075? 下列哪個分數等於 0.075？",
    options: [
      "A. 1/40",
      "B. 3/4",
      "C. 3/40"
    ],
    answer: "C"
  },
  {
    question: "Express 3/8 as a percentage. 將 3/8 轉換為百分比。",
    options: [
      "A. 3.75%",
      "B. 0.375%",
      "C. 37.5%"
    ],
    answer: "C"
  },
  {
    question: "An aeroplane flies 1000 miles and uses 80 gallons of fuel. How much fuel will it use on a 2500 mile flight? 飛機飛 1000 英里耗油 80 加侖，若飛 2500 英里大約耗油多少？",
    options: [
      "A. 240 gallons",
      "B. 250 gallons",
      "C. 200 gallons"
    ],
    answer: "C"
  },
  {
    question: "A pinion gear with 16 teeth is driving a spur gear with 48 teeth at 120 RPM. Find the speed of the pinion gear. 16 齒小齒輪帶動 48 齒大齒輪，大齒輪轉速為 120 RPM，小齒輪轉速為？",
    options: [
      "A. 40 RPM",
      "B. 360 RPM",
      "C. 144 RPM"
    ],
    answer: "B"
  },
  {
    question: "What is the piston displacement of a master cylinder with a 4 cm diameter bore and a piston stroke of 10 cm? 活塞缸徑 4 cm，行程 10 cm，其排量（位移體積）為？",
    options: [
      "A. 8π cu.cm",
      "B. 40π cu.cm",
      "C. 160π cu.cm"
    ],
    answer: "B"
  },
  {
    question: "The curved surface area of a right cone is… 直圓錐的側表面積公式為？（R = 底面半徑, L = 母線長）",
    options: [
      "A. 11/3 πRL",
      "B. πRL",
      "C. πR²H"
    ],
    answer: "B"
  },
  {
    question: "How many millimetres in an inch? 1 英吋等於多少毫米？",
    options: [
      "A. 2.54",
      "B. 25.4",
      "C. 2540"
    ],
    answer: "B"
  },
  {
    question: "Find the area of a circular ring whose outer diameter is 10 cm and inner diameter is 6 cm. 外徑 10 cm、內徑 6 cm 的圓環面積為？",
    options: [
      "A. 64π sq.cm",
      "B. 16π sq.cm",
      "C. 4π sq.cm"
    ],
    answer: "B"
  },
  {
    question: "Find the area of the triangle shown. 求圖中三角形面積。（底 3 cm，高 4 cm）",
    options: [
      "A. 9 cm²",
      "B. 12 cm²",
      "C. 6 cm²"
    ],
    answer: "C"
  },
  {
    question: "What is the area of the shape shown, in square cm?（圖形面積 590 mm²，求 cm²）",
    options: [
      "A. 5900",
      "B. 590",
      "C. 5.9"
    ],
    answer: "C"
  },
  {
    question: "What is the area of the trapezium shown? 圖中梯形面積為？",
    options: [
      "A. Area cannot be calculated from information given.（無法計算）",
      "B. 27 cm²",
      "C. 24 cm²"
    ],
    answer: "C"
  },
  {
    question: "What is the depth of water in the tank shown if the volume of water is 4000 litres? 若水量為 4000 公升，圖示水槽中的水深為？",
    options: [
      "A. 80 cm",
      "B. 5 m",
      "C. 50 cm"
    ],
    answer: "C"
  },
  {
    question: "What is the area of the sector shown? Take π = 3.14. 下圖扇形（半徑 10 cm、圓心角 60°）的面積為？",
    options: [
      "A. 50 cm²",
      "B. 52 1/3 cm²",
      "C. 10.5 cm²"
    ],
    answer: "B"
  },
  {
    question: "What is the volume of metal used in the pipe shown? 圖示圓管的金屬體積為？",
    options: [
      "A. 4500π cm³",
      "B. 45π cm³",
      "C. 18000π cm³"
    ],
    answer: "A"
  },
  {
    question: "24/0 (twenty-four divided by zero) is… 24 除以 0 的結果是？",
    options: [
      "A. nothing（沒有）",
      "B. infinity（無限大）",
      "C. twenty four（24）"
    ],
    answer: "B"
  },
  {
    question: "If 20% of 120 is 24, what is 24% of 20? 已知 120 的 20% 是 24，那 20 的 24% 是多少？",
    options: [
      "A. 4.8",
      "B. 28",
      "C. 18"
    ],
    answer: "A"
  },
  {
    question: "A shop keeper sold his car for £120. If this is 80% of the buying price, how much loss did he make? 店家以 120 英鎊賣車，若這是買入價的 80%，他賠了多少？",
    options: [
      "A. £50",
      "B. £150",
      "C. £30"
    ],
    answer: "C"
  },
  {
    question: "3 + 4 − 5(4 − 2) = ?",
    options: [
      "A. 13",
      "B. 4",
      "C. -3"
    ],
    answer: "C"
  }
]


               function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
          }
          return array;
        }
        function fmtTime(s) {
          const m = Math.floor(s / 60).toString().padStart(2, "0");
          const sec = (s % 60).toString().padStart(2, "0");
          return m + ":" + sec;
        }
        function showView(idToShow) {
          ["welcome","quiz","results","bank"].forEach(id => {
            const el = document.getElementById(id);
            if (!el) return;
            if (id === idToShow) el.classList.remove("hidden");
            else el.classList.add("hidden");
          });
          window.scrollTo({ top: 0, behavior: "smooth" });
        }

        /* ====== 深色模式：持久化 ====== */
        const darkBtn = document.getElementById("darkModeToggle");
        function applyInitialTheme() {
          try {
            const saved = localStorage.getItem('theme');
            const isDark = (saved === 'dark') || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches);
            document.body.classList.toggle('dark', isDark);
            darkBtn.setAttribute('aria-pressed', String(isDark));
          } catch(e) {}
          document.documentElement.classList.remove('preload-dark');
        }
        applyInitialTheme();
        darkBtn.addEventListener('click', function(){
          const willDark = !document.body.classList.contains('dark');
          document.body.classList.toggle('dark', willDark);
          darkBtn.setAttribute('aria-pressed', String(willDark));
          try { localStorage.setItem('theme', willDark ? 'dark' : 'light'); } catch(e){}
        });

        /* ====== 測驗狀態 ====== */
        let shuffledQuestions;
        let current = 0, total = 0, timer = 80 * 60, interval, answers = [];
        let quizActive = false;
        const progressBar = document.getElementById("progressBar");

        /* ====== 元件 ====== */
        const startBtn = document.getElementById("startBtn");
        const prevBtn = document.getElementById("prevBtn");
        const leaveBtn = document.getElementById("leaveBtn");
        const retryBtn = document.getElementById("retryBtn");
        const showWrongBtn = document.getElementById("showWrongBtn");
        const showAllBtn = document.getElementById("showAllBtn");
        const openBankBtn = document.getElementById("openBankBtn");
        const toBankFromResults = document.getElementById("toBankFromResults");

        /* ====== 題庫瀏覽 ====== */
        const bankBody = document.getElementById("bankBody");
        const bankSearch = document.getElementById("bankSearch");
        const perPageSel = document.getElementById("perPage");
        const pageInfo = document.getElementById("pageInfo");
        const prevPageBtn = document.getElementById("prevPage");
        const nextPageBtn = document.getElementById("nextPage");
        const backToWelcome = document.getElementById("backToWelcome");
        const pagination = document.querySelector('#bank .pagination');

        // 抓取自訂時間輸入框
        const durationInput = document.getElementById("durationInput");

        let bankFiltered = questions.slice();
        let page = 1;
        function totalPages() {
          const sel = perPageSel.value;
          if (sel === 'all') return 1;
          return Math.max(1, Math.ceil(bankFiltered.length / parseInt(sel || "10")));
        }

        /* ====== 測驗流程 ====== */
        startBtn.addEventListener("click", () => {
          const n = document.getElementById("nameInput").value.trim();
          const qLimit = parseInt(document.getElementById("questionLimit").value);

          if (!n) return alert("請輸入姓名 / Enter your name");
          if (!qLimit || qLimit <= 0) return alert("請輸入要作答的題數,最多5題 / Enter number of questions");

          // 讀取自訂時間（分鐘）；預設 80，限制 1~300 分鐘
          let minutes = parseInt((durationInput && durationInput.value) ? durationInput.value : "80");
          if (isNaN(minutes)) minutes = 80;
          minutes = Math.max(1, Math.min(999, minutes));

          shuffledQuestions = shuffle(questions.slice()).slice(0, Math.min(qLimit, questions.length));
          total = shuffledQuestions.length;
          current = 0; answers = [];
          timer = minutes * 60;            // 用自訂分鐘
          quizActive = true;

          document.getElementById("welcomeName").innerText = "歡迎: " + n;
          document.getElementById("total").innerText = total;
          // 進場前先把右上角時計顯示成正確的起始值
          document.getElementById("timer").innerText = fmtTime(timer);
          updateProgress();

          // 確保不會重複計時
          clearInterval(interval);
          interval = setInterval(() => {
            if (timer > 0 && quizActive) {
              timer--;
              document.getElementById("timer").innerText = fmtTime(timer);
            } else if (quizActive && timer <= 0) {
              clearInterval(interval);
              finish();
            }
          }, 1000);

          showQ();
          showView("quiz");
        });

        prevBtn.addEventListener("click", () => {
          if (current > 0) {
            current--;
            answers.pop();
            showQ();
          }
        });

        leaveBtn.addEventListener("click", finish);
        retryBtn.addEventListener("click", () => location.reload());
        showWrongBtn.addEventListener("click", () => renderResults(answers.filter(a => !a.correct)));
        showAllBtn.addEventListener("click", () => renderResults(answers));
        openBankBtn.addEventListener("click", enterBank);
        toBankFromResults.addEventListener("click", enterBank);

        /* ====== 題庫事件 ====== */
        bankSearch.addEventListener("input", applyFilter);
        perPageSel.addEventListener("change", () => { page = 1; renderBank(); });
        prevPageBtn.addEventListener("click", () => { if (page > 1) { page--; renderBank(); } });
        nextPageBtn.addEventListener("click", () => { if (page < totalPages()) { page++; renderBank(); } });
        backToWelcome.addEventListener("click", () => showView("welcome"));

        function enterBank() {
          if (quizActive) { alert("測驗進行中不可瀏覽題庫。請先完成或離開考試。"); return; }
          if (!bankSearch.value) { bankFiltered = questions.slice(); page = 1; }
          renderBank();
          showView("bank");
        }

        function applyFilter() {
          const kw = bankSearch.value.trim().toLowerCase();
          bankFiltered = kw
            ? questions.filter(q => q.question.toLowerCase().includes(kw) ||
                                    q.options.some(o => o.toLowerCase().includes(kw)))
            : questions.slice();
          page = 1; renderBank();
        }

        function renderBank() {
          bankBody.innerHTML = "";
          const sel = perPageSel.value;
          const per = (sel === 'all') ? bankFiltered.length : parseInt(sel || "10");
          const start = (page - 1) * per;
          const items = (sel === 'all') ? bankFiltered.slice() : bankFiltered.slice(start, start + per);

          items.forEach((q, idx) => {
            const tr = document.createElement("tr");
            const num = (sel === 'all') ? (idx + 1) : (start + idx + 1);
            const correctText = q.options.find(o => o.charAt(0) === q.answer) || "";

            const optsHtml = q.options.map(o => {
              const isC = (o.charAt(0) === q.answer);
              return `<span class="opt-row ${isC ? 'is-correct' : ''}">${isC ? '<span class="ans-chip">正解</span>' : ''}${o}</span>`;
            }).join("");

            tr.innerHTML =
              '<td class="mono">#' + num + "</td>" +
              "<td>" + q.question + "</td>" +
              "<td>" + optsHtml + "</td>" +
              '<td class="mono ans-cell">' + (q.answer + "｜" + correctText) + "</td>";
            bankBody.append(tr);
          });

          const tp = totalPages();
          if (sel === 'all') {
            if (pagination) pagination.style.display = 'none';
            pageInfo.textContent = "全部顯示（共 " + bankFiltered.length + " 題）";
            prevPageBtn.disabled = true; nextPageBtn.disabled = true;
          } else {
            if (pagination) pagination.style.display = 'flex';
            pageInfo.textContent = "第 " + page + " / " + tp + " 頁（共 " + bankFiltered.length + " 題）";
            prevPageBtn.disabled = (page <= 1); nextPageBtn.disabled = (page >= tp);
          }
        }

        /* ====== 測驗：出題/進度/結束 ====== */
        function showQ() {
          if (current >= total) return finish();
          document.getElementById("current").innerText = current + 1;
          updateProgress();

          const q = shuffledQuestions[current];
          document.getElementById("questionText").innerText = q.question;

          const optDiv = document.getElementById("options");
          optDiv.innerHTML = "";

          let optionsWithFlag = q.options.map(option => ({ text: option, isAnswer: option.charAt(0) === q.answer }));
          optionsWithFlag = shuffle(optionsWithFlag);

          optionsWithFlag.forEach(opt => {
            const lbl = document.createElement("label");
            const rd = document.createElement("input");
            rd.type = "radio"; rd.name = "opt"; rd.value = opt.text;
            rd.onchange = function () {
              answers.push({
                q, selectedText: opt.text,
                correctText: q.options.find(optItem => optItem.charAt(0) === q.answer),
                correct: opt.isAnswer
              });
              current++; showQ();
            };
            lbl.append(rd, " ", opt.text);
            optDiv.append(lbl);
          });
        }

        function updateProgress() {
          const percent = (current / total) * 100;
          progressBar.style.width = percent + "%";
        }

        function finish() {
          clearInterval(interval);
          quizActive = false; // 結束後可進入題庫
          showView("results");
          renderResults(answers);
          document.getElementById("scoreSummary").innerText =
            "答對 " + answers.filter(a => a.correct).length +
            " 題 / 已作答 " + answers.length + " 題 / 共 " + total + " 題";
        }

        function renderResults(data) {
          const tb = document.getElementById("resultsBody");
          tb.innerHTML = "";
          data.forEach(a => {
            const tr = document.createElement("tr");
            tr.innerHTML =
              "<td>" + a.q.question + "</td>" +
              "<td>" + a.selectedText + "</td>" +
              '<td class="ans-cell">' + a.correctText + "</td>" +
              "<td>" + (a.correct ? "O" : "X") + "</td>";
            if (!a.correct) tr.classList.add("wrong");
            tb.append(tr);
          });
        }
      });
    </script>
  </body>
</html>
