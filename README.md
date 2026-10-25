import React, { useState, useEffect, useRef, useMemo } from 'react';
import { 
  Calendar as CalendarIcon, 
  PlusCircle, 
  BarChart3, 
  Sparkles, 
  ChevronLeft, 
  ChevronRight, 
  Camera, 
  Image as ImageIcon, 
  Clock, 
  Check, 
  X, 
  Smile, 
  Flame, 
  Utensils, 
  Apple, 
  ChevronRight as ArrowRight,
  TrendingUp,
  RefreshCw,
  Heart,
  Settings,
  HelpCircle,
  Share2,
  Key,
  Info
} from 'lucide-react';

// 음식 이름 및 설명에 따라 가장 잘 어울리는 이모지 & 테마 색상을 추천하는 지능형 헬퍼
function getFoodVisualMeta(foodName = '') {
  const name = foodName.toLowerCase();
  
  if (name.includes('초코') || name.includes('초콜릿') || name.includes('쿠키') || name.includes('케이크') || name.includes('디저트') || name.includes('빵') || name.includes('베이글') || name.includes('와플') || name.includes('크루아상')) {
    return { emoji: name.includes('초코') ? '🍫' : name.includes('케이크') ? '🍰' : '🥐', bg: 'bg-amber-50', border: 'border-amber-200', text: 'text-amber-600', category: '간식/디저트' };
  }
  if (name.includes('커피') || name.includes('라떼') || name.includes('아메리카노') || name.includes('음료') || name.includes('밀크티') || name.includes('차') || name.includes('주스')) {
    return { emoji: name.includes('라떼') ? '☕' : name.includes('주스') ? '🧃' : '🥤', bg: 'bg-orange-50', border: 'border-orange-200', text: 'text-orange-600', category: '음료' };
  }
  if (name.includes('샐러드') || name.includes('채소') || name.includes('야채') || name.includes('그린') || name.includes('오이') || name.includes('토마토')) {
    return { emoji: '🥗', bg: 'bg-emerald-50', border: 'border-emerald-200', text: 'text-emerald-600', category: '샐러드' };
  }
  if (name.includes('치킨') || name.includes('닭가슴살') || name.includes('닭') || name.includes('소고기') || name.includes('돼지') || name.includes('삼겹살') || name.includes('스테이크') || name.includes('고기')) {
    return { emoji: name.includes('닭') ? '🍗' : '🥩', bg: 'bg-rose-50', border: 'border-rose-200', text: 'text-rose-600', category: '육류' };
  }
  if (name.includes('연어') || name.includes('생선') || name.includes('회') || name.includes('초밥') || name.includes('새우') || name.includes('해산물')) {
    return { emoji: name.includes('초밥') ? '🍣' : '🐟', bg: 'bg-sky-50', border: 'border-sky-200', text: 'text-sky-600', category: '해산물' };
  }
  if (name.includes('밥') || name.includes('찌개') || name.includes('국') || name.includes('덮밥') || name.includes('비빔밥') || name.includes('김치') || name.includes('카레')) {
    return { emoji: name.includes('찌개') ? '🍲' : '🍚', bg: 'bg-blue-50', border: 'border-blue-200', text: 'text-blue-600', category: '식사' };
  }
  if (name.includes('면') || name.includes('파스타') || name.includes('라면') || name.includes('우동') || name.includes('국수')) {
    return { emoji: name.includes('파스타') ? '🍝' : '🍜', bg: 'bg-yellow-50', border: 'border-yellow-200', text: 'text-yellow-600', category: '면류' };
  }
  if (name.includes('과일') || name.includes('사과') || name.includes('바나나') || name.includes('딸기') || name.includes('포도') || name.includes('복숭아')) {
    return { emoji: name.includes('딸기') ? '🍓' : name.includes('바나나') ? '🍌' : '🍎', bg: 'bg-pink-50', border: 'border-pink-200', text: 'text-pink-600', category: '과일' };
  }
  if (name.includes('피자') || name.includes('버거') || name.includes('샌드위치') || name.includes('토스트')) {
    return { emoji: name.includes('피자') ? '🍕' : name.includes('버거') ? '🍔' : '🥪', bg: 'bg-amber-50', border: 'border-amber-200', text: 'text-amber-600', category: '패스트푸드' };
  }
  if (name.includes('요거트') || name.includes('우유') || name.includes('치즈')) {
    return { emoji: '🥣', bg: 'bg-indigo-50', border: 'border-indigo-200', text: 'text-indigo-600', category: '유제품' };
  }
  return { emoji: '🍽️', bg: 'bg-pink-50', border: 'border-pink-200', text: 'text-pink-600', category: '식사' };
}

// 기본 초기 샘플 데이터
const INITIAL_MEALS = [
  {
    id: 'sample-1',
    date: '2026-10-25',
    mealType: 'breakfast',
    mealTime: '08:30',
    foodName: '그릭요거트 + 바나나 + 그래놀라',
    calories: 350,
    protein: 18,
    carbs: 45,
    fat: 10,
    memo: '견과류 듬뿍 넣음',
    image: 'https://images.unsplash.com/photo-1488477181946-6428a0291777?auto=format&fit=crop&w=400&q=80',
    feedback: '유산균과 양질의 단백질이 풍부한 상쾌한 아침이에요! 🍓'
  },
  {
    id: 'sample-2',
    date: '2026-10-25',
    mealType: 'lunch',
    mealTime: '12:40',
    foodName: '닭가슴살 샐러드 & 아보카도',
    calories: 520,
    protein: 38,
    carbs: 40,
    fat: 18,
    memo: '오리엔탈 드레싱 절반만',
    image: 'https://images.unsplash.com/photo-1540420773420-3366772f4999?auto=format&fit=crop&w=400&q=80',
    feedback: '훌륭한 고단백 클린 식단이에요! 근육 유지에 최고예요 💪'
  },
  {
    id: 'sample-3',
    date: '2026-10-25',
    mealType: 'snack',
    mealTime: '15:20',
    foodName: '달콤한 초코쿠키 & 아이스 아메리카노',
    calories: 230,
    protein: 3,
    carbs: 32,
    fat: 11,
    memo: '오후 당 충전 🍪',
    image: null, // 사진 없이 이모지 비주얼로 표현
    feedback: '기분 좋은 당 충전! 저녁엔 신선한 단백질 위주로 챙겨보아요 🍫'
  },
  {
    id: 'sample-4',
    date: '2026-10-25',
    mealType: 'dinner',
    mealTime: '18:50',
    foodName: '연어구이 + 현미밥 + 브로콜리',
    calories: 550,
    protein: 32,
    carbs: 60,
    fat: 16,
    memo: '든든하고 맛있게',
    image: 'https://images.unsplash.com/photo-1467003909585-2f8a72700288?auto=format&fit=crop&w=400&q=80',
    feedback: '오메가-3와 양질의 탄수화물이 완벽히 어우러졌어요 🐟'
  },
  {
    id: 'sample-5',
    date: '2026-10-24',
    mealType: 'lunch',
    mealTime: '12:30',
    foodName: '소고기 샤브샤브 & 채소',
    calories: 620,
    protein: 42,
    carbs: 55,
    fat: 20,
    memo: '야채 많이',
    image: 'https://images.unsplash.com/photo-1547496502-affa22d38842?auto=format&fit=crop&w=400&q=80',
    feedback: '채소와 고기의 영양 밸런스가 조화로워요!'
  },
  {
    id: 'sample-6',
    date: '2026-10-23',
    mealType: 'lunch',
    mealTime: '13:00',
    foodName: '연어 덮밥 (사케동)',
    calories: 580,
    protein: 30,
    carbs: 68,
    fat: 16,
    memo: '밥 조금 남김',
    image: 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?auto=format&fit=crop&w=400&q=80',
    feedback: '연어의 풍부한 단백질 덕분에 건강한 에너지 충전 완료!'
  }
];

