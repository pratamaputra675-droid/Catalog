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
    <!-- HTML5-QRCode Scanner Library -->
    <script src="https://unpkg.com/html5-qrcode"></script>
    <style>
        body { 
            font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', 'Inter', sans-serif;
            -webkit-tap-highlight-color: transparent;
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

        /* Dynamic Theme Variables */
        :root {
            --primary-color: #2563eb;
            --primary-hover: #1d4ed8;
            --bg-color: #F2F2F7;
            --card-bg: #ffffff;
            --text-main: #0f172a;
            --text-muted: #64748b;
            --border-color: #f1f5f9;
            --header-bg: rgba(242, 242, 247, 0.8);
            --header-text: #0f172a;
        }

        /* Dark Mode Theme Default Fallbacks */
        body.theme-dark {
            --bg-color: #000000;
            --card-bg: #1c1c1e;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border-color: #2c2c2e;
            --header-bg: rgba(0, 0, 0, 0.8);
            --header-text: #f8fafc;
        }

        /* Accent Color Schemes */
        body.accent-emerald { --primary-color: #10b981; --primary-hover: #059669; }
        body.accent-purple { --primary-color: #8b5cf6; --primary-hover: #7c3aed; }
        body.accent-orange { --primary-color: #f97316; --primary-hover: #ea580c; }

        /* Custom Theme Overrides applied via JS */
        body {
            background-color: var(--custom-bg-color, var(--bg-color)) !important;
            color: var(--custom-text-color, var(--text-main)) !important;
        }
        .theme-card {
            background-color: var(--card-bg);
            border-color: var(--border-color);
        }
        .theme-accent-bg {
            background-color: var(--primary-color) !important;
        }
        .theme-accent-text {
            color: var(--primary-color) !important;
        }
        
        /* Custom Header Colors */
        header.custom-header-style {
            background-color: var(--custom-header-bg, var(--header-bg)) !important;
        }
        .custom-header-text {
            color: var(--custom-header-text-color, var(--header-text)) !important;
        }

        /* App Outer Layout Spacing */
        .app-layout-padding {
            padding-left: var(--custom-app-padding, 1rem) !important;
            padding-right: var(--custom-app-padding, 1rem) !important;
        }

        /* Font Sizes */
        body.text-size-sm { font-size: 13px; }
        body.text-size-md { font-size: 15px; }
        body.text-size-lg { font-size: 17px; }

        /* Font Scaling Utilities based on active text size */
        body.text-size-sm .text-xs { font-size: 10px; }
        body.text-size-sm .text-sm { font-size: 12px; }
        body.text-size-sm .text-base { font-size: 14px; }
        body.text-size-sm .text-xl { font-size: 18px; }

        body.text-size-lg .text-xs { font-size: 14px; }
        body.text-size-lg .text-sm { font-size: 16px; }
        body.text-size-lg .text-base { font-size: 18px; }
        body.text-size-lg .text-xl { font-size: 24px; }
    </style>
</head>
<body class="antialiased min-h-screen pb-28 selection:bg-blue-500 selection:text-white transition-colors duration-200">

    <!-- iOS Header / Navigation Bar -->
    <header class="sticky top-0 z-40 ios-blur border-b border-slate-200/80 dark:border-zinc-800/80 pt-safe custom-header-style">
        <div class="max-w-md mx-auto app-layout-padding py-3 flex items-center justify-between">
            <div class="flex items-center gap-2.5">
                <div id="appHeaderLogo" class="theme-accent-bg text-white w-9 h-9 rounded-full flex items-center justify-center font-bold text-base shadow-sm">
                    <i class="fa-solid fa-car"></i>
                </div>
                <div>
                    <h1 class="text-base font-bold tracking-tight leading-none custom-header-text" id="appHeaderTitle">Blantikautoparts</h1>
                    <p class="text-[10px] theme-accent-text font-semibold tracking-wider uppercase mt-0.5" id="appHeaderSubtitle">Japan Spec iOS</p>
                </div>
            </div>
            <div class="flex items-center gap-2">
                <button onclick="openSettingsModal()" class="w-8 h-8 rounded-full flex items-center justify-center custom-header-text opacity-70 hover:opacity-100 transition">
                    <i class="fa-solid fa-gear text-base"></i>
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-md mx-auto app-layout-padding pt-4 space-y-4">
        
        <!-- Total Card -->
        <div class="theme-card rounded-2xl p-4 shadow-sm border flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-blue-50 dark:bg-zinc-800 theme-accent-text flex items-center justify-center text-lg">
                    <i class="fa-solid fa-boxes-stacked"></i>
                </div>
                <div>
                    <p class="text-xs font-medium text-slate-400">Total Sparepart</p>
                    <h2 class="text-xl font-bold" id="statTotalItems">0 <span class="text-xs font-normal text-slate-500">Item</span></h2>
                </div>
            </div>
            <button onclick="resetData()" class="p-2 text-slate-400 hover:text-red-500 active:scale-90 transition" title="Reset Data">
                <i class="fa-solid fa-rotate-right text-sm"></i>
            </button>
        </div>

        <!-- Category Summary Accordion/Card -->
        <div class="theme-card rounded-2xl p-4 shadow-sm border">
            <button onclick="toggleCategoryStats()" class="w-full flex items-center justify-between text-left">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-chart-pie theme-accent-text text-sm"></i>
                    <span class="text-xs font-bold uppercase tracking-wider text-slate-500">Stok Per Kategori</span>
                </div>
                <i id="catStatsChevron" class="fa-solid fa-chevron-down text-xs text-slate-400 transition-transform duration-200"></i>
            </button>
            <div id="categoryStatsContainer" class="mt-3 grid grid-cols-2 gap-2 text-xs">
                <!-- Dynamically Populated -->
            </div>
        </div>

        <!-- iOS Search Bar -->
        <div class="relative flex items-center gap-2">
            <div class="relative flex-1">
                <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-slate-400 text-sm"></i>
                <input type="text" id="searchInput" oninput="renderTable()" placeholder="Cari nama, part number, atau mobil..."
                    class="w-full bg-[#E3E3E8] dark:bg-zinc-800 dark:text-white text-sm pl-9 pr-8 py-2 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 transition placeholder:text-slate-500">
            </div>
            <button onclick="openScannerModal()" class="theme-accent-bg text-white w-9 h-9 rounded-xl flex items-center justify-center shadow-sm transition active:scale-95">
                <i class="fa-solid fa-barcode text-base"></i>
            </button>
        </div>

        <!-- Filter Chips (Horizontal Scroll iOS Style) -->
        <div class="flex items-center gap-2 overflow-x-auto no-scrollbar py-1">
            <select id="filterCategory" onchange="renderTable()" class="theme-card text-xs font-semibold px-3 py-2 rounded-xl border shadow-sm focus:outline-none">
                <option value="">Semua Kategori</option>
                <option value="Mesin">Mesin & Filter</option>
                <option value="Kaki-kaki">Kaki-kaki & Suspensi</option>
                <option value="Rem">Pengereman</option>
                <option value="Kelistrikan">Kelistrikan & Aki</option>
                <option value="Bodi & Interior">Bodi & Interior</option>
                <option value="Lainnya">Lainnya</option>
            </select>
            <select id="filterBrand" onchange="renderTable()" class="theme-card text-xs font-semibold px-3 py-2 rounded-xl border shadow-sm focus:outline-none">
                <option value="">Semua Merk</option>
            </select>
        </div>

        <!-- Items Group Card -->
        <div class="theme-card rounded-2xl shadow-sm border overflow-hidden">
            <div id="sparepartList" class="divide-y divide-slate-100 dark:divide-zinc-800">
                <!-- Content dynamically inserted via JavaScript -->
            </div>

            <!-- Empty state -->
            <div id="emptyState" class="hidden p-8 text-center">
                <div class="text-slate-300 dark:text-zinc-700 text-4xl mb-2"><i class="fa-solid fa-box-open"></i></div>
                <p class="text-slate-500 text-xs font-medium">Tidak ada sparepart ditemukan.</p>
            </div>
        </div>
    </main>

    <!-- iOS Bottom Dock Bar -->
    <div class="fixed bottom-0 left-0 right-0 z-40 bg-white/80 dark:bg-zinc-900/80 ios-blur border-t border-slate-200/80 dark:border-zinc-800 pb-safe">
        <div class="max-w-md mx-auto px-4 py-2 flex justify-around items-center text-center">
            <button class="flex flex-col items-center gap-1 theme-accent-text py-1 px-2">
                <i class="fa-solid fa-layer-group text-lg"></i>
                <span class="text-[10px] font-medium">Inventori</span>
            </button>
            <button onclick="openScannerModal()" class="flex flex-col items-center gap-1 text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 transition py-1 px-2">
                <i class="fa-solid fa-qrcode text-lg"></i>
                <span class="text-[10px] font-medium">Scan</span>
            </button>
            <button onclick="openSettingsModal()" class="flex flex-col items-center gap-1 text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 transition py-1 px-2">
                <i class="fa-solid fa-gear text-lg"></i>
                <span class="text-[10px] font-medium">Pengaturan</span>
            </button>
            <button onclick="openModal('add')" class="flex flex-col items-center gap-1 text-slate-400 hover:text-slate-600 dark:hover:text-slate-200 transition py-1 px-2">
                <i class="fa-solid fa-circle-plus text-lg theme-accent-text"></i>
                <span class="text-[10px] font-medium">Tambah</span>
            </button>
        </div>
    </div>

    <!-- Modal Settings (iOS Sheet) -->
    <div id="settingsModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center hidden transition-opacity">
        <div class="bg-[#F2F2F7] dark:bg-zinc-900 w-full max-w-md rounded-t-3xl sm:rounded-2xl shadow-2xl overflow-hidden max-h-[90vh] flex flex-col">
            <div class="px-5 py-3.5 theme-card border-b flex justify-between items-center sticky top-0 z-10">
                <h3 class="font-semibold text-sm">Pengaturan & Tampilan</h3>
                <button type="button" onclick="closeSettingsModal()" class="theme-accent-text font-bold text-sm">Selesai</button>
            </div>

            <div class="p-4 space-y-4 overflow-y-auto">
                <!-- Ubah Logo / Ikon Header -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-2">
                    <p class="text-[11px] font-medium text-slate-400 uppercase tracking-wider">Ikon / Logo Header</p>
                    <div class="grid grid-cols-6 gap-2 pt-1" id="logoSelectorContainer">
                        <button onclick="setAppLogo('fa-car')" data-icon="fa-car" class="w-10 h-10 rounded-xl border flex items-center justify-center text-slate-600 dark:text-slate-200 hover:border-blue-500 transition"><i class="fa-solid fa-car text-sm"></i></button>
                        <button onclick="setAppLogo('fa-screwdriver-wrench')" data-icon="fa-screwdriver-wrench" class="w-10 h-10 rounded-xl border flex items-center justify-center text-slate-600 dark:text-slate-200 hover:border-blue-500 transition"><i class="fa-solid fa-screwdriver-wrench text-sm"></i></button>
                        <button onclick="setAppLogo('fa-gears')" data-icon="fa-gears" class="w-10 h-10 rounded-xl border flex items-center justify-center text-slate-600 dark:text-slate-200 hover:border-blue-500 transition"><i class="fa-solid fa-gears text-sm"></i></button>
                        <button onclick="setAppLogo('fa-warehouse')" data-icon="fa-warehouse" class="w-10 h-10 rounded-xl border flex items-center justify-center text-slate-600 dark:text-slate-200 hover:border-blue-500 transition"><i class="fa-solid fa-warehouse text-sm"></i></button>
                        <button onclick="setAppLogo('fa-store')" data-icon="fa-store" class="w-10 h-10 rounded-xl border flex items-center justify-center text-slate-600 dark:text-slate-200 hover:border-blue-500 transition"><i class="fa-solid fa-store text-sm"></i></button>
                        <button onclick="setAppLogo('fa-truck-fast')" data-icon="fa-truck-fast" class="w-10 h-10 rounded-xl border flex items-center justify-center text-slate-600 dark:text-slate-200 hover:border-blue-500 transition"><i class="fa-solid fa-truck-fast text-sm"></i></button>
                    </div>
                </div>

                <!-- Ubah Nama Header / Toko -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-2">
                    <p class="text-[11px] font-medium text-slate-400 uppercase tracking-wider">Identitas Toko / Header</p>
                    <div class="space-y-2">
                        <div>
                            <label class="text-[10px] text-slate-400 block mb-0.5">Nama Toko / Header</label>
                            <input type="text" id="settingHeaderTitle" oninput="updateHeaderTitle(this.value)" placeholder="Nama Toko" class="w-full bg-slate-100 dark:bg-zinc-800 text-xs font-semibold rounded-lg p-2 focus:outline-none">
                        </div>
                        <div>
                            <label class="text-[10px] text-slate-400 block mb-0.5">Sub-judul / Tagline</label>
                            <input type="text" id="settingHeaderSubtitle" oninput="updateHeaderSubtitle(this.value)" placeholder="Tagline" class="w-full bg-slate-100 dark:bg-zinc-800 text-xs font-semibold rounded-lg p-2 focus:outline-none">
                        </div>
                    </div>
                </div>

                <!-- Custom Warna Latar & Teks -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-3">
                    <p class="text-[11px] font-medium text-slate-400 uppercase tracking-wider">Kustomisasi Warna Latar & Teks</p>
                    
                    <div class="grid grid-cols-2 gap-2 text-xs">
                        <div>
                            <label class="text-[10px] text-slate-400 block mb-1">Latar Halaman</label>
                            <div class="flex items-center gap-2 bg-slate-100 dark:bg-zinc-800 p-1.5 rounded-lg border dark:border-zinc-700">
                                <input type="color" id="pickerBgColor" onchange="setCustomColor('bgColor', this.value)" class="w-6 h-6 rounded cursor-pointer border-0 bg-transparent">
                                <span class="text-[11px] font-mono text-slate-600 dark:text-slate-300" id="pickerBgVal">Default</span>
                            </div>
                        </div>
                        <div>
                            <label class="text-[10px] text-slate-400 block mb-1">Teks Utama</label>
                            <div class="flex items-center gap-2 bg-slate-100 dark:bg-zinc-800 p-1.5 rounded-lg border dark:border-zinc-700">
                                <input type="color" id="pickerTextColor" onchange="setCustomColor('textColor', this.value)" class="w-6 h-6 rounded cursor-pointer border-0 bg-transparent">
                                <span class="text-[11px] font-mono text-slate-600 dark:text-slate-300" id="pickerTextVal">Default</span>
                            </div>
                        </div>
                        <div>
                            <label class="text-[10px] text-slate-400 block mb-1">Latar Header</label>
                            <div class="flex items-center gap-2 bg-slate-100 dark:bg-zinc-800 p-1.5 rounded-lg border dark:border-zinc-700">
                                <input type="color" id="pickerHeaderBgColor" onchange="setCustomColor('headerBgColor', this.value)" class="w-6 h-6 rounded cursor-pointer border-0 bg-transparent">
                                <span class="text-[11px] font-mono text-slate-600 dark:text-slate-300" id="pickerHeaderBgVal">Default</span>
                            </div>
                        </div>
                        <div>
                            <label class="text-[10px] text-slate-400 block mb-1">Teks Header</label>
                            <div class="flex items-center gap-2 bg-slate-100 dark:bg-zinc-800 p-1.5 rounded-lg border dark:border-zinc-700">
                                <input type="color" id="pickerHeaderTextColor" onchange="setCustomColor('headerTextColor', this.value)" class="w-6 h-6 rounded cursor-pointer border-0 bg-transparent">
                                <span class="text-[11px] font-mono text-slate-600 dark:text-slate-300" id="pickerHeaderTextVal">Default</span>
                            </div>
                        </div>
                    </div>
                    <button onclick="resetCustomColors()" class="w-full py-1.5 text-[11px] font-semibold text-slate-500 hover:text-red-500 bg-slate-100 dark:bg-zinc-800 rounded-lg border dark:border-zinc-700 transition">
                        Reset Warna Kustom Ke Default
                    </button>
                </div>

                <!-- Mode Gelap / Terang -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-2">
                    <p class="text-[11px] font-medium text-slate-400 uppercase tracking-wider">Mode Tampilan</p>
                    <div class="grid grid-cols-2 gap-2">
                        <button onclick="setThemeMode('light')" id="btnThemeLight" class="py-2 px-3 rounded-lg border text-xs font-semibold flex items-center justify-center gap-2">
                            <i class="fa-solid fa-sun text-amber-500"></i> Mode Terang
                        </button>
                        <button onclick="setThemeMode('dark')" id="btnThemeDark" class="py-2 px-3 rounded-lg border text-xs font-semibold flex items-center justify-center gap-2">
                            <i class="fa-solid fa-moon text-indigo-400"></i> Mode Gelap
                        </button>
                    </div>
                </div>

                <!-- Warna Aksen -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-2">
                    <p class="text-[11px] font-medium text-slate-400 uppercase tracking-wider">Warna Aksen</p>
                    <div class="flex items-center justify-around py-1">
                        <button onclick="setAccentColor('blue')" class="w-8 h-8 rounded-full bg-blue-600 ring-2 ring-offset-2 ring-blue-600 transition active:scale-95"></button>
                        <button onclick="setAccentColor('emerald')" class="w-8 h-8 rounded-full bg-emerald-500 ring-2 ring-offset-2 ring-emerald-500 transition active:scale-95"></button>
                        <button onclick="setAccentColor('purple')" class="w-8 h-8 rounded-full bg-purple-600 ring-2 ring-offset-2 ring-purple-600 transition active:scale-95"></button>
                        <button onclick="setAccentColor('orange')" class="w-8 h-8 rounded-full bg-orange-500 ring-2 ring-offset-2 ring-orange-500 transition active:scale-95"></button>
                    </div>
                </div>

                <!-- Ukuran Teks -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-2">
                    <p class="text-[11px] font-medium text-slate-400 uppercase tracking-wider">Ukuran Teks</p>
                    <div class="grid grid-cols-3 gap-2 text-center">
                        <button onclick="setTextSize('sm')" id="btnTextSm" class="py-1.5 rounded-lg border text-xs font-medium transition-all">Kecil</button>
                        <button onclick="setTextSize('md')" id="btnTextMd" class="py-1.5 rounded-lg border text-xs font-medium transition-all">Normal</button>
                        <button onclick="setTextSize('lg')" id="btnTextLg" class="py-1.5 rounded-lg border text-xs font-medium transition-all">Besar</button>
                    </div>
                </div>

                <!-- Penyesuaian Margin / Spasi Layout Aplikasi -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-2">
                    <div class="flex justify-between items-center">
                        <p class="text-[11px] font-medium text-slate-400 uppercase tracking-wider">Margin Samping Layout</p>
                        <span id="marginValueDisplay" class="text-xs font-mono font-bold theme-accent-text">16px</span>
                    </div>
                    <div class="flex items-center gap-3 pt-1">
                        <span class="text-[10px] text-slate-400">Rapat</span>
                        <input type="range" id="appMarginSlider" min="8" max="32" step="2" oninput="setAppMargin(this.value)" class="w-full h-1.5 bg-slate-200 dark:bg-zinc-700 rounded-lg appearance-none cursor-pointer accent-blue-600">
                        <span class="text-[10px] text-slate-400">Longgar</span>
                    </div>
                </div>

                <!-- Threshold Stok Minimum -->
                <div class="theme-card rounded-xl p-3 shadow-sm border space-y-2">
                    <div class="flex justify-between items-center">
                        <span class="text-xs font-medium">Batas Stok Menipis</span>
                        <input type="number" id="lowStockThreshold" onchange="updateLowStockThreshold(this.value)" min="1" max="20" class="w-16 text-right bg-slate-100 dark:bg-zinc-800 text-xs font-bold rounded p-1 focus:outline-none">
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Scanner Modal -->
    <div id="scannerModal" class="fixed inset-0 bg-black/60 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white w-full max-w-sm rounded-3xl shadow-2xl overflow-hidden flex flex-col">
            <div class="p-4 bg-slate-900 text-white flex justify-between items-center">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-barcode text-blue-400"></i>
                    <h3 class="font-bold text-sm">Scan Barcode / QR Code</h3>
                </div>
                <button onclick="closeScannerModal()" class="text-slate-400 hover:text-white text-lg px-2">&times;</button>
            </div>
            
            <div class="p-4 flex flex-col items-center">
                <div id="reader" class="w-full rounded-xl overflow-hidden bg-black min-h-[250px]"></div>
                <p class="text-xs text-slate-500 text-center mt-3">Arahkan kamera ke kode barcode / part number sparepart.</p>
            </div>

            <div class="p-3 bg-slate-50 border-t border-slate-100 flex gap-2">
                <button onclick="closeScannerModal()" class="w-full py-2 bg-slate-200 hover:bg-slate-300 text-slate-700 text-xs font-semibold rounded-xl">Batal</button>
            </div>
        </div>
    </div>

    <!-- Modal (iOS Sheet) -->
    <div id="partModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center hidden transition-opacity">
        <div class="bg-[#F2F2F7] dark:bg-zinc-900 w-full max-w-md rounded-t-3xl sm:rounded-2xl shadow-2xl overflow-hidden max-h-[90vh] flex flex-col transform transition-transform">
            <!-- Modal Header -->
            <div class="px-5 py-3.5 theme-card border-b flex justify-between items-center sticky top-0 z-10">
                <button type="button" onclick="closeModal()" class="theme-accent-text text-sm font-normal">Batal</button>
                <h3 id="modalTitle" class="font-semibold text-sm">Tambah Sparepart</h3>
                <button type="button" onclick="document.getElementById('submitBtn').click()" class="theme-accent-text font-bold text-sm">Simpan</button>
            </div>

            <!-- Modal Form -->
            <form id="partForm" onsubmit="handleFormSubmit(event)" class="p-4 space-y-4 overflow-y-auto">
                <input type="hidden" id="partId">
                
                <div class="theme-card rounded-xl p-3 space-y-3 shadow-sm border">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Nama Sparepart</label>
                        <input type="text" id="partName" required placeholder="Filter Oli / Kampas Rem" 
                            class="w-full text-sm focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100 dark:border-zinc-800">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Kode Part / No. Part</label>
                        <div class="flex items-center gap-2">
                            <input type="text" id="partCode" placeholder="04465-YZZE1" 
                                class="w-full text-sm focus:outline-none bg-transparent font-mono">
                            <button type="button" onclick="openScannerModal('formPartCode')" class="theme-accent-bg text-white px-2.5 py-1.5 rounded-lg flex items-center gap-1.5 text-xs font-medium shadow-sm hover:opacity-90 active:scale-95 transition shrink-0" title="Scan Barcode Kode Part">
                                <i class="fa-solid fa-barcode"></i>
                                <span class="text-[10px]">Scan</span>
                            </button>
                        </div>
                    </div>
                </div>

                <div class="theme-card rounded-xl p-3 space-y-3 shadow-sm border">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Kendaraan / Mobil</label>
                        <input type="text" id="partCar" required placeholder="Avanza / Jazz / Xpander" 
                            class="w-full text-sm focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100 dark:border-zinc-800">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Tahun Mobil</label>
                        <input type="text" id="partYear" placeholder="2012 - 2021" 
                            class="w-full text-sm focus:outline-none bg-transparent">
                    </div>
                </div>

                <div class="theme-card rounded-xl p-3 space-y-3 shadow-sm border">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Kategori</label>
                        <select id="partCategory" required class="w-full text-sm focus:outline-none bg-transparent">
                            <option value="Mesin">Mesin & Filter</option>
                            <option value="Kaki-kaki">Kaki-kaki & Suspensi</option>
                            <option value="Rem">Pengereman</option>
                            <option value="Kelistrikan">Kelistrikan & Aki</option>
                            <option value="Bodi & Interior">Bodi & Interior</option>
                            <option value="Lainnya">Lainnya</option>
                        </select>
                    </div>
                    <hr class="border-slate-100 dark:border-zinc-800">
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Merk / Brand</label>
                        <input type="text" id="partBrand" required placeholder="Denso / NGK / KYB" 
                            class="w-full text-sm focus:outline-none bg-transparent">
                    </div>
                </div>

                <div class="theme-card rounded-xl p-3 space-y-3 shadow-sm border">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-500">Jumlah Stok</label>
                        <input type="number" id="partStock" min="0" required placeholder="0" 
                            class="w-20 text-right text-sm font-bold focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100 dark:border-zinc-800">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-500">Harga Beli (Modal)</label>
                        <input type="number" id="partBuyPrice" min="0" required placeholder="0" 
                            class="w-32 text-right text-sm font-semibold opacity-80 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100 dark:border-zinc-800">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-500">Harga Jual</label>
                        <input type="number" id="partSellPrice" min="0" required placeholder="0" 
                            class="w-32 text-right text-sm font-bold theme-accent-text focus:outline-none bg-transparent">
                    </div>
                </div>

                <!-- Tombol Scan Barcode Tambahan di Bagian Bawah Form -->
                <button type="button" onclick="openScannerModal('formPartCode')" class="w-full py-2.5 rounded-xl border border-slate-300 dark:border-zinc-700 bg-slate-100 dark:bg-zinc-800 text-xs font-semibold flex items-center justify-center gap-2 hover:bg-slate-200 dark:hover:bg-zinc-700 transition active:scale-95">
                    <i class="fa-solid fa-barcode theme-accent-text text-sm"></i>
                    <span>Scan Kode Barcode Sparepart</span>
                </button>

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
        let html5QrcodeScanner = null;
        let showCategoryBreakdown = true;
        let scanTargetMode = 'search'; // 'search' or 'formPartCode'

        // App Settings Management
        const defaultSettings = { 
            mode: 'light', 
            accent: 'blue', 
            textSize: 'md', 
            appMargin: 16,
            lowStockLimit: 3,
            headerTitle: 'Blantikautoparts',
            headerSubtitle: 'Japan Spec iOS',
            headerLogo: 'fa-car',
            bgColor: '',
            textColor: '',
            headerBgColor: '',
            headerTextColor: ''
        };
        let appSettings = JSON.parse(localStorage.getItem('app_settings')) || defaultSettings;

        function applySettings() {
            const body = document.body;
            
            // Theme Mode
            body.classList.remove('theme-dark');
            if (appSettings.mode === 'dark') {
                body.classList.add('theme-dark');
            }

            // Accent Color
            body.classList.remove('accent-emerald', 'accent-purple', 'accent-orange');
            if (appSettings.accent && appSettings.accent !== 'blue') {
                body.classList.add('accent-' + appSettings.accent);
            }

            // Text Size
            body.classList.remove('text-size-sm', 'text-size-md', 'text-size-lg');
            body.classList.add('text-size-' + (appSettings.textSize || 'md'));

            // App Outer Margin/Padding
            const marginVal = appSettings.appMargin !== undefined ? appSettings.appMargin : 16;
            body.style.setProperty('--custom-app-padding', marginVal + 'px');
            const marginSlider = document.getElementById('appMarginSlider');
            const marginValDisplay = document.getElementById('marginValueDisplay');
            if (marginSlider) marginSlider.value = marginVal;
            if (marginValDisplay) marginValDisplay.textContent = marginVal + 'px';

            // Header Title & Subtitle
            const headerTitleEl = document.getElementById('appHeaderTitle');
            const headerSubEl = document.getElementById('appHeaderSubtitle');
            if (headerTitleEl) headerTitleEl.textContent = appSettings.headerTitle || defaultSettings.headerTitle;
            if (headerSubEl) headerSubEl.textContent = appSettings.headerSubtitle || defaultSettings.headerSubtitle;

            // Inputs in Settings
            const inputTitle = document.getElementById('settingHeaderTitle');
            const inputSub = document.getElementById('settingHeaderSubtitle');
            if (inputTitle) inputTitle.value = appSettings.headerTitle || defaultSettings.headerTitle;
            if (inputSub) inputSub.value = appSettings.headerSubtitle || defaultSettings.headerSubtitle;

            // Logo Icon
            const headerLogoEl = document.getElementById('appHeaderLogo');
            if (headerLogoEl) {
                headerLogoEl.innerHTML = `<i class="fa-solid ${appSettings.headerLogo || 'fa-car'}"></i>`;
            }

            // Custom Colors
            if (appSettings.bgColor) {
                body.style.setProperty('--custom-bg-color', appSettings.bgColor);
            } else {
                body.style.removeProperty('--custom-bg-color');
            }

            if (appSettings.textColor) {
                body.style.setProperty('--custom-text-color', appSettings.textColor);
            } else {
                body.style.removeProperty('--custom-text-color');
            }

            if (appSettings.headerBgColor) {
                body.style.setProperty('--custom-header-bg', appSettings.headerBgColor);
            } else {
                body.style.removeProperty('--custom-header-bg');
            }

            if (appSettings.headerTextColor) {
                body.style.setProperty('--custom-header-text-color', appSettings.headerTextColor);
            } else {
                body.style.removeProperty('--custom-header-text-color');
            }

            // Color Pickers Values
            const bgPicker = document.getElementById('pickerBgColor');
            const textPicker = document.getElementById('pickerTextColor');
            const hBgPicker = document.getElementById('pickerHeaderBgColor');
            const hTextPicker = document.getElementById('pickerHeaderTextColor');

            if (bgPicker) bgPicker.value = appSettings.bgColor || '#F2F2F7';
            if (textPicker) textPicker.value = appSettings.textColor || '#0f172a';
            if (hBgPicker) hBgPicker.value = appSettings.headerBgColor || '#F2F2F7';
            if (hTextPicker) hTextPicker.value = appSettings.headerTextColor || '#0f172a';

            // Active button indicators in settings
            updateSettingsUIIndicators();

            // Low Stock Threshold
            const lowStockInput = document.getElementById('lowStockThreshold');
            if (lowStockInput) lowStockInput.value = appSettings.lowStockLimit || 3;

            saveSettings();
        }

        function updateSettingsUIIndicators() {
            const btnLight = document.getElementById('btnThemeLight');
            const btnDark = document.getElementById('btnThemeDark');
            if (btnLight && btnDark) {
                if (appSettings.mode === 'dark') {
                    btnDark.classList.add('border-blue-500', 'bg-blue-50', 'dark:bg-zinc-800');
                    btnLight.classList.remove('border-blue-500', 'bg-blue-50', 'dark:bg-zinc-800');
                } else {
                    btnLight.classList.add('border-blue-500', 'bg-blue-50');
                    btnDark.classList.remove('border-blue-500', 'bg-blue-50', 'dark:bg-zinc-800');
                }
            }

            ['sm', 'md', 'lg'].forEach(sz => {
                const btn = document.getElementById('btnText' + sz.charAt(0).toUpperCase() + sz.slice(1));
                if (btn) {
                    if (appSettings.textSize === sz) {
                        btn.classList.add('border-blue-500', 'bg-blue-500', 'text-white');
                    } else {
                        btn.classList.remove('border-blue-500', 'bg-blue-500', 'text-white');
                    }
                }
            });

            // Highlight selected logo
            const logoBtns = document.querySelectorAll('#logoSelectorContainer button');
            logoBtns.forEach(btn => {
                const icon = btn.getAttribute('data-icon');
                if (icon === (appSettings.headerLogo || 'fa-car')) {
                    btn.classList.add('border-blue-500', 'ring-2', 'ring-blue-500');
                } else {
                    btn.classList.remove('border-blue-500', 'ring-2', 'ring-blue-500');
                }
            });
        }

        function saveSettings() {
            localStorage.setItem('app_settings', JSON.stringify(appSettings));
        }

        function setAppMargin(value) {
            appSettings.appMargin = parseInt(value);
            applySettings();
        }

        function setCustomColor(key, value) {
            appSettings[key] = value;
            applySettings();
        }

        function resetCustomColors() {
            appSettings.bgColor = '';
            appSettings.textColor = '';
            appSettings.headerBgColor = '';
            appSettings.headerTextColor = '';
            applySettings();
            showToast("Warna kustom telah di-reset.");
        }

        function setAppLogo(iconClass) {
            appSettings.headerLogo = iconClass;
            applySettings();
            showToast("Logo header berhasil diubah.");
        }

        function updateHeaderTitle(val) {
            appSettings.headerTitle = val;
            applySettings();
        }

        function updateHeaderSubtitle(val) {
            appSettings.headerSubtitle = val;
            applySettings();
        }

        function toggleCategoryStats() {
            const container = document.getElementById('categoryStatsContainer');
            const chevron = document.getElementById('catStatsChevron');
            if (!container || !chevron) return;

            showCategoryBreakdown = !showCategoryBreakdown;
            if (showCategoryBreakdown) {
                container.classList.remove('hidden');
                chevron.classList.remove('rotate-180');
            } else {
                container.classList.add('hidden');
                chevron.classList.add('rotate-180');
            }
        }

        function openScannerModal(target = 'search') {
            scanTargetMode = target;
            const modal = document.getElementById('scannerModal');
            modal.classList.remove('hidden');

            if (!html5QrcodeScanner) {
                html5QrcodeScanner = new Html5Qrcode("reader");
            }

            const config = { fps: 10, qrbox: { width: 220, height: 180 } };
            
            html5QrcodeScanner.start(
                { facingMode: "environment" },
                config,
                onScanSuccess,
                onScanFailure
            ).catch(err => {
                showToast("Gagal mengakses kamera: " + err, true);
            });
        }

        function closeScannerModal() {
            if (html5QrcodeScanner && html5QrcodeScanner.isScanning) {
                html5QrcodeScanner.stop().then(() => {
                    document.getElementById('scannerModal').classList.add('hidden');
                }).catch(err => {
                    document.getElementById('scannerModal').classList.add('hidden');
                });
            } else {
                document.getElementById('scannerModal').classList.add('hidden');
            }
        }

        function onScanSuccess(decodedText, decodedResult) {
            if (html5QrcodeScanner && html5QrcodeScanner.isScanning) {
                html5QrcodeScanner.stop().then(() => {
                    document.getElementById('scannerModal').classList.add('hidden');
                    processScannedCode(decodedText);
                });
            } else {
                document.getElementById('scannerModal').classList.add('hidden');
                processScannedCode(decodedText);
            }
        }

        function onScanFailure(error) {
            // Quiet failure
        }

        function processScannedCode(code) {
            if (scanTargetMode === 'formPartCode') {
                const partCodeInput = document.getElementById('partCode');
                if (partCodeInput) {
                    partCodeInput.value = code;
                    showToast(`Kode Part terisi: ${code}`);
                }
            } else {
                const searchInput = document.getElementById('searchInput');
                if (searchInput) {
                    searchInput.value = code;
                    renderTable();
                    showToast(`Hasil scan: ${code}`);
                }
            }
        }

        function renderCategoryStats() {
            const container = document.getElementById('categoryStatsContainer');
            if (!container) return;

            const categories = ["Mesin", "Kaki-kaki", "Rem", "Kelistrikan", "Bodi & Interior", "Lainnya"];
            
            const counts = {};
            categories.forEach(cat => counts[cat] = 0);
            spareparts.forEach(p => {
                if (counts[p.category] !== undefined) {
                    counts[p.category] += (p.stock || 0);
                } else {
                    counts["Lainnya"] += (p.stock || 0);
                }
            });

            container.innerHTML = categories.map(cat => `
                <div class="flex items-center justify-between p-2 rounded-xl bg-slate-100/70 dark:bg-zinc-800/60 border border-slate-200/50 dark:border-zinc-700/50">
                    <span class="text-slate-500 font-medium truncate mr-1">${cat}</span>
                    <span class="font-bold theme-accent-text">${counts[cat]} <span class="text-[10px] text-slate-400 font-normal">pcs</span></span>
                </div>
            `).join('');
        }

        function renderTable() {
            updateBrandFilterOptions();
            renderCategoryStats();

            const container = document.getElementById('sparepartList');
            const emptyState = document.getElementById('emptyState');
            const searchVal = document.getElementById('searchInput').value.toLowerCase();
            const categoryVal = document.getElementById('filterCategory').value;
            const brandVal = document.getElementById('filterBrand').value;

            container.innerHTML = '';

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

            document.getElementById('statTotalItems').innerHTML = `${spareparts.length} <span class="text-xs font-normal text-slate-500">Item</span>`;

            if (filtered.length === 0) {
                emptyState.classList.remove('hidden');
                return;
            } else {
                emptyState.classList.add('hidden');
            }

            filtered.forEach(item => {
                const card = document.createElement('div');
                card.className = "p-3.5 hover:bg-slate-50 dark:hover:bg-zinc-800/50 transition flex items-center justify-between gap-3";
                
                const isLowStock = item.stock <= (appSettings.lowStockLimit || 3);

                card.innerHTML = `
                    <div class="flex-1 min-w-0">
                        <div class="flex items-center gap-1.5 mb-0.5">
                            <h3 class="font-semibold text-sm truncate">${item.name}</h3>
                            ${item.code ? `<span class="bg-slate-100 dark:bg-zinc-800 text-slate-500 dark:text-slate-400 text-[10px] font-mono font-medium px-1.5 py-0.2 rounded border border-slate-200 dark:border-zinc-700">${item.code}</span>` : ''}
                        </div>
                        <p class="text-xs text-slate-400 truncate flex items-center gap-1">
                            <i class="fa-solid fa-car text-[10px]"></i> ${item.car} ${item.year ? `(${item.year})` : ''}
                        </p>
                        <div class="flex items-center gap-2 mt-1.5">
                            <span class="text-[10px] font-semibold text-slate-500 bg-slate-100 dark:bg-zinc-800 dark:text-zinc-400 px-2 py-0.5 rounded-full">${item.brand || 'Lainnya'}</span>
                            <span class="text-xs font-bold theme-accent-text">${formatRupiah(item.sellPrice)}</span>
                        </div>
                    </div>
                    <div class="flex flex-col items-end gap-2">
                        <span class="text-xs font-bold px-2 py-0.5 rounded-full ${isLowStock ? 'bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300' : 'bg-emerald-100 text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300'}">
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

        function setThemeMode(mode) {
            appSettings.mode = mode;
            applySettings();
        }

        function setAccentColor(accent) {
            appSettings.accent = accent;
            applySettings();
        }

        function setTextSize(size) {
            appSettings.textSize = size;
            applySettings();
        }

        function updateLowStockThreshold(limit) {
            appSettings.lowStockLimit = parseInt(limit) || 3;
            applySettings();
            renderTable();
        }

        function openSettingsModal() {
            document.getElementById('settingsModal').classList.remove('hidden');
        }

        function closeSettingsModal() {
            document.getElementById('settingsModal').classList.add('hidden');
        }

        // Initial Default Spareparts Data
        const defaultParts = [
            { id: 1, name: "Kampas Rem Depan Set", code: "04465-YZZE1", year: "2012 - 2021", category: "Rem", brand: "Akimbo / Genuine", car: "Toyota Avanza / Xenia", stock: 8, buyPrice: 175000, sellPrice: 240000 },
            { id: 2, name: "Filter Oli Mesin", code: "15400-RTA-003", year: "2014 - 2022", category: "Mesin", brand: "Denso", car: "Honda Jazz / Brio", stock: 15, buyPrice: 35000, sellPrice: 55000 },
            { id: 3, name: "Busi Iridium Standard", code: "IZFR6K11", year: "2017 - 2023", category: "Kelistrikan", brand: "NGK", car: "Mitsubishi Xpander", stock: 2, buyPrice: 95000, sellPrice: 135000 },
            { id: 4, name: "Link Stabilizer Depan", code: "SL-S080", year: "2012 - 2018", category: "Kaki-kaki", brand: "555 / Japan", car: "Suzuki Ertiga", stock: 4, buyPrice: 110000, sellPrice: 165000 },
            { id: 5, name: "Shock Absorber Belakang", code: "48530-09L10", year: "2016 - 2023", category: "Kaki-kaki", brand: "KYB (Kayaba)", car: "Toyota Innova Reborn", stock: 1, buyPrice: 450000, sellPrice: 620000 }
        ];

        let spareparts = JSON.parse(localStorage.getItem('spareparts_data')) || defaultParts;

        function formatRupiah(number) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits: 0 }).format(number);
        }

        function updateBrandFilterOptions() {
            const filterBrandSelect = document.getElementById('filterBrand');
            if (!filterBrandSelect) return;
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
            const stock = parseInt(document.getElementById('partStock').value);
            const buyPrice = parseInt(document.getElementById('partBuyPrice').value);
            const sellPrice = parseInt(document.getElementById('partSellPrice').value);

            if (sellPrice < buyPrice) {
                showToast("Peringatan: Harga jual lebih rendah dari modal!", true);
            }

            if (id) {
                const index = spareparts.findIndex(i => i.id == id);
                if (index !== -1) {
                    spareparts[index] = { id: Number(id), name, code, year, category, brand, car, stock, buyPrice, sellPrice };
                    showToast("Data berhasil diperbarui!");
                }
            } else {
                const newId = spareparts.length > 0 ? Math.max(...spareparts.map(i => i.id)) + 1 : 1;
                spareparts.push({ id: newId, name, code, year, category, brand, car, stock, buyPrice, sellPrice });
                showToast("Sparepart berhasil ditambahkan!");
            }

            saveData();
            renderTable();
            closeModal();
        }

        function deletePart(id) {
            spareparts = spareparts.filter(i => i.id !== id);
            saveData();
            renderTable();
            showToast("Sparepart dihapus.");
        }

        function resetData() {
            spareparts = [...defaultParts];
            saveData();
            renderTable();
            showToast("Data direset.");
        }

        window.onload = function() {
            applySettings();
            renderTable();
        }
    </script>
</body>
</html>
