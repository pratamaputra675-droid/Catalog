<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Inventori Sparepart Mobil - Mobile</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        ios: {
                            blue: '#3b82f6',
                            emerald: '#10b981',
                            red: '#ef4444',
                            amber: '#f59e0b'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', '-apple-system', 'BlinkMacSystemFont', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        :root {
            --bg-body: #090d16;
            --bg-card: #131b2e;
            --bg-input: #090d16;
            --border-color: #1e293b;
            --text-primary: #f8fafc;
            --text-secondary: #94a3b8;
        }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            background-color: var(--bg-body);
            color: var(--text-primary);
            -webkit-tap-highlight-color: transparent;
            user-select: none;
            transition: background-color 0.2s ease, color 0.2s ease;
        }
        
        /* Dynamic Theme Applied Classes */
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
        
        .theme-text-primary {
            color: var(--text-primary) !important;
        }
        
        .theme-text-secondary {
            color: var(--text-secondary) !important;
        }

        /* Hide scrollbars for clean app look */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        /* Smooth safe-area padding for mobile notches */
        .pt-safe { padding-top: env(safe-area-inset-top, 12px); }
        .pb-safe { padding-bottom: env(safe-area-inset-bottom, 20px); }
    </style>
</head>
<body id="appBody" class="min-h-screen flex flex-col justify-between max-w-md mx-auto border-x border-slate-800/60 shadow-2xl relative overflow-x-hidden text-sm">

    <!-- TOP HEADER -->
    <header class="sticky top-0 z-30 theme-card backdrop-blur-md border-b pt-safe pb-3 flex items-center justify-between dynamic-padding">
        <div class="flex items-center space-x-2.5">
            <div id="headerLogoContainer" class="w-9 h-9 rounded-xl bg-gradient-to-tr from-blue-600 to-indigo-500 flex items-center justify-center text-white shadow-md shadow-blue-500/20 overflow-hidden shrink-0">
                <i id="headerLogoIcon" class="fa-solid fa-car-wrench text-base"></i>
                <img id="headerLogoImg" src="" alt="Logo" class="w-full h-full object-cover hidden">
            </div>
            <div>
                <h1 id="storeNameText" onclick="openEditStoreModal()" class="font-bold theme-text-primary tracking-tight leading-tight flex items-center gap-1.5 cursor-pointer active:opacity-70 text-base">
                    <span>Toko Sparepart Jaya</span>
                    <i class="fa-solid fa-pen-to-square text-[10px] theme-text-secondary"></i>
                </h1>
                <span id="storeSubtitleText" onclick="openEditStoreModal()" class="text-[11px] theme-text-secondary cursor-pointer active:opacity-70">Inventori Sparepart</span>
            </div>
        </div>

        <!-- Alert Badge / Quick Indicator -->
        <button onclick="filterEmptyStock()" class="relative p-2.5 theme-card rounded-xl border active:scale-95 transition" title="Stok Habis">
            <i class="fa-solid fa-triangle-exclamation text-red-400 text-sm"></i>
            <span id="emptyStockBadge" class="absolute -top-1 -right-1 bg-red-500 text-white text-[10px] font-bold w-4 h-4 rounded-full flex items-center justify-center hidden">0</span>
        </button>
    </header>

    <!-- MAIN CONTENT VIEW CANVAS -->
    <main class="flex-1 py-2 space-y-2.5 pb-24 dynamic-padding">

        <!-- VIEW 1: STOK / INVENTORY LIST -->
        <section id="viewStock" class="space-y-2">
            
            <!-- Quick Search Bar -->
            <div class="relative">
                <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 theme-text-secondary text-xs"></i>
                <input type="text" id="searchInput" oninput="applyFilters()" placeholder="Cari gabungan: Avanza Filter, JZ 02..." class="w-full theme-input border rounded-xl pl-8 pr-7 py-2 text-xs placeholder-slate-400 focus:outline-none focus:border-ios-blue transition">
                <button id="clearSearchBtn" onclick="clearSearch()" class="hidden absolute right-2.5 top-1/2 -translate-y-1/2 theme-text-secondary hover:opacity-80">
                    <i class="fa-solid fa-circle-xmark text-xs"></i>
                </button>
            </div>

            <!-- Filter Chips (Horizontal Scrollable) -->
            <div class="flex items-center space-x-1.5 overflow-x-auto no-scrollbar py-0.5 text-[11px]">
                <button onclick="setFilterChip('ALL', this)" class="chip-btn active-chip px-2.5 py-1 rounded-full font-medium whitespace-nowrap bg-ios-blue text-white shadow-sm">Semua</button>
                <button onclick="setFilterChip('EMPTY', this)" class="chip-btn px-2.5 py-1 rounded-full font-medium whitespace-nowrap theme-card border text-red-400">Stok Habis</button>
                <button onclick="setFilterChip('Mesin', this)" class="chip-btn px-2.5 py-1 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary">Mesin</button>
                <button onclick="setFilterChip('VBelt', this)" class="chip-btn px-2.5 py-1 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary">VBelt</button>
                <button onclick="setFilterChip('Rem & Kaki-kaki', this)" class="chip-btn px-2.5 py-1 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary">Rem</button>
                <button onclick="setFilterChip('Oli & Cairan', this)" class="chip-btn px-2.5 py-1 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary">Oli & Cairan</button>
                <button onclick="setFilterChip('Kelistrikan', this)" class="chip-btn px-2.5 py-1 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary">Kelistrikan</button>
            </div>

            <!-- Spare Part Mobile Cards List -->
            <div id="partsListContainer" class="space-y-2">
                <!-- Dynamically generated mobile cards -->
            </div>

            <!-- Empty State -->
            <div id="emptyState" class="hidden py-8 text-center theme-text-secondary space-y-2">
                <i class="fa-solid fa-boxes-packing text-2xl opacity-50"></i>
                <p class="text-xs font-medium">Tidak ada sparepart ditemukan</p>
            </div>
        </section>


        <!-- VIEW 2: TAMBAH SPAREPART BARU -->
        <section id="viewAddPart" class="hidden space-y-4">
            <div class="theme-card border rounded-2xl p-4 space-y-3.5 shadow-lg">
                <h2 class="text-sm font-bold theme-text-primary flex items-center gap-2">
                    <i class="fa-solid fa-plus-circle text-ios-blue"></i> Tambah Part Baru
                </h2>

                <form id="addPartForm" onsubmit="handleSaveNewPart(event)" class="space-y-3 text-xs">
                    <div>
                        <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Kode / SKU</label>
                        <div class="flex gap-2">
                            <input type="text" id="addSKU" required placeholder="SKU-1001" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                            <button type="button" onclick="generateAddSKU()" class="theme-card border px-3 py-2.5 rounded-xl theme-text-secondary font-medium active:scale-95">Auto</button>
                        </div>
                    </div>

                    <div>
                        <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Nama Sparepart</label>
                        <input type="text" id="addName" required placeholder="Contoh: Kampas Rem Depan Avanza" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                    </div>

                    <div class="grid grid-cols-2 gap-2.5">
                        <div>
                            <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Kategori</label>
                            <select id="addCategory" required onchange="toggleVBeltField('add')" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
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
                            <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Kesesuaian Mobil</label>
                            <input type="text" id="addCarModel" required placeholder="Avanza / Xenia" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                        </div>
                    </div>

                    <!-- KOLOM KHUSUS UKURAN VBELT (ADD) -->
                    <div id="addVBeltSizeContainer" class="hidden">
                        <label class="block text-[11px] font-semibold text-amber-400 uppercase mb-1 flex items-center gap-1">
                            <i class="fa-solid fa-ruler-horizontal text-[10px]"></i> Ukuran VBelt
                        </label>
                        <input type="text" id="addVBeltSize" placeholder="Contoh: 4PK 855 / K-38 / 6PK 1210" class="w-full theme-input border border-amber-500/50 rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-amber-400">
                    </div>

                    <div class="grid grid-cols-2 gap-2.5">
                        <div>
                            <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Harga Beli (Rp)</label>
                            <input type="number" id="addBuyPrice" min="0" required placeholder="150000" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                        </div>
                        <div>
                            <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Harga Jual (Rp)</label>
                            <input type="number" id="addSellPrice" min="0" required placeholder="200000" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                        </div>
                    </div>

                    <div>
                        <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Jumlah Stok Awal</label>
                        <input type="number" id="addStock" min="0" required placeholder="10" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                    </div>

                    <div class="flex gap-2 pt-1">
                        <button type="button" onclick="cancelAddPart()" class="w-1/3 min-h-[44px] theme-card border theme-text-secondary font-semibold rounded-xl transition flex items-center justify-center space-x-1 active:scale-95">
                            <i class="fa-solid fa-xmark"></i>
                            <span>Batal</span>
                        </button>
                        <button type="submit" class="w-2/3 min-h-[44px] bg-ios-emerald hover:bg-emerald-600 text-white font-semibold rounded-xl transition flex items-center justify-center space-x-2 active:scale-98 shadow-md shadow-emerald-500/20">
                            <i class="fa-solid fa-floppy-disk"></i>
                            <span>Simpan Sparepart</span>
                        </button>
                    </div>
                </form>
            </div>
        </section>


        <!-- VIEW 3: PENGATURAN & RINGKASAN DATA -->
        <section id="viewSettings" class="hidden space-y-4">
            
            <!-- Quick Summary Metrics -->
            <div class="grid grid-cols-2 gap-2.5">
                <div class="theme-card border p-3.5 rounded-2xl">
                    <span class="text-[10px] theme-text-secondary uppercase font-semibold">Total Item</span>
                    <h4 id="summaryTotalTypes" class="text-xl font-bold theme-text-primary mt-0.5">0</h4>
                </div>
                <div class="theme-card border p-3.5 rounded-2xl">
                    <span class="text-[10px] theme-text-secondary uppercase font-semibold">Total Unit Stok</span>
                    <h4 id="summaryTotalUnits" class="text-xl font-bold theme-text-primary mt-0.5">0</h4>
                </div>
            </div>

            <!-- TAMPILAN & KUSTOMISASI -->
            <div class="theme-card border rounded-2xl p-4 space-y-3.5">
                <h3 class="text-xs font-bold theme-text-primary uppercase tracking-wider flex items-center gap-2">
                    <i class="fa-solid fa-palette text-ios-blue"></i> Pengaturan Tampilan Header & Tema
                </h3>

                <!-- Logo Header Picker -->
                <div class="space-y-2">
                    <label class="block text-[11px] font-semibold theme-text-secondary uppercase">Logo Header</label>
                    <div class="grid grid-cols-5 gap-2">
                        <button onclick="setLogoPreset('fa-car-wrench')" class="logo-preset-btn p-2 rounded-xl theme-input border flex items-center justify-center theme-text-primary active:scale-95 text-base">
                            <i class="fa-solid fa-car-wrench"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-car')" class="logo-preset-btn p-2 rounded-xl theme-input border flex items-center justify-center theme-text-primary active:scale-95 text-base">
                            <i class="fa-solid fa-car"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-gear')" class="logo-preset-btn p-2 rounded-xl theme-input border flex items-center justify-center theme-text-primary active:scale-95 text-base">
                            <i class="fa-solid fa-gear"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-screwdriver-wrench')" class="logo-preset-btn p-2 rounded-xl theme-input border flex items-center justify-center theme-text-primary active:scale-95 text-base">
                            <i class="fa-solid fa-screwdriver-wrench"></i>
                        </button>
                        <button onclick="setLogoPreset('fa-warehouse')" class="logo-preset-btn p-2 rounded-xl theme-input border flex items-center justify-center theme-text-primary active:scale-95 text-base">
                            <i class="fa-solid fa-warehouse"></i>
                        </button>
                    </div>

                    <!-- Upload Gambar Logo dari Galeri -->
                    <div class="pt-1">
                        <label for="logoFileInput" class="w-full min-h-[40px] theme-input border border-dashed hover:border-ios-blue rounded-xl px-3 flex items-center justify-center gap-2 text-xs theme-text-secondary cursor-pointer active:scale-98">
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
                        <span id="paddingValText" class="theme-text-primary font-mono text-[11px]">px-4</span>
                    </div>
                    <input type="range" id="paddingRange" min="2" max="6" step="1" value="4" oninput="changePadding(this.value)" class="w-full accent-ios-blue cursor-pointer">
                </div>

                <!-- Ukuran Teks (Font Size) -->
                <div class="space-y-1.5 pt-1">
                    <div class="flex justify-between items-center text-xs">
                        <span class="theme-text-secondary font-medium">Ukuran Teks Aplikasi</span>
                        <span id="textSizeText" class="theme-text-primary font-mono text-[11px]">Sedang</span>
                    </div>
                    <div class="grid grid-cols-3 gap-2">
                        <button onclick="setTextSize('sm')" id="btnTextSm" class="px-2 py-1.5 theme-input border theme-text-secondary text-xs rounded-xl">Kecil</button>
                        <button onclick="setTextSize('base')" id="btnTextBase" class="px-2 py-1.5 theme-input border theme-text-secondary text-xs rounded-xl">Sedang</button>
                        <button onclick="setTextSize('lg')" id="btnTextLg" class="px-2 py-1.5 theme-input border theme-text-secondary text-xs rounded-xl">Besar</button>
                    </div>
                </div>

                <!-- Tema Latar Belakang Presets -->
                <div class="space-y-1.5 pt-1">
                    <span class="block theme-text-secondary font-medium text-xs">Preset Tema Latar Belakang</span>
                    <div class="grid grid-cols-3 gap-2 mb-2">
                        <button onclick="setTheme('dark-blue')" title="Gelap Biru" class="h-9 rounded-xl bg-[#090d16] border-2 border-blue-500/60 active:scale-95 flex items-center justify-center text-xs text-slate-200 font-medium">Gelap</button>
                        <button onclick="setTheme('midnight')" title="Midnight Black" class="h-9 rounded-xl bg-[#050508] border border-slate-800 active:scale-95 flex items-center justify-center text-xs text-slate-200 font-medium">Hitam</button>
                        <button onclick="setTheme('slate')" title="Slate Grey" class="h-9 rounded-xl bg-[#0f172a] border border-slate-700 active:scale-95 flex items-center justify-center text-xs text-slate-200 font-medium">Slate</button>
                    </div>
                    <div class="grid grid-cols-3 gap-2">
                        <button onclick="setTheme('emerald')" title="Emerald Green" class="h-9 rounded-xl bg-[#022c22] border border-emerald-800 active:scale-95 flex items-center justify-center text-xs text-slate-200 font-medium">Hijau</button>
                        <button onclick="setTheme('light-clean')" title="Terang Putih" class="h-9 rounded-xl bg-[#f8fafc] border border-slate-300 active:scale-95 flex items-center justify-center text-xs text-slate-800 font-semibold">Terang</button>
                        <button onclick="setTheme('light-soft')" title="Terang Soft Slate" class="h-9 rounded-xl bg-[#e2e8f0] border border-slate-400 active:scale-95 flex items-center justify-center text-xs text-slate-800 font-semibold">Soft Terang</button>
                    </div>
                </div>

                <!-- KUSTOMISASI WARNA BEBAS (CUSTOM COLOR PICKERS) -->
                <div class="space-y-2 pt-2 border-t border-slate-700/50">
                    <span class="block theme-text-secondary font-semibold text-[11px] uppercase tracking-wider">Kustom Warna Manual</span>
                    <div class="grid grid-cols-2 gap-2 text-xs">
                        <div class="theme-input p-2 rounded-xl border flex items-center justify-between">
                            <span class="theme-text-secondary text-[11px]">Warna Latar</span>
                            <input type="color" id="customBgPicker" onchange="setCustomBgColor(this.value)" class="w-7 h-7 bg-transparent rounded cursor-pointer border-0">
                        </div>
                        <div class="theme-input p-2 rounded-xl border flex items-center justify-between">
                            <span class="theme-text-secondary text-[11px]">Warna Teks</span>
                            <input type="color" id="customTextPicker" onchange="setCustomTextColor(this.value)" class="w-7 h-7 bg-transparent rounded cursor-pointer border-0">
                        </div>
                    </div>
                </div>

                <!-- TOMBOL SIMPAN PENGATURAN -->
                <div class="pt-2">
                    <button onclick="saveAppSettingsExplicit()" class="w-full min-h-[44px] bg-ios-blue hover:bg-blue-600 text-white font-semibold rounded-xl transition flex items-center justify-center space-x-2 active:scale-98 shadow-md shadow-blue-500/20">
                        <i class="fa-solid fa-floppy-disk"></i>
                        <span>Simpan Pengaturan</span>
                    </button>
                </div>
            </div>

            <!-- Store Actions Card -->
            <div class="theme-card border rounded-2xl p-4 space-y-3">
                <h3 class="text-xs font-bold theme-text-primary uppercase tracking-wider">Pengelolaan Data</h3>
                
                <div class="space-y-2">
                    <button onclick="openEditStoreModal()" class="w-full min-h-[44px] theme-input border rounded-xl px-3 flex items-center justify-between text-xs theme-text-primary active:scale-98">
                        <span class="flex items-center gap-2"><i class="fa-solid fa-store text-ios-blue"></i> Ubah Nama & Subtitle Toko</span>
                        <i class="fa-solid fa-chevron-right theme-text-secondary text-[10px]"></i>
                    </button>

                    <button onclick="exportToCSV()" class="w-full min-h-[44px] theme-input border rounded-xl px-3 flex items-center justify-between text-xs theme-text-primary active:scale-98">
                        <span class="flex items-center gap-2"><i class="fa-solid fa-file-csv text-emerald-400"></i> Ekspor Laporan (CSV)</span>
                        <i class="fa-solid fa-download theme-text-secondary text-[10px]"></i>
                    </button>

                    <button onclick="resetToDemoData()" class="w-full min-h-[44px] theme-input border border-red-500/30 rounded-xl px-3 flex items-center justify-between text-xs text-red-400 active:scale-98">
                        <span class="flex items-center gap-2"><i class="fa-solid fa-rotate-left"></i> Reset Data ke Demo Default</span>
                        <i class="fa-solid fa-trash-can text-[10px]"></i>
                    </button>
                </div>
            </div>

        </section>

    </main>

    <!-- MOBILE BOTTOM NAVIGATION BAR -->
    <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto theme-card backdrop-blur-md border-t z-40 px-3 py-2 pb-safe">
        <div class="grid grid-cols-3 gap-1 text-center">
            
            <button onclick="switchTab('stock')" id="nav-stock" class="nav-btn text-ios-blue flex flex-col items-center py-1 rounded-xl transition active:scale-90">
                <i class="fa-solid fa-boxes-stacked text-base"></i>
                <span class="text-[10px] font-medium mt-1">Stok</span>
            </button>

            <button onclick="switchTab('add')" id="nav-add" class="nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-xl transition active:scale-90">
                <i class="fa-solid fa-plus-circle text-base"></i>
                <span class="text-[10px] font-medium mt-1">Tambah</span>
            </button>

            <button onclick="switchTab('settings')" id="nav-settings" class="nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-xl transition active:scale-90">
                <i class="fa-solid fa-gear text-base"></i>
                <span class="text-[10px] font-medium mt-1">Pengaturan</span>
            </button>

        </div>
    </nav>

    <!-- EDIT / DETAIL DRAWER MODAL -->
    <div id="editDrawer" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-end justify-center hidden">
        <div class="theme-card border-t w-full max-w-md rounded-t-3xl p-5 space-y-4 max-h-[85vh] overflow-y-auto no-scrollbar">
            
            <div class="flex items-center justify-between border-b border-slate-700/50 pb-3">
                <h3 class="text-sm font-bold theme-text-primary flex items-center gap-2">
                    <i class="fa-solid fa-pen-to-square text-ios-blue"></i> Edit Sparepart
                </h3>
                <button onclick="closeDrawer()" class="w-8 h-8 rounded-full theme-input border flex items-center justify-center theme-text-secondary active:scale-90">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <form onsubmit="handleSaveEditPart(event)" class="space-y-3 text-xs">
                <input type="hidden" id="editPartId">

                <div>
                    <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Nama Part</label>
                    <input type="text" id="editName" required class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                </div>

                <div class="grid grid-cols-2 gap-2.5">
                    <div>
                        <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Kode / SKU</label>
                        <input type="text" id="editSKU" required class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                    </div>
                    <div>
                        <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Kategori</label>
                        <select id="editCategory" required onchange="toggleVBeltField('edit')" class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
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

                <!-- KOLOM KHUSUS UKURAN VBELT (EDIT) -->
                <div id="editVBeltSizeContainer" class="hidden">
                    <label class="block text-[11px] font-semibold text-amber-400 uppercase mb-1 flex items-center gap-1">
                        <i class="fa-solid fa-ruler-horizontal text-[10px]"></i> Ukuran VBelt
                    </label>
                    <input type="text" id="editVBeltSize" placeholder="Contoh: 4PK 855 / K-38 / 6PK 1210" class="w-full theme-input border border-amber-500/50 rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-amber-400">
                </div>

                <div>
                    <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Kesesuaian Mobil</label>
                    <input type="text" id="editCarModel" required class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                </div>

                <div class="grid grid-cols-2 gap-2.5">
                    <div>
                        <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Harga Beli (Rp)</label>
                        <input type="number" id="editBuyPrice" required class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                    </div>
                    <div>
                        <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Harga Jual (Rp)</label>
                        <input type="number" id="editSellPrice" required class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                    </div>
                </div>

                <div>
                    <label class="block text-[11px] font-semibold theme-text-secondary uppercase mb-1">Stok Saat Ini</label>
                    <input type="number" id="editStock" required class="w-full theme-input border rounded-xl px-3 py-2.5 text-xs focus:outline-none focus:border-ios-blue">
                </div>

                <div class="pt-2 flex gap-2">
                    <button type="button" onclick="deletePartFromDrawer()" class="w-1/3 min-h-[44px] bg-red-500/20 hover:bg-red-500/30 border border-red-500/30 text-red-400 font-semibold rounded-xl transition flex items-center justify-center space-x-1 active:scale-95">
                        <i class="fa-solid fa-trash-can"></i>
                        <span>Hapus</span>
                    </button>
                    <button type="submit" class="w-2/3 min-h-[44px] bg-ios-blue hover:bg-blue-600 text-white font-semibold rounded-xl transition flex items-center justify-center space-x-2 active:scale-98 shadow-md shadow-blue-500/20">
                        <i class="fa-solid fa-floppy-disk"></i>
                        <span>Simpan Perubahan</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL EDIT STORE NAME & SUBTITLE -->
    <div id="storeModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="theme-card border rounded-2xl w-full max-w-xs p-4 space-y-3 shadow-2xl">
            <h3 class="text-xs font-bold theme-text-primary uppercase flex items-center gap-1.5">
                <i class="fa-solid fa-store text-ios-blue"></i> Ubah Identitas Toko
            </h3>
            <div>
                <label class="block text-[10px] theme-text-secondary uppercase mb-1 font-semibold">Nama Toko</label>
                <input type="text" id="inputStoreName" placeholder="Toko Sparepart Jaya" class="w-full theme-input border rounded-xl px-3 py-2 text-xs focus:outline-none focus:border-ios-blue">
            </div>
            <div>
                <label class="block text-[10px] theme-text-secondary uppercase mb-1 font-semibold">Teks Subtitle (Dibawah Nama Toko)</label>
                <input type="text" id="inputStoreSubtitle" placeholder="Inventori Sparepart" class="w-full theme-input border rounded-xl px-3 py-2 text-xs focus:outline-none focus:border-ios-blue">
            </div>
            <div class="flex justify-end space-x-2 pt-1 text-xs">
                <button type="button" onclick="closeStoreModal()" class="px-3 py-2 theme-input border theme-text-secondary rounded-xl font-medium active:scale-95">Batal</button>
                <button type="button" onclick="saveStoreName()" class="px-3 py-2 bg-ios-blue text-white rounded-xl font-semibold active:scale-95 shadow-md shadow-blue-500/20">Simpan</button>
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
            padding: 4,
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
            showToast("Pengaturan tampilan berhasil disimpan!", "success");
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

        // DYNAMIC COLOR HELPER FUNCTIONS
        function adjustHexColor(hex, percent) {
            let num = parseInt(hex.replace('#',''), 16),
                amt = Math.round(2.55 * percent),
                R = (num >> 16) + amt,
                G = (num >> 8 & 0x00FF) + amt,
                B = (num & 0x0000FF) + amt;
            return '#' + (0x1000000 + (R<255?R<0?0:R:255)*0x10000 + (G<255?G<0?0:G:255)*0x100 + (B<255?B<0?0:B:255)).toString(16).slice(1);
        }

        function applyAppSettings() {
            const paddingMap = { 2: 'px-2', 3: 'px-3', 4: 'px-4', 5: 'px-5', 6: 'px-6' };
            const selectedPaddingClass = paddingMap[appSettings.padding] || 'px-4';
            
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
                if (btn) {
                    btn.classList.remove('bg-ios-blue', 'text-white');
                }
            });

            if (appSettings.textSize === 'sm') document.getElementById('btnTextSm')?.classList.add('bg-ios-blue', 'text-white');
            if (appSettings.textSize === 'base') document.getElementById('btnTextBase')?.classList.add('bg-ios-blue', 'text-white');
            if (appSettings.textSize === 'lg') document.getElementById('btnTextLg')?.classList.add('bg-ios-blue', 'text-white');

            const root = document.documentElement;
            
            // THEME DEFINITIONS
            const themes = {
                'dark-blue': { bg: '#090d16', card: '#131b2e', input: '#0d1322', border: '#1e293b', textPrimary: '#f8fafc', textSecondary: '#94a3b8' },
                'midnight': { bg: '#050508', card: '#0f0f17', input: '#09090f', border: '#1e1e2e', textPrimary: '#f8fafc', textSecondary: '#94a3b8' },
                'slate': { bg: '#0f172a', card: '#1e293b', input: '#0f172a', border: '#334155', textPrimary: '#f8fafc', textSecondary: '#94a3b8' },
                'emerald': { bg: '#022c22', card: '#064e3b', input: '#022c22', border: '#047857', textPrimary: '#f8fafc', textSecondary: '#a7f3d0' },
                'light-clean': { bg: '#f8fafc', card: '#ffffff', input: '#f1f5f9', border: '#cbd5e1', textPrimary: '#0f172a', textSecondary: '#64748b' },
                'light-soft': { bg: '#e2e8f0', card: '#f1f5f9', input: '#ffffff', border: '#cbd5e1', textPrimary: '#1e293b', textSecondary: '#475569' }
            };

            let activeTheme = themes[appSettings.theme] || themes['dark-blue'];

            if (appSettings.theme === 'custom') {
                const bg = appSettings.customBg || '#090d16';
                const text = appSettings.customText || '#f8fafc';
                activeTheme = {
                    bg: bg,
                    card: adjustHexColor(bg, 12),
                    input: adjustHexColor(bg, -5),
                    border: adjustHexColor(bg, 25),
                    textPrimary: text,
                    textSecondary: adjustHexColor(text, -30)
                };
            }

            root.style.setProperty('--bg-body', activeTheme.bg);
            root.style.setProperty('--bg-card', activeTheme.card);
            root.style.setProperty('--bg-input', activeTheme.input);
            root.style.setProperty('--border-color', activeTheme.border);
            root.style.setProperty('--text-primary', activeTheme.textPrimary);
            root.style.setProperty('--text-secondary', activeTheme.textSecondary);
        }

        // RENDER HEADER
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
                logoIcon.className = `fa-solid ${appSettings.logoValue || 'fa-car-wrench'} text-base`;
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

        // CUSTOMIZATION SETTINGS FUNCTIONS
        function setLogoPreset(iconClass) {
            appSettings.logoType = 'preset';
            appSettings.logoValue = iconClass;
            document.getElementById('uploadLogoText').innerText = 'Upload Logo dari Galeri';
            renderHeader();
            showToast("Preset logo dipilih", "info");
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
                document.getElementById('uploadLogoText').innerText = `Logo Terpasang: ${file.name.substring(0, 15)}...`;
                renderHeader();
                showToast("Logo kustom dipilih dari galeri", "info");
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
            showToast("Preset tema dipilih", "info");
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

        // TOGGLE VBELT FIELD VISIBILITY
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

        // RENDER CARDS (HOME)
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
                let badgeColor = "bg-emerald-500/10 text-emerald-400 border-emerald-500/20";
                let badgeText = "Aman";

                if (p.stock === 0) {
                    badgeColor = "bg-red-500/10 text-red-400 border-red-500/20";
                    badgeText = "Habis";
                }

                // VBelt Size Badge
                const vbeltBadgeHtml = (p.category === 'VBelt' && p.vbeltSize) 
                    ? `<span class="inline-flex items-center px-1.5 py-0.2 rounded text-[9px] font-bold bg-amber-500/20 text-amber-300 border border-amber-400/30">
                        <i class="fa-solid fa-ruler-horizontal text-[8px] mr-1"></i>${escapeHtml(p.vbeltSize)}
                       </span>` 
                    : '';

                const card = document.createElement('div');
                card.className = "theme-card border rounded-xl p-2.5 shadow-sm flex flex-col justify-between space-y-1.5 active:opacity-90 transition";
                
                card.innerHTML = `
                    <div class="flex items-start justify-between gap-1.5 cursor-pointer" onclick="openDrawer('${p.id}')">
                        <div class="space-y-0.5">
                            <div class="flex items-center gap-1 flex-wrap">
                                <!-- HIGHLIGHTED COLOR BACKGROUND SKU BADGE -->
                                <span class="inline-flex items-center px-1.5 py-0.2 rounded text-[9px] font-bold font-mono bg-blue-500/20 text-blue-400 border border-blue-400/30 shadow-sm">
                                    <i class="fa-solid fa-barcode text-[8px] mr-1 opacity-70"></i>${escapeHtml(p.sku)}
                                </span>
                                ${vbeltBadgeHtml}
                                <span class="text-[9px] theme-text-secondary">| ${escapeHtml(p.category)}</span>
                            </div>
                            <h3 class="text-xs font-bold theme-text-primary leading-tight">${escapeHtml(p.name)}</h3>
                            <p class="text-[10px] theme-text-secondary leading-none"><i class="fa-solid fa-car text-[9px] mr-1"></i>${escapeHtml(p.carModel)}</p>
                        </div>
                        <span class="px-1.5 py-0.2 text-[9px] font-semibold border rounded-full shrink-0 ${badgeColor}">
                            ${badgeText}
                        </span>
                    </div>

                    <div class="flex items-center justify-between border-t border-slate-700/50 pt-1.5">
                        <div class="flex items-baseline gap-1">
                            <span class="text-[9px] theme-text-secondary uppercase">Harga:</span>
                            <span class="text-xs font-bold text-emerald-400 font-mono">${formatRp(p.sellPrice)}</span>
                        </div>

                        <div class="flex items-center theme-input border rounded-lg px-1 py-0.5 space-x-1.5">
                            <button onclick="event.stopPropagation(); quickStock('${p.id}', -1)" class="w-6 h-6 rounded theme-card theme-text-primary flex items-center justify-center font-bold active:bg-red-500/20 active:text-red-400 text-xs">-</button>
                            <span class="font-bold text-xs theme-text-primary min-w-[18px] text-center">${p.stock}</span>
                            <button onclick="event.stopPropagation(); quickStock('${p.id}', 1)" class="w-6 h-6 rounded theme-card theme-text-primary flex items-center justify-center font-bold active:bg-emerald-500/20 active:text-emerald-400 text-xs">+</button>
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

        // FILTER CHIPS LOGIC
        function setFilterChip(chipValue, btnElem) {
            activeFilterChip = chipValue;
            document.querySelectorAll('.chip-btn').forEach(b => {
                b.className = "chip-btn px-3 py-1.5 rounded-full font-medium whitespace-nowrap theme-card border theme-text-secondary";
            });
            btnElem.className = "chip-btn px-3 py-1.5 rounded-full font-medium whitespace-nowrap bg-ios-blue text-white shadow-sm";
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

        // MULTI-FIELD & MULTI-KEYWORD COMBINATION SEARCH
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

        // TAB NAVIGATION
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
                        nav.className = "nav-btn text-ios-blue flex flex-col items-center py-1 rounded-xl transition active:scale-90";
                    } else {
                        view.classList.add('hidden');
                        nav.className = "nav-btn theme-text-secondary flex flex-col items-center py-1 rounded-xl transition active:scale-90";
                    }
                }
            });

            if (tab === 'settings') renderSummary();
        }

        // DRAWER (EDIT / DELETE)
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
                showToast("Data sparepart diperbarui", "success");
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

        // ADD PART LOGIC
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
            showToast("Batal menambah sparepart", "info");
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
            showToast("Sparepart baru ditambahkan!", "success");
            switchTab('stock');
        }

        // SETTINGS & SUMMARY
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
                showToast("Nama & Subtitle toko disimpan", "success");
            }
        }

        function exportToCSV() {
            if (spareparts.length === 0) {
                showToast("Tidak ada data untuk diekspor", "error");
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
            showToast("Laporan CSV berhasil diunduh", "success");
        }

        function resetToDemoData() {
            if (confirm("Reset seluruh data ke sampel awal default?")) {
                spareparts = [...demoParts];
                storeName = "Toko Sparepart Jaya";
                storeSubtitle = "Inventori Sparepart";
                saveStorage();
                renderHeader();
                renderCards();
                renderSummary();
                showToast("Data di-reset ke data demo", "info");
            }
        }

        // TOAST NOTIFICATION
        function showToast(message, type = "info") {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            
            let bg = "bg-slate-800 text-white border-slate-700";
            if (type === "success") bg = "bg-emerald-600 text-white border-emerald-500";
            if (type === "error") bg = "bg-red-600 text-white border-red-500";

            toast.className = `px-3.5 py-2 rounded-xl border text-xs font-medium shadow-lg transition-all transform duration-300 opacity-0 translate-y-[-10px] pointer-events-auto ${bg}`;
            toast.innerText = message;

            container.appendChild(toast);

            setTimeout(() => {
                toast.classList.remove('opacity-0', 'translate-y-[-10px]');
            }, 10);

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-[-10px]');
                setTimeout(() => toast.remove(), 300);
            }, 2500);
        }
    </script>
</body>
</html>