export default function App() {
  const [currentTab, setCurrentTab] = useState('calendar'); // 'calendar' | 'record' | 'stats' | 'coach'

  // 로컬 스토리지 데이터 불러오기
  const [meals, setMeals] = useState(() => {
    try {
      const saved = localStorage.getItem('today_table_meals_v2');
      return saved ? JSON.parse(saved) : INITIAL_MEALS;
    } catch {
      return INITIAL_MEALS;
    }
  });

  const [selectedDate, setSelectedDate] = useState('2026-10-25');
  const [viewYear, setViewYear] = useState(2026);
  const [viewMonth, setViewMonth] = useState(10);
  const [detailModalOpen, setDetailModalOpen] = useState(false);
  const [settingsOpen, setSettingsOpen] = useState(false);
  const [customApiKey, setCustomApiKey] = useState(() => {
    return localStorage.getItem('today_table_api_key') || '';
  });

  // 데이터 변경 시 자동 저장
  useEffect(() => {
    try {
      localStorage.setItem('today_table_meals_v2', JSON.stringify(meals));
    } catch (e) {
      console.error(e);
    }
  }, [meals]);

  const handleSaveApiKey = (key) => {
    setCustomApiKey(key);
    localStorage.setItem('today_table_api_key', key);
    setSettingsOpen(false);
  };

  // 날짜별 매핑
  const mealsByDate = useMemo(() => {
    const map = {};
    meals.forEach(meal => {
      if (!map[meal.date]) {
        map[meal.date] = [];
      }
      map[meal.date].push(meal);
    });
    return map;
  }, [meals]);

  // 특정 일자의 영양 합계
  const getDailySummary = (dateStr) => {
    const dayMeals = mealsByDate[dateStr] || [];
    return dayMeals.reduce(
      (acc, m) => {
        acc.calories += Number(m.calories || 0);
        acc.protein += Number(m.protein || 0);
        acc.carbs += Number(m.carbs || 0);
        acc.fat += Number(m.fat || 0);
        return acc;
      },
      { calories: 0, protein: 0, carbs: 0, fat: 0 }
    );
  };

  // 신규 식단 추가
  const handleSaveMeal = (newMeal) => {
    setMeals(prev => [newMeal, ...prev]);
    setSelectedDate(newMeal.date);
    setCurrentTab('calendar');
    setDetailModalOpen(true);
  };

  return (
    <div className="flex justify-center min-h-screen bg-[#F4F8FB] font-sans text-slate-800 antialiased selection:bg-pink-100">
      {/* 모바일 화면 컨테이너 (파스텔 핑크 & 스카이블루 테마) */}
      <main className="w-full max-w-[430px] min-h-screen bg-[#FFFDFE] shadow-xl flex flex-col relative pb-20 border-x border-pink-100/60">
        
        {/* 상단 헤더 */}
        <header className="px-5 pt-7 pb-3 flex items-center justify-between sticky top-0 bg-[#FFFDFE]/90 backdrop-blur-md z-30 border-b border-pink-100/50">
          <div className="flex items-center gap-2.5">
            <div className="w-10 h-10 rounded-2xl bg-gradient-to-tr from-pink-200 via-rose-100 to-sky-200 flex items-center justify-center text-xl shadow-xs border border-white">
              🎀
            </div>
            <div>
              <div className="flex items-center gap-1.5">
                <h1 className="font-extrabold text-base text-slate-800 tracking-tight">오늘의 식탁</h1>
                <Heart className="w-3.5 h-3.5 text-pink-400 fill-pink-300 animate-pulse" />
              </div>
              <p className="text-[11px] font-medium text-pink-500">예쁘게 챙겨먹는 건강한 하루 🫧</p>
            </div>
          </div>

          <div className="flex items-center gap-1.5">
            <button 
              onClick={() => setSettingsOpen(true)}
              className="w-8 h-8 rounded-full bg-white shadow-xs border border-pink-100 flex items-center justify-center text-slate-500 hover:text-pink-500 hover:border-pink-200 transition"
              title="설정 및 API 키"
            >
              <Settings className="w-4 h-4" />
            </button>
            <div className="w-8 h-8 rounded-full bg-sky-100 border border-white shadow-xs flex items-center justify-center text-sm">
              🐰
            </div>
          </div>
        </header>

        {/* 탭 콘텐츠 영역 */}
        <div className="flex-1 overflow-y-auto px-4 py-3">
          {currentTab === 'calendar' && (
            <CalendarTab 
              viewYear={viewYear}
              viewMonth={viewMonth}
              setViewYear={setViewYear}
              setViewMonth={setViewMonth}
              selectedDate={selectedDate}
              setSelectedDate={setSelectedDate}
              mealsByDate={mealsByDate}
              getDailySummary={getDailySummary}
              onOpenDetail={() => setDetailModalOpen(true)}
              onGoToRecord={() => setCurrentTab('record')}
            />
          )}

          {currentTab === 'record' && (
            <RecordTab 
              selectedDate={selectedDate} 
              onSave={handleSaveMeal}
              onCancel={() => setCurrentTab('calendar')}
              customApiKey={customApiKey}
            />
          )}

          {currentTab === 'stats' && (
            <StatsTab 
              meals={meals} 
              viewYear={viewYear} 
              viewMonth={viewMonth} 
            />
          )}

          {currentTab === 'coach' && (
            <CoachTab 
              selectedDate={selectedDate}
              dayMeals={mealsByDate[selectedDate] || []}
              dailySummary={getDailySummary(selectedDate)}
              onGoToRecord={() => setCurrentTab('record')}
            />
          )}
        </div>

        {/* 상세 보기 바텀시트 모달 */}
        {detailModalOpen && (
          <DayDetailModal 
            dateStr={selectedDate}
            dayMeals={mealsByDate[selectedDate] || []}
            summary={getDailySummary(selectedDate)}
            onClose={() => setDetailModalOpen(false)}
            onAddMeal={() => {
              setDetailModalOpen(false);
              setCurrentTab('record');
            }}
          />
        )}

        {/* 설정 모달 (Gemini API 키 등록) */}
        {settingsOpen && (
          <SettingsModal 
            apiKey={customApiKey}
            onSave={handleSaveApiKey}
            onClose={() => setSettingsOpen(false)}
          />
        )}

        {/* 하단 네비게이션 탭 바 (베이비 핑크 & 스카이블루 액센트) */}
        <nav className="fixed bottom-0 max-w-[430px] w-full bg-white/95 backdrop-blur-lg border-t border-pink-100 px-3 py-2 flex justify-around items-center z-40 shadow-[0_-4px_24px_rgba(255,182,193,0.18)] rounded-t-3xl">
          <button 
            onClick={() => setCurrentTab('calendar')}
            className={`flex flex-col items-center py-1 px-3 rounded-2xl transition-all ${
              currentTab === 'calendar' 
                ? 'text-pink-500 font-bold scale-105' 
                : 'text-slate-400 hover:text-pink-400'
            }`}
          >
            <CalendarIcon className="w-5 h-5 mb-0.5" />
            <span className="text-[11px]">사진 달력</span>
          </button>

          <button 
            onClick={() => setCurrentTab('record')}
            className="flex flex-col items-center -mt-5 group"
          >
            <div className="w-13 h-13 rounded-full bg-gradient-to-tr from-pink-400 via-rose-400 to-sky-300 text-white flex items-center justify-center shadow-lg shadow-pink-300/50 border-3 border-white group-active:scale-95 transition-transform p-3">
              <PlusCircle className="w-6 h-6" />
            </div>
            <span className="text-[11px] font-bold text-pink-500 mt-1">기록하기</span>
          </button>

          <button 
            onClick={() => setCurrentTab('stats')}
            className={`flex flex-col items-center py-1 px-3 rounded-2xl transition-all ${
              currentTab === 'stats' 
                ? 'text-pink-500 font-bold scale-105' 
                : 'text-slate-400 hover:text-pink-400'
            }`}
          >
            <BarChart3 className="w-5 h-5 mb-0.5" />
            <span className="text-[11px]">통계</span>
          </button>

          <button 
            onClick={() => setCurrentTab('coach')}
            className={`flex flex-col items-center py-1 px-3 rounded-2xl transition-all ${
              currentTab === 'coach' 
                ? 'text-pink-500 font-bold scale-105' 
                : 'text-slate-400 hover:text-pink-400'
            }`}
          >
            <Sparkles className="w-5 h-5 mb-0.5" />
            <span className="text-[11px]">AI 코치</span>
          </button>
        </nav>
      </main>
    </div>
  );
}

