<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>رفيق العلاج - Patient Companion</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts (Cairo for Arabic) -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #f3f4f6;
            -webkit-tap-highlight-color: transparent;
        }
        
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }

        .app-container {
            max-width: 480px;
            margin: 0 auto;
            background-color: #ffffff;
            min-height: 100vh;
            position: relative;
            box-shadow: 0 0 20px rgba(0,0,0,0.05);
            padding-bottom: 85px;
        }

        .nav-item.active {
            color: #3b82f6;
        }
        
        .nav-item.active i {
            transform: translateY(-2px);
        }

        .fade-in {
            animation: fadeIn 0.3s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity:1; transform: translateY(0); }
        }

        input[type=range] {
            -webkit-appearance: none; 
            background: transparent; 
        }
        
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 20px;
            width: 20px;
            border-radius: 50%;
            background: #3b82f6;
            cursor: pointer;
            margin-top: -8px; 
            box-shadow: 0 2px 6px rgba(59, 130, 246, 0.4);
        }

        input[type=range]::-webkit-slider-runnable-track {
            width: 100%;
            height: 4px;
            cursor: pointer;
            background: #e5e7eb;
            border-radius: 2px;
        }

        .modal {
            transition: opacity 0.25s ease;
        }
        body.modal-active {
            overflow: hidden;
        }

        .profile-header {
            background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
        }
    </style>
</head>
<body>

<div class="app-container">
    
    <!-- Header -->
    <header class="bg-white p-6 sticky top-0 z-10 shadow-sm">
        <div class="flex justify-between items-center">
            <div>
                <h1 class="text-2xl font-bold text-gray-800">رفيق العلاج</h1>
                <p class="text-sm text-gray-500" id="header-subtitle">مرحباً بك في رحلة التعافي</p>
            </div>
            <button onclick="switchTab('profile')" class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center text-blue-600 font-bold hover:bg-blue-200 transition">
                <i class="fas fa-user"></i>
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <main id="main-content" class="p-4 space-y-6">
        <!-- Content injected via JS -->
    </main>

    <!-- Bottom Navigation (5 Items) -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white border-t border-gray-100 pb-safe max-w-[480px] mx-auto z-20">
        <div class="flex justify-around items-center h-16">
            <button onclick="switchTab('home')" class="nav-item active flex flex-col items-center justify-center w-full h-full text-gray-400 hover:text-blue-500 transition-colors">
                <i class="fas fa-home text-lg mb-1"></i>
                <span class="text-[10px] font-medium">الرئيسية</span>
            </button>
            <button onclick="switchTab('meds')" class="nav-item flex flex-col items-center justify-center w-full h-full text-gray-400 hover:text-blue-500 transition-colors">
                <i class="fas fa-pills text-lg mb-1"></i>
                <span class="text-[10px] font-medium">الأدوية</span>
            </button>
            <button onclick="switchTab('symptoms')" class="nav-item flex flex-col items-center justify-center w-full h-full text-gray-400 hover:text-blue-500 transition-colors">
                <i class="fas fa-chart-line text-lg mb-1"></i>
                <span class="text-[10px] font-medium">الأعراض</span>
            </button>
            <button onclick="switchTab('reports')" class="nav-item flex flex-col items-center justify-center w-full h-full text-gray-400 hover:text-blue-500 transition-colors">
                <i class="fas fa-folder-open text-lg mb-1"></i>
                <span class="text-[10px] font-medium">ملفاتي</span>
            </button>
            <button onclick="switchTab('tips')" class="nav-item flex flex-col items-center justify-center w-full h-full text-gray-400 hover:text-blue-500 transition-colors">
                <i class="fas fa-lightbulb text-lg mb-1"></i>
                <span class="text-[10px] font-medium">نصائح</span>
            </button>
        </div>
    </nav>

    <!-- Add Medication Modal -->
    <div id="addMedModal" class="modal opacity-0 pointer-events-none fixed w-full h-full top-0 left-0 flex items-center justify-center z-50">
        <div class="modal-overlay absolute w-full h-full bg-gray-900 opacity-50"></div>
        <div class="modal-container bg-white w-11/12 md:max-w-md mx-auto rounded-2xl shadow-2xl z-50 overflow-y-auto p-6 fade-in">
            <div class="flex justify-between items-center pb-3 border-b">
                <p class="text-xl font-bold text-gray-800">إضافة دواء جديد</p>
                <div class="cursor-pointer z-50" onclick="toggleModal('addMedModal')">
                    <i class="fas fa-times text-gray-500"></i>
                </div>
            </div>
            <div class="my-5 space-y-4">
                <div>
                    <label class="block text-gray-700 text-sm font-bold mb-2">اسم الدواء</label>
                    <input id="newMedName" class="w-full bg-gray-50 border border-gray-300 rounded-xl py-3 px-4 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500" type="text" placeholder="مثال: بانادول">
                </div>
                <div class="flex gap-2">
                    <div class="w-1/2">
                        <label class="block text-gray-700 text-sm font-bold mb-2">الوقت</label>
                        <input id="newMedTime" class="w-full bg-gray-50 border border-gray-300 rounded-xl py-3 px-4 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500" type="time">
                    </div>
                    <div class="w-1/2">
                        <label class="block text-gray-700 text-sm font-bold mb-2">الجرعة</label>
                        <input id="newMedDose" class="w-full bg-gray-50 border border-gray-300 rounded-xl py-3 px-4 text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500" type="text" placeholder="مثال: قرص واحد">
                    </div>
                </div>
            </div>
            <div class="flex justify-end pt-2">
                <button onclick="addNewMedication()" class="px-4 py-2 bg-blue-600 text-white rounded-xl font-bold shadow-lg hover:bg-blue-700 transition">حفظ الدواء</button>
            </div>
        </div>
    </div>

