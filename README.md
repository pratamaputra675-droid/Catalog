<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Inventori Sparepart Mobil</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        ios: {
                            blue: '#007AFF',
                            green: '#34C759',
                            red: '#FF3B30',
                            orange: '#FF9500',
                            gray: '#8E8E93',
                            bgDark: '#000000',
                            cardDark: '#1C1C1E',
                            borderDark: '#2C2C2E',
                            inputDark: '#2C2C2E'
                        }
                    },
                    fontFamily: {
                        sans: ['-apple-system', 'BlinkMacSystemFont', '"SF Pro Text"', '"SF Pro Display"', '"Helvetica Neue"', 'Helvetica', 'Arial', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        :root {
            --bg-body: #000000;
            --bg-card: #1C1C1E;
            --bg-input: #2C2C2E;
            --border-color: #2C2C2E;
            --text-primary: #FFFFFF;
            --text-secondary: #8E8E93;
            --header-glass: rgba(28, 28, 30, 0.75);
        }

        html, body {
            overflow: hidden !important;
            height: 100% !important;
            width: 100vw !important;
            margin: 0;
            padding: 0;
            touch-action: pan-y;
            background-color: #0d0d0f;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", sans-serif;
            color: var(--text-primary);
            -webkit-tap-highlight-color: transparent;
            user-select: none;
            -webkit-user-select: none;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Outer iPhone Screen Simulation Frame */
        .iphone-frame {
            width: 100%;
            max-width: 390px;
            height: 100dvh;
            max-height: 844px;
            background-color: var(--bg-body);
            border-radius: 0px;
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7);
            transition: background-color 0.25s ease, color 0.25s ease;
        }

        @media (min-height: 700px) and (min-width: 420px) {
            .iphone-frame {
                height: 94dvh;
                border-radius: 40px;
                border: 8px solid #222;
            }
        }
        
        .theme-card {
            background-color: var(--bg-card) !important;
            border-color: var(--border-color) !important;
            color: var(--text-primary);
        }
        
        .theme-input {
            background-color: var(--bg-input) !important;
            border-color: var(--border-color) !important;
            color: var(--text-primary) !important;
        }

        .theme-glass {
            background-color: var(--header-glass) !important;
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
        }
        
        .theme-text-primary { color: var(--text-primary) !important; }
        .theme-text-secondary { color: var(--text-secondary) !important; }

        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        .pt-safe { padding-top: max(env(safe-area-inset-top), 10px); }
        .pb-safe { padding-bottom: max(env(safe-area-inset-bottom), 14px); }

        /* Smooth iOS Sheet Animations */
        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }
        .animate-sheet { animation: slideUp 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
    </style>
</head>
<body id="appBody" class="text-sm selection:bg-ios-blue selection:text-white">

    <!-- IPHONE DEVICE SCREEN CONTAINER FRAME -->
    <div class="iphone-frame relative">

        <!-- TOP iOS STATUS / NAVIGATION BAR -->
        <header class="shrink-0 z-30 theme-glass border-b border-slate-800/40 pt-safe pb-2 dynamic-padding transition-all flex items-center justify-between">
            <div class="flex items-center space-x-1.5">
                <div id="headerLogoContainer" class="w-6 h-6 rounded-lg bg-gradient-to-tr from-ios-blue to-indigo-600 flex items-center justify-center text-white shadow-md shadow-ios-blue/20 overflow-hidden shrink-0">
                    <i id="headerLogoIcon" class="fa-solid fa-car-wrench text-[11px]"></i>
                    <img id="headerLogoImg" src="" alt="Logo" class="w-full h-full object-cover hidden">
                </div>
                <div>
                    <h1 id="storeNameText" onclick="openEditStoreModal()" class="font-bold theme-text-primary tracking-tight leading-none flex items-center gap-1 cursor-pointer active:opacity-70 text-[13px]">
                        <span>Toko Sparepart Jaya</span>
                        <i class="fa-solid fa-chevron-right text-[7px] theme-text-secondary opacity-60"></i>
                    </h1>
                    <span id="storeSubtitleText" onclick="openEditStoreModal()" class="text-[9px] theme-text-secondary cursor-pointer active:opacity-70 font-normal">Inventori Sparepart</span>
                </div>
            </div>

            <!-- Alert Badge -->
            <button onclick="filterEmptyStock()" class="relative w-6 h-6 theme-card rounded-full border border-slate-700/50 flex items-center justify-center active:scale-90 transition" title="Stok Habis">
                <i class="fa-solid fa-triangle-exclamation text-ios-orange text-[10px]"></i>
                <span id="emptyStockBadge" class="absolute -top-1 -right-1 bg-ios-red text-white text-[8px] font-bold w-3.5 h-3.5 rounded-full flex items-center justify-center hidden border-2 border-black">0</span>
            </button>
        </header>

        <!-- MAIN SCROLLABLE INNER VIEW CANVAS -->
        <main class="flex-1 overflow-y-auto no-scrollbar py-1.5 space-y-1.5 pb-20 dynamic-padding">

            <!-- VIEW 1: STOK / INVENTORY LIST -->
            <section id="viewStock" class="space-y-1.5">

                <!-- SEARCH & FILTER INPUT BAR -->
                <div class="relative">
                    <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 theme-text-secondary text-xs"></i>
                    <input type="text" id="searchInput" oninput="applyFilters()" placeholder="Cari nama, SKU, atau mobil..." class="w-full theme-input border rounded-2xl pl-8 pr-8 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                    <button id="clearSearchBtn" onclick="clearSearch()" class="absolute right-2.5 top-1/2 -translate-y-1/2 w-4 h-4 rounded-full theme-card border flex items-center justify-center text-[10px] theme-text-secondary hidden">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <!-- CATEGORY CHIPS SCROLLABLE HORIZONTAL -->
                <div class="flex gap-1.5 overflow-x-auto no-scrollbar py-0.5 text-[10px]">
                    <button onclick="setFilterChip('ALL', this)" class="chip-btn active-chip px-2.5 py-1 rounded-full bg-ios-blue text-white font-semibold shrink-0 transition active:scale-95">Semua</button>
                    <button onclick="setFilterChip('Mesin', this)" class="chip-btn px-2.5 py-1 rounded-full theme-card theme-text-secondary font-medium shrink-0 transition active:scale-95 border">Mesin</button>
                    <button onclick="setFilterChip('VBelt', this)" class="chip-btn px-2.5 py-1 rounded-full theme-card theme-text-secondary font-medium shrink-0 transition active:scale-95 border">VBelt</button>
                    <button onclick="setFilterChip('Rem & Kaki-kaki', this)" class="chip-btn px-2.5 py-1 rounded-full theme-card theme-text-secondary font-medium shrink-0 transition active:scale-95 border">Rem & Kaki</button>
                    <button onclick="setFilterChip('Oli & Cairan', this)" class="chip-btn px-2.5 py-1 rounded-full theme-card theme-text-secondary font-medium shrink-0 transition active:scale-95 border">Oli & Cairan</button>
                    <button onclick="setFilterChip('Kelistrikan', this)" class="chip-btn px-2.5 py-1 rounded-full theme-card theme-text-secondary font-medium shrink-0 transition active:scale-95 border">Kelistrikan</button>
                    <button onclick="setFilterChip('EMPTY', this)" class="chip-btn px-2.5 py-1 rounded-full theme-card text-ios-orange border border-ios-orange/30 font-medium shrink-0 transition active:scale-95">Stok Habis</button>
                </div>

                <!-- PARTS LIST CONTAINER -->
                <div id="partsListContainer" class="space-y-1.5"></div>

                <!-- EMPTY STATE SEARCH OR DATA -->
                <div id="emptyState" class="hidden py-8 text-center space-y-2 theme-card rounded-2xl border">
                    <i class="fa-solid fa-box-open text-2xl theme-text-secondary"></i>
                    <p class="text-xs theme-text-secondary font-medium">Sparepart tidak ditemukan</p>
                </div>

            </section>

        <!-- VIEW 2: TAMBAH SPAREPART BARU -->
        <section id="viewAddPart" class="hidden space-y-3">
            <div class="theme-card border rounded-3xl p-4 space-y-3.5 shadow-lg">
                <div class="flex items-center justify-between border-b border-slate-800/40 pb-2.5">
                    <h2 class="text-sm font-bold theme-text-primary flex items-center gap-2">
                        <i class="fa-solid fa-plus-circle text-ios-blue"></i> Tambah Part Baru
                    </h2>
                    <span class="text-[10px] theme-text-secondary">Lengkapi Form</span>
                </div>

                <form id="addPartForm" onsubmit="handleSaveNewPart(event)" class="space-y-3 text-xs">
                    <div>
                        <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Kode / SKU</label>
                        <div class="flex gap-2">
                            <input type="text" id="addSKU" required placeholder="SKU-1001" class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                            <button type="button" onclick="generateAddSKU()" class="theme-card border px-3.5 py-2.5 rounded-2xl theme-text-secondary font-medium active:scale-95 transition">Auto</button>
                        </div>
                    </div>

                    <div>
                        <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Nama Sparepart</label>
                        <input type="text" id="addName" required placeholder="Contoh: Kampas Rem Depan Avanza" class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                    </div>

                    <div class="grid grid-cols-2 gap-2.5">
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Kategori</label>
                            <select id="addCategory" required onchange="toggleVBeltField('add')" class="w-full theme-input border rounded-2xl px-3 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                                <option value="Mesin">Mesin</option>
                                <option value="VBelt">VBelt</option>
                                <option value="Rem & Kaki-kaki">Rem & Kaki-kaki</option>
                                <option value="Oli & Cairan">Oli & Cairan</option>
                                <option value="Kelistrikan">Kelistrikan</option>
                                <option value="Bodi & Aksesoris">Bodi & Aksesoris</option>
                                <option value="Lainnya">Lainnya</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Kesesuaian Mobil</label>
                            <input type="text" id="addCarModel" required placeholder="Avanza / Xenia" class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                        </div>
                    </div>

                    <!-- VBELT SIZE FIELD (ADD) -->
                    <div id="addVBeltSizeContainer" class="hidden">
                        <label class="block text-[10px] font-semibold text-ios-orange uppercase mb-1 tracking-wider flex items-center gap-1">
                            <i class="fa-solid fa-ruler-horizontal text-[10px]"></i> Ukuran VBelt
                        </label>
                        <input type="text" id="addVBeltSize" placeholder="Contoh: 4PK 855 / K-38 / 6PK 1210" class="w-full theme-input border border-ios-orange/40 rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:border-ios-orange">
                    </div>

                    <div class="grid grid-cols-2 gap-2.5">
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Harga Beli (Rp)</label>
                            <input type="number" id="addBuyPrice" min="0" required placeholder="150000" class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                        </div>
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Harga Jual (Rp)</label>
                            <input type="number" id="addSellPrice" min="0" required placeholder="200000" class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                        </div>
                    </div>

                    <div>
                        <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Jumlah Stok Awal</label>
                        <input type="number" id="addStock" min="0" required placeholder="10" class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                    </div>

                    <div class="flex gap-2 pt-2">
                        <button type="button" onclick="cancelAddPart()" class="w-1/3 min-h-[44px] theme-card border theme-text-secondary font-semibold rounded-2xl transition flex items-center justify-center space-x-1 active:scale-95">
                            <i class="fa-solid fa-xmark"></i>
                            <span>Batal</span>
                        </button>
                        <button type="submit" class="w-2/3 min-h-[44px] bg-ios-green hover:opacity-90 text-white font-semibold rounded-2xl transition flex items-center justify-center space-x-2 active:scale-95 shadow-lg shadow-ios-green/20">
                            <i class="fa-solid fa-floppy-disk"></i>
                            <span>Simpan Sparepart</span>
                        </button>
                    </div>
                </form>
            </div>
        </section>


        <!-- VIEW 3: PENGATURAN & RINGKASAN DATA -->
        <section id="viewSettings" class="hidden space-y-3">
            
            <!-- Quick Summary Metrics -->
            <div class="grid grid-cols-2 gap-2.5">
                <div class="theme-card border p-3.5 rounded-3xl">
                    <span class="text-[10px] theme-text-secondary uppercase font-semibold tracking-wider">Total Item</span>
                    <h4 id="summaryTotalTypes" class="text-xl font-bold theme-text-primary mt-0.5">0</h4>
                </div>
                <div class="theme-card border p-3.5 rounded-3xl">
                    <span class="text-[10px] theme-text-secondary uppercase font-semibold tracking-wider">Total Unit Stok</span>
                    <h4 id="summaryTotalUnits" class="text-xl font-bold theme-text-primary mt-0.5">0</h4>
                </div>
            </div>

            <!-- TAMPILAN & KUSTOMISASI -->
            <div class="theme-card border rounded-3xl p-4 space-y-3.5">
                <h3 class="text-xs font-bold theme-text-primary uppercase tracking-wider flex items-center gap-2">
                    <i class="fa-solid fa-palette text-ios-blue"></i> Tampilan Header & Tema
                </h3>

                <!-- Logo Header Picker -->
                <div class="space-y-2">
                    <label class="block text-[10px] font-semibold theme-text-secondary uppercase tracking-wider">Logo Header</label>
                    <div class="grid grid-cols-5 gap-2">
                        <button onclick="setLogoPreset('fa-car-wrench')" class="logo-preset-btn p-2 rounded-2xl theme-input border flex items-center justify-center theme-text-primary active:scale-90 transition text-sm">
                            <i class="fa-solid fa-car-wrench"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-car')" class="logo-preset-btn p-2 rounded-2xl theme-input border flex items-center justify-center theme-text-primary active:scale-90 transition text-sm">
                            <i class="fa-solid fa-car"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-gear')" class="logo-preset-btn p-2 rounded-2xl theme-input border flex items-center justify-center theme-text-primary active:scale-90 transition text-sm">
                            <i class="fa-solid fa-gear"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-screwdriver-wrench')" class="logo-preset-btn p-2 rounded-2xl theme-input border flex items-center justify-center theme-text-primary active:scale-90 transition text-sm">
                            <i class="fa-solid fa-screwdriver-wrench"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-warehouse')" class="logo-preset-btn p-2 rounded-2xl theme-input border flex items-center justify-center theme-text-primary active:scale-90 transition text-sm">
                            <i class="fa-solid fa-warehouse"></i>
                        </button>
                    </div>

                    <!-- Upload Gambar Logo dari Galeri -->
                    <div class="pt-1">
                        <label for="logoFileInput" class="w-full min-h-[40px] theme-input border border-dashed rounded-2xl px-3 flex items-center justify-center gap-2 text-xs theme-text-secondary cursor-pointer active:scale-98 transition">
                            <i class="fa-solid fa-image text-ios-blue"></i>
                            <span id="uploadLogoText">Upload Logo dari Galeri</span>
                        </label>
                        <input type="file" id="logoFileInput" accept="image/*" onchange="handleLogoUpload(event)" class="hidden">
                    </div>
                </div>

                <!-- Penyesuaian Jarak Sisi Kanan/Kiri (Padding) -->
                <div class="space-y-1.5 pt-1">
                    <div class="flex justify-between items-center text-xs">
                        <span class="theme-text-secondary font-medium">Jarak Sisi Kanan/Kiri</span>
                        <span id="paddingValText" class="theme-text-primary font-mono text-[11px]">px-2</span>
                    </div>
                    <input type="range" id="paddingRange" min="1" max="6" step="1" value="2" oninput="changePadding(this.value)" class="w-full accent-ios-blue cursor-pointer">
                </div>

                <!-- Ukuran Teks (Font Size) -->
                <div class="space-y-1.5 pt-1">
                    <div class="flex justify-between items-center text-xs">
                        <span class="theme-text-secondary font-medium">Ukuran Teks Aplikasi</span>
                        <span id="textSizeText" class="theme-text-primary font-mono text-[11px]">Sedang</span>
                    </div>
                    <div class="grid grid-cols-3 gap-2">
                        <button onclick="setTextSize('sm')" id="btnTextSm" class="px-2 py-2 theme-input border theme-text-secondary text-xs rounded-2xl transition active:scale-95">Kecil</button>
                        <button onclick="setTextSize('base')" id="btnTextBase" class="px-2 py-2 theme-input border theme-text-secondary text-xs rounded-2xl transition active:scale-95">Sedang</button>
                        <button onclick="setTextSize('lg')" id="btnTextLg" class="px-2 py-2 theme-input border theme-text-secondary text-xs rounded-2xl transition active:scale-95">Besar</button>
                    </div>
                </div>

                <!-- Tema Latar Belakang Presets -->
                <div class="space-y-1.5 pt-1">
                    <span class="block theme-text-secondary font-medium text-xs">Preset Tema Latar Belakang</span>
                    <div class="grid grid-cols-3 gap-2 mb-2">
                        <button onclick="setTheme('dark-blue')" title="Gelap Biru" class="h-9 rounded-2xl bg-[#000000] border-2 border-ios-blue/60 active:scale-95 transition flex items-center justify-center text-xs text-slate-200 font-medium">Gelap</button>
                        <button onclick="setTheme('midnight')" title="Midnight Black" class="h-9 rounded-2xl bg-[#050508] border border-slate-800 active:scale-95 transition flex items-center justify-center text-xs text-slate-200 font-medium">Hitam</button>
                        <button onclick="setTheme('slate')" title="Slate Grey" class="h-9 rounded-2xl bg-[#0f172a] border border-slate-700 active:scale-95 transition flex items-center justify-center text-xs text-slate-200 font-medium">Slate</button>
                    </div>
                    <div class="grid grid-cols-3 gap-2">
                        <button onclick="setTheme('emerald')" title="Emerald Green" class="h-9 rounded-2xl bg-[#022c22] border border-emerald-800 active:scale-95 transition flex items-center justify-center text-xs text-slate-200 font-medium">Hijau</button>
                        <button onclick="setTheme('light-clean')" title="Terang Putih" class="h-9 rounded-2xl bg-[#f2f2f7] border border-slate-300 active:scale-95 transition flex items-center justify-center text-xs text-slate-800 font-semibold">Terang</button>
                        <button onclick="setTheme('light-soft')" title="Terang Soft Slate" class="h-9 rounded-2xl bg-[#e5e5ea] border border-slate-400 active:scale-95 transition flex items-center justify-center text-xs text-slate-800 font-semibold">Soft Terang</button>
                    </div>
                </div>

                <!-- KUSTOMISASI WARNA BEBAS -->
                <div class="space-y-2 pt-2 border-t border-slate-800/40">
                    <span class="block theme-text-secondary font-semibold text-[10px] uppercase tracking-wider">Kustom Warna Manual</span>
                    <div class="grid grid-cols-2 gap-2 text-xs">
                        <div class="theme-input p-2 rounded-2xl border flex items-center justify-between">
                            <span class="theme-text-secondary text-[11px]">Warna Latar</span>
                            <input type="color" id="customBgPicker" onchange="setCustomBgColor(this.value)" class="w-7 h-7 bg-transparent rounded cursor-pointer border-0">
                        </div>
                        <div class="theme-input p-2 rounded-2xl border flex items-center justify-between">
                            <span class="theme-text-secondary text-[11px]">Warna Teks</span>
                            <input type="color" id="customTextPicker" onchange="setCustomTextColor(this.value)" class="w-7 h-7 bg-transparent rounded cursor-pointer border-0">
                        </div>
                    </div>
                </div>

                <!-- TOMBOL SIMPAN PENGATURAN -->
                <div class="pt-1">
                    <button onclick="saveAppSettingsExplicit()" class="w-full min-h-[44px] bg-ios-blue hover:opacity-90 text-white font-semibold rounded-2xl transition flex items-center justify-center space-x-2 active:scale-95 shadow-lg shadow-ios-blue/20">
                        <i class="fa-solid fa-floppy-disk"></i>
                        <span>Simpan Pengaturan</span>
                    </button>
                </div>
            </div>

            <!-- Store Actions Card -->
            <div class="theme-card border rounded-3xl p-4 space-y-2.5">
                <h3 class="text-xs font-bold theme-text-primary uppercase tracking-wider">Pengelolaan Data</h3>
                
                <div class="space-y-2">
                    <button onclick="openEditStoreModal()" class="w-full min-h-[44px] theme-input border rounded-2xl px-3.5 flex items-center justify-between text-xs theme-text-primary active:scale-98 transition">
                        <span class="flex items-center gap-2.5"><i class="fa-solid fa-store text-ios-blue"></i> Ubah Identitas Toko</span>
                        <i class="fa-solid fa-chevron-right theme-text-secondary text-[10px]"></i>
                    </button>

                    <button onclick="exportToCSV()" class="w-full min-h-[44px] theme-input border rounded-2xl px-3.5 flex items-center justify-between text-xs theme-text-primary active:scale-98 transition">
                        <span class="flex items-center gap-2.5"><i class="fa-solid fa-file-csv text-ios-green"></i> Ekspor Laporan (CSV)</span>
                        <i class="fa-solid fa-download theme-text-secondary text-[10px]"></i>
                    </button>

                    <button onclick="resetToDemoData()" class="w-full min-h-[44px] theme-input border border-ios-red/30 rounded-2xl px-3.5 flex items-center justify-between text-xs text-ios-red active:scale-98 transition">
                        <span class="flex items-center gap-2.5"><i class="fa-solid fa-rotate-left"></i> Reset Data ke Default</span>
                        <i class="fa-solid fa-trash-can text-[10px]"></i>
                    </button>
                </div>
            </div>

        </section>

        </main>

        <!-- MOBILE BOTTOM TAB BAR (iOS NATIVE TAB BAR) -->
        <nav class="absolute bottom-0 left-0 right-0 w-full theme-glass border-t border-slate-800/40 z-40 px-4 py-1.5 pb-safe">
            <div class="grid grid-cols-3 gap-1 text-center">
                
                <button onclick="switchTab('stock')" id="nav-stock" class="nav-btn text-ios-blue flex flex-col items-center py-1 rounded-2xl transition active:scale-90">
                    <i class="fa-solid fa-boxes-stacked text-base"></i>
                    <span class="text-[9px] font-medium mt-0.5">Stok</span>
                </button>

                <button onclick="switchTab('add')" id="nav-add" class="nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-2xl transition active:scale-90">
                    <i class="fa-solid fa-circle-plus text-base"></i>
                    <span class="text-[9px] font-medium mt-0.5">Tambah</span>
                </button>

                <button onclick="switchTab('settings')" id="nav-settings" class="nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-2xl transition active:scale-90">
                    <i class="fa-solid fa-gear text-base"></i>
                    <span class="text-[9px] font-medium mt-0.5">Pengaturan</span>
                </button>

            </div>
        </nav>

        <!-- EDIT / DETAIL iOS SHEET MODAL -->
        <div id="editDrawer" class="absolute inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-end justify-center hidden">
            <div class="theme-card border-t border-slate-700/50 w-full rounded-t-[32px] p-4 space-y-3 max-h-[85%] overflow-y-auto no-scrollbar animate-sheet shadow-2xl">
                
                <!-- Sheet Drag Indicator Handle -->
                <div class="w-12 h-1 bg-slate-600/50 rounded-full mx-auto mb-1"></div>

                <div class="flex items-center justify-between border-b border-slate-800/40 pb-2.5">
                    <h3 class="text-sm font-bold theme-text-primary flex items-center gap-2">
                        <i class="fa-solid fa-pen-to-square text-ios-blue"></i> Edit Sparepart
                    </h3>
                    <button onclick="closeDrawer()" class="w-7 h-7 rounded-full theme-input border flex items-center justify-center theme-text-secondary active:scale-90 transition">
                        <i class="fa-solid fa-xmark text-xs"></i>
                    </button>
                </div>

                <form onsubmit="handleSaveEditPart(event)" class="space-y-3 text-xs">
                    <input type="hidden" id="editPartId">

                    <div>
                        <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Nama Part</label>
                        <input type="text" id="editName" required class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                    </div>

                    <div class="grid grid-cols-2 gap-2.5">
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Kode / SKU</label>
                            <input type="text" id="editSKU" required class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                        </div>
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Kategori</label>
                            <select id="editCategory" required onchange="toggleVBeltField('edit')" class="w-full theme-input border rounded-2xl px-3 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                                <option value="Mesin">Mesin</option>
                                <option value="VBelt">VBelt</option>
                                <option value="Rem & Kaki-kaki">Rem & Kaki-kaki</option>
                                <option value="Oli & Cairan">Oli & Cairan</option>
                                <option value="Kelistrikan">Kelistrikan</option>
                                <option value="Bodi & Aksesoris">Bodi & Aksesoris</option>
                                <option value="Lainnya">Lainnya</option>
                            </select>
                        </div>
                    </div>

                    <!-- VBELT SIZE FIELD (EDIT) -->
                    <div id="editVBeltSizeContainer" class="hidden">
                        <label class="block text-[10px] font-semibold text-ios-orange uppercase mb-1 tracking-wider flex items-center gap-1">
                            <i class="fa-solid fa-ruler-horizontal text-[10px]"></i> Ukuran VBelt
                        </label>
                        <input type="text" id="editVBeltSize" placeholder="Contoh: 4PK 855 / K-38 / 6PK 1210" class="w-full theme-input border border-ios-orange/40 rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:border-ios-orange">
                    </div>

                    <div>
                        <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Kesesuaian Mobil</label>
                        <input type="text" id="editCarModel" required class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                    </div>

                    <div class="grid grid-cols-2 gap-2.5">
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Harga Beli (Rp)</label>
                            <input type="number" id="editBuyPrice" required class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                        </div>
                        <div>
                            <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Harga Jual (Rp)</label>
                            <input type="number" id="editSellPrice" required class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                        </div>
                    </div>

                    <div>
                        <label class="block text-[10px] font-semibold theme-text-secondary uppercase mb-1 tracking-wider">Stok Saat Ini</label>
                        <input type="number" id="editStock" required class="w-full theme-input border rounded-2xl px-3.5 py-2.5 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                    </div>

                    <div class="pt-2 flex gap-2">
                        <button type="button" onclick="deletePartFromDrawer()" class="w-1/3 min-h-[44px] bg-ios-red/10 border border-ios-red/30 text-ios-red font-semibold rounded-2xl transition flex items-center justify-center space-x-1 active:scale-95">
                            <i class="fa-solid fa-trash-can"></i>
                            <span>Hapus</span>
                        </button>
                        <button type="submit" class="w-2/3 min-h-[44px] bg-ios-blue hover:opacity-90 text-white font-semibold rounded-2xl transition flex items-center justify-center space-x-2 active:scale-95 shadow-lg shadow-ios-blue/20">
                            <i class="fa-solid fa-floppy-disk"></i>
                            <span>Simpan Edit</span>
                        </button>
                    </div>
                </form>
            </div>
        </div>

        <!-- MODAL EDIT STORE NAME & SUBTITLE -->
        <div id="storeModal" class="absolute inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
            <div class="theme-card border rounded-3xl w-full max-w-xs p-4 space-y-3 shadow-2xl animate-sheet">
                <h3 class="text-xs font-bold theme-text-primary uppercase tracking-wider flex items-center gap-1.5">
                    <i class="fa-solid fa-store text-ios-blue"></i> Identitas Toko
                </h3>
                <div>
                    <label class="block text-[10px] theme-text-secondary uppercase mb-1 font-semibold tracking-wider">Nama Toko</label>
                    <input type="text" id="inputStoreName" placeholder="Toko Sparepart Jaya" class="w-full theme-input border rounded-2xl px-3 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                </div>
                <div>
                    <label class="block text-[10px] theme-text-secondary uppercase mb-1 font-semibold tracking-wider">Subtitle</label>
                    <input type="text" id="inputStoreSubtitle" placeholder="Inventori Sparepart" class="w-full theme-input border rounded-2xl px-3 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-ios-blue/50">
                </div>
                <div class="flex justify-end space-x-2 pt-1 text-xs">
                    <button type="button" onclick="closeStoreModal()" class="px-3.5 py-2 theme-input border theme-text-secondary rounded-2xl font-medium active:scale-95 transition">Batal</button>
                    <button type="button" onclick="saveStoreName()" class="px-3.5 py-2 bg-ios-blue text-white rounded-2xl font-semibold active:scale-95 transition shadow-md shadow-ios-blue/20">Simpan</button>
                </div>
            </div>
        </div>

        <!-- TOAST NOTIFICATION CONTAINER -->
        <div id="toastContainer" class="absolute top-14 left-0 right-0 z-50 flex flex-col items-center pointer-events-none px-4 space-y-2"></div>

    </div>

    <!-- APP JAVASCRIPT LOGIC -->
    <script>
        // DEMO INITIAL DATA
        const demoParts = [
            { id: '1', sku: 'AVZ-FLT-01', name: 'Filter Oli Avanza/Xenia', category: 'Oli & Cairan', vbeltSize: '', carModel: 'Avanza, Xenia, Rush', buyPrice: 28000, sellPrice: 40000, stock: 24 },
            { id: '2', sku: 'JZ-BRK-02', name: 'Kampas Rem Depan Jazz RS', category: 'Rem & Kaki-kaki', vbeltSize: '', carModel: 'Honda Jazz GE8/GK5', buyPrice: 185000, sellPrice: 250000, stock: 3 },
            { id: '3', sku: 'VBT-AVZ-03', name: 'Tali Kipas / Fan Belt Alternator', category: 'VBelt', vbeltSize: '4PK 855', carModel: 'Avanza / Xenia 1.3', buyPrice: 45000, sellPrice: 65000, stock: 8 },
            { id: '4', sku: 'INV-SPK-04', name: 'Busi Iridium Tough Innova', category: 'Kelistrikan', vbeltSize: '', carModel: 'Innova Bensin 2.0', buyPrice: 75000, sellPrice: 110000, stock: 12 },
            { id: '5', sku: 'XPN-OIL-05', name: 'Oli Shell Helix HX8 5W-30 (4L)', category: 'Oli & Cairan', vbeltSize: '', carModel: 'Xpander, Livina', buyPrice: 320000, sellPrice: 410000, stock: 1 },
            { id: '6', sku: 'AGY-CLT-06', name: 'Kopling Set Agya/Ayla 1.2', category: 'Mesin', vbeltSize: '', carModel: 'Toyota Agya, Ayla', buyPrice: 650000, sellPrice: 820000, stock: 0 }
        ];

        // STATE
        let spareparts = [];
        let storeName = "Toko Sparepart Jaya";
        let storeSubtitle = "Inventori Sparepart";
        let activeFilterChip = "ALL";

        // APP SETTINGS STATE
        let appSettings = {
            logoType: 'preset',
            logoValue: 'fa-car-wrench',
            padding: 2,
            textSize: 'base',
            theme: 'dark-blue',
            customBg: '',
            customText: ''
        };

        // INIT
        window.onload = function() {
            loadStorage();
            applyAppSettings();
            renderHeader();
            renderCards();
            renderSummary();
            generateAddSKU();
        };

        function loadStorage() {
            try {
                const localParts = localStorage.getItem('mob_spareparts');
                const localStore = localStorage.getItem('mob_store_name');
                const localSub = localStorage.getItem('mob_store_sub');
                const localSettings = localStorage.getItem('mob_app_settings');

                spareparts = localParts ? JSON.parse(localParts) : [...demoParts];
                storeName = localStore || "Toko Sparepart Jaya";
                storeSubtitle = localSub || "Inventori Sparepart";
                if (localSettings) {
                    appSettings = { ...appSettings, ...JSON.parse(localSettings) };
                }
            } catch (e) {
                console.warn('Gagal memuat dari localStorage:', e);
                spareparts = [...demoParts];
            }
            saveStorage();
        }

        function saveStorage() {
            try {
                localStorage.setItem('mob_spareparts', JSON.stringify(spareparts));
                localStorage.setItem('mob_store_name', storeName);
                localStorage.setItem('mob_store_sub', storeSubtitle);
                localStorage.setItem('mob_app_settings', JSON.stringify(appSettings));
            } catch (e) {
                console.warn('Gagal menyimpan ke localStorage:', e);
            }
        }

        function saveAppSettingsExplicit() {
            saveStorage();
            showToast("Pengaturan & file tersimpan otomatis", "success");
        }

        function formatRp(val) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits: 0 }).format(val);
        }

        function escapeHtml(str) {
            if (!str) return '';
            return String(str)
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }

        function adjustHexColor(hex, percent) {
            let num = parseInt(hex.replace('#',''), 16),
                amt = Math.round(2.55 * percent),
                R = (num >> 16) + amt,
                G = (num >> 8 & 0x00FF) + amt,
                B = (num & 0x0000FF) + amt;
            return '#' + (0x1000000 + (R<255?R<0?0:R:255)*0x10000 + (G<255?G<0?0:G:255)*0x100 + (B<255?B<0?0:B:255)).toString(16).slice(1);
        }

        function applyAppSettings() {
            const paddingMap = { 1: 'px-1', 2: 'px-2', 3: 'px-3', 4: 'px-4', 5: 'px-5', 6: 'px-6' };
            const selectedPaddingClass = paddingMap[appSettings.padding] || 'px-2';
            
            document.querySelectorAll('.dynamic-padding').forEach(el => {
                el.className = el.className.replace(/px-\d/g, selectedPaddingClass);
            });

            const pRange = document.getElementById('paddingRange');
            if (pRange) pRange.value = appSettings.padding;
            const pText = document.getElementById('paddingValText');
            if (pText) pText.innerText = selectedPaddingClass;

            const frame = document.querySelector('.iphone-frame');
            if (frame) {
                frame.classList.remove('text-xs', 'text-sm', 'text-base');
                if (appSettings.textSize === 'sm') frame.classList.add('text-xs');
                else if (appSettings.textSize === 'lg') frame.classList.add('text-base');
                else frame.classList.add('text-sm');
            }

            const txtLabelMap = { 'sm': 'Kecil', 'base': 'Sedang', 'lg': 'Besar' };
            const txtText = document.getElementById('textSizeText');
            if (txtText) txtText.innerText = txtLabelMap[appSettings.textSize] || 'Sedang';

            ['btnTextSm', 'btnTextBase', 'btnTextLg'].forEach(id => {
                const btn = document.getElementById(id);
                if (btn) btn.classList.remove('bg-ios-blue', 'text-white');
            });

            if (appSettings.textSize === 'sm') document.getElementById('btnTextSm')?.classList.add('bg-ios-blue', 'text-white');
            if (appSettings.textSize === 'base') document.getElementById('btnTextBase')?.classList.add('bg-ios-blue', 'text-white');
            if (appSettings.textSize === 'lg') document.getElementById('btnTextLg')?.classList.add('bg-ios-blue', 'text-white');

            const root = document.documentElement;
            
            const themes = {
                'dark-blue': { bg: '#000000', card: '#1C1C1E', input: '#2C2C2E', border: '#2C2C2E', textPrimary: '#FFFFFF', textSecondary: '#8E8E93', glass: 'rgba(28, 28, 30, 0.75)' },
                'midnight': { bg: '#050508', card: '#12121A', input: '#1A1A24', border: '#252536', textPrimary: '#FFFFFF', textSecondary: '#9A9AB0', glass: 'rgba(18, 18, 26, 0.75)' },
                'slate': { bg: '#0F172A', card: '#1E293B', input: '#334155', border: '#334155', textPrimary: '#F8FAFC', textSecondary: '#94A3B8', glass: 'rgba(30, 41, 59, 0.75)' },
                'emerald': { bg: '#022C22', card: '#064E3B', input: '#047857', border: '#047857', textPrimary: '#F8FAFC', textSecondary: '#A7F3D0', glass: 'rgba(6, 78, 59, 0.75)' },
                'light-clean': { bg: '#F2F2F7', card: '#FFFFFF', input: '#E5E5EA', border: '#E5E5EA', textPrimary: '#000000', textSecondary: '#6C6C70', glass: 'rgba(255, 255, 255, 0.8)' },
                'light-soft': { bg: '#E5E5EA', card: '#F2F2F7', input: '#FFFFFF', border: '#D1D1D6', textPrimary: '#000000', textSecondary: '#545456', glass: 'rgba(242, 242, 247, 0.85)' }
            };

            let activeTheme = themes[appSettings.theme] || themes['dark-blue'];

            if (appSettings.theme === 'custom') {
                const bg = appSettings.customBg || '#000000';
                const text = appSettings.customText || '#FFFFFF';
                activeTheme = {
                    bg: bg,
                    card: adjustHexColor(bg, 12),
                    input: adjustHexColor(bg, 20),
                    border: adjustHexColor(bg, 25),
                    textPrimary: text,
                    textSecondary: adjustHexColor(text, -35),
                    glass: adjustHexColor(bg, 12) + 'C0'
                };
            }

            root.style.setProperty('--bg-body', activeTheme.bg);
            root.style.setProperty('--bg-card', activeTheme.card);
            root.style.setProperty('--bg-input', activeTheme.input);
            root.style.setProperty('--border-color', activeTheme.border);
            root.style.setProperty('--text-primary', activeTheme.textPrimary);
            root.style.setProperty('--text-secondary', activeTheme.textSecondary);
            root.style.setProperty('--header-glass', activeTheme.glass);
        }

        function renderHeader() {
            document.getElementById('storeNameText').querySelector('span').innerText = storeName;
            document.getElementById('storeSubtitleText').innerText = storeSubtitle;
            
            const logoIcon = document.getElementById('headerLogoIcon');
            const logoImg = document.getElementById('headerLogoImg');

            if (appSettings.logoType === 'custom' && appSettings.logoValue) {
                logoImg.src = appSettings.logoValue;
                logoImg.classList.remove('hidden');
                logoIcon.classList.add('hidden');
            } else {
                logoImg.classList.add('hidden');
                logoIcon.classList.remove('hidden');
                logoIcon.className = `fa-solid ${appSettings.logoValue || 'fa-car-wrench'} text-sm`;
            }

            const emptyCount = spareparts.filter(p => p.stock === 0).length;
            const badge = document.getElementById('emptyStockBadge');
            if (emptyCount > 0) {
                badge.innerText = emptyCount;
                badge.classList.remove('hidden');
            } else {
                badge.classList.add('hidden');
            }
        }

        function setLogoPreset(iconClass) {
            appSettings.logoType = 'preset';
            appSettings.logoValue = iconClass;
            document.getElementById('uploadLogoText').innerText = 'Upload Logo dari Galeri';
            saveStorage();
            renderHeader();
            showToast("Logo header dipilih & disimpan", "info");
        }

        function handleLogoUpload(event) {
            const file = event.target.files[0];
            if (!file) return;

            if (file.size > 2 * 1024 * 1024) {
                showToast("Ukuran foto maksimal 2MB", "error");
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                appSettings.logoType = 'custom';
                appSettings.logoValue = e.target.result;
                document.getElementById('uploadLogoText').innerText = `Terpasang: ${file.name.substring(0, 12)}...`;
                saveStorage();
                renderHeader();
                showToast("Foto logo disimpan", "info");
            };
            reader.readAsDataURL(file);
        }

        function changePadding(val) {
            appSettings.padding = parseInt(val);
            applyAppSettings();
            saveStorage();
        }

        function setTextSize(size) {
            appSettings.textSize = size;
            applyAppSettings();
            saveStorage();
            showToast(`Ukuran teks: ${size === 'sm' ? 'Kecil' : size === 'lg' ? 'Besar' : 'Sedang'}`, "info");
        }

        function setTheme(themeKey) {
            appSettings.theme = themeKey;
            appSettings.customBg = '';
            appSettings.customText = '';
            applyAppSettings();
            saveStorage();
            showToast("Tema dipilih & disimpan", "info");
        }

        function setCustomBgColor(hexColor) {
            appSettings.customBg = hexColor;
            appSettings.theme = 'custom';
            applyAppSettings();
            saveStorage();
        }

        function setCustomTextColor(hexColor) {
            appSettings.customText = hexColor;
            appSettings.theme = 'custom';
            applyAppSettings();
            saveStorage();
        }

        function toggleVBeltField(formPrefix) {
            const cat = document.getElementById(formPrefix + 'Category').value;
            const container = document.getElementById(formPrefix + 'VBeltSizeContainer');
            if (cat === 'VBelt') {
                container.classList.remove('hidden');
            } else {
                container.classList.add('hidden');
                document.getElementById(formPrefix + 'VBeltSize').value = '';
            }
        }

        function renderCards(data = null) {
            const list = data || getFilteredParts();
            const container = document.getElementById('partsListContainer');
            const emptyState = document.getElementById('emptyState');

            container.innerHTML = '';

            if (list.length === 0) {
                emptyState.classList.remove('hidden');
                return;
            } else {
                emptyState.classList.add('hidden');
            }

            list.forEach(p => {
                let badgeColor = "bg-ios-green/15 text-ios-green border-ios-green/30";
                let badgeText = "Aman";

                if (p.stock === 0) {
                    badgeColor = "bg-ios-red/15 text-ios-red border-ios-red/30";
                    badgeText = "Habis";
                }

                const vbeltBadgeHtml = (p.category === 'VBelt' && p.vbeltSize) 
                    ? `<span class="inline-flex items-center px-1 py-0.2 rounded text-[8px] font-bold bg-ios-orange/20 text-ios-orange border border-ios-orange/30">
                        <i class="fa-solid fa-ruler-horizontal text-[6px] mr-0.5"></i>${escapeHtml(p.vbeltSize)}
                       </span>` 
                    : '';

                const card = document.createElement('div');
                card.className = "theme-card border rounded-lg p-1.5 shadow-sm transition duration-150 cursor-pointer";
                
                card.innerHTML = `
                    <div>
                        <!-- Baris Atas: SKU, Kategori, Badge Status -->
                        <div onclick="openDrawer('${p.id}')" class="flex items-center justify-between gap-1 mb-1">
                            <div class="flex items-center gap-1 flex-wrap">
                                <span class="inline-flex items-center px-1 py-0.2 rounded text-[8px] font-bold font-mono bg-ios-blue/20 text-ios-blue border border-ios-blue/30">
                                    <i class="fa-solid fa-barcode text-[7px] mr-0.5 opacity-80"></i>${escapeHtml(p.sku)}
                                </span>
                                ${vbeltBadgeHtml}
                                <span class="text-[8px] theme-text-secondary font-medium">| ${escapeHtml(p.category)}</span>
                            </div>
                            <span class="px-1 py-0.2 text-[7px] font-bold border rounded-full shrink-0 ${badgeColor}">
                                ${badgeText}
                            </span>
                        </div>

                        <!-- Baris Tengah: Nama Sparepart -> Mobil -->
                        <div onclick="openDrawer('${p.id}')" class="space-y-0.5">
                            <h3 class="text-xs font-bold theme-text-primary leading-tight">${escapeHtml(p.name)}</h3>
                            <div class="text-[9px] theme-text-secondary leading-tight flex items-center gap-1">
                                <i class="fa-solid fa-car text-[7px]"></i> ${escapeHtml(p.carModel)}
                            </div>
                        </div>

                        <!-- Baris Bawah: Harga & Tombol Kontrol Stok (+ / -) -->
                        <div class="pt-1.5 flex items-center justify-between border-t border-slate-800/20 mt-1">
                            <div onclick="openDrawer('${p.id}')">
                                <span class="text-[8px] theme-text-secondary uppercase font-medium">Harga: </span>
                                <span class="text-xs font-bold text-ios-green font-mono">${formatRp(p.sellPrice)}</span>
                            </div>

                            <!-- Tombol Penambahan & Pengurangan Stok Quick-Actions -->
                            <div class="flex items-center space-x-1.5 bg-ios-input/40 p-0.5 rounded-lg border border-slate-800/40">
                                <button onclick="event.stopPropagation(); updateStockQuick('${p.id}', -1)" class="w-6 h-6 rounded-md theme-card border border-slate-700/60 flex items-center justify-center text-ios-red active:scale-90 transition font-bold text-xs" title="Kurangi Stok">
                                    <i class="fa-solid fa-minus text-[9px]"></i>
                                </button>
                                
                                <span onclick="openDrawer('${p.id}')" class="font-bold text-xs theme-text-primary font-mono px-1 min-w-[28px] text-center">${p.stock}</span>
                                
                                <button onclick="event.stopPropagation(); updateStockQuick('${p.id}', 1)" class="w-6 h-6 rounded-md theme-card border border-slate-700/60 flex items-center justify-center text-ios-green active:scale-90 transition font-bold text-xs" title="Tambah Stok">
                                    <i class="fa-solid fa-plus text-[9px]"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;

                container.appendChild(card);
            });
        }

        // QUICK STOCK UPDATE (+ / -)
        function updateStockQuick(id, change) {
            const index = spareparts.findIndex(p => p.id === id);
            if (index === -1) return;

            const currentStock = spareparts[index].stock;
            if (change < 0 && currentStock <= 0) {
                showToast("Stok sudah 0", "error");
                return;
            }

            spareparts[index].stock += change;
            saveStorage();
            renderHeader();
            renderCards();
            renderSummary();

            const newStock = spareparts[index].stock;
            const actionText = change > 0 ? `+${change}` : `${change}`;
            showToast(`Stok ${spareparts[index].sku}: ${newStock} (${actionText})`, change > 0 ? "success" : "info");
        }

        // FILTER & SEARCH LOGIC
        function getFilteredParts() {
            const searchVal = document.getElementById('searchInput')?.value.trim().toLowerCase() || '';
            const searchTokens = searchVal.split(/\s+/).filter(t => t.length > 0);

            return spareparts.filter(p => {
                // Category Filter Chip
                if (activeFilterChip === 'EMPTY' && p.stock > 0) return false;
                if (activeFilterChip !== 'ALL' && activeFilterChip !== 'EMPTY' && p.category !== activeFilterChip) return false;

                // Search Query Tokens (All tokens must match somewhere in sku, name, category, vbeltSize, or carModel)
                if (searchTokens.length > 0) {
                    const combinedText = `${p.sku} ${p.name} ${p.category} ${p.vbeltSize || ''} ${p.carModel}`.toLowerCase();
                    const matchesAllTokens = searchTokens.every(token => combinedText.includes(token));
                    if (!matchesAllTokens) return false;
                }

                return true;
            });
        }

        function applyFilters() {
            const searchVal = document.getElementById('searchInput')?.value || '';
            const clearBtn = document.getElementById('clearSearchBtn');
            if (clearBtn) {
                if (searchVal.length > 0) clearBtn.classList.remove('hidden');
                else clearBtn.classList.add('hidden');
            }
            renderCards();
        }

        function clearSearch() {
            const searchInput = document.getElementById('searchInput');
            if (searchInput) searchInput.value = '';
            applyFilters();
        }

        function setFilterChip(chipValue, btnEl) {
            activeFilterChip = chipValue;
            document.querySelectorAll('.chip-btn').forEach(btn => {
                btn.classList.remove('active-chip', 'bg-ios-blue', 'text-white');
                btn.classList.add('theme-card', 'theme-text-secondary');
            });
            if (btnEl) {
                btnEl.classList.remove('theme-card', 'theme-text-secondary');
                btnEl.classList.add('active-chip', 'bg-ios-blue', 'text-white');
            }
            renderCards();
        }

        function filterEmptyStock() {
            activeFilterChip = 'EMPTY';
            document.querySelectorAll('.chip-btn').forEach(btn => {
                btn.classList.remove('active-chip', 'bg-ios-blue', 'text-white');
                btn.classList.add('theme-card', 'theme-text-secondary');
            });
            const emptyChip = Array.from(document.querySelectorAll('.chip-btn')).find(b => b.textContent.includes('Stok Habis'));
            if (emptyChip) {
                emptyChip.classList.remove('theme-card', 'theme-text-secondary');
                emptyChip.classList.add('active-chip', 'bg-ios-blue', 'text-white');
            }
            switchTab('stock');
            renderCards();
        }

        // VIEW NAVIGATION / TABS
        function switchTab(tab) {
            document.getElementById('viewStock').classList.add('hidden');
            document.getElementById('viewAddPart').classList.add('hidden');
            document.getElementById('viewSettings').classList.add('hidden');

            document.querySelectorAll('.nav-btn').forEach(b => {
                b.classList.remove('text-ios-blue');
                b.classList.add('theme-text-secondary');
            });

            if (tab === 'stock') {
                document.getElementById('viewStock').classList.remove('hidden');
                document.getElementById('nav-stock').classList.add('text-ios-blue');
                document.getElementById('nav-stock').classList.remove('theme-text-secondary');
                renderCards();
            } else if (tab === 'add') {
                document.getElementById('viewAddPart').classList.remove('hidden');
                document.getElementById('nav-add').classList.add('text-ios-blue');
                document.getElementById('nav-add').classList.remove('theme-text-secondary');
            } else if (tab === 'settings') {
                document.getElementById('viewSettings').classList.remove('hidden');
                document.getElementById('nav-settings').classList.add('text-ios-blue');
                document.getElementById('nav-settings').classList.remove('theme-text-secondary');
                renderSummary();
            }
        }

        // FORM HANDLERS
        function generateAddSKU() {
            const nextNum = spareparts.length + 101;
            document.getElementById('addSKU').value = `PRT-${nextNum}`;
        }

        function handleSaveNewPart(e) {
            e.preventDefault();
            const sku = document.getElementById('addSKU').value.trim();
            const name = document.getElementById('addName').value.trim();
            const category = document.getElementById('addCategory').value;
            const vbeltSize = document.getElementById('addVBeltSize').value.trim();
            const carModel = document.getElementById('addCarModel').value.trim();
            const buyPrice = parseFloat(document.getElementById('addBuyPrice').value) || 0;
            const sellPrice = parseFloat(document.getElementById('addSellPrice').value) || 0;
            const stock = parseInt(document.getElementById('addStock').value) || 0;

            const newPart = {
                id: Date.now().toString(),
                sku, name, category, vbeltSize, carModel, buyPrice, sellPrice, stock
            };

            spareparts.unshift(newPart);
            saveStorage();
            showToast("Sparepart berhasil ditambahkan", "success");

            // Reset form
            document.getElementById('addPartForm').reset();
            generateAddSKU();
            toggleVBeltField('add');
            switchTab('stock');
            renderHeader();
            renderSummary();
        }

        function cancelAddPart() {
            document.getElementById('addPartForm').reset();
            generateAddSKU();
            toggleVBeltField('add');
            switchTab('stock');
        }

        // DRAWER EDIT LOGIC
        function openDrawer(id) {
            const p = spareparts.find(item => item.id === id);
            if (!p) return;

            document.getElementById('editPartId').value = p.id;
            document.getElementById('editName').value = p.name;
            document.getElementById('editSKU').value = p.sku;
            document.getElementById('editCategory').value = p.category;
            document.getElementById('editVBeltSize').value = p.vbeltSize || '';
            document.getElementById('editCarModel').value = p.carModel;
            document.getElementById('editBuyPrice').value = p.buyPrice;
            document.getElementById('editSellPrice').value = p.sellPrice;
            document.getElementById('editStock').value = p.stock;

            toggleVBeltField('edit');
            document.getElementById('editDrawer').classList.remove('hidden');
        }

        function closeDrawer() {
            document.getElementById('editDrawer').classList.add('hidden');
        }

        function handleSaveEditPart(e) {
            e.preventDefault();
            const id = document.getElementById('editPartId').value;
            const index = spareparts.findIndex(p => p.id === id);
            if (index === -1) return;

            spareparts[index].name = document.getElementById('editName').value.trim();
            spareparts[index].sku = document.getElementById('editSKU').value.trim();
            spareparts[index].category = document.getElementById('editCategory').value;
            spareparts[index].vbeltSize = document.getElementById('editVBeltSize').value.trim();
            spareparts[index].carModel = document.getElementById('editCarModel').value.trim();
            spareparts[index].buyPrice = parseFloat(document.getElementById('editBuyPrice').value) || 0;
            spareparts[index].sellPrice = parseFloat(document.getElementById('editSellPrice').value) || 0;
            spareparts[index].stock = parseInt(document.getElementById('editStock').value) || 0;

            saveStorage();
            closeDrawer();
            renderHeader();
            renderCards();
            renderSummary();
            showToast("Sparepart berhasil diperbarui", "success");
        }

        function deletePartFromDrawer() {
            const id = document.getElementById('editPartId').value;
            if (!id) return;

            spareparts = spareparts.filter(p => p.id !== id);
            saveStorage();
            closeDrawer();
            renderHeader();
            renderCards();
            renderSummary();
            showToast("Sparepart dihapus", "info");
        }

        // STORE MODAL
        function openEditStoreModal() {
            document.getElementById('inputStoreName').value = storeName;
            document.getElementById('inputStoreSubtitle').value = storeSubtitle;
            document.getElementById('storeModal').classList.remove('hidden');
        }

        function closeStoreModal() {
            document.getElementById('storeModal').classList.add('hidden');
        }

        function saveStoreName() {
            const newName = document.getElementById('inputStoreName').value.trim();
            const newSub = document.getElementById('inputStoreSubtitle').value.trim();
            if (newName) storeName = newName;
            if (newSub) storeSubtitle = newSub;

            saveStorage();
            renderHeader();
            closeStoreModal();
            showToast("Identitas toko disimpan", "success");
        }

        // SUMMARY & REPORTS
        function renderSummary() {
            const totalTypes = spareparts.length;
            const totalUnits = spareparts.reduce((acc, p) => acc + p.stock, 0);

            const elTypes = document.getElementById('summaryTotalTypes');
            const elUnits = document.getElementById('summaryTotalUnits');
            if (elTypes) elTypes.innerText = totalTypes;
            if (elUnits) elUnits.innerText = totalUnits;
        }

        function exportToCSV() {
            let csvContent = "data:text/csv;charset=utf-8,SKU,Nama Sparepart,Kategori,Ukuran VBelt,Kesesuaian Mobil,Harga Beli,Harga Jual,Stok\n";
            spareparts.forEach(p => {
                const row = `"${p.sku}","${p.name}","${p.category}","${p.vbeltSize || ''}","${p.carModel}",${p.buyPrice},${p.sellPrice},${p.stock}`;
                csvContent += row + "\n";
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Inventori_Sparepart_${new Date().toISOString().slice(0,10)}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            showToast("File CSV berhasil diunduh", "success");
        }

        function resetToDemoData() {
            spareparts = [...demoParts];
            saveStorage();
            renderHeader();
            renderCards();
            renderSummary();
            showToast("Data direset ke default demo", "info");
        }

        // TOAST NOTIFICATIONS
        function showToast(message, type = "info") {
            const container = document.getElementById('toastContainer');
            if (!container) return;

            const toast = document.createElement('div');
            let iconClass = "fa-circle-info text-ios-blue";
            if (type === "success") iconClass = "fa-circle-check text-ios-green";
            if (type === "error") iconClass = "fa-triangle-exclamation text-ios-red";

            toast.className = "theme-card border px-3 py-2 rounded-2xl shadow-xl flex items-center gap-2 text-xs animate-sheet pointer-events-auto transition-all duration-300";
            toast.innerHTML = `<i class="fa-solid ${iconClass}"></i><span>${escapeHtml(message)}</span>`;

            container.appendChild(toast);

            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transform = 'translateY(-10px)';
                setTimeout(() => toast.remove(), 300);
            }, 2500);
        }
    </script>
</body>
</html>
