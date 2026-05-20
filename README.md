```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>중국어 수행평가 완벽 대비 연습기</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700;900&family=Noto+Sans+SC:wght@400;500;700&display=swap');
    body {
      font-family: 'Noto Sans KR', 'Noto Sans SC', sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    /* 버튼 터치 피드백 개선 */
    .active-scale:active {
      transform: scale(0.96);
    }
  </style>
</head>
<body class="bg-slate-50 min-h-screen text-slate-800 pb-12">

  <div class="max-w-4xl mx-auto px-4 py-6">
    <!-- Header -->
    <header class="text-center mb-6 bg-white p-5 rounded-2xl shadow-sm border border-slate-100">
      <h1 class="text-xl md:text-2xl font-bold text-indigo-600 flex items-center justify-center gap-1.5">
        <i data-lucide="sparkles" class="w-6 h-6 text-amber-500 animate-pulse"></i>
        중국어 수행평가 대비 연습기
      </h1>
      <p class="text-slate-500 mt-1 text-xs md:text-sm">
        병음 추천 조각 완전 랜덤 셔플 적용! 더 정교해진 자판 연습모드
      </p>
      
      <!-- Progress/Stats -->
      <div class="grid grid-cols-3 gap-2 mt-4 max-w-sm mx-auto text-xs">
        <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
          <span class="block text-slate-400">학습 진행도</span>
          <span id="progress-text" class="font-bold text-slate-700 text-sm">1 / 17</span>
        </div>
        <div class="bg-emerald-50 p-2 rounded-xl border border-emerald-100 text-emerald-700">
          <span class="block text-emerald-500">맞힌 문제</span>
          <span id="correct-count" class="font-bold text-sm">0</span>
        </div>
        <div class="bg-rose-50 p-2 rounded-xl border border-rose-100 text-rose-700">
          <span class="block text-rose-500">오답 수</span>
          <span id="wrong-count" class="font-bold text-sm">0</span>
        </div>
      </div>
      
      <!-- Progress bar -->
      <div class="w-full bg-slate-100 rounded-full h-1.5 mt-3 overflow-hidden">
        <div id="progress-bar" class="bg-indigo-600 h-1.5 rounded-full transition-all duration-300" style="width: 5.8%"></div>
      </div>
    </header>

    <!-- Main Practice Area -->
    <main class="bg-white rounded-2xl p-5 shadow-sm border border-slate-100 mb-5">
      <!-- Section Tag -->
      <div class="mb-3">
        <span id="lesson-tag" class="inline-block bg-indigo-50 text-indigo-700 text-xs font-bold px-3 py-1 rounded-full border border-indigo-100">
          제1과 好久不见! 오랜만이야! (본문 1)
        </span>
      </div>

      <!-- Korean Meaning Box -->
      <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 mb-5">
        <div class="text-[10px] text-slate-400 font-bold mb-1 flex items-center gap-1 uppercase tracking-wider">
          <i data-lucide="languages" class="w-3.5 h-3.5"></i> 제시된 한국어 뜻
        </div>
        <h2 id="korean-meaning" class="text-lg md:text-xl font-bold text-slate-800 leading-snug">
          정말 오랜만이야! (정말 오랫동안 못 만났네요!)
        </h2>
      </div>

      <!-- Drag and Drop Zone -->
      <div class="mb-5">
        <div class="text-xs text-slate-400 font-medium mb-2 flex items-center gap-1 justify-between">
          <span class="flex items-center gap-1">
            <i data-lucide="grip-horizontal" class="w-3.5 h-3.5 text-indigo-500"></i> 한자 순서 배열 (아래 카드를 눌러 순서대로 올리세요)
          </span>
          <button id="reset-drag-btn" class="text-indigo-600 hover:text-indigo-800 flex items-center gap-0.5 font-bold text-xs active-scale">
            <i data-lucide="refresh-cw" class="w-3 h-3"></i> 초기화
          </button>
        </div>

        <!-- Drop Slots Container -->
        <div id="drop-zone" class="min-h-[60px] bg-slate-50/50 border border-slate-200 rounded-xl p-3 flex flex-wrap gap-1.5 items-center justify-center mb-3 transition-colors duration-200">
          <!-- Placed cards will appear here -->
        </div>

        <!-- Available Cards Container -->
        <div id="cards-zone" class="bg-slate-50 p-3 rounded-xl border border-slate-100 flex flex-wrap gap-1.5 justify-center items-center min-h-[60px]">
          <!-- Shuffled cards go here -->
        </div>
      </div>

      <!-- Pinyin Input Mode Selector -->
      <div class="mb-4 flex border-b border-slate-100 pb-2">
        <button id="mode-keypad-btn" class="flex-1 text-center py-2 text-xs font-bold border-b-2 border-indigo-600 text-indigo-600 flex items-center justify-center gap-1.5">
          <i data-lucide="layout-grid" class="w-4 h-4"></i> 병음 셔플 키패드 모드
        </button>
        <button id="mode-keyboard-btn" class="flex-1 text-center py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-600 flex items-center justify-center gap-1.5">
          <i data-lucide="keyboard" class="w-4 h-4"></i> 직접 타이핑 모드
        </button>
      </div>

      <!-- Pinyin Input / Display Zone -->
      <div class="mb-5">
        <!-- 1. Keypad Mode Display -->
        <div id="pinyin-display-area" class="bg-slate-50 border border-slate-200 rounded-xl p-4 min-h-[56px] flex flex-wrap gap-1.5 items-center justify-start mb-3">
          <span class="text-slate-300 text-xs w-full select-none" id="pinyin-placeholder">아래 랜덤 추천 조각들을 직접 골라 알맞은 병음을 완성하세요!</span>
        </div>

        <!-- Keyboard Mode Input -->
        <input 
          type="text" 
          id="pinyin-keyboard-input" 
          placeholder="한어병음을 직접 자판으로 입력하세요..." 
          class="hidden w-full bg-slate-50 border border-slate-200 focus:border-indigo-500 focus:bg-white focus:ring-2 focus:ring-indigo-100 rounded-xl px-4 py-3 text-base font-medium outline-none transition-all duration-200"
          autocomplete="off"
        >

        <!-- 2. Integrated Pinyin Keypad -->
        <div id="pinyin-keypad-container" class="space-y-3">
          <!-- Character Specific Sound Elements Row (SHUFFLED) -->
          <div class="bg-indigo-50/50 p-2.5 rounded-xl border border-indigo-100/40">
            <span class="text-[10px] text-indigo-600 font-bold block mb-1.5 flex items-center gap-0.5">
              <i data-lucide="shuffle" class="w-3.5 h-3.5 text-indigo-500"></i> 뒤섞인 병음 조각 풀 (터치하여 순서대로 입력)
            </span>
            <div id="pinyin-helper-syllables" class="flex flex-wrap gap-1.5">
              <!-- Helper syllables injected by JS -->
            </div>
          </div>

          <!-- Quick Tone Editor Row -->
          <div class="bg-slate-100/70 p-2 rounded-xl border border-slate-200 flex flex-col gap-1.5">
            <span class="text-[10px] text-slate-500 font-bold flex justify-between items-center">
              <span>✍️ 마지막 입력 단어 '성조 변경' (안 누르면 경성)</span>
              <button id="keypad-backspace" class="text-rose-600 flex items-center gap-0.5 font-bold active-scale">
                <i data-lucide="backspace" class="w-3.5 h-3.5"></i> 뒤로가기/삭제
              </button>
            </span>
            <div class="grid grid-cols-4 gap-1">
              <button class="tone-btn bg-white hover:bg-slate-50 border border-slate-200 py-1.5 rounded-lg text-sm font-bold active-scale" data-tone="1">1성 ( ¯ )</button>
              <button class="tone-btn bg-white hover:bg-slate-50 border border-slate-200 py-1.5 rounded-lg text-sm font-bold active-scale" data-tone="2">2성 ( ˊ )</button>
              <button class="tone-btn bg-white hover:bg-slate-50 border border-slate-200 py-1.5 rounded-lg text-sm font-bold active-scale" data-tone="3">3성 ( ˇ )</button>
              <button class="tone-btn bg-white hover:bg-slate-50 border border-slate-200 py-1.5 rounded-lg text-sm font-bold active-scale" data-tone="4">4성 ( ˋ )</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Action Buttons -->
      <div class="flex flex-col sm:flex-row gap-2.5">
        <button 
          id="check-btn" 
          class="flex-1 bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3.5 px-6 rounded-xl shadow-md transition-all duration-150 flex items-center justify-center gap-2 active-scale"
        >
          <i data-lucide="check-circle" class="w-4 h-4"></i> 채점하기
        </button>
        <button 
          id="next-btn" 
          class="hidden flex-1 bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3.5 px-6 rounded-xl shadow-md transition-all duration-150 flex items-center justify-center gap-2 active-scale"
        >
          다음 문제로 <i data-lucide="arrow-right" class="w-4 h-4"></i>
        </button>
      </div>
    </main>

    <!-- Feedback Box -->
    <div id="feedback-box" class="hidden rounded-2xl p-4 border mb-5 transition-all duration-300 transform scale-95 opacity-0">
      <div class="flex items-start gap-3">
        <div id="feedback-icon-container" class="p-1.5 rounded-lg">
          <!-- Icon injected here -->
        </div>
        <div class="flex-1">
          <h3 id="feedback-title" class="text-base font-bold">정답입니다!</h3>
          <div class="mt-2 space-y-1.5 text-xs md:text-sm">
            <div>
              <span class="text-slate-400 block text-[10px] font-semibold">정답 한자</span>
              <span id="feedback-correct-hanzi" class="font-bold text-base text-slate-800 tracking-wide"></span>
            </div>
            <div>
              <span class="text-slate-400 block text-[10px] font-semibold">정답 한어병음</span>
              <span id="feedback-correct-pinyin" class="font-bold text-sm text-indigo-700"></span>
            </div>
            <div id="feedback-tip-container" class="bg-indigo-50/50 p-2.5 rounded-lg border border-indigo-100/50 mt-1.5 text-[11px] text-slate-600">
              <span class="font-bold text-indigo-800 block mb-0.5">💡 수행평가 필기 암기공식</span>
              <span id="feedback-tip"></span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Quick Navigation -->
    <section class="bg-white rounded-2xl p-4 shadow-sm border border-slate-100">
      <h4 class="text-xs font-bold text-slate-500 mb-2.5 flex items-center gap-1">
        <i data-lucide="list" class="w-3.5 h-3.5"></i> 원하는 문제 바로풀기
      </h4>
      <div id="quick-nav" class="flex flex-wrap gap-1.5">
        <!-- Buttons injected by JS -->
      </div>
    </section>
  </div>

  <!-- JS Code -->
  <script>
    // --- DATABASE ---
    const quizDatabase = [
      // 1과 본문1
      {
        id: 1,
        lesson: "제1과 好久不见! 오랜만이야! (본문 1)",
        korean: "정말 오랜만이야! (정말 오랫동안 못 만났네요!)",
        hanzi: ["好", "久", "不", "见", "！"],
        pinyin: ["Hǎo", "jiǔ", "bú", "jiàn", "！"],
        clean_pinyin: ["hao", "jiu", "bu", "jian"],
        helper_syllables: ["hao", "jiu", "bu", "jian", "hen", "man", "zui", "jin", "mang"],
        tip: "이 문장에서 '好(hǎo)'는 '정말, 아주'라는 뜻으로 '久(오래되다)'를 강조하는 부사 역할을 합니다."
      },
      {
        id: 2,
        lesson: "제1과 好久不见! 오랜만이야! (본문 1)",
        korean: "너 요즘 어때? (안부를 물음)",
        hanzi: ["你", "最", "近", "怎", "么", "样", "？"],
        pinyin: ["Nǐ", "zuìjìn", "zěnmeyàng", "？"],
        clean_pinyin: ["ni", "zuijin", "zenmeyang"],
        helper_syllables: ["ni", "zui", "jin", "zuijin", "zenme", "yang", "zenmeyang", "ma", "hao"],
        tip: "最近(zuìjìn)은 '요즘', 怎么样(zěnmeyàng)은 '어때?'라는 뜻의 의문대명사입니다."
      },
      {
        id: 3,
        lesson: "제1과 好久不见! 오랜만이야! (본문 1)",
        korean: "아주 좋아, 단지 막 개학해서, 매일마다 아주 일찍 기상하는 게, 난 아직 익숙하지 않아.",
        hanzi: ["挺", "好", "的", "，", "只", "es", "刚", "开", "学", "，", "每", "天", "都", "很", "早", "起", "床", "，", "我", "还", "不", "习", "惯", "。"],
        hanzi: ["挺", "好", "의", "，", "只", "是", "刚", "开", "学", "，", "每", "天", "都", "很", "早", "起", "床", "，", "我", "还", "不", "习", "惯", "。"],
        pinyin: ["Tǐng", "hǎo", "de", "，", "zhǐshì", "gāng", "kāixué", "，", "měitiān", "dōu", "hěn", "zǎo", "qǐchuáng", "，", "wǒ", "hái", "bù", "xǐguàn", "。"],
        clean_pinyin: ["ting", "hao", "de", "zhishi", "gang", "kaixue", "meitian", "dou", "hen", "zao", "qichuang", "wo", "hai", "bu", "xiguan"],
        helper_syllables: ["ting", "hao", "de", "zhishi", "gang", "kaixue", "meitian", "dou", "hen", "zao", "qichuang", "wo", "hai", "bu", "xiguan", "shang", "ke", "liao"],
        tip: "① '表达(단지)' / '刚(방금)'. ② '都(모두)'의 위치와 '还不(아직 ~하지 않다)'의 어순에 꼭 주의하세요!"
      },
      {
        id: 4,
        lesson: "제1과 好久不见! 오랜만이야! (본문 1)",
        korean: "조급해하지 마, 천천히 익숙해질 거야.",
        hanzi: ["别", "着", "急", "，", "慢", "慢", "儿", "会", "习", "惯", "的", "。"],
        pinyin: ["Bié", "zháojí", "，", "mànmānr", "huì", "xǐguàn", "de", "。"],
        clean_pinyin: ["bie", "zhaoji", "manmanr", "hui", "xiguan", "de"],
        helper_syllables: ["bie", "zhaoji", "manmanr", "man", "man", "r", "hui", "xiguan", "de", "hen", "bu", "tai"],
        tip: "★ 중요 서술형 포인트 ★ '慢慢儿(mànmānr)'은 형용사 중첩(AA)으로 그 자체가 강화된 형태이므로 앞에 '很'을 절대 함께 쓸 수 없습니다! (很慢慢儿 X)"
      },
      {
        id: 5,
        lesson: "제1과 好久不见! 오랜만이야! (본문 1)",
        korean: "고마워. (고마움의 대상은 뒤에!)",
        hanzi: ["谢", "谢", "你", "。"],
        pinyin: ["Xièxie", "nǐ", "。"],
        clean_pinyin: ["xiexie", "ni"],
        helper_syllables: ["xiexie", "ni", "nin", "ta", "men", "laoshi"],
        tip: "중국어에서는 감사 대상이 동사 뒤에 옵니다: 谢谢 + 대상(기본적으로 你/老师)"
      },
      // 1과 본문2
      {
        id: 6,
        lesson: "제1과 好久不见! 오랜만이야! (본문 2)",
        korean: "여러분 안녕하세요.",
        hanzi: ["大", "가", "好", "！"],
        hanzi: ["大", "家", "好", "！"],
        pinyin: ["Dàjiā", "hǎo", "！"],
        clean_pinyin: ["dajia", "hao"],
        helper_syllables: ["dajia", "hao", "nimen", "laoshi", "zao", "nin"],
        tip: "[대상 + 好]는 인사의 가장 대중적인 어순 공식입니다."
      },
      {
        id: 7,
        lesson: "제1과 好久不见! 오랜만이야! (본문 2)",
        korean: "여러분 만나서 반갑습니다.",
        hanzi: ["我", "认", "识", "你", "们", "很", "高", "兴", "。"],
        pinyin: ["Wǒ", "rènshi", "nǐmen", "hěn", "gāoxìng", "。"],
        clean_pinyin: ["wo", "renshi", "nimen", "hen", "gaoxing"],
        helper_syllables: ["wo", "renshi", "nimen", "hen", "gaoxing", "ye", "bu", "shao"],
        tip: "복수를 나타내는 접미사 '们(men)'은 인칭대명사 '你们' 뒤에 붙습니다."
      },
      {
        id: 8,
        lesson: "제1과 好久不见! 오랜만이야! (본문 2)",
        korean: "제가 자기소개를 해볼게요.",
        hanzi: ["我", "来", "自", "我", "介", "绍", "一", "下", "。"],
        pinyin: ["Wǒ", "lái", "zìwǒ", "jièshào", "yíxià", "。"],
        clean_pinyin: ["wo", "lai", "ziwo", "jieshao", "yixia"],
        helper_syllables: ["wo", "lai", "ziwo", "jieshao", "yixia", "yidianr", "yihuir", "shao"],
        tip: "문장 끝의 '一下(yíxià)'는 동사 뒤에 붙어 '좀 ~해보다'라는 의미로 행동을 완화하고 공손함을 더해줍니다."
      },
      {
        id: 9,
        lesson: "제1과 好久不见! 오랜만이야! (본문 2)",
        korean: "저는 왕리라고 하고, 올해 18살입니다.",
        hanzi: ["我", "叫", "王丽", "，", "今", "년", "十", "八", "岁", "。"],
        hanzi: ["我", "叫", "王丽", "，", "今", "年", "十", "八", "岁", "。"],
        pinyin: ["Wǒ", "jiào", "Wáng", "Lì", "，", "jīnnián", "shíbā", "suì", "。"],
        clean_pinyin: ["wo", "jiao", "wang", "li", "jinnian", "shiba", "sui"],
        helper_syllables: ["wo", "jiao", "wang", "li", "jinnian", "shiba", "sui", "shi", "jiu", "sui"],
        tip: "★ 나이, 학년 등을 밝힐 때 동사(是)를 생략하는 '명사술어문'을 사용합니다."
      },
      {
        id: 10,
        lesson: "제1과 好久不见! 오랜만이야! (본문 2)",
        korean: "저는 한국사람이고, 평택에서 왔어요.",
        hanzi: ["我", "是", "韩", "国", "人", "，", "从", "平", "泽", "来", "의", "。"],
        hanzi: ["我", "是", "韩", "국", "人", "，", "从", "平", "泽", "来", "的", "。"],
        pinyin: ["Wǒ", "shì", "Hánguórén", "，", "cóng", "Píngzé", "lái", "de", "。"],
        clean_pinyin: ["wo", "shi", "hanguoren", "cong", "pingze", "lai", "de"],
        helper_syllables: ["wo", "shi", "hanguoren", "cong", "pingze", "lai", "de", "zai", "dao", "qu"],
        tip: "★ 순서 필수 기억 ★ [ S + 전치사(从) + 명사(장소) + V + 的 ] 구조로 출신지나 기점을 표현합니다."
      },
      {
        id: 11,
        lesson: "제1과 好久不见! 오랜만이야! (본문 2)",
        korean: "많은 지도 부탁드려요.",
        hanzi: ["请", "多", "多", "指", "教", "。"],
        pinyin: ["Qǐng", "duōduō", "zhǐjiào", "。"],
        clean_pinyin: ["qing", "duoduo", "zhijiao"],
        helper_syllables: ["qing", "duoduo", "zhijiao", "bangzhu", "xie", "wen"],
        tip: "'多多(duōduō)'는 형용사 중첩으로 '많이많이' 부탁한다는 관용적인 공손함의 표시입니다."
      },
      // 2과 본문1
      {
        id: 12,
        lesson: "제2과 你觉得汉语难吗？ (본문 1)",
        korean: "다음 교시는 무슨 수업이지?",
        hanzi: ["下", "一", "节", "是", "什", "么", "课", "？"],
        pinyin: ["Xià", "yì", "jié", "shì", "shénme", "kè", "？"],
        clean_pinyin: ["xia", "yi", "jie", "shi", "shenme", "ke"],
        helper_syllables: ["xia", "yi", "jie", "shi", "shenme", "ke", "na", "ge", "shang"],
        tip: "'节(jié)'은 수업 시간을 뜻하는 중요한 양사입니다."
      },
      {
        id: 13,
        lesson: "제2과 你觉得汉语难吗？ (본문 1)",
        korean: "중국어 수업이야.",
        hanzi: ["汉", "语", "课", "。"],
        pinyin: ["Hànyǔ", "kè", "。"],
        clean_pinyin: ["hanyu", "ke"],
        helper_syllables: ["hanyu", "ke", "yingyu", "tiyu", "shu", "xue"],
        tip: "중국어는 '汉语(Hànyǔ)'입니다."
      },
      {
        id: 14,
        lesson: "제2과 你觉得汉语难吗？ (본문 1)",
        korean: "너는 중국어가 어렵다고 생각하니?",
        hanzi: ["你", "觉", "得", "汉", "语", "难", "吗", "？"],
        pinyin: ["Nǐ", "juéde", "Hànyǔ", "nán", "ma", "？"],
        clean_pinyin: ["ni", "juede", "hanyu", "nan", "ma"],
        helper_syllables: ["ni", "juede", "hanyu", "nan", "ma", "bu", "hen", "shuo"],
        tip: "자신의 생각이나 주관적인 의견을 나타낼 때 동사 '觉得(juéde)'를 씁니다."
      },
      {
        id: 15,
        lesson: "제2과 你觉得汉语难吗？ (본문 1)",
        korean: "처음 시작했을 때 좀 어려웠지만, 점점 재미있어지고 있어.",
        hanzi: ["开", "始", "的", "时", "候", "有", "点", "儿", "难", "，", "可", "是", "越", "来", "越", "有", "意", "思", "了", "。"],
        pinyin: ["Kāishǐ", "de", "shíhou", "yǒudiǎnr", "nán", "，", "kěshì", "yuèláiyuè", "yǒu", "yìsi", "le", "。"],
        clean_pinyin: ["kaishi", "de", "shihou", "youdianr", "nan", "keshi", "yuelaiyue", "you", "yisi", "le"],
        helper_syllables: ["kaishi", "de", "shihou", "youdianr", "nan", "keshi", "yuelaiyue", "you", "yisi", "le", "yidianr", "tai"],
        tip: "★ 초특급 기출 문법 ★\n① '有点儿'은 '조금, 약간'이라는 뜻으로 불만을 품은 채 형용사 '앞'에 옵니다.\n② '越来越~了'는 상태가 점차 깊어지는 변화를 말합니다."
      },
      {
        id: 16,
        lesson: "제2과 你觉得汉语难吗？ (본문 1)",
        korean: "가장 좋은 것은 많이 듣고, 많이 말하고, 많이 쓰는 거야.",
        hanzi: ["最", "好", "多", "听", "、", "多", "说", "、", "多", "写", "。"],
        pinyin: ["Zuìhǎo", "duō", "tīng", "，", "duō", "shuō", "，", "duō", "xiě", "。"],
        clean_pinyin: ["zuihao", "duo", "ting", "duo", "shuo", "duo", "xie"],
        helper_syllables: ["zuihao", "duo", "ting", "shuo", "xie", "kan", "du", "shao"],
        tip: "'最好(가장 좋기로는)'는 부사이므로 문장 앞에 서서 아래 행동들을 권유합니다."
      },
      {
        id: 17,
        lesson: "제2과 你觉得汉语难吗？ (본문 1)",
        korean: "맞아! 한 입 먹어서 뚱보가 되는 건 아니야.",
        hanzi: ["对", "！", "胖", "자", "不", "是", "一", "口", "吃", "成", "的", "！"],
        hanzi: ["对", "！", "胖", "子", "不", "是", "一", "口", "吃", "成", "의", "！"],
        hanzi: ["对", "！", "胖", "子", "不", "是", "一", "口", "吃", "成", "的", "！"],
        pinyin: ["Duì", "！", "Pàngzi", "bú", "shì", "yìkǒu", "chīchéng", "de", "！"],
        clean_pinyin: ["dui", "pangzi", "bu", "shi", "yikou", "chicheng", "de"],
        helper_syllables: ["dui", "pangzi", "bu", "shi", "yikou", "chicheng", "de", "cheng", "pàng", "chi"],
        tip: "중국의 유명한 속담으로, 무엇이든 한 번에 급하게 하려 하지 말고 '끈기를 갖고 꾸준히 실천하자'는 교훈입니다."
      }
    ];

    // --- DICTIONARY FOR TONE CONVERSION ---
    const toneMap = {
      "hao": { "0": "hao", "1": "hāo", "2": "háo", "3": "hǎo", "4": "hào" },
      "jiu": { "0": "jiu", "1": "jiū", "2": "jiú", "3": "jiǔ", "4": "jiù" },
      "bu": { "0": "bu", "1": "bū", "2": "bú", "3": "bǔ", "4": "bù" },
      "jian": { "0": "jian", "1": "jiān", "2": "jián", "3": "jiǎn", "4": "jiàn" },
      "ni": { "0": "ni", "1": "nī", "2": "ní", "3": "nǐ", "4": "nì" },
      "zui": { "0": "zui", "1": "zuī", "2": "zuí", "3": "zuǐ", "4": "zuì" },
      "jin": { "0": "jin", "1": "jīn", "2": "jín", "3": "jǐn", "4": "jìn" },
      "zuijin": { "0": "zuijin", "1": "zuījīn", "2": "zuí jín", "3": "zuǐ jǐn", "4": "zuìjìn" },
      "zenme": { "0": "zenme", "1": "zēnme", "2": "zénme", "3": "zěnme", "4": "zènme" },
      "yang": { "0": "yang", "1": "yāng", "2": "yáng", "3": "yǎng", "4": "yàng" },
      "zenmeyang": { "0": "zenmeyang", "1": "zēnmeyāng", "2": "zénmeyáng", "3": "zěnmeyàng", "4": "zènmeyàng" },
      "ting": { "0": "ting", "1": "tīng", "2": "tíng", "3": "tǐng", "4": "tìng" },
      "de": { "0": "de", "1": "dē", "2": "dé", "3": "dě", "4": "de" },
      "zhishi": { "0": "zhishi", "1": "zhīshī", "2": "zhíshí", "3": "zhǐshì", "4": "zhìshì" },
      "gang": { "0": "gang", "1": "gāng", "2": "gáng", "3": "gǎng", "4": "gàng" },
      "kaixue": { "0": "kaixue", "1": "kāixué", "2": "káixué", "3": "kǎixuě", "4": "kàixuè" },
      "meitian": { "0": "meitian", "1": "měitiān", "2": "méitián", "3": "měitiǎn", "4": "mèitiàn" },
      "dou": { "0": "dou", "1": "dōu", "2": "dóu", "3": "dǒu", "4": "dòu" },
      "hen": { "0": "hen", "1": "hēn", "2": "hén", "3": "hěn", "4": "hèn" },
      "zao": { "0": "zao", "1": "zāo", "2": "záo", "3": "zǎo", "4": "zào" },
      "qichuang": { "0": "qichuang", "1": "qīchuāng", "2": "qíchuáng", "3": "qǐchuǎng", "4": "qìchuàng" },
      "wo": { "0": "wo", "1": "wō", "2": "wó", "3": "wǒ", "4": "wò" },
      "hai": { "0": "hai", "1": "hāi", "2": "hái", "3": "hǎi", "4": "hài" },
      "xiguan": { "0": "xiguan", "1": "xīguān", "2": "xíguán", "3": "xǐguǎn", "4": "xìguàn" },
      "bie": { "0": "bie", "1": "biē", "2": "bié", "3": "biě", "4": "biè" },
      "zhaoji": { "0": "zhaoji", "1": "zhāojī", "2": "zháojí", "3": "zhǎojǐ", "4": "zhàojì" },
      "manmanr": { "0": "manmanr", "1": "mānmānr", "2": "mánmánr", "3": "mǎnmǎnr", "4": "mànmānr" },
      "man": { "0": "man", "1": "mān", "2": "mán", "3": "mǎn", "4": "màn" },
      "r": { "0": "r", "1": "r", "2": "r", "3": "r", "4": "r" },
      "hui": { "0": "hui", "1": "huī", "2": "huí", "3": "huǐ", "4": "huì" },
      "xiexie": { "0": "xiexie", "1": "xiēxiē", "2": "xiéxié", "3": "xiěxiě", "4": "xièxie" },
      "nin": { "0": "nin", "1": "nīn", "2": "nín", "3": "nǐn", "4": "nìn" },
      "ta": { "0": "ta", "1": "tā", "2": "tá", "3": "tǎ", "4": "tà" },
      "men": { "0": "men", "1": "mēn", "2": "mén", "3": "měn", "4": "men" },
      "laoshi": { "0": "laoshi", "1": "lāoshī", "2": "láoshí", "3": "lǎoshī", "4": "làoshì" },
      "dajia": { "0": "dajia", "1": "dājiā", "2": "dájiá", "3": "dǎjiǎ", "4": "dàjiā" },
      "renshi": { "0": "renshi", "1": "rēnshī", "2": "rénshí", "3": "rěnshǐ", "4": "rènshi" },
      "gaoxing": { "0": "gaoxing", "1": "gāoxīng", "2": "gáoxíng", "3": "gǎoxǐng", "4": "gāoxìng" },
      "lai": { "0": "lai", "1": "lāi", "2": "lái", "3": "lǎi", "4": "lài" },
      "ziwo": { "0": "ziwo", "1": "zīwō", "2": "zíwó", "3": "zǐwǒ", "4": "zìwò" },
      "jieshao": { "0": "jieshao", "1": "jiēshāo", "2": "jiésháo", "3": "jiěshǎo", "4": "jièshào" },
      "yixia": { "0": "yixia", "1": "yīxiā", "2": "yíxià", "3": "yǐxiǎ", "4": "yìxià" },
      "yidianr": { "0": "yidianr", "1": "yīdiānr", "2": "yídiánr", "3": "yǐdiǎnr", "4": "yìdiǎnr" },
      "yihuir": { "0": "yihuir", "1": "yīhuīr", "2": "yíhuír", "3": "yǐhuǐr", "4": "yìhuìr" },
      "shao": { "0": "shao", "1": "shāo", "2": "sháo", "3": "shǎo", "4": "shào" },
      "wang": { "0": "wang", "1": "wāng", "2": "wáng", "3": "wǎng", "4": "wàng" },
      "li": { "0": "li", "1": "lī", "2": "lí", "3": "lǐ", "4": "lì" },
      "jinnian": { "0": "jinnian", "1": "jīnnián", "2": "jínnián", "3": "jǐnniǎn", "4": "jìnniàn" },
      "shiba": { "0": "shiba", "1": "shībā", "2": "shíbā", "3": "shǐbā", "4": "shìbā" },
      "sui": { "0": "sui", "1": "suī", "2": "suí", "3": "suǐ", "4": "suì" },
      "shi": { "0": "shi", "1": "shī", "2": "shí", "3": "shǐ", "4": "shì" },
      "jiu": { "0": "jiu", "1": "jiū", "2": "jiú", "3": "jiǔ", "4": "jiù" },
      "hanguoren": { "0": "hanguoren", "1": "hānguōrén", "2": "Hánguórén", "3": "hǎnguǒrén", "4": "hànguòrén" },
      "cong": { "0": "cong", "1": "cōng", "2": "cóng", "3": "cǒng", "4": "còng" },
      "pingze": { "0": "pingze", "1": "pīngzē", "2": "Píngzé", "3": "pǐngzě", "4": "pìngzè" },
      "zai": { "0": "zai", "1": "zāi", "2": "zái", "3": "zǎi", "4": "zài" },
      "dao": { "0": "dao", "1": "dāo", "2": "dáo", "3": "dǎo", "4": "dào" },
      "qu": { "0": "qu", "1": "qū", "2": "qú", "3": "qǔ", "4": "qù" },
      "zhijiao": { "0": "zhijiao", "1": "zhījiāo", "2": "zhíjiáo", "3": "zhǐjiào", "4": "zhìjiào" },
      "xia": { "0": "xia", "1": "xiā", "2": "xiá", "3": "xiǎ", "4": "xià" },
      "yi": { "0": "yi", "1": "yī", "2": "yí", "3": "yǐ", "4": "yì" },
      "jie": { "0": "jie", "1": "jiē", "2": "jié", "3": "jiě", "4": "jiè" },
      "shenme": { "0": "shenme", "1": "shēnme", "2": "shénme", "3": "shěnme", "4": "shènme" },
      "ke": { "0": "ke", "1": "kē", "2": "ké", "3": "kě", "4": "kè" },
      "na": { "0": "na", "1": "nā", "2": "ná", "3": "nǎ", "4": "nà" },
      "ge": { "0": "ge", "1": "gē", "2": "gé", "3": "gě", "4": "gè" },
      "shang": { "0": "shang", "1": "shāng", "2": "sháng", "3": "shǎng", "4": "shàng" },
      "hanyu": { "0": "hanyu", "1": "hānyū", "2": "Hànyǔ", "3": "hǎnyǔ", "4": "hànyù" },
      "yingyu": { "0": "yingyu", "1": "yīngyǔ", "2": "yíngyǔ", "3": "yǐngyǔ", "4": "yìngyǔ" },
      "tiyu": { "0": "tiyu", "1": "tīyǔ", "2": "tíyǔ", "3": "tǐyǔ", "4": "tìyǔ" },
      "juede": { "0": "juede", "1": "juēdē", "2": "juéde", "3": "juědě", "4": "juèdè" },
      "nan": { "0": "nan", "1": "nān", "2": "nán", "3": "nǎn", "4": "nàn" },
      "ma": { "0": "ma", "1": "mā", "2": "má", "3": "mǎ", "4": "ma" },
      "youdianr": { "0": "youdianr", "1": "yōudiānr", "2": "yóudiánr", "3": "yǒudiǎnr", "4": "yòudiānr" },
      "keshi": { "0": "keshi", "1": "kēshī", "2": "kěshì", "3": "kěshǐ", "4": "kèshì" },
      "yuelaiyue": { "0": "yuelaiyue", "1": "yuēláiyuē", "2": "yuéláiyué", "3": "yuěláiyuě", "4": "yuèláiyuè" },
      "you": { "0": "you", "1": "yōu", "2": "yóu", "3": "yǒu", "4": "yòu" },
      "yisi": { "0": "yisi", "1": "yīsī", "2": "yísí", "3": "yǐsǐ", "4": "yìsi" },
      "le": { "0": "le", "1": "lē", "2": "lé", "3": "lě", "4": "le" },
      "zuihao": { "0": "zuihao", "1": "zuīhāo", "2": "zuíháo", "3": "Zuìhǎo", "4": "zuìhào" },
      "duo": { "0": "duo", "1": "duō", "2": "duó", "3": "duǒ", "4": "duò" },
      "ting": { "0": "ting", "1": "tīng", "2": "tíng", "3": "tǐng", "4": "tìng" },
      "shuo": { "0": "shuo", "1": "shuō", "2": "shuó", "3": "shuǒ", "4": "shuò" },
      "xie": { "0": "xie", "1": "xiē", "2": "xié", "3": "xiě", "4": "xiè" },
      "kan": { "0": "kan", "1": "kān", "2": "kán", "3": "kǎn", "4": "kàn" },
      "du": { "0": "du", "1": "dū", "2": "dú", "3": "dǔ", "4": "dù" },
      "shao": { "0": "shao", "1": "shāo", "2": "sháo", "3": "shǎo", "4": "shào" },
      "dui": { "0": "dui", "1": "duī", "2": "duí", "3": "duǐ", "4": "Duì" },
      "pangzi": { "0": "pangzi", "1": "pāngzī", "2": "Pàngzi", "3": "pǎngzǐ", "4": "pàngzǐ" },
      "yikou": { "0": "yikou", "1": "yīkōu", "2": "yí kǒu", "3": "yǐkǒu", "4": "yìkǒu" },
      "chicheng": { "0": "chicheng", "1": "chīchēng", "2": "chīchéng", "3": "chǐchěng", "4": "chìchèng" }
    };

    // --- APP STATE ---
    let currentIdx = 0;
    let placedCards = [];
    let availableCards = [];
    let keypadPinyinWords = [];
    let isKeypadMode = true;
    let correctAnswers = 0;
    let wrongAnswers = 0;
    let isChecked = false;

    // --- DOM ELEMENTS ---
    const progressText = document.getElementById('progress-text');
    const correctCount = document.getElementById('correct-count');
    const wrongCount = document.getElementById('wrong-count');
    const progressBar = document.getElementById('progress-bar');
    const lessonTag = document.getElementById('lesson-tag');
    const koreanMeaning = document.getElementById('korean-meaning');
    const dropZone = document.getElementById('drop-zone');
    const cardsZone = document.getElementById('cards-zone');
    
    // Input elements
    const pinyinDisplayArea = document.getElementById('pinyin-display-area');
    const pinyinPlaceholder = document.getElementById('pinyin-placeholder');
    const pinyinKeyboardInput = document.getElementById('pinyin-keyboard-input');
    const pinyinKeypadContainer = document.getElementById('pinyin-keypad-container');
    const pinyinHelperSyllables = document.getElementById('pinyin-helper-syllables');
    const keypadBackspace = document.getElementById('keypad-backspace');
    
    // Mode Buttons
    const modeKeypadBtn = document.getElementById('mode-keypad-btn');
    const modeKeyboardBtn = document.getElementById('mode-keyboard-btn');
    
    const checkBtn = document.getElementById('check-btn');
    const nextBtn = document.getElementById('next-btn');
    const resetDragBtn = document.getElementById('reset-drag-btn');
    
    // Feedback
    const feedbackBox = document.getElementById('feedback-box');
    const feedbackIconContainer = document.getElementById('feedback-icon-container');
    const feedbackTitle = document.getElementById('feedback-title');
    const feedbackCorrectHanzi = document.getElementById('feedback-correct-hanzi');
    const feedbackCorrectPinyin = document.getElementById('feedback-correct-pinyin');
    const feedbackTip = document.getElementById('feedback-tip');
    const quickNav = document.getElementById('quick-nav');

    // --- INITIALIZATION ---
    function initApp() {
      renderQuickNav();
      setupEvents();
      loadQuestion(0);
      lucide.createIcons();
    }

    // --- SETUP ACTIONS & EVENTS ---
    function setupEvents() {
      // Toggle keypad mode
      modeKeypadBtn.addEventListener('click', () => {
        isKeypadMode = true;
        modeKeypadBtn.className = "flex-1 text-center py-2 text-xs font-bold border-b-2 border-indigo-600 text-indigo-600 flex items-center justify-center gap-1.5";
        modeKeyboardBtn.className = "flex-1 text-center py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-600 flex items-center justify-center gap-1.5";
        pinyinKeyboardInput.classList.add('hidden');
        pinyinKeypadContainer.classList.remove('hidden');
        renderKeypadPinyinOutput();
      });

      // Toggle keyboard mode
      modeKeyboardBtn.addEventListener('click', () => {
        isKeypadMode = false;
        modeKeyboardBtn.className = "flex-1 text-center py-2 text-xs font-bold border-b-2 border-indigo-600 text-indigo-600 flex items-center justify-center gap-1.5";
        modeKeypadBtn.className = "flex-1 text-center py-2 text-xs font-bold border-b-2 border-transparent text-slate-400 hover:text-slate-600 flex items-center justify-center gap-1.5";
        pinyinKeypadContainer.classList.add('hidden');
        pinyinKeyboardInput.classList.remove('hidden');
        pinyinKeyboardInput.focus();
      });

      // Backspace for keypad
      keypadBackspace.addEventListener('click', () => {
        if (isChecked || keypadPinyinWords.length === 0) return;
        keypadPinyinWords.pop();
        renderKeypadPinyinOutput();
      });

      // Apply tones
      document.querySelectorAll('.tone-btn').forEach(btn => {
        btn.addEventListener('click', () => {
          if (isChecked || keypadPinyinWords.length === 0) return;
          const toneNum = btn.getAttribute('data-tone');
          const lastIdx = keypadPinyinWords.length - 1;
          const lastWordObj = keypadPinyinWords[lastIdx];
          
          if (toneMap[lastWordObj.base]) {
            const mappedTone = toneMap[lastWordObj.base][toneNum] || lastWordObj.base;
            keypadPinyinWords[lastIdx].styled = mappedTone;
            renderKeypadPinyinOutput();
          }
        });
      });
    }

    // --- QUICK NAVIGATION ---
    function renderQuickNav() {
      quickNav.innerHTML = '';
      quizDatabase.forEach((q, index) => {
        const btn = document.createElement('button');
        btn.innerText = `${index + 1}`;
        btn.className = `w-9 h-9 rounded-lg font-bold text-xs transition-all duration-150 border flex items-center justify-center active-scale ${
          index === currentIdx 
            ? 'bg-indigo-600 border-indigo-600 text-white shadow-sm' 
            : 'bg-white border-slate-200 text-slate-600 hover:bg-slate-50'
        }`;
        btn.addEventListener('click', () => {
          loadQuestion(index);
        });
        quickNav.appendChild(btn);
      });
    }

    // --- LOAD QUESTION DATA ---
    function loadQuestion(index) {
      currentIdx = index;
      const question = quizDatabase[currentIdx];
      
      // Update basic text
      progressText.innerText = `${currentIdx + 1} / ${quizDatabase.length}`;
      progressBar.style.width = `${((currentIdx + 1) / quizDatabase.length) * 100}%`;
      lessonTag.innerText = question.lesson;
      koreanMeaning.innerText = question.korean;
      
      // Reset inputs & states
      pinyinKeyboardInput.value = '';
      pinyinKeyboardInput.disabled = false;
      
      keypadPinyinWords = [];
      renderKeypadPinyinOutput();

      placedCards = [];
      isChecked = false;
      
      // Re-render Navigation active states
      Array.from(quickNav.children).forEach((btn, idx) => {
        if (idx === currentIdx) {
          btn.className = 'w-9 h-9 rounded-lg font-bold text-xs bg-indigo-600 border-indigo-600 text-white shadow-sm flex items-center justify-center';
        } else {
          btn.className = 'w-9 h-9 rounded-lg font-bold text-xs bg-white border-slate-200 text-slate-600 hover:bg-slate-50 flex items-center justify-center';
        }
      });

      // Prepare Cards - SHUFFLED
      availableCards = shuffleArray([...question.hanzi]);
      renderCards();
      renderHelperSyllables();
      
      // Reset buttons
      checkBtn.classList.remove('hidden');
      nextBtn.classList.add('hidden');
      feedbackBox.classList.add('hidden');
      feedbackBox.className = "hidden rounded-2xl p-4 border mb-5 transition-all duration-300 transform scale-95 opacity-0";
    }

    // --- CARD RENDERING ---
    function renderCards() {
      // 1. Placed Cards (Drop Zone)
      dropZone.innerHTML = '';
      if (placedCards.length === 0) {
        dropZone.innerHTML = `<span class="text-slate-300 text-xs py-2 select-none">여기에 한자가 차례대로 나타납니다</span>`;
      } else {
        placedCards.forEach((char, index) => {
          const card = createCardElement(char, index, true);
          dropZone.appendChild(card);
        });
      }

      // 2. Available Cards (Pool)
      cardsZone.innerHTML = '';
      if (availableCards.length === 0) {
        cardsZone.innerHTML = `<span class="text-indigo-500 text-[11px] py-1.5 flex items-center gap-1 select-none font-bold"><i data-lucide="check-circle-2" class="w-3.5 h-3.5"></i> 한자 배열 완료! 아래 한어병음도 채워보세요.</span>`;
        lucide.createIcons();
      } else {
        availableCards.forEach((char, index) => {
          const card = createCardElement(char, index, false);
          cardsZone.appendChild(card);
        });
      }
    }

    // --- CARD ELEMENT CREATOR ---
    function createCardElement(char, index, isPlaced) {
      const card = document.createElement('button');
      card.innerText = char;
      
      let baseStyle = "px-3.5 py-1.5 bg-white border rounded-lg font-bold text-base md:text-lg shadow-sm active-scale transition-all duration-100 select-none flex items-center justify-center ";
      
      if (isPlaced) {
        if (isPunctuation(char)) {
          card.className = baseStyle + "border-indigo-100 text-indigo-600 bg-indigo-50/50";
        } else {
          card.className = baseStyle + "border-slate-300 text-slate-800 hover:bg-slate-50";
        }
        
        if (!isChecked) {
          card.addEventListener('click', () => {
            placedCards.splice(index, 1);
            availableCards.push(char);
            renderCards();
          });
        }
      } else {
        if (isPunctuation(char)) {
          card.className = baseStyle + "border-slate-200 text-indigo-400 bg-slate-50";
        } else {
          card.className = baseStyle + "border-slate-200 text-slate-700 hover:bg-slate-100";
        }
        
        if (!isChecked) {
          card.addEventListener('click', () => {
            availableCards.splice(index, 1);
            placedCards.push(char);
            renderCards();
          });
        }
      }
      
      return card;
    }

    // --- HELPER SYLLABLES FOR KEYPAD (NOW TRULY SHUFFLED!) ---
    function renderHelperSyllables() {
      pinyinHelperSyllables.innerHTML = '';
      const question = quizDatabase[currentIdx];
      
      // SHUFFLE the helper options before displaying
      const rawSyllables = question.helper_syllables || [];
      const shuffledSyllables = shuffleArray([...rawSyllables]);
      
      shuffledSyllables.forEach(s => {
        const btn = document.createElement('button');
        btn.innerText = s;
        btn.className = "px-3 py-1.5 bg-white hover:bg-indigo-50 text-slate-700 hover:text-indigo-700 border border-slate-200 hover:border-indigo-200 rounded-lg text-xs md:text-sm font-bold shadow-sm transition-all duration-100 active-scale";
        
        btn.addEventListener('click', () => {
          if (isChecked) return;
          // Add basic raw syllable to current keypad array
          keypadPinyinWords.push({ base: s, styled: s });
          renderKeypadPinyinOutput();
        });
        
        pinyinHelperSyllables.appendChild(btn);
      });
    }

    // --- RENDER KEYPAD PINYIN RESULT DISPLAY ---
    function renderKeypadPinyinOutput() {
      const childNodes = Array.from(pinyinDisplayArea.children);
      childNodes.forEach(node => {
        if (node.id !== 'pinyin-placeholder') {
          node.remove();
        }
      });

      if (keypadPinyinWords.length === 0) {
        pinyinPlaceholder.classList.remove('hidden');
      } else {
        pinyinPlaceholder.classList.add('hidden');
        keypadPinyinWords.forEach((wordObj, index) => {
          const item = document.createElement('span');
          item.innerText = wordObj.styled;
          item.className = "px-2.5 py-1 bg-indigo-50 border border-indigo-200 text-indigo-800 font-bold text-sm md:text-base rounded-md flex items-center gap-1 cursor-pointer hover:bg-rose-50 hover:border-rose-200 hover:text-rose-700 transition-colors duration-100";
          
          if (!isChecked) {
            item.title = "터치하여 제거";
            item.addEventListener('click', () => {
              keypadPinyinWords.splice(index, 1);
              renderKeypadPinyinOutput();
            });
          }
          
          pinyinDisplayArea.appendChild(item);
        });
      }
    }

    // --- HELPERS ---
    function isPunctuation(char) {
      return ["，", "。", "！", "？", "、"].includes(char);
    }

    function shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }

    function sanitizePinyin(str) {
      if (!str) return '';
      return str.toLowerCase()
        .normalize("NFD").replace(/[\u0300-\u036f]/g, "")
        .replace(/[^a-z0-9]/g, '');
    }

    // --- RESET DRAGGING ---
    resetDragBtn.addEventListener('click', () => {
      if (isChecked) return;
      const question = quizDatabase[currentIdx];
      placedCards = [];
      availableCards = shuffleArray([...question.hanzi]);
      renderCards();
    });

    // --- GRADING & CHECK ANSWER ---
    checkBtn.addEventListener('click', () => {
      if (isChecked) return;

      const question = quizDatabase[currentIdx];
      const userHanziStr = placedCards.join('').trim();
      
      let userPinyinStr = '';
      if (isKeypadMode) {
        userPinyinStr = keypadPinyinWords.map(w => w.styled).join(' ').trim();
      } else {
        userPinyinStr = pinyinKeyboardInput.value.trim();
      }

      // Check: Cards must all be placed
      if (availableCards.length > 0) {
        showWarningModal("모든 한자 카드를 어순에 맞게 놓아야 합니다!");
        return;
      }

      // Check: Pinyin input must not be empty
      if (userPinyinStr === '') {
        showWarningModal("한어병음이 입력되지 않았습니다. 병음 조각을 터치하거나 직접 기입하세요!");
        return;
      }

      isChecked = true;

      // Check results
      const correctHanziStr = question.hanzi.join('').trim();
      const isHanziCorrect = (userHanziStr === correctHanziStr);

      const cleanExpected = question.clean_pinyin.map(p => sanitizePinyin(p)).join('');
      const cleanUser = sanitizePinyin(userPinyinStr);
      const isPinyinCorrect = (cleanUser === cleanExpected);

      const isBothCorrect = isHanziCorrect && isPinyinCorrect;

      // Update Scores & UI
      if (isBothCorrect) {
        correctAnswers++;
        correctCount.innerText = correctAnswers;
        showFeedback(true, question);
      } else {
        wrongAnswers++;
        wrongCount.innerText = wrongAnswers;
        showFeedback(false, question, isHanziCorrect, isPinyinCorrect);
      }

      checkBtn.classList.add('hidden');
      nextBtn.classList.remove('hidden');
      pinyinKeyboardInput.disabled = true;
    });

    // --- SHOW FEEDBACK ---
    function showFeedback(isCorrect, question, isHanziCorrect = true, isPinyinCorrect = true) {
      feedbackBox.classList.remove('hidden');
      feedbackBox.className = "rounded-2xl p-4 border mb-5 transition-all duration-300 transform scale-100 opacity-100 ";
      
      if (isCorrect) {
        feedbackBox.classList.add('bg-emerald-50', 'border-emerald-200');
        feedbackIconContainer.className = "p-2 rounded-lg bg-emerald-500 text-white";
        feedbackIconContainer.innerHTML = `<i data-lucide="award" class="w-5 h-5"></i>`;
        feedbackTitle.innerText = "정답입니다! 잘하셨어요!";
        feedbackTitle.className = "text-sm font-bold text-emerald-800";
      } else {
        feedbackBox.classList.add('bg-rose-50', 'border-rose-200');
        feedbackIconContainer.className = "p-2 rounded-lg bg-rose-500 text-white";
        feedbackIconContainer.innerHTML = `<i data-lucide="alert-triangle" class="w-5 h-5"></i>`;
        
        let errorMsg = "틀린 부분이 있습니다.";
        if (!isHanziCorrect && !isPinyinCorrect) {
          errorMsg = "한자 순서와 한어병음이 모두 어색합니다.";
        } else if (!isHanziCorrect) {
          errorMsg = "한자 카드의 순서가 틀렸습니다.";
        } else if (!isPinyinCorrect) {
          errorMsg = "한어병음 표기나 기입 순서가 정확하지 않습니다.";
        }
        
        feedbackTitle.innerText = errorMsg;
        feedbackTitle.className = "text-sm font-bold text-rose-800";
      }

      feedbackCorrectHanzi.innerText = question.correct_string;
      feedbackCorrectPinyin.innerText = question.pinyin.join(' ');
      feedbackTip.innerText = question.tip;

      lucide.createIcons();
    }

    // --- WARNING BANNER ---
    function showWarningModal(msg) {
      let warningBanner = document.getElementById('warning-banner');
      if (!warningBanner) {
        warningBanner = document.createElement('div');
        warningBanner.id = 'warning-banner';
        warningBanner.className = "fixed bottom-5 left-1/2 transform -translate-x-1/2 bg-slate-900/95 text-white text-xs font-semibold px-5 py-3 rounded-full shadow-lg flex items-center gap-1.5 z-50 border border-slate-700 transition-all duration-200 opacity-0 scale-90";
        document.body.appendChild(warningBanner);
      }
      
      warningBanner.innerHTML = `<i data-lucide="info" class="w-3.5 h-3.5 text-rose-400"></i> ${msg}`;
      lucide.createIcons();
      
      warningBanner.classList.remove('opacity-0', 'scale-90');
      warningBanner.classList.add('opacity-100', 'scale-100');
      
      setTimeout(() => {
        warningBanner.classList.remove('opacity-100', 'scale-100');
        warningBanner.classList.add('opacity-0', 'scale-90');
      }, 2500);
    }

    // --- NEXT QUESTION ---
    nextBtn.addEventListener('click', () => {
      const nextIdx = (currentIdx + 1) % quizDatabase.length;
      loadQuestion(nextIdx);
    });

    window.onload = () => {
      initApp();
    };
  </script>
</body>
</html>

```
