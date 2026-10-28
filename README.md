
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>iPhone AutoSpare Inventori</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Inter Font -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            -webkit-tap-highlight-color: transparent;
            transition: background-color 0.2s ease, color 0.2s ease;
        }
        /* Custom Scrollbar for iOS feel */
        ::-webkit-scrollbar {
            width: 0px;
            background: transparent;
        }

        /* TEMA: DARK (DEFAULT) */
        body.theme-dark {
            background-color: #0d0d0e;
            color: #f3f4f6;
        }
        body.theme-dark .app-bg-card {
            background-color: #17171c;
            border-color: #26262e;
            color: #f3f4f6;
        }
        body.theme-dark .app-input-bg {
            background-color: #26262e;
            border-color: #383842;
            color: #ffffff;
        }
        body.theme-dark .glass-nav {
            background: rgba(18, 18, 22, 0.85);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-top-color: rgba(38, 38, 46, 0.8);
        }

        /* TEMA: LIGHT */
        body.theme-light {
            background-color: #f2f2f7;
            color: #1c1c1e;
        }
        body.theme-light .app-bg-card {
            background-color: #ffffff;
            border-color: #e5e5ea;
            color: #1c1c1e;
        }
        body.theme-light .app-input-bg {
            background-color: #e5e5ea;
            border-color: #d1d1d6;
            color: #000000;
        }
        body.theme-light .glass-nav {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-top-color: rgba(200, 200, 206, 0.8);
        }

        /* TEMA: MIDNIGHT */
        body.theme-midnight {
            background-color: #091322;
            color: #e2e8f0;
        }
        body.theme-midnight .app-bg-card {
            background-color: #112240;
            border-color: #1d3557;
            color: #e2e8f0;
        }
        body.theme-midnight .app-input-bg {
            background-color: #1d3557;
            border-color: #2a4a75;
            color: #ffffff;
        }
        body.theme-midnight .glass-nav {
            background: rgba(10, 25, 47, 0.85);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-top-color: rgba(29, 53, 87, 0.8);
        }

        /* UKURAN TEKS */
        .text-size-sm { font-size: 0.875rem; }
        .text-size-base { font-size: 1rem; }
        .text-size-lg { font-size: 1.125rem; }
    </style>
