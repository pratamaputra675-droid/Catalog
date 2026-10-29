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
    </style>
</head>
<body class="bg-[#F2F2F7] text-slate-900 antialiased min-h-screen pb-28 selection:bg-blue-500 selection:text-white">

    <!-- iOS Header / Navigation Bar -->
    <header class="sticky top-0 z-40 bg-[#F2F2F7]/80 ios-blur border-b border-slate-200/80 pt-safe">
        <div id="headerContainer" class="max-w-md mx-auto px-4 py-3 flex items-center justify-between transition-all">
            <div class="flex items-center gap-2.5">
                <div id="headerLogoContainer" class="bg-blue-600 text-white w-9 h-9 rounded-full flex items-center justify-center font-bold text-base shadow-sm overflow-hidden shrink-0">
                    <img id="headerLogoImg" class="w-full h-full object-cover hidden" alt="Logo">
                    <i id="headerLogoIcon" class="fa-solid fa-car"></i>
                </div>
                <div>
                    <h1 id="headerTitleText" class="text-base font-bold tracking-tight text-slate-900 leading-none">Blantikautoparts</h1>
                    <p id="headerSubtitleText" class="text-[10px] text-blue-600 font-semibold tracking-wider uppercase mt-0.5">Japan Spec iOS</p>
                </div>
            </div>
            <button onclick="openModal('add')" class="bg-blue-600 hover:bg-blue-700 active:scale-95 text-white w-8 h-8 rounded-full flex items-center justify-center shadow-md transition-all shrink-0">
                <i class="fa-solid fa-plus text-sm"></i>
            </button>
        </div>
    </header>

    <main id="mainContainer" class="max-w-md mx-auto px-4 pt-4 space-y-4 transition-all">
        <!-- Total Card -->
        <div id="statCard" class="bg-white rounded-2xl p-4 shadow-sm border border-slate-100 flex items-center justify-between">
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

        <!-- iOS Search Bar -->
        <div class="relative">
            <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-slate-400 text-sm"></i>
            <input type="text" id="searchInput" oninput="renderTable()" placeholder="Cari nama, part number, atau mobil..."
                class="w-full bg-[#E3E3E8] text-slate-900 text-sm pl-9 pr-8 py-2 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 transition placeholder:text-slate-500">
        </div>

        <!-- Filter Chips -->
        <div class="flex items-center gap-2 overflow-x-auto no-scrollbar py-1">
            <select id="filterCategory" onchange="renderTable()" class="bg-white text-slate-700 text-xs font-semibold px-3 py-2 rounded-xl border border-slate-200 shadow-sm focus:outline-none">
                <option value="">Semua Kategori</option>
                <option value="VBelt">VBelt / Tali Kipas</option>
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
                <!-- Content dynamically inserted -->
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
        <div class="max-w-md mx-auto px-6 py-2.5 flex justify-around items-center text-center">
            <button onclick="window.scrollTo({top: 0, behavior: 'smooth'})" class="flex flex-col items-center gap-0.5 text-blue-600">
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

    <!-- Modal Pengaturan Layar Utama (iOS Style Sheet) -->
    <div id="settingsModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center hidden transition-opacity">
        <div class="bg-[#F2F2F7] w-full max-w-md rounded-t-3xl sm:rounded-2xl shadow-2xl overflow-hidden max-h-[90vh] flex flex-col">
            <div class="px-5 py-3.5 bg-white border-b border-slate-200/80 flex justify-between items-center sticky top-0 z-10">
                <span class="w-10"></span>
                <h3 class="font-semibold text-slate-900 text-sm">Pengaturan Layar Utama</h3>
                <button type="button" onclick="closeSettingsModal()" class="text-blue-600 font-bold text-sm">Selesai</button>
            </div>

            <div class="p-4 space-y-4 overflow-y-auto">
                <!-- Identitas Header & Logo -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider">Identitas Aplikasi (Header)</label>
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Nama Header</label>
                        <input type="text" id="settingHeaderTitle" oninput="updateSettings()" placeholder="Nama Toko / Header" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-slate-50 p-2 rounded-lg border border-slate-200">
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Subtitle Header</label>
                        <input type="text" id="settingHeaderSubtitle" oninput="updateSettings()" placeholder="Subtitle / Slogan" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-slate-50 p-2 rounded-lg border border-slate-200">
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Logo Aplikasi (Galeri / Gambar)</label>
                        <div class="flex items-center gap-3">
                            <input type="file" id="settingLogoFile" accept="image/*" onchange="handleLogoUpload(event)" class="hidden">
                            <button type="button" onclick="document.getElementById('settingLogoFile').click()" class="bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold px-3 py-2 rounded-lg transition border border-slate-200 flex items-center gap-2">
                                <i class="fa-solid fa-image text-blue-600"></i> Pilih dari Galeri
                            </button>
                            <button type="button" id="removeLogoBtn" onclick="removeCustomLogo()" class="text-red-500 hover:text-red-600 text-xs font-medium hidden">
                                Hapus Logo
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Tampilan Layout & Ukuran Teks -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider">Tata Letak & Ukuran Teks</label>
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Ukuran Teks Aplikasi</label>
                        <select id="settingTextSize" onchange="updateSettings()" class="w-full text-sm font-semibold text-slate-900 focus:outline-none bg-slate-50 p-2 rounded-lg border border-slate-200">
                            <option value="small">Kecil</option>
                            <option value="medium">Normal / Standar</option>
                            <option value="large">Besar</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-slate-600 mb-1">Jarak Sisi Kanan & Kiri (Padding Side)</label>
                        <select id="settingPaddingSide" onchange="updateSettings()" class="w-full text-sm font-semibold text-slate-900 focus:outline-none bg-slate-50 p-2 rounded-lg border border-slate-200">
                            <option value="px-2">Rapat (Jarak Kecil)</option>
                            <option value="px-4">Sedang (Standar iOS)</option>
                            <option value="px-6">Longgar (Jarak Lebar)</option>
                        </select>
                    </div>
                </div>

                <!-- Urutan Tampilan Item -->
                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider">Urutan Tampilan Item</label>
                    <select id="settingSortBy" onchange="updateSettings()" class="w-full text-sm font-semibold text-slate-900 focus:outline-none bg-transparent">
                        <option value="newest">Terbaru Ditambahkan</option>
                        <option value="stockAsc">Stok Terendah Lebih Dulu</option>
                        <option value="nameAsc">Nama Sparepart (A - Z)</option>
                        <option value="priceDesc">Harga Jual Tertinggi</option>
                    </select>
                </div>

                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100 divide-y divide-slate-100">
                    <div class="flex items-center justify-between pt-1">
                        <div>
                            <p class="text-sm font-semibold text-slate-900">Tampilkan Ringkasan Stok</p>
                            <p class="text-[11px] text-slate-400">Menampilkan kartu total item di bagian atas</p>
                        </div>
                        <input type="checkbox" id="settingShowStat" onchange="updateSettings()" class="w-5 h-5 accent-blue-600 rounded">
                    </div>
                    <div class="flex items-center justify-between pt-3">
                        <div>
                            <p class="text-sm font-semibold text-slate-900">Tampilan Ringkas / Kompak</p>
                            <p class="text-[11px] text-slate-400">Menyembunyikan detail merk & mobil di daftar</p>
                        </div>
                        <input type="checkbox" id="settingCompactView" onchange="updateSettings()" class="w-5 h-5 accent-blue-600 rounded">
                    </div>
                </div>

                <div class="bg-white rounded-xl p-3 space-y-2 shadow-sm border border-slate-100">
                    <div class="flex justify-between items-center">
                        <label class="text-sm font-semibold text-slate-900">Batas Stok Tipis</label>
                        <input type="number" id="settingLowStockThreshold" min="1" max="50" onchange="updateSettings()" 
                            class="w-16 text-right font-bold text-sm text-blue-600 focus:outline-none bg-slate-50 p-1 rounded-lg border border-slate-200">
                    </div>
                    <p class="text-[11px] text-slate-400">Tandai warna kuning jika stok kurang dari atau sama dengan jumlah ini.</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Form Sparepart (iOS Sheet) -->
    <div id="partModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center hidden transition-opacity">
        <div class="bg-[#F2F2F7] w-full max-w-md rounded-t-3xl sm:rounded-2xl shadow-2xl overflow-hidden max-h-[90vh] flex flex-col transform transition-transform">
            <div class="px-5 py-3.5 bg-white border-b border-slate-200/80 flex justify-between items-center sticky top-0 z-10">
                <button type="button" onclick="closeModal()" class="text-blue-600 text-sm font-normal">Batal</button>
                <h3 id="modalTitle" class="font-semibold text-slate-900 text-sm">Tambah Sparepart</h3>
                <button type="button" onclick="document.getElementById('submitBtn').click()" class="text-blue-600 font-bold text-sm">Simpan</button>
            </div>

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
                        <select id="partCategory" required onchange="toggleVbeltField()" class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                            <option value="Mesin">Mesin & Filter</option>
                            <option value="VBelt">VBelt / Tali Kipas</option>
                            <option value="Kaki-kaki">Kaki-kaki & Suspensi</option>
                            <option value="Rem">Pengereman</option>
                            <option value="Kelistrikan">Kelistrikan & Aki</option>
                            <option value="Bodi & Interior">Bodi & Interior</option>
                            <option value="Lainnya">Lainnya</option>
                        </select>
                    </div>
                    <hr class="border-slate-100">
                    <div id="vbeltSizeGroup" class="hidden">
                        <label class="block text-[11px] font-medium text-blue-600 uppercase tracking-wider mb-1">Ukuran VBelt (Nomor / Kode Ukuran)</label>
                        <input type="text" id="partVbeltSize" placeholder="Contoh: 4PK850 / 6PK1210 / B-35" 
                            class="w-full text-sm font-semibold text-blue-600 focus:outline-none bg-blue-50/50 p-1.5 rounded-lg border border-blue-200">
                        <hr class="border-slate-100 mt-3">
                    </div>
                    <div>
                        <label class="block text-[11px] font-medium text-slate-400 uppercase tracking-wider mb-1">Merk / Brand</label>
                        <input type="text" id="partBrand" required placeholder="Denso / Bando / Mitsuboshi" 
                            class="w-full text-sm text-slate-900 focus:outline-none bg-transparent">
                    </div>
                </div>

                <div class="bg-white rounded-xl p-3 space-y-3 shadow-sm border border-slate-100">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-600">Jumlah Stok</label>
                        <input type="number" id="partStock" min="0" required placeholder="0" 
                            class="w-20 text-right text-sm font-bold text-slate-900 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-600">Harga Beli (Modal)</label>
                        <input type="text" id="partBuyPrice" required placeholder="0" 
                            class="w-32 text-right text-sm font-semibold text-slate-700 focus:outline-none bg-transparent">
                    </div>
                    <hr class="border-slate-100">
                    <div class="flex justify-between items-center">
                        <label class="text-xs font-medium text-slate-600">Harga Jual</label>
                        <input type="text" id="partSellPrice" required placeholder="0" 
                            class="w-32 text-right text-sm font-bold text-blue-600 focus:outline-none bg-transparent">
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
        // Initial Default Spareparts Data
        const defaultParts = [
            { id: 1, name: "Kampas Rem Depan Set", code: "04465-YZZE1", year: "2012 - 2021", category: "Rem", vbeltSize: "", brand: "Akimbo / Genuine", car: "Toyota Avanza / Xenia", stock: 8, buyPrice: 175000, sellPrice: 240000 },
            { id: 2, name: "VBelt Alternator / Fan Belt", code: "4PK850", year: "2015 - 2022", category: "VBelt", vbeltSize: "4PK850", brand: "Bando", car: "Toyota Avanza Dual VVT-i", stock: 12, buyPrice: 65000, sellPrice: 95000 },
            { id: 3, name: "Filter Oli Mesin", code: "15400-RTA-003", year: "2014 - 2022", category: "Mesin", vbeltSize: "", brand: "Denso", car: "Honda Jazz / Brio", stock: 15, buyPrice: 35000, sellPrice: 55000 },
            { id: 4, name: "Busi Iridium Standard", code: "IZFR6K11", year: "2017 - 2023", category: "Kelistrikan", vbeltSize: "", brand: "NGK", car: "Mitsubishi Xpander", stock: 2, buyPrice: 95000, sellPrice: 135000 },
            { id: 5, name: "Link Stabilizer Depan", code: "SL-S080", year: "2012 - 2018", category: "Kaki-kaki", vbeltSize: "", brand: "555 / Japan", car: "Suzuki Ertiga", stock: 4, buyPrice: 110000, sellPrice: 165000 },
            { id: 6, name: "Shock Absorber Belakang", code: "48530-09L10", year: "2016 - 2023", category: "Kaki-kaki", vbeltSize: "", brand: "KYB (Kayaba)", car: "Toyota Innova Reborn", stock: 1, buyPrice: 450000, sellPrice: 620000 }
        ];

        let spareparts = JSON.parse(localStorage.getItem('spareparts_data')) || defaultParts;

        // App Settings Configuration
        let appSettings = JSON.parse(localStorage.getItem('blantik_settings')) || {
            headerTitle: 'Blantikautoparts',
            headerSubtitle: 'Japan Spec iOS',
            customLogo: null,
            textSize: 'medium',
            paddingSide: 'px-4',
            sortBy: 'newest',
            showStat: true,
            compactView: false,
            lowStockThreshold: 3
        };

        function formatRupiah(number) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits: 0 }).format(number);
        }

        function formatCurrencyInput(val) {
            let number = val.toString().replace(/\D/g, '');
            if (!number) return '';
            return new Intl.NumberFormat('id-ID').format(number);
        }

        function parseCurrencyInput(val) {
            if (!val) return 0;
            return parseInt(val.toString().replace(/\D/g, '')) || 0;
        }

        function setupCurrencyInputs() {
            const buyInput = document.getElementById('partBuyPrice');
            const sellInput = document.getElementById('partSellPrice');

            [buyInput, sellInput].forEach(input => {
                input.addEventListener('input', function(e) {
                    let formatted = formatCurrencyInput(e.target.value);
                    e.target.value = formatted;
                });
            });
        }

        function showToast(message, isError = false) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMessage');
            const toastIcon = document.getElementById('toastIcon');
            
            toastMsg.textContent = message;
            toastIcon.innerHTML = isError ? '<i class="fa-solid fa-triangle-exclamation text-amber-400"></i>' : '<i class="fa-solid fa-circle-check text-emerald-400"></i>';
            
            toast.classList.remove('-translate-y-20', 'opacity-0');
            setTimeout(() => {
                toast.classList.add('-translate-y-20', 'opacity-0');
            }, 3000);
        }

        function saveData() {
            localStorage.setItem('spareparts_data', JSON.stringify(spareparts));
        }

        function openSettingsModal() {
            document.getElementById('settingHeaderTitle').value = appSettings.headerTitle || 'Blantikautoparts';
            document.getElementById('settingHeaderSubtitle').value = appSettings.headerSubtitle || 'Japan Spec iOS';
            document.getElementById('settingTextSize').value = appSettings.textSize || 'medium';
            document.getElementById('settingPaddingSide').value = appSettings.paddingSide || 'px-4';
            document.getElementById('settingSortBy').value = appSettings.sortBy;
            document.getElementById('settingShowStat').checked = appSettings.showStat;
            document.getElementById('settingCompactView').checked = appSettings.compactView;
            document.getElementById('settingLowStockThreshold').value = appSettings.lowStockThreshold;

            const removeBtn = document.getElementById('removeLogoBtn');
            if (appSettings.customLogo) {
                removeBtn.classList.remove('hidden');
            } else {
                removeBtn.classList.add('hidden');
            }

            document.getElementById('settingsModal').classList.remove('hidden');
        }

        function closeSettingsModal() {
            document.getElementById('settingsModal').classList.add('hidden');
        }

        function handleLogoUpload(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    appSettings.customLogo = e.target.result;
                    updateSettings();
                    document.getElementById('removeLogoBtn').classList.remove('hidden');
                    showToast("Logo berhasil diperbarui!");
                };
                reader.readAsDataURL(file);
            }
        }

        function removeCustomLogo() {
            appSettings.customLogo = null;
            document.getElementById('removeLogoBtn').classList.add('hidden');
            document.getElementById('settingLogoFile').value = '';
            updateSettings();
            showToast("Logo dikembalikan ke bawaan.");
        }

        function applyAppearanceSettings() {
            // Apply Header & Subtitle
            document.getElementById('headerTitleText').textContent = appSettings.headerTitle || 'Blantikautoparts';
            document.getElementById('headerSubtitleText').textContent = appSettings.headerSubtitle || 'Japan Spec iOS';

            // Apply Custom Logo
            const logoImg = document.getElementById('headerLogoImg');
            const logoIcon = document.getElementById('headerLogoIcon');
            if (appSettings.customLogo) {
                logoImg.src = appSettings.customLogo;
                logoImg.classList.remove('hidden');
                logoIcon.classList.add('hidden');
            } else {
                logoImg.classList.add('hidden');
                logoIcon.classList.remove('hidden');
            }

            // Apply Text Size
            const body = document.body;
            body.classList.remove('text-xs', 'text-sm', 'text-base');
            if (appSettings.textSize === 'small') {
                body.classList.add('text-xs');
            } else if (appSettings.textSize === 'large') {
                body.classList.add('text-base');
            } else {
                body.classList.add('text-sm');
            }

            // Apply Side Padding Margins
            const headerContainer = document.getElementById('headerContainer');
            const mainContainer = document.getElementById('mainContainer');
            ['px-2', 'px-4', 'px-6'].forEach(p => {
                headerContainer.classList.remove(p);
                mainContainer.classList.remove(p);
            });
            const selectedPadding = appSettings.paddingSide || 'px-4';
            headerContainer.classList.add(selectedPadding);
            mainContainer.classList.add(selectedPadding);
        }

        function updateSettings() {
            appSettings.headerTitle = document.getElementById('settingHeaderTitle').value.trim() || 'Blantikautoparts';
            appSettings.headerSubtitle = document.getElementById('settingHeaderSubtitle').value.trim() || 'Japan Spec iOS';
            appSettings.textSize = document.getElementById('settingTextSize').value;
            appSettings.paddingSide = document.getElementById('settingPaddingSide').value;
            appSettings.sortBy = document.getElementById('settingSortBy').value;
            appSettings.showStat = document.getElementById('settingShowStat').checked;
            appSettings.compactView = document.getElementById('settingCompactView').checked;
            appSettings.lowStockThreshold = parseInt(document.getElementById('settingLowStockThreshold').value) || 3;

            localStorage.setItem('blantik_settings', JSON.stringify(appSettings));
            applyAppearanceSettings();
            renderTable();
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

        function toggleVbeltField() {
            const cat = document.getElementById('partCategory').value;
            const sizeGroup = document.getElementById('vbeltSizeGroup');
            if (cat === 'VBelt') {
                sizeGroup.classList.remove('hidden');
            } else {
                sizeGroup.classList.add('hidden');
                document.getElementById('partVbeltSize').value = '';
            }
        }

        function renderTable() {
            updateBrandFilterOptions();

            const container = document.getElementById('sparepartList');
            const emptyState = document.getElementById('emptyState');
            const searchVal = document.getElementById('searchInput').value.toLowerCase().trim();
            const categoryVal = document.getElementById('filterCategory').value;
            const brandVal = document.getElementById('filterBrand').value;

            // Apply Show/Hide Stat Setting
            const statCard = document.getElementById('statCard');
            if (appSettings.showStat) {
                statCard.classList.remove('hidden');
            } else {
                statCard.classList.add('hidden');
            }

            container.innerHTML = '';

            // Split search query into individual search terms for combined multi-keyword searching
            const searchTerms = searchVal.split(/\s+/).filter(term => term.length > 0);

            let filtered = spareparts.filter(item => {
                const itemBrand = (item.brand || '').toLowerCase();
                const itemCode = (item.code || '').toLowerCase();
                const itemYear = (item.year || '').toLowerCase();
                const itemName = (item.name || '').toLowerCase();
                const itemCar = (item.car || '').toLowerCase();
                const itemCategory = (item.category || '').toLowerCase();
                const itemVbeltSize = (item.vbeltSize || '').toLowerCase();

                // Combine all item text into one searchable string for term verification
                const fullItemText = `${itemName} ${itemCode} ${itemCar} ${itemYear} ${itemCategory} ${itemBrand} ${itemVbeltSize}`;

                // Check if ALL search terms match the item
                const matchesSearch = searchTerms.every(term => fullItemText.includes(term));

                const matchesCat = categoryVal === "" || item.category === categoryVal;
                const matchesBrand = brandVal === "" || item.brand === brandVal;
                return matchesSearch && matchesCat && matchesBrand;
            });

            // Apply Sorting
            filtered.sort((a, b) => {
                if (appSettings.sortBy === 'stockAsc') return a.stock - b.stock;
                if (appSettings.sortBy === 'nameAsc') return a.name.localeCompare(b.name);
                if (appSettings.sortBy === 'priceDesc') return b.sellPrice - a.sellPrice;
                return b.id - a.id;
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
                card.className = "p-3.5 hover:bg-slate-50 transition flex items-center justify-between gap-3";
                
                const isLowStock = item.stock <= appSettings.lowStockThreshold;

                if (appSettings.compactView) {
                    card.innerHTML = `
                        <div class="flex-1 min-w-0">
                            ${item.code ? `<span class="inline-block bg-indigo-50 text-indigo-700 font-bold text-[10px] font-mono px-1.5 py-0.2 rounded border border-indigo-200 mb-0.5">${item.code}</span>` : ''}
                            ${item.vbeltSize ? `<span class="inline-block bg-blue-50 text-blue-700 text-[10px] font-mono font-bold px-1.5 py-0.2 rounded border border-blue-200 mb-0.5 ml-1"><i class="fa-solid fa-ruler-horizontal text-[9px] mr-1"></i>${item.vbeltSize}</span>` : ''}
                            <h3 class="font-semibold text-slate-900 text-sm truncate">${item.name}</h3>
                            <span class="text-xs font-bold text-blue-600">${formatRupiah(item.sellPrice)}</span>
                        </div>
                        <div class="flex items-center gap-3">
                            <span class="text-xs font-bold px-2 py-0.5 rounded-full ${isLowStock ? 'bg-amber-100 text-amber-800' : 'bg-emerald-100 text-emerald-800'}">
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
                } else {
                    card.innerHTML = `
                        <div class="flex-1 min-w-0">
                            <div class="mb-0.5 flex flex-wrap gap-1 items-center">
                                ${item.code ? `<span class="bg-indigo-50 text-indigo-700 font-bold text-[10px] font-mono px-1.5 py-0.2 rounded border border-indigo-200">${item.code}</span>` : ''}
                                ${item.vbeltSize ? `<span class="bg-blue-50 text-blue-700 text-[10px] font-mono font-bold px-1.5 py-0.2 rounded border border-blue-200"><i class="fa-solid fa-ruler-horizontal text-[9px] mr-1"></i>${item.vbeltSize}</span>` : ''}
                            </div>
                            <h3 class="font-semibold text-slate-900 text-sm truncate mb-0.5">${item.name}</h3>
                            <p class="text-xs text-slate-500 truncate flex items-center gap-1">
                                <i class="fa-solid fa-car text-[10px] text-slate-400"></i> ${item.car} ${item.year ? `(${item.year})` : ''}
                            </p>
                            <div class="flex items-center gap-2 mt-1.5">
                                <span class="text-[10px] font-semibold text-slate-500 bg-slate-100 px-2 py-0.5 rounded-full">${item.brand || 'Lainnya'}</span>
                                <span class="text-xs font-bold text-blue-600">${formatRupiah(item.sellPrice)}</span>
                            </div>
                        </div>
                        <div class="flex flex-col items-end gap-2">
                            <span class="text-xs font-bold px-2 py-0.5 rounded-full ${isLowStock ? 'bg-amber-100 text-amber-800' : 'bg-emerald-100 text-emerald-800'}">
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
                }
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
                    document.getElementById('partCategory').value = part.category || 'Mesin';
                    document.getElementById('partVbeltSize').value = part.vbeltSize || '';
                    document.getElementById('partBrand').value = part.brand || '';
                    document.getElementById('partCar').value = part.car;
                    document.getElementById('partStock').value = part.stock;
                    document.getElementById('partBuyPrice').value = formatCurrencyInput(part.buyPrice.toString());
                    document.getElementById('partSellPrice').value = formatCurrencyInput(part.sellPrice.toString());
                }
            } else {
                title.textContent = 'Tambah Sparepart';
            }

            toggleVbeltField();
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
            const vbeltSize = document.getElementById('partVbeltSize').value.trim();
            const brand = document.getElementById('partBrand').value.trim();
            const car = document.getElementById('partCar').value.trim();
            const stock = parseInt(document.getElementById('partStock').value) || 0;
            const buyPrice = parseCurrencyInput(document.getElementById('partBuyPrice').value);
            const sellPrice = parseCurrencyInput(document.getElementById('partSellPrice').value);

            if (sellPrice < buyPrice) {
                showToast("Peringatan: Harga jual lebih rendah dari modal!", true);
            }

            if (id) {
                const index = spareparts.findIndex(i => i.id == id);
                if (index !== -1) {
                    spareparts[index] = { id: Number(id), name, code, year, category, vbeltSize, brand, car, stock, buyPrice, sellPrice };
                    showToast("Data berhasil diperbarui!");
                }
            } else {
                const newId = spareparts.length > 0 ? Math.max(...spareparts.map(i => i.id)) + 1 : 1;
                spareparts.push({ id: newId, name, code, year, category, vbeltSize, brand, car, stock, buyPrice, sellPrice });
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
            setupCurrencyInputs();
            applyAppearanceSettings();
            renderTable();
        }
    </script>
</body>
</html>
