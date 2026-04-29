# j89012g.github.io
Website for Record Exercise

<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>172cm 女神計畫 V3.3</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { font-family: -apple-system, system-ui, sans-serif; background: #fff5f5; color: #4a5568; -webkit-tap-highlight-color: transparent; }
        .card { background: white; border-radius: 20px; box-shadow: 0 4px 15px rgba(244,63,94,0.1); margin-bottom: 20px; padding: 20px; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .active-tab { color: #f43f5e; border-bottom: 3px solid #f43f5e; }
        input[type="checkbox"] { width: 24px; height: 24px; accent-color: #f43f5e; cursor: pointer; }
        input[type="number"], input[type="text"] { border-bottom: 2px solid #fee2e2; outline: none; padding: 4px; background: transparent; width: 100%; }
        .btn-save { background: linear-gradient(135deg, #f43f5e, #fb7185); color: white; padding: 16px; border-radius: 15px; width: 100%; font-weight: bold; box-shadow: 0 4px 10px rgba(244,63,94,0.3); transition: transform 0.1s; }
        .btn-save:active { transform: scale(0.98); }
        .workout-day-btn { padding: 8px 16px; border-radius: 20px; font-size: 13px; border: 1px solid #f43f5e; color: #f43f5e; }
        .workout-day-btn.active { background: #f43f5e; color: white; }
    </style>
</head>
<body class="pb-32">

    <header class="bg-gradient-to-r from-rose-500 to-rose-400 text-white p-8 text-center rounded-b-[40px] shadow-lg">
        <h1 class="text-xl font-bold tracking-widest uppercase">Abs Awakening 90D</h1>
        <div id="countdown" class="text-4xl font-black my-2">D-90</div>
        <p class="text-xs opacity-90">172cm / 110g 蛋白質 / 2500ml 水</p>
    </header>

    <main class="p-5">
        
        <section id="dashboard" class="tab-content active">
            <h2 class="text-lg font-bold mb-4">🏆 今日達成狀況</h2>
            <div class="card">
                <div class="flex justify-between items-center mb-4">
                    <span class="font-medium">🏋️ 重訓 (週目標 4 次)</span>
                    <span id="workout-count-display" class="font-black text-rose-500 text-xl text-right">0 / 4</span>
                </div>
                <div class="flex justify-between gap-2">
                    <input type="checkbox" class="track-check" data-id="w1">
                    <input type="checkbox" class="track-check" data-id="w2">
                    <input type="checkbox" class="track-check" data-id="w3">
                    <input type="checkbox" class="track-check" data-id="w4">
                </div>
            </div>

            <div class="card">
                <div class="flex justify-between items-center mb-4">
                    <span class="font-medium text-blue-600">💧 水量 (每格350ml)</span>
                    <span id="water-count-display" class="font-black text-blue-500 text-xl text-right">0 / 7</span>
                </div>
                <div class="grid grid-cols-7 gap-2">
                    <input type="checkbox" class="track-check water-node" data-id="d1"> 
                    <input type="checkbox" class="track-check water-node" data-id="d2"> 
                    <input type="checkbox" class="track-check water-node" data-id="d3"> 
                    <input type="checkbox" class="track-check water-node" data-id="d4">
                    <input type="checkbox" class="track-check water-node" data-id="d5"> 
                    <input type="checkbox" class="track-check water-node" data-id="d6">
                    <input type="checkbox" class="track-check water-node" data-id="d7">
                </div>
            </div>

            <button id="main-save-btn" onclick="saveToHistory()" class="btn-save text-lg">✨ 完成今日打卡 (存檔並清空)</button>
            <p id="save-status" class="text-center mt-3 font-bold text-green-500 hidden">✅ 已成功存入歷史紀錄！</p>
        </section>

        <section id="diet" class="tab-content">
            <h2 class="text-lg font-bold mb-4">🥗 蛋白質 110g 目標</h2>
            <div class="card">
                <div class="mb-5">
                    <label class="text-xs font-bold text-rose-500 uppercase">Meal 1 (12:00)</label>
                    <input type="number" id="p1" class="p-input text-xl font-bold" placeholder="輸入蛋白質(g)" oninput="calcProtein()">
                </div>
                <div class="mb-5">
                    <label class="text-xs font-bold text-rose-500 uppercase">Snack (15:30)</label>
                    <input type="number" id="p2" class="p-input text-xl font-bold" placeholder="輸入蛋白質(g)" oninput="calcProtein()">
                </div>
                <div class="mb-5">
                    <label class="text-xs font-bold text-rose-500 uppercase">Meal 3 (19:30)</label>
                    <input type="number" id="p3" class="p-input text-xl font-bold" placeholder="輸入蛋白質(g)" oninput="calcProtein()">
                </div>
                <div class="mt-6 p-6 bg-rose-500 text-white rounded-2xl text-center shadow-lg">
                    <div class="text-sm opacity-80 mb-1">今日總計蛋白質</div>
                    <div class="text-4xl font-black"><span id="total-p-display">0</span> / 110g</div>
                </div>
            </div>
        </section>

        <section id="workout" class="tab-content">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-lg font-bold">🏋️ Matrix 器材</h2>
                <div class="flex gap-2">
                    <button class="workout-day-btn active" id="btn-mon" onclick="setWorkout('mon')">週一</button>
                    <button class="workout-day-btn" id="btn-tue" onclick="setWorkout('tue')">週二</button>
                    <button class="workout-day-btn" id="btn-thu" onclick="setWorkout('thu')">週四</button>
                </div>
            </div>
            <div id="workout-list"></div>
            <div class="card bg-orange-50 border-none mt-4">
                <p id="cardio-text" class="text-sm text-orange-700 font-medium"></p>
            </div>
        </section>

        <section id="history" class="tab-content">
            <h2 class="text-lg font-bold mb-4">📜 歷史戰績</h2>
            <div id="history-container" class="space-y-4"></div>
            
            <div id="clear-confirm-box" class="hidden mt-8 p-4 bg-rose-50 rounded-xl border border-rose-200 text-center">
                <p class="text-sm text-rose-700 font-bold mb-3">⚠️ 確定要刪除所有紀錄嗎？<br><span class="text-xs text-rose-500 font-normal">清空後無法復原喔！</span></p>
                <div class="flex gap-3 justify-center">
                    <button onclick="cancelClear()" class="px-6 py-2 bg-slate-300 text-slate-700 rounded-xl text-sm font-bold active:scale-95 transition-transform">取消</button>
                    <button onclick="executeClear()" class="px-6 py-2 bg-rose-500 text-white rounded-xl text-sm font-bold shadow-md active:scale-95 transition-transform">確定清空</button>
                </div>
            </div>
            
            <button id="btn-clear-trigger" onclick="triggerClear()" class="mt-8 py-4 text-sm font-medium text-slate-400 w-full underline hover:text-rose-500 active:text-rose-600 transition-colors">清空歷史紀錄</button>
        </section>

    </main>

    <nav class="fixed bottom-6 left-5 right-5 bg-white/95 backdrop-blur-md rounded-3xl shadow-2xl flex justify-around p-4 border border-rose-100">
        <button onclick="showTab('dashboard', this)" class="active-tab flex flex-col items-center w-1/4">
            <span class="text-xl">📊</span><span class="text-[10px] font-bold">打卡</span>
        </button>
        <button onclick="showTab('diet', this)" class="text-slate-400 flex flex-col items-center w-1/4">
            <span class="text-xl">🥗</span><span class="text-[10px] font-bold">飲食</span>
        </button>
        <button onclick="showTab('workout', this)" class="text-slate-400 flex flex-col items-center w-1/4">
            <span class="text-xl">🏋️</span><span class="text-[10px] font-bold">課表</span>
        </button>
        <button onclick="showTab('history', this)" class="text-slate-400 flex flex-col items-center w-1/4">
            <span class="text-xl">📜</span><span class="text-[10px] font-bold">歷史</span>
        </button>
    </nav>

    <script>
        const workoutData = {
            mon: { title: "上半身 + 核心", cardio: "🏃 有氧：坡度快走 (坡5, 速6, 20分)", moves: ["滑輪下拉", "坐姿推胸", "滑輪伐木", "懸垂舉腿"] },
            tue: { title: "下半身 + 燃脂", cardio: "🏃 有氧：慢跑 (速8, 20分)", moves: ["仰臥蹬腿", "史密斯深蹲", "腿部伸展", "平板支撐"] },
            thu: { title: "全身 + HIIT", cardio: "🏃 有氧：HIIT (衝30s/走60s, 10回)", moves: ["壺鈴擺盪", "坐姿划船", "山爬者", "真空腹訓練"] }
        };

        function initHistory() {
            if (!localStorage.getItem('goddess_initialized')) {
                const sampleData = [
                    {
                        date: "4/27 (一)", protein: "112", workout: "1 / 4", water: "7 / 7",
                        details: [
                            { name: "滑輪下拉", sets: ["20kg*12下", "25kg*10下", "25kg*10下"] },
                            { name: "坐姿推胸", sets: ["15kg*12下", "15kg*12下"] }
                        ]
                    },
                    {
                        date: "4/28 (二)", protein: "105", workout: "2 / 4", water: "6 / 7",
                        details: [
                            { name: "仰臥蹬腿", sets: ["40kg*15下", "45kg*12下", "50kg*10下"] },
                            { name: "史密斯深蹲", sets: ["20kg*12下", "20kg*12下"] }
                        ]
                    }
                ];
                localStorage.setItem('goddess_history', JSON.stringify(sampleData));
                localStorage.setItem('goddess_initialized', 'true');
            }
        }

        function getLastRecord(moveName) {
            const history = JSON.parse(localStorage.getItem('goddess_history') || '[]');
            for (let entry of history) {
                if (entry.details) {
                    const move = entry.details.find(d => d.name === moveName);
                    if (move && move.sets && move.sets.length > 0) {
                        return move.sets[move.sets.length - 1]; 
                    }
                }
            }
            return "無紀錄";
        }

        function showTab(id, el) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('nav button').forEach(b => { b.classList.remove('active-tab', 'text-rose-500'); b.classList.add('text-slate-400'); });
            el.classList.add('active-tab', 'text-rose-500');
            if(id === 'history') {
                renderHistory();
                cancelClear(); // 確保切換分頁時，確認框是關閉的
            }
        }

        function setWorkout(day) {
            document.querySelectorAll('.workout-day-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('btn-'+day).classList.add('active');
            const d = workoutData[day];
            let html = `<h3 class="font-bold text-rose-500 mb-3">${d.title}</h3>`;
            
            d.moves.forEach(m => {
                const last = getLastRecord(m);
                html += `
                <div class="card p-4 mb-3 exercise-item">
                    <div class="flex justify-between items-start mb-2">
                        <div>
                            <span class="text-sm font-bold text-slate-700 exercise-name">${m}</span>
                            <div class="text-[10px] text-slate-400 mt-1">上次：${last}</div>
                        </div>
                        <button onclick="addSet(this)" class="text-[10px] border border-rose-400 text-rose-500 px-2 py-1 rounded-full hover:bg-rose-50">+ 組數</button>
                    </div>
                    <div class="sets-container space-y-2">
                        <div class="flex gap-2 items-center set-row justify-end">
                            <input type="number" placeholder="重量" class="weight-input w-14 text-sm border-b text-center text-rose-600 font-bold"> <span class="text-xs text-slate-400">kg</span>
                            <span class="text-xs text-slate-300 mx-1">x</span>
                            <input type="number" placeholder="次數" class="reps-input w-14 text-sm border-b text-center text-rose-600 font-bold"> <span class="text-xs text-slate-400">下</span>
                        </div>
                    </div>
                </div>`;
            });
            document.getElementById('workout-list').innerHTML = html;
            document.getElementById('cardio-text').innerText = d.cardio;
        }

        function addSet(btn) {
            const container = btn.parentElement.nextElementSibling;
            const setHtml = `
                <div class="flex gap-2 items-center set-row justify-end mt-2 animate-pulse">
                    <input type="number" placeholder="重量" class="weight-input w-14 text-sm border-b text-center text-rose-600 font-bold"> <span class="text-xs text-slate-400">kg</span>
                    <span class="text-xs text-slate-300 mx-1">x</span>
                    <input type="number" placeholder="次數" class="reps-input w-14 text-sm border-b text-center text-rose-600 font-bold"> <span class="text-xs text-slate-400">下</span>
                </div>
            `;
            container.insertAdjacentHTML('beforeend', setHtml);
            setTimeout(() => {
                const newRows = container.querySelectorAll('.animate-pulse');
                newRows.forEach(r => r.classList.remove('animate-pulse'));
            }, 500);
        }

        function calcProtein() {
            const p1 = Number(document.getElementById('p1').value) || 0;
            const p2 = Number(document.getElementById('p2').value) || 0;
            const p3 = Number(document.getElementById('p3').value) || 0;
            document.getElementById('total-p-display').innerText = p1 + p2 + p3;
        }

        document.addEventListener('change', (e) => {
            if(e.target.classList.contains('track-check')) {
                const wCount = document.querySelectorAll('#dashboard input[data-id^="w"]:checked').length;
                document.getElementById('workout-count-display').innerText = `${wCount} / 4`;
                const dCount = document.querySelectorAll('.water-node:checked').length;
                document.getElementById('water-count-display').innerText = `${dCount} / 7`;
            }
        });

        function saveToHistory() {
            const pTotal = document.getElementById('total-p-display').innerText;
            const wDisplay = document.getElementById('workout-count-display').innerText;
            const dDisplay = document.getElementById('water-count-display').innerText;
            const now = new Date();
            const dateStr = `${now.getMonth()+1}/${now.getDate()} (${['日','一','二','三','四','五','六'][now.getDay()]})`;

            const workoutDetails = [];
            document.querySelectorAll('.exercise-item').forEach(item => {
                const name = item.querySelector('.exercise-name').innerText;
                const sets = [];
                item.querySelectorAll('.set-row').forEach(row => {
                    const weight = row.querySelector('.weight-input').value;
                    const reps = row.querySelector('.reps-input').value;
                    if (weight || reps) {
                        sets.push(`${weight || 0}kg*${reps || 0}下`);
                    }
                });
                if (sets.length > 0) {
                    workoutDetails.push({ name, sets });
                }
            });

            const history = JSON.parse(localStorage.getItem('goddess_history') || '[]');
            history.unshift({ 
                date: dateStr, 
                protein: pTotal, 
                workout: wDisplay, 
                water: dDisplay,
                details: workoutDetails 
            });
            localStorage.setItem('goddess_history', JSON.stringify(history));

            document.getElementById('save-status').classList.remove('hidden');
            document.getElementById('main-save-btn').innerText = "✅ 存檔成功";
            
            setTimeout(() => {
                document.getElementById('p1').value = '';
                document.getElementById('p2').value = '';
                document.getElementById('p3').value = '';
                document.querySelectorAll('input[type="checkbox"]').forEach(c => c.checked = false);
                document.querySelectorAll('.weight-input, .reps-input').forEach(input => input.value = '');
                
                document.getElementById('total-p-display').innerText = '0';
                document.getElementById('workout-count-display').innerText = '0 / 4';
                document.getElementById('water-count-display').innerText = '0 / 7';
                document.getElementById('save-status').classList.add('hidden');
                document.getElementById('main-save-btn').innerText = "✨ 完成今日打卡 (存檔並清空)";
                
                try {
                    const activeBtn = document.querySelector('.workout-day-btn.active');
                    if(activeBtn) {
                        const currentDay = activeBtn.id.replace('btn-', '');
                        setWorkout(currentDay);
                    }
                } catch(err) {}
            }, 800);
        }

        function renderHistory() {
            const history = JSON.parse(localStorage.getItem('goddess_history') || '[]');
            const container = document.getElementById('history-container');
            if(history.length === 0) {
                container.innerHTML = '<p class="text-slate-400 text-center py-10">尚無紀錄，開始妳的第一天！</p>';
                return;
            }
            container.innerHTML = history.map(h => {
                let detailsHtml = '';
                if (h.details && h.details.length > 0) {
                    detailsHtml = `<div class="mt-4 border-t border-rose-100 pt-3 text-xs text-left">`;
                    h.details.forEach(d => {
                        detailsHtml += `
                            <div class="mb-2">
                                <span class="font-bold text-rose-500 block mb-1">🔸 ${d.name}</span>
                                <span class="text-slate-500 pl-4 block leading-relaxed">${d.sets.join(' <br> ')}</span>
                            </div>`;
                    });
                    detailsHtml += `</div>`;
                }

                return `
                <div class="card p-4 border-l-4 border-rose-500">
                    <div class="flex justify-between items-center mb-3">
                        <span class="font-bold text-slate-700">${h.date}</span>
                        <span class="text-[10px] bg-rose-100 text-rose-600 px-2 py-1 rounded-full">已達成</span>
                    </div>
                    <div class="grid grid-cols-3 text-center gap-2 text-xs">
                        <div class="bg-slate-50 p-2 rounded">🍗 蛋白質<br><b class="text-rose-500 text-sm">${h.protein}g</b></div>
                        <div class="bg-slate-50 p-2 rounded">💧 水量<br><b class="text-blue-500 text-sm">${h.water}</b></div>
                        <div class="bg-slate-50 p-2 rounded">🎯 訓練<br><b class="text-slate-500 text-sm">${h.workout}</b></div>
                    </div>
                    ${detailsHtml}
                </div>
                `;
            }).join('');
        }

        // --- 取代原本會被瀏覽器擋住的 confirm() ---
        
        // 1. 展開確認區塊
        function triggerClear() {
            document.getElementById('clear-confirm-box').classList.remove('hidden');
            document.getElementById('btn-clear-trigger').classList.add('hidden');
            // 讓畫面稍微往下滑，確保能看到按鈕
            window.scrollTo(0, document.body.scrollHeight);
        }

        // 2. 取消清空
        function cancelClear() {
            document.getElementById('clear-confirm-box').classList.add('hidden');
            document.getElementById('btn-clear-trigger').classList.remove('hidden');
        }

        // 3. 確實執行清空 (純粹的邏輯，不會被擋)
        function executeClear() {
            // 清除資料
            localStorage.removeItem('goddess_history');
            localStorage.setItem('goddess_initialized', 'true'); 
            
            // 重新渲染歷史畫面
            renderHistory();
            
            // 同步清空「課表」裡的「上次紀錄」
            try {
                const activeBtn = document.querySelector('.workout-day-btn.active');
                if (activeBtn) {
                    const currentDay = activeBtn.id.replace('btn-', '');
                    setWorkout(currentDay);
                }
            } catch(e) {}
            
            // 關閉確認區塊
            cancelClear();
        }

        // 倒數計時
        const diff = Math.ceil((new Date('2026-07-28') - new Date()) / (86400000));
        document.getElementById('countdown').innerText = `D-${diff > 0 ? diff : 0}`;

        // 初始化
        initHistory();
        setWorkout('mon');
    </script>
</body>
</html>
