<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>傻B - HKDSE 中文科十二篇範文問答平台</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&family=Noto+Serif+TC:wght@500;700;900&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Noto Sans TC"', 'sans-serif'],
                        serif: ['"Noto Serif TC"', 'serif'],
                    },
                    colors: {
                        dse: {
                            navy: '#1B365D',
                            red: '#9B1B30',
                            gold: '#D4AF37',
                            light: '#F8FAF9',
                            dark: '#1A202C'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: #F3F4F6;
        }
        .classical-text {
            font-family: 'Noto Serif TC', serif;
            line-height: 2.1;
            letter-spacing: 0.05em;
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #F1F1F1;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #C1C1C1;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #A8A8A8;
        }
    </style>
</head>
<body class="text-slate-800 antialiased bg-slate-100 min-h-screen flex flex-col justify-between">

    <!-- MANDATORY LOGIN OVERLAY GATEWAY (Visible by default until authenticated) -->
    <div id="login-gate" class="fixed inset-0 z-50 bg-slate-900/90 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-2xl shadow-2xl border border-slate-100 overflow-hidden transform transition-all">
            <div class="bg-dse-navy text-white p-6 text-center relative">
                <span class="bg-dse-gold text-dse-navy font-black text-xs px-3 py-1 rounded-full uppercase tracking-wider mb-2 inline-block">HKDSE 中文卷一專攻</span>
                <h2 class="text-2xl font-black tracking-wide">傻B - HKDSE 中文科</h2>
                <p class="text-sm font-medium text-amber-300 mt-1">十二篇範文問答平台</p>
            </div>
            
            <div class="p-6">
                <!-- Toggle Tabs -->
                <div class="flex border-b border-slate-200 mb-6">
                    <button id="gate-tab-login" onclick="switchGateTab('login')" class="w-1/2 py-2 text-center font-bold text-dse-navy border-b-2 border-dse-navy">
                        考生登入
                    </button>
                    <button id="gate-tab-register" onclick="switchGateTab('register')" class="w-1/2 py-2 text-center font-bold text-slate-400 border-b-2 border-transparent">
                        帳號註冊
                    </button>
                </div>

                <form id="gate-auth-form" onsubmit="handleGateAuth(event)">
                    <div class="space-y-4">
                        <div>
                            <label class="block text-xs font-bold text-slate-700 uppercase mb-1">考生姓名 / 用戶名</label>
                            <div class="relative">
                                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400"><i class="fa-solid fa-user"></i></span>
                                <input type="text" id="gate-username" required placeholder="例如：張小明" class="w-full pl-9 pr-3.5 py-2.5 border border-slate-300 rounded-lg focus:ring-2 focus:ring-dse-navy focus:outline-none text-sm">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-700 uppercase mb-1">密碼</label>
                            <div class="relative">
                                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400"><i class="fa-solid fa-key"></i></span>
                                <input type="password" id="gate-password" required placeholder="請輸入密碼" class="w-full pl-9 pr-3.5 py-2.5 border border-slate-300 rounded-lg focus:ring-2 focus:ring-dse-navy focus:outline-none text-sm">
                            </div>
                        </div>
                        <div id="gate-confirm-pw-container" class="hidden">
                            <label class="block text-xs font-bold text-slate-700 uppercase mb-1">確認密碼</label>
                            <div class="relative">
                                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-400"><i class="fa-solid fa-check-double"></i></span>
                                <input type="password" id="gate-confirm-password" placeholder="請再次輸入密碼" class="w-full pl-9 pr-3.5 py-2.5 border border-slate-300 rounded-lg focus:ring-2 focus:ring-dse-navy focus:outline-none text-sm">
                            </div>
                        </div>
                    </div>

                    <p id="gate-auth-error" class="text-xs text-red-600 font-bold mt-3 hidden text-center"></p>

                    <button type="submit" id="gate-submit-btn" class="w-full mt-6 bg-dse-navy hover:bg-slate-800 text-white font-bold py-3 rounded-xl shadow-md transition">
                        登入平台開始練習
                    </button>
                </form>

                <p class="text-xs text-center text-slate-400 mt-4">
                    <i class="fa-solid fa-shield-halved mr-1"></i>必須先登入方可進入問答平台解鎖十二篇範文題目
                </p>
            </div>
        </div>
    </div>

    <!-- MAIN APPLICATION CONTAINER (Hidden until authenticated) -->
    <div id="main-app-container" class="hidden min-h-screen flex flex-col">
        <!-- Header -->
        <header class="bg-dse-navy text-white shadow-md sticky top-0 z-40">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <span class="bg-dse-gold text-dse-navy font-black text-xl px-2.5 py-1 rounded shadow">傻B</span>
                    <div>
                        <h1 class="font-bold text-lg leading-tight tracking-wide">HKDSE 中文科十二篇範文問答平台</h1>
                        <p class="text-xs text-slate-300 hidden sm:block">指定文言經典篇章全收錄研習 Portal</p>
                    </div>
                </div>

                <!-- User Bar -->
                <div class="flex items-center space-x-4">
                    <div class="flex items-center space-x-3">
                        <span class="text-sm bg-blue-900/60 px-3 py-1.5 rounded-full border border-blue-400/30 flex items-center">
                            <i class="fa-solid fa-user-graduate text-dse-gold mr-2"></i>
                            <span id="nav-user-name" class="font-medium text-amber-200">已登入學生</span>
                        </span>
                        <button onclick="logout()" class="text-xs bg-red-600 hover:bg-red-700 text-white font-semibold py-1.5 px-3 rounded transition shadow">
                            <i class="fa-solid fa-right-from-bracket mr-1"></i>登出
                        </button>
                    </div>
                </div>
            </div>
        </header>

        <!-- Main Content Portal -->
        <main class="flex-grow max-w-7xl w-full mx-auto p-4 sm:p-6 lg:p-8">
            
            <!-- Dashboard View -->
            <div id="dashboard-view">
                <!-- User Stats Summary Bar -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center space-x-4">
                        <div class="p-3 bg-blue-100 text-blue-700 rounded-lg"><i class="fa-solid fa-file-signature text-xl"></i></div>
                        <div>
                            <div class="text-xs text-slate-500 font-medium">累計練習次數</div>
                            <div id="stat-completed" class="text-xl font-bold text-slate-800">0</div>
                        </div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center space-x-4">
                        <div class="p-3 bg-emerald-100 text-emerald-700 rounded-lg"><i class="fa-solid fa-trophy text-xl"></i></div>
                        <div>
                            <div class="text-xs text-slate-500 font-medium">平均得分率</div>
                            <div id="stat-avg-score" class="text-xl font-bold text-slate-800">0%</div>
                        </div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center space-x-4">
                        <div class="p-3 bg-purple-100 text-purple-700 rounded-lg"><i class="fa-solid fa-chart-line text-xl"></i></div>
                        <div>
                            <div class="text-xs text-slate-500 font-medium">預測評級估算</div>
                            <div id="stat-predicted-level" class="text-xl font-bold text-purple-700">Level --</div>
                        </div>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center space-x-4">
                        <div class="p-3 bg-amber-100 text-amber-700 rounded-lg"><i class="fa-solid fa-clock-rotate-left text-xl"></i></div>
                        <div>
                            <div class="text-xs text-slate-500 font-medium">最近一次練習</div>
                            <div id="stat-last-active" class="text-xs font-bold text-slate-800 mt-1">無記錄</div>
                        </div>
                    </div>
                </div>

                <!-- Passage Cards Grid -->
                <div class="mb-6 flex justify-between items-center">
                    <div>
                        <h2 class="text-2xl font-bold text-slate-800">十二篇指定文言經典篇章</h2>
                        <p class="text-sm text-slate-500">點擊任意篇章開始試題演練，試題含考評局關鍵字詞釋義與長答示範答案</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="passage-cards-container">
                    <!-- Passage cards injected dynamically via JS -->
                </div>
            </div>

            <!-- Quiz View (Initially Hidden) -->
            <div id="quiz-view" class="hidden">
                <!-- Navigation Bar -->
                <div class="mb-4 flex justify-between items-center bg-white p-3 rounded-lg shadow-sm border border-slate-200">
                    <button onclick="returnToDashboard()" class="text-sm font-semibold text-slate-600 hover:text-dse-navy flex items-center transition">
                        <i class="fa-solid fa-arrow-left mr-2"></i>返回十二篇主頁
                    </button>
                    <div class="flex items-center space-x-4">
                        <span id="quiz-title-badge" class="font-bold text-dse-navy text-base">篇章練習</span>
                        <span class="text-xs bg-slate-100 text-slate-600 px-2.5 py-1 rounded border">總分：<span id="passage-max-score">0</span>分</span>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                    <!-- Passage Content -->
                    <div class="lg:col-span-5">
                        <div class="bg-white rounded-xl shadow-md border border-slate-200 p-6 sticky top-20 max-h-[80vh] flex flex-col">
                            <div class="border-b pb-3 mb-4 flex justify-between items-center">
                                <div>
                                    <span class="text-xs font-bold text-dse-red bg-red-50 border border-red-200 px-2 py-0.5 rounded">HKDSE 範文</span>
                                    <h3 id="passage-title" class="text-xl font-bold text-dse-navy font-serif mt-1">--</h3>
                                </div>
                                <span id="passage-author" class="text-sm font-serif text-slate-500 font-bold">--</span>
                            </div>
                            
                            <!-- Scrollable Passage Body -->
                            <div id="passage-content" class="classical-text text-slate-800 text-base sm:text-lg overflow-y-auto custom-scrollbar pr-3 space-y-4 flex-grow">
                                <!-- Text inserted via JS -->
                            </div>

                            <div class="mt-4 pt-3 border-t text-xs text-slate-400 flex justify-between items-center">
                                <span><i class="fa-solid fa-info-circle mr-1"></i>滾動可查閱原文全文</span>
                                <span class="font-mono">卷一 閱讀能力</span>
                            </div>
                        </div>
                    </div>

                    <!-- Questions Sheet -->
                    <div class="lg:col-span-7">
                        <form id="quiz-form" onsubmit="event.preventDefault(); submitQuiz();">
                            <div id="questions-container" class="space-y-6">
                                <!-- Questions rendered via JS -->
                            </div>

                            <!-- Submit Box -->
                            <div class="mt-8 bg-white p-6 rounded-xl border border-slate-200 shadow-sm flex flex-col sm:flex-row justify-between items-center gap-4">
                                <div>
                                    <h4 class="font-bold text-slate-800">完成練習？</h4>
                                    <p class="text-xs text-slate-500">提交後系統將即時核對答案並計算預測 Level。</p>
                                </div>
                                <button type="submit" id="submit-quiz-btn" class="w-full sm:w-auto bg-dse-red hover:bg-red-800 text-white font-bold py-3 px-8 rounded-xl shadow-lg transition transform hover:-translate-y-0.5">
                                    <i class="fa-solid fa-paper-plane mr-2"></i>提交答案並評分
                                </button>
                            </div>
                        </form>

                        <!-- Score Result Panel -->
                        <div id="result-panel" class="hidden mt-8 bg-white rounded-xl shadow-xl border-2 border-dse-gold p-6">
                            <div class="text-center pb-6 border-b border-slate-200">
                                <span class="text-xs font-bold text-slate-500 tracking-widest uppercase">練習成績報告 Result</span>
                                <div class="flex justify-center items-baseline space-x-2 mt-2">
                                    <span id="final-score" class="text-5xl font-extrabold text-dse-navy">0</span>
                                    <span class="text-xl text-slate-400 font-bold">/ <span id="final-max-score">0</span> 分</span>
                                </div>
                                <div class="mt-3 inline-block px-4 py-1.5 rounded-full text-sm font-bold shadow-sm" id="score-grade-badge">
                                    預測能力等級: Level --
                                </div>
                            </div>

                            <div class="mt-6">
                                <h4 class="font-bold text-slate-800 mb-3"><i class="fa-solid fa-list-check mr-2 text-dse-navy"></i>應試建議與檢討</h4>
                                <p id="score-feedback" class="text-sm text-slate-600 bg-slate-50 p-4 rounded-lg border border-slate-200 leading-relaxed">
                                    --
                                </p>
                            </div>

                            <div class="mt-6 flex justify-end">
                                <button onclick="returnToDashboard()" class="bg-dse-navy hover:bg-slate-800 text-white font-bold py-2.5 px-6 rounded-lg transition shadow">
                                    返回十二篇篇章主頁
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <script>
        // =========================================================================
        // 1. DATABASE: ALL 12 HKDSE PRESCRIBED CLASSICAL TEXTS
        // =========================================================================
        const TWELVE_CLASSICAL_TEXTS = [
            {
                id: "lun_yu",
                title: "《論語》七則",
                author: "孔子及其弟子",
                era: "先秦",
                fullText: [
                    "【1】 子曰：「君子食無求飽，居無求安，敏於事而慎於言，就有道而正焉，可謂好學也已。」",
                    "【2】 子曰：「由！誨女知之乎？知之為知之，不知為不知，是知也。」",
                    "【3】 子曰：「見賢思齊焉，見不賢而內自省也。」",
                    "【4】 子曰：「質勝文則野，文勝質則史。文質彬彬，然後君子。」",
                    "【5】 曾子曰：「士不可以不弘毅，任重而道遠。仁以為己任，不亦重乎？死而後已，不亦遠乎？」",
                    "【6】 子曰：「譬如為山，未成一簶，止，吾止也。譬如平地，雖覆一簶，進，吾往也。」",
                    "【7】 子曰：「知之者不如好之者，好之者不如樂之者。」"
                ],
                questions: [
                    {
                        id: "q_ly_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_ly_1a", prompt: "(1) 就有道而「正」焉。", targetWord: "正", correctAnswer: "糾正 / 框正", keywords: ["糾正", "框正", "改正", "正"], score: 2 },
                            { subId: "q_ly_1b", prompt: "(2) 是「知」也。", targetWord: "知", correctAnswer: "通「智」，智慧", keywords: ["智", "智慧", "明智"], score: 2 }
                        ]
                    },
                    {
                        id: "q_ly_2",
                        type: "mc",
                        title: "2. 在「質勝文則野，文勝質則史」中，孔子認為怎樣才能稱得上「君子」？",
                        score: 2,
                        options: [
                            "A. 樸實勝過文采禮飾",
                            "B. 文采禮飾勝過樸實質樸",
                            "C. 外在文采與內在質樸配合得當（文質彬彬）",
                            "D. 專心學習禮樂知識而不顧外表"
                        ],
                        correctAnswer: "C",
                        explanation: "「文質彬彬，然後君子」指外在文采飾貌與內在質樸品德互相融合調和。"
                    },
                    {
                        id: "q_ly_3",
                        type: "long",
                        title: "3. 曾子曰「士不可以不弘毅，任重而道遠」，試說明「重」與「遠」分別指什麼？（4分）",
                        score: 4,
                        keywords: ["仁", "死", "一生", "責任"],
                        modelAnswer: "「重」指將實現「仁」的道義作為自己的責任（仁以為己任），責任重大；（2分）\n「遠」指這個重任要貫徹終身，直至死亡才停止（死而後已），路途遙遠。（2分）"
                    }
                ]
            },
            {
                id: "yu_wo_suo_yu_ye",
                title: "《魚我所欲也》",
                author: "孟子",
                era: "先秦",
                fullText: [
                    "【1】 魚，我所欲也；熊掌，亦我所欲也。二者不可得兼，舍魚而取熊掌者也。生，亦我所欲也；義，亦我所欲也。二者不可得兼，舍生而取義者也。",
                    "【2】 生亦我所欲，所欲有甚於生者，故不為苟得也；死亦我所惡，所惡有甚於死者，故患有所不辟也。非獨賢者有是心也，人皆有之，賢者能勿喪耳。"
                ],
                questions: [
                    {
                        id: "q_yu_1",
                        type: "mc",
                        title: "1. 本段文字中，孟子以「魚與熊掌」比喻什麼？",
                        score: 2,
                        options: [
                            "A. 物質享受與精神追求",
                            "B. 「生」（生命）與「義」（道義）",
                            "C. 個人利益與國家利益",
                            "D. 貧窮困苦與富貴榮華"
                        ],
                        correctAnswer: "B",
                        explanation: "孟子以魚比喻「生」，熊掌比喻「義」，說明二者不可兼得時應「捨生取義」。"
                    },
                    {
                        id: "q_yu_2",
                        type: "definition",
                        title: "2. 解釋以下劃有下劃線的字詞意思：",
                        score: 2,
                        subQuestions: [
                            { subId: "q_yu_2a", prompt: "故患有所不「辟」也。", targetWord: "辟", correctAnswer: "通「避」，逃避", keywords: ["避", "逃避", "躲避"], score: 2 }
                        ]
                    }
                ]
            },
            {
                id: "xiao_yao_you",
                title: "《逍遙遊》（節錄）",
                author: "莊子",
                era: "先秦",
                fullText: [
                    "【1】 惠子謂莊子曰：「魏王貽我大瓠之種，我樹之成而實五石。以盛水漿，其堅不能自舉也。剖之以為瓢，則瓠落無所容。非不 get（大）也，吾為其無用而掊之。」",
                    "【2】 莊子曰：「夫子固拙於用大矣。宋人有善為不龜手之藥者，世世以洴澼絖為事。客聞之，請買其方百金。聚族而謀曰：『我世世為洴澼絖，不過數金；今一朝而鬻技百金，請與之。』客得之，以說吳王。越有難，吳王使之將。冬，與越人水戰，大敗越人，裂地而封之。能不龜手一也，或以封，或不免於洴澼絖，則所用之異也。今子有五石之瓠，何不慮以為大樽而浮乎江湖，乃患其瓠落無所容？則夫子猶有蓬之心也夫！」"
                ],
                questions: [
                    {
                        id: "q_xy_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_xy_1a", prompt: "(1) 「鬻」技百金。", targetWord: "鬻", correctAnswer: "賣 / 出售", keywords: ["賣", "出售"], score: 2 },
                            { subId: "q_xy_1b", prompt: "(2) 則夫子猶有「蓬」之心也夫！", targetWord: "蓬", correctAnswer: "茅塞 / 不通達", keywords: ["蓬草", "茅塞", "不通達", "受限"], score: 2 }
                        ]
                    },
                    {
                        id: "q_xy_2",
                        type: "mc",
                        title: "2. 莊子舉出「不龜手之藥」的故事，旨在向惠子說明什麼道理？",
                        score: 2,
                        options: [
                            "A. 藥方應世代相傳不應輕易賣給他人",
                            "B. 同一種事物若思想不局限，使用方法不同效果大異",
                            "C. 商業交易時應當精明計算獲利",
                            "D. 吳國能夠擊敗越國全靠不龜手之藥"
                        ],
                        correctAnswer: "B",
                        explanation: "莊子以此說明「所用之異」，批評惠子思路狹隘，受「有用無用」的俗念束縛。"
                    }
                ]
            },
            {
                id: "quan_xue",
                title: "《勸學》（節錄）",
                author: "荀子",
                era: "先秦",
                fullText: [
                    "【1】 君子曰：學不可以已。青，取之於藍，而青於藍；冰，水為之，而寒於水。木直中繩，輮以為輪，其曲中規。",
                    "【2】 登高而招，臂非加長也，而見者遠；順風而呼，聲非加疾也，而聞者彰。假輿馬者，非利足也，而致千里；假舟楫者，非能水也，而絕江河。君子生非異也，善假於物也。"
                ],
                questions: [
                    {
                        id: "q_qx_1",
                        type: "mc",
                        title: "1. 荀子總結「君子生非異也，善假於物也」，其中「物」在全文指什麼？",
                        score: 2,
                        options: [
                            "A. 自然界山川資源",
                            "B. 後天的學習與工具",
                            "C. 身邊的賢能朋友",
                            "D. 豐富的金錢財產"
                        ],
                        correctAnswer: "B",
                        explanation: "「物」比喻後天的學習、教化與客觀條件的借鑒。"
                    }
                ]
            },
            {
                id: "lian_po",
                title: "《廉頗藺相如列傳》（節錄）",
                author: "司馬遷",
                era: "西漢",
                fullText: [
                    "【1】 廉頗曰：「我為趙將，有攻城野戰之大功，而藺相如徒以口舌為勞，而位居我上！且相如素賤人，吾羞，不忍為之下！」宣告曰：「我見相如，必辱之！」",
                    "【2】 相如聞，不肯與會。相如曰：「顧吾念之，強秦之所以不敢加兵於趙者，徒以吾兩人在也。今兩虎共鬥，其勢不俱生。吾所以為此者，以先國家之急而後私仇也！」廉頗聞之，肉袒負荊謝罪。"
                ],
                questions: [
                    {
                        id: "q_lp_1",
                        type: "mc",
                        title: "1. 藺相如面對廉頗的挑釁退讓避匿，其根本原因是什麼？",
                        score: 2,
                        options: [
                            "A. 畏懼廉頗身為大將軍的武力",
                            "B. 以國家安全利益為先，避免將相不和讓秦國有機可乘",
                            "C. 欲藉此向趙王控訴廉頗霸道",
                            "D. 聽從門客建議故意示弱"
                        ],
                        correctAnswer: "B",
                        explanation: "「先國家之急而後私仇也」，藺相如以國事大局為重。"
                    }
                ]
            },
            {
                id: "chu_shi_biao",
                title: "《出師表》",
                author: "諸葛亮",
                era: "三國",
                fullText: [
                    "【1】 先帝創業未半而中道崩殂，今天下三分，益州疲弊，此誠危急存亡之秋也。然侍衛之臣不懈於內，忠志之士忘身於外者，蓋追先帝之殊遇，欲報之於陛下也。",
                    "【2】 親賢臣，遠小人，此先漢之所以興隆也；親小人，遠賢臣，此後漢之所以傾頹也。"
                ],
                questions: [
                    {
                        id: "q_csb_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 2,
                        subQuestions: [
                            { subId: "q_csb_1a", prompt: "此誠危急存亡之「秋」也。", targetWord: "秋", correctAnswer: "關鍵時刻 / 時期", keywords: ["時刻", "時期", "關頭"], score: 2 }
                        ]
                    }
                ]
            },
            {
                id: "shi_shuo",
                title: "《師說》",
                author: "韓愈",
                era: "唐代",
                fullText: [
                    "【1】 古之學者必有師。師者，所以傳道受業解惑也。人非生而知之者，孰能無惑？惑而不從師，其為惑也，終不解矣。",
                    "【2】 聖人無常師。孔子師郯子、訐謨、師襄、老聃。郯子之徒，其賢不及孔子。孔子曰：三人行，則必有我師。是故弟子不必不如師，師不必賢於弟子，聞道有先後，術業有專攻，如是而已。"
                ],
                questions: [
                    {
                        id: "q_ss_1",
                        type: "mc",
                        title: "1. 韓愈認為擇師的標準是什麼？",
                        score: 2,
                        options: [
                            "A. 老師的社會地位與財富",
                            "B. 老師的年紀是否比自己大",
                            "C. 誰先領悟道理或具備專長（「道之所存，師之所存」）",
                            "D. 必須是朝廷認可的官學名師"
                        ],
                        correctAnswer: "C",
                        explanation: "韓愈強調「無貴無賤，無長無少，道之所存，師之所存也」。"
                    }
                ]
            },
            {
                id: "xi_shan",
                title: "《始得西山宴遊記》",
                author: "柳宗元",
                era: "唐代",
                fullText: [
                    "【1】 自余為僇人，居是州，恆惴慄。其隙也，則施施而行，漫漫而遊。",
                    "【2】 心凝形釋，與萬化冥合。然後知吾嚮之未始遊，遊於是乎始，故題之曰《始得西山宴遊記》。"
                ],
                questions: [
                    {
                        id: "q_xs_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 2,
                        subQuestions: [
                            { subId: "q_xs_1a", prompt: "自余為「僇人」。", targetWord: "僇人", correctAnswer: "受辱有罪的人（受貶官員）", keywords: ["有罪", "受辱", "貶", "罪人"], score: 2 }
                        ]
                    }
                ]
            },
            {
                id: "yue_yang_lou",
                title: "《岳陽樓記》",
                author: "范仲淹",
                era: "宋代",
                fullText: [
                    "【1】 予觀夫巴陵勝狀，在洞庭一湖。銜遠山，吞長江，浩浩湯湯，橫無際涯；朝暉夕陰，氣象萬千。此則岳陽樓之大觀也。",
                    "【2】 不以物喜，不以己悲。居廟堂之高則憂其民，處江湖之遠則憂其君。是進亦憂，退亦憂。然則何時而樂耶？其必曰「先天下之憂而憂，後天下之樂而樂」乎！"
                ],
                questions: [
                    {
                        id: "q_yyl_1",
                        type: "mc",
                        title: "1. 范仲淹提出「不以物喜，不以己悲」，體現了怎樣的情懷？",
                        score: 2,
                        options: [
                            "A. 消極避世，歸隱田園",
                            "B. 不因外界環境或個人榮辱而動搖信念的高尚情操",
                            "C. 追求名利富貴的熱切心態",
                            "D. 對政治失意產生的怨恨情緒"
                        ],
                        correctAnswer: "B",
                        explanation: "「不以物喜，不以己悲」是古仁人之心，超脫個人榮辱患得患失。"
                    }
                ]
            },
            {
                id: "zui_weng_ting",
                title: "《醉翁亭記》",
                author: "歐陽修",
                era: "宋代",
                fullText: [
                    "【1】 環滁皆山也。其西南諸峰，林壑尤美，望之蔚然而深秀者，琅琊也。山行六七里，漸聞水聲潺潺而瀉出於兩峰之間者，釀泉也。峰回路轉，有亭翼然臨於泉上者，醉翁亭也。",
                    "【2】 醉翁之意不在酒，在乎山水之間也。山水之樂，得之心而寓之酒也。"
                ],
                questions: [
                    {
                        id: "q_zwt_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 2,
                        subQuestions: [
                            { subId: "q_zwt_1a", prompt: "醉翁之「意」不在酒。", targetWord: "意", correctAnswer: "情趣 / 心意 / 意旨", keywords: ["情趣", "心意", "意旨", "意圖"], score: 2 }
                        ]
                    }
                ]
            },
            {
                id: "tang_shi",
                title: "唐詩三首",
                author: "李白、杜甫",
                era: "唐代",
                fullText: [
                    "【1】《蜀道難》（節錄）：噫吁戲，危乎高哉！蜀道之難，難於上青天！",
                    "【2】《登高》：風急天高猿嘯哀，渚清沙白鳥飛回。無邊落木蕭蕭下，不盡長江滾滾來。萬里悲秋常作客，百年多病獨登台。艱難苦恨繁霜鬢，潦倒新停濁酒杯。",
                    "【3】《月下獨酌》：花間一壺酒，獨酌無相親。舉杯邀明月，對影成三人。"
                ],
                questions: [
                    {
                        id: "q_ts_1",
                        type: "mc",
                        title: "1. 杜甫《登高》中「萬里悲秋常作客，百年多病獨登台」表達了什麼情感？",
                        score: 2,
                        options: [
                            "A. 喜悅於秋天雄渾壯麗的景象",
                            "B. 身處異鄉漂泊、年老多病與國破家亡的深沉悲苦",
                            "C. 隱居山林的悠閒自得",
                            "D. 對好友登高重聚的期待"
                        ],
                        correctAnswer: "B",
                        explanation: "集「常作客（漂泊）」、「多病」、「獨登台（孤單）」於一身，極寫悲苦。"
                    }
                ]
            },
            {
                id: "song_ci",
                title: "宋詞三首",
                author: "蘇軾、李清照、辛棄疾",
                era: "宋代",
                fullText: [
                    "【1】《念奴嬌·赤壁懷古》：大江東去，浪淘盡，千古風流人物。故壘西邊，人道是，三國周郎赤壁。",
                    "【2】《聲聲慢·尋尋覓覓》：尋尋覓覓，冷冷清清，悽悽慘慘戚戚。乍暖還寒時候，最難將息。",
                    "【3】《青玉案·元夕》：眾裏尋他千百度。驀然回首，那人卻在，燈火闌珊處。"
                ],
                questions: [
                    {
                        id: "q_sc_1",
                        type: "mc",
                        title: "1. 辛棄疾《青玉案·元夕》中「燈火闌珊處」的「那人」象徵什麼？",
                        score: 2,
                        options: [
                            "A. 元宵節喧鬧歡樂的人群",
                            "B. 詞人孤高自守、不隨波逐流的高潔人格",
                            "C. 已經拋棄詞人的昔日情人",
                            "D. 朝廷中討好迎合的庸俗官員"
                        ],
                        correctAnswer: "B",
                        explanation: "「那人」在燈火稀疏處孤獨自處，寄託詞人自甘寂寞、堅持忠貞人格的形象。"
                    }
                ]
            }
        ];

        // =========================================================================
        // 2. STATE MANAGEMENT & MANDATORY LOGIN ENFORCEMENT
        // =========================================================================
        let currentUser = null;
        let activePassage = null;

        window.addEventListener('DOMContentLoaded', () => {
            enforceAuthSession();
        });

        function enforceAuthSession() {
            const storedUser = localStorage.getItem('dse_shaba_user');
            if (storedUser) {
                currentUser = JSON.parse(storedUser);
                unlockAppPortal();
            } else {
                lockAppPortal();
            }
        }

        function lockAppPortal() {
            document.getElementById('login-gate').classList.remove('hidden');
            document.getElementById('main-app-container').classList.add('hidden');
        }

        function unlockAppPortal() {
            document.getElementById('login-gate').classList.add('hidden');
            document.getElementById('main-app-container').classList.remove('hidden');
            document.getElementById('nav-user-name').innerText = currentUser.name;
            renderDashboardStats();
            renderPassageCards();
        }

        function switchGateTab(tab) {
            const isLogin = tab === 'login';
            document.getElementById('gate-tab-login').className = isLogin 
                ? 'w-1/2 py-2 text-center font-bold text-dse-navy border-b-2 border-dse-navy' 
                : 'w-1/2 py-2 text-center font-bold text-slate-400 border-b-2 border-transparent';
            document.getElementById('gate-tab-register').className = !isLogin 
                ? 'w-1/2 py-2 text-center font-bold text-dse-navy border-b-2 border-dse-navy' 
                : 'w-1/2 py-2 text-center font-bold text-slate-400 border-b-2 border-transparent';
            
            document.getElementById('gate-confirm-pw-container').classList.toggle('hidden', isLogin);
            document.getElementById('gate-submit-btn').innerText = isLogin ? '登入平台開始練習' : '創建帳號並登入';
            document.getElementById('gate-auth-error').classList.add('hidden');
        }

        function handleGateAuth(e) {
            e.preventDefault();
            const username = document.getElementById('gate-username').value.trim();
            const password = document.getElementById('gate-password').value;
            const isRegister = !document.getElementById('gate-confirm-pw-container').classList.contains('hidden');
            const errorEl = document.getElementById('gate-auth-error');

            if (!username || !password) {
                errorEl.innerText = '請填寫用戶名稱與密碼！';
                errorEl.classList.remove('hidden');
                return;
            }

            const usersDb = JSON.parse(localStorage.getItem('dse_users_db') || '{}');

            if (isRegister) {
                const confirmPassword = document.getElementById('gate-confirm-password').value;
                if (password !== confirmPassword) {
                    errorEl.innerText = '兩次密碼不一致！';
                    errorEl.classList.remove('hidden');
                    return;
                }
                if (usersDb[username]) {
                    errorEl.innerText = '該用戶名已被註冊！';
                    errorEl.classList.remove('hidden');
                    return;
                }

                usersDb[username] = { password: password, history: [] };
                localStorage.setItem('dse_users_db', JSON.stringify(usersDb));
                currentUser = { name: username, history: [] };
            } else {
                if (!usersDb[username] || usersDb[username].password !== password) {
                    errorEl.innerText = '用戶名或密碼無效！';
                    errorEl.classList.remove('hidden');
                    return;
                }
                currentUser = { name: username, history: usersDb[username].history || [] };
            }

            localStorage.setItem('dse_shaba_user', JSON.stringify(currentUser));
            unlockAppPortal();
        }

        function logout() {
            currentUser = null;
            localStorage.removeItem('dse_shaba_user');
            lockAppPortal();
        }

        // =========================================================================
        // 3. STATS & EVALUATION ENGINE
        // =========================================================================
        function renderDashboardStats() {
            if (!currentUser || !currentUser.history) return;

            const history = currentUser.history;
            const totalCount = history.length;
            document.getElementById('stat-completed').innerText = totalCount;

            if (totalCount === 0) {
                document.getElementById('stat-avg-score').innerText = "0%";
                document.getElementById('stat-predicted-level').innerText = "Level --";
                document.getElementById('stat-last-active').innerText = "無記錄";
                return;
            }

            const totalPercentage = history.reduce((acc, item) => acc + (item.score / item.maxScore), 0);
            const avgPercentage = Math.round((totalPercentage / totalCount) * 100);
            document.getElementById('stat-avg-score').innerText = `${avgPercentage}%`;

            let predictedLevel = "Level 1";
            if (avgPercentage >= 85) predictedLevel = "Level 5**";
            else if (avgPercentage >= 75) predictedLevel = "Level 5*";
            else if (avgPercentage >= 68) predictedLevel = "Level 5";
            else if (avgPercentage >= 58) predictedLevel = "Level 4";
            else if (avgPercentage >= 48) predictedLevel = "Level 3";
            else if (avgPercentage >= 38) predictedLevel = "Level 2";

            document.getElementById('stat-predicted-level').innerText = predictedLevel;

            const lastItem = history[history.length - 1];
            document.getElementById('stat-last-active').innerText = `${lastItem.date.split(' ')[0]}`;
        }

        // =========================================================================
        // 4. RENDERING CARDS & EXAM INTERFACE
        // =========================================================================
        function renderPassageCards() {
            const container = document.getElementById('passage-cards-container');
            container.innerHTML = TWELVE_CLASSICAL_TEXTS.map((passage, index) => {
                const totalScore = passage.questions.reduce((sum, q) => sum + q.score, 0);
                return `
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 hover:shadow-md transition p-5 flex flex-col justify-between">
                        <div>
                            <div class="flex justify-between items-start mb-2">
                                <span class="text-xs font-bold bg-blue-50 text-blue-800 border border-blue-200 px-2 py-0.5 rounded">${passage.era}</span>
                                <span class="text-xs font-bold text-dse-navy bg-slate-100 px-2 py-0.5 rounded">篇章 #${index + 1}</span>
                            </div>
                            <h3 class="text-xl font-bold font-serif text-dse-navy mb-1">${passage.title}</h3>
                            <p class="text-xs font-medium text-slate-500 mb-3">作者：${passage.author}</p>
                            <p class="text-xs text-slate-600 line-clamp-2 bg-slate-50 p-2.5 rounded border border-slate-100 classical-text mb-4">
                                ${passage.fullText[0]}
                            </p>
                        </div>
                        <button onclick="startQuiz('${passage.id}')" class="w-full bg-dse-navy hover:bg-slate-800 text-white font-bold py-2 px-3 rounded-lg shadow text-xs sm:text-sm transition flex items-center justify-center">
                            <i class="fa-solid fa-pen-to-square mr-2"></i>進入試題練習 (${totalScore}分)
                        </button>
                    </div>
                `;
            }).join('');
        }

        function startQuiz(passageId) {
            activePassage = TWELVE_CLASSICAL_TEXTS.find(p => p.id === passageId);
            if (!activePassage) return;

            document.getElementById('dashboard-view').classList.add('hidden');
            document.getElementById('quiz-view').classList.remove('hidden');
            document.getElementById('result-panel').classList.add('hidden');
            document.getElementById('submit-quiz-btn').disabled = false;
            document.getElementById('submit-quiz-btn').classList.remove('opacity-50', 'cursor-not-allowed');

            document.getElementById('passage-title').innerText = activePassage.title;
            document.getElementById('passage-author').innerText = activePassage.author;
            document.getElementById('quiz-title-badge').innerText = `練習篇章：${activePassage.title}`;
            
            const totalScore = activePassage.questions.reduce((sum, q) => sum + q.score, 0);
            document.getElementById('passage-max-score').innerText = totalScore;

            const passageContent = document.getElementById('passage-content');
            passageContent.innerHTML = activePassage.fullText.map(para => `<p>${para}</p>`).join('');

            renderQuestions(activePassage.questions);
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function renderQuestions(questions) {
            const container = document.getElementById('questions-container');
            container.innerHTML = questions.map((q) => {
                if (q.type === 'mc') {
                    return `
                        <div class="question-block bg-white p-5 rounded-xl border border-slate-200 shadow-sm" id="card-${q.id}">
                            <div class="flex justify-between items-start mb-3">
                                <span class="font-bold text-slate-800 text-sm sm:text-base">${q.title}</span>
                                <span class="text-xs bg-slate-100 font-bold text-slate-600 px-2 py-1 rounded ml-2 whitespace-nowrap">${q.score}分</span>
                            </div>
                            <div class="space-y-2 mt-3">
                                ${q.options.map((opt, optIdx) => {
                                    const optVal = String.fromCharCode(65 + optIdx);
                                    return `
                                        <label class="flex items-center p-3 rounded-lg border border-slate-200 hover:bg-blue-50/50 cursor-pointer transition">
                                            <input type="radio" name="${q.id}" value="${optVal}" class="w-4 h-4 text-dse-navy focus:ring-dse-navy">
                                            <span class="ml-3 text-sm text-slate-700 font-medium">${opt}</span>
                                        </label>
                                    `;
                                }).join('')}
                            </div>
                            <div class="feedback-box hidden mt-4 p-4 rounded-lg text-sm"></div>
                        </div>
                    `;
                } else if (q.type === 'definition') {
                    return `
                        <div class="question-block bg-white p-5 rounded-xl border border-slate-200 shadow-sm" id="card-${q.id}">
                            <div class="flex justify-between items-start mb-3">
                                <span class="font-bold text-slate-800 text-sm sm:text-base">${q.title}</span>
                                <span class="text-xs bg-slate-100 font-bold text-slate-600 px-2 py-1 rounded ml-2 whitespace-nowrap">${q.score}分</span>
                            </div>
                            <div class="space-y-3 mt-3">
                                ${q.subQuestions.map(sub => `
                                    <div class="bg-slate-50 p-3 rounded-lg border border-slate-200">
                                        <label class="block text-xs sm:text-sm font-bold text-slate-800 mb-1.5">${sub.prompt}</label>
                                        <div class="flex items-center space-x-2">
                                            <span class="text-xs sm:text-sm font-bold text-dse-navy font-serif whitespace-nowrap">「${sub.targetWord}」解作：</span>
                                            <input type="text" name="${sub.subId}" placeholder="請輸入字詞解釋..." class="w-full px-3 py-1.5 border border-slate-300 rounded focus:ring-2 focus:ring-dse-navy focus:outline-none text-xs sm:text-sm">
                                        </div>
                                    </div>
                                `).join('')}
                            </div>
                            <div class="feedback-box hidden mt-4 p-4 rounded-lg text-sm"></div>
                        </div>
                    `;
                } else if (q.type === 'long') {
                    return `
                        <div class="question-block bg-white p-5 rounded-xl border border-slate-200 shadow-sm" id="card-${q.id}">
                            <div class="flex justify-between items-start mb-3">
                                <span class="font-bold text-slate-800 text-sm sm:text-base">${q.title}</span>
                                <span class="text-xs bg-slate-100 font-bold text-slate-600 px-2 py-1 rounded ml-2 whitespace-nowrap">${q.score}分</span>
                            </div>
                            <div class="mt-3">
                                <textarea name="${q.id}" rows="4" placeholder="請在此輸入長答作答內容..." class="w-full p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-dse-navy focus:outline-none text-xs sm:text-sm leading-relaxed"></textarea>
                            </div>
                            <div class="feedback-box hidden mt-4 p-4 rounded-lg text-sm"></div>
                        </div>
                    `;
                }
            }).join('');
        }

        // =========================================================================
        // 5. SUBMISSION & EVALUATION
        // =========================================================================
        function submitQuiz() {
            if (!activePassage) return;

            let earnedScore = 0;
            let totalPossibleScore = 0;

            activePassage.questions.forEach(q => {
                totalPossibleScore += q.score;
                const cardEl = document.getElementById(`card-${q.id}`);
                const feedbackEl = cardEl.querySelector('.feedback-box');
                feedbackEl.classList.remove('hidden', 'bg-emerald-50', 'bg-red-50', 'text-emerald-800', 'text-red-800', 'border-emerald-200', 'border-red-200', 'border');

                if (q.type === 'mc') {
                    const selected = document.querySelector(`input[name="${q.id}"]:checked`);
                    const userAns = selected ? selected.value : null;

                    if (userAns === q.correctAnswer) {
                        earnedScore += q.score;
                        feedbackEl.classList.add('bg-emerald-50', 'text-emerald-800', 'border', 'border-emerald-200');
                        feedbackEl.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-600 mr-2"></i><strong>答案正確 (+${q.score}分)</strong><br><span class="text-xs mt-1 block">${q.explanation}</span>`;
                    } else {
                        feedbackEl.classList.add('bg-red-50', 'text-red-800', 'border', 'border-red-200');
                        feedbackEl.innerHTML = `<i class="fa-solid fa-circle-xmark text-red-600 mr-2"></i><strong>答案錯誤 (+0分)</strong><br><span class="text-xs mt-1 block">正確答案為 <strong>${q.correctAnswer}</strong>。${q.explanation}</span>`;
                    }
                } else if (q.type === 'definition') {
                    let subScoreTotal = 0;
                    let feedbackHtml = '<div class="space-y-1 text-xs sm:text-sm">';

                    q.subQuestions.forEach(sub => {
                        const inputEl = document.querySelector(`input[name="${sub.subId}"]`);
                        const val = inputEl ? inputEl.value.trim() : '';
                        const isMatch = sub.keywords.some(kw => val.includes(kw));

                        if (isMatch) {
                            subScoreTotal += sub.score;
                            feedbackHtml += `<p class="text-emerald-700"><i class="fa-solid fa-check text-xs mr-1"></i>${sub.prompt}: 正確 (+${sub.score}分)</p>`;
                        } else {
                            feedbackHtml += `<p class="text-red-700"><i class="fa-solid fa-xmark text-xs mr-1"></i>${sub.prompt}: 參考答案為【<strong>${sub.correctAnswer}</strong>】</p>`;
                        }
                    });

                    feedbackHtml += '</div>';
                    earnedScore += subScoreTotal;

                    feedbackEl.classList.add(subScoreTotal === q.score ? 'bg-emerald-50' : 'bg-red-50', 'border', 'border-slate-200');
                    feedbackEl.innerHTML = feedbackHtml;
                } else if (q.type === 'long') {
                    const textEl = document.querySelector(`textarea[name="${q.id}"]`);
                    const val = textEl ? textEl.value.trim() : '';

                    let matchedKeywords = 0;
                    q.keywords.forEach(kw => {
                        if (val.includes(kw)) matchedKeywords++;
                    });

                    let estimatedLongScore = 0;
                    if (val.length > 5) {
                        if (matchedKeywords >= 3) estimatedLongScore = q.score;
                        else if (matchedKeywords >= 2) estimatedLongScore = Math.ceil(q.score * 0.75);
                        else if (matchedKeywords >= 1) estimatedLongScore = Math.floor(q.score * 0.5);
                        else estimatedLongScore = 1;
                    }

                    earnedScore += estimatedLongScore;

                    feedbackEl.classList.add('bg-blue-50', 'text-slate-700', 'border', 'border-blue-200');
                    feedbackEl.innerHTML = `
                        <div class="font-bold text-dse-navy mb-1 flex justify-between text-xs sm:text-sm">
                            <span><i class="fa-solid fa-scale-balanced mr-1"></i>系統評估得分：${estimatedLongScore} / ${q.score} 分</span>
                        </div>
                        <div class="text-xs bg-white p-3 rounded border border-blue-100 whitespace-pre-line text-slate-600">
                            <strong>【考評局示範答案與評分指引】：</strong>\n${q.modelAnswer}
                        </div>
                    `;
                }
            });

            document.getElementById('final-score').innerText = earnedScore;
            document.getElementById('final-max-score').innerText = totalPossibleScore;

            const scorePercent = Math.round((earnedScore / totalPossibleScore) * 100);
            const badgeEl = document.getElementById('score-grade-badge');
            const feedbackTextEl = document.getElementById('score-feedback');

            let gradeText = "";
            let feedbackMsg = "";

            if (scorePercent >= 85) {
                gradeText = "預測能力等級: Level 5**";
                badgeEl.className = "mt-3 inline-block px-4 py-1.5 rounded-full text-sm font-bold shadow-sm bg-amber-100 text-amber-800 border border-amber-300";
                feedbackMsg = "頂尖表現！文言詞義理解極為精準，完全具備 DSE 中文科 5** 水平。";
            } else if (scorePercent >= 70) {
                gradeText = "預測能力等級: Level 5 / 5*";
                badgeEl.className = "mt-3 inline-block px-4 py-1.5 rounded-full text-sm font-bold shadow-sm bg-emerald-100 text-emerald-800 border border-emerald-300";
                feedbackMsg = "良好表現！文言基礎紮實，建議多加注意長答題關鍵字的精準表述。";
            } else if (scorePercent >= 50) {
                gradeText = "預測能力等級: Level 3 / 4";
                badgeEl.className = "mt-3 inline-block px-4 py-1.5 rounded-full text-sm font-bold shadow-sm bg-blue-100 text-blue-800 border border-blue-300";
                feedbackMsg = "達標成績！建議加強熟讀範文原文與實詞虛詞含意。";
            } else {
                gradeText = "預測能力等級: Level 1 / 2";
                badgeEl.className = "mt-3 inline-block px-4 py-1.5 rounded-full text-sm font-bold shadow-sm bg-red-100 text-red-800 border border-red-300";
                feedbackMsg = "仍有進步空間！請詳閱課文註釋並多做試題練習。";
            }

            badgeEl.innerText = gradeText;
            feedbackTextEl.innerText = feedbackMsg;

            document.getElementById('result-panel').classList.remove('hidden');
            document.getElementById('submit-quiz-btn').disabled = true;
            document.getElementById('submit-quiz-btn').classList.add('opacity-50', 'cursor-not-allowed');

            if (currentUser) {
                const newRecord = {
                    passageTitle: activePassage.title,
                    score: earnedScore,
                    maxScore: totalPossibleScore,
                    date: new Date().toLocaleString('zh-HK')
                };

                currentUser.history.push(newRecord);
                localStorage.setItem('dse_shaba_user', JSON.stringify(currentUser));

                const usersDb = JSON.parse(localStorage.getItem('dse_users_db') || '{}');
                if (usersDb[currentUser.name]) {
                    usersDb[currentUser.name].history = currentUser.history;
                    localStorage.setItem('dse_users_db', JSON.stringify(usersDb));
                }
            }

            document.getElementById('result-panel').scrollIntoView({ behavior: 'smooth' });
        }

        function returnToDashboard() {
            document.getElementById('quiz-view').classList.add('hidden');
            document.getElementById('dashboard-view').classList.remove('hidden');
            activePassage = null;
            if (currentUser) {
                renderDashboardStats();
            }
        }
    </script>
</body>
</html>
