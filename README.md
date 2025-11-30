# family-trip
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>新竹 • 家族小旅行</title>
    
    <!-- PWA 設定: 讓網頁像 App 一樣全螢幕執行 -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#f9f9f6">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/2060/2060193.png">
    <link rel="icon" type="image/png" href="https://cdn-icons-png.flaticon.com/512/2060/2060193.png">

    <!-- 引入 Tailwind CSS (樣式庫) -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- 引入 FontAwesome (圖示庫) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <!-- 引入 Google Fonts (日系字體) -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&family=Zen+Maru+Gothic:wght@500;700&display=swap" rel="stylesheet">

    <!-- 自定義主題配置 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        muji: {
                            bg: '#f9f9f6',       /* 米白紙張質感背景 */
                            card: '#ffffff',     /* 純白卡片 */
                            text: '#444444',     /* 深灰文字 */
                            sub: '#888888',      /* 淺灰副標 */
                            accent: '#9a8c83',   /* 暖褐點綴色 */
                            line: '#e6e6e6',     /* 極細分隔線 */
                            highlight: '#d46b6b' /* 重點提示紅 */
                        }
                    },
                    fontFamily: {
                        sans: ['"Noto Sans TC"', 'sans-serif'],
                        round: ['"Zen Maru Gothic"', 'sans-serif'] /* 標題專用圓體 */
                    },
                    boxShadow: {
                        'soft': '0 4px 20px -2px rgba(0, 0, 0, 0.05)',
                        'float': '0 10px 30px -10px rgba(0, 0, 0, 0.1)'
                    }
                }
            }
        }
    </script>

    <style>
        /* 隱藏滾動條但保留滾動功能 */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        /* 頁面切換淡入動畫 */
        .fade-in { animation: fadeIn 0.4s cubic-bezier(0.4, 0, 0.2, 1); }
        @keyframes fadeIn { 
            from { opacity: 0; transform: translateY(10px); } 
            to { opacity: 1; transform: translateY(0); } 
        }

        /* iOS 底部安全區域設定 */
        body { padding-bottom: env(safe-area-inset-bottom); }
        .pb-safe { padding-bottom: env(safe-area-inset-bottom); }

        /* 自定義 Checkbox 樣式 */
        .custom-check:checked + div {
            background-color: #9a8c83;
            border-color: #9a8c83;
        }
        .custom-check:checked + div:after {
            content: '';
            position: absolute;
            left: 5px; top: 2px; width: 5px; height: 9px;
            border: solid white; border-width: 0 2px 2px 0;
            transform: rotate(45deg);
        }
    </style>
