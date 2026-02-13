<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>영어회화 스피드퀴즈 Level 2</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            -webkit-user-select: none; -moz-user-select: none; -ms-user-select: none; user-select: none;
            -webkit-touch-callout: none; -webkit-tap-highlight-color: transparent;
            word-break: keep-all; background-color: #f8fafc; font-family: system-ui, -apple-system, sans-serif;
        }
        .card { perspective: 1000px; }
        .card-inner {
            position: relative; width: 100%; height: 320px;
            transition: transform 0.6s; transform-style: preserve-3d; cursor: pointer;
        }
        .card.flipped .card-inner { transform: rotateY(180deg); }
        .card-front, .card-back {
            position: absolute; width: 100%; height: 100%;
            backface-visibility: hidden; display: flex; flex-direction: column;
            align-items: center; justify-content: center; border-radius: 1.5rem;
            padding: 2rem; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .card-back { transform: rotateY(180deg); }
        .active-tab { background-color: #4f46e5 !important; color: white !important; }
        .star-checked { color: #fbbf24 !important; fill: #fbbf24 !important; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body oncontextmenu="return false">

    <div class="max-w-md mx-auto px-4 py-6">
        <header class="text-center mb-6">
            <h1 class="text-2xl font-black text-indigo-600 tracking-tight italic">영어회화 스피드퀴즈</h1>
            <p class="text-[10px] text-slate-400 mt-1 font-bold">DAY 051 - 075</p>
        </header>

        <div class="flex space-x-2 mb-6 overflow-x-auto pb-2 no-scrollbar font-bold text-xs">
            <button onclick="selectWeek('w11')" id="tab-w11" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100 active-tab">Day 51-55</button>
            <button onclick="selectWeek('w12')" id="tab-w12" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 56-60</button>
            <button onclick="selectWeek('w13')" id="tab-w13" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 61-65</button>
            <button onclick="selectWeek('w14')" id="tab-w14" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 66-70</button>
            <button onclick="selectWeek('w15')" id="tab-w15" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">Day 71-75</button>
            <button onclick="selectWeek('total')" id="tab-total" class="week-tab flex-shrink-0 px-4 py-2 bg-white rounded-full shadow-sm border border-slate-100">전체 범위</button>
        </div>

        <div id="setup-screen" class="bg-white rounded-3xl p-8 shadow-xl border border-slate-100 text-center">
            <div id="selected-week-title" class="text-indigo-500 font-black text-xl mb-6 italic tracking-widest uppercase">DAY 51-55</div>
            <div class="space-y-4">
                <button onclick="startQuiz('mild')" class="w-full py-5 bg-emerald-500 text-white rounded-2xl font-black shadow-lg shadow-emerald-100 active:scale-95 transition">순한맛 시작</button>
                <button onclick="startQuiz('spicy')" class="w-full py-5 bg-orange-500 text-white rounded-2xl font-black shadow-lg shadow-orange-100 active:scale-95 transition">매운맛 시작</button>
            </div>
            <div class="mt-8 p-4 bg-slate-50 rounded-xl text-[11px] text-slate-400 text-left leading-relaxed">
                <p class="mb-2 text-indigo-600 font-bold underline">학습 모드 설명</p>
                <p>• <strong>순한맛</strong>: 대표예제 + Model examples (교재1)</p>
                <p>• <strong>매운맛</strong>: 위 구성 + Small talk, Cases in point, Further studies 전체 포함</p>
                <p class="mt-2 text-rose-400 font-bold italic">※ 선택 구간에서 랜덤 10문제가 출제됩니다.</p>
            </div>
        </div>

        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6 px-2">
                <span id="progress-text" class="px-3 py-1 bg-indigo-100 text-indigo-600 rounded-full text-xs font-bold">1 / 10</span>
                <button onclick="location.reload()" class="text-slate-400 font-bold text-xs uppercase">Exit ✕</button>
            </div>

            <div id="card-container" class="card" onclick="flipCard()">
                <div class="card-inner">
                    <div class="card-front bg-white border border-slate-100">
                        <span class="text-indigo-400 font-black text-[10px] tracking-widest mb-6 uppercase">Korean</span>
                        <p id="q-korean" class="text-lg font-bold leading-snug px-4 text-center"></p>
                        <p class="mt-10 text-slate-300 text-xs font-bold animate-pulse">터치하여 정답 확인</p>
                    </div>
                    <div class="card-back bg-indigo-600 text-white">
                        <button id="star-btn" onclick="toggleStar(event)" class="absolute top-6 right-6 text-white/40 transition-all">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-9 w-9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14l-5-4.87 6.91-1.01L12 2z"/>
                            </svg>
                        </button>
                        <span class="text-indigo-200 font-black text-[10px] tracking-widest mb-6 uppercase">English</span>
                        <p id="q-english" class="text-lg font-black leading-snug px-4 mb-8 text-center"></p>
                        <button onclick="event.stopPropagation(); playTTS();" class="w-16 h-16 bg-white/20 rounded-full flex items-center justify-center active:scale-90 transition shadow-inner">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                            </svg>
                        </button>
                        <div id="q-info" class="absolute bottom-6 text-[9px] text-indigo-300 font-bold uppercase tracking-widest italic"></div>
                    </div>
                </div>
            </div>
            <button id="next-btn" onclick="nextQuestion()" class="w-full mt-8 py-5 bg-indigo-600 text-white rounded-2xl font-black shadow-xl hidden active:scale-95 transition">다음 문제 NEXT</button>
        </div>

        <div id="result-screen" class="hidden bg-white rounded-3xl p-6 shadow-xl border border-slate-100">
            <h2 class="text-center font-black text-xl text-slate-800 mb-8 tracking-tighter">학습 완료 리포트</h2>
            <div id="starred-list" class="space-y-4 max-h-[400px] overflow-y-auto pr-2 custom-scroll"></div>
            <button onclick="location.reload()" class="w-full mt-10 py-5 bg-slate-900 text-white rounded-2xl font-bold active:scale-95 transition">메인으로</button>
        </div>
    </div>

    <script>
        const allData = [
            // Day 50 (Baseline)
            {d:50, s:"교재1", k:"“그렇지만 여기는 원래 그래요.”라는 변명을 자주 듣게 될 겁니다.", e:"You’ll often hear excuses like, “but that’s just how things work here.”"},
            {d:50, s:"교재1", k:"자본주의 사회에서는 원래 그런 거죠.", e:"That’s just how things work in capitalism."},
            {d:50, s:"교재1", k:"처음에는 CEO의 딸이 저보다 먼저 승진해서 정말 화가 났습니다. 근데 가족 운영 기업에서는 원래 그렇다는 걸 알게 되었지요.", e:"At first, I was mad about the CEO’s daughter being promoted faster than me. But then I realized that’s just how things work at a family owned company."},
            {d:50, s:"교재1", k:"워라밸이 보장되면 참 좋겠지만, 대부분 한국 기업은 그렇지가 않아요.", e:"I wish I had a nice balance between work and home life, but that’s not how it works in most Korean companies."},
            {d:50, s:"교재1", k:"네가 요금을 내면, 팁은 내가 낼게.", e:"If you are paying the fare, then I’ve got the tip!"},
            {d:50, s:"교재4", k:"너 월세 밀리면 안 되잖아.", e:"I don’t want you to get behind on your rent."},
            {d:50, s:"교재4", k:"우리 등록금이 오른다는 소식 들었어?", e:"Did you hear about our tuition going up?"},
            {d:50, s:"교재4", k:"주문하신 음식 10분 안에 준비됩니다.", e:"Your order will be ready in 10 minutes."},
            {d:50, s:"대표", k:"여기서는 원래 그래요.", e:"That’s just how things work here."},

            // Day 51
            {d:51, s:"교재1", k:"뭐든 정말 더 잘하고 싶으면 올인을 해야 해.", e:"If you really want to get better at anything, you should fully commit to it."},
            {d:51, s:"교재1", k:"조금만 견디세요. 중급 레벨에 도달하면 실력이 느는 데 훨씬 더 오래 걸리거든요.", e:"Hang in there. Once you reach the intermediate level, it takes way longer to get better."},
            {d:51, s:"교재1", k:"무언가를 더 잘하려면 연습만이 답이다.", e:"Practice is the only way to get better at something."},
            {d:51, s:"교재1", k:"저는 뭐든 잘 늘지 않는 것 같아요.", e:"I can’t seem to get better at anything."},
            {d:51, s:"교재1", k:"대학 때부터 직접 요리해 오고 있어요. 근데 여전히 더 잘하고 싶답니다. 파스타 하나 만드는 데 한 시간이나 걸리거든요.", e:"I’ve been cooking for myself since university, but I still want to get better. It takes me like an hour to make pasta."},
            {d:51, s:"교재4", k:"죄송해요, 저희 남편이 귀찮게 하죠? 남편이 눈치가 많이 없어요.", e:"I’m sorry, is my husband bothering you? He’s terrible at picking up on hints."},
            {d:51, s:"교재4", k:"제 남자 친구가 옷을 정말 못 골라요.", e:"My boyfriend is terrible at picking out clothes."},
            {d:51, s:"교재4", k:"막걸리 만드는 수업을 들었다면서요. 그럼, 막걸리 잘 만드시겠네요?", e:"I heard that you took a class on how to make makgeolli. So, are you any good (at it)?"},
            {d:51, s:"대표", k:"골프를 더 잘 치고 싶어요.", e:"I want to get better at golf."},

            // Day 52
            {d:52, s:"교재1", k:"회사가 너무 바빠져서, 요즘 헬스장 갈 시간도 없었어요.", e:"I’ve gotten super busy at work, so I haven’t been able to make time to go to the gym."},
            {d:52, s:"교재1", k:"관계에 있어서는 서로를 위해 시간을 내야 한다.", e:"In a relationship, you have to make time for each other."},
            {d:52, s:"교재1", k:"편할 때 들르세요. 언제든 시간 내겠습니다.", e:"Please feel free to come visit me. I can always make time for you."},
            {d:52, s:"교재1", k:"책을 쓰다 보니 너무 바쁘네요. 제대로 밥 챙겨 먹을 시간이 없을 때도 있습니다.", e:"Writing these books has been keeping me super busy. Sometimes, I can’t even find time to have a decent meal."},
            {d:52, s:"교재1", k:"보통 2주에 한 번은 장 보러 가는데, 최근엔 두 달 동안 시간을 못 냈어요.", e:"I normally go grocery shopping every other week, but I haven’t been able to find the time for two months."},
            {d:52, s:"교재4", k:"서울 사람들은 너무 바쁜 듯해.", e:"People in Seoul seem super busy. = People in Seoul are always rushing around."},
            {d:52, s:"교재4", k:"호주 사람들은 여유가 있고 느긋해.", e:"Australians seem relaxed and laid-back."},
            {d:52, s:"교재4", k:"내가 왜 바쁜지를 모르겠어.", e:"I don’t really know what’s been keeping me so busy."},
            {d:52, s:"대표", k:"바빠서 운동 할 짬이 안 나네요.", e:"I can’t seem to find (the) time to exercise."},

            // Day 53
            {d:53, s:"교재1", k:"그 사람은 농구 선수치고는 키가 좀 작다.", e:"He’s kind of short for a basketball player."},
            {d:53, s:"교재1", k:"해가 쨍쨍한 것치고는 상당히 춥다. 그렇지?", e:"It’s rather cold for such a sunny day, isn’t it?"},
            {d:53, s:"교재1", k:"그 여자분은 아시아인치고는 키가 상당히 크다.", e:"She is rather tall for an Asian girl."},
            {d:53, s:"교재1", k:"집 크기를 생각하면 내 월세가 정말 싼 편이다.", e:"The rent is really low for how big my place is."},
            {d:53, s:"교재1", k:"학교 가는 날인 점을 생각하면 놀이동산에 놀랍게 사람이 많더라고요.", e:"The amusement park was surprisingly crowded for a school day."},
            {d:53, s:"교재4", k:"제가 영업 쪽 일을 하는지라 재택은 불가능합니다.", e:"Working from home is not an option for me, since I’m in sales."},
            {d:53, s:"교재4", k:"회사를 그만둔다는 건 생각할 수 없어요. 저 혼자 돈을 벌고 있고 먹여 살려야 할 아이들이 세 명이나 있으니까요.”", e:"Quitting isn’t an option for me. I’m a sole breadwinner and I have three kids to feed."},
            {d:53, s:"교재4", k:"부산에 버스 타고 갔지. 버스가 제일 저렴했으니까.", e:"I took a bus to Busan because it was the cheapest option."},
            {d:53, s:"대표", k:"다이소는 가격을 생각하면 꽤 좋은 물건들을 판다.", e:"Daiso has pretty good products for its low prices"},

            // Day 54
            {d:54, s:"교재1", k:"그 이야기는 저녁 먹으면서 하면 어떨까요?", e:"Maybe we could talk about that over dinner."},
            {d:54, s:"교재1", k:"미안한데 지금 가 봐야 해. 나중에 다시 이야기하자. 커피 한잔하면서.", e:"I’m sorry. I have to go now, but let’s catch up later. Maybe over some coffee."},
            {d:54, s:"교재1", k:"제 여자 친구가 친구들이랑 놀러 나갔어요. 술 마시면서 가십을 나누고 있는 게 틀림없어요.", e:"My girlfriend’s out with friends now. I’m sure they’re sharing gossip over drinks."},
            {d:54, s:"교재1", k:"한국인은 삼겹살과 소주를 하면서 친해지는 것을 좋아하는 것 같아.", e:"Koreans seem to love bonding over pork belly and soju."},
            {d:54, s:"교재1", k:"나 이성 관계 때문에 진지한 조언이 필요해. 커피 말고 술 한잔하면서 이야기하면 어떨까?", e:"I need some serious relationship advice. Maybe we could meet over drinks instead of coffee."},
            {d:54, s:"교재4", k:"아마 저 친구가 낼 거야.", e:"Maybe he’ll pick up the tab."},
            {d:54, s:"교재4", k:"내가 낼게.", e:"It’s on me."},
            {d:54, s:"교재4", k:"내가 낼게. 내가 낸다니까.", e:"Please, I insist."},
            {d:54, s:"교재4", k:"2차 가자. 내가 쏠게.", e:"Another round, on me!"},
            {d:54, s:"교재4", k:"지난번엔 네가 샀으니 이번엔 내가 살게.", e:"I owe you for last time. / You paid last time, so it’s my turn."},
            {d:54, s:"교재4", k:"계산은 어떻게 하시겠어요?", e:"How would you like to pay?"},
            {d:54, s:"교재4", k:"따로 계산할게요.", e:"We’d like to pay separately. / We’d like to split the bill."},
            {d:54, s:"대표", k:"점심 먹으면서 그동안 못했던 이야기하자.", e:"Let’s catch up over lunch."},

            // Day 55
            {d:55, s:"교재1", k:"Terry 선물 사는 거 깜박했다! 파티 가는 길에 빵집 있어? 잠깐 들러서 케이크 사갈까 싶은데.", e:"I forgot to get Terry a gift! Is there a bakery on our way to the party? Maybe we can swing by and grab a cake."},
            {d:55, s:"교재1", k:"이따 오후에 네 사무실에 잠깐 들러도 될까?", e:"Do you mind if I swing by your office later this afternoon?"},
            {d:55, s:"교재1", k:"집에 오는 길에 그 술집 들르면 안 돼! 콘서트장에 늦지 않으려면 서둘러야 해.", e:"Please don’t swing by the bar on your way home! We need to rush a little to make it to the concert on time."},
            {d:55, s:"교재1", k:"집에 가기 전에 잠깐만 들러서 술 한잔만 더 하고 가자. 내가 살게!", e:"Let’s swing by there and have just one more drink before you head home. My treat!"},
            {d:55, s:"교재4", k:"저 회사에서 아주 짧게 일했어요.", e:"I worked briefly at that company."},
            {d:55, s:"교재4", k:"저희 잠깐 만났어요.", e:"We briefly dated."},
            {d:55, s:"교재4", k:"지금 바빠서요. 짧게 설명해 주시겠어요?", e:"I don’t have much time. Can you briefly explain?"},
            {d:55, s:"교재4", k:"그는 10킬로 구간 기록을 세우기 위해 노력하고 있었지만, 약 6킬로 지점에서 호흡을 가다듬기 위해 잠깐 멈춰 섰다.", e:"He was trying to set a new 10 km record, but at around 6 kilometers, he stopped briefly to catch his breath."},
            {d:55, s:"교재4", k:"지난주 자료 잠깐 리뷰해 보자.", e:"Let’s briefly review last week’s material."},
            {d:55, s:"대표", k:"출근 전에 잠깐 우리 집 들러서 커피 한잔하고 가.", e:"Swing by my place for coffee before work."},

            // Day 56
            {d:56, s:"교재1", k:"미안한데 안 돼. 나 이미 약속이 있거든.", e:"I’m sorry, I can’t. I already have plans."},
            {d:56, s:"교재1", k:"이번 주에 등산이나 하면 어떨까 하는데. 이번 일요일에 약속 있어?", e:"I thought maybe we could go for a hike or something this week. Do you have any plans this Sunday?"},
            {d:56, s:"교재1", k:"금요일에 나랑 전시회 갈래? 선약이 없으면 말이야.", e:"Do you want to come with me to the exhibition on Friday? I mean, if you don’t already have plans."},
            {d:56, s:"교재1", k:"실은 대치동에서 부동산 중개업자 분과 약속이 있어요. 거기로 이사를 할까 해서요.", e:"I actually have this appointment with a real estate agent in Daechi-dong. I’m thinking of moving there."},
            {d:56, s:"교재1", k:"제가 매주 화요일에는 퇴근 후에 PT 스케줄이 있습니다.", e:"I have a personal training appointment every Tuesday after work."},
            {d:56, s:"교재4", k:"공덕에 있는 문스타파에 꼭 한번 가보세요.", e:"I really recommend you check out Moon’s Tapa in Gongdeok."},
            {d:56, s:"교재4", k:"난 거기 정말 마음에 들어. 당신도 내일 언제 가서 한번 보고 와.", e:"I really like the place. You should probably check it out sometime tomorrow."},
            {d:56, s:"교재4", k:"현대차 매장에 가서 신형 그랜저 보고 오려고.", e:"I am going to the Hyundai dealership to check out the new Grandeur."},
            {d:56, s:"대표", k:"죄송한데 선약이 있습니다.", e:"I’m afraid I already have plans."},

            // Day 57
            {d:57, s:"교재1", k:"웬만하면 집에서 밥을 더 자주 해 먹어야 할 것 같아. 요즘 좀 빠듯해.", e:"I think we should try cooking at home more. Money is a bit tight right now."},
            {d:57, s:"교재1", k:"너 요새 금전적으로 어려우면 계산은 내가 해도 돼.", e:"I can take care of the check if you’re tight on cash."},
            {d:57, s:"교재1", k:"혹시 할부로 구매할 방법이 있을까요? 이 컴퓨터 너무 마음에 드는데 이번 달에 자금 사정이 좀 안 좋아서요.", e:"Is there any way I can pay for this in installments? I love the computer, but money’s tight this month."},
            {d:57, s:"교재1", k:"이 웹사이트에는 적은 돈으로 요리할 수 있는 레시피가 굉장히 많아요.", e:"This website has a lot of recipes for cooking on a tight budget."},
            {d:57, s:"교재1", k:"제 조카가 캘리포니아로 유학을 가는데, 장학금을 못 받으면 굉장히 팍팍할 거예요.", e:"My nephew is going to California for university, but without any scholarships, his budget is going to be tight."},
            {d:57, s:"교재4", k:"현재 저희 매장에 감자튀김이 모자라서, 햄버거 주문 시 양파링이나 치즈스틱이 함께 나오며 추가 요금은 없습니다. 불편을 드려 죄송합니다.", e:"Since we are currently short on french fries, hamburgers will now come with either onion rings or cheese sticks at no extra charge. We’re sorry for the inconvenience."},
            {d:57, s:"교재4", k:"죄송한데 이번 달 월세 낼 돈이 오십만 원 부족하네요. 지금 이십만 원 보내 드리고 나머지는 일주일 정도 후에 보내 드릴 수 있습니다.", e:"I’m afraid we’re 500,000 won short on rent this month. I can send you 200,000 won right now and the rest in a week or so."},
            {d:57, s:"대표", k:"요새 돈이 좀 궁해.", e:"Money is a bit tight right now."},

            // Day 58
            {d:58, s:"교재1", k:"근무 시간은 긴데, 그래도 급여는 평균 이상이야.", e:"The hours are long, but at least the pay is above average."},
            {d:58, s:"교재1", k:"근무 시간은 나쁘지 않은데 출퇴근이 너무 오래 걸려.", e:"The hours aren’t bad, but my commute takes forever."},
            {d:58, s:"교재1", k:"근무 시간은 너무 좋아. 매일 저녁 애들이랑 시간을 보낼 수도 있고. 근데 급여가 조금 낮아.", e:"The hours are great. I get to spend every evening with my boys. Then again, the pay is a bit low."},
            {d:58, s:"교재1", k:"근무 시간이 어떻게 되는지와 초과 근무가 의무인지 확인하고 싶습니다.", e:"I just wanted to know what the hours are like, and if any overtime is mandatory."},
            {d:58, s:"교재1", k:"근무 시간이 너무 적어서 부업으로 과외를 할까 생각 중입니다. 돈이 정말 필요하거든요.", e:"I’m not getting enough hours, so I’m thinking about doing some tutoring on the side, too. I could really use the money."},
            {d:58, s:"교재4", k:"일자리를 구하고 있어요.", e:"I am looking for a job."},
            {d:58, s:"교재4", k:"저는 주 4일 근무를 합니다.", e:"I only work four days a week."},
            {d:58, s:"교재4", k:"저는 구글 인사과에서 근무하고 있습니다.", e:"I work in HR at Google."},
            {d:58, s:"교재4", k:"제가 지금은 작은 회사에서 일하고 있지만 좀 더 좋은 직장을 알아보고 있습니다.", e:"I’m currently working at a small firm, but I’m looking for something better."},
            {d:58, s:"교재4", k:"그는 6개월째 놀고 있다.", e:"He has been out of work for six months."},
            {d:58, s:"교재4", k:"유감스럽지만 우리는 당신을 해고해야 합니다.", e:"I’m afraid we have to let you go."},
            {d:58, s:"대표", k:"근무 시간은 어때?", e:"What are the hours like?"},

            // Day 59
            {d:59, s:"교재1", k:"매운 한국 음식은 도저히 감당이 안 돼요.", e:"I can’t handle spicy Korean food."},
            {d:59, s:"교재1", k:"한국 여름은 살인적이에요. 제가 열이 많아서 더운 걸 못 견디거든요.", e:"Summers in Korea are brutal. I’m hot-natured, which means I can’t handle the heat."},
            {d:59, s:"교재1", k:"남자아이들로만 구성된 수업을 해야 하는 건 너무 싫어요. 십대 남자아이들 15명은 감당하기 힘듭니다.", e:"I hate when I have to teach all-boys classes. 15 teenage boys are more than I can handle."},
            {d:59, s:"교재1", k:"추가로 더 온다는 사람 있으면 파티 음식은 외부에 맡기자. 10인분 요리하는 건 힘들어.", e:"If anyone else says they are coming, let’s get the party catered. Cooking for ten is more than I can handle."},
            {d:59, s:"교재4", k:"제가 과욕을 부린 것 같아요. 아무래도 수학 학원 강의는 그만둬야 할 것 같아요.", e:"I feel like I bit off more than I can chew. I think I will have to quit my tutoring job at the math academy."},
            {d:59, s:"교재4", k:"프로젝트 하나 더 맡는 건 무리일 것 같아요. 이번 주까지 끝내야 하는 보고서가 이미 세 개나 있어서요.", e:"I don’t think I could take on another project. I already have three reports to finish this week."},
            {d:59, s:"대표", k:"회사에 일이 너무 많긴 한데, 그래도 감당하기 힘든 수준은 아닙니다.", e:"I have a lot on my plate at work, but it’s nothing I can’t handle."},

            // Day 60
            {d:60, s:"교재1", k:"이건 축하해야 할 일이네!", e:"This calls for a celebration!"},
            {d:60, s:"교재1", k:"한잔 더 하러 가야 하겠어.", e:"This calls for another round."},
            {d:60, s:"교재1", k:"이건 올리브기름이 있어야 하거든. 옥수수기름으로 대체해도 될까?", e:"It calls for olive oil. Do you think corn oil will work as a substitute?"},
            {d:60, s:"교재1", k:"이건 바닐라 추출물이 있어야 해. 아직 좀 남았나?", e:"It calls for vanilla extract. Do we still have any?"},
            {d:60, s:"교재1", k:"레시피 보니까 파스타에 치즈를 넣어야 하고, 그 위에다가 치즈 한 겹을 얹어야 한대.", e:"The recipe calls for some cheese in the pasta, but then a whole other layer of cheese on top of that."},
            {d:60, s:"교재4", k:"베트남에서 볶음밥을 주문하게 되면 위에 고수를 많이 얹어 달라고 꼭 부탁하렴.", e:"When you order fried rice in Vietnam, be sure to ask for lots of cilantro on top of it."},
            {d:60, s:"교재4", k:"이 식당 음식이 너무 지나치게 비싸. 게다가, 문 닫는 시간도 들쭉날쭉해.", e:"This restaurant’s food is really overpriced. On top of that, they close at random hours."},
            {d:60, s:"대표", k:"이건 파티해야 돼!", e:"That calls for a party!"},

            // Day 61
            {d:61, s:"교재1", k:"미안한데 잘 못 들었어요. 한 번만 더 이야기해 주시겠어요?", e:"Oh, I’m sorry. I didn’t catch that. Could you repeat it?"},
            {d:61, s:"교재1", k:"성함을 못 들은 것 같습니다.", e:"I’m afraid I didn’t catch your name."},
            {d:61, s:"교재1", k:"(이사한다는 말을 듣고) 날짜를 잘 못 들었어. 언제 다시 이사 간다고?", e:"I didn’t catch the date. When are you moving out again?"},
            {d:61, s:"교재1", k:"(발표자가 청중에게) 혹시 놓친 부분이 있을까요?", e:"Is there anything you weren’t able to catch?"},
            {d:61, s:"교재1", k:"예산에 관해 이야기하실 때 제가 발표 내용 일부를 놓쳤어요.", e:"I didn’t catch the part of your presentation when you talked about the budget."},
            {d:61, s:"교재4", k:"다시 한번 말씀해 주시겠어요? 소음 때문에 잘 못 들었어요.", e:"Could you say that again? I couldn’t quite make you out over all the noise."},
            {d:61, s:"교재4", k:"대충 들리는 말로는, 우리 기차가 10분 정도 늦게 대전역에 도착한다고 하네.", e:"From what I could make out, our train will get to Daejeon station maybe 10 minutes late."},
            {d:61, s:"교재4", k:"동양인들에게는 (미국식과는 다른) 영국식 억양이 알아듣기 힘들 수 있어요.", e:"For Asians, the different British accents can be hard to make out."},
            {d:61, s:"대표", k:"죄송해요. 못 들었어요.", e:"I’m sorry. I didn’t catch that."},

            // Day 62
            {d:62, s:"교재1", k:"당신 어제 새벽 3시나 되어서 집에 들어온 데다, 술 냄새가 진동하더군. 오늘 아픈 게 당연한 거야.", e:"You didn’t get home until 3 a.m., and you reeked of alcohol. No wonder you feel sick today."},
            {d:62, s:"교재1", k:"너희 동네에서 시위가 있었다고 뉴스에서 들었어. 그래서 늦었구나.", e:"I heard on the news that there was a protest in your neighborhood. No wonder you’re late."},
            {d:62, s:"교재1", k:"그 사람은 패션 감각이 전혀 없어요. 그러니까 여자 친구가 안 생기죠.", e:"He has zero fashion sense. No wonder he can’t find a girlfriend."},
            {d:62, s:"교재1", k:"공급망 문제가 있으니, 가격이 올라가는 건 당연하죠.", e:"There’s a supply chain issue, so no wonder prices are rising."},
            {d:62, s:"교재1", k:"재료가 기본적으로 버터, 밀가루, 설탕이군. 그래서 맛이 좋은 거구나.", e:"The ingredients are basically just butter, flour, and sugar. No wonder it tastes good."},
            {d:62, s:"교재4", k:"아침으로 시리얼을 먹은 거야? 좀 더 든든한 걸로 먹어야지.", e:"You just had cereal for breakfast? You should have something more substantial than that."},
            {d:62, s:"교재4", k:"아침을 거르는 경우가 대부분입니다.", e:"I skip breakfast, more often than not."},
            {d:62, s:"교재4", k:"오늘 나랑 점심 가볍게 할까?", e:"You wanna grab lunch with me today?"},
            {d:62, s:"대표", k:"어쩐지 기분이 상쾌해 보이더라.", e:"No wonder you look so refreshed."},

            // Day 63
            {d:63, s:"교재1", k:"제 상황에 해당하는 딱 맞는 단어가 생각이 안 납니다.", e:"I can’t think of the right word for my situation."},
            {d:63, s:"교재1", k:"‘화가 나’보다 내 감정을 더 잘 표현할 수 있는 단어는 없는 듯해.", e:"I can’t think of a better word to describe how I feel than ‘angry’."},
            {d:63, s:"교재1", k:"전시회 너무 멋졌어. 오후 시간을 이보다 더 잘 보낼 수가 있을까?", e:"What a nice exhibition! I can’t think of a better way to spend my afternoon off."},
            {d:63, s:"교재1", k:"남은 치즈를 어디에다 써야 할지 모르겠네.", e:"I can’t think of a use for all this leftover cheese."},
            {d:63, s:"교재1", k:"듀얼 모니터 쓰면 너무 편리해. 동시에 여러 가지 작업을 하거나 작업을 바꿔 가며 하는 게 가능하거든. 단점은 찾을 수가 없어.", e:"Using dual monitors is really convenient. I can multitask or switch between tasks. I can’t think of any downside."},
            {d:63, s:"교재4", k:"A: 이제 구인 공고 올리셨나요?", e:"Have you gotten around to posting that job opening yet?"},
            {d:63, s:"교재4", k:"B: 죄송해요. 정말 깜박했어요.", e:"Oh, I’m sorry. That slipped my mind."},
            {d:63, s:"교재4", k:"잘 기억이 안 나.", e:"I don’t quite remember."},
            {d:63, s:"교재4", k:"나 공과금 안 낸 거 이제 생각났네.", e:"It just occurred to me that I didn’t pay the bill."},
            {d:63, s:"교재4", k:"방금 든 생각인데요. 교사로 일할 때 학생들 대부분이 꿈은 없고 공부에만 전념했던 것 같네요.", e:"It just occurred to me that, when I worked as a teacher, most of my students didn’t have any dreams, and only focused on studying."},
            {d:63, s:"대표", k:"이 말을 어떻게 꺼내야 할지.", e:"I can’t think of the right thing to say."},

            // Day 64
            {d:64, s:"교재1", k:"그래서 내가 다른 재즈 페스티벌에 안 가는 거야. 그나저나 넌 재즈 좋아해?", e:"That’s why I’ll never go to another Jazz Festival. Do you like jazz, by the way?"},
            {d:64, s:"교재1", k:"이 방에는 두 개의 트윈 침대가 구비되어 있습니다. 참고로 해변도 너무 잘 보입니다.", e:"The room comes with two twin beds. You’ll also have a great view of the beach, by the way."},
            {d:64, s:"교재1", k:"제 친구가 트레이너를 구하고 있어요. 참고로 제 친구는 싱글이에요.", e:"I have a friend who’s looking for a trainer. He’s single, by the way."},
            {d:64, s:"교재1", k:"Mark랑 Mindy 먹을 음식도 주문해야 해. 그나저나 그 친구들은 언제 도착한대?", e:"We’ll just have to order something for Mark and Mindy. When are they coming, by the way?"},
            {d:64, s:"교재4", k:"A: Greg, 비 오니?", e:"Is it raining, Greg?"},
            {d:64, s:"교재4", k:"B: 아니, 그래도 비옷 챙기렴. 비 올지도 모르잖아.", e:"No, but bring a raincoat anyway. It might rain."},
            {d:64, s:"교재4", k:"A: 어디로 2차를 가야 할지 모르겠네. 주변에 내가 제일 좋아하는 바는 벌써 영업이 끝났거든.", e:"I don’t know where a good place would be to keep drinking. My favorite bar around here is already closed."},
            {d:64, s:"교재4", k:"B: 사실, 괜찮아. 어차피 나 지금 집에 가서 자야 하거든.", e:"Actually, it’s okay. It’s about time for me to get home and go to bed anyway."},
            {d:64, s:"교재4", k:"파리 물가가 비싸긴 하지. 그래도 많은 사람들이 파리에서 휴가를 즐기고 싶어 해.", e:"Paris is expensive, but many people would like to vacation there, anyway."},
            {d:64, s:"대표", k:"새해가 코앞이네. 그나저나 너 부모님 댁에 갈 거야?", e:"New Year’s is just around the corner. Are you going to your parent’s house, by the way?"},

            // Day 65
            {d:65, s:"교재1", k:"너 남대문 열린 거 아니?", e:"Are you aware your zipper is down?"},
            {d:65, s:"교재1", k:"너 이에 뭐 낀 거 알아?", e:"Are you aware there is something in your teeth?"},
            {d:65, s:"교재1", k:"이 좌석이 예약석인 줄을 몰랐습니다.", e:"I wasn’t aware that this table was reserved."},
            {d:65, s:"교재1", k:"너 아는지 모르겠는데, 화장실 세면대 막혔어.", e:"I don’t know if you’re aware of this, but the bathroom sink is clogged."},
            {d:65, s:"교재1", k:"네가 아는지 모르겠지만, 수지 동생이 많이 아파.", e:"I don’t know if you’re aware of it, but Suzie’s brother has been seriously ill."},
            {d:65, s:"교재4", k:"지퍼가 열렸네.", e:"Your fly is open."},
            {d:65, s:"교재4", k:"내가 너를 잃는 것 같은 느낌이 들어.", e:"I feel like I’m losing you."},
            {d:65, s:"대표", k:"시간 가는 줄도 몰랐네.", e:"I wasn’t aware of the time."},

            // Day 66
            {d:66, s:"교재1", k:"(그룹 콜(단체 통화) 상황에서) 이제 전화를 끊어야 할 듯하네요.", e:"I’m afraid we’ll have to let you go."},
            {d:66, s:"교재1", k:"벌써 밤 11시네. 전화 끊어야겠다.", e:"It’s already 11 p.m. I think I will have to let you go."},
            {d:66, s:"교재1", k:"전화 끊어야 할 듯해. 나 버스 타거든.", e:"I think I will have to let you go. I’m getting on the bus."},
            {d:66, s:"교재1", k:"전화 끊어야겠다. 다른 전화가 들어와서.", e:"I’ll have to let you go. I’m getting another call."},
            {d:66, s:"교재1", k:"(줌 회의 중에) 이 회의 바로 다음에 다른 회의가 잡혀 있어서, 죄송하지만 여기서 마쳐야 할 것 같습니다.", e:"I have another meeting scheduled right after this, so I’m afraid I’ll have to let you go."},
            {d:66, s:"교재4", k:"나 아직 사무실이야. 아무래도 오늘은 못 갈 거 같아.", e:"I’m still in the office. I don’t think I can make it today."},
            {d:66, s:"교재4", k:"접속 장애로 인해 오전 내내 폴더에 접속을 할 수가 없네요.", e:"Due to connection issues, I’ve been unable to get into the folder all morning."},
            {d:66, s:"교재4", k:"제 네이버 계정에 접속이 안 되는데 왜 그런지 모르겠어요.", e:"I can’t get into my Naver account, but I don’t know why."},
            {d:66, s:"대표", k:"이제 그만 들어가 보렴.", e:"I think I will have to let you go."},

            // Day 67
            {d:67, s:"교재1", k:"한 정거장만 더 가면 돼. 곧 도착해!", e:"I’m only one stop away. I’ll be there soon!"},
            {d:67, s:"교재1", k:"주말이 이틀밖에 안 남았는데, 아직 계획이 없어. 나랑 서울 숲 근처에서 뭐 할래?", e:"The weekend is only two days away, but I don’t have any plans yet. Do you want to do something with me around Seoul Forest?"},
            {d:67, s:"교재1", k:"여자 친구 생일이 일주일밖에 안 남았다니. 뭘 해 줘야 할지 모르겠어.", e:"I can’t believe my girlfriend’s birthday is only a week away. I still don’t know what to get her."},
            {d:67, s:"교재1", k:"밸런타인데이가 며칠 안 남았네. 아내를 놀라게 해 주고 싶은데, 뭐가 제일 좋을지 고민이야.", e:"Valentine’s Day is only a few days away. I want to surprise my wife, but I still can’t figure out what would be best."},
            {d:67, s:"교재4", k:"저 학위 마치기까지 한 학기밖에 안 남았어요.", e:"I’m only one semester away from finishing my degree."},
            {d:67, s:"교재4", k:"이제 8년만 있으면 연금을 받습니다.", e:"I’m only eight years away from getting my pension."},
            {d:67, s:"교재4", k:"콘서트 시작까지 2시간밖에 안 남았어요.", e:"We’re only two hours away from the concert getting started."},
            {d:67, s:"교재4", k:"이 책 끝내는 데 딱 33일 남았어!", e:"I am only 33 days away from finishing this book!"},
            {d:67, s:"대표", k:"설이 일주일도 채 안 남았네.", e:"Lunar New Year is less than a week away."},

            // Day 68
            {d:68, s:"교재1", k:"(공사가 지연되는 상황에서) 3개월 지연되고 있는 상태입니다.", e:"I’m afraid we are three months behind schedule."},
            {d:68, s:"교재1", k:"<이상한 변호사 우영우> 보고 있는데 한 3, 4화 정도 밀렸거든. 그러니까 줄거리 미리 말해서 초 치지 마.", e:"I’ve been watching Extraordinary Attorney Woo, but I’m, like, three or four episodes behind. Please don’t spoil anything."},
            {d:68, s:"교재1", k:"공과금이 한 번 밀리기 시작하면, 계속 밀리게 돼.", e:"Once you get behind on your bills, it can be difficult to catch up."},
            {d:68, s:"교재1", k:"나 숙제가 좀 밀렸어. 주말 내내 못 한 숙제를 해야 해.", e:"I’m pretty behind on homework. I’ll have to spend all weekend catching up."},
            {d:68, s:"교재4", k:"나 지금 터널 통과 중이야.", e:"I am going through a tunnel."},
            {d:68, s:"교재4", k:"요즘 Sally가 여러 가지로 힘들어.", e:"Sally is going through a lot lately."},
            {d:68, s:"교재4", k:"옷장을 샅샅이 뒤져 봤는데도, 결혼식에 뭐 입고 갈지를 모르겠네요.", e:"I went through my whole closet, but I can’t figure out what to wear for the wedding."},
            {d:68, s:"대표", k:"일이 밀려서 점심 먹을 시간도 없어.", e:"I don’t have time to grab lunch. I’m behind on work."},

            // Day 69
            {d:69, s:"교재1", k:"A: 매출이 살아나고 있습니다. Sales are picking up.", e:"Sales are picking up."},
            {d:69, s:"교재1", k:"B: 확실히 말이죠. That’s for sure.", e:"That’s for sure."},
            {d:69, s:"교재1", k:"A: 정치인들은 다 거짓말쟁이야.", e:"Politicians are all liars."},
            {d:69, s:"교재1", k:"B: 그건 확실해.", e:"That’s for sure."},
            {d:69, s:"교재1", k:"A: 우리는 이와 같은 무역 전쟁을 계속해서는 안 됩니다.", e:"We can’t afford to continue this trade war."},
            {d:69, s:"교재1", k:"B: 그 점은 분명합니다.", e:"That’s for sure."},
            {d:69, s:"교재1", k:"A: 제 남편이 바람을 피울 사람은 아니에요.", e:"My husband is not the kind of guy who would ever cheat."},
            {d:69, s:"교재1", k:"B: 그건 확실해요.", e:"That’s for sure."},
            {d:69, s:"교재1", k:"A: Jonathan은 그 정도 스펙이면 다른 직장 구하는 데 문제 없을 거야.", e:"Jonathan won’t have any trouble getting another job with his qualifications."},
            {d:69, s:"교재1", k:"B: 내 말이.", e:"That’s for sure."},
            {d:69, s:"교재4", k:"전적으로 동의합니다.", e:"I couldn’t agree with you more."},
            {d:69, s:"교재4", k:"제 말이요. (상대의 말에 맞장구 치는 말)", e:"You can say that again."},
            {d:69, s:"교재4", k:"그 점에 있어서는 동의하기 힘듭니다.", e:"I can’t agree with you on that."},
            {d:69, s:"대표", k:"누가 아니래.", e:"That’s for sure."},

            // Day 70
            {d:70, s:"교재1", k:"블랙과 화이트의 컬러 조합은 잘못될 수가 없습니다.", e:"You can’t go wrong with the color combination of black and white."},
            {d:70, s:"교재1", k:"어두운 회색 슈트는 언제나 옳지.", e:"You can’t go wrong with a dark gray suit."},
            {d:70, s:"교재1", k:"날씨 좋은 곳 원하면, 무조건 샌디에이고가 답이다.", e:"If you want good weather, you can’t go wrong with San Diego."},
            {d:70, s:"교재1", k:"감자와 치즈는 언제나 옳다.", e:"Potatoes and cheese. You can’t go wrong with them."},
            {d:70, s:"교재1", k:"BMW는 언제나 옳은 선택이지.", e:"You can never go wrong with BMW."},
            {d:70, s:"교재4", k:"보통은 역까지 2~3분밖에 안 걸리는데, 항상 최소 6분은 일찍 집을 나섭니다. 혹시 모르니까 말이죠.", e:"It usually only takes two or three minutes to get to the station, but I always try to leave my place at least six minutes early, to be safe."},
            {d:70, s:"교재4", k:"아침에 눈이 온다더라. 혹시 모르니 버스보다는 지하철 타는 게 나을 것 같아.", e:"I heard it will snow in the morning. It might be better to take the subway rather than the bus, just to be safe."},
            {d:70, s:"교재4", k:"고수가 오천 원 정도 한다던데. 그래도 혹시 모르니 만 원 가져가 보려고.", e:"I heard cilantro is around 5,000 won. I’ll bring 10,000 won to be safe."},
            {d:70, s:"대표", k:"검은색이 진리지.", e:"You can’t go wrong with black."},

            // Day 71
            {d:71, s:"교재1", k:"저녁 먹고 갈래?", e:"Can you stay for dinner?"},
            {d:71, s:"교재1", k:"저 사실 이케아에 가구 보러 다녀왔어요.", e:"I actually went shopping for furniture at IKEA."},
            {d:71, s:"교재1", k:"나가서 잠시 바람 좀 쐬어도 될까요?", e:"Can I go out just a couple of minutes for some fresh air?"},
            {d:71, s:"교재1", k:"나 면접이 있어서 먼저 일어나 봐야 할 것 같아.", e:"I’m afraid I’ll have to leave early for an interview."},
            {d:71, s:"교재1", k:"내일 아침에 건강 검진이 있어서 저녁 8시부터 금식해야 해요.", e:"I have to fast, starting at 8 p.m., for my check-up tomorrow morning."},
            {d:71, s:"교재4", k:"디자인 때문에 아이폰으로 선택했어요.", e:"I chose the iPhone for its design."},
            {d:71, s:"교재4", k:"그 여자는 그 남자 돈 보고 만나는 거야?", e:"Is she with him for his money?"},
            {d:71, s:"교재4", k:"그는 지각해서 혼났다.", e:"He got told off for coming in late."},
            {d:71, s:"교재4", k:"그 집 리모델링한다고 영업을 안 해.", e:"They are closed for remodeling."},
            {d:71, s:"대표", k:"나가서 바람이나 좀 쐬고 오자.", e:"Let’s go out for some fresh air."},

            // Day 72
            {d:72, s:"교재1", k:"너 만나서 저녁 먹으니 좋긴 한데 너무 갑작스럽게 연락했네. 어떤 일 때문에 보자고 한 거니?", e:"I’m happy to meet you for dinner, but you kind of called me out of the blue. What is this about?"},
            {d:72, s:"교재1", k:"Sally가 문자에 답이 없네. 너 혹시 Sally가 왜 그러는지 아니?", e:"Sally isn’t responding to my texts. Do you know what that’s about?"},
            {d:72, s:"교재1", k:"보니까 우리 이따가 회의가 하나 더 잡혀 있군요. 어떤 내용일까요?", e:"I see we have another meeting scheduled later. What is it about?"},
            {d:72, s:"교재1", k:"혹시 어떤 일 때문에 그러시는지 여쭤봐도 될까요?", e:"May I ask what this is about?"},
            {d:72, s:"교재4", k:"James랑 일하는 게 쉽지가 않더군요.", e:"It was tough working with James."},
            {d:72, s:"교재4", k:"그동안 함께 일할 수 있어서 너무 좋았습니다. 앞으로 성공만이 깃들길 기원합니다.", e:"It was a great pleasure working with you. I wish you nothing but success in the future."},
            {d:72, s:"교재4", k:"스코틀랜드 사람들 말은 늘 이해하기 어려웠어. 그래서 자막 없이 이번 영화를 보는 게 너무 힘들더라.", e:"I’ve always had trouble understanding Scottish people, so it was tough watching the movie with no subtitles."},
            {d:72, s:"교재4", k:"계단으로 드레서를 옮기는 게 진짜 힘들더군요.", e:"It was a real struggle getting the dresser up all those stairs."},
            {d:72, s:"대표", k:"무슨 일이신가요? 제 검사 결과 관련된 건가요?", e:"What is this about? Is this about my test results?"},

            // Day 73
            {d:73, s:"교재1", k:"그 여자분 텍사스 사람 같더라.", e:"She sounded like a Texan."},
            {d:73, s:"교재1", k:"들어 보니까 그 사람 최고의 남자 친구네.", e:"He sounds like a perfect boyfriend."},
            {d:73, s:"교재1", k:"이 후기들 보니까 퇴근 후에 가서 시간 보내기 좋은 곳인 듯.", e:"These reviews make it sound like a chill place to hang out after work."},
            {d:73, s:"교재1", k:"말하는 거 들어 보니 우리랑 캠핑 갈 생각이 별로 없는 것 같군.", e:"You don’t sound like you are really interested in joining us for camping."},
            {d:73, s:"교재1", k:"진짜 제대로 된 바비큐를 못 먹어 본 모양이군.", e:"It sounds like you haven’t really tried authentic barbecue."},
            {d:73, s:"교재4", k:"너 힘이 없어 보인다.", e:"You look low on energy."},
            {d:73, s:"교재4", k:"그 셔츠 입지 마. 범생 같아 보이잖아.", e:"You shouldn’t wear that shirt. It makes you look like a nerd."},
            {d:73, s:"교재4", k:"보니까 저 여성분 성형한 듯.", e:"She looks like she got some work done."},
            {d:73, s:"교재4", k:"보니까 너 머리 엄청 신경 쓰나 보다. 아침마다 머리 만지는 데 얼마나 걸리는지 궁금하네.", e:"It looks like you are very particular about your hair. I wonder how long it takes you to fix it every morning."},
            {d:73, s:"대표", k:"그게 훨씬 더 낫겠다.", e:"That sounds like an even better plan to me."},

            // Day 74
            {d:74, s:"교재1", k:"제가 뭐 운전을 잘 못하는 것도 아닌데요.", e:"It’s not like I’m a bad driver or anything."},
            {d:74, s:"교재1", k:"내가 늘 패스트푸드를 먹는 것도 아닌데.", e:"It’s not like I eat fast food all the time."},
            {d:74, s:"교재1", k:"우리 가스 요금이 너무 많이 나왔더라, 근데 괜찮아. 감당하기 힘든 수준까지는 아니니.", e:"Our gas bill was really high, but it’s okay. It’s not like we can’t afford it."},
            {d:74, s:"교재1", k:"직장을 잃는 게 아니잖아. 그냥 전근 가는 거잖아.", e:"It’s not like you’re losing your job. It’s just a transfer."},
            {d:74, s:"교재1", k:"제가 무슨 욕을 했나요? 뭘 그걸 가지고 그러시죠?", e:"It’s not like I swore at you. What’s the big deal?"},
            {d:74, s:"교재4", k:"민수가 연남동에 있는 바에서 어떤 미국 여자에게 작업을 걸고 있는 걸 봤어.", e:"I saw Minsu hitting on some American girl at a bar in Yeonnam-dong."},
            {d:74, s:"교재4", k:"지난 세 명의 여자 친구들과 헤어져서인지, 이제 한 명에게 집중을 못 할 것 같은 기분이 드네요.", e:"After my last three girlfriends broke up with me, I feel like I have some commitment issues."},
            {d:74, s:"교재4", k:"Tom이랑 저는 사실 진지하게 만나고 있습니다.", e:"Tom and I are actually in a serious relationship."},
            {d:74, s:"교재4", k:"보니까 그 친구 가볍게 만날 사람을 찾는 듯해.", e:"It seems like he is just looking for a casual relationship."},
            {d:74, s:"대표", k:"저희가 뭐 진지하게 사귀고 그런 건 아니에요.", e:"It’s not like we are in a serious relationship or anything."},

            // Day 75
            {d:75, s:"교재1", k:"저녁 사 줘서 고마워! 근데 진짜 우리 집 가서 커피 안 하려고?", e:"Thank you for dinner! Are you sure you don’t want to come up for coffee?"},
            {d:75, s:"교재1", k:"진짜 우리랑 같이 저녁 안 하려고? 지난번에도 사장님 제안 거절했잖아!", e:"Are you sure you’re not going to join us for dinner? You turned down the boss last time, too!"},
            {d:75, s:"교재1", k:"내 여자 친구의 친구랑 진짜 소개팅 안 할 거야?", e:"Are you sure you don’t want to go on a blind date with my girlfriend’s friend?"},
            {d:75, s:"교재1", k:"진짜 적금 계좌 안 만드실 거예요? 시중 은행 중에 저희 금리가 제일 세거든요.", e:"Are you sure you don’t want to open a savings account? We’re offering higher interest rates than anyone else."},
            {d:75, s:"교재4", k:"마트 영업 끝나기 직전에 가면, 조리 식품을 30% 할인된 가격에 살 수 있어.", e:"If you go to the store just before closing, you can get like 30% off on prepared food."},
            {d:75, s:"교재4", k:"새 모델이 다음 주에 할인에 들어갑니다.", e:"The new model goes on sale next week."},
            {d:75, s:"교재4", k:"원래는 이 초밥 살 생각이 없었는데, 보니까 60% 세일을 하더라고.", e:"I didn’t plan on buying this sushi, but I saw it was marked down 60%."},
            {d:75, s:"대표", k:"정말로 내 초밥 안 먹어 볼 거야? 진짜 맛있는데.", e:"Are you sure you don’t want to try my sushi? It’s really good."}
        ];

        allData.forEach((item, index) => item.id = index + 1);
        const sourceMap = { 
            "대표": "대표예제", 
            "교재1": "Model examples", 
            "교재2": "Small talk", 
            "교재3": "Cases in point", 
            "교재4": "Further studies" 
        };

        let selectedWeekMode = 'w11';
        let quizPool = [];
        let currentIndex = 0;
        let starredIds = new Set();

        function selectWeek(mode) {
            selectedWeekMode = mode;
            document.querySelectorAll('.week-tab').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(`tab-${mode}`).classList.add('active-tab');
            
            const titleEl = document.getElementById('selected-week-title');
            const titles = {
                'w11': 'DAY 51-55', 'w12': 'DAY 56-60', 'w13': 'DAY 61-65',
                'w14': 'DAY 66-70', 'w15': 'DAY 71-75', 'total': 'DAY 51-75 전체'
            };
            titleEl.innerText = titles[mode];
        }

        function unlockAudio() {
            if ('speechSynthesis' in window) {
                const msg = new SpeechSynthesisUtterance("");
                window.speechSynthesis.speak(msg);
            }
        }

        function startQuiz(difficulty) {
            unlockAudio();
            let filtered = [];
            
            // 주차 필터링
            if (selectedWeekMode === 'total') {
                filtered = allData.filter(item => item.d >= 51 && item.d <= 75);
            } else {
                const startDay = (parseInt(selectedWeekMode.replace('w', '')) - 1) * 5 + 1;
                const endDay = startDay + 4;
                filtered = allData.filter(item => item.d >= startDay && item.d <= endDay);
            }

            // 난이도 필터링
            if (difficulty === 'mild') {
                filtered = filtered.filter(item => item.s === '대표' || item.s === '교재1');
            }

            if (filtered.length === 0) {
                alert("해당 범위에 데이터가 없습니다.");
                return;
            }

            quizPool = filtered.sort(() => Math.random() - 0.5).slice(0, 10);
            currentIndex = 0;
            starredIds.clear();
            
            document.getElementById('setup-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            showQuestion();
        }

        function showQuestion() {
            const item = quizPool[currentIndex];
            const card = document.getElementById('card-container');
            card.classList.remove('flipped');
            document.getElementById('next-btn').classList.add('hidden');
            document.getElementById('star-btn').classList.remove('star-checked');
            
            document.getElementById('q-korean').innerText = item.k;
            document.getElementById('q-english').innerText = item.e;
            document.getElementById('q-info').innerText = `Day ${item.d} - ${sourceMap[item.s]}`;
            document.getElementById('progress-text').innerText = `${currentIndex + 1} / ${quizPool.length}`;
            
            if (starredIds.has(item.id)) document.getElementById('star-btn').classList.add('star-checked');
        }

        function flipCard() {
            document.getElementById('card-container').classList.add('flipped');
            document.getElementById('next-btn').classList.remove('hidden');
        }

        function toggleStar(e) {
            e.stopPropagation();
            const item = quizPool[currentIndex];
            const starBtn = document.getElementById('star-btn');
            if (starredIds.has(item.id)) {
                starredIds.delete(item.id);
                starBtn.classList.remove('star-checked');
            } else {
                starredIds.add(item.id);
                starBtn.classList.add('star-checked');
            }
        }

        function playTTS() {
            const text = document.getElementById('q-english').innerText;
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const msg = new SpeechSynthesisUtterance(text);
                msg.lang = 'en-US';
                msg.rate = 0.9;
                window.speechSynthesis.speak(msg);
            }
        }

        function nextQuestion() {
            currentIndex++;
            if (currentIndex < quizPool.length) showQuestion();
            else showResults();
        }

        function showResults() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            const listEl = document.getElementById('starred-list');
            listEl.innerHTML = '<p class="text-[10px] text-slate-400 font-black mb-4 uppercase tracking-widest text-center">⭐ 체크한 문장 다시보기</p>';
            
            const starredItems = quizPool.filter(item => starredIds.has(item.id));
            if (starredItems.length === 0) {
                listEl.innerHTML += '<p class="text-sm text-slate-400 py-12 text-center font-bold">오답이 없습니다! 완벽해요.</p>';
            } else {
                starredItems.forEach(item => {
                    const div = document.createElement('div');
                    div.className = "p-5 bg-indigo-50 rounded-2xl border border-indigo-100 mb-3";
                    div.innerHTML = `
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-[9px] bg-indigo-500 text-white px-1.5 py-0.5 rounded font-black">DAY ${item.d}</span>
                            <span class="text-[9px] text-indigo-400 font-bold italic">${sourceMap[item.s]}</span>
                        </div>
                        <p class="text-xs text-slate-500 mb-1">${item.k}</p>
                        <p class="text-sm font-black text-indigo-900">${item.e}</p>
                    `;
                    listEl.appendChild(div);
                });
            }
        }
    </script>
</body>
</html>
