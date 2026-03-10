[Kr.html](https://github.com/user-attachments/files/25872349/Kr.html)
import React, { useState, useEffect, useRef } from 'react';
import { BookOpen, Headphones, Edit3, Mic, Home, Volume2, CheckCircle2, XCircle, ArrowRight, RefreshCw, Info, HelpCircle, PenTool, Trash2, AlertCircle } from 'lucide-react';

// ==========================================
// 1. TOPIK 1 擴充詞彙資料庫 (60個高頻核心字)
// ==========================================
const vocabDatabase = [
  // 形容詞 (相反詞與常見)
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

  // 動詞 (日常與考試高頻)
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
  { id: 50, word: '도와주다', meaning: '幫忙', pos: '動詞', 단어: '도와주다', example: '저 좀 도와주세요.', exampleMeaning: '請幫幫我。' },

  // 副詞與其他 (極常考)
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

// 提取韓文初聲
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

export default function App() {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [stats, setStats] = useState({ vocabLearned: 0, listeningScore: 0, writingScore: 0, speakingScore: 0 });
  const [mistakes, setMistakes] = useState(new Set()); // 儲存錯題的 ID

  const addMistake = (id) => {
    setMistakes(prev => new Set([...prev, id]));
  };

  const removeMistake = (id) => {
    setMistakes(prev => {
      const newSet = new Set(prev);
      newSet.delete(id);
      return newSet;
    });
  };

  const speak = (text) => {
    if ('speechSynthesis' in window) {
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = 'ko-KR';
      utterance.rate = 0.8; 
      window.speechSynthesis.speak(utterance);
    }
  };

  return (
    <div className="flex flex-col md:flex-row h-screen bg-slate-50 font-sans text-slate-800">
      <nav className="w-full md:w-64 bg-white shadow-lg flex md:flex-col justify-around md:justify-start p-4 shrink-0 z-10 overflow-x-auto">
        <div className="hidden md:flex items-center gap-3 mb-8 px-2">
          <div className="w-10 h-10 bg-indigo-600 rounded-xl flex items-center justify-center text-white font-bold text-xl shrink-0">
            PRO
          </div>
          <h1 className="text-xl font-bold text-slate-800 tracking-tight shrink-0">TOPIK 1<br/><span className="text-sm text-slate-500 font-normal">旗艦版</span></h1>
        </div>

        <div className="flex md:flex-col gap-2 min-w-max md:min-w-0">
          <NavItem icon={<Home />} label="主頁" isActive={activeTab === 'dashboard'} onClick={() => setActiveTab('dashboard')} />
          <NavItem icon={<BookOpen />} label="詞彙" isActive={activeTab === 'vocab'} onClick={() => setActiveTab('vocab')} />
          <NavItem icon={<Headphones />} label="聽力" isActive={activeTab === 'listening'} onClick={() => setActiveTab('listening')} />
          <NavItem icon={<Edit3 />} label="書寫" isActive={activeTab === 'writing'} onClick={() => setActiveTab('writing')} />
          <NavItem icon={<Mic />} label="口說" isActive={activeTab === 'speaking'} onClick={() => setActiveTab('speaking')} />
          <NavItem icon={<AlertCircle />} label={`錯題本 (${mistakes.size})`} isActive={activeTab === 'mistakes'} onClick={() => setActiveTab('mistakes')} badge={mistakes.size} />
        </div>
      </nav>

      <main className="flex-1 p-4 md:p-8 overflow-y-auto">
        {activeTab === 'dashboard' && <Dashboard stats={stats} mistakesCount={mistakes.size} setActiveTab={setActiveTab} />}
        {activeTab === 'vocab' && <VocabPractice speak={speak} setStats={setStats} />}
        {activeTab === 'listening' && <ListeningPractice speak={speak} setStats={setStats} addMistake={addMistake} />}
        {activeTab === 'writing' && <WritingPractice setStats={setStats} speak={speak} addMistake={addMistake} />}
        {activeTab === 'speaking' && <SpeakingPractice speak={speak} setStats={setStats} />}
        {activeTab === 'mistakes' && <MistakeReview mistakes={mistakes} removeMistake={removeMistake} speak={speak} />}
      </main>
    </div>
  );
}

function NavItem({ icon, label, isActive, onClick, badge }) {
  return (
    <button
      onClick={onClick}
      className={`flex items-center gap-2 p-3 rounded-xl transition-all duration-200 w-full text-sm md:text-base whitespace-nowrap ${
        isActive ? 'bg-indigo-50 text-indigo-700 font-semibold' : 'text-slate-500 hover:bg-slate-100'
      }`}
    >
      <span className={isActive ? 'text-indigo-600' : 'text-slate-400'}>{icon}</span>
      <span className="flex-1 text-left">{label}</span>
      {badge > 0 && (
        <span className="bg-red-500 text-white text-xs font-bold px-2 py-0.5 rounded-full">{badge}</span>
      )}
    </button>
  );
}

function Dashboard({ stats, mistakesCount, setActiveTab }) {
  return (
    <div className="max-w-4xl mx-auto space-y-6">
      <header className="mb-8">
        <h2 className="text-3xl font-bold text-slate-800">歡迎來到旗艦版！🚀</h2>
        <p className="text-slate-500 mt-2">全新 60 個核心單字庫，加入「無鍵盤拼字」與「手寫板」功能。</p>
      </header>

      <div className="grid grid-cols-2 md:grid-cols-5 gap-4">
        <StatCard title="已學詞彙" value={stats.vocabLearned} icon={<BookOpen className="text-blue-500" />} bgColor="bg-blue-50" />
        <StatCard title="聽力得分" value={stats.listeningScore} icon={<Headphones className="text-purple-500" />} bgColor="bg-purple-50" />
        <StatCard title="書寫得分" value={stats.writingScore} icon={<Edit3 className="text-green-500" />} bgColor="bg-green-50" />
        <StatCard title="口說達標" value={stats.speakingScore} icon={<Mic className="text-orange-500" />} bgColor="bg-orange-50" />
        <div onClick={() => setActiveTab('mistakes')} className="bg-red-50 p-5 rounded-2xl shadow-sm border border-red-100 flex flex-col items-center text-center cursor-pointer hover:bg-red-100 transition">
          <div className="p-3 rounded-full bg-white text-red-500 mb-3"><AlertCircle /></div>
          <p className="text-red-600 text-sm font-medium">待複習錯題</p>
          <p className="text-2xl font-bold text-red-700">{mistakesCount}</p>
        </div>
      </div>

      <div className="mt-8 bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
        <h3 className="text-xl font-bold mb-4">功能捷徑</h3>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
          <ActionCard onClick={() => setActiveTab('writing')} title="無鍵盤拼字練習" desc="點擊方塊拼出單字，不再需要切換輸入法。" icon={<Edit3 size={24} />} color="indigo" />
          <ActionCard onClick={() => setActiveTab('mistakes')} title="錯題手寫板 (罰抄)" desc="親筆寫下錯過的單字，加深肌肉記憶。" icon={<PenTool size={24} />} color="red" />
        </div>
      </div>
    </div>
  );
}

function StatCard({ title, value, icon, bgColor }) {
  return (
    <div className="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 flex flex-col items-center text-center">
      <div className={`p-3 rounded-full ${bgColor} mb-3`}>{icon}</div>
      <p className="text-slate-500 text-sm font-medium">{title}</p>
      <p className="text-2xl font-bold text-slate-800">{value}</p>
    </div>
  );
}

function ActionCard({ onClick, title, desc, icon, color }) {
  const colors = {
    indigo: 'bg-indigo-50 text-indigo-600 hover:border-indigo-200',
    red: 'bg-red-50 text-red-600 hover:border-red-200'
  };
  return (
    <button onClick={onClick} className={`flex flex-col items-start p-6 rounded-2xl border-2 border-transparent transition-all text-left ${colors[color]}`}>
      <div className="mb-4">{icon}</div>
      <h4 className="font-bold text-lg mb-1">{title}</h4>
      <p className="text-sm opacity-80">{desc}</p>
    </button>
  );
}

// ==========================================
// 功能 1 & 2: 詞彙記憶與聽力 (簡化程式碼保留功能)
// ==========================================
function VocabPractice({ speak, setStats }) {
  const [currentIndex, setCurrentIndex] = useState(0);
  const [showMeaning, setShowMeaning] = useState(false);
  const word = vocabDatabase[currentIndex];

  const handleNext = () => { setShowMeaning(false); setCurrentIndex(p => (p + 1) % vocabDatabase.length); setStats(p => ({ ...p, vocabLearned: p.vocabLearned + 1 })); };
  const handlePrev = () => { setShowMeaning(false); setCurrentIndex(p => (p - 1 + vocabDatabase.length) % vocabDatabase.length); };

  return (
    <div className="max-w-2xl mx-auto flex flex-col items-center justify-center h-full">
      <div className="w-full flex justify-between items-center mb-6">
        <h2 className="text-2xl font-bold">詞彙記憶卡 (60字)</h2>
        <span className="text-slate-400 bg-white px-3 py-1 rounded-full text-sm border shadow-sm">{currentIndex + 1} / {vocabDatabase.length}</span>
      </div>
      <div className="w-full aspect-[4/3] md:aspect-[16/9] bg-white rounded-3xl shadow-md border border-slate-100 flex flex-col items-center justify-center p-8 cursor-pointer relative" onClick={() => setShowMeaning(!showMeaning)}>
        <button onClick={(e) => { e.stopPropagation(); speak(word.word); }} className="absolute top-6 right-6 p-3 bg-indigo-50 text-indigo-600 rounded-full hover:bg-indigo-100 transition"><Volume2 size={24} /></button>
        <span className="text-sm font-semibold text-indigo-500 bg-indigo-50 px-3 py-1 rounded-full mb-4">{word.pos}</span>
        <h3 className="text-5xl md:text-7xl font-bold text-slate-800 mb-6">{word.word}</h3>
        {showMeaning ? (
          <div className="text-center animate-fadeIn w-full">
            <p className="text-2xl text-slate-700 font-medium mb-6">{word.meaning}</p>
            <div className="bg-slate-50 p-4 rounded-xl border border-slate-100"><p className="text-lg mb-2">{word.example}</p><p className="text-sm text-slate-500">{word.exampleMeaning}</p></div>
          </div>
        ) : <p className="text-slate-400 text-sm mt-8 animate-pulse">點擊卡片查看解釋</p>}
      </div>
      <div className="flex gap-4 mt-8 w-full">
        <button onClick={handlePrev} className="flex-1 py-4 bg-white border border-slate-200 rounded-xl font-semibold text-slate-600 hover:bg-slate-50 transition">上一張</button>
        <button onClick={handleNext} className="flex-1 py-4 bg-indigo-600 rounded-xl font-semibold text-white hover:bg-indigo-700 transition">下一張</button>
      </div>
    </div>
  );
}

function ListeningPractice({ speak, setStats, addMistake }) {
  const [question, setQuestion] = useState(null);
  const [options, setOptions] = useState([]);
  const [selected, setSelected] = useState(null);

  const generateQuestion = () => {
    setSelected(null);
    const correctWord = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
    let wrongOptions = [];
    while (wrongOptions.length < 3) {
      const wrong = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
      if (wrong.id !== correctWord.id && !wrongOptions.find(w => w.id === wrong.id)) wrongOptions.push(wrong);
    }
    setQuestion(correctWord);
    setOptions([correctWord, ...wrongOptions].sort(() => Math.random() - 0.5));
  };
  useEffect(() => { generateQuestion(); }, []);

  const handleSelect = (option) => {
    if (selected) return;
    setSelected(option);
    if (option.id === question.id) setStats(p => ({ ...p, listeningScore: p.listeningScore + 1 }));
    else addMistake(question.id); // 聽錯加入錯題本
  };

  if (!question) return null;
  return (
    <div className="max-w-2xl mx-auto flex flex-col items-center justify-center h-full">
      <h2 className="text-2xl font-bold mb-8">聽力測驗</h2>
      <button onClick={() => speak(question.word)} className="w-24 h-24 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center hover:bg-indigo-200 transition-all shadow-inner mb-12"><Volume2 size={40} /></button>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4 w-full">
        {options.map((opt, idx) => {
          let btnClass = "py-4 px-6 bg-white border-2 rounded-2xl text-lg font-medium transition-all text-slate-700 ";
          if (!selected) btnClass += "border-slate-100 hover:border-indigo-300";
          else if (opt.id === question.id) btnClass += "border-green-500 bg-green-50 text-green-700";
          else if (selected.id === opt.id) btnClass += "border-red-500 bg-red-50 text-red-700";
          else btnClass += "border-slate-100 opacity-50";
          return <button key={idx} onClick={() => handleSelect(opt)} className={btnClass} disabled={selected !== null}>{opt.meaning}</button>;
        })}
      </div>
      {selected && (
        <div className="mt-8 flex flex-col items-center animate-fadeIn">
          <p className="text-slate-600 mb-6 text-center"><span className="font-bold text-2xl mr-2">{question.word}</span></p>
          <button onClick={generateQuestion} className="px-8 py-3 bg-indigo-600 text-white rounded-xl font-semibold hover:bg-indigo-700 flex items-center gap-2">下一題 <ArrowRight size={18} /></button>
        </div>
      )}
    </div>
  );
}

// ==========================================
// 功能 3: 升級版書寫練習 (無鍵盤拼字)
// ==========================================
function WritingPractice({ setStats, speak, addMistake }) {
  const [question, setQuestion] = useState(null);
  const [pool, setPool] = useState([]); // 可選的音節方塊
  const [answer, setAnswer] = useState([]); // 已選的音節
  const [status, setStatus] = useState('playing'); // playing, correct, wrong
  const [hintLevel, setHintLevel] = useState(0);

  const generateQuestion = () => {
    const randomWord = vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)];
    setQuestion(randomWord);
    
    // 拆分單字成音節
    const targetChars = randomWord.word.split('');
    
    // 從資料庫隨機抽取干擾音節
    const allChars = vocabDatabase.map(w => w.word).join('').split('');
    let distractors = [];
    while (distractors.length < 4) { // 給4個干擾項
      const randChar = allChars[Math.floor(Math.random() * allChars.length)];
      if (!targetChars.includes(randChar) && !distractors.includes(randChar)) {
        distractors.push(randChar);
      }
    }
    
    // 混合並打亂
    const combinedPool = [...targetChars, ...distractors].map((char, index) => ({ id: index, char }));
    setPool(combinedPool.sort(() => Math.random() - 0.5));
    setAnswer(Array(targetChars.length).fill(null));
    setStatus('playing');
    setHintLevel(0);
  };

  useEffect(() => { generateQuestion(); }, []);

  const handleSelectPool = (item) => {
    if (status !== 'playing') return;
    const emptyIndex = answer.findIndex(val => val === null);
    if (emptyIndex !== -1) {
      const newAnswer = [...answer];
      newAnswer[emptyIndex] = item;
      setAnswer(newAnswer);
      setPool(pool.filter(p => p.id !== item.id));
    }
  };

  const handleSelectAnswer = (item, index) => {
    if (status !== 'playing' || !item) return;
    const newAnswer = [...answer];
    newAnswer[index] = null;
    setAnswer(newAnswer);
    setPool([...pool, item]);
  };

  const checkAnswer = () => {
    if (answer.includes(null)) return; // 還沒拼完
    
    const currentWord = answer.map(a => a.char).join('');
    if (currentWord === question.word) {
      setStatus('correct');
      speak(question.word);
      if (hintLevel === 0) setStats(prev => ({ ...prev, writingScore: prev.writingScore + 1 }));
    } else {
      setStatus('wrong');
      addMistake(question.id); // 拼錯加入錯題本
    }
  };

  const requestHint = () => {
    if (hintLevel < 2) {
      setHintLevel(prev => prev + 1);
      addMistake(question.id); // 用了提示就加入錯題本
      // 若有錯先清空答案，讓方塊回歸
      if(status === 'wrong') {
         const returnedPool = [...pool, ...answer.filter(a => a !== null)];
         setPool(returnedPool);
         setAnswer(Array(question.word.length).fill(null));
         setStatus('playing');
      }
    }
  };

  // 當答案填滿時自動檢查
  useEffect(() => {
    if (answer.length > 0 && !answer.includes(null) && status === 'playing') {
      checkAnswer();
    }
  }, [answer]);

  if (!question) return null;

  return (
    <div className="max-w-xl mx-auto flex flex-col items-center justify-center h-full">
      <div className="w-full bg-white p-6 md:p-8 rounded-3xl shadow-sm border border-slate-100 text-center relative overflow-hidden">
        <h2 className="text-xl font-bold text-slate-500 mb-2">拼出這個單字 (無鍵盤模式)</h2>
        <div className="flex justify-center items-end gap-3 mb-6">
          <p className="text-5xl font-black text-slate-800">{question.meaning}</p>
          <span className="text-sm font-medium text-slate-400 bg-slate-100 px-2 py-1 rounded mb-1">{question.pos}</span>
        </div>

        {/* 提示區塊 */}
        <div className="h-12 mb-6 flex items-center justify-center">
          {hintLevel === 1 && (
            <div className="text-indigo-600 bg-indigo-50 px-4 py-2 rounded-lg font-bold text-xl border border-indigo-100">
              初聲提示：{getChoSeong(question.word)}
            </div>
          )}
          {hintLevel === 2 && (
            <div className="text-red-500 bg-red-50 px-4 py-2 rounded-lg font-bold text-xl border border-red-100">
              答案是：{question.word}
            </div>
          )}
        </div>

        {/* 答案放置區 (Slots) */}
        <div className={`flex justify-center gap-3 mb-8 p-4 rounded-2xl ${status === 'wrong' ? 'bg-red-50 border border-red-200' : status === 'correct' ? 'bg-green-50 border border-green-200' : 'bg-slate-50 border border-slate-200'}`}>
          {answer.map((item, idx) => (
            <div 
              key={idx} 
              onClick={() => handleSelectAnswer(item, idx)}
              className={`w-16 h-16 md:w-20 md:h-20 flex items-center justify-center text-3xl font-bold rounded-xl transition-all ${item ? 'bg-indigo-600 text-white shadow-md cursor-pointer hover:bg-indigo-700' : 'bg-white border-2 border-dashed border-slate-300 text-slate-300'}`}
            >
              {item ? item.char : ''}
            </div>
          ))}
        </div>

        {/* 音節選擇區 (Pool) */}
        {status === 'playing' && (
          <div className="flex flex-wrap justify-center gap-3 mb-8">
            {pool.map(item => (
              <button
                key={item.id}
                onClick={() => handleSelectPool(item)}
                className="w-14 h-14 md:w-16 md:h-16 bg-white border-2 border-slate-200 rounded-xl text-2xl font-bold text-slate-700 hover:border-indigo-400 hover:text-indigo-600 hover:-translate-y-1 transition-all shadow-sm"
              >
                {item.char}
              </button>
            ))}
          </div>
        )}

        <div className="flex flex-col items-center h-24">
          {status === 'playing' && (
             <button type="button" onClick={requestHint} disabled={hintLevel === 2} className={`px-6 py-2 rounded-xl transition ${hintLevel === 2 ? 'bg-slate-100 text-slate-300' : 'text-slate-500 underline hover:text-slate-800'}`}>
               {hintLevel === 0 ? '不知道怎麼拼？(提示)' : hintLevel === 1 ? '直接看答案' : '已顯示答案'}
             </button>
          )}
          {status === 'wrong' && (
            <div className="animate-fadeIn w-full flex flex-col items-center">
              <p className="text-red-500 font-bold mb-3"><XCircle className="inline mr-1"/> 拼錯囉，請重試！</p>
              <button onClick={() => {
                setPool([...pool, ...answer.filter(a=>a!==null)].sort(()=>Math.random()-0.5));
                setAnswer(Array(question.word.length).fill(null));
                setStatus('playing');
              }} className="px-6 py-2 bg-slate-200 text-slate-700 rounded-lg hover:bg-slate-300">清空重拼</button>
            </div>
          )}
          {status === 'correct' && (
            <div className="animate-fadeIn w-full flex flex-col items-center">
              <p className="text-green-600 font-bold text-lg mb-4 flex items-center gap-1"><CheckCircle2 /> 完全正確！</p>
              <button onClick={generateQuestion} className="w-full py-3 bg-slate-800 text-white rounded-xl font-semibold hover:bg-slate-900 transition-colors flex justify-center items-center gap-2">
                下一題 <ArrowRight size={18} />
              </button>
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

// ==========================================
// 功能 4: 錯題本與手寫板 (罰抄功能) - 新增
// ==========================================
function MistakeReview({ mistakes, removeMistake, speak }) {
  const [selectedWord, setSelectedWord] = useState(null);

  // 取得錯題列表的詳細資料
  const mistakeWords = vocabDatabase.filter(w => mistakes.has(w.id));

  if (selectedWord) {
    return <DrawingCanvas word={selectedWord} goBack={() => setSelectedWord(null)} markLearned={() => { removeMistake(selectedWord.id); setSelectedWord(null); }} speak={speak} />;
  }

  return (
    <div className="max-w-4xl mx-auto h-full flex flex-col">
      <header className="mb-6">
        <h2 className="text-2xl font-bold flex items-center gap-2 text-red-600"><AlertCircle /> 錯題溫習區</h2>
        <p className="text-slate-500 mt-2">點擊單字進入「手寫板」進行罰抄練習。練習完畢後可將其移出租題本。</p>
      </header>

      {mistakeWords.length === 0 ? (
        <div className="flex-1 flex flex-col items-center justify-center text-slate-400 bg-white rounded-3xl border border-slate-100 border-dashed">
          <CheckCircle2 size={64} className="mb-4 text-green-300" />
          <p className="text-lg font-medium text-slate-500">太棒了！目前沒有錯題。</p>
          <p className="text-sm">在聽力或拼字練習中答錯的單字會出現在這裡。</p>
        </div>
      ) : (
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
          {mistakeWords.map(word => (
            <div key={word.id} onClick={() => setSelectedWord(word)} className="bg-white p-5 rounded-2xl border border-slate-200 hover:border-indigo-400 hover:shadow-md cursor-pointer transition group">
              <div className="flex justify-between items-start mb-2">
                <span className="text-xs font-semibold text-red-500 bg-red-50 px-2 py-1 rounded">{word.pos}</span>
                <button onClick={(e) => { e.stopPropagation(); removeMistake(word.id); }} className="text-slate-300 hover:text-red-500" title="移出租題本"><Trash2 size={16}/></button>
              </div>
              <p className="text-3xl font-bold text-slate-800 mb-1">{word.word}</p>
              <p className="text-slate-500">{word.meaning}</p>
              <div className="mt-4 flex items-center text-indigo-500 text-sm font-medium opacity-0 group-hover:opacity-100 transition">
                <PenTool size={14} className="mr-1"/> 進入手寫練習 <ArrowRight size={14} className="ml-1"/>
              </div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

// 實際的手寫板組件 (HTML5 Canvas)
function DrawingCanvas({ word, goBack, markLearned, speak }) {
  const canvasRef = useRef(null);
  const [isDrawing, setIsDrawing] = useState(false);

  useEffect(() => {
    // 預設發音一次
    speak(word.word);
    
    // 初始化畫布尺寸
    const canvas = canvasRef.current;
    if (canvas) {
       // 為了支援高解析度螢幕
       const rect = canvas.getBoundingClientRect();
       canvas.width = rect.width;
       canvas.height = rect.height;
       const ctx = canvas.getContext('2d');
       ctx.lineCap = 'round';
       ctx.lineJoin = 'round';
       ctx.lineWidth = 6;
       ctx.strokeStyle = '#312e81'; // 深靛藍色
    }
  }, [word]);

  const getCoordinates = (e) => {
    const canvas = canvasRef.current;
    const rect = canvas.getBoundingClientRect();
    if (e.touches && e.touches.length > 0) {
      return { x: e.touches[0].clientX - rect.left, y: e.touches[0].clientY - rect.top };
    }
    return { x: e.clientX - rect.left, y: e.clientY - rect.top };
  };

  const startDrawing = (e) => {
    e.preventDefault(); // 防止滾動
    const { x, y } = getCoordinates(e);
    const ctx = canvasRef.current.getContext('2d');
    ctx.beginPath();
    ctx.moveTo(x, y);
    setIsDrawing(true);
  };

  const draw = (e) => {
    e.preventDefault();
    if (!isDrawing) return;
    const { x, y } = getCoordinates(e);
    const ctx = canvasRef.current.getContext('2d');
    ctx.lineTo(x, y);
    ctx.stroke();
  };

  const stopDrawing = () => { setIsDrawing(false); };

  const clearCanvas = () => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  };

  return (
    <div className="max-w-3xl mx-auto flex flex-col h-full">
      <div className="flex items-center justify-between mb-6">
        <button onClick={goBack} className="text-slate-500 hover:text-slate-800 font-medium flex items-center gap-1">← 返回錯題本</button>
        <button onClick={() => speak(word.word)} className="p-2 bg-indigo-50 text-indigo-600 rounded-full"><Volume2 size={20} /></button>
      </div>

      <div className="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm flex-1 flex flex-col relative overflow-hidden">
        <div className="text-center mb-6 relative z-10 pointer-events-none">
          <p className="text-lg font-medium text-slate-500 mb-1">{word.meaning} ({word.pos})</p>
          <p className="text-6xl font-black text-slate-800 tracking-widest">{word.word}</p>
          <p className="text-sm text-slate-400 mt-2">請在下方空白處跟著描寫或罰抄</p>
        </div>

        {/* 畫布區域 */}
        <div className="flex-1 relative border-2 border-dashed border-slate-300 rounded-xl bg-slate-50 overflow-hidden touch-none" style={{ backgroundImage: 'linear-gradient(#f1f5f9 1px, transparent 1px), linear-gradient(90deg, #f1f5f9 1px, transparent 1px)', backgroundSize: '40px 40px'}}>
          <canvas
            ref={canvasRef}
            className="w-full h-full cursor-crosshair absolute top-0 left-0"
            onMouseDown={startDrawing}
            onMouseMove={draw}
            onMouseUp={stopDrawing}
            onMouseOut={stopDrawing}
            onTouchStart={startDrawing}
            onTouchMove={draw}
            onTouchEnd={stopDrawing}
          />
        </div>

        <div className="flex gap-4 mt-6">
          <button onClick={clearCanvas} className="flex-1 py-3 bg-slate-100 text-slate-600 font-bold rounded-xl hover:bg-slate-200">
            清除重寫
          </button>
          <button onClick={markLearned} className="flex-1 py-3 bg-green-500 text-white font-bold rounded-xl hover:bg-green-600 shadow-lg shadow-green-200 flex items-center justify-center gap-2">
            <CheckCircle2 size={20}/> 記住了，移出租題本
          </button>
        </div>
      </div>
    </div>
  );
}

// ==========================================
// 功能 5: 口說練習 (簡化程式碼保留功能)
// ==========================================
function SpeakingPractice({ speak, setStats }) {
  const [question, setQuestion] = useState(null);
  const [isRecording, setIsRecording] = useState(false);
  const [transcript, setTranscript] = useState('');
  const [score, setScore] = useState(null);

  const generateQuestion = () => {
    setQuestion(vocabDatabase[Math.floor(Math.random() * vocabDatabase.length)]);
    setTranscript(''); setScore(null); setIsRecording(false);
  };
  useEffect(() => { generateQuestion(); }, []);

  const handleSpeakClick = () => {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (!SpeechRecognition) return alert("瀏覽器不支援，請用 Chrome。");
    const recognition = new SpeechRecognition();
    recognition.lang = 'ko-KR';
    recognition.onstart = () => { setIsRecording(true); setTranscript(''); setScore(null); };
    recognition.onresult = (e) => {
      const speech = e.results[0][0].transcript;
      setTranscript(speech);
      let match = 0;
      const target = question.example.replace(/[^\w\s\uAC00-\uD7A3]/g, '').replace(/\s+/g, '');
      const spoken = speech.replace(/[^\w\s\uAC00-\uD7A3]/g, '').replace(/\s+/g, '');
      for(let i=0; i<Math.min(target.length, spoken.length); i++) if(target[i]===spoken[i]) match++;
      let finalScore = Math.round((match / Math.max(target.length, spoken.length)) * 100) || 0;
      if (finalScore < 50 && spoken.length > 0) finalScore = Math.min(100, finalScore + 30);
      setScore(finalScore);
      if(finalScore >= 80) setStats(p => ({...p, speakingScore: p.speakingScore + 1}));
    };
    recognition.onend = () => setIsRecording(false);
    recognition.start();
  };

  if (!question) return null;
  return (
    <div className="max-w-2xl mx-auto flex flex-col items-center justify-center h-full text-center">
      <h2 className="text-2xl font-bold mb-8">口說發音評分</h2>
      <div className="bg-white p-8 rounded-3xl shadow-sm border border-slate-100 w-full mb-8">
        <p className="text-sm font-semibold text-indigo-500 mb-2">目標句 ({question.word})</p>
        <p className="text-3xl font-bold text-slate-800 mb-4">{question.example}</p>
        <button onClick={() => speak(question.example)} className="inline-flex items-center gap-2 px-4 py-2 bg-slate-100 rounded-full"><Volume2 size={16} /> 聽範例發音</button>
      </div>
      <button onClick={handleSpeakClick} disabled={isRecording} className={`w-24 h-24 rounded-full flex items-center justify-center text-white ${isRecording ? 'bg-red-500 animate-pulse scale-110' : 'bg-indigo-600 hover:scale-105'}`}><Mic size={40} /></button>
      {transcript && (
        <div className="mt-8 w-full bg-slate-50 p-6 rounded-2xl">
          <p className="text-xl font-medium mb-4">你說的："{transcript}"</p>
          {score !== null && (
            <div className="flex flex-col items-center">
              <div className={`text-5xl font-black mb-6 ${score >= 80 ? 'text-green-500' : 'text-red-500'}`}>{score} <span className="text-2xl">/ 100</span></div>
              <button onClick={generateQuestion} className="px-6 py-3 bg-white border text-slate-700 rounded-xl font-semibold">挑戰下一句</button>
            </div>
          )}
        </div>
      )}
    </div>
  );
}