</head>
<body class="bg-muji-bg text-muji-text font-sans h-[100dvh] flex flex-col overflow-hidden">

    <!-- HEADER: 頂部標題 -->
    <header class="bg-muji-bg/90 backdrop-blur-md px-6 pt-12 pb-4 z-20 shrink-0 border-b border-transparent transition-all" id="main-header">
        <div class="flex justify-between items-end">
            <div>
                <h1 class="text-2xl font-round font-bold tracking-widest text-muji-accent">
                    <i class="fa-solid fa-cloud text-lg mr-2 opacity-60"></i>家族旅行
                </h1>
                <p class="text-[10px] text-muji-sub mt-1 tracking-[0.2em] uppercase">Hsinchu • Dec 27-28</p>
            </div>
            <div class="text-right">
                <span class="text-3xl font-round font-bold text-muji-text">2</span>
                <span class="text-xs text-muji-sub">DAYS</span>
            </div>
        </div>
    </header>

    <!-- MAIN: 主要內容顯示區 (動態切換) -->
    <main id="app-container" class="flex-1 overflow-y-auto no-scrollbar p-6 pb-32 relative">
        <!-- 內容由 JavaScript 渲染 -->
    </main>

    <!-- NAV: 底部導航列 -->
    <nav class="fixed bottom-0 w-full bg-white/90 backdrop-blur-xl border-t border-muji-line pb-safe pt-2 shadow-[0_-5px_20px_-5px_rgba(0,0,0,0.05)] z-50">
        <div class="flex justify-around items-center h-16 max-w-md mx-auto">
            <button onclick="switchTab('plan')" class="nav-btn group flex flex-col items-center justify-center w-1/4 text-muji-sub transition-all duration-300" id="btn-plan">
                <div class="mb-1 p-1 rounded-full group-active:scale-90 transition-transform">
                    <i class="fa-regular fa-calendar-check text-xl"></i>
                </div>
                <span class="text-[10px] font-medium tracking-wider">行程</span>
            </button>
            <button onclick="switchTab('guide')" class="nav-btn group flex flex-col items-center justify-center w-1/4 text-muji-sub transition-all duration-300" id="btn-guide">
                <div class="mb-1 p-1 rounded-full group-active:scale-90 transition-transform">
                    <i class="fa-solid fa-map-location-dot text-xl"></i>
                </div>
                <span class="text-[10px] font-medium tracking-wider">導覽</span>
            </button>
            <button onclick="switchTab('lists')" class="nav-btn group flex flex-col items-center justify-center w-1/4 text-muji-sub transition-all duration-300" id="btn-lists">
                <div class="mb-1 p-1 rounded-full group-active:scale-90 transition-transform">
                    <i class="fa-solid fa-list-check text-xl"></i>
                </div>
                <span class="text-[10px] font-medium tracking-wider">清單</span>
            </button>
            <button onclick="switchTab('info')" class="nav-btn group flex flex-col items-center justify-center w-1/4 text-muji-sub transition-all duration-300" id="btn-info">
                <div class="mb-1 p-1 rounded-full group-active:scale-90 transition-transform">
                    <i class="fa-solid fa-circle-info text-xl"></i>
                </div>
                <span class="text-[10px] font-medium tracking-wider">資訊</span>
            </button>
        </div>
    </nav>

    <!-- MODAL: 安裝教學 (首次進入顯示) -->
    <div id="install-guide" class="fixed inset-0 bg-black/60 z-[100] hidden flex items-end justify-center pb-safe">
        <div class="bg-white m-4 p-6 rounded-3xl shadow-2xl w-full max-w-sm relative animate-[bounce-up_0.5s_ease-out]">
            <button onclick="closeGuide()" class="absolute top-4 right-4 text-gray-300 hover:text-gray-500 transition"><i class="fa-solid fa-xmark text-xl"></i></button>
            <div class="text-center">
                <div class="w-12 h-12 bg-muji-bg rounded-full flex items-center justify-center mx-auto mb-4 text-2xl text-muji-accent">
                    <i class="fa-solid fa-mobile-screen-button"></i>
                </div>
                <h3 class="font-round font-bold text-lg mb-2 text-muji-text">加入主畫面</h3>
                <p class="text-sm text-gray-500 mb-6 leading-relaxed">為了更好的體驗，請點擊瀏覽器選單，選擇「加入主畫面」，就能像 App 一樣使用囉！</p>
                <button onclick="closeGuide()" class="bg-muji-text text-white w-full py-3 rounded-xl font-bold text-sm tracking-wide shadow-lg active:scale-95 transition">我知道了</button>
            </div>
            <!-- 指示箭頭 (簡單動畫) -->
            <div class="absolute -bottom-8 left-1/2 -translate-x-1/2 text-white/80 text-2xl animate-bounce">
                <i class="fa-solid fa-arrow-down"></i>
            </div>
        </div>
    </div>

    <!-- JAVASCRIPT 邏輯核心 -->
    <script>
        /* ================= 資料設定區 (DATA) ================= */
        
        // 1. 行程表資料
        const tripData = {
            days: [
                {
                    date: "12/27",
                    weekday: "週六",
                    title: "內灣慢活．山林溫泉",
                    events: [
                        { time: "10:00", title: "準時出發", desc: "快樂出門，確認行李帶齊", type: "transport", highlight: true },
                        { time: "11:00", title: "竹東包Sir牛肉麵", desc: "在地人氣午餐 (水餃/牛肉麵)", type: "food", link: "https://maps.app.goo.gl/search/竹東包sir牛肉麵" },
                        { time: "13:00", title: "內灣老街", desc: "逛老街、走吊橋、吃野薑花粽", type: "spot", link: "https://maps.app.goo.gl/search/內灣老街" },
                        { time: "15:00", title: "喜蜜的家 民宿", desc: "Check-in 休息 (車程約15分)", type: "stay", link: "https://maps.app.goo.gl/search/喜蜜的家民宿" },
                        { time: "17:00", title: "石上湯屋", desc: "享受美人湯溫泉 (車程10分)", type: "relax", link: "https://maps.app.goo.gl/search/石上湯屋" },
                        { time: "19:00", title: "溫馨晚餐", desc: "民宿內煮火鍋圍爐", type: "food" }
                    ]
                },
                {
                    date: "12/28",
                    weekday: "週日",
                    title: "質感購物．日式饗宴",
                    events: [
                        { time: "09:00", title: "山林早餐", desc: "享用民宿準備的早餐", type: "food" },
                        { time: "11:00", title: "退房出發", desc: "前往竹北遠百 (車程約50分)", type: "transport", highlight: true },
                        { time: "12:00", title: "竹北遠東百貨", desc: "逛街購物、客家圓樓造型", type: "spot", link: "https://maps.app.goo.gl/search/竹北遠東百貨" },
                        { time: "12:30", title: "涮乃葉", desc: "日式涮涮鍋吃到飽 (100分鐘)", type: "food", link: "https://maps.app.goo.gl/search/涮乃葉+竹北遠百" },
                        { time: "15:00", title: "快樂返程", desc: "帶著戰利品回家", type: "transport" }
                    ]
                }
            ]
        };

        // 2. 景點深度導覽資料
        const guideData = [
            {
                title: "內灣老街",
                tag: "懷舊鐵道",
                desc: "位於新竹橫山鄉，舊時為運送林木與礦產的重鎮。必訪『內灣戲院』感受懷舊氛圍，必吃『野薑花粽』品嚐客家風味。走在內灣吊橋上，俯瞰油羅溪谷，是新竹最經典的山城漫遊路線。",
                map: "內灣老街"
            },
            {
                title: "石上湯屋",
                tag: "美人湯",
                desc: "隱身於尖石鄉翠綠山林間，提供優質的碳酸氫鈉泉（俗稱美人湯），能滋潤肌膚。半露天的湯屋設計，讓人在泡湯的同時能呼吸森林芬多精，夜晚更能仰望星空，徹底放鬆。",
                map: "新竹尖石石上湯屋"
            },
            {
                title: "竹北遠東百貨",
                tag: "客家圓樓",
                desc: "新竹最新的購物地標，建築外觀融合了客家山城與圓樓意象。內部空間寬敞舒適，集結了眾多知名品牌與美食餐廳，是雨天備案或午後休閒的最佳去處。",
                map: "竹北遠東百貨"
            }
        ];

        // 3. 預設行前清單
        const defaultChecklist = [
            "身分證 / 健保卡",
            "手機充電器 / 行動電源",
            "個人藥品 (暈車/常備藥)",
            "換洗衣物 (2套)",
            "保暖外套 (新竹風大)",
            "泳衣/泳帽 (若泡大眾池)",
            "現金 (老街小吃使用)",
            "保溫瓶 / 水壺"
        ];

        /* ================= 程式邏輯區 (LOGIC) ================= */

        let currentTab = 'plan';
        const container = document.getElementById('app-container');
        
        // 讀取 LocalStorage 資料
        let checklist = JSON.parse(localStorage.getItem('trip_checklist')) || defaultChecklist.map((t, i) => ({ id: i, text: t, checked: false }));
        let memoContent = localStorage.getItem('trip_memo') || '';

        // 切換分頁功能
        function switchTab(tab) {
            currentTab = tab;
            
            // 更新按鈕樣式
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-muji-accent');
                btn.classList.add('text-muji-sub');
                btn.querySelector('div').classList.remove('bg-muji-accent/10');
            });
            const activeBtn = document.getElementById(`btn-${tab}`);
            activeBtn.classList.replace('text-muji-sub', 'text-muji-accent');
            activeBtn.querySelector('div').classList.add('bg-muji-accent/10');

            // 渲染內容 (加入淡入動畫)
            container.innerHTML = '';
            container.classList.remove('fade-in');
            void container.offsetWidth; // Trigger Reflow
            container.classList.add('fade-in');

            if (tab === 'plan') renderPlan();
            else if (tab === 'guide') renderGuide();
            else if (tab === 'lists') renderLists();
            else if (tab === 'info') renderInfo();
        }

        // 渲染行程表
        function renderPlan() {
            let html = '';
            tripData.days.forEach((day, index) => {
                // 時間軸線條樣式
                const isLastDay = index === tripData.days.length - 1;
                
                html += `
                    <div class="mb-10 relative">
                        <!-- 日期標頭 -->
                        <div class="sticky top-0 bg-muji-bg/95 backdrop-blur-sm py-4 z-10 mb-6 flex items-baseline border-b border-muji-line">
                            <h2 class="text-3xl font-round font-bold text-muji-text mr-3">${day.date}</h2>
                            <span class="text-sm font-bold text-muji-accent bg-muji-accent/10 px-2 py-1 rounded-md">${day.weekday}</span>
                            <span class="text-xs text-muji-sub ml-auto tracking-wide">${day.title}</span>
                        </div>
                        
                        <!-- 時間軸容器 -->
                        <div class="ml-3 pl-6 border-l-2 border-dashed border-muji-line/60 space-y-8 pb-4">
                `;

                day.events.forEach(event => {
                    // 圖示邏輯
                    let icon = 'fa-circle', iconColor = 'text-muji-sub';
                    if (event.type === 'food') { icon = 'fa-utensils'; iconColor = 'text-orange-400'; }
                    if (event.type === 'transport') { icon = 'fa-car-side'; iconColor = 'text-blue-400'; }
                    if (event.type === 'stay') { icon = 'fa-bed'; iconColor = 'text-indigo-400'; }
                    if (event.type === 'spot') { icon = 'fa-camera'; iconColor = 'text-emerald-400'; }
                    if (event.type === 'relax') { icon = 'fa-hot-tub-person'; iconColor = 'text-rose-400'; }

                    // 高亮樣式
                    const cardClass = event.highlight 
                        ? 'bg-white border-l-4 border-l-muji-highlight shadow-soft ring-1 ring-red-50' 
                        : 'bg-white shadow-soft border border-transparent';

                    html += `
                        <div class="relative group">
                            <!-- 時間軸圓點 -->
                            <div class="absolute -left-[34px] top-0 w-8 h-8 rounded-full bg-muji-bg flex items-center justify-center border-2 border-white shadow-sm z-10">
                                <i class="fa-solid ${icon} text-xs ${iconColor}"></i>
                            </div>
                            
                            <!-- 卡片本體 -->
                            <div class="${cardClass} rounded-2xl p-5 transition-transform active:scale-[0.98]">
                                <div class="flex justify-between items-start mb-2">
                                    <span class="font-round font-bold text-xl text-muji-text">${event.time}</span>
                                    ${event.link ? `
                                        <a href="${event.link}" target="_blank" class="text-xs flex items-center gap-1 text-muji-accent bg-muji-bg px-3 py-1.5 rounded-full border border-muji-line hover:bg-white transition">
                                            <i class="fa-solid fa-location-arrow"></i> 導航
                                        </a>` : ''}
                                </div>
                                <h3 class="font-bold text-base text-muji-text mb-1">${event.title}</h3>
                                <p class="text-sm text-muji-sub font-light leading-relaxed">${event.desc}</p>
                            </div>
                        </div>
                    `;
                });
                html += `</div></div>`;
            });
            // 底部留白
            html += `<div class="h-8"></div>`;
            container.innerHTML = html;
        }

        // 渲染導覽
        function renderGuide() {
            let html = `<div class="space-y-8">`;
            guideData.forEach(spot => {
                html += `
                    <div class="bg-white rounded-3xl overflow-hidden shadow-float">
                        <!-- 地圖預覽圖 -->
                        <div class="h-44 bg-gray-100 w-full relative group">
                            <iframe 
                                width="100%" height="100%" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" 
                                src="https://www.openstreetmap.org/export/embed.html?bbox=121.10,24.60,121.25,24.80&layer=mapnik&marker=${spot.map}"
                                class="absolute inset-0 opacity-60 grayscale group-hover:grayscale-0 transition duration-700">
                            </iframe>
                            <div class="absolute inset-0 bg-gradient-to-t from-black/50 to-transparent pointer-events-none"></div>
                            
                            <!-- 標籤 -->
                            <span class="absolute top-4 left-4 bg-white/90 backdrop-blur px-3 py-1 rounded-md text-xs font-bold text-muji-text shadow-sm tracking-widest">
                                #${spot.tag}
                            </span>

                            <!-- 開啟地圖按鈕 -->
                            <a href="https://www.google.com/maps/search/${spot.map}" target="_blank" class="absolute bottom-4 right-4 bg-muji-text text-white px-4 py-2 rounded-full text-xs font-bold shadow-lg flex items-center gap-2 active:scale-95 transition">
                                <i class="fa-solid fa-map-pin"></i> Google Map
                            </a>
                        </div>
                        
                        <!-- 文字內容 -->
                        <div class="p-6">
                            <h2 class="text-2xl font-round font-bold text-muji-text mb-3">${spot.title}</h2>
                            <p class="text-sm text-gray-500 leading-7 text-justify font-light">
                                ${spot.desc}
                            </p>
                        </div>
                    </div>
                `;
            });
            html += `</div>`;
            container.innerHTML = html;
        }

        // 渲染清單
        function renderLists() {
            // Checklist
            let html = `
                <div class="mb-10">
                    <h3 class="text-lg font-round font-bold mb-4 text-muji-accent flex items-center">
                        <i class="fa-solid fa-suitcase mr-2"></i> 行前準備
                    </h3>
                    <div class="space-y-3">
            `;
            
            checklist.forEach((item, i) => {
                html += `
                    <label class="flex items-center space-x-4 bg-white p-4 rounded-2xl shadow-soft cursor-pointer select-none active:bg-gray-50 transition">
                        <div class="relative flex items-center">
                            <input type="checkbox" class="custom-check peer appearance-none w-6 h-6 border-2 border-gray-300 rounded-lg transition-colors cursor-pointer" 
                                onchange="toggleCheck(${i})" ${item.checked ? 'checked' : ''}>
                            <div class="absolute inset-0 rounded-lg pointer-events-none transition-colors"></div>
                        </div>
                        <span class="text-sm font-medium ${item.checked ? 'line-through text-gray-300' : 'text-muji-text'} flex-1">
                            ${item.text}
                        </span>
                    </label>
                `;
            });
            html += `</div></div>`;

            // Memo
            // URL Auto-link logic
            const displayContent = memoContent.replace(/(https?:\/\/[^\s]+)/g, '<a href="$1" target="_blank" class="text-blue-500 underline relative z-10">$1</a>');

            html += `
                <div>
                    <h3 class="text-lg font-round font-bold mb-4 text-muji-accent flex items-center">
                        <i class="fa-regular fa-note-sticky mr-2"></i> 備忘錄
                    </h3>
                    <div class="bg-white rounded-2xl shadow-soft p-1 relative">
                        <textarea 
                            class="w-full h-48 p-4 rounded-xl text-sm text-gray-600 bg-transparent resize-none outline-none leading-relaxed placeholder-gray-300" 
                            placeholder="點擊輸入筆記或貼上網址..." 
                            oninput="saveMemo(this.value)">${memoContent}</textarea>
                        <div class="absolute bottom-3 right-4 text-[10px] text-gray-300 flex items-center gap-1">
                            <i class="fa-solid fa-check"></i> Auto Saved
                        </div>
                    </div>
                </div>
            `;
            container.innerHTML = html;
        }

        // 渲染資訊
        function renderInfo() {
            container.innerHTML = `
                <div class="space-y-6">
                    <!-- 天氣卡片 -->
                    <div class="bg-gradient-to-br from-blue-50 to-white rounded-3xl p-6 shadow-soft active:scale-[0.98] transition cursor-pointer border border-blue-100/50" 
                         onclick="window.open('https://www.cwa.gov.tw/V8/C/W/County/County.html?CID=10004', '_blank')">
                        <div class="flex justify-between items-center">
                            <div>
                                <h3 class="font-bold text-xl text-muji-text mb-1">新竹天氣</h3>
                                <p class="text-xs text-blue-400 font-bold bg-blue-100/50 px-2 py-1 rounded inline-block">點擊查看預報</p>
                            </div>
                            <i class="fa-solid fa-cloud-sun text-5xl text-blue-200"></i>
                        </div>
                    </div>

                    <!-- 緊急聯絡 -->
                    <div class="bg-white rounded-3xl p-6 shadow-soft">
                        <h3 class="font-bold text-lg text-muji-text mb-4 border-b border-muji-line pb-2 flex items-center">
                            <i class="fa-solid fa-shield-heart text-red-400 mr-2"></i> 緊急聯絡
                        </h3>
                        <div class="grid grid-cols-2 gap-4">
                            <a href="tel:110" class="bg-gray-50 p-4 rounded-2xl text-center active:bg-gray-100 transition">
                                <div class="text-2xl font-bold text-muji-text">110</div>
                                <div class="text-xs text-gray-400 mt-1">警察局</div>
                            </a>
                            <a href="tel:119" class="bg-gray-50 p-4 rounded-2xl text-center active:bg-gray-100 transition">
                                <div class="text-2xl font-bold text-muji-text">119</div>
                                <div class="text-xs text-gray-400 mt-1">救護車 / 消防</div>
                            </a>
                        </div>
                    </div>

                    <!-- 民宿資訊 -->
                    <div class="bg-[#f2f0eb] rounded-3xl p-6 border border-[#e6e2db]">
                        <h3 class="font-bold text-base text-[#8c7b75] mb-3 flex items-center">
                            <i class="fa-solid fa-house-chimney mr-2"></i> 喜蜜的家 Tips
                        </h3>
                        <ul class="text-sm text-[#5c5c5c] space-y-3 pl-2">
                            <li class="flex items-start gap-2"><i class="fa-solid fa-clock mt-1 text-[10px] opacity-50"></i> Check-in: 15:00</li>
                            <li class="flex items-start gap-2"><i class="fa-solid fa-clock mt-1 text-[10px] opacity-50"></i> Check-out: 11:00</li>
                            <li class="flex items-start gap-2"><i class="fa-solid fa-pump-soap mt-1 text-[10px] opacity-50"></i> 備品：浴巾、沐浴乳、洗髮乳</li>
                            <li class="flex items-start gap-2"><i class="fa-solid fa-utensils mt-1 text-[10px] opacity-50"></i> 含第二天早餐</li>
                        </ul>
                    </div>
                </div>
            `;
        }

        /* ================= 功能函式 (FUNCTIONS) ================= */

        // 切換 Checklist 狀態並存檔
        function toggleCheck(index) {
            checklist[index].checked = !checklist[index].checked;
            localStorage.setItem('trip_checklist', JSON.stringify(checklist));
            renderLists();
        }

        // 儲存 Memo
        function saveMemo(val) {
            memoContent = val;
            localStorage.setItem('trip_memo', val);
        }

        // 關閉安裝教學
        function closeGuide() {
            document.getElementById('install-guide').classList.add('hidden');
            localStorage.setItem('guide_shown_v2', 'true');
        }

        // 監聽滾動，讓 Header 有玻璃擬態效果變化
        document.getElementById('app-container').addEventListener('scroll', function() {
            const header = document.getElementById('main-header');
            if (this.scrollTop > 20) {
                header.classList.add('shadow-sm', 'bg-white/90');
                header.classList.remove('bg-muji-bg/90');
            } else {
                header.classList.remove('shadow-sm', 'bg-white/90');
                header.classList.add('bg-muji-bg/90');
            }
        });

        /* ================= 初始執行 (INIT) ================= */
        
        switchTab('plan'); // 預設顯示行程頁

        // 判斷是否顯示安裝教學 (僅在手機上且未看過時顯示)
        const isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);
        const hasShownGuide = localStorage.getItem('guide_shown_v2');
        
        if (isMobile && !hasShownGuide) {
            setTimeout(() => {
                document.getElementById('install-guide').classList.remove('hidden');
            }, 1500); // 延遲 1.5 秒跳出比較不干擾
        }

    </script>
</body>
</html>

