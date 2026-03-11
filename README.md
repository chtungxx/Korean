[Kr7.html](https://github.com/user-attachments/files/25892561/Kr7.html)
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
      background-color: #f8fafc; /* slate-50 */
    }
    .touch-none {
      touch-action: none;
    }
    
    /* 淡入動畫 */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(15px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .animate-fadeIn {
      animation: fadeIn 0.3s cubic-bezier(0.4, 0, 0.2, 1) forwards;
    }
    
    @keyframes popIn {
      from { opacity: 0; transform: scale(0.95); }
      to { opacity: 1; transform: scale(1); }
    }
    .animate-popIn {
      animation: popIn 0.3s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
    }

    /* App 風格的捲軸 */
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
<!-- 整個畫面就是一個可以滾動的容器，不再分割左右/上下 -->
<body class="font-sans text-slate-800 h-[100dvh] w-full overflow-y-auto" id="app-container">

  <script>
    // ==========================================
    // 內建 SVG 圖標庫 (保證不白屏)
    // ==========================================
    const ICONS = {
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

    // 所有子頁面共用的超大返回按鈕
    const backBtnHtml = `
      <div class="w-full mb-6">
        <button onclick="switchTab('dashboard')" class="inline-flex items-center gap-2 px-5 py-3 bg-white hover:bg-indigo-50 text-slate-600 hover:text-indigo-700 font-bold rounded-2xl shadow-sm border border-slate-200 transition-all active:scale-95">
          ${getIcon('arrow-left', 'w-5 h-5')} 返回主選單
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

    function switchTab(tabId) {
      state.activeTab = tabId;
      state.mistakesView.selectedWord = null;
      
      if (tabId === 'vocab' && !state.vocab.initialized) state.vocab.initialized = true;
      if (tabId === 'listening') initListening();
      if (tabId === 'writing') initWriting();
      if (tabId === 'speaking') initSpeaking();

      // 每次切換分頁，自動滾動到最頂
      document.getElementById('app-container').scrollTo(0,0);
      renderApp();
    }

    function addMistake(id) { state.mistakes.add(id); }
    function removeMistake(id) { state.mistakes.delete(id); }

    function renderApp() {
      const container = document.getElementById('app-container');
      if (!container) return;
      
      // 統一的內容包裝器
      container.innerHTML = `<div id="content-wrapper" class="w-full max-w-3xl mx-auto p-4 md:p-8 min-h-[100dvh] flex flex-col justify-center"></div>`;
      const wrapper = document.getElementById('content-wrapper');

      switch (state.activeTab) {
        case 'dashboard': renderDashboard(wrapper); break;
        case 'vocab': renderVocab(wrapper); break;
        case 'listening': renderListening(wrapper); break;
        case 'writing': renderWriting(wrapper); break;
        case 'speaking': renderSpeaking(wrapper); break;
        case 'mistakes': renderMistakes(wrapper); break;
      }
    }

    // ==========================================
    // 全新：控制中心主頁 (Dashboard)
    // ==========================================
    function renderDashboard(container) {
      container.innerHTML = `
        <div class="animate-popIn w-full">
          <!-- 標題區 -->
          <div class="text-center mb-10">
            <div class="inline-block px-4 py-1.5 bg-indigo-600 text-white font-bold rounded-full text-sm mb-3 tracking-widest">PRO</div>
            <h1 class="text-4xl md:text-5xl font-black text-slate-800 tracking-tight">TOPIK 1</h1>
            <p class="text-slate-500 mt-2 font-medium text-lg">韓語學習衝刺班</p>
          </div>

          <!-- 精簡學習進度 -->
          <div class="flex justify-between items-center bg-white p-5 rounded-3xl shadow-sm border border-slate-100 mb-8 mx-2">
            <div class="text-center flex-1">
              <div class="text-xs text-slate-400 font-bold mb-1">已學</div>
              <div class="font-black text-xl text-blue-600">${state.stats.vocabLearned}</div>
            </div>
            <div class="w-px h-8 bg-slate-100"></div>
            <div class="text-center flex-1">
              <div class="text-xs text-slate-400 font-bold mb-1">聽力</div>
              <div class="font-black text-xl text-purple-600">${state.stats.listeningScore}</div>
            </div>
            <div class="w-px h-8 bg-slate-100"></div>
            <div class="text-center flex-1">
              <div class="text-xs text-slate-400 font-bold mb-1">拼字</div>
              <div class="font-black text-xl text-green-600">${state.stats.writingScore}</div>
            </div>
            <div class="w-px h-8 bg-slate-100"></div>
            <div class="text-center flex-1">
              <div class="text-xs text-slate-400 font-bold mb-1">口說</div>
              <div class="font-black text-xl text-orange-600">${state.stats.speakingScore}</div>
            </div>
          </div>

          <!-- 九宮格大按鈕選單 -->
          <div class="grid grid-cols-2 gap-4 px-2">
            
            <button onclick="switchTab('vocab')" class="bg-white border-2 border-blue-100 hover:border-blue-400 p-6 rounded-[2rem] flex flex-col items-center justify-center gap-4 transition-all active:scale-95 shadow-sm hover:shadow-md">
              <div class="p-4 bg-blue-50 text-blue-600 rounded-full">${getIcon('book-open', 'w-8 h-8')}</div>
              <span class="font-bold text-slate-700 text-lg">詞彙記憶</span>
            </button>

            <button onclick="switchTab('listening')" class="bg-white border-2 border-purple-100 hover:border-purple-400 p-6 rounded-[2rem] flex flex-col items-center justify-center gap-4 transition-all active:scale-95 shadow-sm hover:shadow-md">
              <div class="p-4 bg-purple-50 text-purple-600 rounded-full">${getIcon('headphones', 'w-8 h-8')}</div>
              <span class="font-bold text-slate-700 text-lg">聽力測驗</span>
            </button>

            <button onclick="switchTab('writing')" class="bg-white border-2 border-green-100 hover:border-green-400 p-6 rounded-[2rem] flex flex-col items-center justify-center gap-4 transition-all active:scale-95 shadow-sm hover:shadow-md">
              <div class="p-4 bg-green-50 text-green-600 rounded-full">${getIcon('edit-3', 'w-8 h-8')}</div>
              <span class="font-bold text-slate-700 text-lg">無鍵盤拼字</span>
            </button>

            <button onclick="switchTab('speaking')" class="bg-white border-2 border-orange-100 hover:border-orange-400 p-6 rounded-[2rem] flex flex-col items-center justify-center gap-4 transition-all active:scale-95 shadow-sm hover:shadow-md">
              <div class="p-4 bg-orange-50 text-orange-600 rounded-full">${getIcon('mic', 'w-8 h-8')}</div>
              <span class="font-bold text-slate-700 text-lg">口說評分</span>
            </button>

            <!-- 錯題本 (橫跨兩欄) -->
            <button onclick="switchTab('mistakes')" class="col-span-2 bg-slate-800 text-white border-2 border-slate-800 hover:bg-slate-700 p-6 rounded-[2rem] flex flex-row items-center justify-center gap-4 transition-all active:scale-95 shadow-lg relative overflow-hidden">
              <div class="absolute -right-4 -bottom-4 opacity-10 pointer-events-none">${getIcon('alert-circle', 'w-32 h-32')}</div>
              <div class="p-3 bg-white/20 rounded-full">${getIcon('pen-tool', 'w-6 h-6 text-white')}</div>
              <div class="text-left">
                <span class="font-bold text-xl block">錯題手寫板</span>
                <span class="text-sm text-slate-300">目前累積 ${state.mistakes.size} 個生字</span>
              </div>
              ${state.mistakes.size > 0 ? `<div class="absolute top-6 right-6 w-4 h-4 bg-red-500 rounded-full animate-pulse border-2 border-slate-800"></div>` : ''}
            </button>

          </div>
        </div>
      `;
    }

    // ==========================================
    // 詞彙記憶卡
    // ==========================================
    function renderVocab(container) {
      const word = vocabDatabase[state.vocab.currentIndex];
      
      let contentHtml = state.vocab.showMeaning ? `
          <div class="text-center animate-fadeIn w-full mt-8">
            <p class="text-2xl text-slate-700 font-medium mb-6">${word.meaning}</p>
            <div class="bg-slate-50 p-5 rounded-2xl border border-slate-100">
              <p class="text-lg mb-2 text-slate-800">${word.example}</p>
              <p class="text-sm text-slate-500">${word.exampleMeaning}</p>
            </div>
          </div>
        ` : `<p class="text-slate-400 text-sm mt-12 animate-pulse">點擊卡片查看解釋與例句</p>`;

      container.innerHTML = `
        <div class="w-full flex-1 flex flex-col items-center justify-start animate-fadeIn py-6">
          ${backBtnHtml}
          
          <div class="w-full flex justify-between items-center mb-6 px-2">
            <h2 class="text-2xl font-black text-slate-800">詞彙記憶卡</h2>
            <span class="text-slate-500 bg-white px-4 py-1.5 rounded-full text-sm font-bold border border-slate-200 shadow-sm">${state.vocab.currentIndex + 1} <span class="text-slate-300">/</span> ${vocabDatabase.length}</span>
          </div>
          
          <div onclick="toggleVocabMeaning()" class="w-full min-h-[300px] bg-white rounded-[2.5rem] shadow-sm border border-slate-200 flex flex-col items-center justify-center p-8 cursor-pointer relative select-none transition-transform active:scale-[0.98]">
            <button onclick="event.stopPropagation(); speak('${word.word}')" class="absolute top-6 right-6 p-4 bg-indigo-50 text-indigo-600 rounded-full hover:bg-indigo-100 transition">
              ${getIcon('volume-2', 'w-6 h-6')}
            </button>
            <span class="text-sm font-bold text-indigo-500 bg-indigo-50 px-4 py-1.5 rounded-full mb-6">${word.pos}</span>
            <h3 class="text-6xl md:text-7xl font-black text-slate-800 text-center tracking-wide">${word.word}</h3>
            ${contentHtml}
          </div>
          
          <div class="flex gap-4 mt-8 w-full">
            <button onclick="prevVocab()" class="flex-1 py-5 bg-white border-2 border-slate-200 rounded-2xl font-bold text-slate-600 hover:bg-slate-50 transition active:scale-95 text-lg">上一張</button>
            <button onclick="nextVocab()" class="flex-1 py-5 bg-indigo-600 rounded-2xl font-bold text-white hover:bg-indigo-700 transition shadow-lg shadow-indigo-200 active:scale-95 text-lg">下一張</button>
          </div>
        </div>
      `;
    }

    function toggleVocabMeaning() { state.vocab.showMeaning = !state.vocab.showMeaning; renderApp(); }
    function nextVocab() { state.vocab.showMeaning = false; state.vocab.currentIndex = (state.vocab.currentIndex + 1) % vocabDatabase.length; state.stats.vocabLearned++; renderApp(); }
    function prevVocab() { state.vocab.showMeaning = false; state.vocab.currentIndex = (state.vocab.currentIndex - 1 + vocabDatabase.length) % vocabDatabase.length; renderApp(); }

    // ==========================================
    // 聽力測驗
    // ==========================================
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
        let btnClass = "py-5 px-6 bg-white border-2 rounded-2xl text-xl font-bold transition-all text-slate-700 ";
        let disabled = selected !== null ? "disabled" : "";
        if (!selected) btnClass += "border-slate-200 hover:border-indigo-400 active:scale-95 shadow-sm";
        else if (opt.id === question.id) btnClass += "border-green-500 bg-green-50 text-green-700 shadow-sm";
        else if (selected.id === opt.id) btnClass += "border-red-500 bg-red-50 text-red-700 shadow-sm";
        else btnClass += "border-slate-100 opacity-40";

        return `<button onclick="handleListenSelect(${idx})" class="${btnClass}" ${disabled}>${opt.meaning}</button>`;
      }).join('');

      let resultHtml = selected ? `
          <div class="mt-8 flex flex-col items-center animate-fadeIn w-full bg-slate-100 p-6 rounded-3xl">
            <p class="text-slate-500 font-bold mb-2">正確答案是</p>
            <p class="text-slate-800 mb-6 text-center"><span class="font-black text-4xl">${question.word}</span></p>
            <button onclick="nextListen()" class="w-full py-4 bg-indigo-600 text-white rounded-2xl font-bold text-lg hover:bg-indigo-700 flex justify-center items-center gap-2 shadow-lg active:scale-95">下一題 ${getIcon('arrow-right', 'w-5 h-5')}</button>
          </div>
        ` : '';

      container.innerHTML = `
        <div class="w-full flex-1 flex flex-col items-center justify-start animate-fadeIn py-6">
          ${backBtnHtml}
          <div class="w-full px-2 mb-10 flex flex-col items-center">
             <h2 class="text-2xl font-black text-slate-800 mb-2">聽力測驗</h2>
             <p class="text-slate-500 text-sm font-medium">點擊播放發音，選出正確的中文意思</p>
          </div>
          
          <button onclick="speak('${question.word}')" class="w-32 h-32 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center hover:bg-indigo-200 transition-transform active:scale-90 shadow-inner mb-12 border-4 border-white outline outline-4 outline-indigo-50">
            ${getIcon('volume-2', 'w-14 h-14 ml-2')}
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

    // ==========================================
    // 拼字練習 (無鍵盤)
    // ==========================================
    function initWriting() {
      const randomWord = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
      state.write.question = randomWord;
      const targetChars = randomWord.word.split('');
      const allChars = vocabDatabase.map(w => w.word).join('').split('');
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

      let hintHtml = hintLevel === 1 ? `<div class="text-indigo-600 bg-indigo-50 px-5 py-2 rounded-xl font-bold text-xl border border-indigo-100">初聲提示：${getChoSeong(question.word)}</div>` :
                     hintLevel === 2 ? `<div class="text-red-500 bg-red-50 px-5 py-2 rounded-xl font-bold text-xl border border-red-100">答案：${question.word}</div>` : '';

      let slotsClass = status === 'wrong' ? 'bg-red-50 border-red-300 shadow-red-100' : status === 'correct' ? 'bg-green-50 border-green-400 shadow-green-100' : 'bg-slate-100 border-slate-200';
      let answerHtml = answer.map((item, idx) => {
        let boxClass = item ? 'bg-indigo-600 text-white shadow-md cursor-pointer hover:bg-indigo-700 border border-indigo-700' : 'bg-white border-2 border-dashed border-slate-300 text-transparent';
        return `<div onclick="handleWriteAnswerClick(${idx})" class="w-16 h-16 md:w-20 md:h-20 flex items-center justify-center text-3xl font-black rounded-2xl transition-all active:scale-95 ${boxClass}">${item ? item.char : '空'}</div>`;
      }).join('');

      let poolHtml = status === 'playing' ? `
          <div class="flex flex-wrap justify-center gap-3 mb-8">
            ${pool.map((item, idx) => `
              <button onclick="handleWritePoolClick(${idx})" class="w-16 h-16 md:w-16 md:h-16 bg-white border-2 border-slate-200 rounded-2xl text-2xl font-black text-slate-700 hover:border-indigo-400 hover:text-indigo-600 hover:-translate-y-1 transition-all shadow-sm active:scale-90">${item.char}</button>
            `).join('')}
          </div>
        ` : '';

      let footerHtml = '';
      if (status === 'playing') {
        let btnText = hintLevel === 0 ? '需要提示？' : hintLevel === 1 ? '直接看答案' : '已顯示答案';
        let btnClass = hintLevel === 2 ? 'bg-slate-200 text-slate-400' : 'bg-slate-800 text-white hover:bg-slate-700 shadow-md active:scale-95';
        footerHtml = `<button type="button" onclick="requestWriteHint()" ${hintLevel === 2 ? 'disabled' : ''} class="px-8 py-3 rounded-full font-bold transition ${btnClass}">${btnText}</button>`;
      } else if (status === 'wrong') {
        footerHtml = `
          <div class="animate-fadeIn w-full flex flex-col items-center">
            <p class="text-red-500 font-bold mb-4 flex items-center justify-center text-lg">${getIcon('x-circle', 'mr-2 w-6 h-6')} 拼錯囉，請重試！</p>
            <button onclick="resetWriteBoard()" class="px-8 py-4 bg-slate-200 text-slate-700 rounded-2xl hover:bg-slate-300 font-black shadow-sm active:scale-95 w-full">清空重拼</button>
          </div>
        `;
      } else if (status === 'correct') {
        footerHtml = `
          <div class="animate-fadeIn w-full flex flex-col items-center">
            <p class="text-green-600 font-bold text-xl mb-6 flex items-center gap-2">${getIcon('check-circle-2', 'w-8 h-8')} 完全正確！</p>
            <button onclick="nextWrite()" class="w-full py-5 bg-green-500 text-white rounded-2xl font-black text-xl hover:bg-green-600 transition-colors flex justify-center items-center gap-2 shadow-lg active:scale-95">
              挑戰下一題 ${getIcon('arrow-right', 'w-6 h-6')}
            </button>
          </div>
        `;
      }

      container.innerHTML = `
        <div class="w-full flex-1 flex flex-col items-center justify-start animate-fadeIn py-6">
          ${backBtnHtml}
          
          <div class="w-full bg-white p-6 md:p-10 rounded-[2.5rem] shadow-sm border border-slate-200 text-center relative overflow-hidden">
            <h2 class="text-xl font-bold text-slate-400 mb-2">請拼出以下單字</h2>
            <div class="flex justify-center items-end gap-3 mb-8">
              <p class="text-5xl font-black text-slate-800 tracking-wide">${question.meaning}</p>
              <span class="text-sm font-bold text-slate-400 bg-slate-100 px-3 py-1 rounded-lg mb-1">${question.pos}</span>
            </div>
            
            <div class="h-12 mb-8 flex items-center justify-center">${hintHtml}</div>
            
            <!-- 答案放置區 -->
            <div class="flex justify-center gap-3 mb-10 p-5 rounded-3xl border-2 shadow-inner ${slotsClass}">${answerHtml}</div>
            
            <!-- 字元選擇區 -->
            ${poolHtml}
            
            <div class="flex flex-col items-center min-h-[6rem] w-full">${footerHtml}</div>
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

    // ==========================================
    // 口說練習
    // ==========================================
    let recognitionInstance = null;
    function initSpeaking() {
      state.speak.question = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
      state.speak.transcript = ''; state.speak.score = null; state.speak.isRecording = false; state.speak.errorMsg = '';
      if(recognitionInstance) try { recognitionInstance.stop(); } catch(e){}
    }

    function renderSpeaking(container) {
      if (!state.speak.question) return;
      const { question, isRecording, transcript, score, errorMsg } = state.speak;

      let btnClass = isRecording ? 'bg-red-500 animate-pulse scale-110 shadow-xl shadow-red-200 border-4 border-red-200' : 'bg-indigo-600 hover:bg-indigo-700 shadow-xl shadow-indigo-200 border-4 border-white outline outline-4 outline-indigo-50 active:scale-95';
      let resultHtml = '';
      
      if (transcript) {
        let scoreHtml = score !== null ? `
            <div class="flex flex-col items-center mt-4">
              <div class="text-sm font-bold text-slate-400 mb-1">AI 發音評分</div>
              <div class="text-6xl font-black mb-8 ${score >= 80 ? 'text-green-500' : score >= 50 ? 'text-orange-500' : 'text-red-500'}">${score} <span class="text-2xl text-slate-300">/ 100</span></div>
              <button onclick="nextSpeak()" class="w-full py-4 bg-slate-800 text-white rounded-2xl font-bold text-lg hover:bg-slate-700 flex justify-center items-center gap-2 active:scale-95 shadow-md">挑戰下一句 ${getIcon('arrow-right', 'w-5 h-5')}</button>
            </div>
          ` : '';
        resultHtml = `
          <div class="mt-8 w-full bg-white border-2 border-slate-100 p-6 rounded-[2rem] shadow-sm animate-fadeIn text-center">
            <p class="text-sm text-slate-400 font-bold mb-2">系統辨識結果：</p>
            <p class="text-2xl font-black mb-4 text-slate-700">"${transcript}"</p>
            <div class="w-full h-px bg-slate-100 mb-4"></div>
            ${scoreHtml}
          </div>
        `;
      }

      container.innerHTML = `
        <div class="w-full flex-1 flex flex-col items-center justify-start animate-fadeIn py-6">
          ${backBtnHtml}
          
          <div class="w-full px-2 mb-6 flex flex-col items-center">
             <h2 class="text-2xl font-black text-slate-800 mb-2">口說發音評分</h2>
          </div>

          <div class="bg-white p-8 rounded-[2.5rem] shadow-sm border border-slate-200 w-full mb-10 relative text-center">
            <p class="text-sm font-black text-indigo-500 bg-indigo-50 inline-block px-4 py-1.5 rounded-full mb-4">目標句 (${question.word} - ${question.meaning})</p>
            <p class="text-4xl font-black text-slate-800 mb-4 leading-tight">${question.example}</p>
            <p class="text-slate-500 font-medium mb-8">${question.exampleMeaning}</p>
            <button onclick="speak('${question.example}')" class="inline-flex items-center justify-center gap-2 px-6 py-3 bg-slate-100 text-slate-600 font-bold rounded-full hover:bg-slate-200 transition active:scale-95 w-full md:w-auto">
              ${getIcon('volume-2', 'w-5 h-5')} 聽老師讀一次
            </button>
          </div>
          
          <button onclick="handleSpeakClick()" ${isRecording ? 'disabled' : ''} class="w-28 h-28 rounded-full flex items-center justify-center text-white transition-all ${btnClass}">
            ${getIcon('mic', 'w-12 h-12')}
          </button>
          <p class="mt-6 text-sm font-bold h-6 text-slate-400 tracking-widest">${isRecording ? '正在聆聽...' : '點擊麥克風開始說話'}</p>
          
          ${errorMsg ? `<div class="mt-4 bg-red-50 text-red-600 p-4 rounded-xl text-sm font-bold border border-red-100 flex items-center gap-2 w-full justify-center">${getIcon('alert-circle', 'w-5 h-5')} ${errorMsg}</div>` : ''}
          ${resultHtml}
        </div>
      `;
    }

    function handleSpeakClick() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) { state.speak.errorMsg = "非常抱歉，當前環境 (如 Koder) 不支援錄音。請在 Safari/Chrome 瀏覽器中打開。"; renderApp(); return; }
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

    // ==========================================
    // 錯題本與手寫板 
    // ==========================================
    let canvasCtx = null;
    let isDrawing = false;

    function renderMistakes(container) {
      if (state.mistakesView.selectedWord) {
        renderCanvasMode(container, state.mistakesView.selectedWord);
        return;
      }

      const mistakeWords = vocabDatabase.filter(w => state.mistakes.has(w.id));
      let contentHtml = mistakeWords.length === 0 ? `
          <div class="flex-1 flex flex-col items-center justify-center text-slate-400 bg-white rounded-[2.5rem] border border-slate-100 border-dashed min-h-[400px] shadow-sm">
            <div class="p-6 bg-green-50 text-green-500 rounded-full mb-6">${getIcon('check-circle-2', 'w-16 h-16')}</div>
            <p class="text-xl font-black text-slate-600 mb-2">太棒了！目前沒有錯題。</p>
            <p class="text-sm font-medium text-center px-8">在聽力或拼字練習中答錯、或使用過提示的單字，會自動被收錄到這裡。</p>
          </div>
        ` : `
          <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 pb-10">
            ${mistakeWords.map(word => `
              <div onclick="openCanvas(${word.id})" class="bg-white p-6 rounded-3xl border border-slate-200 hover:border-red-300 hover:shadow-md cursor-pointer transition-all active:scale-95 relative group">
                <div class="flex justify-between items-start mb-3">
                  <span class="text-xs font-black text-red-600 bg-red-50 px-3 py-1 rounded-full">${word.pos}</span>
                  <button onclick="event.stopPropagation(); removeMistakeAndRender(${word.id})" class="text-slate-300 hover:text-red-500 p-2 bg-slate-50 rounded-full transition-colors" title="移出租題本">${getIcon('trash-2', 'w-4 h-4')}</button>
                </div>
                <p class="text-4xl font-black text-slate-800 mb-2">${word.word}</p>
                <p class="text-slate-500 font-medium">${word.meaning}</p>
                <div class="mt-5 flex items-center text-red-500 text-sm font-bold bg-red-50 py-2 px-3 rounded-xl justify-center group-hover:bg-red-100 transition-colors">
                  ${getIcon('pen-tool', 'w-4 h-4 mr-2')} 進入手寫罰抄
                </div>
              </div>
            `).join('')}
          </div>
        `;

      container.innerHTML = `
        <div class="w-full flex-1 flex flex-col justify-start animate-fadeIn py-6">
          ${backBtnHtml}
          <header class="mb-8 px-2 flex items-center gap-3">
            <div class="p-3 bg-red-50 text-red-600 rounded-xl">${getIcon('alert-circle', 'w-8 h-8')}</div>
            <div>
              <h2 class="text-2xl font-black text-slate-800">錯題溫習區</h2>
              <p class="text-slate-500 font-medium text-sm mt-1">點擊單字進入「手寫板」進行罰抄練習</p>
            </div>
          </header>
          ${contentHtml}
        </div>
      `;
    }

    function removeMistakeAndRender(id) { removeMistake(id); renderApp(); }
    function openCanvas(wordId) { state.mistakesView.selectedWord = vocabDatabase.find(w => w.id === wordId); renderApp(); }
    function closeCanvas() { state.mistakesView.selectedWord = null; renderApp(); }
    function markLearnedFromCanvas() {
      if (state.mistakesView.selectedWord) {
        removeMistake(state.mistakesView.selectedWord.id);
        closeCanvas();
      }
    }

    function renderCanvasMode(container, word) {
      container.innerHTML = `
        <div class="w-full flex-1 flex flex-col justify-start animate-fadeIn py-6">
          
          <div class="flex items-center justify-between mb-6">
            <button onclick="closeCanvas()" class="text-slate-600 hover:text-slate-800 font-bold flex items-center gap-2 py-3 px-5 bg-white rounded-2xl shadow-sm border border-slate-200 active:scale-95 transition-transform">
              ${getIcon('arrow-left', 'w-5 h-5')} 返回列表
            </button>
            <button onclick="speak('${word.word}')" class="p-4 bg-indigo-50 text-indigo-600 rounded-2xl shadow-sm hover:bg-indigo-100 active:scale-95 transition-transform">${getIcon('volume-2', 'w-6 h-6')}</button>
          </div>

          <div class="bg-white p-6 rounded-[2.5rem] shadow-sm border border-slate-200 flex-1 flex flex-col relative overflow-hidden min-h-[500px]">
            
            <div class="text-center mb-6 relative z-10 pointer-events-none">
              <span class="text-sm font-black text-red-500 bg-red-50 px-4 py-1.5 rounded-full mb-3 inline-block">${word.pos}</span>
              <p class="text-6xl md:text-7xl font-black text-slate-800 tracking-widest drop-shadow-sm">${word.word}</p>
              <p class="text-xl font-bold text-slate-500 mt-2">${word.meaning}</p>
            </div>

            <!-- 畫布區 -->
            <div id="canvas-container" class="flex-1 relative border-4 border-slate-100 rounded-3xl overflow-hidden touch-none" style="background-color: #f8fafc; background-image: linear-gradient(#e2e8f0 2px, transparent 2px), linear-gradient(90deg, #e2e8f0 2px, transparent 2px); background-size: 60px 60px; background-position: center center;">
              <p class="absolute top-4 left-0 w-full text-center text-slate-400 font-bold pointer-events-none select-none opacity-50">請在方格內手寫罰抄</p>
              <canvas id="drawing-canvas" class="w-full h-full cursor-crosshair absolute top-0 left-0 touch-none"></canvas>
            </div>

            <div class="flex flex-col md:flex-row gap-3 mt-6">
              <button onclick="clearCanvas()" class="w-full md:w-1/3 py-4 bg-slate-100 text-slate-600 font-black text-lg rounded-2xl hover:bg-slate-200 transition active:scale-95">
                清除重寫
              </button>
              <button onclick="markLearnedFromCanvas()" class="w-full md:w-2/3 py-4 bg-slate-800 text-white font-black text-lg rounded-2xl hover:bg-slate-700 shadow-xl flex items-center justify-center gap-2 transition active:scale-95">
                ${getIcon('check-circle-2', 'w-6 h-6 text-green-400')} 記住了，移出租題本
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
      canvasCtx.lineCap = 'round'; canvasCtx.lineJoin = 'round'; 
      canvasCtx.lineWidth = 8; /* 加粗筆觸 */
      canvasCtx.strokeStyle = '#1e293b'; /* 深石板灰，更像墨水 */

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
    // 啟動應用程式
    // ==========================================
    renderApp(); 

  </script>
</body>
</html>
