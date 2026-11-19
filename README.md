```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>بوابة الدفع الإلكتروني | أبو عبد الرحمن للعدد والحدايد</title>
    <!-- خط Cairo العربي الحديث -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- أيقونات Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <!-- مكتبة QRCode.js لتوليد رمز استجابة سريعة حقيقي وقابل للمسح -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        cairo: ['Cairo', 'sans-serif'],
                    }
                }
            }
        }
    </script>
</head>
<body class="font-cairo bg-slate-950 text-slate-100 min-h-screen flex flex-col items-center justify-center p-4 sm:p-6 selection:bg-amber-400 selection:text-slate-950 relative overflow-x-hidden">

    <!-- إضاءات خلفية خفيفة لإبراز البطاقة المركزية -->
    <div class="fixed inset-0 pointer-events-none overflow-hidden">
        <div class="absolute -top-32 -right-32 w-80 h-80 rounded-full bg-[#ff7900]/15 blur-3xl"></div>
        <div class="absolute -bottom-32 -left-32 w-80 h-80 rounded-full bg-[#e60000]/15 blur-3xl"></div>
    </div>

    <!-- الحاوية الرئيسية المركزية (Mobile-First Payment Card) -->
    <main class="w-full max-w-lg relative z-10 my-auto">
        <div class="bg-slate-900/95 backdrop-blur-md border border-slate-800 rounded-3xl shadow-2xl overflow-hidden">
            
            <!-- 1. رأس البطاقة: الشعار وهوية المتجر -->
            <header class="bg-gradient-to-b from-slate-800/90 to-slate-900 px-6 pt-7 pb-6 text-center border-b border-slate-800">
                <div class="w-16 h-16 rounded-2xl bg-amber-500 text-slate-950 flex items-center justify-center text-3xl mx-auto mb-3.5 shadow-lg shadow-amber-500/20">
                    <i class="fa-solid fa-screwdriver-wrench"></i>
                </div>
                <h1 class="text-2xl sm:text-3xl font-black text-white tracking-tight">
                    أبو عبد الرحمن للعدد والحدايد
                </h1>
                <div class="inline-flex items-center gap-2 bg-slate-800 border border-slate-700 text-amber-400 text-xs sm:text-sm font-bold px-4 py-1.5 rounded-full mt-2.5">
                    <i class="fa-solid fa-bolt"></i>
                    <span>بوابة الدفع والتحويل الإلكتروني السريع</span>
                </div>
            </header>

            <!-- جسم البطاقة -->
            <div class="p-5 sm:p-7 space-y-6">

                <!-- 2. قسم رمز الاستجابة السريعة (QR Code) الجاهز لأورنج كاش -->
                <section aria-label="رمز QR أورنج كاش" class="bg-white text-slate-900 rounded-3xl p-5 sm:p-6 border-4 border-[#ff7900] shadow-xl">
                    <div class="text-center mb-4">
                        <span class="inline-flex items-center gap-1.5 bg-[#ff7900] text-white text-xs font-black px-3.5 py-1 rounded-full mb-2">
                            <i class="fa-solid fa-qrcode"></i>
                            <span>QR جاهز لأورنج كاش</span>
                        </span>
                        <h2 class="text-lg sm:text-xl font-black text-slate-900">امسح الكود لإرسال النقود فوراً</h2>
                        <p class="text-xs text-slate-500 mt-0.5">
                            متوافق مع تطبيق <strong class="text-[#ff7900]">Orange Cash / My Orange</strong>
                        </p>
                    </div>

                    <!-- حاوية رمز الـ QR الفعلي -->
                    <div class="bg-slate-50 border-2 border-dashed border-[#ff7900]/70 rounded-2xl p-4 flex flex-col items-center justify-center mx-auto max-w-[230px]">
                        <div id="orangeQrCode" class="bg-white p-2 rounded-xl shadow-xs flex items-center justify-center"></div>
                        <div class="mt-2.5 flex items-center gap-1.5 text-xs font-black text-[#ff7900] font-mono tracking-wider" dir="ltr">
                            <span>01282102022</span>
                        </div>
                    </div>

                    <!-- حالة المبلغ المحدد في الـ QR -->
                    <p id="qrAmountLabel" class="text-center text-xs font-extrabold text-slate-700 mt-3">
                        الرمز مفتوح لأي مبلغ • التحويل إلى <span dir="ltr">01282102022</span>
                    </p>

                    <!-- حقل إدخال المبلغ اختياري + أزرار مبالغ سريعة -->
                    <div class="mt-4 pt-4 border-t border-slate-200 space-y-3">
                        <div class="flex items-center justify-between">
                            <label for="amountInput" class="text-xs font-extrabold text-slate-700">تحديد المبلغ (اختياري):</label>
                            <button type="button" onclick="clearAmount()" class="text-[11px] font-bold text-slate-500 hover:text-[#ff7900] underline">
                                مسح المبلغ (مفتوح)
                            </button>
                        </div>

                        <div class="relative">
                            <input 
                                type="number" 
                                id="amountInput" 
                                min="1" 
                                step="any"
                                placeholder="اكتب المبلغ بالجنيه المصري..." 
                                oninput="updateOrangeQR(this.value)"
                                class="w-full bg-slate-100 border border-slate-300 focus:border-[#ff7900] focus:bg-white rounded-xl py-2.5 pr-4 pl-14 text-sm font-black text-slate-900 focus:outline-none transition-colors"
                            >
                            <span class="absolute left-3.5 top-1/2 -translate-y-1/2 text-xs font-extrabold text-slate-500">ج.م</span>
                        </div>

                        <!-- أزرار المبالغ السريعة -->
                        <div class="grid grid-cols-4 gap-2">
                            <button type="button" onclick="setQuickAmount(100)" class="bg-slate-100 hover:bg-[#ff7900] hover:text-white text-slate-800 font-extrabold py-2 rounded-xl text-xs transition-colors active:scale-95">
                                100 ج.م
                            </button>
                            <button type="button" onclick="setQuickAmount(250)" class="bg-slate-100 hover:bg-[#ff7900] hover:text-white text-slate-800 font-extrabold py-2 rounded-xl text-xs transition-colors active:scale-95">
                                250 ج.م
                            </button>
                            <button type="button" onclick="setQuickAmount(500)" class="bg-slate-100 hover:bg-[#ff7900] hover:text-white text-slate-800 font-extrabold py-2 rounded-xl text-xs transition-colors active:scale-95">
                                500 ج.م
                            </button>
                            <button type="button" onclick="setQuickAmount(1000)" class="bg-slate-100 hover:bg-[#ff7900] hover:text-white text-slate-800 font-extrabold py-2 rounded-xl text-xs transition-colors active:scale-95">
                                1000 ج.م
                            </button>
                        </div>

                        <!-- أزرار تحميل وحفظ QR أو مشاركته -->
                        <div class="grid grid-cols-2 gap-2.5 pt-1">
                            <button type="button" onclick="downloadQRCode()" class="bg-slate-900 hover:bg-slate-800 text-white font-bold py-2.5 px-3 rounded-xl text-xs flex items-center justify-center gap-2 transition-all active:scale-95">
                                <i class="fa-solid fa-download text-[#ff7900]"></i>
                                <span>حفظ صورة QR</span>
                            </button>
                            <button type="button" onclick="sharePaymentInfo()" class="bg-[#ff7900]/15 hover:bg-[#ff7900]/25 text-[#d96600] font-extrabold py-2.5 px-3 rounded-xl text-xs flex items-center justify-center gap-2 transition-all active:scale-95">
                                <i class="fa-solid fa-share-nodes"></i>
                                <span>مشاركة البيانات</span>
                            </button>
                        </div>
                    </div>
                </section>

                <!-- 3. بطاقات المحافظ الإلكترونية برسم أيقونات SVG مخصصة -->
                <section aria-label="أرقام المحافظ الإلكترونية" class="space-y-4">
                    <div class="flex items-center justify-between px-1">
                        <h3 class="text-sm font-extrabold text-slate-300">أو انسخ رقم المحفظة للتحويل اليدوي:</h3>
                        <span class="text-[11px] text-slate-400">اضغط للنسخ الفوري</span>
                    </div>

                    <!-- بطاقة أورنج كاش (Orange Cash) -->
                    <div class="bg-gradient-to-l from-[#ff7900]/20 via-slate-800/90 to-slate-800 border-2 border-[#ff7900] rounded-2xl p-4 sm:p-5 shadow-lg">
                        <div class="flex items-center justify-between gap-3">
                            <div class="flex items-center gap-3.5 min-w-0">
                                <!-- رسم أيقونة SVG عالية الدقة لأورنج كاش -->
                                <div class="w-16 h-16 rounded-2xl bg-[#ff7900] flex flex-col items-center justify-center shrink-0 shadow-md border border-white/25 p-1.5">
                                    <svg viewBox="0 0 64 64" class="w-9 h-9" fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                                        <rect x="4" y="4" width="56" height="56" rx="12" fill="#FF7900"/>
                                        <rect x="14" y="43" width="36" height="6" rx="2" fill="#FFFFFF"/>
                                        <circle cx="32" cy="24" r="10" stroke="#FFFFFF" stroke-width="4.5"/>
                                        <path d="M28 24L31 27L37 21" stroke="#FFFFFF" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
                                    </svg>
                                    <span class="text-[9px] font-black text-white leading-none mt-0.5 tracking-tight">orange cash</span>
                                </div>

                                <div class="min-w-0">
                                    <span class="inline-block bg-[#ff7900] text-white text-[11px] font-extrabold px-2.5 py-0.5 rounded-md mb-1">
                                        أورنج كاش
                                    </span>
                                    <div class="text-xl sm:text-2xl font-black text-white font-mono tracking-wider select-all" dir="ltr">
                                        01282102022
                                    </div>
                                </div>
                            </div>

                            <!-- أزرار النسخ والاتصال السريع -->
                            <div class="flex flex-col gap-2 shrink-0">
                                <button 
                                    type="button" 
                                    onclick="copyWalletNumber('01282102022', 'أورنج كاش', this)" 
                                    class="bg-[#ff7900] hover:bg-[#e56d00] text-white font-extrabold px-4 py-2.5 rounded-xl text-xs sm:text-sm flex items-center justify-center gap-1.5 shadow-md transition-all active:scale-95"
                                >
                                    <i class="fa-regular fa-copy"></i>
                                    <span>نسخ</span>
                                </button>
                                <a 
                                    href="tel:*7115#" 
                                    class="bg-slate-700 hover:bg-slate-600 text-slate-200 font-bold px-3 py-1.5 rounded-xl text-[11px] flex items-center justify-center gap-1 transition-colors"
                                    title="كود محفظة أورنج كاش *7115#"
                                >
                                    <i class="fa-solid fa-phone text-[#ff7900]"></i>
                                    <span dir="ltr">*7115#</span>
                                </a>
                            </div>
                        </div>
                    </div>

                    <!-- بطاقة فودافون كاش (Vodafone Cash) -->
                    <div class="bg-gradient-to-l from-[#e60000]/20 via-slate-800/90 to-slate-800 border-2 border-[#e60000] rounded-2xl p-4 sm:p-5 shadow-lg">
                        <div class="flex items-center justify-between gap-3">
                            <div class="flex items-center gap-3.5 min-w-0">
                                <!-- رسم أيقونة SVG عالية الدقة لفودافون كاش -->
                                <div class="w-16 h-16 rounded-2xl bg-[#e60000] flex flex-col items-center justify-center shrink-0 shadow-md border border-white/25 p-1.5">
                                    <svg viewBox="0 0 64 64" class="w-9 h-9" fill="none" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                                        <circle cx="32" cy="32" r="26" fill="#FFFFFF"/>
                                        <path d="M36.5 14.5C26.5 14.5 19 21.8 19 31.8C19 41.8 26.6 49 35.8 49C43.3 49 48.5 43.7 48.5 37C48.5 31 44 26.8 38.3 26.8C36 26.8 34 27.6 32.5 28.9C32.6 23 36.4 18.5 41.8 17L36.5 14.5Z" fill="#E60000"/>
                                    </svg>
                                    <span class="text-[8.5px] font-black text-white leading-none mt-0.5 tracking-tight">vodafone cash</span>
                                </div>

                                <div class="min-w-0">
                                    <span class="inline-block bg-[#e60000] text-white text-[11px] font-extrabold px-2.5 py-0.5 rounded-md mb-1">
                                        فودافون كاش
                                    </span>
                                    <div class="text-xl sm:text-2xl font-black text-white font-mono tracking-wider select-all" dir="ltr">
                                        01282102022
                                    </div>
                                </div>
                            </div>

                            <!-- أزرار النسخ والاتصال السريع -->
                            <div class="flex flex-col gap-2 shrink-0">
                                <button 
                                    type="button" 
                                    onclick="copyWalletNumber('01282102022', 'فودافون كاش', this)" 
                                    class="bg-[#e60000] hover:bg-[#cc0000] text-white font-extrabold px-4 py-2.5 rounded-xl text-xs sm:text-sm flex items-center justify-center gap-1.5 shadow-md transition-all active:scale-95"
                                >
                                    <i class="fa-regular fa-copy"></i>
                                    <span>نسخ</span>
                                </button>
                                <a 
                                    href="tel:*9*7#" 
                                    class="bg-slate-700 hover:bg-slate-600 text-slate-200 font-bold px-3 py-1.5 rounded-xl text-[11px] flex items-center justify-center gap-1 transition-colors"
                                    title="كود تحويل فودافون كاش *9*7#"
                                >
                                    <i class="fa-solid fa-phone text-[#e60000]"></i>
                                    <span dir="ltr">*9*7#</span>
                                </a>
                            </div>
                        </div>
                    </div>
                </section>

            </div>

            <!-- 4. رسالة الشكر الختامية البارزة -->
            <footer class="bg-slate-950/90 border-t border-slate-800 px-6 py-6 text-center space-y-2">
                <div class="inline-flex items-center justify-center gap-2 bg-amber-500/10 border border-amber-500/30 text-amber-400 px-5 py-2 rounded-2xl">
                    <i class="fa-solid fa-heart text-rose-500"></i>
                    <span class="text-base sm:text-lg font-black">أتمنى لكم الشكر</span>
                </div>
                <p class="text-xs sm:text-sm text-slate-400 font-semibold">
                    مع أطيب التمنيات لكم بالتوفيق والبركة — <strong class="text-white">أبو عبد الرحمن للعدد والحدايد</strong>
                </p>
            </footer>

        </div>
    </main>

    <!-- إشعار منبثق (Toast Notification) -->
    <div id="toastBox" class="fixed bottom-5 left-1/2 -translate-x-1/2 z-50 transform translate-y-24 opacity-0 transition-all duration-300 pointer-events-none w-max max-w-[90vw]">
        <div class="bg-slate-800 text-white border border-amber-500/50 px-5 py-3 rounded-2xl shadow-2xl flex items-center gap-2.5">
            <i class="fa-solid fa-circle-check text-emerald-400 text-lg"></i>
            <span id="toastText" class="text-sm font-extrabold">تم النسخ!</span>
        </div>
    </div>

    <script>
        const WALLET_NUMBER = "01282102022";
        const MERCHANT_NAME = "أبو عبد الرحمن للعدد والحدايد";

        // توليد وتحديث رمز QR الخاص بأورنج كاش
        function updateOrangeQR(amountValue = "") {
            const qrContainer = document.getElementById("orangeQrCode");
            const amountLabel = document.getElementById("qrAmountLabel");
            if (!qrContainer) return;

            qrContainer.innerHTML = "";
            const numericAmount = parseFloat(amountValue);
            const hasValidAmount = !isNaN(numericAmount) && numericAmount > 0;

            // بناء محتوى الـ QR بصيغة واضحة وقابلة للمسح السريع
            const qrData = hasValidAmount
                ? `ORANGE_CASH|TO:${WALLET_NUMBER}|AMOUNT:${numericAmount}EGP|NAME:ABU_ABDELRAHMAN`
                : `ORANGE_CASH|TO:${WALLET_NUMBER}|NAME:ABU_ABDELRAHMAN`;

            new QRCode(qrContainer, {
                text: qrData,
                width: 175,
                height: 175,
                colorDark: "#0f172a",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });

            if (amountLabel) {
                if (hasValidAmount) {
                    amountLabel.innerHTML = `المبلغ المحدد في الرمز: <span class="text-[#ff7900] text-sm font-black">${numericAmount.toLocaleString("ar-EG")} ج.م</span>`;
                } else {
                    amountLabel.innerHTML = `الرمز مفتوح لأي مبلغ • التحويل إلى <span dir="ltr">${WALLET_NUMBER}</span>`;
                }
            }
        }

        // تحديد مبلغ سريع من الأزرار
        function setQuickAmount(amount) {
            const input = document.getElementById("amountInput");
            input.value = amount;
            updateOrangeQR(amount);
            showToast(`تم تحديث رمز QR بمبلغ ${amount} ج.م`);
        }

        // مسح المبلغ ليكون الرمز مفتوحاً
        function clearAmount() {
            const input = document.getElementById("amountInput");
            input.value = "";
            updateOrangeQR("");
            showToast("تم إعادة ضبط رمز QR ليكون مفتوحاً لأي مبلغ");
        }

        // نسخ رقم المحفظة مع تنبيه "تم النسخ!"
        function copyWalletNumber(number, walletTitle, btnElement) {
            const tempInput = document.createElement("input");
            tempInput.value = number;
            document.body.appendChild(tempInput);
            tempInput.select();
            tempInput.setSelectionRange(0, 99999);

            try {
                document.execCommand("copy");
                showToast(`تم النسخ! رقم ${walletTitle}: ${number}`);
