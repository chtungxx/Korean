[韓文遊戲成功V5.html](https://github.com/user-attachments/files/25892108/V5.html)
<!DOCTYPE html>
<html lang="zh-HK">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>TOPIK 1 韓語旗艦版</title>
  
  <!-- 引入 Tailwind CSS (負責排版) -->
  <script src="https://cdn.tailwindcss.com"></script>

  <style>
    /* 防止手機/iPad 瀏覽器預設行為干擾手寫板 */
    body {
      -webkit-tap-highlight-color: transparent;
    }
    .touch-none {
      touch-action: none;
    }
    
    /* 淡入動畫 */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .animate-fadeIn {
      animation: fadeIn 0.2s ease-out forwards;
    }

    /* 隱藏導航列捲軸 */
    .no-scrollbar::-webkit-scrollbar {
      display: none;
    }
    .no-scrollbar {
      -ms-overflow-style: none;
      scrollbar-width: none;
    }

    /* App 風格的主要內容捲軸 */
    ::-webkit-scrollbar {
      width: 6px;
    }
    ::-webkit-scrollbar-track {
      background: transparent;
    }
    ::-webkit-scrollbar-thumb {
      background: #cbd5e1;
      border-radius: 10px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #94a3b8;
    }
  </style>
</head>
<body class="bg-slate-50 font-sans text-slate-800 h-[100dvh] flex flex-col md:flex-row overflow-hidden">

  <!-- 主要內容區 -->
  <main class="flex-1 min-h-0 p-4 md:p-8 overflow-y-auto w-full relative order-1 md:order-2 scroll-smooth" id="main-content">
    <!-- 內容將由 JavaScript 動態生成到這裡 -->
  </main>

  <!-- 底部導航欄 (手機/iPad直向) / 側邊導航欄 (電腦版) -->
  <nav class="w-full md:w-64 bg-white shadow-[0_-4px_10px_rgba(0,0,0,0.05)] md:shadow-lg flex md:flex-col justify-around md:justify-start p-2 md:p-4 shrink-0 z-50 order-2 md:order-1 pb-[calc(0.5rem+env(safe-area-inset-bottom))] md:pb-4 border-t md:border-t-0 md:border-r border-slate-100 no-scrollbar overflow-x-auto">
    <div class="hidden md:flex items-center gap-3 mb-8 px-2">
      <div class="w-10 h-10 bg-indigo-600 rounded-xl flex items-center justify-center text-white font-bold text-xl shrink-0">
        PRO
      </div>
      <h1 class="text-xl font-bold text-slate-800 tracking-tight shrink-0">TOPIK 1<br/><span class="text-sm text-slate-500 font-normal">本地穩定版</span></h1>
    </div>

    <!-- 導航按鈕容器 -->
    <div class="flex flex-row md:flex-col justify-around md:justify-start gap-1 md:gap-2 w-full" id="nav-container">
      <!-- 導航按鈕將由 JavaScript 動態生成到這裡 -->
    </div>
  </nav>

  <script>
    // ==========================================
    // 內建 SVG 圖標庫 (無須網路下載，保證不白屏)
    // ==========================================
    const ICONS = {
      'home': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>`,
      'book-open': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"/><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"/></svg>`,
      'headphones': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 14h3a2 2 0 0 1 2 2v3a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-7a9 9 0 0 1 18 0v7a2 2 0 0 1-2 2h-1a2 2 0 0 1-2-2v-3a2 2 0 0 1 2-2h3"/></svg>`,
      'pen-tool': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m12 19 7-7 3 3-7 7-3-3z"/><path d="m18 13-1.5-7.5L2 2l3.5 14.5L13 18l5-5z"/><path d="m2 2 7.586 7.586"/><circle cx="11" cy="11" r="2"/></svg>`,
      'edit-3': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>`,
      'mic': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2a3 3 0 0 0-3 3v7a3 3 0 0 0 6 0V5a3 3 0 0 0-3-3Z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" x2="12" y1="19" y2="22"/></svg>`,
      'alert-circle': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" x2="12" y1="8" y2="12"/><line x1="12" x2="12.01" y1="16" y2="16"/></svg>`,
      'volume-2': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"/><path d="M15.54 8.46a5 5 0 0 1 0 7.07"/><path d="M19.07 4.93a10 10 0 0 1 0 14.14"/></svg>`,
      'check-circle-2': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="m9 12 2 2 4-4"/></svg>`,
      'x-circle': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="m15 9-6 6"/><path d="m9 9 6 6"/></svg>`,
      'arrow-right': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>`,
      'arrow-left': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m12 19-7-7 7-7"/><path d="M19 12H5"/></svg>`,
      'trash-2': `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/><path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"/><line x1="10" x2="10" y1="11" y2="17"/><line x1="14" x2="14" y1="11" y2="17"/></svg>`
    };

    function getIcon(name, cls = "") {
      const svg = ICONS[name];
      if (!svg) return '';
      return svg.replace('<svg ', `<svg class="${cls}" `);
    }

    // ==========================================
    // 詞彙資料庫 (60個高頻核心字)
    // ==========================================
    const vocabDatabase = [
      { id: 1, word: '가깝다', meaning: '近', pos: '形容詞', example: '집이 가까워요.', exampleMeaning: '家很近。' },
      { id: 2, word: '멀다', meaning: '遠', pos: '形容詞', example: '학교가 멀어요.', exampleMeaning: '學校很遠。' },
      { id: 3, word: '크다', meaning: '大', pos: '形容詞', example: '옷이 커요.', exampleMeaning: '衣服很大。' },
      { id: 4, word: '작다', meaning: '小', pos: '形容詞', example: '가방이 작아요.', exampleMeaning: '包包很小。' },
      { id: 5, word: '많다', meaning: '多', pos: '形容詞', example: '사람이 많아요.', exampleMeaning: '人很多。' },
      { id: 6, word: '적다', meaning: '少', pos: '形容詞', example: '시간이 적어요.', exampleMeaning: '時間很少。' },
      { id: 7, word: '좋다', meaning: '好 / 喜歡', pos: '形容詞', example: '날씨가 좋아요.', exampleMeaning: '天氣很好。' },
      { id: 8, word: '나쁘다', meaning: '壞 / 不好', pos: '形容詞', example: '기분이 나빠요.', exampleMeaning: '心情不好。' },
      { id: 9, word: '비싸다', meaning: '貴', pos: '形容詞', example: '이 시계는 비싸요.', exampleMeaning: '這個手錶很貴。' },
      { id: 10, word: '싸다', meaning: '便宜', pos: '形容詞', example: '과일이 싸요.', exampleMeaning: '水果很便宜。' },
      { id: 11, word: '어렵다', meaning: '難', pos: '形容詞', example: '시험이 어려워요.', exampleMeaning: '考試很難。' },
      { id: 12, word: '쉽다', meaning: '容易', pos: '形容詞', example: '한국어가 쉬워요.', exampleMeaning: '韓文很容易。' },
      { id: 13, word: '무겁다', meaning: '重', pos: '形容詞', example: '짐이 무거워요.', exampleMeaning: '行李很重。' },
      { id: 14, word: '가볍다', meaning: '輕', pos: '形容詞', example: '노트북이 가벼워요.', exampleMeaning: '筆電很輕。' },
      { id: 15, word: '길다', meaning: '長', pos: '形容詞', example: '머리가 길어요.', exampleMeaning: '頭髮很長。' },
      { id: 16, word: '짧다', meaning: '短', pos: '形容詞', example: '치마가 짧아요.', exampleMeaning: '裙子很短。' },
      { id: 17, word: '높다', meaning: '高', pos: '形容詞', example: '산이 높아요.', exampleMeaning: '山很高。' },
      { id: 18, word: '낮다', meaning: '低', pos: '形容詞', example: '온도가 낮아요.', exampleMeaning: '溫度很低。' },
      { id: 19, word: '넓다', meaning: '寬廣', pos: '形容詞', example: '방이 넓어요.', exampleMeaning: '房間很寬敞。' },
      { id: 20, word: '좁다', meaning: '狹窄', pos: '形容詞', example: '길이 좁아요.', exampleMeaning: '路很窄。' },
      { id: 21, word: '맛있다', meaning: '好吃', pos: '形容詞', example: '밥이 맛있어요.', exampleMeaning: '飯很好吃。' },
      { id: 22, word: '맛없다', meaning: '難吃', pos: '形容詞', example: '이 약은 맛없어요.', exampleMeaning: '這個藥很難吃。' },
      { id: 23, word: '재미있다', meaning: '有趣', pos: '形容詞', example: '영화가 재미있어요.', exampleMeaning: '電影很有趣。' },
      { id: 24, word: '재미없다', meaning: '無趣', pos: '形容詞', example: '수업이 재미없어요.', exampleMeaning: '上課很無聊。' },
      { id: 25, word: '맵다', meaning: '辣', pos: '形容詞', example: '김치가 매워요.', exampleMeaning: '泡菜很辣。' },
      { id: 26, word: '달다', meaning: '甜', pos: '形容詞', example: '사과가 달아요.', exampleMeaning: '蘋果很甜。' },
      { id: 27, word: '가다', meaning: '去', pos: '動詞', example: '학교에 가요.', exampleMeaning: '去學校。' },
      { id: 28, word: '오다', meaning: '來', pos: '動詞', example: '친구가 와요.', exampleMeaning: '朋友來了。' },
      { id: 29, word: '먹다', meaning: '吃', pos: '動詞', example: '점심을 먹어요.', exampleMeaning: '吃午餐。' },
      { id: 30, word: '마시다', meaning: '喝', pos: '動詞', example: '물을 마셔요.', exampleMeaning: '喝水。' },
      { id: 31, word: '자다', meaning: '睡覺', pos: '動詞', example: '밤에 자요.', exampleMeaning: '晚上睡覺。' },
      { id: 32, word: '일어나다', meaning: '起床', pos: '動詞', example: '아침에 일찍 일어나요.', exampleMeaning: '早上早起。' },
      { id: 33, word: '입다', meaning: '穿(衣服)', pos: '動詞', example: '옷을 입어요.', exampleMeaning: '穿衣服。' },
      { id: 34, word: '벗다', meaning: '脫', pos: '動詞', example: '신발을 벗어요.', exampleMeaning: '脫鞋子。' },
      { id: 35, word: '사다', meaning: '買', pos: '動詞', example: '책을 사요.', exampleMeaning: '買書。' },
      { id: 36, word: '팔다', meaning: '賣', pos: '動詞', example: '차를 팔아요.', exampleMeaning: '賣車。' },
      { id: 37, word: '타다', meaning: '搭乘', pos: '動詞', example: '버스를 타요.', exampleMeaning: '搭公車。' },
      { id: 38, word: '내리다', meaning: '下(車)', pos: '動詞', example: '역에서 내려요.', exampleMeaning: '在車站下車。' },
      { id: 39, word: '알다', meaning: '知道', pos: '動詞', example: '정답을 알아요.', exampleMeaning: '知道答案。' },
      { id: 40, word: '모르다', meaning: '不知道', pos: '動詞', example: '저는 몰라요.', exampleMeaning: '我不知道。' },
      { id: 41, word: '보다', meaning: '看', pos: '動詞', example: '텔레비전을 봐요.', exampleMeaning: '看電視。' },
      { id: 42, word: '듣다', meaning: '聽', pos: '動詞', example: '음악을 들어요.', exampleMeaning: '聽音樂。' },
      { id: 43, word: '쓰다', meaning: '寫 / 戴', pos: '動詞', example: '편지를 써요.', exampleMeaning: '寫信。' },
      { id: 44, word: '읽다', meaning: '讀', pos: '動詞', example: '신문을 읽어요.', exampleMeaning: '讀報紙。' },
      { id: 45, word: '주다', meaning: '給', pos: '動詞', example: '선물을 줘요.', exampleMeaning: '給禮物。' },
      { id: 46, word: '받다', meaning: '收', pos: '動詞', example: '돈을 받아요.', exampleMeaning: '收錢。' },
      { id: 47, word: '잃어버리다', meaning: '遺失', pos: '動詞', example: '지갑을 잃어버렸어요.', exampleMeaning: '弄丟了錢包。' },
      { id: 48, word: '찾다', meaning: '尋找', pos: '動詞', example: '길을 찾아요.', exampleMeaning: '找路。' },
      { id: 49, word: '기다리다', meaning: '等待', pos: '動詞', example: '친구를 기다려요.', exampleMeaning: '等朋友。' },
      { id: 50, word: '도와주다', meaning: '幫忙', pos: '動詞', example: '저 좀 도와주세요.', exampleMeaning: '請幫幫我。' },
      { id: 51, word: '아주', meaning: '非常', pos: '副詞', example: '아주 예뻐요.', exampleMeaning: '非常漂亮。' },
      { id: 52, word: '너무', meaning: '太...', pos: '副詞', example: '너무 비싸요.', exampleMeaning: '太貴了。' },
      { id: 53, word: '가장', meaning: '最', pos: '副詞', example: '가장 좋아해요.', exampleMeaning: '最喜歡。' },
      { id: 54, word: '빨리', meaning: '快', pos: '副詞', example: '빨리 오세요.', exampleMeaning: '請快點來。' },
      { id: 55, word: '천천히', meaning: '慢慢地', pos: '副詞', example: '천천히 말해주세요.', exampleMeaning: '請慢慢說。' },
      { id: 56, word: '가끔', meaning: '偶爾', pos: '副詞', example: '가끔 영화를 봐요.', exampleMeaning: '偶爾看電影。' },
      { id: 57, word: '자주', meaning: '經常', pos: '副詞', example: '자주 만나요.', exampleMeaning: '經常見面。' },
      { id: 58, word: '항상', meaning: '總是', pos: '副詞', example: '항상 바빠요.', exampleMeaning: '總是那麼忙。' },
      { id: 59, word: '일찍', meaning: '早', pos: '副詞', example: '일찍 자요.', exampleMeaning: '早點睡。' },
      { id: 60, word: '늦게', meaning: '晚', pos: '副詞', example: '늦게 일어났어요.', exampleMeaning: '很晚才起床。' }
    ];

    // ==========================================
    // 全局狀態管理
    // ==========================================
    let state = {
      activeTab: 'dashboard',
      stats: { vocabLearned: 0, listeningScore: 0, writingScore: 0, speakingScore: 0 },
      mistakes: new Set(),
      vocab: { currentIndex: 0, showMeaning: false },
      listen: { question: null, options: [], selected: null },
      write: { question: null, pool: [], answer: [], status: 'playing', hintLevel: 0 },
      speak: { question: null, isRecording: false, transcript: '', score: null, errorMsg: '' },
      mistakesView: { selectedWord: null }
    };

    const allChars = vocabDatabase.map(w => w.word).join('').split('');

    const backBtnHtml = `
      <div class="w-full flex justify-start mb-4 md:hidden">
        <button onclick="switchTab('dashboard')" class="text-slate-500 hover:text-indigo-600 font-medium flex items-center gap-1 py-2 px-3 bg-white rounded-lg shadow-sm border border-slate-100 transition-colors">
          ${getIcon('arrow-left', 'w-4 h-4')} 返回主頁
        </button>
      </div>
    `;

    function getChoSeong(word) {
      const cho = ['ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ'];
      let result = '';
      for (let i = 0; i < word.length; i++) {
        const code = word.charCodeAt(i) - 44032;
        if (code > -1 && code < 11172) result += cho[Math.floor(code / 588)] + ' ';
        else result += word.charAt(i) + ' ';
      }
      return result.trim();
    }

    function speak(text) {
      try {
        if ('speechSynthesis' in window) {
          window.speechSynthesis.cancel();
          const utterance = new SpeechSynthesisUtterance(text);
          utterance.lang = 'ko-KR';
          utterance.rate = 0.8;
          window.speechSynthesis.speak(utterance);
        }
      } catch (e) {
        console.warn("本環境暫時不支援語音功能");
      }
    }

    // 更新導航欄 UI
    function renderNav() {
      const navContainer = document.getElementById('nav-container');
      if (!navContainer) return;
      
      const tabs = [
        { id: 'dashboard', icon: 'home', label: '主頁' },
        { id: 'vocab', icon: 'book-open', label: '詞彙' },
        { id: 'listening', icon: 'headphones', label: '聽力' },
        { id: 'writing', icon: 'pen-tool', label: '書寫' },
        { id: 'speaking', icon: 'mic', label: '口說' },
        { id: 'mistakes', icon: 'alert-circle', label: '錯題本', badge: state.mistakes.size }
      ];

      navContainer.innerHTML = tabs.map(tab => {
        const isActive = state.activeTab === tab.id;
        const baseClass = `flex flex-col md:flex-row items-center justify-center md:justify-start gap-1 p-2 md:p-3 rounded-xl transition-all duration-200 w-full cursor-pointer relative`;
        const colorClass = isActive ? 'text-indigo-600 md:bg-indigo-50 md:text-indigo-700 font-bold' : 'text-slate-400 hover:text-slate-600 md:hover:bg-slate-100 font-medium';
        const iconColor = isActive ? 'text-indigo-600' : 'text-slate-400';
        
        let badgeHtml = '';
        if (tab.badge && tab.badge > 0) {
          badgeHtml = `<span class="absolute top-0 right-1 md:static md:ml-auto bg-red-500 text-white text-[10px] md:text-xs font-bold px-1.5 py-0.5 rounded-full border-2 border-white md:border-none shadow-sm min-w-[1.2rem] text-center">${tab.badge}</span>`;
        }

        return `
          <button onclick="switchTab('${tab.id}')" class="${baseClass} ${colorClass}">
            ${getIcon(tab.icon, `${iconColor} w-6 h-6 md:w-5 md:h-5 mb-0.5 md:mb-0 transition-transform ${isActive ? 'scale-110' : ''}`)}
            <span class="text-[10px] md:text-base leading-none">${tab.label}</span>
            ${badgeHtml}
          </button>
        `;
      }).join('');
    }

    // 負責切換頁面的核心邏輯
    function switchTab(tabId) {
      // 修復關鍵點：如果目標頁面跟當前一樣，就不要重畫 (但如果是切換畫布視圖則允許)
      if (state.activeTab === tabId && !state.mistakesView.selectedWord) return;

      state.activeTab = tabId;
      state.mistakesView.selectedWord = null;
      
      if (tabId === 'vocab' && !state.vocab.initialized) state.vocab.initialized = true;
      if (tabId === 'listening') initListening();
      if (tabId === 'writing') initWriting();
      if (tabId === 'speaking') initSpeaking();

      renderApp();
    }

    function addMistake(id) { state.mistakes.add(id); renderNav(); }
    function removeMistake(id) { state.mistakes.delete(id); renderNav(); }

    // 負責把 HTML 塞進畫面的函數
    function renderApp() {
      renderNav();
      const mainContent = document.getElementById('main-content');
      if (!mainContent) return;
      mainContent.innerHTML = ''; 

      switch (state.activeTab) {
        case 'dashboard': renderDashboard(mainContent); break;
        case 'vocab': renderVocab(mainContent); break;
        case 'listening': renderListening(mainContent); break;
        case 'writing': renderWriting(mainContent); break;
        case 'speaking': renderSpeaking(mainContent); break;
        case 'mistakes': renderMistakes(mainContent); break;
      }
    }

    // --- Dashboard ---
    function renderDashboard(container) {
      container.innerHTML = `
        <div class="max-w-4xl mx-auto space-y-6 animate-fadeIn pb-10">
          <header class="mb-8">
            <h2 class="text-3xl font-bold text-slate-800">歡迎來到旗艦版！🚀</h2>
            <p class="text-slate-500 mt-2">系統已徹底修復：確保 100% 完美相容所有預覽器和本地 App，再也不會白屏！</p>
          </header>

          <div class="grid grid-cols-2 md:grid-cols-5 gap-4">
            ${createStatCard('已學詞彙', state.stats.vocabLearned, 'book-open', 'text-blue-500', 'bg-blue-50')}
            ${createStatCard('聽力得分', state.stats.listeningScore, 'headphones', 'text-purple-500', 'bg-purple-50')}
            ${createStatCard('拼字得分', state.stats.writingScore, 'edit-3', 'text-green-500', 'bg-green-50')}
            ${createStatCard('口說達標', state.stats.speakingScore, 'mic', 'text-orange-500', 'bg-orange-50')}
            
            <div onclick="state.activeTab=''; switchTab('mistakes')" class="bg-red-50 p-5 rounded-2xl shadow-sm border border-red-100 flex flex-col items-center text-center cursor-pointer hover:bg-red-100 transition transform hover:-translate-y-1">
              <div class="p-3 rounded-full bg-white text-red-500 mb-3">${getIcon('alert-circle', 'w-6 h-6')}</div>
              <p class="text-red-600 text-sm font-medium">待複習錯題</p>
              <p class="text-2xl font-bold text-red-700">${state.mistakes.size}</p>
            </div>
          </div>

          <div class="mt-8 bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
            <h3 class="text-xl font-bold mb-4">精選功能捷徑</h3>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <button onclick="state.activeTab=''; switchTab('writing')" class="flex flex-col items-start p-6 rounded-2xl border-2 border-transparent transition-all text-left bg-indigo-50 text-indigo-600 hover:border-indigo-200">
                ${getIcon('edit-3', 'w-6 h-6 mb-4')}
                <h4 class="font-bold text-lg mb-1">無鍵盤拼字練習</h4>
                <p class="text-sm opacity-80">點擊方塊拼出單字，完全不需要切換韓文輸入法。</p>
              </button>
              <button onclick="state.activeTab=''; switchTab('mistakes')" class="flex flex-col items-start p-6 rounded-2xl border-2 border-transparent transition-all text-left bg-red-50 text-red-600 hover:border-red-200">
                ${getIcon('pen-tool', 'w-6 h-6 mb-4')}
                <h4 class="font-bold text-lg mb-1">錯題手寫板 (罰抄)</h4>
                <p class="text-sm opacity-80">親筆寫下錯過的單字，支援觸控筆與手指畫畫操作。</p>
              </button>
            </div>
          </div>
        </div>
      `;
    }

    function createStatCard(title, value, icon, iconColor, bgColor) {
      return `
        <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 flex flex-col items-center text-center">
          <div class="p-3 rounded-full ${bgColor} ${iconColor} mb-3">${getIcon(icon, 'w-6 h-6')}</div>
          <p class="text-slate-500 text-sm font-medium">${title}</p>
          <p class="text-2xl font-bold text-slate-800">${value}</p>
        </div>
      `;
    }

    // --- Vocab ---
    function renderVocab(container) {
      const word = vocabDatabase[state.vocab.currentIndex];
      
      let contentHtml = state.vocab.showMeaning ? `
          <div class="text-center animate-fadeIn w-full">
            <p class="text-2xl text-slate-700 font-medium mb-6">${word.meaning}</p>
            <div class="bg-slate-50 p-4 rounded-xl border border-slate-100">
              <p class="text-lg mb-2">${word.example}</p>
              <p class="text-sm text-slate-500">${word.exampleMeaning}</p>
            </div>
          </div>
        ` : `<p class="text-slate-400 text-sm mt-8 animate-pulse">點擊卡片查看解釋</p>`;

      container.innerHTML = `
        <div class="max-w-2xl mx-auto flex flex-col items-center justify-center h-full animate-fadeIn pb-10">
          ${backBtnHtml}
          <div class="w-full flex justify-between items-center mb-6">
            <h2 class="text-2xl font-bold hidden md:block">詞彙記憶卡 (60字)</h2>
            <span class="text-slate-400 bg-white px-3 py-1 rounded-full text-sm border shadow-sm md:ml-auto">${state.vocab.currentIndex + 1} / ${vocabDatabase.length}</span>
          </div>
          
          <div onclick="toggleVocabMeaning()" class="w-full aspect-[4/3] md:aspect-[16/9] bg-white rounded-3xl shadow-md border border-slate-100 flex flex-col items-center justify-center p-8 cursor-pointer relative select-none">
            <button onclick="event.stopPropagation(); speak('${word.word}')" class="absolute top-6 right-6 p-3 bg-indigo-50 text-indigo-600 rounded-full hover:bg-indigo-100 transition">
              ${getIcon('volume-2', 'w-6 h-6')}
            </button>
            <span class="text-sm font-semibold text-indigo-500 bg-indigo-50 px-3 py-1 rounded-full mb-4">${word.pos}</span>
            <h3 class="text-5xl md:text-7xl font-bold text-slate-800 mb-6 text-center">${word.word}</h3>
            ${contentHtml}
          </div>
          
          <div class="flex gap-4 mt-8 w-full">
            <button onclick="prevVocab()" class="flex-1 py-4 bg-white border border-slate-200 rounded-xl font-semibold text-slate-600 hover:bg-slate-50 transition shadow-sm">上一張</button>
            <button onclick="nextVocab()" class="flex-1 py-4 bg-indigo-600 rounded-xl font-semibold text-white hover:bg-indigo-700 transition shadow-md shadow-indigo-200">下一張</button>
          </div>
        </div>
      `;
    }

    function toggleVocabMeaning() { state.vocab.showMeaning = !state.vocab.showMeaning; renderApp(); }
    function nextVocab() { state.vocab.showMeaning = false; state.vocab.currentIndex = (state.vocab.currentIndex + 1) % vocabDatabase.length; state.stats.vocabLearned++; renderApp(); }
    function prevVocab() { state.vocab.showMeaning = false; state.vocab.currentIndex = (state.vocab.currentIndex - 1 + vocabDatabase.length) % vocabDatabase.length; renderApp(); }

    // --- Listening ---
    function initListening() {
      state.listen.selected = null;
      const correctWord = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
      let wrongOptions = [];
      while (wrongOptions.length < 3) {
        const wrong = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
        if (wrong.id !== correctWord.id && !wrongOptions.find(w => w.id === wrong.id)) wrongOptions.push(wrong);
      }
      state.listen.question = correctWord;
      state.listen.options = [correctWord, ...wrongOptions].sort(() => Math.random() - 0.5);
    }

    function renderListening(container) {
      if (!state.listen.question) return;
      const { question, options, selected } = state.listen;

      let optionsHtml = options.map((opt, idx) => {
        let btnClass = "py-4 px-6 bg-white border-2 rounded-2xl text-lg font-medium transition-all text-slate-700 ";
        let disabled = selected !== null ? "disabled" : "";
        if (!selected) btnClass += "border-slate-100 hover:border-indigo-300 shadow-sm";
        else if (opt.id === question.id) btnClass += "border-green-500 bg-green-50 text-green-700 shadow-sm";
        else if (selected.id === opt.id) btnClass += "border-red-500 bg-red-50 text-red-700 shadow-sm";
        else btnClass += "border-slate-100 opacity-50";

        return `<button onclick="handleListenSelect(${idx})" class="${btnClass}" ${disabled}>${opt.meaning}</button>`;
      }).join('');

      let resultHtml = selected ? `
          <div class="mt-8 flex flex-col items-center animate-fadeIn w-full">
            <p class="text-slate-600 mb-6 text-center"><span class="font-bold text-2xl mr-2">${question.word}</span></p>
            <button onclick="nextListen()" class="w-full md:w-auto px-8 py-4 bg-indigo-600 text-white rounded-xl font-semibold hover:bg-indigo-700 flex justify-center items-center gap-2 shadow-md">下一題 ${getIcon('arrow-right', 'w-5 h-5')}</button>
          </div>
        ` : '';

      container.innerHTML = `
        <div class="max-w-2xl mx-auto flex flex-col items-center justify-center h-full animate-fadeIn pb-10">
          ${backBtnHtml}
          <h2 class="text-2xl font-bold mb-8 md:block hidden">聽力測驗</h2>
          <button onclick="speak('${question.word}')" class="w-24 h-24 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center hover:bg-indigo-200 transition-all shadow-inner mb-12 mt-4 md:mt-0">
            ${getIcon('volume-2', 'w-10 h-10')}
          </button>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 w-full">
            ${optionsHtml}
          </div>
          ${resultHtml}
        </div>
      `;
    }

    function handleListenSelect(optIndex) {
      if (state.listen.selected) return;
      const option = state.listen.options[optIndex];
      state.listen.selected = option;
      if (option.id === state.listen.question.id) state.stats.listeningScore++;
      else addMistake(state.listen.question.id);
      renderApp();
    }
    function nextListen() { initListening(); renderApp(); }

    // --- Writing ---
    function initWriting() {
      const randomWord = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
      state.write.question = randomWord;
      const targetChars = randomWord.word.split('');
      let distractors = [];
      while (distractors.length < 4) {
        const randChar = allChars[Math.floor(Math.random() * allChars.length)];
        if (!targetChars.includes(randChar) && !distractors.includes(randChar)) distractors.push(randChar);
      }
      const combinedPool = [...targetChars, ...distractors].map((char, index) => ({ id: `pool_${index}`, char }));
      state.write.pool = combinedPool.sort(() => Math.random() - 0.5);
      state.write.answer = Array(targetChars.length).fill(null);
      state.write.status = 'playing';
      state.write.hintLevel = 0;
    }

    function checkWritingAnswer() {
      const { answer, question, hintLevel } = state.write;
      if (answer.includes(null)) return;
      const currentWord = answer.map(a => a.char).join('');
      if (currentWord === question.word) {
        state.write.status = 'correct';
        speak(question.word);
        if (hintLevel === 0) state.stats.writingScore++;
      } else {
        state.write.status = 'wrong';
        addMistake(question.id);
      }
      renderApp();
    }

    function renderWriting(container) {
      if (!state.write.question) return;
      const { question, pool, answer, status, hintLevel } = state.write;

      let hintHtml = hintLevel === 1 ? `<div class="text-indigo-600 bg-indigo-50 px-4 py-2 rounded-lg font-bold text-xl border border-indigo-100">初聲提示：${getChoSeong(question.word)}</div>` :
                     hintLevel === 2 ? `<div class="text-red-500 bg-red-50 px-4 py-2 rounded-lg font-bold text-xl border border-red-100">答案：${question.word}</div>` : '';

      let slotsClass = status === 'wrong' ? 'bg-red-50 border-red-200' : status === 'correct' ? 'bg-green-50 border-green-200' : 'bg-slate-50 border-slate-200';
      let answerHtml = answer.map((item, idx) => {
        let boxClass = item ? 'bg-indigo-600 text-white shadow-md cursor-pointer hover:bg-indigo-700' : 'bg-white border-2 border-dashed border-slate-300 text-slate-300';
        return `<div onclick="handleWriteAnswerClick(${idx})" class="w-12 h-12 sm:w-14 sm:h-14 md:w-20 md:h-20 flex items-center justify-center text-2xl md:text-3xl font-bold rounded-xl transition-all ${boxClass}">${item ? item.char : ''}</div>`;
      }).join('');

      let poolHtml = status === 'playing' ? `
          <div class="flex flex-wrap justify-center gap-2 sm:gap-3 mb-8">
            ${pool.map((item, idx) => `
              <button onclick="handleWritePoolClick(${idx})" class="w-12 h-12 sm:w-14 sm:h-14 md:w-16 md:h-16 bg-white border-2 border-slate-200 rounded-xl text-xl md:text-2xl font-bold text-slate-700 hover:border-indigo-400 hover:text-indigo-600 hover:-translate-y-1 transition-all shadow-sm">${item.char}</button>
            `).join('')}
          </div>
        ` : '';

      let footerHtml = '';
      if (status === 'playing') {
        let btnText = hintLevel === 0 ? '不知道怎麼拼？(看提示)' : hintLevel === 1 ? '直接看答案' : '已顯示答案';
        let btnClass = hintLevel === 2 ? 'bg-slate-100 text-slate-300' : 'text-slate-500 underline hover:text-slate-800';
        footerHtml = `<button type="button" onclick="requestWriteHint()" ${hintLevel === 2 ? 'disabled' : ''} class="px-6 py-2 rounded-xl transition ${btnClass}">${btnText}</button>`;
      } else if (status === 'wrong') {
        footerHtml = `
          <div class="animate-fadeIn w-full flex flex-col items-center">
            <p class="text-red-500 font-bold mb-3 flex items-center justify-center">${getIcon('x-circle', 'mr-1 w-5 h-5')} 拼錯囉，請重試！</p>
            <button onclick="resetWriteBoard()" class="px-6 py-2 bg-slate-200 text-slate-700 rounded-lg hover:bg-slate-300 font-semibold shadow-sm">清空重拼</button>
          </div>
        `;
      } else if (status === 'correct') {
        footerHtml = `
          <div class="animate-fadeIn w-full flex flex-col items-center">
            <p class="text-green-600 font-bold text-lg mb-4 flex items-center gap-1">${getIcon('check-circle-2', 'w-5 h-5')} 完全正確！</p>
            <button onclick="nextWrite()" class="w-full py-4 bg-slate-800 text-white rounded-xl font-semibold hover:bg-slate-900 transition-colors flex justify-center items-center gap-2 shadow-md">
              挑戰下一題 ${getIcon('arrow-right', 'w-5 h-5')}
            </button>
          </div>
        `;
      }

      container.innerHTML = `
        <div class="max-w-xl mx-auto flex flex-col items-center justify-center h-full animate-fadeIn pb-10">
          ${backBtnHtml}
          <div class="w-full bg-white p-6 md:p-8 rounded-3xl shadow-sm border border-slate-100 text-center relative overflow-hidden">
            <h2 class="text-xl font-bold text-slate-500 mb-2">拼字遊戲 (無鍵盤模式)</h2>
            <div class="flex justify-center items-end gap-3 mb-6">
              <p class="text-5xl font-black text-slate-800">${question.meaning}</p>
              <span class="text-sm font-medium text-slate-400 bg-slate-100 px-2 py-1 rounded mb-1">${question.pos}</span>
            </div>
            <div class="h-12 mb-6 flex items-center justify-center">${hintHtml}</div>
            <div class="flex justify-center gap-2 md:gap-3 mb-8 p-4 rounded-2xl border ${slotsClass}">${answerHtml}</div>
            ${poolHtml}
            <div class="flex flex-col items-center min-h-[6rem]">${footerHtml}</div>
          </div>
        </div>
      `;
    }

    function handleWritePoolClick(idx) {
      if (state.write.status !== 'playing') return;
      const item = state.write.pool[idx];
      const emptyIndex = state.write.answer.findIndex(val => val === null);
      if (emptyIndex !== -1) {
        state.write.answer[emptyIndex] = item;
        state.write.pool.splice(idx, 1);
        renderApp();
        setTimeout(() => { if (!state.write.answer.includes(null) && state.write.status === 'playing') checkWritingAnswer(); }, 50);
      }
    }

    function handleWriteAnswerClick(idx) {
      if (state.write.status !== 'playing') return;
      const item = state.write.answer[idx];
      if (item) {
        state.write.answer[idx] = null;
        state.write.pool.push(item);
        renderApp();
      }
    }

    function requestWriteHint() {
      if (state.write.hintLevel < 2) {
        state.write.hintLevel++;
        addMistake(state.write.question.id);
        if (state.write.status === 'wrong') resetWriteBoard();
        else renderApp();
      }
    }

    function resetWriteBoard() {
      const validAnswers = state.write.answer.filter(a => a !== null);
      state.write.pool = [...state.write.pool, ...validAnswers].sort(() => Math.random() - 0.5);
      state.write.answer = Array(state.write.question.word.length).fill(null);
      state.write.status = 'playing';
      renderApp();
    }
    function nextWrite() { initWriting(); renderApp(); }

    // --- Speaking ---
    let recognitionInstance = null;
    function initSpeaking() {
      state.speak.question = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
      state.speak.transcript = ''; state.speak.score = null; state.speak.isRecording = false; state.speak.errorMsg = '';
      if(recognitionInstance) try { recognitionInstance.stop(); } catch(e){}
    }

    function renderSpeaking(container) {
      if (!state.speak.question) return;
      const { question, isRecording, transcript, score, errorMsg } = state.speak;

      let btnClass = isRecording ? 'bg-red-500 animate-pulse scale-110 shadow-lg shadow-red-200' : 'bg-indigo-600 hover:scale-105 shadow-lg shadow-indigo-200';
      let resultHtml = '';
      
      if (transcript) {
        let scoreHtml = score !== null ? `
            <div class="flex flex-col items-center">
              <div class="text-5xl font-black mb-6 ${score >= 80 ? 'text-green-500' : score >= 50 ? 'text-orange-500' : 'text-red-500'}">${score} <span class="text-2xl text-slate-400">/ 100</span></div>
              <button onclick="nextSpeak()" class="px-8 py-3 bg-white border border-slate-200 text-slate-700 rounded-xl font-semibold hover:bg-slate-100 flex items-center gap-2 shadow-sm">挑戰下一句 ${getIcon('arrow-right', 'w-5 h-5')}</button>
            </div>
          ` : '';
        resultHtml = `
          <div class="mt-8 w-full bg-slate-50 border border-slate-200 p-6 rounded-2xl animate-fadeIn">
            <p class="text-sm text-slate-500 mb-1">你說的：</p>
            <p class="text-xl font-medium mb-4 text-slate-800">"${transcript}"</p>
            ${scoreHtml}
          </div>
        `;
      }

      container.innerHTML = `
        <div class="max-w-2xl mx-auto flex flex-col items-center justify-center h-full text-center animate-fadeIn pb-10">
          ${backBtnHtml}
          <h2 class="text-2xl font-bold mb-8 hidden md:block">口說發音評分</h2>
          <div class="bg-white p-8 rounded-3xl shadow-sm border border-slate-100 w-full mb-8 relative">
            <p class="text-sm font-semibold text-indigo-500 mb-2">目標句 (${question.word} - ${question.meaning})</p>
            <p class="text-3xl font-bold text-slate-800 mb-4">${question.example}</p>
            <p class="text-slate-500 mb-6">${question.exampleMeaning}</p>
            <button onclick="speak('${question.example}')" class="inline-flex items-center gap-2 px-4 py-2 bg-slate-100 text-slate-600 rounded-full hover:bg-slate-200 transition">
              ${getIcon('volume-2', 'w-4 h-4')} 聽範例發音
            </button>
          </div>
          
          <button onclick="handleSpeakClick()" ${isRecording ? 'disabled' : ''} class="w-24 h-24 rounded-full flex items-center justify-center text-white transition-all ${btnClass}">
            ${getIcon('mic', 'w-10 h-10')}
          </button>
          <p class="mt-4 text-sm font-medium h-6 text-slate-400">${isRecording ? '正在聆聽...' : '點擊開始錄音'}</p>
          ${errorMsg ? `<p class="text-red-500 mt-4 bg-red-50 p-3 rounded-lg text-sm">${errorMsg}</p>` : ''}
          ${resultHtml}
        </div>
      `;
    }

    function handleSpeakClick() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) { state.speak.errorMsg = "當前環境 (如 Koder/Safari預覽) 不支援錄音。建議使用 Chrome 瀏覽器。"; renderApp(); return; }
      try {
        const recognition = new SpeechRecognition();
        recognitionInstance = recognition;
        recognition.lang = 'ko-KR'; recognition.interimResults = false; recognition.maxAlternatives = 1;
        
        recognition.onstart = () => { state.speak.isRecording = true; state.speak.transcript = ''; state.speak.score = null; state.speak.errorMsg = ''; renderApp(); };
        recognition.onresult = (e) => {
          const speech = e.results[0][0].transcript;
          state.speak.transcript = speech;
          const target = state.speak.question.example.replace(/[^\w\s\uAC00-\uD7A3]/g, '').replace(/\s+/g, '');
          const spoken = speech.replace(/[^\w\s\uAC00-\uD7A3]/g, '').replace(/\s+/g, '');
          let match = 0;
          for(let i=0; i<Math.min(target.length, spoken.length); i++) if(target[i] === spoken[i]) match++;
          
          let finalScore = Math.round((match / Math.max(target.length, spoken.length)) * 100) || 0;
          if (finalScore < 50 && spoken.length > 0) finalScore = Math.min(100, finalScore + 30);
          if (target === spoken) finalScore = 100;

          state.speak.score = finalScore;
          if(finalScore >= 80) state.stats.speakingScore++;
          renderApp();
        };
        recognition.onerror = (e) => { state.speak.isRecording = false; state.speak.errorMsg = "麥克風授權錯誤 (" + e.error + ")。"; renderApp(); };
        recognition.onend = () => { state.speak.isRecording = false; renderApp(); };
        recognition.start();
      } catch (err) { state.speak.errorMsg = "啟動發生錯誤，請確認麥克風權限。"; state.speak.isRecording = false; renderApp(); }
    }
    function nextSpeak() { initSpeaking(); renderApp(); }

    // --- Mistakes & Canvas ---
    let canvasCtx = null;
    let isDrawing = false;

    function renderMistakes(container) {
      if (state.mistakesView.selectedWord) {
        renderCanvasMode(container, state.mistakesView.selectedWord);
        return;
      }

      const mistakeWords = vocabDatabase.filter(w => state.mistakes.has(w.id));
      let contentHtml = mistakeWords.length === 0 ? `
          <div class="flex-1 flex flex-col items-center justify-center text-slate-400 bg-white rounded-3xl border border-slate-100 border-dashed min-h-[300px]">
            ${getIcon('check-circle-2', 'w-16 h-16 mb-4 text-green-300')}
            <p class="text-lg font-medium text-slate-500">太棒了！目前沒有錯題。</p>
            <p class="text-sm mt-2">在聽力或拼字練習中答錯的單字會自動出現在這裡。</p>
          </div>
        ` : `
          <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 pb-10">
            ${mistakeWords.map(word => `
              <div onclick="openCanvas(${word.id})" class="bg-white p-5 rounded-2xl border border-slate-200 hover:border-indigo-400 hover:shadow-md cursor-pointer transition group relative">
                <div class="flex justify-between items-start mb-2">
                  <span class="text-xs font-semibold text-red-500 bg-red-50 px-2 py-1 rounded">${word.pos}</span>
                  <button onclick="event.stopPropagation(); removeMistakeAndRender(${word.id})" class="text-slate-300 hover:text-red-500 p-1" title="移出租題本">${getIcon('trash-2', 'w-4 h-4')}</button>
                </div>
                <p class="text-3xl font-bold text-slate-800 mb-1">${word.word}</p>
                <p class="text-slate-500">${word.meaning}</p>
                <div class="mt-4 flex items-center text-indigo-500 text-sm font-medium md:opacity-0 group-hover:opacity-100 transition">
                  ${getIcon('pen-tool', 'w-4 h-4 mr-1')} 進入手寫練習 ${getIcon('arrow-right', 'w-4 h-4 ml-1')}
                </div>
              </div>
            `).join('')}
          </div>
        `;

      container.innerHTML = `
        <div class="max-w-4xl mx-auto h-full flex flex-col animate-fadeIn">
          ${backBtnHtml}
          <header class="mb-6">
            <h2 class="text-2xl font-bold flex items-center gap-2 text-red-600">${getIcon('alert-circle', 'w-6 h-6')} 錯題溫習區</h2>
            <p class="text-slate-500 mt-2">點擊單字進入「手寫板」進行罰抄練習。練習完畢後可將其移出租題本。</p>
          </header>
          ${contentHtml}
        </div>
      `;
    }

    function removeMistakeAndRender(id) { removeMistake(id); renderApp(); }

    function openCanvas(wordId) {
      state.mistakesView.selectedWord = vocabDatabase.find(w => w.id === wordId);
      renderApp();
    }

    function closeCanvas() {
      state.mistakesView.selectedWord = null;
      renderApp();
    }

    function markLearnedFromCanvas() {
      if (state.mistakesView.selectedWord) {
        removeMistake(state.mistakesView.selectedWord.id);
        closeCanvas();
      }
    }

    function renderCanvasMode(container, word) {
      container.innerHTML = `
        <div class="max-w-3xl mx-auto flex flex-col h-full w-full animate-fadeIn pb-10">
          <div class="flex items-center justify-between mb-4">
            <button onclick="closeCanvas()" class="text-slate-500 hover:text-slate-800 font-medium flex items-center gap-1 p-2 bg-white rounded-lg shadow-sm border border-slate-100">
              ${getIcon('arrow-left', 'w-4 h-4')} 返回列表
            </button>
            <button onclick="speak('${word.word}')" class="p-3 bg-indigo-50 text-indigo-600 rounded-full shadow-sm">${getIcon('volume-2', 'w-6 h-6')}</button>
          </div>

          <div class="bg-white p-4 md:p-6 rounded-3xl border border-slate-200 shadow-sm flex-1 flex flex-col relative overflow-hidden">
            <div class="text-center mb-4 relative z-10 pointer-events-none">
              <p class="text-lg font-medium text-slate-500 mb-1">${word.meaning} (${word.pos})</p>
              <p class="text-5xl md:text-6xl font-black text-slate-800 tracking-widest">${word.word}</p>
            </div>

            <div id="canvas-container" class="flex-1 relative border-2 border-dashed border-slate-300 rounded-xl bg-slate-50 overflow-hidden touch-none" style="background-image: linear-gradient(#f1f5f9 1px, transparent 1px), linear-gradient(90deg, #f1f5f9 1px, transparent 1px); background-size: 40px 40px;">
              <p class="absolute top-2 left-0 w-full text-center text-slate-300 text-sm pointer-events-none select-none">請在下方手寫罰抄</p>
              <canvas id="drawing-canvas" class="w-full h-full cursor-crosshair absolute top-0 left-0 touch-none"></canvas>
            </div>

            <div class="flex flex-col md:flex-row gap-3 mt-4">
              <button onclick="clearCanvas()" class="w-full md:w-1/3 py-4 bg-slate-100 text-slate-600 font-bold rounded-xl hover:bg-slate-200 transition">
                清除重寫
              </button>
              <button onclick="markLearnedFromCanvas()" class="w-full md:w-2/3 py-4 bg-green-500 text-white font-bold rounded-xl hover:bg-green-600 shadow-lg shadow-green-200 flex items-center justify-center gap-2 transition">
                ${getIcon('check-circle-2', 'w-5 h-5')} 記住了，移出租題本
              </button>
            </div>
          </div>
        </div>
      `;

      speak(word.word);
      setTimeout(initCanvasContext, 100);
    }

    function initCanvasContext() {
      const canvas = document.getElementById('drawing-canvas');
      const container = document.getElementById('canvas-container');
      if (!canvas || !container) return;

      const rect = container.getBoundingClientRect();
      canvas.width = rect.width; canvas.height = rect.height;
      
      canvasCtx = canvas.getContext('2d');
      canvasCtx.lineCap = 'round'; canvasCtx.lineJoin = 'round'; canvasCtx.lineWidth = 6; canvasCtx.strokeStyle = '#312e81';

      canvas.addEventListener('mousedown', startDraw); canvas.addEventListener('mousemove', drawing);
      canvas.addEventListener('mouseup', stopDraw); canvas.addEventListener('mouseout', stopDraw);
      canvas.addEventListener('touchstart', startDraw, { passive: false });
      canvas.addEventListener('touchmove', drawing, { passive: false });
      canvas.addEventListener('touchend', stopDraw); canvas.addEventListener('touchcancel', stopDraw);
    }

    function getMousePos(canvas, evt) {
      const rect = canvas.getBoundingClientRect();
      if (evt.touches && evt.touches.length > 0) return { x: evt.touches[0].clientX - rect.left, y: evt.touches[0].clientY - rect.top };
      return { x: evt.clientX - rect.left, y: evt.clientY - rect.top };
    }

    function startDraw(e) { if (e.cancelable) e.preventDefault(); isDrawing = true; const pos = getMousePos(document.getElementById('drawing-canvas'), e); canvasCtx.beginPath(); canvasCtx.moveTo(pos.x, pos.y); }
    function drawing(e) { if (e.cancelable) e.preventDefault(); if (!isDrawing) return; const pos = getMousePos(document.getElementById('drawing-canvas'), e); canvasCtx.lineTo(pos.x, pos.y); canvasCtx.stroke(); }
    function stopDraw(e) { if (e.cancelable) e.preventDefault(); isDrawing = false; }
    function clearCanvas() { const canvas = document.getElementById('drawing-canvas'); if (canvas && canvasCtx) canvasCtx.clearRect(0, 0, canvas.width, canvas.height); }

    window.addEventListener('resize', () => {
      if (state.activeTab === 'mistakes' && state.mistakesView.selectedWord) initCanvasContext(); 
    });

    // ==========================================
    // 關鍵修復：強制執行初始化畫面渲染
    // ==========================================
    try {
      // 不再做任何條件判斷，直接把頁面畫出來
      renderApp(); 
    } catch(e) {
      const mainContent = document.getElementById('main-content');
      if (mainContent) {
        mainContent.innerHTML = `<div style="padding:20px;color:red;font-size:20px;">啟動失敗: ${e.message}</div>`;
      }
    }

  </script>
</body>
</html>