</head>
<body class="theme-dark min-h-screen pb-24" id="app-body">

    <!-- Container Utama App (Padding Horizontal & Max Width Diatur Dinamis) -->
    <div class="max-w-md mx-auto transition-all duration-300 px-4" id="app-container">

        <!-- HEADER UTAMA -->
        <header class="pt-6 pb-4">
            <div class="flex items-center justify-between gap-3">
                <div class="flex items-center gap-3 overflow-hidden">
                    <!-- Custom Logo Preview -->
                    <img id="header-logo" src="" alt="Logo" class="w-12 h-12 rounded-2xl object-cover border border-neutral-700 shadow-md hidden shrink-0">
                    <div class="truncate">
                        <h1 id="header-title-text" class="text-2xl font-bold tracking-tight leading-tight truncate">
                            Inventori Sparepart
                        </h1>
                        <p id="header-subtitle-text" class="text-xs font-medium opacity-70 truncate">
                            AutoParts HQ
                        </p>
                    </div>
                </div>

                <!-- Quick Action Modal trigger -->
                <button onclick="openModal()" class="w-10 h-10 rounded-full bg-blue-600 hover:bg-blue-500 active:scale-95 text-white flex items-center justify-center shadow-lg shadow-blue-600/30 transition-transform shrink-0">
                    <i data-lucide="plus" class="w-5 h-5"></i>
                </button>
            </div>
        </header>

        <!-- VIEWS / PAGES -->
        <main>
            <!-- VIEW 1: UTAMA / INVENTORI -->
            <section id="view-inventory" class="space-y-4">
                
                <!-- Summary Stats Bar -->
                <div id="stats-banner" class="grid grid-cols-2 gap-3">
                    <div class="app-bg-card border rounded-2xl p-3 flex items-center gap-3">
                        <div class="w-9 h-9 rounded-xl bg-blue-500/10 text-blue-400 flex items-center justify-center shrink-0">
                            <i data-lucide="package" class="w-5 h-5"></i>
                        </div>
                        <div>
                            <p class="text-[11px] opacity-70 font-medium">Total Item</p>
                            <p id="stat-total-items" class="text-base font-bold">0</p>
                        </div>
                    </div>
                    <div class="app-bg-card border rounded-2xl p-3 flex items-center gap-3">
                        <div class="w-9 h-9 rounded-xl bg-amber-500/10 text-amber-400 flex items-center justify-center shrink-0">
                            <i data-lucide="alert-triangle" class="w-5 h-5"></i>
                        </div>
                        <div>
                            <p class="text-[11px] opacity-70 font-medium">Stok Kritis</p>
                            <p id="stat-low-stock" class="text-base font-bold text-amber-500">0</p>
                        </div>
                    </div>
                </div>

                <!-- Control Bar: Search & Category Filter -->
                <div class="space-y-2.5">
                    <!-- Multi-term Search Field -->
                    <div class="relative">
                        <i data-lucide="search" class="w-4 h-4 absolute left-3.5 top-3 text-neutral-400"></i>
                        <input type="text" id="search-input" oninput="renderItems()" placeholder="Cari sparepart, kode, merek, atau mobil..." 
                               class="w-full app-input-bg border rounded-xl py-2 pl-10 pr-4 text-sm focus:outline-none focus:border-blue-500 transition-colors">
                    </div>

                    <!-- Category Chips -->
                    <div class="flex gap-2 overflow-x-auto pb-1 scrollbar-none text-xs" id="category-chips">
                        <!-- Populated by JS -->
                    </div>
                </div>

                <!-- Inventory Item List Container -->
                <div id="inventory-list" class="space-y-2.5">
                    <!-- Populated dynamically -->
                </div>

                <!-- Empty State -->
                <div id="empty-state" class="hidden text-center py-12">
                    <i data-lucide="inbox" class="w-12 h-12 text-neutral-500 mx-auto mb-2 opacity-50"></i>
                    <p class="text-sm font-medium opacity-70">Tidak ada sparepart ditemukan</p>
                </div>
            </section>

            <!-- VIEW 2: LAPORAN / STATISTIK -->
            <section id="view-reports" class="hidden space-y-4">
                <h2 class="text-lg font-bold">Ringkasan Laporan</h2>
                <div class="app-bg-card border rounded-2xl p-4 space-y-3">
                    <div class="flex justify-between items-center text-sm border-b border-neutral-700/40 pb-2">
                        <span class="opacity-70">Total Jenis Barang</span>
                        <span id="rep-total-types" class="font-bold">0</span>
                    </div>
                    <div class="flex justify-between items-center text-sm border-b border-neutral-700/40 pb-2">
                        <span class="opacity-70">Total Kuantitas Stok</span>
                        <span id="rep-total-qty" class="font-bold">0</span>
                    </div>
                    <div class="flex justify-between items-center text-sm border-b border-neutral-700/40 pb-2">
                        <span class="opacity-70">Estimasi Nilai Modal</span>
                        <span id="rep-total-cost" class="font-bold text-emerald-500">Rp 0</span>
                    </div>
                    <div class="flex justify-between items-center text-sm">
                        <span class="opacity-70">Estimasi Nilai Jual</span>
                        <span id="rep-total-asset" class="font-bold text-blue-500">Rp 0</span>
                    </div>
                </div>
            </section>

            <!-- VIEW 3: PENGATURAN -->
            <section id="view-settings" class="hidden space-y-5">
                <h2 class="text-lg font-bold">Pengaturan Aplikasi</h2>

                <!-- Atur Header & Logo -->
                <div class="app-bg-card border rounded-2xl p-4 space-y-3">
                    <h3 class="text-xs font-semibold opacity-70 uppercase tracking-wider">Identitas Header</h3>
                    <div>
                        <label class="block text-xs opacity-70 mb-1">Judul Utama</label>
                        <input type="text" id="set-header-title" onchange="updateSettingsFromForm()" class="w-full app-input-bg border rounded-xl p-2.5 text-sm">
                    </div>
                    <div>
                        <label class="block text-xs opacity-70 mb-1">Subtitle</label>
                        <input type="text" id="set-header-subtitle" onchange="updateSettingsFromForm()" class="w-full app-input-bg border rounded-xl p-2.5 text-sm">
                    </div>
                    <div>
                        <label class="block text-xs opacity-70 mb-1">Logo Toko (Galeri)</label>
                        <div class="flex items-center gap-2">
                            <input type="file" id="logo-input" accept="image/*" onchange="handleLogoUpload(event)" class="hidden">
                            <button onclick="document.getElementById('logo-input').click()" class="app-input-bg hover:opacity-80 border rounded-xl px-3 py-2 text-xs font-medium">Pilih Foto</button>
                            <button onclick="removeLogo()" class="text-xs text-red-500 hover:underline">Hapus Logo</button>
                        </div>
                    </div>
                </div>

                <!-- Tampilan Layar Utama & Tema (FUNGSI BARU) -->
                <div class="app-bg-card border rounded-2xl p-4 space-y-3">
                    <h3 class="text-xs font-semibold opacity-70 uppercase tracking-wider">Tampilan & Tema</h3>
                    
                    <div>
                        <label class="block text-xs opacity-70 mb-1">Tema Aplikasi</label>
                        <select id="set-theme" onchange="updateSettingsFromForm()" class="w-full app-input-bg border rounded-xl p-2.5 text-sm">
                            <option value="theme-dark">Gelap (Dark Mode)</option>
                            <option value="theme-light">Terang (Light Mode)</option>
                            <option value="theme-midnight">Midnight Blue</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs opacity-70 mb-1">Ukuran Teks</label>
                        <select id="set-text-size" onchange="updateSettingsFromForm()" class="w-full app-input-bg border rounded-xl p-2.5 text-sm">
                            <option value="text-size-sm">Kecil</option>
                            <option value="text-size-base">Sedang (Default)</option>
                            <option value="text-size-lg">Besar</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs opacity-70 mb-1">Jarak Kanan-Kiri (Padding Sisi)</label>
                        <select id="set-padding" onchange="updateSettingsFromForm()" class="w-full app-input-bg border rounded-xl p-2.5 text-sm">
                            <option value="px-2">Rapat (Kecil - 8px)</option>
                            <option value="px-4">Normal (Sedang - 16px)</option>
                            <option value="px-6">Longgar (Besar - 24px)</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs opacity-70 mb-1">Mode Kartu Item</label>
                        <select id="set-view-mode" onchange="updateSettingsFromForm()" class="w-full app-input-bg border rounded-xl p-2.5 text-sm">
                            <option value="detailed">Detail Lengkap</option>
                            <option value="compact">Ringkas (Compact)</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs opacity-70 mb-1">Urutkan Daftar Berdasarkan</label>
                        <select id="set-sort-by" onchange="updateSettingsFromForm()" class="w-full app-input-bg border rounded-xl p-2.5 text-sm">
                            <option value="name-asc">Nama (A - Z)</option>
                            <option value="name-desc">Nama (Z - A)</option>
                            <option value="stock-asc">Stok Terkecil</option>
                            <option value="stock-desc">Stok Terbanyak</option>
                        </select>
                    </div>

                    <div class="flex items-center justify-between pt-1">
                        <span class="text-xs">Tampilkan Banner Ringkasan</span>
                        <input type="checkbox" id="set-show-stats" onchange="updateSettingsFromForm()" class="w-4 h-4 rounded accent-blue-600">
                    </div>
                </div>

                <!-- Bagikan Data -->
                <div class="app-bg-card border rounded-2xl p-4 space-y-3">
                    <h3 class="text-xs font-semibold opacity-70 uppercase tracking-wider">Bagikan Data</h3>
                    <div class="grid grid-cols-2 gap-2">
                        <button onclick="shareInventoryData('wa')" class="bg-emerald-600 hover:bg-emerald-500 text-white rounded-xl p-2.5 text-xs font-medium flex items-center justify-center gap-2">
                            <i data-lucide="message-square" class="w-4 h-4"></i> WhatsApp
                        </button>
                        <button onclick="shareInventoryData('copy')" class="app-input-bg hover:opacity-80 border rounded-xl p-2.5 text-xs font-medium flex items-center justify-center gap-2">
                            <i data-lucide="copy" class="w-4 h-4"></i> Salin Teks
                        </button>
                    </div>
                </div>
            </section>
        </main>

        <!-- NAVIGASI BAWAH (RATA & SIMETRIS) -->
        <nav class="fixed bottom-0 left-0 right-0 glass-nav border-t z-40">
            <div class="max-w-md mx-auto flex items-center justify-around px-2 py-2 text-center">
                <button onclick="switchTab('inventory')" id="nav-inventory" class="flex-1 flex flex-col items-center py-1 text-blue-500 transition-colors">
                    <i data-lucide="box" class="w-5 h-5 mb-0.5"></i>
                    <span class="text-[10px] font-medium">Stok</span>
                </button>
                <button onclick="switchTab('reports')" id="nav-reports" class="flex-1 flex flex-col items-center py-1 opacity-60 hover:opacity-100 transition-colors">
                    <i data-lucide="bar-chart-3" class="w-5 h-5 mb-0.5"></i>
                    <span class="text-[10px] font-medium">Laporan</span>
                </button>
                <button onclick="switchTab('settings')" id="nav-settings" class="flex-1 flex flex-col items-center py-1 opacity-60 hover:opacity-100 transition-colors">
                    <i data-lucide="settings" class="w-5 h-5 mb-0.5"></i>
                    <span class="text-[10px] font-medium">Pengaturan</span>
                </button>
                <button onclick="openModal()" class="flex-1 flex flex-col items-center py-1 text-blue-500 hover:text-blue-400 transition-colors">
                    <i data-lucide="plus-circle" class="w-5 h-5 mb-0.5"></i>
                    <span class="text-[10px] font-medium">Tambah</span>
                </button>
            </div>
        </nav>

        <!-- MODAL FORM TAMBAH / EDIT ITEM (DENGAN TOMBOL HAPUS) -->
        <div id="modal-backdrop" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 hidden flex items-end sm:items-center justify-center p-0 sm:p-4">
            <div class="app-bg-card w-full max-w-md rounded-t-3xl sm:rounded-3xl border p-5 space-y-4 max-h-[90vh] overflow-y-auto">
                <div class="flex justify-between items-center pb-2 border-b border-neutral-700/40">
                    <h3 id="modal-title" class="font-bold text-base">Tambah Sparepart</h3>
                    <button onclick="closeModal()" class="opacity-60 hover:opacity-100 p-1">
                        <i data-lucide="x" class="w-5 h-5"></i>
                    </button>
                </div>

                <form id="part-form" onsubmit="savePart(event)" class="space-y-3">
                    <input type="hidden" id="part-id">

                    <div>
                        <label class="block text-xs font-medium opacity-70 mb-1">Nama Sparepart *</label>
                        <input type="text" id="part-name" required placeholder="Contoh: Kampas Rem Depan" 
                               class="w-full app-input-bg border rounded-xl p-2.5 text-sm focus:outline-none focus:border-blue-500">
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium opacity-70 mb-1">Kode OEM / Part *</label>
                            <input type="text" id="part-code" required placeholder="Contoh: 04465-YZZE1" 
                                   class="w-full app-input-bg border rounded-xl p-2.5 text-sm font-mono focus:outline-none focus:border-blue-500">
                        </div>
                        <div>
                            <label class="block text-xs font-medium opacity-70 mb-1">Kategori *</label>
                            <select id="part-category" onchange="toggleVbeltSizeField()" required class="w-full app-input-bg border rounded-xl p-2.5 text-sm focus:outline-none focus:border-blue-500">
                                <option value="Mesin">Mesin</option>
                                <option value="Rem">Rem</option>
                                <option value="Kaki-kaki">Kaki-kaki</option>
                                <option value="Kelistrikan">Kelistrikan</option>
                                <option value="V-Belt">V-Belt</option>
                                <option value="Lainnya">Lainnya</option>
                            </select>
                        </div>
                    </div>

                    <!-- Input Khusus V-Belt -->
                    <div id="vbelt-field-container" class="hidden">
                        <label class="block text-xs font-medium text-purple-400 mb-1">Ukuran V-Belt *</label>
                        <input type="text" id="part-vbelt" placeholder="Contoh: 6PK1880" 
                               class="w-full app-input-bg border border-purple-500/50 rounded-xl p-2.5 text-sm font-mono focus:outline-none focus:border-purple-500">
                    </div>

                    <div>
                        <label class="block text-xs font-medium opacity-70 mb-1">Jenis Mobil / Compatibility</label>
                        <input type="text" id="part-car" placeholder="Contoh: Avanza / Xenia 2012-2020" 
                               class="w-full app-input-bg border rounded-xl p-2.5 text-sm focus:outline-none focus:border-blue-500">
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium opacity-70 mb-1">Stok Saat Ini</label>
                            <input type="number" id="part-stock" min="0" required placeholder="10" 
                                   class="w-full app-input-bg border rounded-xl p-2.5 text-sm focus:outline-none focus:border-blue-500">
                        </div>
                        <div>
                            <label class="block text-xs font-medium opacity-70 mb-1">Merek Sparepart</label>
                            <input type="text" id="part-location" placeholder="Contoh: Denso / Aspira / Genuine" 
                                   class="w-full app-input-bg border rounded-xl p-2.5 text-sm focus:outline-none focus:border-blue-500">
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium opacity-70 mb-1">Harga Beli (Modal)</label>
                            <input type="number" id="part-cost" min="0" placeholder="Rp 0" 
                                   class="w-full app-input-bg border rounded-xl p-2.5 text-sm focus:outline-none focus:border-blue-500">
                        </div>
                        <div>
                            <label class="block text-xs font-medium opacity-70 mb-1">Harga Jual *</label>
                            <input type="number" id="part-price" min="0" required placeholder="Rp 0" 
                                   class="w-full app-input-bg border rounded-xl p-2.5 text-sm focus:outline-none focus:border-blue-500">
                        </div>
                    </div>

                    <div class="pt-2 flex flex-col gap-2">
                        <div class="flex gap-3">
                            <button type="button" onclick="closeModal()" class="flex-1 app-input-bg hover:opacity-80 py-3 rounded-xl font-medium text-sm">Batal</button>
                            <button type="submit" class="flex-1 bg-blue-600 hover:bg-blue-500 text-white py-3 rounded-xl font-medium text-sm">Simpan</button>
                        </div>
                        <!-- TOMBOL HAPUS SPAREPART -->
                        <button type="button" id="btn-delete-part" onclick="deleteCurrentPart()" class="hidden w-full bg-red-600/20 hover:bg-red-600/30 text-red-500 border border-red-500/30 py-2.5 rounded-xl font-medium text-sm flex items-center justify-center gap-2 transition-colors">
                            <i data-lucide="trash-2" class="w-4 h-4"></i> Hapus Sparepart
                        </button>
                    </div>
                </form>
            </div>
        </div>

    </div>

    <!-- Script Aplikasi -->
    <script>
        // Initial Mock Data
        let inventory = [
            { id: '1', name: 'Filter Oli Mesin', code: '15601-BZ010', category: 'Mesin', car: 'Avanza / Xenia / Rush', stock: 24, costPrice: 22000, price: 35000, location: 'Toyota OEM', vbeltSize: '' },
            { id: '2', name: 'Kampas Rem Depan (Brake Pad)', code: '04465-YZZE1', category: 'Rem', car: 'Innova / Fortuner 2016+', stock: 3, costPrice: 270000, price: 380000, location: 'Akebono', vbeltSize: '' },
            { id: '3', name: 'Shockbreaker Belakang', code: '48530-BZ120', category: 'Kaki-kaki', car: 'Xpander / Livina', stock: 6, costPrice: 410000, price: 520000, location: 'KYB Kayaba', vbeltSize: '' },
            { id: '4', name: 'Busi Iridium Tough', code: 'VCH16', category: 'Kelistrikan', car: 'Brio / Jazz GK5', stock: 18, costPrice: 70000, price: 95000, location: 'Denso', vbeltSize: '' },
            { id: '5', name: 'Fan Belt / Tali Kipas', code: '9004A-91049', category: 'V-Belt', car: 'Avanza / Xenia 1.3', stock: 12, costPrice: 78000, price: 110000, location: 'Bando', vbeltSize: '6PK1880' }
        ];

        let categories = ['All', 'Mesin', 'Rem', 'Kaki-kaki', 'Kelistrikan', 'V-Belt', 'Lainnya'];
        let selectedCategory = 'All';

        let settings = {
            title: 'Inventori Sparepart',
            subtitle: 'AutoParts HQ',
            logo: '',
            viewMode: 'detailed',
            sortBy: 'name-asc',
            showStats: true,
            theme: 'theme-dark',
            textSize: 'text-size-base',
            padding: 'px-4'
        };

        // Window Onload Initialization
        window.onload = function() {
            lucide.createIcons();
            renderCategoryChips();
            applySettings();
            renderItems();
            updateStats();
        };

        // Tab Switching
        function switchTab(tab) {
            ['inventory', 'reports', 'settings'].forEach(t => {
                const view = document.getElementById(`view-${t}`);
                const nav = document.getElementById(`nav-${t}`);
                if (t === tab) {
                    view.classList.remove('hidden');
                    nav.className = 'flex-1 flex flex-col items-center py-1 text-blue-500 transition-colors';
                } else {
                    view.classList.add('hidden');
                    nav.className = 'flex-1 flex flex-col items-center py-1 opacity-60 hover:opacity-100 transition-colors';
                }
            });

            if (tab === 'reports') renderReports();
        }

        // Render Category Filter Chips
        function renderCategoryChips() {
            const container = document.getElementById('category-chips');
            container.innerHTML = categories.map(cat => `
                <button onclick="setCategory('${cat}')" 
                        class="px-3 py-1.5 rounded-xl font-medium shrink-0 transition-all ${selectedCategory === cat ? 'bg-blue-600 text-white shadow-md shadow-blue-600/20' : 'app-input-bg opacity-70 hover:opacity-100'}">
                    ${cat}
                </button>
            `).join('');
        }

        function setCategory(cat) {
            selectedCategory = cat;
            renderCategoryChips();
            renderItems();
        }

        // Render Main Inventory List
        function renderItems() {
            const searchInput = document.getElementById('search-input').value.toLowerCase().trim();
            const searchTerms = searchInput ? searchInput.split(/\s+/).filter(term => term.length > 0) : [];
            const listContainer = document.getElementById('inventory-list');
            const emptyState = document.getElementById('empty-state');

            let filtered = inventory.filter(item => {
                const matchesCategory = selectedCategory === 'All' || item.category === selectedCategory;
                const itemFullText = `${item.name} ${item.code} ${item.category} ${item.car} ${item.vbeltSize || ''} ${item.location || ''}`.toLowerCase();
                const matchesSearch = searchTerms.length === 0 || searchTerms.every(term => itemFullText.includes(term));
                return matchesCategory && matchesSearch;
            });

            // Sorting
            filtered.sort((a, b) => {
                if (settings.sortBy === 'stock-asc') return a.stock - b.stock;
                if (settings.sortBy === 'stock-desc') return b.stock - a.stock;
                if (settings.sortBy === 'name-desc') return b.name.localeCompare(a.name);
                return a.name.localeCompare(b.name);
            });

            listContainer.innerHTML = '';

            if (filtered.length === 0) {
                emptyState.classList.remove('hidden');
            } else {
                emptyState.classList.add('hidden');
                
                filtered.forEach(item => {
                    const isLow = parseInt(item.stock) <= 5;
                    const card = document.createElement('div');

                    if (settings.viewMode === 'compact') {
                        card.className = `app-bg-card border rounded-xl p-2.5 flex items-center justify-between transition-all ${isLow ? 'border-amber-500/40 bg-amber-500/5' : ''}`;
                        card.innerHTML = `
                            <div class="flex items-center gap-2 overflow-hidden mr-2">
                                <span class="text-[10px] font-semibold text-blue-500 bg-blue-500/10 px-1.5 py-0.5 rounded shrink-0">${item.category}</span>
                                <h3 class="font-semibold text-xs truncate">${item.name}</h3>
                                <span class="text-[10px] font-mono text-emerald-500 bg-emerald-500/10 px-1.5 py-0.5 rounded shrink-0">${item.code}</span>
                            </div>
                            <div class="flex items-center gap-2 shrink-0">
                                <span class="text-xs font-semibold">Stok: ${item.stock}</span>
                                <button onclick="openEditModal('${item.id}')" class="p-1 opacity-60 hover:opacity-100 text-blue-500">
                                    <i data-lucide="edit-2" class="w-3.5 h-3.5"></i>
                                </button>
                                <button onclick="deletePartDirect('${item.id}')" class="p-1 opacity-60 hover:opacity-100 text-red-500">
                                    <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
                                </button>
                            </div>
                        `;
                    } else {
                        card.className = `app-bg-card border rounded-2xl p-3.5 flex justify-between items-center transition-all ${isLow ? 'border-amber-500/40 bg-amber-500/5' : ''}`;
                        card.innerHTML = `
                            <div class="space-y-1 pr-2 flex-1">
                                <div class="flex items-center gap-1.5 flex-wrap">
                                    <span class="text-[10px] font-semibold text-blue-500 bg-blue-500/10 px-1.5 py-0.5 rounded-md">${item.category}</span>
                                    <span class="text-[10px] font-semibold font-mono tracking-tight text-emerald-500 bg-emerald-500/10 border border-emerald-500/20 px-1.5 py-0.5 rounded-md flex items-center gap-1 shrink-0">
                                        <i data-lucide="tag" class="w-2.5 h-2.5 text-emerald-500"></i>
                                        ${item.code}
                                    </span>
                                    ${item.vbeltSize ? `<span class="text-[10px] font-semibold font-mono text-purple-400 bg-purple-500/10 border border-purple-500/20 px-1.5 py-0.5 rounded-md">Ukuran: ${item.vbeltSize}</span>` : ''}
                                </div>
                                <h3 class="font-semibold text-sm leading-snug pt-0.5">${item.name}</h3>
                                <p class="text-xs opacity-70 flex items-center gap-1">
                                    <i data-lucide="car" class="w-3 h-3 opacity-60"></i> ${item.car}
                                </p>
                                <div class="flex items-center gap-3 pt-1 text-xs">
                                    <span class="font-semibold text-emerald-500">Rp ${Number(item.price).toLocaleString('id-ID')}</span>
                                    <span class="opacity-40">•</span>
                                    <span class="opacity-70 font-medium">${item.location || 'Tanpa Merek'}</span>
                                </div>
                            </div>

                            <div class="flex flex-col items-end justify-between gap-3 shrink-0">
                                <span class="text-xs font-semibold px-2 py-0.5 rounded-full ${isLow ? 'bg-amber-500/20 text-amber-500 border border-amber-500/30' : 'app-input-bg opacity-80'}">
                                    Stok: ${item.stock}
                                </span>
                                
                                <div class="flex items-center gap-1">
                                    <button onclick="quickAdjustStock('${item.id}', -1)" class="w-7 h-7 rounded-lg app-input-bg hover:opacity-80 active:scale-95 flex items-center justify-center text-sm font-bold border border-neutral-700/40 transition-transform">-</button>
                                    <button onclick="quickAdjustStock('${item.id}', 1)" class="w-7 h-7 rounded-lg app-input-bg hover:opacity-80 active:scale-95 flex items-center justify-center text-sm font-bold border border-neutral-700/40 transition-transform">+</button>
                                    <button onclick="openEditModal('${item.id}')" class="w-7 h-7 rounded-lg bg-blue-500/10 text-blue-500 hover:bg-blue-500/20 active:scale-95 flex items-center justify-center border border-blue-500/20 transition-transform">
                                        <i data-lucide="edit-2" class="w-3.5 h-3.5"></i>
                                    </button>
                                </div>
                            </div>
                        `;
                    }
                    listContainer.appendChild(card);
                });
            }

            lucide.createIcons();
            updateStats();
        }

        // Quick Stock Adjustments (+ / -)
        function quickAdjustStock(id, delta) {
            const item = inventory.find(i => i.id === id);
            if (item) {
                item.stock = Math.max(0, parseInt(item.stock) + delta);
                renderItems();
            }
        }

        // Update Statistics Banner
        function updateStats() {
            const totalItems = inventory.reduce((acc, curr) => acc + parseInt(curr.stock), 0);
            const lowStockCount = inventory.filter(item => parseInt(item.stock) <= 5).length;

            document.getElementById('stat-total-items').innerText = totalItems;
            document.getElementById('stat-low-stock').innerText = lowStockCount;
        }

        // Render Reports Page Data
        function renderReports() {
            const totalTypes = inventory.length;
            const totalQty = inventory.reduce((acc, curr) => acc + parseInt(curr.stock), 0);
            const totalCost = inventory.reduce((acc, curr) => acc + (parseInt(curr.costPrice || 0) * parseInt(curr.stock)), 0);
            const totalAsset = inventory.reduce((acc, curr) => acc + (parseInt(curr.price || 0) * parseInt(curr.stock)), 0);

            document.getElementById('rep-total-types').innerText = totalTypes;
            document.getElementById('rep-total-qty').innerText = totalQty;
            document.getElementById('rep-total-cost').innerText = `Rp ${totalCost.toLocaleString('id-ID')}`;
            document.getElementById('rep-total-asset').innerText = `Rp ${totalAsset.toLocaleString('id-ID')}`;
        }

        // Modal Controls
        function toggleVbeltSizeField() {
            const categorySelect = document.getElementById('part-category');
            const vbeltContainer = document.getElementById('vbelt-field-container');
            if (categorySelect.value === 'V-Belt') {
                vbeltContainer.classList.remove('hidden');
            } else {
                vbeltContainer.classList.add('hidden');
            }
        }

        function openModal() {
            document.getElementById('part-form').reset();
            document.getElementById('part-id').value = '';
            document.getElementById('modal-title').innerText = 'Tambah Sparepart Baru';
            document.getElementById('btn-delete-part').classList.add('hidden');
            toggleVbeltSizeField();
            document.getElementById('modal-backdrop').classList.remove('hidden');
        }

        function openEditModal(id) {
            const item = inventory.find(i => i.id === id);
            if (!item) return;

            document.getElementById('part-id').value = item.id;
            document.getElementById('part-name').value = item.name;
            document.getElementById('part-code').value = item.code;
            document.getElementById('part-category').value = item.category;
            document.getElementById('part-vbelt').value = item.vbeltSize || '';
            document.getElementById('part-car').value = item.car;
            document.getElementById('part-stock').value = item.stock;
            document.getElementById('part-location').value = item.location || '';
            document.getElementById('part-cost').value = item.costPrice || '';
            document.getElementById('part-price').value = item.price;

            document.getElementById('modal-title').innerText = 'Edit Sparepart';
            document.getElementById('btn-delete-part').classList.remove('hidden');
            toggleVbeltSizeField();
            document.getElementById('modal-backdrop').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('modal-backdrop').classList.add('hidden');
        }

        function savePart(e) {
            e.preventDefault();
            const id = document.getElementById('part-id').value;
            const name = document.getElementById('part-name').value;
            const code = document.getElementById('part-code').value;
            const category = document.getElementById('part-category').value;
            const vbeltSize = category === 'V-Belt' ? document.getElementById('part-vbelt').value : '';
            const car = document.getElementById('part-car').value;
            const stock = parseInt(document.getElementById('part-stock').value) || 0;
            const location = document.getElementById('part-location').value;
            const costPrice = parseInt(document.getElementById('part-cost').value) || 0;
            const price = parseInt(document.getElementById('part-price').value) || 0;

            if (id) {
                // Edit existing
                const index = inventory.findIndex(i => i.id === id);
                if (index !== -1) {
                    inventory[index] = { id, name, code, category, car, stock, costPrice, price, location, vbeltSize };
                }
            } else {
                // Add new
                const newPart = {
                    id: Date.now().toString(),
                    name, code, category, car, stock, costPrice, price, location, vbeltSize
                };
                inventory.unshift(newPart);
            }

            closeModal();
            renderItems();
        }

        // HAPUS SPAREPART
        function deleteCurrentPart() {
            const id = document.getElementById('part-id').value;
            if (!id) return;
            deletePartDirect(id);
            closeModal();
        }

        function deletePartDirect(id) {
            const item = inventory.find(i => i.id === id);
            if (!item) return;
            inventory = inventory.filter(i => i.id !== id);
            renderItems();
        }

        // Settings Page Handling
        function applySettings() {
            document.getElementById('header-title-text').innerText = settings.title;
            document.getElementById('header-subtitle-text').innerText = settings.subtitle;
            document.getElementById('set-header-title').value = settings.title;
            document.getElementById('set-header-subtitle').value = settings.subtitle;
            document.getElementById('set-view-mode').value = settings.viewMode;
            document.getElementById('set-sort-by').value = settings.sortBy;
            document.getElementById('set-show-stats').checked = settings.showStats;
            document.getElementById('set-theme').value = settings.theme;
            document.getElementById('set-text-size').value = settings.textSize;
            document.getElementById('set-padding').value = settings.padding;

            // Apply Theme
            document.body.className = `${settings.theme} ${settings.textSize} min-h-screen pb-24`;

            // Apply Container Padding
            const container = document.getElementById('app-container');
            container.className = `max-w-md mx-auto transition-all duration-300 ${settings.padding}`;

            const banner = document.getElementById('stats-banner');
            if (settings.showStats) {
                banner.classList.remove('hidden');
            } else {
                banner.classList.add('hidden');
            }

            const logoImg = document.getElementById('header-logo');
            if (settings.logo) {
                logoImg.src = settings.logo;
                logoImg.classList.remove('hidden');
            } else {
                logoImg.classList.add('hidden');
            }
        }

        function updateSettingsFromForm() {
            settings.title = document.getElementById('set-header-title').value || 'Inventori Sparepart';
            settings.subtitle = document.getElementById('set-header-subtitle').value || 'AutoParts HQ';
            settings.viewMode = document.getElementById('set-view-mode').value;
            settings.sortBy = document.getElementById('set-sort-by').value;
            settings.showStats = document.getElementById('set-show-stats').checked;
            settings.theme = document.getElementById('set-theme').value;
            settings.textSize = document.getElementById('set-text-size').value;
            settings.padding = document.getElementById('set-padding').value;

            applySettings();
            renderItems();
        }

        function handleLogoUpload(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    settings.logo = e.target.result;
                    applySettings();
                };
                reader.readAsDataURL(file);
            }
        }

        function removeLogo() {
            settings.logo = '';
            document.getElementById('logo-input').value = '';
            applySettings();
        }

        // Share Inventory Data
        function shareInventoryData(method) {
            const storeName = document.getElementById('header-title-text').innerText || 'Inventori Sparepart';
            let message = `*RINGKASAN INVENTORI - ${storeName.toUpperCase()}*\n`;
            message += `Total Item: ${inventory.reduce((acc, curr) => acc + parseInt(curr.stock), 0)} unit\n\n`;
            message += `*DAFTAR STOK SPAREPART:*\n`;

            inventory.forEach((item, index) => {
                message += `${index + 1}. ${item.name} (${item.code})\n`;
                if(item.location) message += `   • Merek: ${item.location}\n`;
                message += `   • Mobil: ${item.car}\n`;
                if(item.vbeltSize) message += `   • Ukuran: ${item.vbeltSize}\n`;
                message += `   • Stok: ${item.stock} unit | Harga: Rp ${Number(item.price).toLocaleString('id-ID')}\n\n`;
            });

            if (method === 'wa') {
                const encodedMsg = encodeURIComponent(message);
                window.open(`https://wa.me/?text=${encodedMsg}`, '_blank');
            } else {
                navigator.clipboard.writeText(message).then(() => {
                    alert('Data inventori berhasil disalin ke clipboard!');
                }).catch(() => {
                    alert('Gagal menyalin data.');
                });
            }
        }
    </script>
</body>
</html>
