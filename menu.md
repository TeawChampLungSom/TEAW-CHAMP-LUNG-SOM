<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>เตี๋ยวแชมป์ลุงสม - Smart Ordering System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Kanit', sans-serif; transition: background-color 0.3s, color 0.3s; }
        body.dark-mode { background-color: #121212; color: #f3f4f6; }
        body.dark-mode .card-bg { background-color: #1e1e1e; border-color: #2e2e2e; }
        body.dark-mode .header-bg { background-color: #181818; border-color: #333; }
        body.dark-mode .item-bg { background-color: #2a2a2a; border-color: #3d3d3d; }

        body.light-mode { background-color: #ffffff; color: #1a1a1a; }
        body.light-mode .card-bg { background-color: #ffffff; border-color: #e5e7eb; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
        body.light-mode .header-bg { background-color: #ffffff; border-color: #e5e7eb; }
        body.light-mode .item-bg { background-color: #f8f9fa; border-color: #e5e7eb; }

        .gold-btn { background: linear-gradient(135deg, #d97706 0%, #b45309 100%); color: white; }
    </style>
</head>
<body class="light-mode max-w-md mx-auto min-h-screen pb-24 relative border-x border-neutral-200 dark:border-neutral-700/30">

    <!-- 🚨 CUSTOM ALERT MODAL (แจ้งเตือนกลางจอภาษาของลูกค้า) -->
    <div id="alertModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="card-bg border-2 border-amber-500 w-full max-w-xs p-5 rounded-2xl text-center space-y-3 shadow-2xl animate-bounce-short">
            <div class="w-12 h-12 bg-amber-500/20 text-amber-500 rounded-full flex items-center justify-center mx-auto text-2xl">⚠️</div>
            <h3 class="font-bold text-amber-500 text-sm" id="alertTitle">Notice / แจ้งเตือน</h3>
            <p class="text-xs text-neutral-600 dark:text-neutral-300 leading-relaxed" id="alertMsg">Please complete your order options.</p>
            <button onclick="closeAlertModal()" class="w-full py-2.5 gold-btn font-bold text-xs rounded-xl active:scale-95">
                OK / ตกลง
            </button>
        </div>
    </div>

    <!-- 🍜 MODAL เพิ่มสินค้าลงตะกร้าสำเร็จ (แจ้งเตือนถามลูกค้าเป็นภาษาที่เลือก) -->
    <div id="addToCartSuccessModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="card-bg border-2 border-amber-500 w-full max-w-xs p-5 rounded-2xl text-center space-y-3 shadow-2xl">
            <div class="w-12 h-12 bg-emerald-500/20 text-emerald-500 rounded-full flex items-center justify-center mx-auto text-2xl">✅</div>
            <h3 class="font-bold text-amber-600 text-sm" id="cartModalTitle">Added to Cart!</h3>
            <p class="text-xs font-semibold text-amber-500 bg-amber-500/10 p-2 rounded-lg border border-amber-500/20" id="cartModalItemName">-</p>
            <p class="text-xs text-neutral-600 dark:text-neutral-300 leading-relaxed" id="cartModalMsg">Would you like to order more or proceed to checkout?</p>
            <div class="grid grid-cols-2 gap-2 pt-2">
                <button onclick="closeCartSuccessModal()" class="py-2.5 bg-neutral-200 dark:bg-neutral-700 text-neutral-800 dark:text-white font-bold text-xs rounded-xl active:scale-95" id="btn_continue_order">
                    Order More
                </button>
                <button onclick="goToCheckoutFromModal()" class="py-2.5 gold-btn font-bold text-xs rounded-xl active:scale-95" id="btn_go_checkout">
                    Checkout Now
                </button>
            </div>
        </div>
    </div>

    <!-- 1. WELCOME SCREEN (ปรับพื้นหลังเป็นสีขาว) -->
    <div id="welcomeScreen" class="fixed inset-0 z-50 flex flex-col items-center justify-center p-6 text-center max-w-md mx-auto bg-white text-neutral-800">
        <div class="w-24 h-24 mb-4 rounded-full overflow-hidden border-2 border-amber-400 shadow-xl shadow-amber-500/20 flex items-center justify-center bg-amber-50">
            <img src="imming1.png" alt="เตี๋ยวแชมป์ลุงสม" class="w-full h-full object-cover">
        </div>
        <h1 class="text-2xl font-bold text-amber-600">TEAW CHAMP LUNG SOM</h1>
        <p class="text-xs text-neutral-500 mb-6">เตี๋ยวแชมป์ลุงสม</p>

        <div class="w-full bg-neutral-50 p-5 rounded-2xl border border-amber-500/30 space-y-4 shadow-sm">
            <p class="text-xs font-semibold text-amber-600">
                🌐 Please Select Your Language<br>
                <span class="text-[10px] text-neutral-500 font-normal">กรุณาเลือกภาษาของคุณ</span>
            </p>

            <div class="grid grid-cols-2 gap-2 text-xs text-left">
                <button onclick="selectLanguage('en')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/gb.png" class="w-5 h-3.5 rounded object-cover shadow"> English</button>
                <button onclick="selectLanguage('zh')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/cn.png" class="w-5 h-3.5 rounded object-cover shadow"> 中文</button>
		<button onclick="selectLanguage('ko')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/kr.png" class="w-5 h-3.5 rounded object-cover shadow"> 한국어</button>
                <button onclick="selectLanguage('ja')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/jp.png" class="w-5 h-3.5 rounded object-cover shadow"> 日本語</button>
                <button onclick="selectLanguage('ru')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/ru.png" class="w-5 h-3.5 rounded object-cover shadow"> Русский</button>
                <button onclick="selectLanguage('ms')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/my.png" class="w-5 h-3.5 rounded object-cover shadow"> B. Melayu</button>
                <button onclick="selectLanguage('vi')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/vn.png" class="w-5 h-3.5 rounded object-cover shadow"> T. Việt</button>
                <button onclick="selectLanguage('my')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/mm.png" class="w-5 h-3.5 rounded object-cover shadow"> မြန်မာ</button>
		<button onclick="selectLanguage('lo')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm">
    <img src="https://flagcdn.com/w40/la.png" class="w-5 h-3.5 rounded object-cover shadow"> ພາສາລາວ
</button>
                <button onclick="selectLanguage('th')" class="p-3 bg-white hover:bg-neutral-100 text-neutral-800 border border-neutral-200 rounded-xl flex items-center gap-2.5 shadow-sm"><img src="https://flagcdn.com/w40/th.png" class="w-5 h-3.5 rounded object-cover shadow"> ภาษาไทย</button>
            </div>
        </div>
    </div>

    <!-- HEADER -->
    <header class="p-4 header-bg border-b sticky top-0 z-40 flex items-center justify-between">
        <div class="flex items-center gap-2.5">
            <div class="w-9 h-9 rounded-full overflow-hidden border border-amber-500/50 flex-shrink-0 bg-black">
                <img src="imming1.png" alt="Logo" class="w-full h-full object-cover">
            </div>
            <div>
                <h1 class="text-base font-bold text-amber-600">TEAW CHAMP LUNG SOM</h1>
                <p class="text-[10px] opacity-70">เตี๋ยวแชมป์ลุงสม</p>
            </div>
        </div>
        <div class="flex items-center gap-2">
            <button onclick="toggleTheme()" class="px-2.5 py-1 bg-neutral-200 dark:bg-neutral-800 rounded-full text-xs font-bold border border-amber-500/30">
                <span id="themeIcon">☀️</span>
            </button>
            <button onclick="openLanguageScreen()" class="px-3 py-1 bg-neutral-200 dark:bg-neutral-800 rounded-full text-xs text-amber-600 flex items-center gap-1.5 font-bold border border-amber-500/20 active:scale-95">
                <img id="currentFlagImg" src="https://flagcdn.com/w40/gb.png" class="w-4 h-3 rounded object-cover">
                <span id="currentLangName">EN</span>
            </button>
        </div>
    </header>

    <!-- MAIN FORM -->
    <main id="orderForm" class="p-4 space-y-4">

        <!-- 1. เลือกซุป -->
        <section class="card-bg p-4 rounded-2xl border">
            <div class="flex justify-between items-center mb-1">
                <h2 class="text-amber-600 font-bold text-sm flex items-center gap-2">
                    <span class="w-5 h-5 gold-btn rounded-full text-xs font-bold flex items-center justify-center">1</span>
                    <span id="label_soup">Select Soup Base</span>
                </h2>
                <span class="text-[10px] text-red-500 font-semibold">*Required</span>
            </div>
            <p class="text-[11px] text-amber-500 font-medium mb-3" id="hint_soup">⚠️ Please select 1 option</p>
            
            <div class="grid grid-cols-2 gap-2 text-xs">
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="soup" value="น้ำใส" onclick="handleSoupChange(false)" class="accent-amber-500 mr-2"><span id="soup_1">Clear Soup</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="soup" value="น้ำตก" onclick="handleSoupChange(false)" class="accent-amber-500 mr-2"><span id="soup_2">Thick Soup</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="soup" value="แห้ง" onclick="handleSoupChange(false)" class="accent-amber-500 mr-2"><span id="soup_3">Dry Noodles</span></label>
                <label class="p-2.5 rounded-xl item-bg border border-amber-500/50 flex items-center cursor-pointer"><input type="radio" name="soup" value="เกาเหลา" onclick="handleSoupChange(true)" class="accent-amber-500 mr-2"><span id="soup_4" class="text-amber-600 font-bold">Soup Only (No Noodles)</span></label>
            </div>
        </section>

        <!-- 2. เลือกเส้น -->
        <section id="noodleSection" class="card-bg p-4 rounded-2xl border">
            <div class="flex justify-between items-center mb-1">
                <h2 class="text-amber-600 font-bold text-sm flex items-center gap-2">
                    <span class="w-5 h-5 gold-btn rounded-full text-xs font-bold flex items-center justify-center">2</span>
                    <span id="label_noodle">Select Noodles</span>
                </h2>
                <span class="text-[10px] text-red-500 font-semibold">*Required</span>
            </div>
            <p class="text-[11px] text-amber-500 font-medium mb-3" id="hint_noodle">⚠️ Please select 1 option</p>

            <div class="grid grid-cols-2 gap-2 text-xs">
                <label class="p-2.5 rounded-xl item-bg border flex items-center"><input type="radio" name="noodle" value="เส้นเล็ก" class="accent-amber-500 mr-2"><span id="nd_1">Thin Rice</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center"><input type="radio" name="noodle" value="เส้นหมี่" class="accent-amber-500 mr-2"><span id="nd_2">Vermicelli</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center"><input type="radio" name="noodle" value="เส้นใหญ่" class="accent-amber-500 mr-2"><span id="nd_3">Flat Rice</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center"><input type="radio" name="noodle" value="บะหมี่" class="accent-amber-500 mr-2"><span id="nd_4">Egg Noodles</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center"><input type="radio" name="noodle" value="วุ้นเส้น" class="accent-amber-500 mr-2"><span id="nd_5">Glass Noodles</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center"><input type="radio" name="noodle" value="มาม่า" class="accent-amber-500 mr-2"><span id="nd_6">Instant</span></label>
            </div>
        </section>

        <!-- 3. ผักและข้อห้าม -->
        <section class="card-bg p-4 rounded-2xl border">
            <h2 class="text-amber-600 font-bold mb-1 text-sm flex items-center gap-2">
                <span class="w-5 h-5 gold-btn rounded-full text-xs font-bold flex items-center justify-center">3</span>
                <span id="label_veg">Veggies & Exclusions</span>
            </h2>
            <p class="text-[11px] text-gray-400 mb-3" id="hint_veg">Optional customization</p>

            <div class="space-y-2 text-xs">
                <div class="grid grid-cols-2 gap-2">
                    <label class="p-2 rounded-lg item-bg border flex items-center"><input type="checkbox" id="veg_spinach" class="accent-amber-500 mr-2"><span id="txt_spinach">Water Spinach</span></label>
                    <label class="p-2 rounded-lg item-bg border flex items-center"><input type="checkbox" id="veg_sprouts" class="accent-amber-500 mr-2"><span id="txt_sprouts">Bean Sprouts</span></label>
                </div>
                <hr class="my-2 opacity-30">
                <p class="text-[11px] text-red-500 font-bold" id="txt_no_title">🚫 Do NOT add:</p>
                <div class="space-y-1.5 opacity-90">
                    <label class="flex items-center"><input type="checkbox" id="no_garlic" class="accent-red-500 mr-2"><span id="txt_no_garlic">No Garlic Oil</span></label>
                    <label class="flex items-center"><input type="checkbox" id="no_coriander" class="accent-red-500 mr-2"><span id="txt_no_coriander">No Coriander</span></label>
                    <label class="flex items-center"><input type="checkbox" id="no_pepper" class="accent-red-500 mr-2"><span id="txt_no_pepper">No Pepper</span></label>
                </div>
            </div>
        </section>

        <!-- 4. ท็อปปิ้ง -->
        <section class="card-bg p-4 rounded-2xl border">
            <div class="flex justify-between items-center mb-1">
                <h2 class="text-amber-600 font-bold text-sm flex items-center gap-2">
                    <span class="w-5 h-5 gold-btn rounded-full text-xs font-bold flex items-center justify-center">4</span>
                    <span id="label_topping">Select Topping</span>
                </h2>
                <span class="text-[10px] text-red-500 font-semibold">*Required</span>
            </div>
            <p class="text-[11px] text-amber-500 font-medium mb-3" id="hint_topping">⚠️ Please select 1 option</p>

            <div class="space-y-2 text-xs">
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="ลูกชิ้นหมู" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_1">Pork Balls</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="ลูกชิ้นเนื้อ" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_2">Beef Balls</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="ลูกชิ้นเนื้อเอ็น" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_3">Tendon Beef Balls</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="หมูหมัก" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_4">Marinated Pork</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="ลูกชิ้นหมู+หมูหมัก" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_5">Pork Balls + Pork</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="ลูกชิ้นเนื้อ+หมูหมัก" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_6">Beef Balls + Pork</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="ลูกชิ้นเอ็นเนื้อ+หมูหมัก" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_8">Tendon Beef Balls + Pork</span></label>
                <label class="p-2.5 rounded-xl item-bg border flex items-center cursor-pointer"><input type="radio" name="topping" value="ลูกชิ้นเนื้อรวม+หมูหมัก" onclick="toggleAllMixLock(false)" class="accent-amber-500 mr-2"><span id="top_9">Mixed Beef Balls + Pork</span></label>
                <label class="p-2.5 rounded-xl item-bg border border-amber-500/50 flex items-center cursor-pointer"><input type="radio" name="topping" value="รวมทุกอย่าง" onclick="toggleAllMixLock(true)" class="accent-amber-500 mr-2"><span id="top_7" class="text-amber-600 font-bold">All Mix (50฿)</span></label>
            </div>

            <div class="mt-4 pt-3 border-t">
                <label class="text-[11px] opacity-70 block mb-2" id="label_size">Size</label>
                <div class="grid grid-cols-2 gap-2 text-xs">
                    <label id="lbl_sz_reg" class="p-2.5 rounded-xl item-bg border text-center cursor-pointer"><input type="radio" id="radio_sz_reg" name="size" value="40" class="accent-amber-500 mr-1"><span id="sz_reg">Regular (40฿)</span></label>
                    <label id="lbl_sz_ext" class="p-2.5 rounded-xl item-bg border text-center cursor-pointer"><input type="radio" id="radio_sz_ext" name="size" value="50" class="accent-amber-500 mr-1"><span id="sz_ext">Extra (50฿)</span></label>
                </div>
            </div>

            <button onclick="addNoodleToCart()" class="w-full mt-4 py-3 gold-btn font-bold text-xs rounded-xl shadow active:scale-95 flex items-center justify-center gap-2">
                <span>🛒</span> <span id="btn_add_bowl">Add Bowl to Cart</span>
            </button>
        </section>

    </main>

    <!-- FLOATING CART BAR -->
    <div id="cartBar" class="fixed bottom-0 left-0 right-0 max-w-md mx-auto p-4 header-bg border-t z-30 shadow-2xl">
        <div class="flex items-center justify-between">
            <div class="flex items-center gap-2">
                <span class="text-2xl">🛒</span>
                <div>
                    <p class="text-[10px] opacity-70" id="lbl_cart_title">Your Cart</p>
                    <p class="text-xs font-bold text-amber-600"><span id="cart_count">0</span> <span id="lbl_items">Items</span> | Total: <span id="cart_total" class="text-base font-black">0</span> ฿</p>
                </div>
            </div>
            <button onclick="openCheckoutModal()" class="px-5 py-2.5 gold-btn font-black text-xs rounded-xl shadow active:scale-95">
                <span id="btn_checkout">Checkout</span> →
            </button>
        </div>
    </div>

    <!-- MODAL CHECKOUT -->
    <div id="checkoutModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 hidden flex items-end justify-center">
        <div class="card-bg border-t-2 border-amber-500 w-full max-w-md p-6 rounded-t-3xl space-y-4 max-h-[85vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b pb-2">
                <h3 class="text-base font-bold text-amber-600" id="modal_title">Order Options</h3>
                <button onclick="closeCheckoutModal()" class="text-xl font-bold">✕</button>
            </div>

            <div id="modalCartItems" class="space-y-2 text-xs max-h-32 overflow-y-auto item-bg p-3 rounded-xl border"></div>

            <div>
                <label class="text-xs opacity-70 block mb-1" id="lbl_dine_mod">Option</label>
                <div class="grid grid-cols-2 gap-2 text-xs">
                    <label class="p-2.5 rounded-xl item-bg border text-center cursor-pointer"><input type="radio" name="dine" value="ทานที่ร้าน (ใส่ถ้วย)" checked class="accent-amber-500 mr-1"><span id="opt_in">Dine-in</span></label>
                    <label class="p-2.5 rounded-xl item-bg border text-center cursor-pointer"><input type="radio" name="dine" value="กลับบ้าน (ใส่ถุง)" class="accent-amber-500 mr-1"><span id="opt_out">Takeaway</span></label>
                </div>
            </div>

            <div>
                <label class="text-xs opacity-70 block mb-1" id="lbl_pay_mod">Payment</label>
                <div class="grid grid-cols-2 gap-2 text-xs">
                    <label class="p-2.5 rounded-xl item-bg border text-center cursor-pointer"><input type="radio" name="pay" value="เงินสด" checked class="accent-amber-500 mr-1"><span id="pay_cash">Cash</span></label>
                    <label class="p-2.5 rounded-xl item-bg border text-center cursor-pointer"><input type="radio" name="pay" value="โอนพร้อมเพย์" class="accent-amber-500 mr-1"><span id="pay_qr">PromptPay</span></label>
                </div>
            </div>

            <button onclick="confirmFinalOrder()" class="w-full py-3.5 gold-btn font-black text-sm rounded-xl shadow active:scale-95">
                <span id="btn_confirm_final">Confirm Order</span>
            </button>
        </div>
    </div>

    <!-- TICKET VIEW FOR STAFF -->
    <div id="ticketView" class="hidden p-4 min-h-screen bg-neutral-900 text-black">
        <div class="bg-amber-50 p-5 rounded-2xl shadow-2xl border-4 border-amber-600 space-y-4">
            
            <div class="bg-black text-amber-400 p-3 rounded-xl text-center font-bold text-xs space-y-1">
                <p class="text-sm text-yellow-300">👉 PLEASE SHOW THIS SCREEN TO THE STAFF 👈</p>
                <p class="text-[10px] text-gray-300" id="show_staff_msg">กรุณาแสดงหน้าจอนี้ให้พ่อค้า/แม่ค้าดู</p>
            </div>

            <div class="bg-amber-100/90 p-3.5 rounded-xl border border-amber-300 text-xs space-y-2">
                <p class="font-bold text-amber-900 border-b border-amber-300/80 pb-1 flex justify-between items-center">
                    <span id="lbl_cust_summary">👤 Order Summary</span>
                    <span class="text-[10px] bg-amber-200 text-amber-800 px-2 py-0.5 rounded-full font-semibold">Customer Copy</span>
                </p>
                
                <div id="customerSummaryList" class="space-y-1.5 pt-1 text-gray-800 font-medium"></div>

                <div class="pt-2 border-t border-amber-300/60 text-xs font-bold text-amber-950 space-y-0.5">
                    <p>📍 <span id="cust_dine_txt"></span></p>
                    <p>💳 <span id="cust_pay_txt"></span></p>
                    <div class="pt-1 flex justify-between items-baseline">
                        <span>💰 <span id="cust_total_lbl">Total:</span></span>
                        <div class="text-right">
                            <span class="text-base font-black text-red-700" id="cust_total_thb"></span>
                            <span class="text-[11px] text-gray-600 block font-semibold" id="cust_total_foreign"></span>
                        </div>
                    </div>
                </div>
            </div>

            <div class="border-b-2 border-dashed border-black/30 pb-2 text-center">
                <h2 class="text-base font-black">📜 ใบสั่งอาหารภาษาไทย (สำหรับครัว)</h2>
            </div>

            <div class="text-xs font-bold space-y-1 bg-white p-2.5 rounded-lg border border-black/10">
                <p>📍 <strong>รูปแบบ:</strong> <span id="tk_dine" class="text-red-700 underline"></span></p>
                <p>💰 <strong>การชำระเงิน:</strong> <span id="tk_pay"></span></p>
            </div>

            <div class="space-y-2">
                <div id="tk_itemList" class="space-y-1.5"></div>
            </div>

            <hr class="border-black/30 my-2 border-dashed">

            <div class="flex justify-between items-center text-base font-black text-red-700">
                <span>💵 ยอดรวมทั้งหมด:</span>
                <span class="text-xl"><span id="tk_total">0</span> บาท</span>
            </div>

            <button onclick="resetAll()" class="w-full py-3 bg-neutral-900 text-white rounded-xl text-xs font-bold active:scale-95 shadow-md">
                ↺ <span id="btn_reset_lang">Order More</span> / สั่งเพิ่มหรือทำรายการใหม่
            </button>
        </div>
    </div>

    <!-- JAVASCRIPT SYSTEM -->
    <script>
        let currentLang = 'en';
        let cart = [];

        // 👉 URL Webhook จาก Google Apps Script เพื่อส่งเตือนเข้า LINE
        const LINE_WEBHOOK_URL = "https://script.google.com/macros/s/AKfycbyxITcTdGbi43L6n5-z15mNTvCZASWYzmlR6rM1fEpGedwZiuwI9mOq0qyCiNYfGiJl/exec";

        const langMeta = {
            en: { name: 'EN', flag: 'https://flagcdn.com/w40/gb.png', rate: 0.028, symbol: 'USD $' },
            zh: { name: '中文', flag: 'https://flagcdn.com/w40/cn.png', rate: 0.20, symbol: 'CNY ¥' },
            ja: { name: '日本語', flag: 'https://flagcdn.com/w40/jp.png', rate: 4.35, symbol: 'JPY ¥' },
            ru: { name: 'RU', flag: 'https://flagcdn.com/w40/ru.png', rate: 2.50, symbol: 'RUB ₽' },
            ms: { name: 'BM', flag: 'https://flagcdn.com/w40/my.png', rate: 0.13, symbol: 'MYR RM' },
            vi: { name: 'VN', flag: 'https://flagcdn.com/w40/vn.png', rate: 710.0, symbol: 'VND ₫' },
            my: { name: 'MM', flag: 'https://flagcdn.com/w40/mm.png', rate: 58.0, symbol: 'MMK K' },
	        ko: { name: '한국어', flag: 'https://flagcdn.com/w40/kr.png', rate: 38.0, symbol: 'KRW ₩' },
	        lo: { name: 'ລາວ', flag: 'https://flagcdn.com/w40/la.png', rate: 600.0, symbol: 'LAK ₭' },
            th: { name: 'ไทย', flag: 'https://flagcdn.com/w40/th.png', rate: 1.0, symbol: 'THB ฿' }
        };

        const dict = {
            en: {
                soup: "Select Soup Base", noodle: "Select Noodles", veg: "Veggies & Exclusions", topping: "Select Topping",
                size: "Size", add_bowl: "Add Bowl to Cart", cart_title: "Your Cart", items: "Items", checkout: "Checkout",
                modal_title: "Order Options", dine: "Option", pay: "Payment", confirm_final: "Confirm Order",
                s1: "Clear Soup", s2: "Thick Soup (Nam Tok)", s3: "Dry Noodles", s4: "Soup Only (No Noodles)",
                n1: "Thin Rice", n2: "Vermicelli", n3: "Flat Rice", n4: "Egg Noodles", n5: "Glass Noodles", n6: "Instant",
                spinach: "Water Spinach", sprouts: "Bean Sprouts", no_title: "Do NOT add:",
                no_garlic: "No Garlic Oil", no_coriander: "No Coriander", no_pepper: "No Pepper",
                t1: "Pork Balls", t2: "Beef Balls", t3: "Tendon Beef Balls", t4: "Marinated Pork", t5: "Pork Balls + Pork", t6: "Beef Balls + Pork", t7: "All Mix (50฿)",
                t8: "Tendon Beef Balls + Pork", t9: "Mixed Beef Balls + Pork",
                reg: "Regular (40฿)", ext: "Extra (50฿)", in: "Dine-in", out: "Takeaway", cash: "Cash", qr: "PromptPay QR",
                cust_summary: "👤 Order Summary", total_lbl: "Total:", reset: "Order More",
                staff_msg: "Please show this screen to the staff",
                hint_select: "⚠️ Please select 1 option", hint_opt: "Optional customization",
                err_soup: "Please select a soup base!", err_noodle: "Please select a noodle type!",
                err_topping: "Please select topping/meat!", err_size: "Please select portion size!",
                err_cart_empty: "Your cart is empty! Please add a noodle bowl first.",
                cart_added_title: "Added to Cart!", cart_added_msg: "Would you like to order more items or proceed to checkout?",
                btn_continue: "Order More", btn_checkout_now: "Checkout Now"
            },

            zh: {
                soup: "选择汤底", noodle: "选择面条", veg: "蔬菜与特殊要求", topping: "选择配料",
                size: "份量", add_bowl: "加入购物车", cart_title: "您的购物车", items: "件", checkout: "去结算",
                modal_title: "订单选项", dine: "用餐方式", pay: "支付方式", confirm_final: "确认提交订单",
                s1: "清汤", s2: "浓汤 (Nam Tok)", s3: "干拌面", s4: "无面条 (仅汤)",
                n1: "细米粉", n2: "细面", n3: "宽米粉", n4: "鸡蛋面", n5: "粉丝", n6: "方便面",
                spinach: "空心菜", sprouts: "豆芽", no_title: "请勿添加:",
                no_garlic: "不要蒜油", no_coriander: "不要香菜", no_pepper: "不要胡椒粉",
                t1: "猪肉丸", t2: "牛肉丸", t3: "牛筋丸", t4: "腌猪肉", t5: "猪肉丸+猪肉", t6: "牛肉丸+猪肉", t7: "全家福 (50฿)",
                t8: "牛筋丸+猪肉", t9: "什锦牛肉丸+猪肉",
                reg: "普通份 (40฿)", ext: "加量份 (50฿)", in: "堂食", out: "外带", cash: "现金", qr: "PromptPay 扫码",
                cust_summary: "👤 您的订单摘要", total_lbl: "总计:", reset: "再点一份",
                staff_msg: "请向工作人员出示此屏幕",
                hint_select: "⚠️ 请选择 1 个选项", hint_opt: "可选的自定义项",
                err_soup: "请选择汤底！", err_noodle: "请选择面条类型！",
                err_topping: "请选择配料/肉类！", err_size: "请选择份量！",
                err_cart_empty: "您的购物车是空的！请先选择一碗面。",
                cart_added_title: "已加入购物车！", cart_added_msg: "您想继续点餐还是去结算？",
                btn_continue: "继续点餐", btn_checkout_now: "去结算"
            },
		ko: {
    soup: "육수/국물 เลือก", noodle: "면 เลือก", veg: "야채 및 제외 항목", topping: "토핑 เลือก",
    size: "크기", add_bowl: "장바구니에 담기", cart_title: "장바구니", items: "개", checkout: "주문하기",
    modal_title: "주문 옵션", dine: "식사 방식", pay: "결제 방법", confirm_final: "주문 확정",
    s1: "맑은 국물", s2: "진한 국물 (Nam Tok)", s3: "비빔면", s4: "면 없음 (국물만)",
    n1: "얇은 쌀국수", n2: "버미셀리 (실쌀국수)", n3: "넓은 쌀국수", n4: "에그누들 (บะหมี่)", n5: "당면", n6: "라면 (Instant)",
    spinach: "모닝글로리 (공심채)", sprouts: "숙주나물", no_title: "제외할 항목:",
    no_garlic: "마늘 오일 빼기", no_coriander: "고수 빼기", no_pepper: "후추 빼기",
    t1: "돼지고기 완자", t2: "소고기 완자", t3: "소힘줄 완자", t4: "양념 돼지고기", t5: "돼지 완자 + 돼지고기", t6: "소 완자 + 돼지고기", t7: "모듬 토핑 (50฿)",
    t8: "소힘줄 완자 + 돼지고기", t9: "모듬 소 완자 + 돼지고기",
    reg: "보통 (40฿)", ext: "곱배기 (50฿)", in: "매장 식사", out: "포장 (Takeaway)", cash: "현금", qr: "PromptPay QR",
    cust_summary: "👤 주문 내역", total_lbl: "총금액:", reset: "추가 주문하기",
    staff_msg: "이 화면을 직원에게 보여주세요",
    hint_select: "⚠️ 1가지 옵션을 선택해주세요", hint_opt: "추가 맞춤 설정",
    err_soup: "육수를 선택해주세요!", err_noodle: "면 종류를 선택해주세요!",
    err_topping: "토핑/고기를 선택해주세요!", err_size: "크기를 선택해주세요!",
    err_cart_empty: "장바구니가 비어 있습니다! 먼저 국수를 선택해주세요.",
    cart_added_title: "장바구니에 담겼습니다!", cart_added_msg: "추가로 주문하시겠습니까, 아니면 결제하시겠습니까?",
    btn_continue: "추가 주문하기", btn_checkout_now: "바로 결제하기"
},
            ja: {
                soup: "スープの選択", noodle: "麺の選択", veg: "野菜・トッピング除外", topping: "具材の選択",
                size: "サイズ", add_bowl: "カートに追加", cart_title: "カート", items: "点", checkout: "注文に進む",
                modal_title: "注文オプション", dine: "お食事場所", pay: "お支払い方法", confirm_final: "注文を確定する",
                s1: "あっさりスープ", s2: "濃厚スープ", s3: "汁なし面", s4: "麺なし (スープのみ)",
                n1: "細米麺", n2: "極細米麺", n3: "平打ち米麺", n4: "中華麺", n5: "春雨", n6: "インスタント麺",
                spinach: "空心菜", sprouts: "もやし", no_title: "入れないもの:",
                no_garlic: "ガーリックオイルなし", no_coriander: "パクチーなし", no_pepper: "コショウなし",
                t1: "豚肉つみれ", t2: "牛肉つみれ", t3: "牛筋つみれ", t4: "味付け豚肉", t5: "豚つみれ+豚肉", t6: "牛つみれ+豚肉", t7: "ミックス (50฿)",
                t8: "牛筋つみれ+豚肉", t9: "ミックス牛つみれ+豚肉",
                reg: "並盛り (40฿)", ext: "大盛り (50฿)", in: "店内飲食", out: "テイクアウト", cash: "現金", qr: "PromptPay QR",
                cust_summary: "👤 注文内容の確認", total_lbl: "合計:", reset: "追加注文",
                staff_msg: "店員にこの画面をお見せください",
                hint_select: "⚠️ 1つのオプションを選択してください", hint_opt: "オプショナル設定",
                err_soup: "スープを選択してください！", err_noodle: "麺の種類を選択してください！",
                err_topping: "具材を選択してください！", err_size: "サイズを選択してください！",
                err_cart_empty: "カートが空です！まず麺を選択してください。",
                cart_added_title: "カートに追加しました！", cart_added_msg: "さらに注文を続けますか？それともお会計に進みますか？",
                btn_continue: "注文を続ける", btn_checkout_now: "お会計に進む"
            },
            ru: {
                soup: "Выберите бульон", noodle: "Выберите лапшу", veg: "Овощи и исключения", topping: "Выберите добавки",
                size: "Размер", add_bowl: "В корзину", cart_title: "Корзина", items: "шт", checkout: "Оформить",
                modal_title: "Параметры заказа", dine: "Где кушать", pay: "Оплата", confirm_final: "Подтвердить заказ",
                s1: "Прозрачный бульон", s2: "Густой бульон", s3: "Сухая лапша", s4: "Без лапши (только бульон)",
                n1: "Тонкая рисовая", n2: "Вермишель", n3: "Широкая рисовая", n4: "Яичная лапша", n5: "Стеклянная лапша", n6: "Лапша б/п",
                spinach: "Шпинат", sprouts: "Ростки маша", no_title: "НЕ добавлять:",
                no_garlic: "Без чеснока", no_coriander: "Без кинзы", no_pepper: "Без перца",
                t1: "Свиные шарики", t2: "Говяжьи шарики", t3: "Шарики из сухожилий", t4: "Маринованная свинина", t5: "Шарики + Свинина", t6: "Говяжьи шарики + Свинина", t7: "Все ингредиенты (50฿)",
                t8: "Шарики из сухожилий + Свинина", t9: "Ассорти говяжьих шариков + Свинина",
                reg: "Обычная (40฿)", ext: "Большая (50฿)", in: "В заведении", out: "С собой", cash: "Наличные", qr: "PromptPay QR",
                cust_summary: "👤 Ваш заказ", total_lbl: "Итого:", reset: "Заказать еще",
                staff_msg: "Покажите этот экран персоналу",
                hint_select: "⚠️ Пожалуйста, выберите 1 вариант", hint_opt: "Дополнительные настройки",
                err_soup: "Пожалуйста, выберите бульон!", err_noodle: "Пожалуйста, выберите лапшу!",
                err_topping: "Пожалуйста, выберите добавку!", err_size: "Пожалуйста, выберите размер!",
                err_cart_empty: "Ваша корзина пуста! Сначала добавьте порцию.",
                cart_added_title: "Добавлено в корзину!", cart_added_msg: "Хотите заказать что-нибудь еще или перейти к оформлению?",
                btn_continue: "Заказать еще", btn_checkout_now: "Оформить заказ"
            },
            ms: {
                soup: "Pilih Sup", noodle: "Pilih Mi", veg: "Sayur & Pilihan", topping: "Pilih Bahan",
                size: "Saiz", add_bowl: "Tambah ke Troli", cart_title: "Troli Anda", items: "Item", checkout: "Pesanan",
                modal_title: "Pilihan Pesanan", dine: "Makan", pay: "Bayaran", confirm_final: "Sahkan Pesanan",
                s1: "Sup Jernih", s2: "Sup Pekat", s3: "Mi Kering", s4: "Tanpa Mi (Sup Sahaja)",
                n1: "Bihun Halus", n2: "Bihun", n3: "Kuey Teow", n4: "Mi Telur", n5: "Suhun", n6: "Mi Segera",
                spinach: "Kangkung", sprouts: "Taugeh", no_title: "JANGAN tambah:",
                no_garlic: "Tanpa Minyak Bawang", no_coriander: "Tanpa Ketumbar", no_pepper: "Tanpa Lada Sulah",
                t1: "Bebola Daging Babi", t2: "Bebola Daging Lembu", t3: "Bebola Urat Lembu", t4: "Daging Babi Perap", t5: "Bebola + Daging Babi", t6: "Bebola Lembu + Babi", t7: "Campur Semua (50฿)",
                t8: "Bebola Urat + Babi", t9: "Bebola Campur + Babi",
                reg: "Biasa (40฿)", ext: "Besar (50฿)", in: "Makan di Kedai", out: "Bungkus", cash: "Tunai", qr: "PromptPay QR",
                cust_summary: "👤 Ringkasan Pesanan", total_lbl: "Jumlah:", reset: "Tambah Pesanan",
                staff_msg: "Sila tunjukkan skrin ini kepada staf",
                hint_select: "⚠️ Sila pilih 1 pilihan", hint_opt: "Pilihan tambahan",
                err_soup: "Sila pilih sup!", err_noodle: "Sila pilih mi!",
                err_topping: "Sila pilih bahan/daging!", err_size: "Sila pilih saiz!",
                err_cart_empty: "Troli anda kosong! Sila tambah mi dahulu.",
                cart_added_title: "Ditambah ke Troli!", cart_added_msg: "Adakah anda ingin menambah pesanan atau terus ke pembayaran?",
                btn_continue: "Tambah Pesanan", btn_checkout_now: "Bayar Sekarang"
            },
            vi: {
                soup: "Chọn nước dùng", noodle: "Chọn loại mì", veg: "Rau & Yêu cầu", topping: "Chọn nhân",
                size: "Kích thước", add_bowl: "Thêm vào giỏ", cart_title: "Giỏ hàng", items: "Món", checkout: "Thanh toán",
                modal_title: "Tùy chọn đặt hàng", dine: "Hình thức", pay: "Thanh toán", confirm_final: "Xác nhận đặt hàng",
                s1: "Nước dùng trong", s2: "Nước dùng đậm đà", s3: "Mì khô", s4: "Không mì (Chỉ nước)",
                n1: "Hủ tiếu nhỏ", n2: "Bún", n3: "Hủ tiếu mềm", n4: "Mì trứng", n5: "Miến", n6: "Mì ăn liền",
                spinach: "Rau muống", sprouts: "Giá đỗ", no_title: "KHÔNG thêm:",
                no_garlic: "Không tỏi phi", no_coriander: "Không rau mùi", no_pepper: "Không tiêu",
                t1: "Bò viên heo", t2: "Bò viên bò", t3: "Bò viên gân", t4: "Thịt heo ướp", t5: "Bò viên heo + Thịt heo", t6: "Bò viên bò + Thịt heo", t7: "Thập cẩm (50฿)",
                t8: "Bò viên gân + Thịt heo", t9: "Bò viên thập cẩm + Thịt heo",
                reg: "Tô vừa (40฿)", ext: "Tô lớn (50฿)", in: "Ăn tại chỗ", out: "Mang về", cash: "Tiền mặt", qr: "PromptPay QR",
                cust_summary: "👤 Tóm tắt đơn hàng", total_lbl: "Tổng cộng:", reset: "Đặt thêm món",
                staff_msg: "Vui lòng đưa màn hình này cho nhân viên",
                hint_select: "⚠️ Vui lòng chọn 1 tùy chọn", hint_opt: "Tùy chỉnh bổ sung",
                err_soup: "Vui lòng chọn nước dùng!", err_noodle: "Vui lòng chọn loại mì!",
                err_topping: "Vui lòng chọn nhân/thịt!", err_size: "Vui lòng chọn kích thước!",
                err_cart_empty: "Giỏ hàng trống! Vui lòng chọn món trước.",
                cart_added_title: "Đã thêm vào giỏ hàng!", cart_added_msg: "Bạn muốn chọn thêm món hay tiến hành thanh toán?",
                btn_continue: "Chọn thêm món", btn_checkout_now: "Thanh toán ngay"
            },
            my: {
                soup: "ဟင်းရည်ရွေးပါ", noodle: "ခေါက်ဆွဲရွေးပါ", veg: "ဟင်းသီးဟင်းရွက်", topping: "အသားရွေးပါ",
                size: "အရွယ်အစား", add_bowl: "ခြင်းထဲထည့်မည်", cart_title: "စျေးဝယ်ခြင်း", items: "ခု", checkout: "မှာယူမည်",
                modal_title: "မှာယူမှုရွေးချယ်စရာများ", dine: "စားသောက်ပုံ", pay: "ငွေပေးချေမှု", confirm_final: "အတည်ပြုမည်",
                s1: "ဟင်းရည်ကြည်", s2: "အပျစ်ဟင်းရည်", s3: "အခြောက်", s4: "ခေါက်ဆွဲမပါ (ဟင်းရည်သီးသန့်)",
                n1: "ဆန်ခေါက်ဆွဲသေး", n2: "ကြာဆံ", n3: "ဆန်ခေါက်ဆွဲကြီး", n4: "ဘဲဥခေါက်ဆွဲ", n5: "ပဲကြာဆံ", n6: "ခေါက်ဆွဲခြောက်",
                spinach: "ကန်စွန်းရွက်", sprouts: "ပဲပင်ပေါက်", no_title: "မထည့်ပါနှင့်:",
                no_garlic: "ကြက်သွန်ဖြူဆီမပါ", no_coriander: "နံနံပင်မပါ", no_pepper: "ငရုတ်ကောင်းမပါ",
                t1: "ဝက်သားလုံး", t2: "အမဲသားလုံး", t3: "အမဲအကြောလုံး", t4: "နှပ်ထားသောဝက်သား", t5: "ဝက်သားလုံး + ဝက်သား", t6: "အမဲသားလုံး + ဝက်သား", t7: "အားလုံးပါ (50฿)",
                t8: "အမဲအကြောလုံး + ဝက်သား", t9: "အမဲသားလုံးစုံ + ဝက်သား",
                reg: "ရိုးရိုး (40฿)", ext: "အထူး (50฿)", in: "ဆိုင်စား", out: "ပါဆယ်", cash: "ငွေသား", qr: "PromptPay QR",
                cust_summary: "👤 မှာယူမှုအကျဉ်းချုပ်", total_lbl: "စုစုပေါင်း:", reset: "ထပ်မံမှာယူမည်",
                staff_msg: "ကျေးဇူးပြု၍ ဤမျက်နှာပြင်ကို ဝန်ထမ်းအား ပြပါ",
                hint_select: "⚠️ ၁ ခု ရွေးချယ်ပါ", hint_opt: "စိတ်ကြိုက် ထပ်မံရွေးချယ်နိုင်သည်",
                err_soup: "ကျေးဇူးပြု၍ ဟင်းရည် ရွေးပါ!", err_noodle: "ကျေးဇူးပြု၍ ခေါက်ဆွဲ ရွေးပါ!",
                err_topping: "ကျေးဇူးပြု၍ အသား ရွေးပါ!", err_size: "ကျေးဇူးပြု၍ အရွယ်အစား ရွေးပါ!",
                err_cart_empty: "စျေးဝယ်ခြင်း လွတ်နေပါသည်! ပထမဦးစွာ ခေါက်ဆွဲ ရွေးပါ။",
                cart_added_title: "ခြင်းထဲသို့ ထည့်ပြီးပါပြီ!", cart_added_msg: "ထပ်မံ မှာယူလိုပါသလား သို့မဟုတ် ငွေပေးချေမှုသို့ ဆက်သွားမလား?",
                btn_continue: "ထပ်မံ မှာယူမည်", btn_checkout_now: "ငွေပေးချေမည်"
            },
	lo: {
    soup: "ເລືອກນ້ຳຊຸບ", noodle: "ເລືອກເສັ້ນ", veg: "ຜັກ ແລະ ຂໍ້ຫ້າມ", topping: "ເລືອກເຄື່ອງ",
    size: "ຂະໜາດ", add_bowl: "ເພີ່ມລົງຕ່າກ້າ", cart_title: "ຕ່າກ້າຂອງທ່ານ", items: "ລາຍການ", checkout: "ສັ່ງຊື້",
    modal_title: "ຕົວເລືອກການສັ່ງຊື້", dine: "ຮູບແບບ", pay: "ການຊຳລະເງິນ", confirm_final: "ຢືນຢັນການສົ່ງລາຍການ",
    s1: "ນ້ຳໃສ", s2: "ນ້ຳຕົກ", s3: "ແຫ້ງ", s4: "ເກົາເຫຼົາ (ບໍ່ໃສ່ເສັ້ນ)",
    n1: "ເສັ້ນເລັກ", n2: "ເສັ້ນໝີ່", n3: "ເສັ້ນໃຫຍ່", n4: "ບະໝີ່", n5: "ວຸ້ນເສັ້ນ", n6: "ມາມ່າ",
    spinach: "ຜັກບຸ້ງ", sprouts: "ຖົ່ວງອກ", no_title: "ສິ່ງທີ່ບໍ່ໃສ່:",
    no_garlic: "ບໍ່ໃສ່ນ້ຳມັນຈຽວ", no_coriander: "ບໍ່ໃສ່ຜັກຊີ", no_pepper: "ບໍ່ໃສ່ພິກໄທ",
    t1: "ລູກຊິ້ນໝູ", t2: "ລູກຊິ້ນງົວ", t3: "ລູກຊິ້ນງົວເອັນ", t4: "ໝູໝັກ", t5: "ລູກຊິ້ນໝູ + ໝູໝັກ", t6: "ລູກຊິ້ນງົວ + ໝູໝັກ", t7: "ຮວມທຸກຢ່າງ (50฿)",
    t8: "ລູກຊິ້ນເອັນງົວ + ໝູໝັກ", t9: "ລູກຊິ້ນງົວຮວມ + ໝູໝັກ",
    reg: "ທຳມະດາ (40฿)", ext: "ພິເສດ (50฿)", in: "ກິນຢູ່ຮ້ານ (ໃສ່ຖ້ວຍ)", out: "ກັບບ້ານ (ໃສ່ຖົງ)", cash: "ເງິນສົດ", qr: "ໂອນພ້ອມເພ",
    cust_summary: "👤 ສະຫຼຸບລາຍການຂອງທ່ານ", total_lbl: "ຍອດຮວມທັງໝົດ:", reset: "ສັ່ງເພີ່ມ ຫຼື ເຮັດລາຍການໃຫມ່",
    staff_msg: "ກະລຸນາສະແດງໜ້າຈໍນີ້ໃຫ້ພໍ່ຄ້າ/ແມ່ຄ້າເບິ່ງ",
    hint_select: "⚠️ ກະລຸນາຄລິກເລືອກ 1 ຢ່າງ", hint_opt: "ເລືອກຜັກ ແລະ ເພີ່ມເຕີມໄດ້ຕາມຊອບ",
    err_soup: "ກະລຸນາເລືອກນ້ຳຊຸບກ່ອນ!", err_noodle: "ກະລຸນາເລືອກເສັ້ນກ່ອນ!",
    err_topping: "ກະລຸນາເລືອກເຄື່ອງ/ເນື້ອສັດກ່ອນ!", err_size: "ກະລຸນາເລືອກຂະໜາດກ່ອນ!",
    err_cart_empty: "ຕ່າກ້າຂອງທ່ານຍັງຫວ່າງຢູ່! ກະລຸນາກົດເລືອກກ໋ວຍເຕ໋ຽວກ່ອນ.",
    cart_added_title: "ເພີ່ມລົງຕ່າກ້າແລ້ວ!", cart_added_msg: "ທ່ານຕ້ອງການເລືອກສັ່ງອາຫານເພີ່ມ ຫຼື ໄປທີ່ລາຍການສັ່ງຊື້ເລີຍ?",
    btn_continue: "ສັ່ງອາຫານຕໍ່", btn_checkout_now: "ເບິ່ງຕ່າກ້າ / ສັ່ງເລີຍ"
},
            th: {
                soup: "เลือกน้ำซุป", noodle: "เลือกเส้น", veg: "ผักและเพิ่มเติม", topping: "เลือกเครื่อง",
                size: "ขนาด", add_bowl: "เพิ่มลงตะกร้า", cart_title: "ตะกร้าของคุณ", items: "รายการ", checkout: "สั่งซื้อ",
                modal_title: "ตัวเลือกสั่งซื้อ", dine: "รูปแบบ", pay: "การชำระเงิน", confirm_final: "ยืนยันการส่งรายการ",
                s1: "น้ำใส", s2: "น้ำตก", s3: "แห้ง", s4: "เกาเหลา (ไม่ใส่เส้น)",
                n1: "เส้นเล็ก", n2: "เส้นหมี่", n3: "เส้นใหญ่", n4: "บะหมี่", n5: "วุ้นเส้น", n6: "มาม่า",
                spinach: "ผักบุ้ง", sprouts: "ถั่วงอก", no_title: "สิ่งที่ไม่ใส่:",
                no_garlic: "ไม่ใส่น้ำมันเจียว", no_coriander: "ไม่ใส่ผักชี", no_pepper: "ไม่ใส่พริกไทย",
                t1: "ลูกชิ้นหมู", t2: "ลูกชิ้นเนื้อ", t3: "ลูกชิ้นเนื้อเอ็น", t4: "หมูหมัก", t5: "ลูกชิ้นหมู + หมูหมัก", t6: "ลูกชิ้นเนื้อ + หมูหมัก", t7: "รวมทุกอย่าง (50฿)",
                t8: "ลูกชิ้นเอ็นเนื้อ + หมูหมัก", t9: "ลูกชิ้นเนื้อรวม + หมูหมัก",
                reg: "ธรรมดา (40฿)", ext: "พิเศษ (50฿)", in: "ทานที่ร้าน (ใส่ถ้วย)", out: "กลับบ้าน (ใส่ถุง)", cash: "เงินสด", qr: "โอนพร้อมเพย์",
                cust_summary: "👤 สรุปรายการของคุณ", total_lbl: "ยอดรวมทั้งสิ้น:", reset: "สั่งเพิ่มหรือทำรายการใหม่",
                staff_msg: "กรุณาแสดงหน้าจอนี้ให้พ่อค้า/แม่ค้าดู",
                hint_select: "⚠️ กรุณาคลิกเลือก 1 อย่าง", hint_opt: "เลือกผักและเพิ่มเติมได้ตามชอบ",
                err_soup: "กรุณาเลือกน้ำซุปก่อนครับ!", err_noodle: "กรุณาเลือกเส้นก่อนครับ!",
                err_topping: "กรุณาเลือกเครื่อง/เนื้อสัตว์ก่อนครับ!", err_size: "กรุณาเลือกขนาดก่อนครับ!",
                err_cart_empty: "ตะกร้าของคุณยังว่างอยู่! กรุณากดเลือกก๋วยเตี๋ยวก่อนครับ",
                cart_added_title: "เพิ่มลงในตะกร้าแล้ว!", cart_added_msg: "คุณต้องการเลือกสั่งอาหารเพิ่ม หรือไปที่รายการสั่งซื้อเลย?",
                btn_continue: "สั่งอาหารต่อ", btn_checkout_now: "ดูตะกร้า / สั่งเลย"
            }
        };

        function showAlert(msg) {
            document.getElementById('alertMsg').innerText = msg;
            document.getElementById('alertModal').classList.remove('hidden');
        }

        function closeAlertModal() {
            document.getElementById('alertModal').classList.add('hidden');
        }

        function toggleTheme() {
            const body = document.body;
            const themeIcon = document.getElementById('themeIcon');
            if (body.classList.contains('dark-mode')) {
                body.classList.remove('dark-mode');
                body.classList.add('light-mode');
                themeIcon.innerText = '🌙';
            } else {
                body.classList.remove('light-mode');
                body.classList.add('dark-mode');
                themeIcon.innerText = '☀️';
            }
        }

        function selectLanguage(lang) {
            currentLang = lang;
            document.getElementById('currentFlagImg').src = langMeta[lang].flag;
            document.getElementById('currentLangName').innerText = langMeta[lang].name;
            applyTranslations(lang);
            document.getElementById('welcomeScreen').classList.add('hidden');
        }

        function openLanguageScreen() {
            document.getElementById('welcomeScreen').classList.remove('hidden');
        }

        function applyTranslations(lang) {
            const data = dict[lang] || dict['en'];
            document.getElementById('label_soup').innerText = data.soup;
            document.getElementById('label_noodle').innerText = data.noodle;
            document.getElementById('label_veg').innerText = data.veg;
            document.getElementById('label_topping').innerText = data.topping;
            document.getElementById('label_size').innerText = data.size;
            document.getElementById('btn_add_bowl').innerText = data.add_bowl;
            document.getElementById('lbl_cart_title').innerText = data.cart_title;
            document.getElementById('lbl_items').innerText = data.items;
            document.getElementById('btn_checkout').innerText = data.checkout;
            document.getElementById('modal_title').innerText = data.modal_title;
            document.getElementById('lbl_dine_mod').innerText = data.dine;
            document.getElementById('lbl_pay_mod').innerText = data.pay;
            document.getElementById('btn_confirm_final').innerText = data.confirm_final;

            // อัปเดตข้อความใบ้ภาษาลูกค้า
            document.getElementById('hint_soup').innerText = data.hint_select;
            document.getElementById('hint_noodle').innerText = data.hint_select;
            document.getElementById('hint_topping').innerText = data.hint_select;
            document.getElementById('hint_veg').innerText = data.hint_opt;

            document.getElementById('soup_1').innerText = data.s1;
            document.getElementById('soup_2').innerText = data.s2;
            document.getElementById('soup_3').innerText = data.s3;
            document.getElementById('soup_4').innerText = data.s4;

            document.getElementById('nd_1').innerText = data.n1;
            document.getElementById('nd_2').innerText = data.n2;
            document.getElementById('nd_3').innerText = data.n3;
            document.getElementById('nd_4').innerText = data.n4;
            document.getElementById('nd_5').innerText = data.n5;
            document.getElementById('nd_6').innerText = data.n6;

            document.getElementById('txt_spinach').innerText = data.spinach;
            document.getElementById('txt_sprouts').innerText = data.sprouts;
            document.getElementById('txt_no_title').innerText = data.no_title;
            document.getElementById('txt_no_garlic').innerText = data.no_garlic;
            document.getElementById('txt_no_coriander').innerText = data.no_coriander;
            document.getElementById('txt_no_pepper').innerText = data.no_pepper;

            document.getElementById('top_1').innerText = data.t1;
            document.getElementById('top_2').innerText = data.t2;
            document.getElementById('top_3').innerText = data.t3;
            document.getElementById('top_4').innerText = data.t4;
            document.getElementById('top_5').innerText = data.t5;
            document.getElementById('top_6').innerText = data.t6;
            document.getElementById('top_7').innerText = data.t7;
            document.getElementById('top_8').innerText = data.t8;
            document.getElementById('top_9').innerText = data.t9;

            document.getElementById('sz_reg').innerText = data.reg;
            document.getElementById('sz_ext').innerText = data.ext;
            document.getElementById('opt_in').innerText = data.in;
            document.getElementById('opt_out').innerText = data.out;
            document.getElementById('pay_cash').innerText = data.cash;
            document.getElementById('pay_qr').innerText = data.qr;

            // ข้อความ Modal ตะกร้า
            document.getElementById('cartModalTitle').innerText = data.cart_added_title;
            document.getElementById('cartModalMsg').innerText = data.cart_added_msg;
            document.getElementById('btn_continue_order').innerText = data.btn_continue;
            document.getElementById('btn_go_checkout').innerText = data.btn_checkout_now;
        }

        function handleSoupChange(isGaoLao) {
            const noodleSection = document.getElementById('noodleSection');
            if (isGaoLao) {
                noodleSection.classList.add('hidden');
            } else {
                noodleSection.classList.remove('hidden');
            }
        }

        function toggleAllMixLock(isAllMix) {
            const radioReg = document.getElementById('radio_sz_reg');
            const radioExt = document.getElementById('radio_sz_ext');
            const lblReg = document.getElementById('lbl_sz_reg');

            if (isAllMix) {
                radioExt.checked = true;
                radioReg.disabled = true;
                lblReg.classList.add('opacity-40', 'cursor-not-allowed');
            } else {
                radioReg.disabled = false;
                lblReg.classList.remove('opacity-40', 'cursor-not-allowed');
            }
        }

        function getNativeTitle(item, lang) {
    const langData = dict[lang] || dict['en'];
    const isGaoLao = (item.rawSoup === "เกาเหลา");

    let nativeSoup = langData.s1;
    if(item.rawSoup === "น้ำตก") nativeSoup = langData.s2;
    if(item.rawSoup === "แห้ง") nativeSoup = langData.s3;
    if(item.rawSoup === "เกาเหลา") nativeSoup = langData.s4;

    let nativeNoodle = "";
    if(!isGaoLao) {
        if(item.rawNoodle === "เส้นเล็ก") nativeNoodle = langData.n1;
        if(item.rawNoodle === "เส้นหมี่") nativeNoodle = langData.n2;
        if(item.rawNoodle === "เส้นใหญ่") nativeNoodle = langData.n3;
        if(item.rawNoodle === "บะหมี่") nativeNoodle = langData.n4;
        if(item.rawNoodle === "วุ้นเส้น") nativeNoodle = langData.n5;
        if(item.rawNoodle === "มาม่า") nativeNoodle = langData.n6;
    }

    let nativeTopping = item.rawTopping;
    if(item.rawTopping === "ลูกชิ้นหมู") nativeTopping = langData.t1;
    if(item.rawTopping === "ลูกชิ้นเนื้อ") nativeTopping = langData.t2;
    if(item.rawTopping === "ลูกชิ้นเนื้อเอ็น") nativeTopping = langData.t3;
    if(item.rawTopping === "หมูหมัก") nativeTopping = langData.t4;
    if(item.rawTopping === "ลูกชิ้นหมู+หมูหมัก") nativeTopping = langData.t5;
    if(item.rawTopping === "ลูกชิ้นเนื้อ+หมูหมัก") nativeTopping = langData.t6;
    if(item.rawTopping === "รวมทุกอย่าง") nativeTopping = langData.t7;
    if(item.rawTopping === "ลูกชิ้นเอ็นเนื้อ+หมูหมัก") nativeTopping = langData.t8;
    if(item.rawTopping === "ลูกชิ้นเนื้อรวม+หมูหมัก") nativeTopping = langData.t9;

    return isGaoLao ? `${nativeSoup} (${nativeTopping})` : `${nativeNoodle} - ${nativeSoup} (${nativeTopping})`;
}

        function getNativeSize(price, lang) {
            const langData = dict[lang] || dict['en'];
            return price === 50 ? langData.ext.split(' ')[0] : langData.reg.split(' ')[0];
        }

        function addNoodleToCart() {
            const langData = dict[currentLang] || dict['en'];

            const soupRadio = document.querySelector('input[name="soup"]:checked');
            if(!soupRadio) {
                showAlert(langData.err_soup);
                return;
            }
            const soupVal = soupRadio.value;
            const isGaoLao = (soupVal === "เกาเหลา");

            let noodleVal = "";
            if(!isGaoLao) {
                const noodleRadio = document.querySelector('input[name="noodle"]:checked');
                if(!noodleRadio) {
                    showAlert(langData.err_noodle);
                    return;
                }
                noodleVal = noodleRadio.value;
            }

            const toppingRadio = document.querySelector('input[name="topping"]:checked');
            if(!toppingRadio) {
                showAlert(langData.err_topping);
                return;
            }
            const toppingVal = toppingRadio.value;

            const sizeRadio = document.querySelector('input[name="size"]:checked');
            if(!sizeRadio) {
                showAlert(langData.err_size);
                return;
            }
            const sizeVal = parseInt(sizeRadio.value);

            const spinach = document.getElementById('veg_spinach').checked;
            const sprouts = document.getElementById('veg_sprouts').checked;
            let veg = [];
            if(spinach) veg.push("ผักบุ้ง");
            if(sprouts) veg.push("ถั่วงอก");

            const noGarlic = document.getElementById('no_garlic').checked;
            const noCoriander = document.getElementById('no_coriander').checked;
            const noPepper = document.getElementById('no_pepper').checked;
            let noList = [];
            if(noGarlic) noList.push("ไม่ใส่น้ำมันเจียว");
            if(noCoriander) noList.push("ไม่ใส่ผักชี");
            if(noPepper) noList.push("ไม่ใส่พริกไทย");

            const thaiTitle = isGaoLao ? `เกาเหลา (${toppingVal})` : `${noodleVal} ${soupVal} (${toppingVal})`;
            const sizeText = sizeVal === 50 ? 'พิเศษ' : 'ธรรมดา';

            const newItem = {
                rawSoup: soupVal,
                rawNoodle: noodleVal,
                rawTopping: toppingVal,
                thaiTitle: thaiTitle,
                size: sizeText,
                price: sizeVal,
                veg: veg.length > 0 ? veg.join("+") : "ไม่ใส่ผัก",
                no: noList.join(", ")
            };

            cart.push(newItem);

            resetFormInputs();
            updateCartUI();

            // 🍜 แสดง Modal เด้งบอกชื่อรายการเป็นภาษาของลูกค้า
            const nativeTitle = getNativeTitle(newItem, currentLang);
            const nativeSize = getNativeSize(sizeVal, currentLang);
            document.getElementById('cartModalItemName').innerText = `🍜 ${nativeTitle} [${nativeSize}]`;
            document.getElementById('addToCartSuccessModal').classList.remove('hidden');
        }

        function closeCartSuccessModal() {
            document.getElementById('addToCartSuccessModal').classList.add('hidden');
        }

        function goToCheckoutFromModal() {
            closeCartSuccessModal();
            openCheckoutModal();
        }

        function resetFormInputs() {
            document.querySelectorAll('input[name="soup"]').forEach(el => el.checked = false);
            document.querySelectorAll('input[name="noodle"]').forEach(el => el.checked = false);
            document.querySelectorAll('input[name="topping"]').forEach(el => el.checked = false);
            document.querySelectorAll('input[name="size"]').forEach(el => el.checked = false);
            document.getElementById('veg_spinach').checked = false;
            document.getElementById('veg_sprouts').checked = false;
            document.getElementById('no_garlic').checked = false;
            document.getElementById('no_coriander').checked = false;
            document.getElementById('no_pepper').checked = false;
            toggleAllMixLock(false);
            document.getElementById('noodleSection').classList.remove('hidden');
        }

        function updateCartUI() {
            document.getElementById('cart_count').innerText = cart.length;
            const total = cart.reduce((sum, item) => sum + item.price, 0);
            document.getElementById('cart_total').innerText = total;
        }

        function openCheckoutModal() {
            const langData = dict[currentLang] || dict['en'];
            if (cart.length === 0) {
                showAlert(langData.err_cart_empty);
                return;
            }
            const container = document.getElementById('modalCartItems');
            container.innerHTML = cart.map((item, idx) => {
                const nativeTitle = getNativeTitle(item, currentLang);
                const nativeSize = getNativeSize(item.price, currentLang);
                return `
                <div class="flex justify-between items-center border-b pb-1">
                    <span>${idx+1}. ${nativeTitle} [${nativeSize}]</span>
                    <span class="text-amber-600 font-bold">${item.price}฿</span>
                </div>
            `}).join('');

            document.getElementById('checkoutModal').classList.remove('hidden');
        }

        function closeCheckoutModal() {
            document.getElementById('checkoutModal').classList.add('hidden');
        }

        function confirmFinalOrder() {
            const dineRadio = document.querySelector('input[name="dine"]:checked');
            const payRadio = document.querySelector('input[name="pay"]:checked');
            const dineVal = dineRadio.value;
            const payVal = payRadio.value;

            const totalTHB = cart.reduce((sum, item) => sum + item.price, 0);

            const langData = dict[currentLang] || dict['en'];
            const meta = langMeta[currentLang] || langMeta['en'];

            const nativeDine = dineVal.includes("ทานที่ร้าน") ? langData.in : langData.out;
            const nativePay = payVal.includes("เงินสด") ? langData.cash : langData.qr;

            const foreignPrice = (totalTHB * meta.rate).toFixed(1);

            document.getElementById('show_staff_msg').innerText = langData.staff_msg;
            document.getElementById('lbl_cust_summary').innerText = langData.cust_summary;
            document.getElementById('cust_dine_txt').innerText = nativeDine;
            document.getElementById('cust_pay_txt').innerText = nativePay;
            document.getElementById('cust_total_lbl').innerText = langData.total_lbl;
            document.getElementById('cust_total_thb').innerText = `${totalTHB} THB (฿)`;

            if(currentLang !== 'th') {
                document.getElementById('cust_total_foreign').innerText = `≈ ${foreignPrice} ${meta.symbol}`;
            } else {
                document.getElementById('cust_total_foreign').innerText = '';
            }

            document.getElementById('customerSummaryList').innerHTML = cart.map((item) => {
                const nativeTitle = getNativeTitle(item, currentLang);
                const nativeSize = getNativeSize(item.price, currentLang);
                return `<p class="border-b border-amber-200/60 pb-1">• ${nativeTitle} <span class="font-bold text-amber-900">[${nativeSize}]</span> - ${item.price} ฿</p>`;
            }).join('');

            document.getElementById('tk_dine').innerText = dineVal;
            document.getElementById('tk_pay').innerText = payVal;
            document.getElementById('tk_total').innerText = totalTHB;

            document.getElementById('tk_itemList').innerHTML = cart.map((item, i) => `
                <div class="bg-white p-2.5 rounded-lg border text-xs text-black shadow-sm">
                    <p class="font-black text-sm text-amber-900">${i+1}. ${item.thaiTitle} <span class="text-red-700">[${item.size}]</span> - ${item.price}.-</p>
                    <p class="text-gray-700">🥬 ผัก: ${item.veg}</p>
                    ${item.no ? `<p class="text-red-600 font-bold">❌ หมายเหตุ: ${item.no}</p>` : ''}
                </div>
            `).join('');

            document.getElementById('btn_reset_lang').innerText = langData.reset;

            // 📲 ยิงแจ้งเตือนเข้า LINE เมื่อสั่งอาหารเสร็จ
            sendLineNotify(dineVal, payVal, totalTHB);

            closeCheckoutModal();
            document.getElementById('orderForm').classList.add('hidden');
            document.getElementById('cartBar').classList.add('hidden');
            document.getElementById('ticketView').classList.remove('hidden');
            window.scrollTo(0, 0);
        }

        // 📲 ฟังก์ชันยิงออเดอร์เข้า LINE Webhook (ปรับแก้ text/plain ป้องกันการติด CORS Block)
        function sendLineNotify(dine, pay, total) {
            if(LINE_WEBHOOK_URL && LINE_WEBHOOK_URL !== "YOUR_LINE_WEBHOOK_URL_HERE") {
                fetch(LINE_WEBHOOK_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'text/plain;charset=utf-8' },
                    body: JSON.stringify({
                        dine: dine,
                        pay: pay,
                        total: total,
                        items: cart
                    })
                })
                .then(res => res.json())
                .then(data => console.log("LINE Notification Result:", data))
                .catch(err => console.log("LINE Send Error:", err));
            }
        }

        function resetAll() {
            cart = [];
            resetFormInputs();
            updateCartUI();
            document.getElementById('orderForm').classList.remove('hidden');
            document.getElementById('cartBar').classList.remove('hidden');
            document.getElementById('ticketView').classList.add('hidden');
            window.scrollTo(0, 0);
        }
    </script>
</body>
</html>
