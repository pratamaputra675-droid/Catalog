<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Blantikautoparts - iOS</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Font Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', 'Inter', sans-serif;
            -webkit-tap-highlight-color: transparent;
            transition: background-color 0.3s ease, color 0.3s ease;
        }
        /* Custom iOS Blur Backdrop */
        .ios-blur {
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
        }
        /* Smooth scrolling & hide scrollbar */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }

        /* Dynamic Color Themes */
        body.theme-light {
            background-color: #F2F2F7;
            color: #0f172a;
        }
        body.theme-dark {
            background-color: #0f172a;
            color: #f8fafc;
        }
        body.theme-dark .bg-white {
            background-color: #1e293b !important;
            border-color: #334155 !important;
            color: #f8fafc !important;
        }
        body.theme-dark .bg-\[\#F2F2F7\] {
            background-color: #0f172a !important;
        }
        body.theme-dark .bg-\[\#E3E3E8\] {
            background-color: #334155 !important;
            color: #f8fafc !important;
        }
        body.theme-dark .bg-slate-50 {
            background-color: #334155 !important;
            border-color: #475569 !important;
        }
        body.theme-dark .text-slate-900 {
            color: #f8fafc !important;
        }
        body.theme-dark .text-slate-700, body.theme-dark .text-slate-600 {
            color: #cbd5e1 !important;
        }
        body.theme-dark .text-slate-500, body.theme-dark .text-slate-400 {
            color: #94a3b8 !important;
        }
        body.theme-dark .border-slate-100, body.theme-dark .border-slate-200, body.theme-dark .border-slate-200\/80 {
            border-color: #334155 !important;
        }
        body.theme-dark select, body.theme-dark input {
            color: #f8fafc !important;
        }

        /* Midnight Blue / Navy Theme */
        body.theme-navy {
            background-color: #0b132b;
            color: #edf2f4;
        }
        body.theme-navy .bg-white {
            background-color: #1c2541 !important;
            border-color: #3a506b !important;
            color: #edf2f4 !important;
        }
        body.theme-navy .bg-\[\#F2F2F7\] {
            background-color: #0b132b !important;
        }
        body.theme-navy .bg-\[\#E3E3E8\] {
            background-color: #3a506b !important;
            color: #edf2f4 !important;
        }
        body.theme-navy .bg-slate-50 {
            background-color: #2e3d59 !important;
            border-color: #3a506b !important;
        }
        body.theme-navy .text-slate-900 {
            color: #edf2f4 !important;
        }
        body.theme-navy .text-slate-700, body.theme-navy .text-slate-600 {
            color: #d8e2dc !important;
        }
        body.theme-navy .text-slate-500, body.theme-navy .text-slate-400 {
            color: #8d99ae !important;
        }
        body.theme-navy .border-slate-100, body.theme-navy .border-slate-200, body.theme-navy .border-slate-200\/80 {
            border-color: #3a506b !important;
        }
        body.theme-navy select, body.theme-navy input {
            color: #edf2f4 !important;
        }

        /* Emerald / Forest Dark Theme */
        body.theme-emerald {
            background-color: #062c20;
            color: #ecfdf5;
        }
        body.theme-emerald .bg-white {
            background-color: #09402f !important;
            border-color: #115e45 !important;
            color: #ecfdf5 !important;
        }
        body.theme-emerald .bg-\[\#F2F2F7\] {
            background-color: #062c20 !important;
        }
        body.theme-emerald .bg-\[\#E3E3E8\] {
            background-color: #115e45 !important;
            color: #ecfdf5 !important;
        }
        body.theme-emerald .bg-slate-50 {
            background-color: #0e4f3a !important;
            border-color: #115e45 !important;
        }
        body.theme-emerald .text-slate-900 {
            color: #ecfdf5 !important;
        }
        body.theme-emerald .text-slate-700, body.theme-emerald .text-slate-600 {
            color: #a7f3d0 !important;
        }
        body.theme-emerald .text-slate-500, body.theme-emerald .text-slate-400 {
            color: #6ee7b7 !important;
        }
        body.theme-emerald .border-slate-100, body.theme-emerald .border-slate-200, body.theme-emerald .border-slate-200\/80 {
            border-color: #115e45 !important;
        }
        body.theme-emerald select, body.theme-emerald input {
            color: #ecfdf5 !important;
        }
    </style>