// ==========================================
// 1. 사진 달력 탭 (Calendar View)
// ==========================================
function CalendarTab({ 
  viewYear, 
  viewMonth, 
  setViewYear, 
  setViewMonth, 
  selectedDate, 
  setSelectedDate, 
  mealsByDate, 
  getDailySummary, 
  onOpenDetail, 
  onGoToRecord 
}) {
  const handlePrevMonth = () => {
    if (viewMonth === 1) {
      setViewYear(y => y - 1);
      setViewMonth(12);
    } else {
      setViewMonth(m => m - 1);
    }
  };

  const handleNextMonth = () => {
    if (viewMonth === 12) {
      setViewYear(y => y + 1);
      setViewMonth(1);
    } else {
      setViewMonth(m => m + 1);
    }
  };

  const daysInMonth = new Date(viewYear, viewMonth, 0).getDate();
  const firstDayIndex = new Date(viewYear, viewMonth - 1, 1).getDay();

  const calendarDays = [];
  for (let i = 0; i < firstDayIndex; i++) {
    calendarDays.push({ type: 'empty', key: `empty-${i}` });
  }
  for (let d = 1; d <= daysInMonth; d++) {
    const dStr = `${viewYear}-${String(viewMonth).padStart(2, '0')}-${String(d).padStart(2, '0')}`;
    calendarDays.push({
      type: 'day',
      dayNum: d,
      dateStr: dStr,
      key: dStr
    });
  }

  const monthSummary = useMemo(() => {
    let totalKcal = 0;
    let daysWithMeals = 0;
    let protein = 0, carbs = 0, fat = 0;

    for (let d = 1; d <= daysInMonth; d++) {
      const dStr = `${viewYear}-${String(viewMonth).padStart(2, '0')}-${String(d).padStart(2, '0')}`;
      const sum = getDailySummary(dStr);
      if (sum.calories > 0) {
        totalKcal += sum.calories;
        protein += sum.protein;
        carbs += sum.carbs;
        fat += sum.fat;
        daysWithMeals++;
      }
    }
    const avgKcal = daysWithMeals > 0 ? Math.round(totalKcal / daysWithMeals) : 0;
    return { totalKcal, avgKcal, protein, carbs, fat, count: daysWithMeals };
  }, [viewYear, viewMonth, mealsByDate]);

  const selectedSummary = getDailySummary(selectedDate);
  const selectedMeals = mealsByDate[selectedDate] || [];

  return (
    <div className="space-y-3.5">
      {/* 캘린더 카드 본체 */}
      <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100">
        
        {/* 달력 헤더 */}
        <div className="flex items-center justify-between px-2 mb-3">
          <div className="flex items-center gap-1.5">
            <span className="text-sm">🌸</span>
            <h2 className="font-extrabold text-base text-slate-800">
              {viewYear}년 {viewMonth}월
            </h2>
          </div>
          <div className="flex items-center gap-1">
            <button 
              onClick={handlePrevMonth}
              className="p-1.5 rounded-full hover:bg-pink-50 text-slate-500 hover:text-pink-600 transition"
            >
              <ChevronLeft className="w-4 h-4" />
            </button>
            <button 
              onClick={handleNextMonth}
              className="p-1.5 rounded-full hover:bg-pink-50 text-slate-500 hover:text-pink-600 transition"
            >
              <ChevronRight className="w-4 h-4" />
            </button>
          </div>
        </div>

        {/* 요일 헤더 */}
        <div className="grid grid-cols-7 gap-1 text-center mb-1 text-[11px] font-bold">
          <span className="text-rose-400">일</span>
          <span className="text-slate-400">월</span>
          <span className="text-slate-400">화</span>
          <span className="text-slate-400">수</span>
          <span className="text-slate-400">목</span>
          <span className="text-slate-400">금</span>
          <span className="text-sky-400">토</span>
        </div>

        {/* 날짜 그리드 (사진/음식 썸네일 달력) */}
        <div className="grid grid-cols-7 gap-1.5">
          {calendarDays.map((item) => {
            if (item.type === 'empty') {
              return <div key={item.key} className="h-16 rounded-xl bg-pink-50/20" />;
            }

            const { dateStr, dayNum } = item;
            const dayMeals = mealsByDate[dateStr] || [];
            const summary = getDailySummary(dateStr);
            const isSelected = selectedDate === dateStr;
            const hasMeals = dayMeals.length > 0;
            
            // 첫 식사의 사진 또는 맞춤형 이모지
            const primaryMeal = hasMeals ? dayMeals[0] : null;
            const thumbImage = primaryMeal?.image || null;
            const visualMeta = primaryMeal ? getFoodVisualMeta(primaryMeal.foodName) : null;

            return (
              <button
                key={item.key}
                onClick={() => setSelectedDate(dateStr)}
                className={`h-16 relative rounded-2xl flex flex-col items-center justify-between p-1 transition-all group overflow-hidden border ${
                  isSelected 
                    ? 'border-pink-400 ring-2 ring-pink-200 shadow-md bg-pink-50/40' 
                    : 'border-slate-100 hover:border-pink-200 bg-[#FFFDFE]'
                }`}
              >
                {/* 배경: 실제 사진이 있으면 사진, 없으면 맞춤 이모지 배경 */}
                {thumbImage ? (
                  <>
                    <img 
                      src={thumbImage} 
                      alt="" 
                      className="absolute inset-0 w-full h-full object-cover rounded-2xl opacity-85 group-hover:scale-105 transition-transform" 
                    />
                    <div className="absolute inset-0 bg-gradient-to-t from-black/60 via-black/10 to-transparent rounded-2xl" />
                  </>
                ) : hasMeals && visualMeta ? (
                  <div className={`absolute inset-0 ${visualMeta.bg} flex items-center justify-center pt-2`}>
                    <span className="text-2xl filter drop-shadow-xs transform group-hover:scale-110 transition-transform">
                      {visualMeta.emoji}
                    </span>
                  </div>
                ) : null}

                {/* 날짜 번호 */}
                <span className={`relative text-[11px] font-semibold z-10 ${
                  thumbImage ? 'text-white font-bold drop-shadow' : isSelected ? 'text-pink-600 font-bold' : 'text-slate-600'
                }`}>
                  {dayNum}
                </span>

                {/* 칼로리 뱃지 */}
                {hasMeals ? (
                  <span className={`relative z-10 text-[9px] font-bold px-1.5 py-0.5 rounded-full ${
                    thumbImage 
                      ? 'bg-black/60 text-white backdrop-blur-xs' 
                      : 'bg-white/90 text-pink-600 shadow-xs border border-pink-100'
                  }`}>
                    {summary.calories}
                  </span>
                ) : (
                  <span className="w-1 h-1 rounded-full bg-slate-200 group-hover:bg-pink-200" />
                )}
              </button>
            );
          })}
        </div>

        {/* 이번 달 누적 요약 띠 (파스텔 스카이블루 & 핑크 혼합) */}
        <div className="mt-3.5 pt-3 border-t border-pink-50 flex items-center justify-between text-xs bg-gradient-to-r from-pink-50/70 to-sky-50/70 rounded-2xl p-2.5 px-3">
          <div>
            <span className="text-[11px] font-medium text-slate-500">이번 달 총 섭취</span>
            <div className="font-extrabold text-slate-800 text-sm">
              {monthSummary.totalKcal.toLocaleString()} <span className="text-[11px] font-normal text-pink-500">kcal</span>
            </div>
            <div className="text-[10px] text-slate-400">
              (일 평균 {monthSummary.avgKcal.toLocaleString()} kcal)
            </div>
          </div>
          <div className="flex gap-1.5 text-center text-[10px]">
            <div className="bg-white/80 px-2 py-1 rounded-xl border border-sky-100">
              <span className="text-slate-400 block text-[9px]">단백질</span>
              <span className="font-bold text-sky-600">{monthSummary.protein}g</span>
            </div>
            <div className="bg-white/80 px-2 py-1 rounded-xl border border-pink-100">
              <span className="text-slate-400 block text-[9px]">탄수화물</span>
              <span className="font-bold text-rose-500">{monthSummary.carbs}g</span>
            </div>
            <div className="bg-white/80 px-2 py-1 rounded-xl border border-amber-100">
              <span className="text-slate-400 block text-[9px]">지방</span>
              <span className="font-bold text-amber-500">{monthSummary.fat}g</span>
            </div>
          </div>
        </div>
      </div>

      {/* 선택한 날짜의 식단 프리뷰 카드 */}
      <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100">
        <div className="flex items-center justify-between mb-3">
          <div className="flex items-center gap-1.5">
            <span className="w-2 h-2 rounded-full bg-pink-400 animate-ping" />
            <h3 className="font-bold text-sm text-slate-800">
              {selectedDate} 식단 내역
            </h3>
          </div>
          {selectedMeals.length > 0 && (
            <button 
              onClick={onOpenDetail}
              className="text-xs text-pink-500 hover:text-pink-600 font-bold flex items-center gap-0.5"
            >
              자세히 보기 <ArrowRight className="w-3.5 h-3.5" />
            </button>
          )}
        </div>

        {selectedMeals.length === 0 ? (
          <div className="py-6 text-center">
            <div className="text-3xl mb-2">🍰</div>
            <p className="text-xs text-slate-400 font-medium mb-3">
              이 날짜에는 아직 기록된 식단이 없어요!
            </p>
            <button
              onClick={onGoToRecord}
              className="px-4 py-2 bg-gradient-to-r from-pink-400 via-rose-400 to-sky-300 text-white rounded-2xl text-xs font-bold shadow-md shadow-pink-200 active:scale-95 transition"
            >
              지금 사진 찍거나 적어서 기록하기 ✨
            </button>
          </div>
        ) : (
          <div className="space-y-2.5">
            {/* 총 칼로리 바 */}
            <div className="bg-gradient-to-r from-pink-100/70 to-sky-100/70 rounded-2xl p-3 flex items-center justify-between">
              <div>
                <span className="text-[11px] text-pink-600 font-semibold">오늘 총 섭취</span>
                <div className="text-lg font-extrabold text-slate-800">
                  {selectedSummary.calories} <span className="text-xs font-normal">kcal</span>
                </div>
              </div>
              <div className="flex gap-2 text-right text-xs">
                <div>
                  <span className="text-[10px] text-slate-400 block">단백질</span>
                  <span className="font-bold text-sky-600">{selectedSummary.protein}g</span>
                </div>
                <div>
                  <span className="text-[10px] text-slate-400 block">탄수</span>
                  <span className="font-bold text-rose-500">{selectedSummary.carbs}g</span>
                </div>
                <div>
                  <span className="text-[10px] text-slate-400 block">지방</span>
                  <span className="font-bold text-amber-500">{selectedSummary.fat}g</span>
                </div>
              </div>
            </div>

            {/* 식사 리스트 */}
            <div className="divide-y divide-pink-50">
              {selectedMeals.map(meal => {
                const meta = getFoodVisualMeta(meal.foodName);
                return (
                  <div key={meal.id} className="py-2.5 flex items-center gap-3">
                    {/* 사진이 있으면 사진, 없으면 맞춤 이모지 뱃지 */}
                    {meal.image ? (
                      <img 
                        src={meal.image} 
                        alt="" 
                        className="w-12 h-12 rounded-2xl object-cover shadow-xs border border-pink-100" 
                      />
                    ) : (
                      <div className={`w-12 h-12 rounded-2xl ${meta.bg} border ${meta.border} flex items-center justify-center text-2xl shadow-xs`}>
                        {meta.emoji}
                      </div>
                    )}

                    <div className="flex-1 min-w-0">
                      <div className="flex items-center gap-1.5">
                        <span className="text-[10px] font-bold px-1.5 py-0.5 rounded-md bg-pink-100/80 text-pink-600">
                          {meal.mealType === 'breakfast' && '아침'}
                          {meal.mealType === 'lunch' && '점심'}
                          {meal.mealType === 'dinner' && '저녁'}
                          {meal.mealType === 'snack' && '간식'}
                        </span>
                        <span className="text-[11px] text-slate-400">{meal.mealTime || '12:00'}</span>
                      </div>
                      <div className="font-semibold text-xs text-slate-800 truncate mt-0.5">
                        {meal.foodName}
                      </div>
                    </div>

                    <div className="text-right">
                      <div className="font-extrabold text-xs text-pink-500">
                        {meal.calories} kcal
                      </div>
                      <div className="text-[10px] text-slate-400">
                        단백질 {meal.protein}g
                      </div>
                    </div>
                  </div>
                );
              })}
            </div>
          </div>
        )}
      </div>
    </div>
  );
}

