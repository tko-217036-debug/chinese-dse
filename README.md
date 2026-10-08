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

    <!-- MANDATORY LOGIN OVERLAY GATEWAY -->
    <div id="login-gate" class="fixed inset-0 z-50 bg-slate-900/90 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-2xl shadow-2xl border border-slate-100 overflow-hidden transform transition-all">
            <div class="bg-dse-navy text-white p-6 text-center relative">
                <span class="bg-dse-gold text-dse-navy font-black text-xs px-3 py-1 rounded-full uppercase tracking-wider mb-2 inline-block">HKDSE 中文卷一專攻</span>
                <h2 class="text-2xl font-black tracking-wide">傻B - HKDSE 中文科</h2>
                <p class="text-sm font-medium text-amber-300 mt-1">十二篇範文問答平台 (全篇章特大題庫版)</p>
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
                        登入平台開始解題
                    </button>
                </form>

                <p class="text-xs text-center text-slate-400 mt-4">
                    <i class="fa-solid fa-shield-halved mr-1"></i>登入後作答數據將自動儲存於此裝置 (localStorage)
                </p>
            </div>
        </div>
    </div>

    <!-- MAIN APPLICATION CONTAINER -->
    <div id="main-app-container" class="hidden min-h-screen flex flex-col">
        <!-- Header -->
        <header class="bg-dse-navy text-white shadow-md sticky top-0 z-40">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex justify-between items-center">
                <div class="flex items-center space-x-3">
                    <span class="bg-dse-gold text-dse-navy font-black text-xl px-2.5 py-1 rounded shadow">傻B</span>
                    <div>
                        <h1 class="font-bold text-lg leading-tight tracking-wide">HKDSE 中文科十二篇範文問答平台</h1>
                        <p class="text-xs text-slate-300 hidden sm:block">指定文言經典篇章特大題庫研習 Portal</p>
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
                        <h2 class="text-2xl font-bold text-slate-800">十二篇指定文言經典篇章 (每篇豐富題庫)</h2>
                        <p class="text-sm text-slate-500">點擊任意篇章解鎖多重選擇題、考評局字詞釋義及高分長問答指引</p>
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
        // EXPANDED QUESTION DATABASE FOR ALL 12 PRESCRIBED CLASSICAL TEXTS
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
                        score: 6,
                        subQuestions: [
                            { subId: "q_ly_1a", prompt: "(1) 就有道而「正」焉。", targetWord: "正", correctAnswer: "糾正 / 框正", keywords: ["糾正", "框正", "改正", "正"], score: 2 },
                            { subId: "q_ly_1b", prompt: "(2) 是「知」也。", targetWord: "知", correctAnswer: "通「智」，智慧", keywords: ["智", "智慧", "明智"], score: 2 },
                            { subId: "q_ly_1c", prompt: "(3) 敏於事而「慎」於言。", targetWord: "慎", correctAnswer: "謹慎", keywords: ["謹慎", "小心"], score: 2 }
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
                        type: "mc",
                        title: "3. 孔子以「為山」與「平地」作比喻，主要想強調什麼道理？",
                        score: 2,
                        options: [
                            "A. 學習應量力而為",
                            "B. 學習成敗完全取決於個人的堅持與否",
                            "C. 堆土造山需要依靠團隊合作",
                            "D. 積少成多需要長期累積"
                        ],
                        correctAnswer: "B",
                        explanation: "「止，吾止也；進，吾往也」，強調學習的主細權完全在於自己是否堅持。"
                    },
                    {
                        id: "q_ly_4",
                        type: "long",
                        title: "4. 曾子曰「士不可以不弘毅，任重而道遠」，試說明「重」與「遠」分別指什麼？（4分）",
                        score: 4,
                        keywords: ["仁", "死", "一生", "責任"],
                        modelAnswer: "「重」指將實現「仁」的道義作為自己的責任（仁以為己任），責任重大；（2分）\n「遠」指這個重任要貫徹終身，直至死亡才停止（死而後已），路途遙遠。（2分）"
                    },
                    {
                        id: "q_ly_5",
                        type: "long",
                        title: "5. 孔子認為對待知識的「知之」、「好之」、「樂之」三種境界有何分別？試以個人理解說明。（4分）",
                        score: 4,
                        keywords: ["知之", "好之", "樂之", "興趣", "快樂"],
                        modelAnswer: "「知之者」只停留於客觀了解知識；（1分）\n「好之者」出自個人喜好主動去追求學習；（1分）\n「樂之者」能將學習融入心靈，以學習本身為最大樂趣與享受，屬最高境界。（2分）"
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
                    "【2】 生亦我所欲，所欲有甚於生者，故不為苟得也；死亦我所惡，所惡有甚於死者，故患有所不辟也。非獨賢者有是心也，人皆有之，賢者能勿喪耳。",
                    "【3】 一單食，一豆羹，得之則生，弗得則死。呼爾而與之，行道之人弗受；蹴爾而與之，乞人不屑也。萬鍾則不辯禮義而受之，萬鍾於我何加焉！"
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
                        score: 4,
                        subQuestions: [
                            { subId: "q_yu_2a", prompt: "(1) 故患有所不「辟」也。", targetWord: "辟", correctAnswer: "通「避」，逃避", keywords: ["避", "逃避", "躲避"], score: 2 },
                            { subId: "q_yu_2b", prompt: "(2) 萬鍾於我何「加」焉！", targetWord: "加", correctAnswer: "益處 / 增加", keywords: ["益處", "好處", "增加"], score: 2 }
                        ]
                    },
                    {
                        id: "q_yu_3",
                        type: "mc",
                        title: "3. 孟子舉出「行道之人」和「乞人」拒受「呼爾蹴爾」之食的例子，旨在說明什麼？",
                        score: 2,
                        options: [
                            "A. 貧窮者往往缺乏修養",
                            "B. 每個人天生皆有羞惡之心與尊嚴（羞惡之心，人皆有之）",
                            "C. 施捨者應態度和藹",
                            "D. 食物品質低下不值得接受"
                        ],
                        correctAnswer: "B",
                        explanation: "即使是飢餓至死的平民或乞丐，亦不肯接受侮辱性的施捨，證明人人皆有本心（羞惡之心）。"
                    },
                    {
                        id: "q_yu_4",
                        type: "long",
                        title: "4. 孟子認為為什麼有些人最終會「受萬鍾而不辯禮義」？試綜合全文解釋。（4分）",
                        score: 4,
                        keywords: ["本心", "喪失", "宮室", "妻妾", "奉養"],
                        modelAnswer: "因為他們為了追求優裕的物質生活（如宮室之美、妻妾之奉、所識貧者得我之感激），（2分）受名利物慾誘惑而「失其本心」（喪失了羞惡之心）。（2分）"
                    }
                ]
            },
            {
                id: "xiao_yao_you",
                title: "《逍遙遊》（節錄）",
                author: "莊子",
                era: "先秦",
                fullText: [
                    "【1】 惠子謂莊子曰：「魏王貽我大瓠之種，我樹之成而實五石。以盛水漿，其堅不能自舉也。剖之以為瓢，則瓠落無所容。非不大也，吾為其無用而掊之。」",
                    "【2】 莊子曰：「夫子固拙於用大矣。宋人有善為不龜手之藥者，世世以洴澼絖為事。客聞之，請買其方百金。聚族而謀曰：『我世世為洴澼絖，不過數金；今一朝而鬻技百金，請與之。』客得之，以說吳王。越有難，吳王使之將。冬，與越人水戰，大敗越人，裂地而封之。能不龟手一也，或以封，或不免於洴澼絖，則所用之異也。今子有五石之瓠，何不慮以為大樽而浮乎江湖，乃患其瓠落無所容？則夫子猶有蓬之心也夫！」"
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
                    },
                    {
                        id: "q_xy_3",
                        type: "mc",
                        title: "3. 莊子建議將五石之大瓠如何使用？",
                        score: 2,
                        options: [
                            "A. 剖開做成水瓢盛水",
                            "B. 做成腰舟（大樽）繫在身上浮游於江湖",
                            "C. 賣給客商換取百金",
                            "D. 用於釀造上等好酒"
                        ],
                        correctAnswer: "B",
                        explanation: "莊子主張「慮以為大樽而浮乎江湖」，突破實用主義的拘束。"
                    },
                    {
                        id: "q_xy_4",
                        type: "long",
                        title: "4. 試分析惠子與莊子在「大瓠」一事上所展現的觀點有何根本分歧？（4分）",
                        score: 4,
                        keywords: ["惠子", "世俗", "實用", "莊子", "超脫", "自由"],
                        modelAnswer: "惠子著重於世俗的實用價值，認為大瓠既不能盛水又不能作瓢即為無用；（2分）\n莊子則突破世俗實用的框架，主張超脫世俗限制，順應自然以達到心靈逍遙自由的境界。（2分）"
                    }
                ]
            },
            {
                id: "quan_xue",
                title: "《勸學》（節錄）",
                author: "荀子",
                era: "先秦",
                fullText: [
                    "【1】 君子曰：學不可以已。青，取之於藍，而青於藍；冰，水為之，而寒於水。木直中繩，輮以為輪，其曲中規。雖有槁暴，不復挺者，輮使之然也。故木受繩則直，金就礪則利，君子博學而日參省乎己，則知明而行無過矣。",
                    "【2】 吾嘗終日而思矣，不如須臾之所學也；吾嘗跂而望矣，不如登高之博見也。登高而招，臂非加長也，而見者遠；順風而呼，聲非加疾也，而聞者彰。假輿馬者，非利足也，而致千里；假舟楫者，非能水也，而絕江河。君子生非異也，善假於物也。"
                ],
                questions: [
                    {
                        id: "q_qx_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_qx_1a", prompt: "(1) 學不可以「已」。", targetWord: "已", correctAnswer: "停止", keywords: ["停止", "止"], score: 2 },
                            { subId: "q_qx_1b", prompt: "(2) 金就「礪」則利。", targetWord: "礪", correctAnswer: "磨刀石", keywords: ["磨刀石", "磨石"], score: 2 }
                        ]
                    },
                    {
                        id: "q_qx_2",
                        type: "mc",
                        title: "2. 荀子總結「君子生非異也，善假於物也」，其中「物」在全文指什麼？",
                        score: 2,
                        options: [
                            "A. 自然界山川資源",
                            "B. 後天的學習與工具條件",
                            "C. 身邊的賢能朋友",
                            "D. 豐富的金錢財產"
                        ],
                        correctAnswer: "B",
                        explanation: "「物」比喻後天所借憑的學習、教化與客觀條件。"
                    },
                    {
                        id: "q_qx_3",
                        type: "mc",
                        title: "3. 「青取之於藍而青於藍，冰水為之而寒於水」這兩個比喻旨在說明什麼？",
                        score: 2,
                        options: [
                            "A. 後天學習能使人超越原本的資質與基礎",
                            "B. 大自然產物比人工加工更優勝",
                            "C. 學習過程非常艱苦",
                            "D. 老師的水平一定比學生高"
                        ],
                        correctAnswer: "A",
                        explanation: "說明經過後天學習加工，人能夠得到提升並超越原本的起點。"
                    },
                    {
                        id: "q_qx_4",
                        type: "long",
                        title: "4. 荀子在文中如何論述「思考」與「學習」的關係？（3分）",
                        score: 3,
                        keywords: ["終日而思", "須臾之所學", "學習", "思考"],
                        modelAnswer: "荀子指出「吾嘗終日而思矣，不如須臾之所學也」，（1分）說明空想並無益處，強調後天主動學習遠比盲目空想更具實效。（2分）"
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
                    "【2】 相如聞，不肯與會。相如每朝，常稱病，不欲與廉頗爭列。已而相如出，望見廉頗，相如引車避匿。",
                    "【3】 舍人相與諫曰：「臣等離親戚而事君者，徒慕君之高義也。今君與廉頗同列，廉君宣惡言，而君畏匿之，恐懼殊甚。庸人尚羞之，況於將相乎！臣等不肖，請辭去。」",
                    "【4】 相如固止之，曰：「顧吾念之，強秦之所以不敢加兵於趙者，徒以吾兩人在也。今兩虎共鬥，其勢不俱生。吾所以為此者，以先國家之急而後私仇也！」廉頗聞之，肉袒負荊謝罪。"
                ],
                questions: [
                    {
                        id: "q_lp_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_lp_1a", prompt: "(1) 相如「素」賤人。", targetWord: "素", correctAnswer: "本來 / 向來", keywords: ["本來", "向來", "一向"], score: 2 },
                            { subId: "q_lp_1b", prompt: "(2) 相如「引」車避匿。", targetWord: "引", correctAnswer: "掉轉 / 牽引", keywords: ["掉轉", "拉回", "牽引", "退"], score: 2 }
                        ]
                    },
                    {
                        id: "q_lp_2",
                        type: "mc",
                        title: "2. 藺相如面對廉頗的挑釁退讓避匿，其根本原因是什麼？",
                        score: 2,
                        options: [
                            "A. 畏懼廉頗身為大將軍的武力",
                            "B. 以國家安全利益為先，避免將相不和讓秦國有機可乘",
                            "C. 欲藉此向趙王控訴廉頗霸道",
                            "D. 聽從門客建議故意示弱"
                        ],
                        correctAnswer: "B",
                        explanation: "「先國家之急而後私仇也」，藺相如以國事大局為重。"
                    },
                    {
                        id: "q_lp_3",
                        type: "mc",
                        title: "3. 廉頗得知藺相如避讓的真正原因後，有何反應？",
                        score: 2,
                        options: [
                            "A. 認為相如虛偽裝模作樣",
                            "B. 肉袒負荊親自到相如家謝罪",
                            "C. 向趙王請辭大將軍之職",
                            "D. 擺設酒宴請相如前來"
                        ],
                        correctAnswer: "B",
                        explanation: "廉頗知錯能改，坦誠「肉袒負荊」拜謝。"
                    },
                    {
                        id: "q_lp_4",
                        type: "long",
                        title: "4. 試分別分析藺相如與廉頗二人在「將相和」一節中所展現的人格品質。（4分）",
                        score: 4,
                        keywords: ["藺相如", "顧全大局", "胸襟廣闊", "廉頗", "直率", "知錯能改"],
                        modelAnswer: "藺相如：顧全大局、胸襟廣闊，為國家利益甘願忍受個人屈辱；（2分）\n廉頗：性格率直、勇於改過（知錯能改），有大將光明磊落之風。（2分）"
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
                    "【2】 誠宜開張聖聽，以光先帝遺德，恢弘志士之氣，不宜妄自菲薄，引喻失義，以塞忠諫之路也。",
                    "【3】 親賢臣，遠小人，此先漢之所以興隆也；親小人，遠賢臣，此後漢之所以傾頹也。"
                ],
                questions: [
                    {
                        id: "q_csb_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_csb_1a", prompt: "(1) 此誠危急存亡之「秋」也。", targetWord: "秋", correctAnswer: "關鍵時刻 / 時期", keywords: ["時刻", "時期", "關頭"], score: 2 },
                            { subId: "q_csb_1b", prompt: "(2) 以「光」先帝遺德。", targetWord: "光", correctAnswer: "發揚光大", keywords: ["發揚光大", "光大"], score: 2 }
                        ]
                    },
                    {
                        id: "q_csb_2",
                        type: "mc",
                        title: "2. 諸葛亮在《出師表》開篇分析蜀漢當時面臨的客觀形勢是：",
                        score: 2,
                        options: [
                            "A. 兵強馬壯，蓄勢待發",
                            "B. 天下三分，益州疲弊，正值危急存亡之際",
                            "C. 魏吳兩國內亂，蜀漢有機可乘",
                            "D. 國庫充盈，百姓安居樂業"
                        ],
                        correctAnswer: "B",
                        explanation: "「先帝創業未半……益州疲弊，此誠危急存亡之秋也」。"
                    },
                    {
                        id: "q_csb_3",
                        type: "long",
                        title: "3. 諸葛亮在文中向後主劉禪提出了哪三條治國建議？（6分）",
                        score: 6,
                        keywords: ["開張聖聽", "廣開言路", "賞罰分明", "嚴明賞罰", "親賢臣遠小人", "親賢遠佞"],
                        modelAnswer: "① 廣開言路（開張聖聽），不宜妄自菲薄；（2分）\n② 嚴明賞罰（賞罰一律），宮中府中俱為一體；（2分）\n③ 親賢遠佞（親賢臣，遠小人）。（2分）"
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
                    "【2】 嗟乎！師道之不傳也久矣！欲人之無惑也難矣！古之聖人，其出人也遠矣，猶且從師而問焉；今之眾人，其下聖人也亦遠矣，而恥學於師。是故聖益聖，愚益愚。",
                    "【3】 聖人無常師。孔子師郯子、訐謨、師襄、老聃。郯子之徒，其賢不及孔子。孔子曰：三人行，則必有我師。是故弟子不必不如師，師不必賢於弟子，聞道有先後，術業有專攻，如是而已。"
                ],
                questions: [
                    {
                        id: "q_ss_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_ss_1a", prompt: "(1) 師者，所以傳道「受」業解惑也。", targetWord: "受", correctAnswer: "通「授」，教授", keywords: ["授", "教授", "傳授"], score: 2 },
                            { subId: "q_ss_1b", prompt: "(2) 術業有「專攻」。", targetWord: "專攻", correctAnswer: "專門研究", keywords: ["專門研究", "專研", "擅長"], score: 2 }
                        ]
                    },
                    {
                        id: "q_ss_2",
                        type: "mc",
                        title: "2. 韓愈認為擇師的根本標準是什麼？",
                        score: 2,
                        options: [
                            "A. 老師的社會地位與爵位",
                            "B. 老師的年紀是否比自己大",
                            "C. 誰掌握了道理（「道之所存，師之所存」）",
                            "D. 是否具備朝廷官職"
                        ],
                        correctAnswer: "C",
                        explanation: "韓愈強調「無貴無賤，無長無少，道之所存，師之所存也」。"
                    },
                    {
                        id: "q_ss_3",
                        type: "mc",
                        title: "3. 韓愈寫《師說》的主要時代背景與動機是：",
                        score: 2,
                        options: [
                            "A. 讚揚士大夫階層勤奮好學",
                            "B. 批判當時士大夫階層恥於從師求學風氣，提倡尊師重道",
                            "C. 為李氏子盤參加科舉考試作準備",
                            "D. 宣傳儒家孝道思想"
                        ],
                        correctAnswer: "B",
                        explanation: "針對中唐士大夫「恥學於師」的風氣進行針砭與匡正。"
                    },
                    {
                        id: "q_ss_4",
                        type: "long",
                        title: "4. 韓愈在文中運用了哪三組對比來批判當時「恥學於師」的不良風氣？（6分）",
                        score: 6,
                        keywords: ["古之聖人", "今之眾人", "愛其子", "於其身", "巫醫樂師百工", "士大夫"],
                        modelAnswer: "① 古之聖人（從師）與今之眾人（恥學於師）的對比；（2分）\n② 擇師教子（擇師而教之）與自身恥於從師（於其身也則恥師焉）的對比；（2分）\n③ 巫醫樂師百工之人（不恥相師）與士大夫之族（群聚而笑之）的對比。（2分）"
                    }
                ]
            },
            {
                id: "xi_shan",
                title: "《始得西山宴遊記》",
                author: "柳宗元",
                era: "唐代",
                fullText: [
                    "【1】 自余為僇人，居是州，恆惴慄。其隙也，則施施而行，漫漫而遊。日與其徒上高山，入深林，窮迴溪，幽泉怪石，無遠不到。",
                    "【2】 攀草牽棘，設水置棋，尋撞傾欹，到則鋪席而坐。傾壺而醉，醉則相枕以臥，臥而夢。意有所極，夢亦同趣。覺而起，起而歸。以為凡是州之山水有異態者，皆我有也，而未始知西山之怪特。",
                    "【3】 心凝形釋，與萬化冥合。然後知吾嚮之未始遊，遊於是乎始，故題之曰《始得西山宴遊記》。"
                ],
                questions: [
                    {
                        id: "q_xs_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_xs_1a", prompt: "(1) 自余為「僇人」。", targetWord: "僇人", correctAnswer: "受辱有罪的人（受貶官員）", keywords: ["有罪", "受辱", "貶", "罪人"], score: 2 },
                            { subId: "q_xs_1b", prompt: "(2) 「嚮」之未始遊。", targetWord: "嚮", correctAnswer: "通「向」，以往 / 從前", keywords: ["以往", "從前", "之前"], score: 2 }
                        ]
                    },
                    {
                        id: "q_xs_2",
                        type: "mc",
                        title: "2. 柳宗元在未發現西山之前，遊覽永州山水的心態是怎样的？",
                        score: 2,
                        options: [
                            "A. 充滿熱情，專注於山水美景",
                            "B. 憂懼不安（恆惴慄），藉漫遊尋求短暫解脫與宣洩",
                            "C. 悠然自得，完全忘卻政治失意",
                            "D. 敷衍了事，極度厭倦"
                        ],
                        correctAnswer: "B",
                        explanation: "「自余為僇人，居是州，恆惴慄」，內心充斥被貶的恐懼與憂悶。"
                    },
                    {
                        id: "q_xs_3",
                        type: "long",
                        title: "3. 文中「心凝形釋，與萬化冥合」表達了作者怎樣的精神境界？（4分）",
                        score: 4,
                        keywords: ["心凝形釋", "萬化冥合", "忘我", "解脫", "融合"],
                        modelAnswer: "指作者精神凝聚、超脫身體束縛（心凝形釋），（2分）達到與大自然萬物融為一體的忘我境界，心靈獲得徹底解脫。（2分）"
                    }
                ]
            },
            {
                id: "yue_yang_lou",
                title: "《岳陽樓記》",
                author: "范仲淹",
                era: "宋代",
                fullText: [
                    "【1】 慶曆四年春，滕子京適守巴陵郡。越明年，政通人和，百廢具興。乃重修岳陽樓，增其舊制，刻唐賢今人詩賦於其上，屬予作文以記之。",
                    "【2】 予觀夫巴陵勝狀，在洞庭一湖。銜遠山，吞長江，浩浩湯湯，橫無際涯；朝暉夕陰，氣象萬千。此則岳陽樓之大觀也。",
                    "【3】 不以物喜，不以己悲。居廟堂之高則憂其民，處江湖之遠則憂其君。是進亦憂，退亦憂。然則何時而樂耶？其必曰「先天下之憂而憂，後天下之樂而樂」乎！"
                ],
                questions: [
                    {
                        id: "q_yyl_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_yyl_1a", prompt: "(1) 百廢「具」興。", targetWord: "具", correctAnswer: "通「俱」，全 / 皆", keywords: ["俱", "全", "皆", "都"], score: 2 },
                            { subId: "q_yyl_1b", prompt: "(2) 「屬」予作文以記之。", targetWord: "屬", correctAnswer: "通「囑」，叮囑 / 囑託", keywords: ["囑", "叮囑", "囑託", "拜託"], score: 2 }
                        ]
                    },
                    {
                        id: "q_yyl_2",
                        type: "mc",
                        title: "2. 范仲淹寫「遷客騷人」覽物之情，陰晴兩種天氣帶出的情緒分別是：",
                        score: 2,
                        options: [
                            "A. 陰天悲傷痛苦，晴天喜悅自得",
                            "B. 陰天憤世嫉俗，晴天豁達大度",
                            "C. 陰天平靜淡泊，晴天興奮狂喜",
                            "D. 陰晴天氣皆不影響其情緒"
                        ],
                        correctAnswer: "A",
                        explanation: "「去國懷鄉，憂讒畏譏，滿目蕭然，感極而悲」與「心曠神怡，寵辱偕忘，把酒臨風，其喜洋洋」。"
                    },
                    {
                        id: "q_yyl_3",
                        type: "long",
                        title: "3. 試解釋「先天下之憂而憂，後天下之樂而樂」的深刻意涵。（4分）",
                        score: 4,
                        keywords: ["擔憂", "享樂", "人民", "天下", "抱負"],
                        modelAnswer: "指在天下人擔憂之前先去擔憂國家人民的疾苦；（2分）在天下人都享樂之後自己才去享受快樂。體現以天下為己任的高尚政治抱負。（2分）"
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
                    "【2】 醉翁之意不在酒，在乎山水之間也。山水之樂，得之心而寓之酒也。",
                    "【3】 禽鳥知山林之樂，而不知人之樂；人知從太守遊而樂，而不知太守之樂其樂也。醉能同其樂，醒能述以文者，太守也。太守謂誰？廬陵歐陽修也。"
                ],
                questions: [
                    {
                        id: "q_zwt_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_zwt_1a", prompt: "(1) 醉翁之「意」不在酒。", targetWord: "意", correctAnswer: "情趣 / 心意", keywords: ["情趣", "心意", "意旨"], score: 2 },
                            { subId: "q_zwt_1b", prompt: "(2) 有亭「翼然」臨於泉上。", targetWord: "翼然", correctAnswer: "像鳥展翅張開的樣子", keywords: ["鳥展翅", "展翅", "飛翔"], score: 2 }
                        ]
                    },
                    {
                        id: "q_zwt_2",
                        type: "mc",
                        title: "2. 文中「太守之樂其樂」中的「太守之樂」核心是指什麼？",
                        score: 2,
                        options: [
                            "A. 痛快飲酒醉倒的樂趣",
                            "B. 與民同樂（因百姓安居樂業而感到快樂）",
                            "C. 獨自品味山水風景之樂",
                            "D. 擺脫朝廷政務的輕鬆樂趣"
                        ],
                        correctAnswer: "B",
                        explanation: "最高層次的樂是「樂民之樂」，即與民同樂。"
                    },
                    {
                        id: "q_zwt_3",
                        type: "long",
                        title: "3. 歐陽修在文中如何層層遞進描寫「山林之樂」、「人之樂」與「太守之樂」？（4分）",
                        score: 4,
                        keywords: ["禽鳥", "遊人", "太守", "與民同樂"],
                        modelAnswer: "① 禽鳥僅知自然的山林之樂；（1分）\n② 遊人知道跟隨太守遊山玩水之樂（人之樂）；（1分）\n③ 太守則以能讓百姓快樂並共享其樂為最高樂趣（太守之樂/與民同樂），層層遞進。（2分）"
                    }
                ]
            },
            {
                id: "tang_shi",
                title: "唐詩三首",
                author: "李白、杜甫",
                era: "唐代",
                fullText: [
                    "【1】《蜀道難》（節錄）：噫吁戲，危乎高哉！蜀道之難，難於上青天！……上有六龍回日之高標，下有衝波逆折之回川。黃鶴之飛尚不得過，猿猱欲度愁攀援。",
                    "【2】《登高》：風急天高猿嘯哀，渚清沙白鳥飛回。無邊落木蕭蕭下，不盡長江滾滾來。萬里悲秋常作客，百年多病獨登台。艱難苦恨繁霜鬢，潦倒新停濁酒杯。",
                    "【3】《月下獨酌》：花間一壺酒，獨酌無相親。舉杯邀明月，對影成三人。"
                ],
                questions: [
                    {
                        id: "q_ts_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_ts_1a", prompt: "(1) 萬里悲秋常「作客」。", targetWord: "作客", correctAnswer: "寄居異鄉 / 漂泊客居", keywords: ["寄居", "異鄉", "客居", "漂泊"], score: 2 },
                            { subId: "q_ts_1b", prompt: "(2) 艱難「苦恨」繁霜鬢。", targetWord: "苦恨", correctAnswer: "極度遺憾 / 深以為苦", keywords: ["遺憾", "痛恨", "苦於", "深以為苦"], score: 2 }
                        ]
                    },
                    {
                        id: "q_ts_2",
                        type: "mc",
                        title: "2. 李白《月下獨酌》中「舉杯邀明月，對影成三人」中的「三人」指哪三者？",
                        score: 2,
                        options: [
                            "A. 李白、杜甫、高適",
                            "B. 李白、明月、自己的影子",
                            "C. 李白、月光、花朵",
                            "D. 李白與兩位酒友"
                        ],
                        correctAnswer: "B",
                        explanation: "三人分別指李白自己、邀來的明月、以及月光照射下的影子。"
                    },
                    {
                        id: "q_ts_3",
                        type: "long",
                        title: "3. 試分析杜甫《登高》中「無邊落木蕭蕭下，不盡長江滾滾來」一聯在寫景與抒情上的妙處。（4分）",
                        score: 4,
                        keywords: ["落木", "長江", "對仗", "時光", "衰老", "悲秋"],
                        modelAnswer: "寫景：寫出秋山落葉紛飛的蕭瑟與長江奔流不息的宏偉壯闊，對仗極其工整；（2分）\n抒情：以落木比喻自己年老體衰，以滾滾長江比喻歷史時光無情流逝，寄託了深沉的悲秋與身世之感。（2分）"
                    }
                ]
            },
            {
                id: "song_ci",
                title: "宋詞三首",
                author: "蘇軾、李清照、辛棄疾",
                era: "宋代",
                fullText: [
                    "【1】《念奴嬌·赤壁懷古》：大江東去，浪淘盡，千古風流人物。故壘西邊，人道是，三國周郎赤壁。亂石穿空，驚濤拍岸，捲起千堆雪。江山如畫，一時多少豪傑。",
                    "【2】《聲聲慢·尋尋覓覓》：尋尋覓覓，冷冷清清，悽悽慘惨戚戚。乍暖還寒時候，最難將息。三杯兩盞淡酒，怎敵他、晚來風急！雁過也，正傷心，却是舊時相識。",
                    "【3】《青玉案·元夕》：東風夜放花千樹。更吹落、星如雨。寶馬雕車香滿路。鳳簫聲動，玉壺光轉，一夜魚龍舞。蛾兒雪柳黃金縷。笑語盈盈暗香去。眾裏尋他千百度。驀然回首，那人卻在，燈火闌珊處。"
                ],
                questions: [
                    {
                        id: "q_sc_1",
                        type: "definition",
                        title: "1. 解釋以下劃有下劃線的字詞意思：",
                        score: 4,
                        subQuestions: [
                            { subId: "q_sc_1a", prompt: "(1) 最難「將息」。", targetWord: "將息", correctAnswer: "調養 / 休息", keywords: ["調養", "休息", "保養"], score: 2 },
                            { subId: "q_sc_1b", prompt: "(2) 燈火「闌珊」處。", targetWord: "闌珊", correctAnswer: "零落 / 稀疏 / 衰退", keywords: ["零落", "稀疏", "昏暗", "衰退"], score: 2 }
                        ]
                    },
                    {
                        id: "q_sc_2",
                        type: "mc",
                        title: "2. 蘇軾在《念奴嬌·赤壁懷古》中描寫周瑜「羽扇綸巾，談笑間，強虜灰飛煙滅」，用意何在？",
                        score: 2,
                        options: [
                            "A. 嘲笑周瑜年少輕狂",
                            "B. 以周瑜年少有為功業顯赫，反襯自己早生華發功業未成的坎坷失意",
                            "C. 純粹記錄赤壁之戰歷史過程",
                            "D. 抒發對曹操兵敗的同情"
                        ],
                        correctAnswer: "B",
                        explanation: "以周瑜的英姿颯爽與功名卓越，反襯自己被貶黃州、功業未立的無奈。"
                    },
                    {
                        id: "q_sc_3",
                        type: "long",
                        title: "3. 辛棄疾《青玉案·元夕》下片中，盛裝婦女與「那人」的形象有何對比？試分析其深意。（4分）",
                        score: 4,
                        keywords: ["盛裝婦女", "喧鬧", "那人", "孤高", "不隨波逐流"],
                        modelAnswer: "對比：盛裝婦女趨炎附勢、追逐熱鬧喧囂；而「那人」則獨自立於燈火稀疏孤寂處；（2分）\n深意：寄託作者不願與庸俗官僚同流合污、堅持孤高純潔人格與愛國志向的心志。（2分）"
                    }
                ]
            }
        ];

        // =========================================================================
        // STATE MANAGEMENT & LOCAL STORAGE PERSISTENCE
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
            document.getElementById('gate-submit-btn').innerText = isLogin ? '登入平台開始解題' : '創建帳號並登入';
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
        // DASHBOARD & EXAM EVALUATION LOGIC
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

        function renderPassageCards() {
            const container = document.getElementById('passage-cards-container');
            container.innerHTML = TWELVE_CLASSICAL_TEXTS.map((passage, index) => {
                const totalScore = passage.questions.reduce((sum, q) => sum + q.score, 0);
                const questionCount = passage.questions.length;
                return `
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 hover:shadow-md transition p-5 flex flex-col justify-between">
                        <div>
                            <div class="flex justify-between items-start mb-2">
                                <span class="text-xs font-bold bg-blue-50 text-blue-800 border border-blue-200 px-2 py-0.5 rounded">${passage.era}</span>
                                <span class="text-xs font-bold text-dse-navy bg-slate-100 px-2 py-0.5 rounded">篇章 #${index + 1}</span>
                            </div>
                            <h3 class="text-xl font-bold font-serif text-dse-navy mb-1">${passage.title}</h3>
                            <p class="text-xs font-medium text-slate-500 mb-2">作者：${passage.author}</p>
                            <div class="flex items-center space-x-2 mb-3">
                                <span class="text-xs bg-amber-50 text-amber-800 border border-amber-200 px-2 py-0.5 rounded font-bold">${questionCount} 道精選題</span>
                                <span class="text-xs bg-emerald-50 text-emerald-800 border border-emerald-200 px-2 py-0.5 rounded font-bold">${totalScore} 滿分</span>
                            </div>
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
                feedbackMsg = "頂尖表現！文言詞義與範文題型掌握極為精準，完全具備 DSE 中文科 5** 水平。";
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