</head>
<body class="bg-[#F2F2F7] text-slate-900 antialiased min-h-screen pb-28 selection:bg-blue-500 selection:text-white theme-light">

    <!-- iOS Header / Navigation Bar -->
    <header class="sticky top-0 z-40 bg-[#F2F2F7]/80 ios-blur border-b border-slate-200/80 pt-safe">
        <div class="app-container max-w-md mx-auto px-4 py-3 flex items-center justify-between transition-all duration-300">
            <div class="flex items-center gap-2.5">
                <div class="bg-blue-600 text-white w-9 h-9 rounded-full flex items-center justify-center font-bold text-base shadow-sm">
                    <i class="fa-solid fa-car"></i>
                </div>
                <div>
                    <h1 class="text-base font-bold tracking-tight text-slate-900 leading-none">Blantikautoparts</h1>
                    <p class="text-[10px] text-blue-600 font-semibold tracking-wider uppercase mt-0.5">Japan Spec iOS</p>
                </div>
            </div>
            <div class="flex items-center gap-1">
                <button onclick="openModal('add')" class="bg-blue-600 hover:bg-blue-700 active:scale-95 text-white w-8 h-8 rounded-full flex items-center justify-center shadow-md transition-all">
                    <i class="fa-solid fa-plus text-sm"></i>
                </button>
                <button onclick="openSettingsModal()" class="bg-slate-200 hover:bg-slate-300 active:scale-95 text-slate-700 w-8 h-8 rounded-full flex items-center justify-center shadow-sm transition-all ml-1">
                    <i class="fa-solid fa-gear text-sm"></i>
                </button>
            </div>
        </div>
    </header>

    <main class="app-container max-w-md mx-auto px-4 pt-4 space-y-4 transition-all duration-300" id="mainContent">

        <!-- Total Card -->
        <div class="bg-white rounded-2xl p-4 shadow-sm border border-slate-100 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center text-lg">
                    <i class="fa-solid fa-boxes-stacked"></i>
                </div>
                <div>
                    <p class="text-xs font-medium text-slate-400">Total Sparepart</p>
                    <h2 class="text-xl font-bold text-slate-900" id="statTotalItems">0 <span class="text-xs font-normal text-slate-500">Item</span></h2>
                </div>
            </div>
            <button onclick="resetData()" class="p-2 text-slate-400 hover:text-red-500 active:scale-90 transition" title="Reset Data">
                <i class="fa-solid fa-rotate-right text-sm"></i>
            </button>
        </div>

        <!-- Total Sparepart Sesuai Kategori (Grid Breakdown) -->
        <div class="bg-white rounded-2xl p-3 shadow-sm border border-slate-100 space-y-2">
            <p class="text-[11px] font-semibold text-slate-400 uppercase tracking-wider px-1">Stok Per Kategori</p>
            <div id="categoryBreakdown" class="grid grid-cols-2 sm:grid-cols-3 gap-2">
                <!-- Dynamically populated via JavaScript -->
            </div>
        </div>

        <!-- iOS Search Bar -->
        <div class="relative flex items-center gap-2">
            <div class="relative flex-1">
                <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-slate-400 text-sm"></i>
                <input type="text" id="searchInput" oninput="renderTable()" placeholder="Cari nama, part number, atau mobil..."
                    class="w-full bg-[#E3E3E8] text-slate-900 text-sm pl-9 pr-8 py-2 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 transition placeholder:text-slate-500">
            </div>
        </div>

        <!-- Filter Chips (Horizontal Scroll iOS Style) -->
        <div class="flex items-center gap-2 overflow-x-auto no-scrollbar py-1">
            <select id="filterCategory" onchange="renderTable()" class="bg-white text-slate-700 text-xs font-semibold px-3 py-2 rounded-xl border border-slate-200 shadow-sm focus:outline-none">
                <option value="">Semua Kategori</option>
                <option value="Mesin">Mesin & Filter</option>
                <option value="Kaki-kaki">Kaki-kaki & Suspensi</option>
                <option value="Rem">Pengereman</option>
                <option value="Kelistrikan">Kelistrikan & Aki</option>
                <option value="Bodi & Interior">Bodi & Interior</option>
                <option value="Lainnya">Lainnya</option>
            </select>
            <select id="filterBrand" onchange="renderTable()" class="bg-white text-slate-700 text-xs font-semibold px-3 py-2 rounded-xl border border-slate-200 shadow-sm focus:outline-none">
                <option value="">Semua Merk</option>
            </select>
        </div>

        <!-- Items Group Card -->
        <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
            <div id="sparepartList" class="divide-y divide-slate-100">
                <!-- Content dynamically inserted via JavaScript -->
            </div>

            <!-- Empty state -->
            <div id="emptyState" class="hidden p-8 text-center">
                <div class="text-slate-300 text-4xl mb-2"><i class="fa-solid fa-box-open"></i></div>
                <p class="text-slate-500 text-xs font-medium">Tidak ada sparepart ditemukan.</p>
            </div>
        </div>

    </main>

    <!-- iOS Bottom Dock Bar -->
    <div class="fixed bottom-0 left-0 right-0 z-40 bg-white/80 ios-blur border-t border-slate-200/80 pb-safe">
        <div class="app-container max-w-md mx-auto px-6 py-2.5 flex justify-around items-center text-center transition-all duration-300">
            <button class="flex flex-col items-center gap-0.5 text-blue-600">
                <i class="fa-solid fa-layer-group text-lg"></i>
                <span class="text-[10px] font-medium">Inventori</span>
            </button>
            <button onclick="openModal('add')" class="flex flex-col items-center gap-0.5 text-slate-400 hover:text-slate-600 transition">
                <i class="fa-solid fa-circle-plus text-lg"></i>
                <span class="text-[10px] font-medium">Tambah</span>
            </button>
            <button onclick="openSettingsModal()" class="flex flex-col items-center gap-0.5 text-slate-400 hover:text-slate-600 transition">
                <i class="fa-solid fa-gear text-lg"></i>
                <span class="text-[10px] font-medium">Pengaturan</span>
            </button>
        </div>
    </div>

    <!-- Settings Modal -->
    <div id="settingsModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center hidden transition-opacity">
        <div class="bg-[#F2F2F7] w-full max-w-lg rounded-t-3xl sm:rounded-2xl shadow-2xl overflow-hidden max-h-[90vh] flex flex-col">
            <!-- Modal Header -->
            <div class="px-5 py-3.5 bg-white border-b border-slate-200/80 flex justify-between items-center sticky top-0 z-10">
                <button type="button" onclick="closeSettingsModal()" class="text-blue-600 text-sm font-normal">Tutup</button>
                <h3 class="font-semibold text-slate-900 text-sm">Pengaturan Aplikasi</h3>
                <button type="button" onclick="saveSettings()" class="text-blue-600 font-bold text-sm">Simpan</button>
            </div>

            <!-- Settings Form -->
            <div class="p-4 space-y-4 overflow-y-auto">
                
                <!-- Theme / Dark Mode Options -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-2">Tema & Warna Tampilan</label>
                        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2">
                            <button type="button" onclick="setTheme('light')" id="themeBtnLight" class="p-2 rounded-xl border text-center transition flex flex-col items-center gap-1">
                                <span class="w-6 h-6 rounded-full bg-[#F2F2F7] border border-slate-300 flex items-center justify-center text-xs text-slate-800"><i class="fa-solid fa-sun"></i></span>
                                <span class="text-[11px] font-semibold">Light</span>
                            </button>
                            <button type="button" onclick="setTheme('dark')" id="themeBtnDark" class="p-2 rounded-xl border text-center transition flex flex-col items-center gap-1">
                                <span class="w-6 h-6 rounded-full bg-slate-900 border border-slate-700 flex items-center justify-center text-xs text-white"><i class="fa-solid fa-moon"></i></span>
                                <span class="text-[11px] font-semibold">Dark Mode</span>
                            </button>
                            <button type="button" onclick="setTheme('navy')" id="themeBtnNavy" class="p-2 rounded-xl border text-center transition flex flex-col items-center gap-1">
                                <span class="w-6 h-6 rounded-full bg-[#0b132b] border border-blue-900 flex items-center justify-center text-xs text-blue-400"><i class="fa-solid fa-shield-halved"></i></span>
                                <span class="text-[11px] font-semibold">Navy Dark</span>
                            </button>
                            <button type="button" onclick="setTheme('emerald')" id="themeBtnEmerald" class="p-2 rounded-xl border text-center transition flex flex-col items-center gap-1">
                                <span class="w-6 h-6 rounded-full bg-[#062c20] border border-emerald-900 flex items-center justify-center text-xs text-emerald-400"><i class="fa-solid fa-leaf"></i></span>
                                <span class="text-[11px] font-semibold">Emerald Dark</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Container Width Customization -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-2">Lebar Maksimal Tampilan</label>
                        <div class="grid grid-cols-2 sm:grid-cols-4 gap-2">
                            <button type="button" onclick="setAppWidth('compact')" id="widthBtnCompact" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Ramping (sm)</button>
                            <button type="button" onclick="setAppWidth('standard')" id="widthBtnStandard" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Standar (md)</button>
                            <button type="button" onclick="setAppWidth('tablet')" id="widthBtnTablet" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Tablet (lg)</button>
                            <button type="button" onclick="setAppWidth('full')" id="widthBtnFull" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Penuh (100%)</button>
                        </div>
                    </div>
                </div>

                <!-- Side Margins / Padding Customization -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-2">Jarak Pinggir Kiri & Kanan (Side Padding)</label>
                        <div class="grid grid-cols-4 gap-2">
                            <button type="button" onclick="setAppPadding('tight')" id="padBtnTight" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Rapat (px-2)</button>
                            <button type="button" onclick="setAppPadding('normal')" id="padBtnNormal" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Normal (px-4)</button>
                            <button type="button" onclick="setAppPadding('wide')" id="padBtnWide" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Longgar (px-6)</button>
                            <button type="button" onclick="setAppPadding('extra')" id="padBtnExtra" class="py-2 px-1 text-center text-xs font-semibold rounded-lg border border-slate-200 transition">Sangat Longgar (px-8)</button>
                        </div>
                    </div>
                </div>

                <!-- Font Size Customization -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-2">Ukuran Font Aplikasi</label>
                        <div class="grid grid-cols-3 gap-2">
                            <button type="button" onclick="setFontSize('sm')" id="fontBtnSm" class="py-2 text-xs font-semibold rounded-lg border border-slate-200 hover:bg-slate-50 transition">Kecil</button>
                            <button type="button" onclick="setFontSize('base')" id="fontBtnBase" class="py-2 text-xs font-semibold rounded-lg border border-slate-200 hover:bg-slate-50 transition">Normal</button>
                            <button type="button" onclick="setFontSize('lg')" id="fontBtnLg" class="py-2 text-xs font-semibold rounded-lg border border-slate-200 hover:bg-slate-50 transition">Besar</button>
                        </div>
                    </div>
                </div>

                <!-- Header Name Customization -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Nama Toko / Header</label>
                        <input type="text" id="settingHeaderTitle" placeholder="Blantikautoparts" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Sub-judul / Tagline</label>
                        <input type="text" id="settingHeaderSubtitle" placeholder="Japan Spec iOS" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                    </div>
                </div>

                <!-- Display Preferences -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-xs font-medium text-slate-800">Tampilkan Modal / Hpp</p>
                            <p class="text-[10px] text-slate-400">Tampilkan modal pada ringkasan item</p>
                        </div>
                        <input type="checkbox" id="settingShowBuyPrice" onchange="renderTable()" class="w-4 h-4 accent-blue-600 rounded">
                    </div>
                </div>

                <!-- Data Management -->
                <div class="bg-white rounded-xl p-3 space-y-2 shadow-sm border border-slate-100">
                    <button type="button" onclick="resetData()" class="w-full text-left text-xs font-semibold text-red-600 py-1 flex items-center justify-between">
                        <span>Reset Data Sparepart ke Default</span>
                        <i class="fa-solid fa-rotate-right text-xs"></i>
                    </button>
                </div>

            </div>
        </div>
    </div>

    <!-- Modal (iOS Sheet) -->
    <div id="partModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center hidden transition-opacity">
        <div class="bg-[#F2F2F7] w-full max-w-md rounded-t-3xl sm:rounded-2xl shadow-2xl overflow-hidden max-h-[90vh] flex flex-col transform transition-transform">
            <!-- Modal Header -->
            <div class="px-5 py-3.5 bg-white border-b border-slate-200/80 flex justify-between items-center sticky top-0 z-10">
                <button type="button" onclick="closeModal()" class="text-blue-600 text-sm font-normal">Batal</button>
                <h3 id="modalTitle" class="font-semibold text-slate-900 text-sm">Tambah Sparepart</h3>
                <button type="button" onclick="document.getElementById('submitBtn').click()" class="text-blue-600 font-bold text-sm">Simpan</button>
            </div>

            <!-- Modal Form -->
            <form id="partForm" onsubmit="handleFormSubmit(event)" class="p-4 space-y-4 overflow-y-auto">
                <input type="hidden" id="partId">
                
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Nama Sparepart</label>
                        <input type="text" id="partName" required placeholder="Filter Oli / Kampas Rem" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Kode Part / No. Part</label>
                        <input type="text" id="partCode" placeholder="04465-YZZE1" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent font-mono">
                    </div>
                </div>

                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Kendaraan / Mobil</label>
                        <input type="text" id="partCar" required placeholder="Avanza / Jazz / Xpander" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Tahun Mobil</label>
                        <input type="text" id="partYear" placeholder="2012 - 2021" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                    </div>
                </div>

                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Kategori</label>
                        <select id="partCategory" required class="w-full text-sm text-slate-900 focus:outline-none bg-transparent font-medium">
                            <option value="Mesin">Mesin & Filter</option>
                            <option value="Kaki-kaki">Kaki-kaki & Suspensi</option>
                            <option value="Rem">Pengereman</option>
                            <option value="Kelistrikan">Kelistrikan & Aki</option>
                            <option value="Bodi & Interior">Bodi & Interior</option>
                            <option value="Lainnya">Lainnya</option>
                        </select>
                    </div>
                    <hr class="border-slate-100">
                    <div id="brandContainer">
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Merk / Brand</label>
                        <input type="text" id="partBrand" required placeholder="Denso / NGK / KYB" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                    </div>
                </div>

                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div class="flex justify-between items-center" id="stockContainer">
                        <label class="text-xs font-medium text-slate-600">Jumlah Stok</label>
                        <input type="number" id="partStock" min="0" required placeholder="0" 
                            class="w-20 text-right text-sm font-bold text-slate-900 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div class="flex justify-between items-center" id="buyPriceContainer">
                        <label class="text-xs font-medium text-slate-600">Harga Beli (Modal)</label>
                        <input type="number" id="partBuyPrice" min="0" required placeholder="0" 
                            class="w-32 text-right text-sm font-semibold text-slate-700 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-600">Harga Jual</label>
                        <input type="number" id="partSellPrice" min="0" required placeholder="0" 
                            class="w-32 text-right text-sm font-bold text-blue-600 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-600">Jasa / Ongkos Pasang (Opsional)</label>
                        <input type="number" id="partServiceFee" min="0" placeholder="0" 
                            class="w-32 text-right text-sm font-semibold text-amber-600 focus:outline-none bg-transparent">
                    </div>
                </div>

                <button type="submit" id="submitBtn" class="hidden"></button>
            </form>
        </div>
    </div>

    <!-- Toast Banner -->
    <div id="toast" class="fixed top-4 left-1/2 -translate-x-1/2 z-50 bg-slate-900/90 backdrop-blur-md text-white text-xs font-medium px-4 py-2.5 rounded-full shadow-lg transform -translate-y-20 opacity-0 transition-all duration-300 flex items-center gap-2">
        <span id="toastIcon" class="text-emerald-400"><i class="fa-solid fa-circle-check"></i></span>
        <span id="toastMessage">Berhasil menyimpan data</span>
    </div>

    <script>
        let appSettings = JSON.parse(localStorage.getItem('app_settings')) || {
            fontSize: 'base',
            headerTitle: 'Blantikautoparts',
            headerSubtitle: 'Japan Spec iOS',
            showBuyPrice: false,
            theme: 'light',
            appWidth: 'standard',
            appPadding: 'normal'
        };

        // Initial Default Spareparts Data
        const defaultParts = [
            { id: 1, name: "Kampas Rem Depan Set", code: "04465-YZZE1", year: "2012 - 2021", category: "Rem", brand: "Akimbo / Genuine", car: "Toyota Avanza / Xenia", stock: 8, buyPrice: 175000, sellPrice: 240000, serviceFee: 50000 },
            { id: 2, name: "Filter Oli Mesin", code: "15400-RTA-003", year: "2014 - 2022", category: "Mesin", brand: "Denso", car: "Honda Jazz / Brio", stock: 15, buyPrice: 35000, sellPrice: 55000, serviceFee: 20000 },
            { id: 3, name: "Busi Iridium Standard", code: "IZFR6K11", year: "2017 - 2023", category: "Kelistrikan", brand: "NGK", car: "Mitsubishi Xpander", stock: 2, buyPrice: 95000, sellPrice: 135000, serviceFee: 30000 },
            { id: 4, name: "Link Stabilizer Depan", code: "SL-S080", year: "2012 - 2018", category: "Kaki-kaki", brand: "555 / Japan", car: "Suzuki Ertiga", stock: 4, buyPrice: 110000, sellPrice: 165000, serviceFee: 50000 },
            { id: 5, name: "Shock Absorber Belakang", code: "48530-09L10", year: "2016 - 2023", category: "Kaki-kaki", brand: "KYB (Kayaba)", car: "Toyota Innova Reborn", stock: 1, buyPrice: 450000, sellPrice: 620000, serviceFee: 100000 }
        ];

        let spareparts = JSON.parse(localStorage.getItem('spareparts_data')) || defaultParts;

        function applySettings() {
            // Apply Header Text
            const titleElem = document.querySelector('header h1');
            const subElem = document.querySelector('header p');
            if (titleElem) titleElem.textContent = appSettings.headerTitle || 'Blantikautoparts';
            if (subElem) subElem.textContent = appSettings.headerSubtitle || 'Japan Spec iOS';

            // Apply Theme
            const body = document.body;
            body.classList.remove('theme-light', 'theme-dark', 'theme-navy', 'theme-emerald');
            body.classList.add(`theme-${appSettings.theme || 'light'}`);

            // Apply App Width & Padding Classes to all container elements
            const containers = document.querySelectorAll('.app-container');
            containers.forEach(el => {
                // Width
                el.classList.remove('max-w-sm', 'max-w-md', 'max-w-4xl', 'max-w-full');
                if (appSettings.appWidth === 'compact') {
                    el.classList.add('max-w-sm');
                } else if (appSettings.appWidth === 'tablet') {
                    el.classList.add('max-w-4xl');
                } else if (appSettings.appWidth === 'full') {
                    el.classList.add('max-w-full');
                } else {
                    el.classList.add('max-w-md'); // default standard
                }

                // Side Padding / Margin
                el.classList.remove('px-2', 'px-4', 'px-6', 'px-8');
                if (appSettings.appPadding === 'tight') {
                    el.classList.add('px-2');
                } else if (appSettings.appPadding === 'wide') {
                    el.classList.add('px-6');
                } else if (appSettings.appPadding === 'extra') {
                    el.classList.add('px-8');
                } else {
                    el.classList.add('px-4'); // default normal
                }
            });

            // Apply Font Size
            body.classList.remove('text-xs', 'text-sm', 'text-base');
            if (appSettings.fontSize === 'sm') {
                body.style.fontSize = '13px';
            } else if (appSettings.fontSize === 'lg') {
                body.style.fontSize = '16px';
            } else {
                body.style.fontSize = '14px';
            }

            // Update Settings Modal Fields
            document.getElementById('settingHeaderTitle').value = appSettings.headerTitle || '';
            document.getElementById('settingHeaderSubtitle').value = appSettings.headerSubtitle || '';
            document.getElementById('settingShowBuyPrice').checked = appSettings.showBuyPrice || false;

            // Update Theme Buttons UI
            ['light', 'dark', 'navy', 'emerald'].forEach(t => {
                const btn = document.getElementById(`themeBtn${t.charAt(0).toUpperCase() + t.slice(1)}`);
                if (btn) {
                    if (appSettings.theme === t) {
                        btn.className = "p-2 rounded-xl border border-blue-600 bg-blue-50/20 text-blue-600 text-center transition flex flex-col items-center gap-1 shadow-sm";
                    } else {
                        btn.className = "p-2 rounded-xl border border-slate-200 text-center transition flex flex-col items-center gap-1 hover:bg-slate-50";
                    }
                }
            });

            // Update App Width Buttons UI
            ['compact', 'standard', 'tablet', 'full'].forEach(w => {
                const btn = document.getElementById(`widthBtn${w.charAt(0).toUpperCase() + w.slice(1)}`);
                if (btn) {
                    if (appSettings.appWidth === w) {
                        btn.className = "py-2 px-1 text-center text-xs font-semibold rounded-lg bg-blue-600 text-white border border-blue-600 transition shadow-sm";
                    } else {
                        btn.className = "py-2 px-1 text-center text-xs font-semibold rounded-lg bg-white text-slate-700 border border-slate-200 hover:bg-slate-50 transition";
                    }
                }
            });

            // Update Side Padding Buttons UI
            ['tight', 'normal', 'wide', 'extra'].forEach(p => {
                const btn = document.getElementById(`padBtn${p.charAt(0).toUpperCase() + p.slice(1)}`);
                if (btn) {
                    if ((appSettings.appPadding || 'normal') === p) {
                        btn.className = "py-2 px-1 text-center text-xs font-semibold rounded-lg bg-blue-600 text-white border border-blue-600 transition shadow-sm";
                    } else {
                        btn.className = "py-2 px-1 text-center text-xs font-semibold rounded-lg bg-white text-slate-700 border border-slate-200 hover:bg-slate-50 transition";
                    }
                }
            });

            // Update Font Choice Buttons UI
            ['sm', 'base', 'lg'].forEach(size => {
                const btn = document.getElementById(`fontBtn${size.charAt(0).toUpperCase() + size.slice(1)}`);
                if (btn) {
                    if (appSettings.fontSize === size) {
                        btn.className = "py-2 text-xs font-semibold rounded-lg bg-blue-600 text-white border border-blue-600 transition shadow-sm";
                    } else {
                        btn.className = "py-2 text-xs font-semibold rounded-lg bg-white text-slate-700 border border-slate-200 hover:bg-slate-50 transition";
                    }
                }
            });
        }

        function setTheme(theme) {
            appSettings.theme = theme;
            applySettings();
        }

        function setAppWidth(width) {
            appSettings.appWidth = width;
            applySettings();
        }

        function setAppPadding(padding) {
            appSettings.appPadding = padding;
            applySettings();
        }

        function setFontSize(size) {
            appSettings.fontSize = size;
            applySettings();
        }

        function openSettingsModal() {
            applySettings();
            document.getElementById('settingsModal').classList.remove('hidden');
        }

        function closeSettingsModal() {
            document.getElementById('settingsModal').classList.add('hidden');
        }

        function saveSettings() {
            appSettings.headerTitle = document.getElementById('settingHeaderTitle').value.trim() || 'Blantikautoparts';
            appSettings.headerSubtitle = document.getElementById('settingHeaderSubtitle').value.trim() || 'Japan Spec iOS';
            appSettings.showBuyPrice = document.getElementById('settingShowBuyPrice').checked;

            localStorage.setItem('app_settings', JSON.stringify(appSettings));
            applySettings();
            renderTable();
            closeSettingsModal();
            showToast("Pengaturan berhasil disimpan!");
        }

        function formatRupiah(number) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits: 0 }).format(number);
        }

        function updateBrandFilterOptions() {
            const filterBrandSelect = document.getElementById('filterBrand');
            const currentSelected = filterBrandSelect.value;
            const brands = [...new Set(spareparts.map(p => p.brand || 'Lainnya'))].sort();
            
            filterBrandSelect.innerHTML = '<option value="">Semua Merk</option>';
            brands.forEach(b => {
                if(b) {
                    const opt = document.createElement('option');
                    opt.value = b;
                    opt.textContent = b;
                    if(b === currentSelected) opt.selected = true;
                    filterBrandSelect.appendChild(opt);
                }
            });
        }

        function showToast(message, isError = false) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMessage');
            const toastIcon = document.getElementById('toastIcon');
            
            toastMsg.textContent = message;
            toastIcon.innerHTML = isError ? '<i class="fa-solid fa-circle-exclamation text-amber-400"></i>' : '<i class="fa-solid fa-circle-check text-emerald-400"></i>';
            
            toast.classList.remove('-translate-y-20', 'opacity-0');
            setTimeout(() => {
                toast.classList.add('-translate-y-20', 'opacity-0');
            }, 3000);
        }

        function saveData() {
            localStorage.setItem('spareparts_data', JSON.stringify(spareparts));
        }

        function renderTable() {
            updateBrandFilterOptions();

            const container = document.getElementById('sparepartList');
            const emptyState = document.getElementById('emptyState');
            const searchVal = document.getElementById('searchInput').value.toLowerCase();
            const categoryVal = document.getElementById('filterCategory').value;
            const brandVal = document.getElementById('filterBrand').value;

            container.innerHTML = '';

            // Calculate item counts per category
            const categoryCounts = {
                "Mesin": 0,
                "Kaki-kaki": 0,
                "Rem": 0,
                "Kelistrikan": 0,
                "Bodi & Interior": 0,
                "Lainnya": 0
            };

            let totalUnits = 0;
            spareparts.forEach(item => {
                const cat = item.category || 'Lainnya';
                const count = Number(item.stock) || 0;
                if (categoryCounts.hasOwnProperty(cat)) {
                    categoryCounts[cat] += count;
                } else {
                    categoryCounts["Lainnya"] += count;
                }
                totalUnits += count;
            });

            // Render category breakdown UI
            const breakdownContainer = document.getElementById('categoryBreakdown');
            if (breakdownContainer) {
                const catIcons = {
                    "Mesin": "fa-gears",
                    "Kaki-kaki": "fa-car-side",
                    "Rem": "fa-circle-dot",
                    "Kelistrikan": "fa-bolt",
                    "Bodi & Interior": "fa-car-rear",
                    "Lainnya": "fa-ellipsis"
                };

                breakdownContainer.innerHTML = Object.entries(categoryCounts).map(([cat, count]) => `
                    <div onclick="filterByCategory('${cat}')" class="bg-slate-50 hover:bg-blue-50/60 active:scale-95 transition cursor-pointer p-2 rounded-xl border border-slate-100 flex items-center justify-between">
                        <div class="flex items-center gap-1.5 min-w-0">
                            <i class="fa-solid ${catIcons[cat] || 'fa-box'} text-xs text-blue-500"></i>
                            <span class="text-[11px] font-medium text-slate-600 truncate">${cat}</span>
                        </div>
                        <span class="text-xs font-bold text-slate-900 ml-1">${count}</span>
                    </div>
                `).join('');
            }

            const filtered = spareparts.filter(item => {
                const itemBrand = item.brand || '';
                const itemCode = item.code || '';
                const itemYear = item.year || '';
                const matchesSearch = item.name.toLowerCase().includes(searchVal) || 
                                      item.car.toLowerCase().includes(searchVal) || 
                                      item.category.toLowerCase().includes(searchVal) ||
                                      itemCode.toLowerCase().includes(searchVal) ||
                                      itemYear.toLowerCase().includes(searchVal) ||
                                      itemBrand.toLowerCase().includes(searchVal);
                const matchesCat = categoryVal === "" || item.category === categoryVal;
                const matchesBrand = brandVal === "" || itemBrand === brandVal;
                return matchesSearch && matchesCat && matchesBrand;
            });

            document.getElementById('statTotalItems').innerHTML = `${spareparts.length} <span class="text-xs font-normal text-slate-500">Jenis (${totalUnits} Stok Unit)</span>`;

            if (filtered.length === 0) {
                emptyState.classList.remove('hidden');
                return;
            } else {
                emptyState.classList.add('hidden');
            }

            filtered.forEach(item => {
                const card = document.createElement('div');
                card.className = "p-3.5 hover:bg-slate-50 transition flex items-center justify-between gap-3";
                const serviceFeeVal = Number(item.serviceFee) || 0;
                
                card.innerHTML = `
                    <div class="flex-1 min-w-0">
                        <div class="flex items-center gap-1.5 mb-0.5">
                            <h3 class="font-semibold text-slate-900 text-sm truncate">${item.name}</h3>
                            ${item.code ? `<span class="bg-slate-100 text-slate-600 text-[10px] font-mono font-medium px-1.5 py-0.2 rounded border border-slate-200">${item.code}</span>` : ''}
                        </div>
                        <p class="text-xs text-slate-500 truncate flex items-center gap-1">
                            <i class="fa-solid fa-car text-[10px] text-slate-400"></i> ${item.car} ${item.year ? `(${item.year})` : ''}
                        </p>
                        <div class="flex flex-col gap-0.5 mt-1.5">
                            <div class="flex items-center gap-2 flex-wrap">
                                <span class="text-[10px] font-semibold text-slate-500 bg-slate-100 px-2 py-0.5 rounded-full">${item.brand || 'Lainnya'}</span>
                                <span class="text-xs font-bold text-blue-600">${formatRupiah(item.sellPrice)}</span>
                                ${appSettings.showBuyPrice ? `<span class="text-[10px] font-medium text-slate-400">(Modal: ${formatRupiah(item.buyPrice)})</span>` : ''}
                            </div>
                            ${serviceFeeVal > 0 ? `
                                <div class="text-[11px] font-medium text-amber-600/90 flex items-center gap-1 mt-0.5">
                                    <i class="fa-solid fa-wrench text-[9px]"></i> Jasa: ${formatRupiah(serviceFeeVal)}
                                </div>
                            ` : ''}
                        </div>
                    </div>
                    <div class="flex flex-col items-end gap-2">
                        <span class="text-xs font-bold px-2 py-0.5 rounded-full ${item.stock < 3 ? 'bg-amber-100 text-amber-800' : 'bg-emerald-100 text-emerald-800'}">
                            ${item.stock} unit
                        </span>
                        <div class="flex items-center gap-1">
                            <button onclick="openModal('edit', ${item.id})" class="p-1 text-slate-400 hover:text-blue-600 transition">
                                <i class="fa-solid fa-pen-to-square text-xs"></i>
                            </button>
                            <button onclick="deletePart(${item.id})" class="p-1 text-slate-400 hover:text-red-500 transition">
                                <i class="fa-solid fa-trash-can text-xs"></i>
                            </button>
                        </div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function openModal(mode, id = null) {
            const modal = document.getElementById('partModal');
            const title = document.getElementById('modalTitle');
            const form = document.getElementById('partForm');
            
            form.reset();
            document.getElementById('partId').value = '';

            if (mode === 'edit' && id !== null) {
                title.textContent = 'Edit Sparepart';
                const part = spareparts.find(i => i.id === id);
                if (part) {
                    document.getElementById('partId').value = part.id;
                    document.getElementById('partName').value = part.name;
                    document.getElementById('partCode').value = part.code || '';
                    document.getElementById('partYear').value = part.year || '';
                    document.getElementById('partCategory').value = part.category;
                    document.getElementById('partBrand').value = part.brand || '';
                    document.getElementById('partCar').value = part.car;
                    document.getElementById('partStock').value = part.stock;
                    document.getElementById('partBuyPrice').value = part.buyPrice;
                    document.getElementById('partSellPrice').value = part.sellPrice;
                    document.getElementById('partServiceFee').value = part.serviceFee || '';
                }
            } else {
                title.textContent = 'Tambah Sparepart';
            }

            modal.classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('partModal').classList.add('hidden');
        }

        function handleFormSubmit(e) {
            e.preventDefault();
            const id = document.getElementById('partId').value;
            const name = document.getElementById('partName').value.trim();
            const code = document.getElementById('partCode').value.trim();
            const year = document.getElementById('partYear').value.trim();
            const category = document.getElementById('partCategory').value;
            const brand = document.getElementById('partBrand').value.trim();
            const car = document.getElementById('partCar').value.trim();
            const stock = parseInt(document.getElementById('partStock').value) || 0;
            const buyPrice = parseInt(document.getElementById('partBuyPrice').value) || 0;
            const sellPrice = parseInt(document.getElementById('partSellPrice').value) || 0;
            const serviceFee = parseInt(document.getElementById('partServiceFee').value) || 0;

            if (sellPrice < buyPrice) {
                showToast("Peringatan: Harga jual lebih rendah dari modal!", true);
            }

            if (id) {
                const index = spareparts.findIndex(i => i.id == id);
                if (index !== -1) {
                    spareparts[index] = { id: Number(id), name, code, year, category, brand, car, stock, buyPrice, sellPrice, serviceFee };
                    showToast("Data berhasil diperbarui!");
                }
            } else {
                const newId = spareparts.length > 0 ? Math.max(...spareparts.map(i => i.id)) + 1 : 1;
                spareparts.push({ id: newId, name, code, year, category, brand, car, stock, buyPrice, sellPrice, serviceFee });
                showToast("Item berhasil ditambahkan!");
            }

            saveData();
            renderTable();
            closeModal();
        }

        function deletePart(id) {
            spareparts = spareparts.filter(i => i.id !== id);
            saveData();
            renderTable();
            showToast("Item dihapus.");
        }

        function resetData() {
            spareparts = [...defaultParts];
            saveData();
            renderTable();
            showToast("Data direset.");
        }

        function filterByCategory(catName) {
            const select = document.getElementById('filterCategory');
            if (select) {
                select.value = select.value === catName ? '' : catName;
                renderTable();
            }
        }

        window.onload = function() {
            applySettings();
            renderTable();
        }
    </script>
</body>
</html>