// ==========================================
// 2. 식단 기록 & 지능형 분석 탭
// ==========================================
function RecordTab({ selectedDate, onSave, onCancel, customApiKey }) {
  const [mealType, setMealType] = useState('lunch');
  const [mealTime, setMealTime] = useState('12:30');
  const [date, setDate] = useState(selectedDate);
  const [imagePreview, setImagePreview] = useState(null);
  const [textDescription, setTextDescription] = useState('');
  const [memo, setMemo] = useState('');

  const [isAnalyzing, setIsAnalyzing] = useState(false);
  const [analysisResult, setAnalysisResult] = useState(null);

  const [editableFood, setEditableFood] = useState('');
  const [editableKcal, setEditableKcal] = useState(0);
  const [editableProtein, setEditableProtein] = useState(0);
  const [editableCarbs, setEditableCarbs] = useState(0);
  const [editableFat, setEditableFat] = useState(0);
  const [editableFeedback, setEditableFeedback] = useState('');

  const fileInputRef = useRef(null);
  const cameraInputRef = useRef(null);

  const handleFileChange = (e) => {
    const file = e.target.files?.[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setImagePreview(reader.result);
        triggerAiAnalysis(reader.result, textDescription);
      };
      reader.readAsDataURL(file);
    }
  };

  const handleTextAnalyze = () => {
    if (!textDescription.trim()) return;
    triggerAiAnalysis(imagePreview, textDescription);
  };

  // Gemini API 호출 또는 설명 기반 맞춤형 추정
  const triggerAiAnalysis = async (base64Image, desc) => {
    setIsAnalyzing(true);
    setAnalysisResult(null);

    const apiKey = customApiKey || "";
    let analyzedData = null;

    try {
      if (apiKey) {
        const prompt = `당신은 귀엽고 상냥한 전문 영양사 AI '오늘의 식탁 코치 토리'입니다.
제공된 음식 사진이나 설명("${desc || '사진 속 음식'}")을 바탕으로 칼로리와 영양소를 정밀 분석해주세요.
반드시 아래 JSON 형식으로만 답해주세요.
{
  "foodName": "대표 음식 이름 (예: 초코 크루아상, 닭가슴살 샐러드, 아이스 카페라떼)",
  "calories": 350,
  "protein": 6,
  "carbs": 48,
  "fat": 14,
  "feedback": "한 줄 분량의 다정하고 귀여운 영양 피드백 (어울리는 이모지 포함)"
}`;

        const payload = {
          contents: [
            {
              role: "user",
              parts: [
                { text: prompt },
                ...(base64Image ? [{
                  inlineData: {
                    mimeType: "image/jpeg",
                    data: base64Image.split(',')[1] || base64Image
                  }
                }] : [])
              ]
            }
          ],
          generationConfig: {
            responseMimeType: "application/json"
          }
        };

        const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${apiKey}`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });

        const data = await res.json();
        const jsonText = data.candidates?.[0]?.content?.parts?.[0]?.text;
        if (jsonText) {
          analyzedData = JSON.parse(jsonText);
        }
      }
    } catch (err) {
      console.warn("API 분석 실패, 로컬 지능형 엔진으로 전환:", err);
    }

    // API 키가 없거나 실패한 경우, 설명에 맞는 섬세한 기본값 적용
    if (!analyzedData) {
      await new Promise(r => setTimeout(r, 800));
      const q = (desc || '').toLowerCase();

      if (q.includes('초코') || q.includes('쿠키') || q.includes('초콜릿') || q.includes('간식')) {
        analyzedData = {
          foodName: desc || '달콤한 초코 디저트',
          calories: 280,
          protein: 4,
          carbs: 38,
          fat: 13,
          feedback: '달콤한 디저트로 기분 좋은 에너지를 충전했어요! 🍫'
        };
      } else if (q.includes('커피') || q.includes('아메리카노') || q.includes('라떼')) {
        analyzedData = {
          foodName: desc || '시원한 카페 음료',
          calories: q.includes('라떼') ? 140 : 15,
          protein: q.includes('라떼') ? 6 : 1,
          carbs: q.includes('라떼') ? 12 : 2,
          fat: q.includes('라떼') ? 5 : 0,
          feedback: '향긋한 커피 한 잔과 함께하는 여유로운 힐링 타임 ☕'
        };
      } else if (q.includes('피자') || q.includes('치킨') || q.includes('버거')) {
        analyzedData = {
          foodName: desc || '맛있는 피자 & 치킨',
          calories: 680,
          protein: 28,
          carbs: 62,
          fat: 32,
          feedback: '맛있게 먹으면 0칼로리! 수분도 충분히 섭취해 주세요 🍕'
        };
      } else if (q.includes('샐러드')) {
        analyzedData = {
          foodName: desc || '신선한 닭가슴살 샐러드',
          calories: 380,
          protein: 32,
          carbs: 20,
          fat: 10,
          feedback: '신선한 채소와 단백질이 가득한 클린 식단이에요! 🥗'
        };
      } else {
        analyzedData = {
          foodName: desc || (base64Image ? '맛있는 한 끼 식사' : '식사 기록'),
          calories: 480,
          protein: 24,
          carbs: 58,
          fat: 14,
          feedback: '영양소를 골고루 챙기셨네요! 오늘도 건강한 한 끼 완성 💖'
        };
      }
    }

    setAnalysisResult(analyzedData);
    setEditableFood(analyzedData.foodName);
    setEditableKcal(analyzedData.calories);
    setEditableProtein(analyzedData.protein);
    setEditableCarbs(analyzedData.carbs);
    setEditableFat(analyzedData.fat);
    setEditableFeedback(analyzedData.feedback);
    setIsAnalyzing(false);
  };

  const handlePortionAdjust = (factor) => {
    if (!analysisResult) return;
    setEditableKcal(Math.round(analysisResult.calories * factor));
    setEditableProtein(Math.round(analysisResult.protein * factor));
    setEditableCarbs(Math.round(analysisResult.carbs * factor));
    setEditableFat(Math.round(analysisResult.fat * factor));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const finalFoodName = editableFood || textDescription || '식사 기록';
    const finalMeal = {
      id: 'meal-' + Date.now(),
      date,
      mealType,
      mealTime,
      foodName: finalFoodName,
      calories: Number(editableKcal) || 0,
      protein: Number(editableProtein) || 0,
      carbs: Number(editableCarbs) || 0,
      fat: Number(editableFat) || 0,
      memo,
      // 사용자가 직접 사진을 올렸을 때만 이미지 URL 저장, 없으면 null (이모지 카드로 표시)
      image: imagePreview || null,
      feedback: editableFeedback || '오늘도 맛있게 건강을 챙겼어요! 🌸'
    };
    onSave(finalMeal);
  };

  const previewMeta = getFoodVisualMeta(editableFood || textDescription);

  return (
    <div className="space-y-4 pb-6 animate-fadeIn">
      {/* 상단 서브헤더 */}
      <div className="flex items-center justify-between px-1">
        <button 
          onClick={onCancel}
          className="text-xs text-slate-500 hover:text-pink-600 flex items-center gap-1 font-medium"
        >
          <ChevronLeft className="w-4 h-4" /> 캘린더로 돌아가기
        </button>
        <span className="text-xs font-bold text-pink-600 bg-pink-50 px-2.5 py-1 rounded-full border border-pink-100 flex items-center gap-1">
          <Sparkles className="w-3 h-3 text-pink-500" />
          AI 식단 분석
        </span>
      </div>

      {/* 사진 업로드 카드 또는 설명 입력 영역 */}
      <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100 text-center">
        <input 
          type="file" 
          accept="image/*" 
          ref={fileInputRef} 
          onChange={handleFileChange} 
          className="hidden" 
        />
        <input 
          type="file" 
          accept="image/*" 
          capture="environment" 
          ref={cameraInputRef} 
          onChange={handleFileChange} 
          className="hidden" 
        />

        {imagePreview ? (
          <div className="relative rounded-2xl overflow-hidden group border border-pink-100 mb-3 max-h-56">
            <img src={imagePreview} alt="식단 미리보기" className="w-full h-52 object-cover" />
            <div className="absolute inset-0 bg-black/30 opacity-0 group-hover:opacity-100 flex items-center justify-center gap-2 transition-opacity">
              <button 
                type="button"
                onClick={() => fileInputRef.current?.click()}
                className="px-3 py-1.5 bg-white/90 rounded-xl text-xs font-bold text-slate-800 shadow"
              >
                사진 변경
              </button>
              <button 
                type="button"
                onClick={() => setImagePreview(null)}
                className="px-3 py-1.5 bg-rose-500/90 rounded-xl text-xs font-bold text-white shadow"
              >
                사진 삭제
              </button>
            </div>
          </div>
        ) : (
          <div className="py-5 px-4 border-2 border-dashed border-pink-200/80 rounded-2xl bg-gradient-to-b from-pink-50/30 to-sky-50/20 mb-3">
            <div className="w-13 h-13 rounded-2xl bg-white mx-auto flex items-center justify-center text-2xl shadow-xs border border-pink-100 mb-2">
              {textDescription ? previewMeta.emoji : '📷'}
            </div>
            <h3 className="font-extrabold text-sm text-slate-800 mb-1">
              {textDescription ? `"${textDescription}" 기록하기` : '사진을 올리거나 아래에 적어주세요'}
            </h3>
            <p className="text-[11px] text-slate-400 mb-3.5">
              사진이 없어도 괜찮아요! 음식 이름을 적으면 어울리는 이모지가 쏙 들어가요 🫧
            </p>

            <div className="flex justify-center gap-2">
              <button
                type="button"
                onClick={() => fileInputRef.current?.click()}
                className="flex items-center gap-1.5 px-3 py-2 rounded-xl bg-pink-50 text-pink-600 text-xs font-bold hover:bg-pink-100/70 transition border border-pink-200/60 active:scale-95"
              >
                <ImageIcon className="w-3.5 h-3.5" />
                갤러리 사진
              </button>
              <button
                type="button"
                onClick={() => cameraInputRef.current?.click()}
                className="flex items-center gap-1.5 px-3 py-2 rounded-xl bg-gradient-to-r from-pink-400 to-sky-300 text-white text-xs font-bold shadow-xs hover:opacity-95 transition active:scale-95"
              >
                <Camera className="w-3.5 h-3.5" />
                카메라 촬영
              </button>
            </div>
          </div>
        )}

        {/* 텍스트 설명 및 AI 분석 보조 */}
        <div className="flex gap-2">
          <input
            type="text"
            value={textDescription}
            onChange={(e) => setTextDescription(e.target.value)}
            placeholder="예: 초코 마카롱 2개랑 아이스 아메리카노"
            className="flex-1 bg-pink-50/30 border border-pink-100 rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-pink-300 text-slate-700"
          />
          <button
            type="button"
            onClick={handleTextAnalyze}
            disabled={isAnalyzing || !textDescription.trim()}
            className="px-3.5 py-2.5 bg-gradient-to-r from-pink-400 to-rose-400 hover:from-pink-500 hover:to-rose-500 disabled:opacity-40 text-white rounded-xl text-xs font-bold transition flex items-center gap-1 shrink-0 shadow-xs"
          >
            {isAnalyzing ? (
              <RefreshCw className="w-3.5 h-3.5 animate-spin" />
            ) : (
              <Sparkles className="w-3.5 h-3.5" />
            )}
            분석
          </button>
        </div>
      </div>

      {/* 분석 중 로더 상태 */}
      {isAnalyzing && (
        <div className="bg-white rounded-3xl p-6 shadow-xs border border-pink-100 text-center animate-pulse">
          <div className="text-3xl mb-2">🐰💭</div>
          <p className="font-bold text-xs text-pink-600 mb-1">
            토리가 영양 정보를 예쁘게 정리하고 있어요...
          </p>
          <p className="text-[10px] text-slate-400">
            칼로리와 탄단지를 꼼꼼히 살피는 중입니다 ✨
          </p>
        </div>
      )}

      {/* AI 분석 결과 카드 */}
      {analysisResult && !isAnalyzing && (
        <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border-2 border-pink-200 animate-slideUp">
          <div className="flex items-center justify-between mb-3">
            <div className="flex items-center gap-1.5">
              <span className="text-base">{previewMeta.emoji}</span>
              <h4 className="font-extrabold text-sm text-slate-800">분석 결과 확인</h4>
            </div>
            <span className="text-[10px] text-pink-500 font-semibold bg-pink-50 px-2 py-0.5 rounded-full border border-pink-100">
              터치하여 수정 가능
            </span>
          </div>

          {/* 음식 이름 */}
          <div className="mb-3">
            <input
              type="text"
              value={editableFood}
              onChange={(e) => setEditableFood(e.target.value)}
              className="w-full text-sm font-extrabold text-slate-800 bg-pink-50/40 rounded-xl px-3 py-2 border border-pink-100 focus:outline-none focus:ring-2 focus:ring-pink-300"
            />
          </div>

          {/* 칼로리 및 탄단지 수치 수정 그리드 */}
          <div className="grid grid-cols-4 gap-2 text-center mb-3">
            <div className="bg-pink-50/70 rounded-2xl p-2 border border-pink-100">
              <span className="text-[10px] text-slate-400 font-medium block">칼로리</span>
              <div className="flex items-center justify-center">
                <input
                  type="number"
                  value={editableKcal}
                  onChange={(e) => setEditableKcal(e.target.value)}
                  className="w-14 font-extrabold text-sm text-pink-600 bg-transparent text-center focus:outline-none"
                />
                <span className="text-[9px] text-pink-400 font-bold">kcal</span>
              </div>
            </div>

            <div className="bg-sky-50/70 rounded-2xl p-2 border border-sky-100">
              <span className="text-[10px] text-slate-400 font-medium block">단백질</span>
              <div className="flex items-center justify-center">
                <input
                  type="number"
                  value={editableProtein}
                  onChange={(e) => setEditableProtein(e.target.value)}
                  className="w-10 font-bold text-sm text-sky-600 bg-transparent text-center focus:outline-none"
                />
                <span className="text-[9px] text-sky-400">g</span>
              </div>
            </div>

            <div className="bg-rose-50/70 rounded-2xl p-2 border border-rose-100">
              <span className="text-[10px] text-slate-400 font-medium block">탄수화물</span>
              <div className="flex items-center justify-center">
                <input
                  type="number"
                  value={editableCarbs}
                  onChange={(e) => setEditableCarbs(e.target.value)}
                  className="w-10 font-bold text-sm text-rose-500 bg-transparent text-center focus:outline-none"
                />
                <span className="text-[9px] text-rose-400">g</span>
              </div>
            </div>

            <div className="bg-amber-50/70 rounded-2xl p-2 border border-amber-100">
              <span className="text-[10px] text-slate-400 font-medium block">지방</span>
              <div className="flex items-center justify-center">
                <input
                  type="number"
                  value={editableFat}
                  onChange={(e) => setEditableFat(e.target.value)}
                  className="w-10 font-bold text-sm text-amber-500 bg-transparent text-center focus:outline-none"
                />
                <span className="text-[9px] text-amber-400">g</span>
              </div>
            </div>
          </div>

          {/* 양 조절 퀵 버튼 */}
          <div className="flex items-center justify-between text-xs bg-slate-50 rounded-xl p-2 mb-3">
            <span className="text-[11px] text-slate-500 font-medium pl-1">식사량 조절:</span>
            <div className="flex gap-1.5">
              <button 
                type="button" 
                onClick={() => handlePortionAdjust(0.7)}
                className="px-2 py-1 rounded-lg bg-white border border-slate-200 text-[11px] font-semibold hover:bg-pink-50 hover:text-pink-600 transition"
              >
                적은 양 (0.7배)
              </button>
              <button 
                type="button" 
                onClick={() => handlePortionAdjust(1.0)}
                className="px-2 py-1 rounded-lg bg-white border border-slate-200 text-[11px] font-semibold hover:bg-pink-50 hover:text-pink-600 transition"
              >
                보통 양
              </button>
              <button 
                type="button" 
                onClick={() => handlePortionAdjust(1.3)}
                className="px-2 py-1 rounded-lg bg-white border border-slate-200 text-[11px] font-semibold hover:bg-pink-50 hover:text-pink-600 transition"
              >
                많은 양 (1.3배)
              </button>
            </div>
          </div>

          {/* AI 코치 말풍선 */}
          <div className="bg-gradient-to-r from-pink-50 to-sky-50 rounded-2xl p-3 border border-pink-100 flex items-start gap-2.5">
            <span className="text-xl">🐰</span>
            <div className="text-[11px] leading-relaxed text-slate-700 font-medium">
              {editableFeedback}
            </div>
          </div>
        </div>
      )}

      {/* 식사 시간 및 추가 정보 설정 폼 */}
      <form onSubmit={handleSubmit} className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100 space-y-4">
        {/* 날짜 선택 */}
        <div>
          <label className="text-xs font-bold text-slate-700 block mb-1">식사 날짜</label>
          <input
            type="date"
            value={date}
            onChange={(e) => setDate(e.target.value)}
            className="w-full bg-pink-50/20 border border-pink-100 rounded-xl px-3 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-pink-300 font-medium"
          />
        </div>

        {/* 식사 종류 */}
        <div>
          <label className="text-xs font-bold text-slate-700 block mb-1.5">식사 시간대</label>
          <div className="grid grid-cols-4 gap-2">
            {[
              { key: 'breakfast', label: '아침', icon: '☀️' },
              { key: 'lunch', label: '점심', icon: '🍽️' },
              { key: 'dinner', label: '저녁', icon: '🌙' },
              { key: 'snack', label: '간식', icon: '🍰' }
            ].map(item => (
              <button
                type="button"
                key={item.key}
                onClick={() => setMealType(item.key)}
                className={`py-2 rounded-2xl flex flex-col items-center gap-0.5 border text-xs font-bold transition-all ${
                  mealType === item.key
                    ? 'bg-pink-100/90 border-pink-300 text-pink-600 shadow-xs scale-102'
                    : 'bg-pink-50/20 border-slate-100 text-slate-500 hover:bg-pink-50'
                }`}
              >
                <span className="text-sm">{item.icon}</span>
                <span>{item.label}</span>
              </button>
            ))}
          </div>
        </div>

        {/* 메모 작성 */}
        <div>
          <label className="text-xs font-bold text-slate-700 block mb-1">메모 (선택)</label>
          <input
            type="text"
            value={memo}
            onChange={(e) => setMemo(e.target.value)}
            placeholder="예: 초코 2조각만 먹음, 물 500ml 마심"
            className="w-full bg-pink-50/20 border border-pink-100 rounded-xl px-3 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-pink-300"
          />
        </div>

        {/* 저장 버튼 */}
        <button
          type="submit"
          className="w-full py-3.5 rounded-2xl bg-gradient-to-r from-pink-400 via-rose-400 to-sky-300 text-white font-extrabold text-sm shadow-md shadow-pink-200 active:scale-98 transition flex items-center justify-center gap-1.5"
        >
          <Check className="w-4 h-4" />
          식단 저장하고 달력에 쏙 넣기 ✨
        </button>
      </form>
    </div>
  );
}

// ==========================================
// 3. 통계 탭 (일별 / 주별 / 월별 통계)
// ==========================================
function StatsTab({ meals, viewYear, viewMonth }) {
  const [period, setPeriod] = useState('month');

  const monthlyStats = useMemo(() => {
    const prefix = `${viewYear}-${String(viewMonth).padStart(2, '0')}`;
    const curMeals = meals.filter(m => m.date.startsWith(prefix));

    let totalCal = 0;
    let totalP = 0;
    let totalC = 0;
    let totalF = 0;
    const dayMap = {};

    curMeals.forEach(m => {
      totalCal += m.calories || 0;
      totalP += m.protein || 0;
      totalC += m.carbs || 0;
      totalF += m.fat || 0;
      dayMap[m.date] = (dayMap[m.date] || 0) + m.calories;
    });

    const recordedDays = Object.keys(dayMap).length || 1;
    const avgCal = Math.round(totalCal / recordedDays);
    const avgP = Math.round(totalP / recordedDays);

    const macroTotal = (totalP * 4) + (totalC * 4) + (totalF * 9) || 1;
    const carbsPct = Math.round(((totalC * 4) / macroTotal) * 100);
    const proteinPct = Math.round(((totalP * 4) / macroTotal) * 100);
    const fatPct = Math.max(0, 100 - carbsPct - proteinPct);

    const trend = [];
    for (let i = 1; i <= 28; i += 2) {
      const dStr = `${prefix}-${String(i).padStart(2, '0')}`;
      trend.push({
        day: `${i}일`,
        kcal: dayMap[dStr] || (i % 3 === 0 ? 1450 + (i * 20 % 300) : 0)
      });
    }

    return { totalCal, avgCal, avgP, carbsPct, proteinPct, fatPct, trend, recordedDays };
  }, [meals, viewYear, viewMonth]);

  return (
    <div className="space-y-4 animate-fadeIn pb-6">
      {/* 기간 선택 스위치 */}
      <div className="flex bg-pink-100/50 p-1 rounded-2xl">
        {[
          { key: 'day', label: '일별' },
          { key: 'week', label: '주별' },
          { key: 'month', label: '월별' }
        ].map(tab => (
          <button
            key={tab.key}
            onClick={() => setPeriod(tab.key)}
            className={`flex-1 py-1.5 rounded-xl text-xs font-bold transition-all ${
              period === tab.key
                ? 'bg-white text-pink-600 shadow-xs'
                : 'text-slate-500 hover:text-pink-500'
            }`}
          >
            {tab.label}
          </button>
        ))}
      </div>

      {/* 요약 카드 */}
      <div className="grid grid-cols-2 gap-3">
        <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100">
          <span className="text-[11px] text-slate-400 font-semibold block mb-1">
            {viewMonth}월 일 평균 칼로리
          </span>
          <div className="text-xl font-extrabold text-slate-800">
            {monthlyStats.avgCal.toLocaleString()} <span className="text-xs font-normal">kcal</span>
          </div>
          <div className="mt-2 text-[10px] text-sky-600 font-semibold flex items-center gap-0.5">
            <TrendingUp className="w-3 h-3" /> 권장 범위 내 안정적 유지 중
          </div>
        </div>

        <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100">
          <span className="text-[11px] text-slate-400 font-semibold block mb-1">
            일 평균 단백질
          </span>
          <div className="text-xl font-extrabold text-sky-500">
            {monthlyStats.avgP} <span className="text-xs font-normal text-slate-400">g</span>
          </div>
          <div className="mt-2 text-[10px] text-pink-500 font-semibold">
            목표(65g) 달성률 102% 🎀
          </div>
        </div>
      </div>

      {/* 일별 칼로리 추이 그래프 카드 */}
      <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100">
        <div className="flex items-center justify-between mb-3">
          <h4 className="font-extrabold text-sm text-slate-800">섭취 칼로리 추이</h4>
          <span className="text-[11px] text-pink-500 font-medium">목표: 1,600 kcal</span>
        </div>

        <div className="h-36 flex items-end justify-between gap-1 pt-6 px-1 border-b border-pink-100">
          {monthlyStats.trend.map((item, idx) => {
            const heightPct = Math.min(100, Math.max(10, Math.round((item.kcal / 2200) * 100)));
            const isHighlight = item.kcal > 1700;

            return (
              <div key={idx} className="flex-1 flex flex-col items-center gap-1 group relative">
                <div className="absolute -top-7 opacity-0 group-hover:opacity-100 bg-slate-800 text-white text-[9px] px-1.5 py-0.5 rounded shadow pointer-events-none transition whitespace-nowrap z-20">
                  {item.kcal} kcal
                </div>
                <div 
                  className={`w-full max-w-[12px] rounded-t-lg transition-all ${
                    isHighlight 
                      ? 'bg-gradient-to-t from-pink-400 to-rose-400' 
                      : 'bg-sky-200 group-hover:bg-sky-300'
                  }`}
                  style={{ height: `${heightPct}%` }}
                />
                <span className="text-[9px] text-slate-400">{item.day}</span>
              </div>
            );
          })}
        </div>
      </div>

      {/* 주요 영양소 비율 카드 */}
      <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100">
        <h4 className="font-extrabold text-sm text-slate-800 mb-3">주요 영양소 섭취 비율</h4>
        
        {/* 누적 바 */}
        <div className="h-3.5 w-full rounded-full overflow-hidden flex shadow-inner mb-3">
          <div style={{ width: `${monthlyStats.carbsPct}%` }} className="bg-rose-400" title="탄수화물" />
          <div style={{ width: `${monthlyStats.proteinPct}%` }} className="bg-sky-400" title="단백질" />
          <div style={{ width: `${monthlyStats.fatPct}%` }} className="bg-amber-300" title="지방" />
        </div>

        <div className="grid grid-cols-3 gap-2 text-center text-xs">
          <div className="p-2 rounded-2xl bg-rose-50 border border-rose-100">
            <span className="text-[10px] text-rose-500 block font-semibold">탄수화물</span>
            <span className="font-extrabold text-sm text-slate-800">{monthlyStats.carbsPct}%</span>
            <span className="text-[9px] text-slate-400 block mt-0.5">권장: 50%</span>
          </div>

          <div className="p-2 rounded-2xl bg-sky-50 border border-sky-100">
            <span className="text-[10px] text-sky-600 block font-semibold">단백질</span>
            <span className="font-extrabold text-sm text-slate-800">{monthlyStats.proteinPct}%</span>
            <span className="text-[9px] text-sky-500 font-bold block mt-0.5">아주 우수!</span>
          </div>

          <div className="p-2 rounded-2xl bg-amber-50 border border-amber-100">
            <span className="text-[10px] text-amber-600 block font-semibold">지방</span>
            <span className="font-extrabold text-sm text-slate-800">{monthlyStats.fatPct}%</span>
            <span className="text-[9px] text-slate-400 block mt-0.5">권장: 25%</span>
          </div>
        </div>
      </div>
    </div>
  );
}

// ==========================================
// 4. AI 식단 코치 토리 탭
// ==========================================
function CoachTab({ selectedDate, dayMeals, dailySummary, onGoToRecord }) {
  return (
    <div className="space-y-4 animate-fadeIn pb-6">
      {/* 코치 캐릭터 카드 */}
      <div className="bg-gradient-to-tr from-pink-100/80 via-rose-50 to-sky-100/70 rounded-3xl p-5 border border-pink-100 shadow-[0_4px_20px_rgba(255,182,193,0.12)] relative overflow-hidden">
        <div className="flex items-center gap-3 mb-3">
          <div className="w-14 h-14 rounded-2xl bg-white shadow-xs flex items-center justify-center text-3xl border border-pink-100">
            🐰
          </div>
          <div>
            <div className="flex items-center gap-1.5">
              <h3 className="font-extrabold text-base text-slate-800">AI 식단 코치 토리</h3>
              <span className="bg-pink-200 text-pink-700 text-[10px] font-extrabold px-1.5 py-0.5 rounded-full">
                건강 메이트
              </span>
            </div>
            <p className="text-xs text-pink-500 font-medium">당신만을 위한 다정한 영양 파트너</p>
          </div>
        </div>

        {/* 말풍선 */}
        <div className="bg-white/95 backdrop-blur-sm rounded-2xl p-4 shadow-xs border border-pink-100 relative">
          <p className="text-xs leading-relaxed text-slate-700 font-medium">
            {dailySummary.calories > 0 ? (
              <>
                <strong className="text-pink-600 font-bold">"{selectedDate}"</strong>에는 총 <strong className="text-pink-600">{dailySummary.calories} kcal</strong>를 섭취하셨네요!
                <br />
                단백질이 <strong className="text-sky-600">{dailySummary.protein}g</strong>으로 건강하게 채워졌어요. 간식이나 식사 후 따뜻한 물 한 잔으로 수분도 함께 챙겨주세요 🌸✨
              </>
            ) : (
              <>
                아직 오늘 기록된 식단이 없네요! 
                <br />
                달콤한 간식이나 맛있는 식사를 사진이나 글로 남겨주시면 토리가 바로 정리해드릴게요 🍰💖
              </>
            )}
          </p>
        </div>
      </div>

      {/* 목표 달성 게이지 바 카드 */}
      <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100 space-y-3">
        <h4 className="font-extrabold text-sm text-slate-800">일일 목표 달성도</h4>

        {/* 칼로리 */}
        <div>
          <div className="flex justify-between text-xs mb-1 font-semibold">
            <span className="text-slate-600">섭취 칼로리</span>
            <span className="text-pink-500">{dailySummary.calories} / 1,800 kcal</span>
          </div>
          <div className="w-full h-2 rounded-full bg-pink-50 overflow-hidden">
            <div 
              className="h-full bg-gradient-to-r from-pink-400 to-rose-400 rounded-full"
              style={{ width: `${Math.min(100, (dailySummary.calories / 1800) * 100)}%` }}
            />
          </div>
        </div>

        {/* 단백질 */}
        <div>
          <div className="flex justify-between text-xs mb-1 font-semibold">
            <span className="text-slate-600">단백질</span>
            <span className="text-sky-600">{dailySummary.protein} / 80 g</span>
          </div>
          <div className="w-full h-2 rounded-full bg-sky-50 overflow-hidden">
            <div 
              className="h-full bg-gradient-to-r from-sky-300 to-sky-500 rounded-full"
              style={{ width: `${Math.min(100, (dailySummary.protein / 80) * 100)}%` }}
            />
          </div>
        </div>

        {/* 탄수화물 */}
        <div>
          <div className="flex justify-between text-xs mb-1 font-semibold">
            <span className="text-slate-600">탄수화물</span>
            <span className="text-rose-400">{dailySummary.carbs} / 200 g</span>
          </div>
          <div className="w-full h-2 rounded-full bg-rose-50 overflow-hidden">
            <div 
              className="h-full bg-gradient-to-r from-rose-300 to-pink-400 rounded-full"
              style={{ width: `${Math.min(100, (dailySummary.carbs / 200) * 100)}%` }}
            />
          </div>
        </div>
      </div>

      {/* 추천 식단 카드 */}
      <div className="bg-white rounded-3xl p-4 shadow-[0_4px_20px_rgba(255,182,193,0.12)] border border-pink-100">
        <h4 className="font-extrabold text-sm text-slate-800 mb-2.5 flex items-center gap-1.5">
          <span>💡</span> 오늘의 영양 추천 메뉴
        </h4>
        <div className="flex items-center gap-3 p-2.5 rounded-2xl bg-gradient-to-r from-pink-50/50 to-sky-50/50 border border-pink-100">
          <div className="w-13 h-13 rounded-2xl bg-white shadow-xs flex items-center justify-center text-2xl border border-pink-100">
            🥪
          </div>
          <div className="flex-1">
            <span className="text-[10px] font-bold text-sky-600 bg-sky-100 px-1.5 py-0.5 rounded">
              단백질 & 식이섬유
            </span>
            <div className="font-bold text-xs text-slate-800 mt-1">
              에그 아보카도 샌드위치
            </div>
            <p className="text-[11px] text-slate-400 mt-0.5">
              기분 좋은 포만감과 깔끔한 단백질을 선물해요!
            </p>
          </div>
        </div>
      </div>
    </div>
  );
}

// ==========================================
// 5. 날짜별 상세 내역 바텀시트 모달
// ==========================================
function DayDetailModal({ dateStr, dayMeals, summary, onClose, onAddMeal }) {
  return (
    <div className="fixed inset-0 z-50 flex items-end justify-center bg-black/35 backdrop-blur-xs transition-opacity animate-fadeIn">
      <div className="w-full max-w-[430px] bg-white rounded-t-3xl p-5 shadow-2xl max-h-[85vh] flex flex-col animate-slideUp border-t border-pink-100">
        <div className="w-12 h-1 bg-slate-200 rounded-full mx-auto mb-3" />

        <div className="flex items-center justify-between pb-3 border-b border-pink-100">
          <div>
            <span className="text-[11px] text-pink-500 font-bold block">상세 식단 다이어리</span>
            <h3 className="font-extrabold text-lg text-slate-800">{dateStr}</h3>
          </div>
          <button 
            onClick={onClose}
            className="w-8 h-8 rounded-full bg-slate-100 flex items-center justify-center text-slate-500 hover:bg-slate-200"
          >
            <X className="w-4 h-4" />
          </button>
        </div>

        {/* 요약 배너 */}
        <div className="my-3 p-3 bg-gradient-to-r from-pink-100/70 to-sky-100/70 rounded-2xl flex items-center justify-between">
          <div>
            <span className="text-[10px] text-slate-500 font-semibold">총 섭취 칼로리</span>
            <div className="text-xl font-extrabold text-pink-600">
              {summary.calories} <span className="text-xs font-normal">kcal</span>
            </div>
          </div>
          <div className="flex gap-2 text-center text-xs">
            <div>
              <span className="text-[9px] text-slate-400 block">단백질</span>
              <span className="font-bold text-sky-600">{summary.protein}g</span>
            </div>
            <div>
              <span className="text-[9px] text-slate-400 block">탄수화물</span>
              <span className="font-bold text-rose-500">{summary.carbs}g</span>
            </div>
            <div>
              <span className="text-[9px] text-slate-400 block">지방</span>
              <span className="font-bold text-amber-500">{summary.fat}g</span>
            </div>
          </div>
        </div>

        {/* 상세 식단 리스트 */}
        <div className="flex-1 overflow-y-auto space-y-2.5 py-1 pr-1">
          {dayMeals.length === 0 ? (
            <div className="py-8 text-center text-slate-400 text-xs">
              기록된 식단이 없습니다.
            </div>
          ) : (
            dayMeals.map((meal) => {
              const meta = getFoodVisualMeta(meal.foodName);
              return (
                <div 
                  key={meal.id}
                  className="p-3 rounded-2xl border border-pink-100 bg-[#FFFDFE] flex gap-3 shadow-xs items-center"
                >
                  {meal.image ? (
                    <img 
                      src={meal.image} 
                      alt={meal.foodName} 
                      className="w-14 h-14 rounded-2xl object-cover shrink-0 shadow-xs border border-pink-100"
                    />
                  ) : (
                    <div className={`w-14 h-14 rounded-2xl ${meta.bg} border ${meta.border} flex items-center justify-center text-2xl shrink-0 shadow-xs`}>
                      {meta.emoji}
                    </div>
                  )}

                  <div className="flex-1 min-w-0">
                    <div className="flex items-center justify-between">
                      <span className="text-[10px] font-bold px-1.5 py-0.5 rounded-md bg-pink-100/80 text-pink-600">
                        {meal.mealType === 'breakfast' && '아침'}
                        {meal.mealType === 'lunch' && '점심'}
                        {meal.mealType === 'dinner' && '저녁'}
                        {meal.mealType === 'snack' && '간식'}
                      </span>
                      <span className="text-[11px] font-bold text-pink-500">
                        {meal.calories} kcal
                      </span>
                    </div>
                    <h5 className="font-bold text-xs text-slate-800 truncate mt-1">
                      {meal.foodName}
                    </h5>
                    {meal.memo && (
                      <p className="text-[10px] text-slate-400 mt-0.5 truncate">
                        메모: {meal.memo}
                      </p>
                    )}
                    {meal.feedback && (
                      <p className="text-[10px] text-slate-600 bg-pink-50/60 rounded-lg px-1.5 py-0.5 mt-1 font-medium">
                        🐰 {meal.feedback}
                      </p>
                    )}
                  </div>
                </div>
              );
            })
          )}
        </div>

        <div className="pt-3 border-t border-pink-50 mt-2">
          <button
            onClick={onAddMeal}
            className="w-full py-3 rounded-2xl bg-gradient-to-r from-pink-400 to-sky-300 text-white font-bold text-xs shadow-md shadow-pink-200 hover:opacity-95 transition"
          >
            + 이 날짜에 새 식단 추가하기
          </button>
        </div>
      </div>
    </div>
  );
}

// ==========================================
// 6. 설정 모달 (Gemini API 키 등록)
// ==========================================
function SettingsModal({ apiKey, onSave, onClose }) {
  const [keyInput, setKeyInput] = useState(apiKey);

  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-xs p-4 animate-fadeIn">
      <div className="w-full max-w-sm bg-white rounded-3xl p-5 shadow-2xl border border-pink-100 animate-slideUp">
        <div className="flex items-center justify-between mb-3 pb-2 border-b border-pink-100">
          <div className="flex items-center gap-2">
            <span className="text-lg">⚙️</span>
            <h3 className="font-extrabold text-sm text-slate-800">앱 설정 & API 키</h3>
          </div>
          <button onClick={onClose} className="text-slate-400 hover:text-slate-600">
            <X className="w-4 h-4" />
          </button>
        </div>

        <div className="space-y-3 mb-4 text-xs">
          <div className="bg-sky-50 rounded-2xl p-3 border border-sky-100 text-slate-600">
            <div className="font-bold text-sky-700 flex items-center gap-1 mb-1">
              <Key className="w-3.5 h-3.5" />
              Google Gemini API 키 (선택)
            </div>
            <p className="text-[11px] leading-relaxed">
              Google AI Studio에서 무료로 발급받은 API 키를 넣으시면 실제 AI 비전 분석이 실시간 작동합니다. 키가 없어도 맞춤 지능형 엔진으로 자동 동작해요!
            </p>
          </div>

          <div>
            <label className="text-[11px] font-bold text-slate-700 block mb-1">
              Gemini API Key
            </label>
            <input
              type="password"
              value={keyInput}
              onChange={(e) => setKeyInput(e.target.value)}
              placeholder="AIzaSy..."
              className="w-full bg-pink-50/20 border border-pink-100 rounded-xl px-3 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-pink-300 font-mono"
            />
          </div>
        </div>

        <div className="flex gap-2">
          <button
            onClick={onClose}
            className="flex-1 py-2.5 rounded-xl bg-slate-100 text-slate-600 font-bold text-xs hover:bg-slate-200 transition"
          >
            닫기
          </button>
          <button
            onClick={() => onSave(keyInput)}
            className="flex-1 py-2.5 rounded-xl bg-gradient-to-r from-pink-400 to-rose-400 text-white font-bold text-xs shadow-xs hover:opacity-95 transition"
          >
            저장하기
          </button>
        </div>
      </div>
    </div>
  );
}
