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

        body {
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", sans-serif;
            background-color: var(--bg-body);
            color: var(--text-primary);
            -webkit-tap-highlight-color: transparent;
            user-select: none;
            -webkit-user-select: none;
            transition: background-color 0.25s ease, color 0.25s ease;
            touch-action: manipulation;
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
        
        .pt-safe { padding-top: max(env(safe-area-inset-top), 12px); }
        .pb-safe { padding-bottom: max(env(safe-area-inset-bottom), 16px); }

        /* Smooth iOS Sheet Animations */
        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }
        .animate-sheet { animation: slideUp 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
    </style>
</head>
<body id="appBody" class="min-h-screen flex flex-col justify-between max-w-md mx-auto border-x border-slate-800/40 shadow-2xl relative overflow-x-hidden text-sm selection:bg-ios-blue selection:text-white">

    <!-- TOP iOS NAVIGATION BAR -->
    <header class="sticky top-0 z-30 theme-glass border-b border-slate-800/40 pt-safe pb-1.5 flex items-center justify-between dynamic-padding transition-all">
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

    <!-- MAIN CONTENT VIEW CANVAS -->
    <main class="flex-1 py-1 space-y-1.5 pb-24 dynamic-padding">

        <!-- VIEW 1: STOK / INVENTORY LIST -->
        <section id="viewStock" class="space-y-1.5">
            
            <!-- iOS Style Search Bar -->
            <div class="relative">
                <i class="fa-solid fa-magnifying-glass absolute left-2.5 top-1/2 -translate-y-1/2 theme-text-secondary text-[10px]"></i>
                <input type="text" id="searchInput" oninput="applyFilters()" placeholder="Cari sparepart, SKU, mobil..." class="w-full theme-input border rounded-xl pl-7 pr-6 py-1 text-xs placeholder:theme-text-secondary focus:outline-none focus:ring-1 focus:ring-ios-blue/50 transition shadow-inner">
                <button id="clearSearchBtn" onclick="clearSearch()" class="hidden absolute right-2 top-1/2 -translate-y-1/2 theme-text-secondary hover:opacity-80">
                    <i class="fa-solid fa-circle-xmark text-xs"></i>
                </button>
            </div>

            <!-- iOS Filter Segment Chips -->
            <div class="flex items-center space-x-1 overflow-x-auto no-scrollbar py-0.5 text-[9px]">
                <button onclick="setFilterChip('ALL', this)" class="chip-btn active-chip px-2 py-0.5 rounded-full font-medium whitespace-nowrap bg-ios-blue text-white shadow-sm transition active:scale-95">Semua</button>
                <button onclick="setFilterChip('EMPTY', this)" class="chip-btn px-2 py-0.5 rounded-full font-medium whitespace-nowrap theme-card border text-ios-red transition active:scale-95">Stok Habis</button>
                <button onclick="setFilterChip('Mesin', this)" class="chip-btn px-2 py-0.5 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary transition active:scale-95">Mesin</button>
                <button onclick="setFilterChip('VBelt', this)" class="chip-btn px-2 py-0.5 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary transition active:scale-95">VBelt</button>
                <button onclick="setFilterChip('Rem & Kaki-kaki', this)" class="chip-btn px-2 py-0.5 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary transition active:scale-95">Rem</button>
                <button onclick="setFilterChip('Oli & Cairan', this)" class="chip-btn px-2 py-0.5 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary transition active:scale-95">Oli & Cairan</button>
                <button onclick="setFilterChip('Kelistrikan', this)" class="chip-btn px-2 py-0.5 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary transition active:scale-95">Kelistrikan</button>
            </div>

            <!-- Spare Part Mobile Cards List -->
            <div id="partsListContainer" class="space-y-1">
                <!-- Dynamically generated mobile cards -->
            </div>

            <!-- Empty State -->
            <div id="emptyState" class="hidden py-10 text-center theme-text-secondary space-y-1.5">
                <i class="fa-solid fa-boxes-packing text-2xl opacity-40"></i>
                <p class="text-xs font-medium">Tidak ada sparepart ditemukan</p>
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
    <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto theme-glass border-t border-slate-800/40 z-40 px-6 py-2 pb-safe">
        <div class="grid grid-cols-3 gap-1 text-center">
            
            <button onclick="switchTab('stock')" id="nav-stock" class="nav-btn text-ios-blue flex flex-col items-center py-1 rounded-2xl transition active:scale-90">
                <i class="fa-solid fa-boxes-stacked text-lg"></i>
                <span class="text-[10px] font-medium mt-0.5">Stok</span>
            </button>

            <button onclick="switchTab('add')" id="nav-add" class="nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-2xl transition active:scale-90">
                <i class="fa-solid fa-circle-plus text-lg"></i>
                <span class="text-[10px] font-medium mt-0.5">Tambah</span>
            </button>

            <button onclick="switchTab('settings')" id="nav-settings" class="nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-2xl transition active:scale-90">
                <i class="fa-solid fa-gear text-lg"></i>
                <span class="text-[10px] font-medium mt-0.5">Pengaturan</span>
            </button>

        </div>
    </nav>

    <!-- EDIT / DETAIL iOS SHEET MODAL -->
    <div id="editDrawer" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-end justify-center hidden">
        <div class="theme-card border-t border-slate-700/50 w-full max-w-md rounded-t-[32px] p-5 space-y-3.5 max-h-[85vh] overflow-y-auto no-scrollbar animate-sheet shadow-2xl">
            
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
    <div id="storeModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
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
    <div id="toastContainer" class="fixed top-14 left-0 right-0 z-50 flex flex-col items-center pointer-events-none px-4 space-y-2"></div>

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

            saveStorage();
        }

        function saveStorage() {
            localStorage.setItem('mob_spareparts', JSON.stringify(spareparts));
            localStorage.setItem('mob_store_name', storeName);
            localStorage.setItem('mob_store_sub', storeSubtitle);
            localStorage.setItem('mob_app_settings', JSON.stringify(appSettings));
        }

        function saveAppSettingsExplicit() {
            saveStorage();
            showToast("Pengaturan tampilan disimpan", "success");
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

            const body = document.getElementById('appBody');
            body.classList.remove('text-xs', 'text-sm', 'text-base');
            if (appSettings.textSize === 'sm') body.classList.add('text-xs');
            else if (appSettings.textSize === 'lg') body.classList.add('text-base');
            else body.classList.add('text-sm');

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
            renderHeader();
            showToast("Logo header dipilih", "info");
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
                renderHeader();
                showToast("Logo kustom dipasang", "info");
            };
            reader.readAsDataURL(file);
        }

        function changePadding(val) {
            appSettings.padding = parseInt(val);
            applyAppSettings();
        }

        function setTextSize(size) {
            appSettings.textSize = size;
            applyAppSettings();
            showToast(`Ukuran teks: ${size === 'sm' ? 'Kecil' : size === 'lg' ? 'Besar' : 'Sedang'}`, "info");
        }

        function setTheme(themeKey) {
            appSettings.theme = themeKey;
            appSettings.customBg = '';
            appSettings.customText = '';
            applyAppSettings();
            showToast("Tema dipilih", "info");
        }

        function setCustomBgColor(hexColor) {
            appSettings.customBg = hexColor;
            appSettings.theme = 'custom';
            applyAppSettings();
        }

        function setCustomTextColor(hexColor) {
            appSettings.customText = hexColor;
            appSettings.theme = 'custom';
            applyAppSettings();
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
                card.className = "theme-card border rounded-lg p-1.5 shadow-sm active:scale-[0.99] transition duration-150 cursor-pointer";
                
                card.innerHTML = `
                    <div onclick="openDrawer('${p.id}')">
                        <div class="flex items-center gap-1 flex-wrap mb-0.5">
                            <span class="inline-flex items-center px-1 py-0.2 rounded text-[8px] font-bold font-mono bg-ios-blue/20 text-ios-blue border border-ios-blue/30">
                                <i class="fa-solid fa-barcode text-[7px] mr-0.5 opacity-80"></i>${escapeHtml(p.sku)}
                            </span>
                            ${vbeltBadgeHtml}
                            <span class="text-[8px] theme-text-secondary font-medium">| ${escapeHtml(p.category)}</span>
                            <span class="px-1 py-0.2 text-[7px] font-bold border rounded-full shrink-0 ${badgeColor} ml-auto">
                                ${badgeText}
                            </span>
                        </div>
                        <h3 class="text-xs font-bold theme-text-primary leading-tight">${escapeHtml(p.name)}</h3>
                        <p class="text-[9px] theme-text-secondary leading-tight flex items-center gap-1 mt-0.5"><i class="fa-solid fa-car text-[7px]"></i> ${escapeHtml(p.carModel)}</p>
                    </div>

                    <div class="flex items-center justify-between mt-1 pt-1 border-t border-slate-800/30">
                        <div class="flex items-baseline gap-1">
                            <span class="text-[8px] theme-text-secondary uppercase font-semibold">Harga:</span>
                            <span class="text-xs font-bold text-ios-green font-mono">${formatRp(p.sellPrice)}</span>
                        </div>

                        <div class="flex items-center theme-input border rounded-md p-0.5 space-x-1 ml-2 shrink-0">
                            <button onclick="event.stopPropagation(); quickStock('${p.id}', -1)" class="w-4 h-4 rounded theme-card theme-text-primary flex items-center justify-center font-bold active:bg-ios-red/20 active:text-ios-red text-xs transition">-</button>
                            <span class="font-bold text-[11px] theme-text-primary min-w-[14px] text-center">${p.stock}</span>
                            <button onclick="event.stopPropagation(); quickStock('${p.id}', 1)" class="w-4 h-4 rounded theme-card theme-text-primary flex items-center justify-center font-bold active:bg-ios-green/20 active:text-ios-green text-xs transition">+</button>
                        </div>
                    </div>
                `;

                container.appendChild(card);
            });
        }

        function quickStock(id, delta) {
            const index = spareparts.findIndex(p => p.id === id);
            if (index !== -1) {
                const newStock = spareparts[index].stock + delta;
                if (newStock < 0) {
                    showToast("Stok minimal 0", "error");
                    return;
                }
                spareparts[index].stock = newStock;
                saveStorage();
                renderHeader();
                renderCards();
                renderSummary();
                showToast(`Stok ${spareparts[index].name}: ${newStock}`, "success");
            }
        }

        function setFilterChip(chipValue, btnElem) {
            activeFilterChip = chipValue;
            document.querySelectorAll('.chip-btn').forEach(b => {
                b.className = "chip-btn px-3 py-1.5 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary transition active:scale-95";
            });
            btnElem.className = "chip-btn px-3 py-1.5 rounded-full font-medium whitespace-nowrap bg-ios-blue text-white shadow-sm transition active:scale-95";
            applyFilters();
        }

        function filterEmptyStock() {
            switchTab('stock');
            const emptyBtn = document.querySelectorAll('.chip-btn')[1];
            if (emptyBtn) setFilterChip('EMPTY', emptyBtn);
        }

        function clearSearch() {
            document.getElementById('searchInput').value = '';
            document.getElementById('clearSearchBtn').classList.add('hidden');
            applyFilters();
        }

        function getFilteredParts() {
            const queryRaw = (document.getElementById('searchInput').value || '').trim().toLowerCase();
            const clearBtn = document.getElementById('clearSearchBtn');
            if (queryRaw) clearBtn.classList.remove('hidden');
            else clearBtn.classList.add('hidden');

            const tokens = queryRaw.split(/\s+/).filter(t => t.length > 0);

            return spareparts.filter(p => {
                const combinedField = `${p.sku} ${p.name} ${p.category} ${p.vbeltSize || ''} ${p.carModel}`.toLowerCase();
                const matchesSearch = tokens.every(token => combinedField.includes(token));

                let matchesChip = true;
                if (activeFilterChip === 'EMPTY') matchesChip = (p.stock === 0);
                else if (activeFilterChip !== 'ALL') matchesChip = (p.category === activeFilterChip);

                return matchesSearch && matchesChip;
            });
        }

        function applyFilters() {
            renderCards(getFilteredParts());
        }

        function switchTab(tab) {
            const tabViewMap = {
                'stock': 'viewStock',
                'add': 'viewAddPart',
                'settings': 'viewSettings'
            };

            ['stock', 'add', 'settings'].forEach(t => {
                const viewId = tabViewMap[t];
                const view = document.getElementById(viewId);
                const nav = document.getElementById('nav-' + t);

                if (view && nav) {
                    if (t === tab) {
                        view.classList.remove('hidden');
                        nav.className = "nav-btn text-ios-blue flex flex-col items-center py-1 rounded-2xl transition active:scale-90";
                    } else {
                        view.classList.add('hidden');
                        nav.className = "nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-2xl transition active:scale-90";
                    }
                }
            });

            if (tab === 'settings') renderSummary();
        }

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

            if (index !== -1) {
                const category = document.getElementById('editCategory').value;
                spareparts[index] = {
                    id,
                    name: document.getElementById('editName').value.trim(),
                    sku: document.getElementById('editSKU').value.trim(),
                    category: category,
                    vbeltSize: category === 'VBelt' ? document.getElementById('editVBeltSize').value.trim() : '',
                    carModel: document.getElementById('editCarModel').value.trim(),
                    buyPrice: Number(document.getElementById('editBuyPrice').value),
                    sellPrice: Number(document.getElementById('editSellPrice').value),
                    stock: Number(document.getElementById('editStock').value)
                };

                saveStorage();
                renderHeader();
                renderCards();
                renderSummary();
                closeDrawer();
                showToast("Sparepart diperbarui", "success");
            }
        }

        function deletePartFromDrawer() {
            const id = document.getElementById('editPartId').value;
            if (confirm("Hapus sparepart ini?")) {
                spareparts = spareparts.filter(p => p.id !== id);
                saveStorage();
                renderHeader();
                renderCards();
                renderSummary();
                closeDrawer();
                showToast("Sparepart dihapus", "info");
            }
        }

        function generateAddSKU() {
            const cat = document.getElementById('addCategory').value || 'PRT';
            const prefix = cat === 'VBelt' ? 'VBT' : cat.substring(0, 3).toUpperCase();
            document.getElementById('addSKU').value = `${prefix}-${Math.floor(100 + Math.random() * 900)}`;
        }

        function cancelAddPart() {
            const form = document.getElementById('addPartForm');
            if (form) form.reset();
            toggleVBeltField('add');
            generateAddSKU();
            switchTab('stock');
            showToast("Batal menambah", "info");
        }

        function handleSaveNewPart(e) {
            e.preventDefault();
            const category = document.getElementById('addCategory').value;
            const newP = {
                id: Date.now().toString(),
                sku: document.getElementById('addSKU').value.trim(),
                name: document.getElementById('addName').value.trim(),
                category: category,
                vbeltSize: category === 'VBelt' ? document.getElementById('addVBeltSize').value.trim() : '',
                carModel: document.getElementById('addCarModel').value.trim(),
                buyPrice: Number(document.getElementById('addBuyPrice').value),
                sellPrice: Number(document.getElementById('addSellPrice').value),
                stock: Number(document.getElementById('addStock').value)
            };

            spareparts.push(newP);
            saveStorage();
            renderHeader();
            renderCards();
            renderSummary();

            e.target.reset();
            toggleVBeltField('add');
            generateAddSKU();
            showToast("Sparepart ditambahkan!", "success");
            switchTab('stock');
        }

        function renderSummary() {
            document.getElementById('summaryTotalTypes').innerText = spareparts.length;
            
            let totalUnits = 0;
            spareparts.forEach(p => {
                totalUnits += p.stock;
            });

            document.getElementById('summaryTotalUnits').innerText = totalUnits.toLocaleString('id-ID');
        }

        function openEditStoreModal() {
            document.getElementById('inputStoreName').value = storeName;
            document.getElementById('inputStoreSubtitle').value = storeSubtitle;
            document.getElementById('storeModal').classList.remove('hidden');
        }

        function closeStoreModal() {
            document.getElementById('storeModal').classList.add('hidden');
        }

        function saveStoreName() {
            const valName = document.getElementById('inputStoreName').value.trim();
            const valSub = document.getElementById('inputStoreSubtitle').value.trim();
            if (valName) {
                storeName = valName;
                storeSubtitle = valSub || "Inventori Sparepart";
                saveStorage();
                renderHeader();
                closeStoreModal();
                showToast("Identitas toko disimpan", "success");
            }
        }

        function exportToCSV() {
            if (spareparts.length === 0) {
                showToast("Tidak ada data", "error");
                return;
            }

            let csvContent = "data:text/csv;charset=utf-8,SKU,Nama Sparepart,Kategori,Ukuran VBelt,Kesesuaian Mobil,Harga Beli,Harga Jual,Stok\n";
            spareparts.forEach(p => {
                const row = [
                    `"${p.sku}"`,
                    `"${p.name}"`,
                    `"${p.category}"`,
                    `"${p.vbeltSize || ''}"`,
                    `"${p.carModel}"`,
                    p.buyPrice,
                    p.sellPrice,
                    p.stock
                ].join(",");
                csvContent += row + "\n";
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Inventori_Sparepart_${new Date().toISOString().slice(0,10)}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            showToast("Laporan CSV diunduh", "success");
        }

        function resetToDemoData() {
            if (confirm("Reset seluruh data ke sampel default?")) {
                spareparts = [...demoParts];
                storeName = "Toko Sparepart Jaya";
                storeSubtitle = "Inventori Sparepart";
                saveStorage();
                renderHeader();
                renderCards();
                renderSummary();
                showToast("Reset ke data demo", "info");
            }
        }

        function showToast(message, type = "info") {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            
            let bg = "bg-slate-900/90 text-white border-slate-700/80";
            if (type === "success") bg = "bg-ios-green text-white border-ios-green/80";
            if (type === "error") bg = "bg-ios-red text-white border-ios-red/80";

            toast.className = `px-4 py-2.5 rounded-full border text-xs font-semibold shadow-2xl backdrop-blur-md transition-all transform duration-300 opacity-0 translate-y-[-12px] pointer-events-auto ${bg}`;
            toast.innerText = message;

            container.appendChild(toast);

            setTimeout(() => {
                toast.classList.remove('opacity-0', 'translate-y-[-12px]');
            }, 10);

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-[-12px]');
                setTimeout(() => toast.remove(), 300);
            }, 2200);
        }
    </script>
</body>
</html>