</div>

<script>
    // --- Data ---
    let medications = [
        { id: 1, name: 'باراسيتامول (Paracetamol)', time: '08:00', dose: '500 ملغ', taken: false, icon: 'fa-tablets' },
        { id: 2, name: 'مضاد الغثيان', time: '08:00', dose: 'قرص واحد', taken: true, icon: 'fa-capsules' },
        { id: 3, name: 'فيتامين د', time: '14:00', dose: 'قطرات', taken: false, icon: 'fa-sun' },
        { id: 4, name: 'مسكن الألم', time: '20:00', dose: 'حسب الحاجة', taken: false, icon: 'fa-heart-pulse' },
    ];

    const tipsData = [
        { 
            title: 'العناية بالفم', 
            icon: 'fa-tooth',
            short: 'نصائح للحفاظ على نظافة الفم وتقليل التقرحات.',
            detail: 'استخدم فرشاة أسنان ناعمة جداً وشعيرات ناعمة. المضمضة بمحلول ملحي دافئ (ملعقة صغيرة ملح في كوب ماء) 4-6 مرات يومياً. تجنب الأطعمة الحارة أو الحمضية أو القاسية. استخدم غسول فم خالي من الكحول. ترطيب الشفاه بمرطب طبيعي.',
            color: 'bg-red-100 text-red-600'
        },
        { 
            title: 'التغذية المناسبة', 
            icon: 'fa-apple-whole',
            short: 'أفضل الأطعمة لرفع المناعة وتحمل العلاج.',
            detail: 'ركز على البروتينات (لحوم، بيض، بقوليات، أسماك). تناول وجبات صغيرة ومتكررة (5-6 وجبات) بدلاً من وجبات كبيرة. اشرب كميات كافية من الماء (8-10 أكواب يومياً). تجنب الأطعمة النيئة أو غير المطبوخة جيداً. أضف الزنجبيل للطعام لتقليل الغثيان.',
            color: 'bg-green-100 text-green-600'
        },
        { 
            title: 'إدارة التعب والإرهاق', 
            icon: 'fa-bed',
            short: 'كيف توفر طاقتك خلال اليوم.',
            detail: 'خذ قيلولة قصيرة (20-30 دقيقة) ولا تزيد عن ساعة. لا تتردد في طلب المساعدة في المهام المنزلية. مارس تمارين خفيفة مثل المشي 15 دقيقة يومياً. نظم نشاطاتك في أوقات الطاقة العالية. استخدم تقنيات الاسترخاء والتنفس العميق.',
            color: 'bg-blue-100 text-blue-600'
        },
        { 
            title: 'العناية بالبشرة', 
            icon: 'fa-hand-sparkles',
            short: 'حماية البشرة من الجفاف والحساسية.',
            detail: 'استخدم مرطبات خالية من العطور والكحول. تجنب التعرض المباشر لأشعة الشمس واستخدم واقي شمس SPF 50+. استحم بماء فاتر وليس ساخن. استخدم صابون لطيف خالي من المواد الكيميائية القاسية. ارتدِ ملابس قطنية واسعة.',
            color: 'bg-orange-100 text-orange-600'
        },
        { 
            title: 'التعامل مع الغثيان', 
            icon: 'fa-stomach',
            short: 'طرق طبيعية ودوائية لتقليل الغثيان.',
            detail: 'تناول وجبات جافة مثل البسكويت قبل النهوض من السرير. اشرب الزنجبيل الطازج أو النعناع. تجنب الروائح القوية. تناول الطعام بارد أو بدرجة حرارة الغرفة. استخدم أدوية مضادة للغثيان حسب وصفة الطبيب. تجنب الاستلقاء مباشرة بعد الأكل.',
            color: 'bg-yellow-100 text-yellow-600'
        },
        { 
            title: 'تساقط الشعر', 
            icon: 'fa-user-injured',
            short: 'نصائح للعناية بالشعر وفروة الرأس.',
            detail: 'استخدم شامبو لطيف للأطفال. تجنب الصبغات والحرارة على الشعر. قص الشعر قصيراً قبل البدء بالعلاج. احفظ كرامتك بارتداء قبعة أو وشاح أنيق. فروة الرأس قد تكون حساسة فاحمها من الشمس. الشعر عادة يعود للنمو بعد انتهاء العلاج.',
            color: 'bg-purple-100 text-purple-600'
        },
        { 
            title: 'الصحة النفسية', 
            icon: 'fa-brain',
            short: 'الحفاظ على التوازن النفسي أثناء العلاج.',
            detail: 'تحدث عن مشاعرك مع شخص تثق به. انضم لمجموعة دعم للمرضى. مارس التأمل واليوغا الخفيفة. اكتب يومياتك للتعبير عن مشاعرك. لا تتردد في طلب مساعدة نفسية متخصصة. تذكر أن المشاعر السلبية طبيعية ومؤقتة.',
            color: 'bg-indigo-100 text-indigo-600'
        },
        { 
            title: 'النوم الجيد', 
            icon: 'fa-moon',
            short: 'تحسين جودة النوم أثناء فترة العلاج.',
            detail: 'حافظ على جدول نوم منتظم. تجنب الكافيين بعد الظهر. اجعل غرفة النوم مظلمة وهادئة. تجنب الشاشات قبل النوم بساعة. استخدم وسائد مريحة لدعم الجسم. جرب تمارين الاسترخاء قبل النوم. استشر الطبيب إذا كان الألم يمنع النوم.',
            color: 'bg-slate-100 text-slate-600'
        },
        { 
            title: 'الوقاية من العدوى', 
            icon: 'fa-shield-virus',
            short: 'حماية نفسك من العدوى أثناء العلاج.',
            detail: 'اغسل يديك频繁اً بالماء والصابون. تجنب الأماكن المزدحمة. ارتدِ كمامة في الأماكن العامة. تجنب التعامل مع المرضى. حافظ على نظافة المنزل. تأكد من طهي الطعام جيداً. احصل على اللقاحات الموصى بها من طبيبك.',
            color: 'bg-teal-100 text-teal-600'
        },
        { 
            title: 'النشاط البدني', 
            icon: 'fa-person-walking',
            short: 'تمارين مناسبة خلال فترة العلاج.',
            detail: 'امشِ 15-30 دقيقة يومياً حسب طاقتك. جرب تمارين التمدد الخفيفة. تجنب التمارين الشاقة. استمع لجسمك وتوقف إذا شعرت بألم. مارس تمارين التنفس العميق. السباحة الخفيفة قد تكون مفيدة. استشر الطبيب قبل بدء أي برنامج رياضي.',
            color: 'bg-emerald-100 text-emerald-600'
        }
    ];

    // Symptom History Storage
    let symptomLogs = [
        { date: '2024-01-15', pain: 3, nausea: 2, temp: 36.5, fatigue: 4, appetite: 6, sleep: 7, mood: 6 },
        { date: '2024-01-16', pain: 4, nausea: 3, temp: 36.8, fatigue: 5, appetite: 5, sleep: 6, mood: 5 },
        { date: '2024-01-17', pain: 2, nausea: 1, temp: 36.4, fatigue: 3, appetite: 7, sleep: 8, mood: 7 },
        { date: '2024-01-18', pain: 5, nausea: 4, temp: 37.0, fatigue: 6, appetite: 4, sleep: 5, mood: 4 },
        { date: '2024-01-19', pain: 3, nausea: 2, temp: 36.6, fatigue: 4, appetite: 6, sleep: 7, mood: 6 },
    ];

    // Reports Storage
    let reports = [
        { id: 1, title: 'تحليل صورة دم كاملة', date: '2024-01-10', type: 'pdf', doctor: 'د. أحمد' },
        { id: 2, title: 'روشتة علاج كيميائي', date: '2024-01-12', type: 'image', doctor: 'د. سارة' },
        { id: 3, title: 'تقرير أشعة مقطعية', date: '2024-01-14', type: 'pdf', doctor: 'د. محمد' },
    ];

    // Patient Profile
    let patientProfile = {
        name: 'محمد أحمد',
        age: 45,
        bloodType: 'O+',
        diagnosis: 'سرطان القولون - المرحلة الثانية',
        doctor: 'د. سارة محمود',
        hospital: 'معهد الأورام القومي',
        treatmentStart: '2024-01-01',
        nextAppointment: '2024-01-25',
        phone: '01012345678',
        emergencyContact: '01234567890'
    };

    // Chart Data
    let chartData = {
        labels: ['15 يناير', '16 يناير', '17 يناير', '18 يناير', '19 يناير', '20 يناير', '21 يناير'],
         [3, 4, 2, 5, 3, 4, 2]
    };

    // --- State ---
    let currentTab = 'home';
    let chartInstance = null;

    // --- Render Functions ---

    function renderHome() {
        document.getElementById('header-subtitle').innerText = 'مرحباً بك في رحلة التعافي';
        const nextMeds = medications.filter(m => !m.taken);
        const nextMed = nextMeds.length > 0 ? nextMeds[0] : null;
        const lastLog = symptomLogs[symptomLogs.length - 1];

        return `
            <div class="fade-in space-y-6">
                <!-- Hero Image -->
                <div class="relative rounded-2xl overflow-hidden shadow-lg h-48">
                    <img src="https://image.qwenlm.ai/public_source/197dca94-2621-486b-b8ca-f890d16c32ae/160a9e912-0d8d-4065-a7b4-98b1b4ec3efb.png" alt="Resting" class="w-full h-full object-cover">
                    <div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent flex flex-col justify-end p-4">
                        <h2 class="text-white text-xl font-bold">يومك جميل وصحتك أغلى</h2>
                        <p class="text-gray-200 text-sm">استمر في رحلة التعافي، أنت قوي.</p>
                    </div>
                </div>

                <!-- Next Dose Card -->
                ${nextMed ? `
                <div class="bg-blue-50 border border-blue-100 rounded-2xl p-4 flex items-center justify-between shadow-sm">
                    <div class="flex items-center gap-4">
                        <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center text-blue-600">
                            <i class="fas fa-clock text-xl"></i>
                        </div>
                        <div>
                            <h3 class="font-bold text-gray-800">الجرعة القادمة</h3>
                            <p class="text-sm text-gray-600">${nextMed.name}</p>
                            <p class="text-xs text-blue-600 font-bold mt-1">${nextMed.time}</p>
                        </div>
                    </div>
                    <button onclick="switchTab('meds')" class="bg-blue-600 text-white px-4 py-2 rounded-xl text-sm font-bold shadow-md hover:bg-blue-700 transition">
                        تناول
                    </button>
                </div>
                ` : `
                <div class="bg-green-50 border border-green-100 rounded-2xl p-4 flex items-center gap-4 shadow-sm">
                    <div class="w-12 h-12 bg-green-100 rounded-full flex items-center justify-center text-green-600">
                        <i class="fas fa-check-circle text-xl"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-gray-800">أحسنت!</h3>
                        <p class="text-sm text-gray-600">أكملت جميع أدوية اليوم.</p>
                    </div>
                </div>
                `}

                <!-- Quick Stats -->
                <div class="grid grid-cols-2 gap-4">
                    <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 text-center" onclick="switchTab('symptoms')">
                        <div class="text-3xl font-bold text-gray-800 mb-1">${lastLog ? lastLog.temp : '36.5'}°</div>
                        <div class="text-xs text-gray-500">آخر حرارة مسجلة</div>
                    </div>
                    <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 text-center" onclick="switchTab('reports')">
                        <div class="text-3xl font-bold text-gray-800 mb-1">${reports.length}</div>
                        <div class="text-xs text-gray-500">تقارير طبية</div>
                    </div>
                </div>

                <!-- Quick Tips -->
                <div class="bg-gradient-to-r from-purple-50 to-blue-50 rounded-2xl p-4 border border-purple-100">
                    <div class="flex items-center gap-3 mb-3">
                        <div class="w-8 h-8 bg-purple-100 rounded-full flex items-center justify-center text-purple-600">
                            <i class="fas fa-lightbulb"></i>
                        </div>
                        <h3 class="font-bold text-gray-800">نصيحة اليوم</h3>
                    </div>
                    <p class="text-sm text-gray-600">${tipsData[0].short}</p>
                    <button onclick="switchTab('tips')" class="text-purple-600 text-sm font-bold mt-2">اقرأ المزيد ←</button>
                </div>
            </div>
        `;
    }

    function renderMeds() {
        document.getElementById('header-subtitle').innerText = 'التزم بالمواعيد لضمان الفعالية';
        return `
            <div class="fade-in space-y-4">
                <div class="flex items-center justify-between">
                    <div class="flex items-center gap-4">
                        <img src="https://image.qwenlm.ai/public_source/197dca94-2621-486b-b8ca-f890d16c32ae/1f501215b-c3e2-4705-aeea-bf502f2bcc13.png" class="w-16 h-16 object-contain">
                        <div>
                            <h2 class="text-2xl font-bold text-gray-800">جدول الأدوية</h2>
                            <p class="text-gray-500 text-sm">${medications.filter(m=>m.taken).length} من ${medications.length} تم تناولها</p>
                        </div>
                    </div>
                    <button onclick="toggleModal('addMedModal')" class="w-10 h-10 rounded-full bg-blue-600 text-white flex items-center justify-center shadow-lg hover:bg-blue-700 transition">
                        <i class="fas fa-plus"></i>
                    </button>
                </div>

                <div class="space-y-3">
                    ${medications.map(med => `
                        <div class="bg-white p-4 rounded-2xl shadow-sm border ${med.taken ? 'border-green-200 bg-green-50/30' : 'border-gray-100'} flex items-center justify-between transition-all duration-300">
                            <div class="flex items-center gap-4">
                                <div class="w-10 h-10 rounded-full ${med.taken ? 'bg-green-100 text-green-600' : 'bg-gray-100 text-gray-400'} flex items-center justify-center">
                                    <i class="fas ${med.icon}"></i>
                                </div>
                                <div>
                                    <h3 class="font-bold text-gray-800 ${med.taken ? 'line-through text-gray-400' : ''}">${med.name}</h3>
                                    <div class="flex gap-2 text-xs text-gray-500 mt-1">
                                        <span class="bg-gray-100 px-2 py-0.5 rounded">${med.time}</span>
                                        <span class="bg-gray-100 px-2 py-0.5 rounded">${med.dose}</span>
                                    </div>
                                </div>
                            </div>
                            <button onclick="toggleMed(${med.id})" class="w-8 h-8 rounded-full border-2 flex items-center justify-center transition-colors ${med.taken ? 'bg-green-500 border-green-500 text-white' : 'border-gray-300 text-transparent hover:border-green-500'}">
                                <i class="fas fa-check"></i>
                            </button>
                        </div>
                    `).join('')}
                </div>
            </div>
        `;
    }

    function renderSymptoms() {
        document.getElementById('header-subtitle').innerText = 'سجل يومياً لمساعدة طبيبك';
        const today = new Date().toISOString().split('T')[0];
        const todayLog = symptomLogs.find(l => l.date === today);
        const lastLog = symptomLogs[symptomLogs.length - 1] || {};

        return `
            <div class="fade-in space-y-6">
                <div class="flex items-center gap-4">
                    <img src="https://image.qwenlm.ai/public_source/197dca94-2621-486b-b8ca-f890d16c32ae/186473a6b-1df5-462d-8ab9-3bc1fcf4c630.png" class="w-16 h-16 object-contain">
                    <div>
                        <h2 class="text-2xl font-bold text-gray-800">تتبع الأعراض</h2>
                        <p class="text-gray-500 text-sm">كيف تشعر اليوم؟</p>
                    </div>
                </div>

                <!-- Input Form -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-gray-100 space-y-5">
                    <h3 class="font-bold text-gray-700 border-b pb-2">تسجيل اليوم (${today})</h3>
                    
                    ${todayLog ? `
                        <div class="bg-green-50 text-green-700 p-3 rounded-xl text-sm text-center">
                            <i class="fas fa-check-circle ml-2"></i> تم تسجيل بيانات اليوم بالفعل
                        </div>
                    ` : ''}

                    <!-- Pain -->
                    <div>
                        <div class="flex justify-between mb-2">
                            <label class="text-sm font-medium text-gray-600"><i class="fas fa-heart-crack text-red-500 ml-1"></i> مستوى الألم</label>
                            <span id="pain-val" class="text-sm font-bold text-blue-600">${todayLog ? todayLog.pain : 5}</span>
                        </div>
                        <input type="range" id="input-pain" min="1" max="10" value="${todayLog ? todayLog.pain : 5}" class="w-full" oninput="document.getElementById('pain-val').innerText = this.value">
                    </div>

                    <!-- Nausea -->
                    <div>
                        <div class="flex justify-between mb-2">
                            <label class="text-sm font-medium text-gray-600"><i class="fas fa-stomach text-yellow-500 ml-1"></i> الغثيان</label>
                            <span id="nausea-val" class="text-sm font-bold text-blue-600">${todayLog ? todayLog.nausea : 3}</span>
                        </div>
                        <input type="range" id="input-nausea" min="1" max="10" value="${todayLog ? todayLog.nausea : 3}" class="w-full" oninput="document.getElementById('nausea-val').innerText = this.value">
                    </div>

                    <!-- Fatigue -->
                    <div>
                        <div class="flex justify-between mb-2">
                            <label class="text-sm font-medium text-gray-600"><i class="fas fa-bed text-purple-500 ml-1"></i> التعب والإرهاق</label>
                            <span id="fatigue-val" class="text-sm font-bold text-blue-600">${todayLog ? todayLog.fatigue : 4}</span>
                        </div>
                        <input type="range" id="input-fatigue" min="1" max="10" value="${todayLog ? todayLog.fatigue : 4}" class="w-full" oninput="document.getElementById('fatigue-val').innerText = this.value">
                    </div>

                    <!-- Appetite -->
                    <div>
                        <div class="flex justify-between mb-2">
                            <label class="text-sm font-medium text-gray-600"><i class="fas fa-utensils text-green-500 ml-1"></i> الشهية</label>
                            <span id="appetite-val" class="text-sm font-bold text-blue-600">${todayLog ? todayLog.appetite : 6}</span>
                        </div>
                        <input type="range" id="input-appetite" min="1" max="10" value="${todayLog ? todayLog.appetite : 6}" class="w-full" oninput="document.getElementById('appetite-val').innerText = this.value">
                    </div>

                    <!-- Sleep Quality -->
                    <div>
                        <div class="flex justify-between mb-2">
                            <label class="text-sm font-medium text-gray-600"><i class="fas fa-moon text-indigo-500 ml-1"></i> جودة النوم</label>
                            <span id="sleep-val" class="text-sm font-bold text-blue-600">${todayLog ? todayLog.sleep : 7}</span>
                        </div>
                        <input type="range" id="input-sleep" min="1" max="10" value="${todayLog ? todayLog.sleep : 7}" class="w-full" oninput="document.getElementById('sleep-val').innerText = this.value">
                    </div>

                    <!-- Mood -->
                    <div>
                        <div class="flex justify-between mb-2">
                            <label class="text-sm font-medium text-gray-600"><i class="fas fa-face-smile text-pink-500 ml-1"></i> الحالة المزاجية</label>
                            <span id="mood-val" class="text-sm font-bold text-blue-600">${todayLog ? todayLog.mood : 6}</span>
                        </div>
                        <input type="range" id="input-mood" min="1" max="10" value="${todayLog ? todayLog.mood : 6}" class="w-full" oninput="document.getElementById('mood-val').innerText = this.value">
                    </div>

                    <!-- Temperature -->
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="text-sm font-medium text-gray-600 block mb-2"><i class="fas fa-thermometer-half text-red-500 ml-1"></i> درجة الحرارة</label>
                            <input type="number" id="input-temp" step="0.1" value="${todayLog ? todayLog.temp : 36.5}" class="w-full bg-gray-50 border border-gray-200 rounded-xl px-3 py-2 text-center focus:outline-none focus:ring-2 focus:ring-blue-500">
                        </div>
                        <div>
                            <label class="text-sm font-medium text-gray-600 block mb-2"><i class="fas fa-weight-scale text-blue-500 ml-1"></i> الوزن (كجم)</label>
                            <input type="number" id="input-weight" value="75" class="w-full bg-gray-50 border border-gray-200 rounded-xl px-3 py-2 text-center focus:outline-none focus:ring-2 focus:ring-blue-500">
                        </div>
                    </div>

                    <button onclick="saveSymptoms()" class="w-full bg-blue-600 text-white py-3 rounded-xl font-bold shadow-lg shadow-blue-200 hover:bg-blue-700 transition transform active:scale-95">
                        حفظ السجل
                    </button>
                </div>

                <!-- History List -->
                <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100">
                    <h3 class="font-bold text-gray-700 mb-4">سجل الأعراض السابق</h3>
                    <div class="space-y-3 max-h-60 overflow-y-auto no-scrollbar">
                        ${symptomLogs.slice().reverse().map(log => `
                            <div class="flex justify-between items-center p-3 bg-gray-50 rounded-xl">
                                <div class="text-sm font-bold text-gray-600">${log.date}</div>
                                <div class="flex gap-2 text-xs flex-wrap justify-end">
                                    <span class="bg-red-100 text-red-600 px-2 py-1 rounded">ألم: ${log.pain}</span>
                                    <span class="bg-yellow-100 text-yellow-600 px-2 py-1 rounded">غثيان: ${log.nausea}</span>
                                    <span class="bg-purple-100 text-purple-600 px-2 py-1 rounded">تعب: ${log.fatigue}</span>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                </div>

                <!-- Chart -->
                <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100">
                    <h3 class="font-bold text-gray-700 mb-4 text-center">تطور الألم خلال الأسبوع</h3>
                    <canvas id="symptomChart"></canvas>
                </div>
            </div>
        `;
    }

    function renderReports() {
        document.getElementById('header-subtitle').innerText = 'جميع مستنداتك الطبية في مكان واحد';
        return `
            <div class="fade-in space-y-4">
                <div class="flex items-center gap-4 mb-2">
                    <img src="https://image.qwenlm.ai/public_source/197dca94-2621-486b-b8ca-f890d16c32ae/10a1e5edb-dc9a-4f09-9638-dd2713c09736.png" class="w-16 h-16 object-contain">
                    <div>
                        <h2 class="text-2xl font-bold text-gray-800">ملفي الطبي</h2>
                        <p class="text-gray-500 text-sm">تحاليل، أشعة، وروشتات</p>
                    </div>
                </div>

                <!-- Upload Button -->
                <div class="border-2 border-dashed border-blue-300 rounded-2xl p-6 flex flex-col items-center justify-center bg-blue-50 text-center cursor-pointer hover:bg-blue-100 transition" onclick="triggerFileUpload()">
                    <div class="w-12 h-12 bg-blue-200 rounded-full flex items-center justify-center text-blue-600 mb-3">
                        <i class="fas fa-cloud-upload-alt text-xl"></i>
                    </div>
                    <h3 class="font-bold text-blue-800">إضافة تقرير جديد</h3>
                    <p class="text-xs text-blue-600 mt-1">صورة أو PDF (تحليل، روشتة...)</p>
                    <input type="file" id="fileInput" class="hidden" accept="image/*,.pdf" onchange="handleFileSelect(event)">
                </div>

                <!-- Files List -->
                <div class="space-y-3">
                    <h3 class="font-bold text-gray-700 px-1">المستندات المرفوعة (${reports.length})</h3>
                    ${reports.map(report => `
                        <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 flex items-center justify-between">
                            <div class="flex items-center gap-4">
                                <div class="w-12 h-12 rounded-xl ${report.type === 'pdf' ? 'bg-red-100 text-red-600' : 'bg-purple-100 text-purple-600'} flex items-center justify-center text-xl">
                                    <i class="fas ${report.type === 'pdf' ? 'fa-file-pdf' : 'fa-file-image'}"></i>
                                </div>
                                <div>
                                    <h4 class="font-bold text-gray-800 text-sm">${report.title}</h4>
                                    <div class="flex gap-2 text-xs text-gray-500 mt-1">
                                        <span>${report.date}</span>
                                        <span>•</span>
                                        <span>${report.doctor}</span>
                                    </div>
                                </div>
                            </div>
                            <button class="text-gray-400 hover:text-blue-600">
                                <i class="fas fa-ellipsis-v"></i>
                            </button>
                        </div>
                    `).join('')}
                </div>
            </div>
        `;
    }

    function renderTips() {
        document.getElementById('header-subtitle').innerText = 'نصائح للتعامل مع الآثار الجانبية';
        return `
            <div class="fade-in space-y-4">
                <div class="flex items-center gap-4 mb-2">
                    <img src="https://image.qwenlm.ai/public_source/197dca94-2621-486b-b8ca-f890d16c32ae/1659febcf-17e5-4b63-a07a-5719555dbb34.png" class="w-16 h-16 object-contain">
                    <div>
                        <h2 class="text-2xl font-bold text-gray-800">نصائح يومية</h2>
                        <p class="text-gray-500 text-sm">${tipsData.length} نصيحة لمساعدتك</p>
                    </div>
                </div>

                <!-- Search -->
                <div class="relative">
                    <input type="text" id="tipSearch" placeholder="ابحث عن نصيحة..." class="w-full bg-gray-50 border border-gray-200 rounded-xl py-3 px-4 pr-10 focus:outline-none focus:ring-2 focus:ring-blue-500" oninput="filterTips()">
                    <i class="fas fa-search absolute left-4 top-1/2 transform -translate-y-1/2 text-gray-400"></i>
                </div>

                <div class="space-y-3" id="tipsContainer">
                    ${tipsData.map((tip, index) => `
                        <div class="bg-white rounded-2xl shadow-sm border border-gray-100 overflow-hidden tip-item" data-title="${tip.title}">
                            <button onclick="toggleTip(${index})" class="w-full p-4 flex items-center justify-between text-right hover:bg-gray-50 transition">
                                <div class="flex items-center gap-3">
                                    <div class="w-10 h-10 rounded-full ${tip.color} flex items-center justify-center flex-shrink-0">
                                        <i class="fas ${tip.icon}"></i>
                                    </div>
                                    <span class="font-bold text-gray-800">${tip.title}</span>
                                </div>
                                <i id="arrow-${index}" class="fas fa-chevron-down text-gray-400 transition-transform duration-300"></i>
                            </button>
                            <div id="content-${index}" class="hidden px-4 pb-4 text-gray-600 text-sm leading-relaxed border-t border-gray-50 pt-3 bg-gray-50/50">
                                ${tip.detail}
                            </div>
                        </div>
                    `).join('')}
                </div>
            </div>
        `;
    }

    function renderProfile() {
        document.getElementById('header-subtitle').innerText = 'الملف الشخصي للمريض';
        return `
            <div class="fade-in space-y-6">
                <!-- Profile Header -->
                <div class="profile-header rounded-2xl p-6 text-white relative overflow-hidden">
                    <div class="absolute top-0 left-0 w-full h-full opacity-10">
                        <i class="fas fa-heartbeat text-9xl absolute -bottom-4 -right-4"></i>
                    </div>
                    <div class="relative z-10">
                        <div class="w-20 h-20 bg-white/20 rounded-full flex items-center justify-center text-3xl mb-4 mx-auto">
                            <i class="fas fa-user"></i>
                        </div>
                        <h2 class="text-2xl font-bold text-center">${patientProfile.name}</h2>
                        <p class="text-white/80 text-center text-sm mt-1">${patientProfile.diagnosis}</p>
                    </div>
                </div>

                <!-- Info Cards -->
                <div class="grid grid-cols-2 gap-3">
                    <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100">
                        <div class="text-gray-500 text-xs mb-1">العمر</div>
                        <div class="text-xl font-bold text-gray-800">${patientProfile.age} سنة</div>
                    </div>
                    <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100">
                        <div class="text-gray-500 text-xs mb-1">فصيلة الدم</div>
                        <div class="text-xl font-bold text-gray-800">${patientProfile.bloodType}</div>
                    </div>
                </div>

                <!-- Doctor Info -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-gray-100">
                    <h3 class="font-bold text-gray-700 mb-4 flex items-center gap-2">
                        <i class="fas fa-user-doctor text-blue-600"></i>
                        معلومات الطبيب
                    </h3>
                    <div class="space-y-3">
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 bg-blue-100 rounded-full flex items-center justify-center text-blue-600">
                                <i class="fas fa-stethoscope"></i>
                            </div>
                            <div>
                                <div class="text-sm text-gray-500">اسم الطبيب</div>
                                <div class="font-bold text-gray-800">${patientProfile.doctor}</div>
                            </div>
                        </div>
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 bg-green-100 rounded-full flex items-center justify-center text-green-600">
                                <i class="fas fa-hospital"></i>
                            </div>
                            <div>
                                <div class="text-sm text-gray-500">المستشفى</div>
                                <div class="font-bold text-gray-800">${patientProfile.hospital}</div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Treatment Info -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-gray-100">
                    <h3 class="font-bold text-gray-700 mb-4 flex items-center gap-2">
                        <i class="fas fa-calendar-alt text-purple-600"></i>
                        مواعيد العلاج
                    </h3>
                    <div class="space-y-3">
                        <div class="flex justify-between items-center p-3 bg-purple-50 rounded-xl">
                            <div>
                                <div class="text-sm text-gray-500">بداية العلاج</div>
                                <div class="font-bold text-gray-800">${patientProfile.treatmentStart}</div>
                            </div>
                            <i class="fas fa-flag-checkered text-purple-600"></i>
                        </div>
                        <div class="flex justify-between items-center p-3 bg-blue-50 rounded-xl">
                            <div>
                                <div class="text-sm text-gray-500">الموعد القادم</div>
                                <div class="font-bold text-blue-600">${patientProfile.nextAppointment}</div>
                            </div>
                            <i class="fas fa-calendar-check text-blue-600"></i>
                        </div>
                    </div>
                </div>

                <!-- Emergency Contact -->
                <div class="bg-red-50 p-5 rounded-2xl border border-red-100">
                    <h3 class="font-bold text-red-700 mb-3 flex items-center gap-2">
                        <i class="fas fa-phone-volume"></i>
                        طوارئ
                    </h3>
                    <div class="flex justify-between items-center">
                        <div>
                            <div class="text-sm text-red-600">رقم الطوارئ</div>
                            <div class="font-bold text-red-800 text-lg">${patientProfile.emergencyContact}</div>
                        </div>
                        <a href="tel:${patientProfile.emergencyContact}" class="w-12 h-12 bg-red-600 rounded-full flex items-center justify-center text-white hover:bg-red-700 transition">
                            <i class="fas fa-phone"></i>
                        </a>
                    </div>
                </div>

                <!-- Edit Button -->
                <button onclick="alert('خاصية تعديل الملف الشخصي ستكون متاحة قريباً')" class="w-full bg-gray-800 text-white py-3 rounded-xl font-bold shadow-lg hover:bg-gray-900 transition">
                    <i class="fas fa-edit ml-2"></i>
                    تعديل المعلومات
                </button>
            </div>
        `;
    }

    // --- Logic Functions ---

    function switchTab(tabName) {
        currentTab = tabName;
        const main = document.getElementById('main-content');
        
        document.querySelectorAll('.nav-item').forEach(el => {
            el.classList.remove('active', 'text-blue-600');
            el.classList.add('text-gray-400');
        });
        const activeNav = document.querySelector(`button[onclick="switchTab('${tabName}')"]`);
        if(activeNav) {
            activeNav.classList.add('active', 'text-blue-600');
            activeNav.classList.remove('text-gray-400');
        }

        if (tabName === 'home') main.innerHTML = renderHome();
        if (tabName === 'meds') main.innerHTML = renderMeds();
        if (tabName === 'symptoms') {
            main.innerHTML = renderSymptoms();
            setTimeout(initChart, 100);
        }
        if (tabName === 'reports') main.innerHTML = renderReports();
        if (tabName === 'tips') main.innerHTML = renderTips();
        if (tabName === 'profile') main.innerHTML = renderProfile();
        
        window.scrollTo(0,0);
    }

    function toggleModal(modalID) {
        const modal = document.getElementById(modalID);
        const body = document.querySelector('body');
        modal.classList.toggle('opacity-0');
        modal.classList.toggle('pointer-events-none');
        body.classList.toggle('modal-active');
    }

    function addNewMedication() {
        const name = document.getElementById('newMedName').value;
        const time = document.getElementById('newMedTime').value;
        const dose = document.getElementById('newMedDose').value;

        if(name && time) {
            medications.push({
                id: Date.now(),
                name: name,
                time: time,
                dose: dose || 'حسب الإرشاد',
                taken: false,
                icon: 'fa-prescription-bottle-alt'
            });
            toggleModal('addMedModal');
            document.getElementById('newMedName').value = '';
            document.getElementById('newMedTime').value = '';
            document.getElementById('newMedDose').value = '';
            document.getElementById('main-content').innerHTML = renderMeds();
        } else {
            alert('يرجى إدخال اسم الدواء والوقت على الأقل');
        }
    }

    function toggleMed(id) {
        const med = medications.find(m => m.id === id);
        if (med) {
            med.taken = !med.taken;
            if(currentTab === 'meds') {
                document.getElementById('main-content').innerHTML = renderMeds();
            } else if (currentTab === 'home') {
                document.getElementById('main-content').innerHTML = renderHome();
            }
        }
    }

    function toggleTip(index) {
        const content = document.getElementById(`content-${index}`);
        const arrow = document.getElementById(`arrow-${index}`);
        
        if (content.classList.contains('hidden')) {
            content.classList.remove('hidden');
            content.classList.add('fade-in');
            arrow.style.transform = 'rotate(180deg)';
        } else {
            content.classList.add('hidden');
            arrow.style.transform = 'rotate(0deg)';
        }
    }

    function filterTips() {
        const search = document.getElementById('tipSearch').value.toLowerCase();
        const tips = document.querySelectorAll('.tip-item');
        
        tips.forEach(tip => {
            const title = tip.getAttribute('data-title').toLowerCase();
            if(title.includes(search)) {
                tip.style.display = 'block';
            } else {
                tip.style.display = 'none';
            }
        });
    }

    function saveSymptoms() {
        const pain = parseInt(document.getElementById('input-pain').value);
        const nausea = parseInt(document.getElementById('input-nausea').value);
        const temp = parseFloat(document.getElementById('input-temp').value);
        const fatigue = parseInt(document.getElementById('input-fatigue').value);
        const appetite = parseInt(document.getElementById('input-appetite').value);
        const sleep = parseInt(document.getElementById('input-sleep').value);
        const mood = parseInt(document.getElementById('input-mood').value);
        const today = new Date().toISOString().split('T')[0];

        symptomLogs = symptomLogs.filter(l => l.date !== today);
        symptomLogs.push({ date: today, pain, nausea, temp, fatigue, appetite, sleep, mood });

        chartData.data.push(pain);
        chartData.labels.push('اليوم');
        if(chartData.data.length > 7) {
            chartData.data.shift();
            chartData.labels.shift();
        }

        alert('تم حفظ بيانات الأعراض بنجاح!');
        document.getElementById('main-content').innerHTML = renderSymptoms();
        setTimeout(initChart, 100);
    }

    function triggerFileUpload() {
        document.getElementById('fileInput').click();
    }

    function handleFileSelect(event) {
        const file = event.target.files[0];
        if (file) {
            const type = file.type.includes('pdf') ? 'pdf' : 'image';
            const newReport = {
                id: Date.now(),
                title: file.name,
                date: new Date().toISOString().split('T')[0],
                type: type,
                doctor: 'غير محدد'
            };
            reports.unshift(newReport);
            document.getElementById('main-content').innerHTML = renderReports();
        }
    }

    function initChart() {
        const ctx = document.getElementById('symptomChart').getContext('2d');
        
        if (chartInstance) {
            chartInstance.destroy();
        }

        chartInstance = new Chart(ctx, {
            type: 'line',
             {
                labels: chartData.labels,
                datasets: [{
                    label: 'مستوى الألم',
                     chartData.data,
                    borderColor: '#3b82f6',
                    backgroundColor: 'rgba(59, 130, 246, 0.1)',
                    tension: 0.4,
                    fill: true,
                    pointBackgroundColor: '#ffffff',
                    pointBorderColor: '#3b82f6',
                    pointBorderWidth: 2,
                    pointRadius: 4
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: { display: false }
                },
                scales: {
                    y: { beginAtZero: true, max: 10, grid: { color: '#f3f4f6' } },
                    x: { grid: { display: false } }
                }
            }
        });
    }

    // Initialize App
    switchTab('home');

</script>
</body>
</html>

