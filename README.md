# dai15550.github.io
test web
<!DOCTYPE html>
<html lang="vi" class="scroll-smooth">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Trợ Lý Học Tập & Quản Lý Ghi Chú Cá Nhân — CNTT 4.0 | Trần Đãi Phong</title>
  <meta name="description" content="Ứng dụng Quản lý Ghi chú Cá nhân & Thư viện số 8 bài học CNTT 4.0 - Sinh viên Trần Đãi Phong (MSSV: P05007, Lớp: F203) - FPT Polytechnic Tây Nguyên." />
  
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  
  <!-- Google Fonts: Inter -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

  <!-- Tailwind Configuration for FPT Polytechnic Brand Colors & Dark Mode -->
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
          },
          colors: {
            fpt: {
              orange: '#F37021',
              orangeHover: '#D95D14',
              orangeLight: '#FFF3EC',
              navy: '#1E293B',
              navyDark: '#0F172A',
              green: '#00A651',
              blue: '#0066B3'
            }
          }
        }
      }
    }
  </script>

  <style>
    /* Custom scrollbar & refined micro-interactions */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: transparent;
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(148, 163, 184, 0.4);
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(243, 112, 33, 0.7);
    }
    .tab-active {
      border-bottom: 3px solid #F37021;
      color: #F37021;
      font-weight: 700;
    }
    .dark .tab-active {
      color: #FB923C;
      border-bottom-color: #FB923C;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(6px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .animate-fade-in {
      animation: fadeIn 0.25s ease-out forwards;
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 dark:bg-slate-950 dark:text-slate-100 font-sans antialiased transition-colors duration-200 min-h-screen flex flex-col">

  <!-- Toast Notification Container (Replaces alert/confirm) -->
  <div id="toastContainer" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 max-w-sm w-full pointer-events-none px-4 sm:px-0"></div>

  <!-- Top Utility Bar -->
  <div class="bg-fpt-navy dark:bg-slate-900 text-slate-200 text-xs border-b border-slate-700/60">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-2 flex flex-wrap items-center justify-between gap-2">
      <div class="flex items-center gap-2">
        <span class="inline-flex items-center gap-1.5 px-2 py-0.5 rounded-full bg-fpt-orange/20 text-orange-300 font-semibold border border-fpt-orange/30">
          <span class="w-1.5 h-1.5 rounded-full bg-fpt-orange animate-pulse"></span>
          FPT Polytechnic Tây Nguyên
        </span>
        <span class="hidden sm:inline text-slate-400">|</span>
        <span class="hidden sm:inline text-slate-300">Môn học: <strong>Công nghệ thông tin 4.0 (COM108 / CNTT 4.0)</strong></span>
      </div>
      <div class="flex items-center gap-2 ml-auto">
        <!-- Export JSON -->
        <button onclick="exportBackupJSON()" title="Xuất dữ liệu sao lưu JSON" class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 transition">
          <svg class="w-3.5 h-3.5 text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"/></svg>
          <span>Xuất JSON</span>
        </button>
        <!-- Import JSON -->
        <label title="Nhập dữ liệu từ file JSON" class="cursor-pointer inline-flex items-center gap-1.5 px-2.5 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 transition">
          <svg class="w-3.5 h-3.5 text-sky-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12"/></svg>
          <span>Nhập JSON</span>
          <input type="file" id="importJsonInput" accept=".json" class="hidden" onchange="importBackupJSON(event)" />
        </label>
        <!-- Dark Mode Toggle -->
        <button onclick="toggleDarkMode()" id="themeToggleBtn" title="Chuyển đổi giao diện Sáng / Tối" class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-amber-300 border border-slate-700 transition">
          <svg id="sunIcon" class="w-3.5 h-3.5 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"/></svg>
          <svg id="moonIcon" class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"/></svg>
          <span id="themeLabel" class="hidden md:inline">Tối</span>
        </button>
      </div>
    </div>
  </div>

  <!-- Header & Student Profile Banner -->
  <header class="bg-white dark:bg-slate-900 border-b border-slate-200 dark:border-slate-800 shadow-sm">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
      <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-6">
        
        <!-- Left: App Title & Student Profile Card -->
        <div class="flex items-start sm:items-center gap-4">
          <!-- Student Monogram Avatar -->
          <div class="w-14 h-14 sm:w-16 sm:h-16 rounded-2xl bg-gradient-to-br from-fpt-orange to-amber-500 text-white flex flex-col items-center justify-center font-extrabold text-xl shadow-md shadow-orange-500/20 shrink-0 select-none">
            <span>ĐP</span>
            <span class="text-[9px] font-semibold tracking-wider uppercase opacity-90">FPoly</span>
          </div>
          
          <div>
            <div class="flex flex-wrap items-center gap-2 mb-1">
              <span class="px-2.5 py-0.5 text-xs font-bold rounded-md bg-orange-100 text-fpt-orange dark:bg-orange-950/60 dark:text-orange-400">
                CNTT 4.0 — ASSIGNMENT
              </span>
              <span class="text-xs text-slate-500 dark:text-slate-400">GitHub Pages Personal Knowledge Hub</span>
            </div>
            <h1 class="text-xl sm:text-2xl lg:text-3xl font-extrabold tracking-tight text-slate-900 dark:text-white leading-tight">
              Trợ Lý Học Tập & Quản Lý Ghi Chú Cá Nhân — CNTT 4.0
            </h1>
            
            <!-- Student Info Badges -->
            <div class="mt-2.5 flex flex-wrap items-center gap-2 text-xs sm:text-sm">
              <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 font-medium border border-slate-200 dark:border-slate-700">
                <svg class="w-3.5 h-3.5 text-fpt-orange" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/></svg>
                SV: <strong class="text-slate-900 dark:text-white">Trần Đãi Phong</strong>
              </span>
              <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 font-medium border border-slate-200 dark:border-slate-700">
                <svg class="w-3.5 h-3.5 text-sky-600 dark:text-sky-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H5a2 2 0 00-2 2v9a2 2 0 002 2h14a2 2 0 002-2V8a2 2 0 00-2-2h-5m-4 0V5a2 2 0 114 0v1m-4 0a2 2 0 104 0m-5 8a2 2 0 100-4 2 2 0 000 4zm0 0c1.306 0 2.417.835 2.83 2M9 14a3.001 3.001 0 00-2.83 2M15 11h3m-3 4h2"/></svg>
                MSSV: <strong class="text-slate-900 dark:text-white">P05007</strong>
              </span>
              <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 font-medium border border-slate-200 dark:border-slate-700">
                <svg class="w-3.5 h-3.5 text-emerald-600 dark:text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4"/></svg>
                Lớp: <strong class="text-slate-900 dark:text-white">F203</strong>
              </span>
              <span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-orange-50 dark:bg-orange-950/40 text-fpt-orange dark:text-orange-300 font-medium border border-orange-200 dark:border-orange-800/60">
                Cao đẳng FPT Polytechnic Tây Nguyên
              </span>
            </div>
          </div>
        </div>

        <!-- Right: Quick Stats Dashboard -->
        <div class="grid grid-cols-3 gap-3 sm:gap-4 shrink-0">
          <div class="bg-slate-50 dark:bg-slate-800/80 border border-slate-200 dark:border-slate-700 rounded-xl p-3 text-center min-w-[95px]">
            <div class="text-xs font-medium text-slate-500 dark:text-slate-400">Ghi chú học tập</div>
            <div class="mt-1 flex items-baseline justify-center gap-1">
              <span id="statTotalNotes" class="text-2xl font-extrabold text-fpt-orange">12</span>
              <span class="text-xs text-slate-400">/10+</span>
            </div>
            <div id="statDaysSpan" class="text-[11px] font-medium text-emerald-600 dark:text-emerald-400 mt-0.5">6 ngày liên tục</div>
          </div>

          <div class="bg-slate-50 dark:bg-slate-800/80 border border-slate-200 dark:border-slate-700 rounded-xl p-3 text-center min-w-[95px]">
            <div class="text-xs font-medium text-slate-500 dark:text-slate-400">Thư viện số</div>
            <div class="mt-1 flex items-baseline justify-center gap-1">
              <span class="text-2xl font-extrabold text-slate-900 dark:text-white">8</span>
              <span class="text-xs text-slate-400">Slides</span>
            </div>
            <div class="text-[11px] font-medium text-sky-600 dark:text-sky-400 mt-0.5">Đủ 4 yếu tố Y2</div>
          </div>

          <div class="bg-slate-50 dark:bg-slate-800/80 border border-slate-200 dark:border-slate-700 rounded-xl p-3 text-center min-w-[95px]">
            <div class="text-xs font-medium text-slate-500 dark:text-slate-400">Tiến độ ASM</div>
            <div class="mt-1 flex items-baseline justify-center gap-1">
              <span id="statProgress" class="text-2xl font-extrabold text-emerald-600 dark:text-emerald-400">100%</span>
            </div>
            <div class="w-full bg-slate-200 dark:bg-slate-700 h-1.5 rounded-full mt-1.5 overflow-hidden">
              <div id="statProgressBar" class="bg-emerald-500 h-full rounded-full transition-all duration-500" style="width: 100%"></div>
            </div>
          </div>
        </div>

      </div>
    </div>

    <!-- Navigation Tabs Bar -->
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <nav class="flex space-x-2 sm:space-x-6 overflow-x-auto no-scrollbar border-t border-slate-100 dark:border-slate-800/80" aria-label="Tabs">
        <button id="tabBtn-notes" onclick="switchTab('notes')" class="tab-active whitespace-nowrap py-3.5 px-2 text-sm font-medium flex items-center gap-2 transition">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/></svg>
          <span>1. Quản lý Ghi chú & Trợ lý AI</span>
          <span id="tabBadgeNotes" class="ml-1 px-2 py-0.5 text-xs rounded-full bg-orange-100 text-fpt-orange dark:bg-orange-950 dark:text-orange-300 font-bold">12</span>
        </button>

        <button id="tabBtn-library" onclick="switchTab('library')" class="text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white whitespace-nowrap py-3.5 px-2 text-sm font-medium flex items-center gap-2 transition">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/></svg>
          <span>2. Thư viện Số 8 Bài Học</span>
          <span class="ml-1 px-2 py-0.5 text-xs rounded-full bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 font-semibold">8 Slides</span>
        </button>

        <button id="tabBtn-wireframe" onclick="switchTab('wireframe')" class="text-slate-600 dark:text-slate-400 hover:text-slate-900 dark:hover:text-white whitespace-nowrap py-3.5 px-2 text-sm font-medium flex items-center gap-2 transition">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 5a1 1 0 011-1h14a1 1 0 011 1v2a1 1 0 01-1 1H5a1 1 0 01-1-1V5zM4 13a1 1 0 011-1h6a1 1 0 011 1v6a1 1 0 01-1 1H5a1 1 0 01-1-1v-6zM16 13a1 1 0 011-1h2a1 1 0 011 1v6a1 1 0 01-1 1h-2a1 1 0 01-1-1v-6z"/></svg>
          <span>3. Sơ đồ Phác thảo & Quy trình (Y4)</span>
        </button>
      </nav>
    </div>
  </header>

  <!-- Main Content Container -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">

    <!-- ===================================================================== -->
    <!-- TAB 1: QUẢN LÝ GHI CHÚ CÁ NHÂN & TRỢ LÝ AI (PHẦN 2 - Y5 & Y6)         -->
    <!-- ===================================================================== -->
    <section id="tabContent-notes" class="animate-fade-in space-y-6">
      
      <!-- Action & Filter Toolbar -->
      <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 p-4 sm:p-5 shadow-sm">
        <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4 pb-4 border-b border-slate-100 dark:border-slate-800">
          <div>
            <h2 class="text-lg font-bold text-slate-900 dark:text-white flex items-center gap-2">
              <span>Nhật ký & Quản lý Ghi chú Học tập</span>
              <span class="text-xs font-normal px-2 py-0.5 rounded-full bg-emerald-50 text-emerald-700 dark:bg-emerald-950/60 dark:text-emerald-300 border border-emerald-200 dark:border-emerald-800">Lưu tự động LocalStorage</span>
            </h2>
            <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-0.5">
              Đáp ứng Phần 2 (Y5 & Y6): Tối thiểu 10 ghi chú trải dài qua 6 ngày học tập, tích hợp Trợ lý AI tóm tắt & sinh câu hỏi ôn tập.
            </p>
          </div>
          
          <div class="flex flex-wrap items-center gap-2.5">
            <button onclick="openNoteModal()" class="inline-flex items-center gap-2 px-4 py-2.5 rounded-xl bg-fpt-orange hover:bg-fpt-orangeHover text-white font-semibold text-sm shadow-sm shadow-orange-500/25 transition active:scale-[0.99]">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M12 4v16m8-8H4"/></svg>
              <span>Thêm ghi chú mới</span>
            </button>
            <button onclick="openResetConfirmModal()" class="inline-flex items-center gap-1.5 px-3.5 py-2.5 rounded-xl bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 font-medium text-xs sm:text-sm border border-slate-200 dark:border-slate-700 transition" title="Khôi phục 12 ghi chú chuẩn ban đầu">
              <svg class="w-4 h-4 text-slate-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"/></svg>
              <span>Reset dữ liệu mẫu (12 ghi chú)</span>
            </button>
          </div>
        </div>

        <!-- Search & Filters Grid -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-12 gap-3 pt-4">
          <!-- Real-time Search -->
          <div class="lg:col-span-5 relative">
            <svg class="w-4 h-4 text-slate-400 absolute left-3.5 top-1/2 -translate-y-1/2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
            <input type="text" id="searchInput" oninput="renderNotes()" placeholder="Tìm tiêu đề, nội dung, thẻ #tag..." class="w-full pl-10 pr-4 py-2 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange transition" />
          </div>

          <!-- Filter by Slide -->
          <div class="lg:col-span-4">
            <select id="slideFilter" onchange="renderNotes()" class="w-full px-3.5 py-2 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange transition">
              <option value="all">📚 Tất cả 8 Slide bài học</option>
              <option value="1">Slide 1: CMCN 4.0 & Chuyển đổi số</option>
              <option value="2">Slide 2: Máy tính & Phần cứng</option>
              <option value="3">Slide 3: Hệ điều hành & Đa nền tảng</option>
              <option value="4">Slide 4: Phần mềm, Cloud & Nền tảng</option>
              <option value="5">Slide 5: Mạng máy tính, Internet & IoT</option>
              <option value="6">Slide 6: Tư duy tính toán & Tự động hóa</option>
              <option value="7">Slide 7: Trí tuệ nhân tạo & Dữ liệu</option>
              <option value="8">Slide 8: An toàn thông tin & Đạo đức số</option>
            </select>
          </div>

          <!-- Filter by Study Day & Important -->
          <div class="lg:col-span-3 flex gap-2">
            <select id="dayFilter" onchange="renderNotes()" class="w-full px-3 py-2 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange transition">
              <option value="all">📅 Tất cả 6 ngày học</option>
              <option value="Ngày 1">Ngày 1 (14/11/2026)</option>
              <option value="Ngày 2">Ngày 2 (15/11/2026)</option>
              <option value="Ngày 3">Ngày 3 (16/11/2026)</option>
              <option value="Ngày 4">Ngày 4 (17/11/2026)</option>
              <option value="Ngày 5">Ngày 5 (18/11/2026)</option>
              <option value="Ngày 6">Ngày 6 (19/11/2026)</option>
            </select>
            <button id="starFilterBtn" onclick="toggleStarFilter()" title="Chỉ hiện ghi chú quan trọng" class="px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-slate-400 hover:text-amber-500 transition shrink-0">
              <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"/></svg>
            </button>
          </div>
        </div>
      </div>

      <!-- Main Split Layout: Notes Grid (Left 7 cols) + AI Study Assistant Panel (Right 5 cols) -->
      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">
        
        <!-- Left Column: Notes List -->
        <div class="lg:col-span-7 space-y-4">
          <div class="flex items-center justify-between text-xs text-slate-500 dark:text-slate-400 px-1">
            <span>Hiển thị <strong id="visibleNotesCount" class="text-slate-800 dark:text-slate-200">12</strong> ghi chú</span>
            <span>Nhấn vào ghi chú để kích hoạt <strong>Trợ lý AI</strong> bên cạnh 👉</span>
          </div>

          <div id="notesListContainer" class="space-y-3.5">
            <!-- Dynamic Note Cards Injected Here -->
          </div>
        </div>

        <!-- Right Column: AI Study Assistant Panel (Y6 Requirement) -->
        <div class="lg:col-span-5 lg:sticky lg:top-5">
          <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm overflow-hidden">
            <!-- AI Panel Header -->
            <div class="bg-gradient-to-r from-fpt-navy via-slate-800 to-slate-900 p-4 text-white">
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2.5">
                  <div class="w-9 h-9 rounded-xl bg-fpt-orange/20 border border-fpt-orange/40 flex items-center justify-center text-fpt-orange">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/></svg>
                  </div>
                  <div>
                    <h3 class="text-sm font-bold flex items-center gap-1.5">
                      <span>Trợ Lý Học Tập AI (Y6)</span>
                      <span class="px-1.5 py-0.5 text-[10px] rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30">Smart Study</span>
                    </h3>
                    <p class="text-[11px] text-slate-300">Phân tích ghi chú, tạo Quiz & Prompt chuẩn Slide 7</p>
                  </div>
                </div>
              </div>
            </div>

            <!-- Selected Note Context Box -->
            <div class="p-4 border-b border-slate-100 dark:border-slate-800 bg-slate-50/70 dark:bg-slate-800/40">
              <div class="text-[11px] font-semibold uppercase tracking-wider text-slate-400 mb-1">Ghi chú đang được chọn phân tích:</div>
              <div id="aiSelectedNoteTitle" class="text-sm font-bold text-slate-900 dark:text-white line-clamp-2">
                <!-- Populated dynamically -->
              </div>
              <div id="aiSelectedNoteMeta" class="text-xs text-fpt-orange font-medium mt-1">
                <!-- Populated dynamically -->
              </div>
            </div>

            <!-- AI Action Buttons -->
            <div class="p-4 space-y-2.5 border-b border-slate-100 dark:border-slate-800">
              <button onclick="runAIAction('summarize')" class="w-full flex items-center justify-between px-3.5 py-2.5 rounded-xl bg-orange-50 hover:bg-orange-100 dark:bg-orange-950/40 dark:hover:bg-orange-950/70 text-fpt-orange dark:text-orange-300 border border-orange-200 dark:border-orange-800/60 text-xs sm:text-sm font-semibold transition">
                <span class="flex items-center gap-2">
                  <span>✨</span>
                  <span>Tóm tắt ý chính ghi chú đang chọn</span>
                </span>
                <span class="text-xs opacity-75">Key Points →</span>
              </button>

              <button onclick="runAIAction('quiz')" class="w-full flex items-center justify-between px-3.5 py-2.5 rounded-xl bg-sky-50 hover:bg-sky-100 dark:bg-sky-950/40 dark:hover:bg-sky-950/70 text-sky-700 dark:text-sky-300 border border-sky-200 dark:border-sky-800/60 text-xs sm:text-sm font-semibold transition">
                <span class="flex items-center gap-2">
                  <span>🎯</span>
                  <span>Tạo 3 câu hỏi ôn tập trắc nghiệm</span>
                </span>
                <span class="text-xs opacity-75">Quiz 3 câu →</span>
              </button>

              <button onclick="runAIAction('prompt')" class="w-full flex items-center justify-between px-3.5 py-2.5 rounded-xl bg-emerald-50 hover:bg-emerald-100 dark:bg-emerald-950/40 dark:hover:bg-emerald-950/70 text-emerald-700 dark:text-emerald-300 border border-emerald-200 dark:border-emerald-800/60 text-xs sm:text-sm font-semibold transition">
                <span class="flex items-center gap-2">
                  <span>🧠</span>
                  <span>Gợi ý Prompt Engineering (Slide 7)</span>
                </span>
                <span class="text-xs opacity-75">Copy Prompt →</span>
              </button>
            </div>

            <!-- AI Output Display Area -->
            <div class="p-4">
              <div class="flex items-center justify-between mb-2">
                <span id="aiOutputHeading" class="text-xs font-bold uppercase tracking-wider text-slate-500 dark:text-slate-400">Kết quả phân tích AI</span>
                <button onclick="copyAIOutput()" class="text-xs text-fpt-orange hover:underline font-medium flex items-center gap-1">
                  <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"/></svg>
                  <span>Sao chép</span>
                </button>
              </div>
              <div id="aiOutputBox" class="p-3.5 rounded-xl bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800 text-xs sm:text-sm text-slate-700 dark:text-slate-300 leading-relaxed max-h-[420px] overflow-y-auto space-y-2.5">
                <!-- Populated dynamically -->
              </div>
            </div>
          </div>
        </div>

      </div>
    </section>

    <!-- ===================================================================== -->
    <!-- TAB 2: THƯ VIỆN SỐ 8 BÀI HỌC (PHẦN 1 - YÊU CẦU Y1 & Y2)               -->
    <!-- ===================================================================== -->
    <section id="tabContent-library" class="hidden animate-fade-in space-y-6">
      <!-- Library Banner -->
      <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 p-5 shadow-sm flex flex-col md:flex-row md:items-center justify-between gap-4">
        <div>
          <span class="text-xs font-bold uppercase tracking-wider text-fpt-orange">Phần 1 — Yêu cầu Y1 & Y2</span>
          <h2 class="text-xl font-extrabold text-slate-900 dark:text-white mt-0.5">Thư viện Số Cá nhân: Hệ thống 8 Bài học CNTT 4.0</h2>
          <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-1">
            Mỗi bài học tích hợp đầy đủ 4 thành phần bắt buộc: <strong>(a)</strong> Tóm tắt Slide/Tài liệu, <strong>(b)</strong> Sơ đồ/Infographic trực quan SVG, <strong>(c)</strong> Liên kết tham chiếu uy tín, và <strong>(d)</strong> Ghi chú cá nhân liên kết trực tiếp.
          </p>
        </div>
        <div class="flex items-center gap-2 shrink-0">
          <span class="px-3 py-1.5 rounded-xl bg-slate-100 dark:bg-slate-800 text-xs font-semibold text-slate-700 dark:text-slate-300 border border-slate-200 dark:border-slate-700">
            8/8 Chủ đề hoàn chỉnh
          </span>
        </div>
      </div>

      <!-- 8 Slides Grid -->
      <div id="slidesLibraryGrid" class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <!-- Populated dynamically via JS -->
      </div>
    </section>

    <!-- ===================================================================== -->
    <!-- TAB 3: SƠ ĐỒ PHÁC THẢO & QUY TRÌNH HỆ THỐNG (PHẦN 2 - YÊU CẦU Y4)     -->
    <!-- ===================================================================== -->
    <section id="tabContent-wireframe" class="hidden animate-fade-in space-y-6">
      <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 p-5 sm:p-6 shadow-sm">
        <div class="border-b border-slate-100 dark:border-slate-800 pb-4 mb-6">
          <span class="text-xs font-bold uppercase tracking-wider text-fpt-orange">Phần 2 — Yêu cầu Y4 (Thiết kế & Kiến trúc hệ thống)</span>
          <h2 class="text-xl font-extrabold text-slate-900 dark:text-white mt-0.5">Sơ đồ Phác thảo Giao diện (Wireframe) & Luồng Vận hành Ứng dụng</h2>
          <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-1">
            Tài liệu trực quan phục vụ báo cáo & bảo vệ Assignment môn CNTT 4.0 — Sinh viên Trần Đãi Phong (P05007).
          </p>
        </div>

        <!-- 3-Part Visual Diagram Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
          
          <!-- (a) Wireframe Giao diện chính -->
          <div class="rounded-2xl border border-slate-200 dark:border-slate-700 bg-slate-50/70 dark:bg-slate-800/40 p-4">
            <div class="flex items-center justify-between mb-3">
              <span class="text-sm font-bold text-slate-900 dark:text-white">(a) Phác thảo Giao diện Chính (Main Hub)</span>
              <span class="text-[11px] px-2 py-0.5 rounded bg-orange-100 text-fpt-orange dark:bg-orange-950 dark:text-orange-300 font-semibold">Wireframe UI-01</span>
            </div>
            <svg viewBox="0 0 460 280" class="w-full h-auto rounded-xl border border-slate-300 dark:border-slate-600 bg-white dark:bg-slate-900">
              <!-- Top Header Box -->
              <rect x="14" y="14" width="432" height="52" rx="8" fill="#1E293B"/>
              <circle cx="38" cy="40" r="14" fill="#F37021"/>
              <text x="38" y="44" text-anchor="middle" fill="#FFF" font-size="10" font-weight="bold">ĐP</text>
              <text x="62" y="36" fill="#FFF" font-size="11" font-weight="bold">HEADER: Trợ Lý Học Tập & Quản Lý Ghi Chú CNTT 4.0</text>
              <text x="62" y="51" fill="#CBD5E1" font-size="9">SV: Trần Đãi Phong | MSSV: P05007 | Lớp: F203 | Stats (12 Notes / 8 Slides)</text>
              
              <!-- Navigation Tabs Bar -->
              <rect x="14" y="74" width="432" height="28" rx="6" fill="#F1F5F9" stroke="#CBD5E1"/>
              <rect x="20" y="78" width="135" height="20" rx="4" fill="#F37021"/>
              <text x="87" y="91" text-anchor="middle" fill="#FFF" font-size="9" font-weight="bold">Tab 1: Quản lý Ghi chú</text>
              <text x="225" y="91" text-anchor="middle" fill="#475569" font-size="9" font-weight="bold">Tab 2: Thư viện 8 Slides</text>
              <text x="365" y="91" text-anchor="middle" fill="#475569" font-size="9" font-weight="bold">Tab 3: Wireframe & Flow</text>

              <!-- Filter Bar -->
              <rect x="14" y="110" width="432" height="32" rx="6" fill="#FFF7ED" stroke="#FDBA74"/>
              <text x="26" y="130" fill="#9A3412" font-size="9" font-weight="bold">[Tìm kiếm từ khóa]  +  [Lọc Slide 1-8]  +  [Lọc Ngày 1-6]  +  [+ Thêm mới]</text>

              <!-- Split Workspace: Left Notes + Right AI -->
              <rect x="14" y="150" width="250" height="116" rx="8" fill="#F8FAFC" stroke="#94A3B8" stroke-dasharray="4 2"/>
              <text x="24" y="167" fill="#0F172A" font-size="9.5" font-weight="bold">Danh sách Thẻ Ghi chú (CRUD Cards)</text>
              <rect x="24" y="175" width="230" height="38" rx="5" fill="#FFF" stroke="#CBD5E1"/>
              <text x="32" y="191" fill="#1E293B" font-size="8.5" font-weight="bold">📌 Ghi chú #1: Tổng quan CMCN 4.0 & CĐS</text>
              <text x="32" y="204" fill="#64748B" font-size="8">Slide 1 | Ngày 1 | [Sửa] [Xóa] [Chọn phân tích AI]</text>
              <rect x="24" y="219" width="230" height="38" rx="5" fill="#FFF" stroke="#CBD5E1"/>
              <text x="32" y="235" fill="#1E293B" font-size="8.5" font-weight="bold">📌 Ghi chú #2: Kiến trúc Von Neumann & CPU</text>
              <text x="32" y="248" fill="#64748B" font-size="8">Slide 2 | Ngày 2 | [Sửa] [Xóa] [Chọn phân tích AI]</text>

              <!-- Right AI Panel -->
              <rect x="274" y="150" width="172" height="116" rx="8" fill="#EFF6FF" stroke="#60A5FA"/>
              <text x="284" y="167" fill="#1E3A8A" font-size="9.5" font-weight="bold">Panel Trợ lý AI (Y6)</text>
              <rect x="284" y="176" width="152" height="20" rx="4" fill="#F37021"/>
              <text x="360" y="189" text-anchor="middle" fill="#FFF" font-size="8.5" font-weight="bold">✨ Tóm tắt ý chính</text>
              <rect x="284" y="201" width="152" height="20" rx="4" fill="#0284C7"/>
              <text x="360" y="214" text-anchor="middle" fill="#FFF" font-size="8.5" font-weight="bold">🎯 Sinh 3 câu hỏi Quiz</text>
              <rect x="284" y="226" width="152" height="32" rx="4" fill="#FFF" stroke="#93C5FD"/>
              <text x="292" y="240" fill="#1E40AF" font-size="8">Output: Bullet points / Quiz</text>
              <text x="292" y="251" fill="#1E40AF" font-size="8">& Prompt mẫu chuẩn Slide 7</text>
            </svg>
          </div>

          <!-- (b) Wireframe Modal Thêm/Sửa & Trợ lý AI -->
          <div class="rounded-2xl border border-slate-200 dark:border-slate-700 bg-slate-50/70 dark:bg-slate-800/40 p-4">
            <div class="flex items-center justify-between mb-3">
              <span class="text-sm font-bold text-slate-900 dark:text-white">(b) Phác thảo Chức năng Thêm/Sửa & Bộ lọc thông minh</span>
              <span class="text-[11px] px-2 py-0.5 rounded bg-sky-100 text-sky-700 dark:bg-sky-950 dark:text-sky-300 font-semibold">Wireframe UI-02</span>
            </div>
            <svg viewBox="0 0 460 280" class="w-full h-auto rounded-xl border border-slate-300 dark:border-slate-600 bg-white dark:bg-slate-900">
              <!-- Left: Modal Form -->
              <rect x="14" y="14" width="235" height="252" rx="8" fill="#F8FAFC" stroke="#475569" stroke-width="1.5"/>
              <rect x="14" y="14" width="235" height="30" rx="8" fill="#F37021"/>
              <text x="26" y="33" fill="#FFF" font-size="10" font-weight="bold">MODAL: Thêm / Cập nhật Ghi chú</text>
              
              <text x="26" y="60" fill="#334155" font-size="8.5" font-weight="bold">1. Tiêu đề ghi chú (*)</text>
              <rect x="26" y="65" width="211" height="20" rx="4" fill="#FFF" stroke="#CBD5E1"/>
              
              <text x="26" y="100" fill="#334155" font-size="8.5" font-weight="bold">2. Thuộc Slide (1–8) & Ngày học (1–6)</text>
              <rect x="26" y="105" width="102" height="20" rx="4" fill="#FFF" stroke="#CBD5E1"/>
              <rect x="135" y="105" width="102" height="20" rx="4" fill="#FFF" stroke="#CBD5E1"/>

              <text x="26" y="140" fill="#334155" font-size="8.5" font-weight="bold">3. Thẻ #Tags & Link tham chiếu</text>
              <rect x="26" y="145" width="211" height="20" rx="4" fill="#FFF" stroke="#CBD5E1"/>

              <text x="26" y="180" fill="#334155" font-size="8.5" font-weight="bold">4. Nội dung kiến thức chi tiết (*)</text>
              <rect x="26" y="185" width="211" height="44" rx="4" fill="#FFF" stroke="#CBD5E1"/>

              <rect x="135" y="237" width="102" height="22" rx="5" fill="#16A34A"/>
              <text x="186" y="251" text-anchor="middle" fill="#FFF" font-size="9" font-weight="bold">💾 Lưu LocalStorage</text>

              <!-- Right: Multi-criteria Filter & Backup -->
              <rect x="262" y="14" width="184" height="120" rx="8" fill="#F0FDF4" stroke="#22C55E"/>
              <text x="274" y="34" fill="#15803D" font-size="9.5" font-weight="bold">Cơ chế Lọc Đa chiều (Real-time)</text>
              <text x="274" y="54" fill="#166534" font-size="8.5">• Tìm kiếm chuỗi trong Tiêu đề/Tag</text>
              <text x="274" y="72" fill="#166534" font-size="8.5">• Lọc theo Chủ đề Slide 1 → Slide 8</text>
              <text x="274" y="90" fill="#166534" font-size="8.5">• Lọc theo Tiến độ Ngày 1 → Ngày 6</text>
              <text x="274" y="108" fill="#166534" font-size="8.5">• Lọc Ghi chú đánh dấu Sao (★)</text>

              <rect x="262" y="146" width="184" height="120" rx="8" fill="#FFFBEB" stroke="#F59E0B"/>
              <text x="274" y="166" fill="#B45309" font-size="9.5" font-weight="bold">An toàn & Sao lưu Dữ liệu</text>
              <text x="274" y="186" fill="#92400E" font-size="8.5">• Tự động đồng bộ vào trình duyệt</text>
              <text x="274" y="204" fill="#92400E" font-size="8.5">• Xuất file sao lưu .JSON cá nhân</text>
              <text x="274" y="222" fill="#92400E" font-size="8.5">• Khôi phục nhanh 12 ghi chú chuẩn</text>
              <text x="274" y="240" fill="#92400E" font-size="8.5">• Chạy tĩnh 100% trên GitHub Pages</text>
            </svg>
          </div>
        </div>

        <!-- (c) Sơ đồ luồng vận hành hệ thống (System Data Flow) -->
        <div class="rounded-2xl border border-slate-200 dark:border-slate-700 bg-slate-50/70 dark:bg-slate-800/40 p-5">
          <div class="flex flex-wrap items-center justify-between gap-2 mb-4">
            <span class="text-base font-bold text-slate-900 dark:text-white">(c) Sơ đồ Luồng Vận hành Hệ thống (Input → Storage & AI Engine → Output)</span>
            <span class="text-xs px-2.5 py-0.5 rounded-full bg-emerald-100 text-emerald-700 dark:bg-emerald-950 dark:text-emerald-300 font-semibold">Architecture Flow</span>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-3 gap-4 items-stretch">
            <!-- Stage 1: INPUT -->
            <div class="rounded-xl bg-white dark:bg-slate-900 border-2 border-orange-400 p-4 flex flex-col justify-between shadow-sm">
              <div>
                <div class="inline-block px-2.5 py-0.5 rounded-md bg-orange-100 text-fpt-orange text-xs font-extrabold mb-2">1. INPUT (Đầu vào)</div>
                <h4 class="font-bold text-sm text-slate-900 dark:text-white mb-2">Tương tác & Tri thức Sinh viên</h4>
                <ul class="text-xs text-slate-600 dark:text-slate-300 space-y-1.5 list-disc pl-4">
                  <li>Nhập ghi chú bài giảng mới (Tiêu đề, Slide 1–8, Ngày học, Nội dung, Tags, Link).</li>
                  <li>Truy cập kho 8 Slides chuẩn chương trình CNTT 4.0.</li>
                  <li>Nhập từ khóa tìm kiếm, chọn bộ lọc hoặc nhập file backup <code>.json</code>.</li>
                </ul>
              </div>
              <div class="mt-3 pt-2 border-t border-slate-100 dark:border-slate-800 text-[11px] font-semibold text-fpt-orange">
                Người dùng: Trần Đãi Phong (P05007)
              </div>
            </div>

            <!-- Stage 2: PROCESSING & STORAGE -->
            <div class="rounded-xl bg-white dark:bg-slate-900 border-2 border-sky-500 p-4 flex flex-col justify-between shadow-sm">
              <div>
                <div class="inline-block px-2.5 py-0.5 rounded-md bg-sky-100 text-sky-700 text-xs font-extrabold mb-2">2. XỬ LÝ & LƯU TRỮ</div>
                <h4 class="font-bold text-sm text-slate-900 dark:text-white mb-2">LocalStorage & Bộ xử lý AI Client-side</h4>
                <ul class="text-xs text-slate-600 dark:text-slate-300 space-y-1.5 list-disc pl-4">
                  <li><strong>CRUD Engine:</strong> Xác thực dữ liệu, cập nhật mảng đối tượng JSON vào <code>localStorage</code> tức thì.</li>
                  <li><strong>AI Analyzer (Y6):</strong> Trích xuất câu trọng tâm, sinh bộ câu hỏi trắc nghiệm theo chủ đề Slide và tạo mẫu Prompt Engineering (Zero-shot / CoT).</li>
                </ul>
              </div>
              <div class="mt-3 pt-2 border-t border-slate-100 dark:border-slate-800 text-[11px] font-semibold text-sky-600 dark:text-sky-400">
                Công nghệ: HTML5 + Tailwind CSS + Vanilla JS
              </div>
            </div>

            <!-- Stage 3: OUTPUT -->
            <div class="rounded-xl bg-white dark:bg-slate-900 border-2 border-emerald-500 p-4 flex flex-col justify-between shadow-sm">
              <div>
                <div class="inline-block px-2.5 py-0.5 rounded-md bg-emerald-100 text-emerald-700 text-xs font-extrabold mb-2">3. OUTPUT (Đầu ra)</div>
                <h4 class="font-bold text-sm text-slate-900 dark:text-white mb-2">Kết quả Học tập & Báo cáo</h4>
                <ul class="text-xs text-slate-600 dark:text-slate-300 space-y-1.5 list-disc pl-4">
                  <li>Giao diện Dashboard cập nhật số lượng ghi chú, tiến độ 6 ngày & thống kê real-time.</li>
                  <li>Bản tóm tắt ý chính, bộ Quiz tự kiểm tra & Prompt sẵn sàng dùng trên ChatGPT/Gemini.</li>
                  <li>Tệp <code>.json</code> sao lưu & Trang web tĩnh triển khai trực tiếp trên GitHub Pages.</li>
                </ul>
              </div>
              <div class="mt-3 pt-2 border-t border-slate-100 dark:border-slate-800 text-[11px] font-semibold text-emerald-600 dark:text-emerald-400">
                Trạng thái: Sẵn sàng triển khai GitHub Pages
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

  </main>

  <!-- Footer -->
  <footer class="bg-white dark:bg-slate-900 border-t border-slate-200 dark:border-slate-800 mt-10">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-5 flex flex-col sm:flex-row items-center justify-between gap-3 text-xs text-slate-500 dark:text-slate-400">
      <div>
        © 2026 <strong>Trần Đãi Phong</strong> (MSSV: <strong>P05007</strong> — Lớp: <strong>F203</strong>) | Cao đẳng FPT Polytechnic Tây Nguyên.
      </div>
      <div class="flex items-center gap-3">
        <span>Assignment Môn: <strong>CNTT 4.0</strong></span>
        <span>•</span>
        <span class="text-fpt-orange font-semibold">Single-Page GitHub Pages Edition</span>
      </div>
    </div>
  </footer>

  <!-- ===================================================================== -->
  <!-- MODAL 1: CREATE / EDIT NOTE MODAL                                     -->
  <!-- ===================================================================== -->
  <div id="noteModal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-950/60 backdrop-blur-xs p-4 overflow-y-auto">
    <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 max-w-xl w-full shadow-2xl overflow-hidden animate-fade-in my-auto">
      <div class="bg-fpt-navy px-5 py-4 flex items-center justify-between text-white">
        <h3 id="modalTitle" class="font-bold text-base flex items-center gap-2">
          <span class="w-2.5 h-2.5 rounded-full bg-fpt-orange"></span>
          <span>Thêm Ghi Chú Học Tập Mới</span>
        </h3>
        <button onclick="closeNoteModal()" class="text-slate-400 hover:text-white transition p-1">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
        </button>
      </div>

      <form id="noteForm" onsubmit="saveNoteForm(event)" class="p-5 space-y-4">
        <input type="hidden" id="noteIdInput" />

        <div>
          <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 dark:text-slate-300 mb-1.5">
            Tiêu đề ghi chú <span class="text-red-500">*</span>
          </label>
          <input type="text" id="noteTitleInput" required placeholder="VD: Phân biệt Tin học hóa và Chuyển đổi số (CĐS)..." class="w-full px-3.5 py-2.5 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange" />
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3.5">
          <div>
            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 dark:text-slate-300 mb-1.5">
              Thuộc bài học (Slide 1–8) <span class="text-red-500">*</span>
            </label>
            <select id="noteSlideInput" required class="w-full px-3.5 py-2.5 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange">
              <option value="1">Slide 1: CMCN 4.0 & Chuyển đổi số</option>
              <option value="2">Slide 2: Máy tính & Phần cứng</option>
              <option value="3">Slide 3: Hệ điều hành & Đa nền tảng</option>
              <option value="4">Slide 4: Phần mềm, Cloud & Nền tảng</option>
              <option value="5">Slide 5: Mạng máy tính, Internet & IoT</option>
              <option value="6">Slide 6: Tư duy tính toán & Tự động hóa</option>
              <option value="7">Slide 7: Trí tuệ nhân tạo & Dữ liệu</option>
              <option value="8">Slide 8: An toàn thông tin & Đạo đức số</option>
            </select>
          </div>

          <div>
            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 dark:text-slate-300 mb-1.5">
              Ngày ghi chú (Tiến độ học tập) <span class="text-red-500">*</span>
            </label>
            <select id="noteDayInput" required class="w-full px-3.5 py-2.5 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange">
              <option value="Ngày 1|14/11/2026">Ngày 1 — 14/11/2026</option>
              <option value="Ngày 2|15/11/2026">Ngày 2 — 15/11/2026</option>
              <option value="Ngày 3|16/11/2026">Ngày 3 — 16/11/2026</option>
              <option value="Ngày 4|17/11/2026">Ngày 4 — 17/11/2026</option>
              <option value="Ngày 5|18/11/2026">Ngày 5 — 18/11/2026</option>
              <option value="Ngày 6|19/11/2026">Ngày 6 — 19/11/2026</option>
            </select>
          </div>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3.5">
          <div>
            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 dark:text-slate-300 mb-1.5">
              Thẻ từ khóa (cách nhau dấu phẩy)
            </label>
            <input type="text" id="noteTagsInput" placeholder="VD: CMCN4.0, AI, BigData" class="w-full px-3.5 py-2 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange" />
          </div>

          <div>
            <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 dark:text-slate-300 mb-1.5">
              Link tham chiếu ngoài (Tùy chọn)
            </label>
            <input type="url" id="noteLinkInput" placeholder="https://..." class="w-full px-3.5 py-2 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange" />
          </div>
        </div>

        <div>
          <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 dark:text-slate-300 mb-1.5">
            Nội dung chi tiết bài học <span class="text-red-500">*</span>
          </label>
          <textarea id="noteContentInput" rows="4" required placeholder="Ghi chép lại các khái niệm cốt lõi, ví dụ thực tiễn, công thức hoặc nhận xét cá nhân..." class="w-full px-3.5 py-2.5 text-sm rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 focus:outline-none focus:ring-2 focus:ring-fpt-orange/50 focus:border-fpt-orange"></textarea>
        </div>

        <div class="flex items-center justify-between pt-1">
          <label class="inline-flex items-center gap-2 cursor-pointer select-none">
            <input type="checkbox" id="noteImportantInput" class="w-4 h-4 rounded text-fpt-orange focus:ring-fpt-orange border-slate-300" />
            <span class="text-sm font-medium text-slate-700 dark:text-slate-300">Đánh dấu ghi chú quan trọng (★)</span>
          </label>
        </div>

        <div class="flex items-center justify-end gap-2.5 pt-3 border-t border-slate-100 dark:border-slate-800">
          <button type="button" onclick="closeNoteModal()" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 dark:text-slate-300 hover:bg-slate-100 dark:hover:bg-slate-800 transition">
            Hủy bỏ
          </button>
          <button type="submit" class="px-5 py-2 rounded-xl text-sm font-semibold bg-fpt-orange hover:bg-fpt-orangeHover text-white shadow-sm transition">
            Lưu ghi chú
          </button>
        </div>
      </form>
    </div>
  </div>

  <!-- ===================================================================== -->
  <!-- MODAL 2: CUSTOM CONFIRMATION MODAL (REPLACES window.confirm)          -->
  <!-- ===================================================================== -->
  <div id="confirmModal" class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-950/60 backdrop-blur-xs p-4">
    <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 max-w-md w-full p-5 shadow-2xl animate-fade-in">
      <div class="flex items-start gap-3">
        <div class="w-10 h-10 rounded-xl bg-amber-100 dark:bg-amber-950/70 text-amber-600 dark:text-amber-400 flex items-center justify-center shrink-0">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"/></svg>
        </div>
        <div>
          <h4 id="confirmModalTitle" class="text-base font-bold text-slate-900 dark:text-white">Xác nhận thao tác</h4>
          <p id="confirmModalDesc" class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-1">Bạn có chắc chắn muốn thực hiện hành động này không?</p>
        </div>
      </div>
      <div class="flex justify-end gap-2 mt-5">
        <button onclick="closeConfirmModal()" class="px-4 py-2 rounded-xl text-xs sm:text-sm font-medium text-slate-600 dark:text-slate-300 hover:bg-slate-100 dark:hover:bg-slate-800 transition">Hủy</button>
        <button id="confirmModalActionBtn" class="px-4 py-2 rounded-xl text-xs sm:text-sm font-semibold bg-fpt-orange hover:bg-fpt-orangeHover text-white transition">Đồng ý</button>
      </div>
    </div>
  </div>

  <!-- ===================================================================== -->
  <!-- JAVASCRIPT APPLICATION LOGIC & DATA                                   -->
  <!-- ===================================================================== -->
  <script>
    // -------------------------------------------------------------------------
    // 1. MASTER DATA: 8 SLIDES OF CNTT 4.0 (Y1 & Y2 DIGITAL LIBRARY)
    // -------------------------------------------------------------------------
    const SLIDES_DATA = [
      {
        id: 1,
        code: "Slide 1",
        title: "Tổng quan về CMCN 4.0, CNTT 4.0 & Chuyển đổi số",
        badgeColor: "orange",
        summary: "Khái quát lịch sử 4 cuộc Cách mạng công nghiệp (1.0 Cơ khí hóa, 2.0 Điện khí hóa, 3.0 Tự động hóa & Máy tính, 4.0 Hệ thống thực-ảo CPS kết hợp AI, IoT, Big Data, Cloud). Phân biệt rõ Tin học hóa (Digitization/Digitalization - số hóa quy trình sẵn có) với Chuyển đổi số (Digital Transformation - thay đổi mô hình vận hành dựa trên dữ liệu) cùng 3 trụ cột quốc gia: Chính phủ số, Kinh tế số, Xã hội số.",
        keyConcepts: ["CMCN 1.0 → 4.0", "AI • IoT • Big Data • Cloud", "Tin học hóa vs Chuyển đổi số", "3 trụ cột CĐS Việt Nam"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="18" text-anchor="middle" fill="#F37021" font-size="9.5" font-weight="bold">3 TRỤ CỘT CHUYỂN ĐỔI SỐ QUỐC GIA & CÔNG NGHỆ LÕI 4.0</text>
            <rect x="16" y="28" width="102" height="42" rx="6" fill="#1E293B"/>
            <text x="67" y="47" text-anchor="middle" fill="#FFF" font-size="9" font-weight="bold">Chính phủ số</text>
            <text x="67" y="60" text-anchor="middle" fill="#94A3B8" font-size="7.5">DVCQG • VNeID</text>
            <rect x="129" y="28" width="102" height="42" rx="6" fill="#F37021"/>
            <text x="180" y="47" text-anchor="middle" fill="#FFF" font-size="9" font-weight="bold">Kinh tế số</text>
            <text x="180" y="60" text-anchor="middle" fill="#FFEDD5" font-size="7.5">TMĐT • Fintech • AI</text>
            <rect x="242" y="28" width="102" height="42" rx="6" fill="#0284C7"/>
            <text x="293" y="47" text-anchor="middle" fill="#FFF" font-size="9" font-weight="bold">Xã hội số</text>
            <text x="293" y="60" text-anchor="middle" fill="#E0F2FE" font-size="7.5">Công dân số • EdTech</text>
            <rect x="16" y="78" width="328" height="26" rx="5" fill="#FFF7ED" stroke="#FDBA74"/>
            <text x="180" y="94" text-anchor="middle" fill="#9A3412" font-size="8.5" font-weight="bold">Nền tảng lõi: Trí tuệ nhân tạo (AI) + IoT + Dữ liệu lớn (Big Data) + Cloud</text>
          </svg>
        `,
        references: [
          { label: "Cổng Dịch vụ công Quốc gia (DVCQG)", url: "https://dichvucong.gov.vn" },
          { label: "Cổng Chuyển đổi số Quốc gia (Bộ TT&TT)", url: "https://dx.gov.vn" }
        ]
      },
      {
        id: 2,
        code: "Slide 2",
        title: "Máy tính & Phần cứng",
        badgeColor: "sky",
        summary: "Phân tích kiến trúc máy tính kinh điển Von Neumann gồm 4 khối chức năng: Bộ xử lý trung tâm (CPU: ALU, CU, Registers), Bộ nhớ chính (RAM khả biến tốc độ cao & ROM bất biến chứa Firmware/BIOS), Thiết bị lưu trữ thứ cấp (SSD NVMe, HDD) và Thiết bị nhập/xuất (I/O). Cập nhật xu hướng phần cứng kỷ nguyên AI với GPU, NPU (Neural Processing Unit) tích hợp trên chip hiện đại.",
        keyConcepts: ["Kiến trúc Von Neumann", "CPU (ALU + CU)", "RAM vs ROM", "SSD NVMe • GPU & NPU"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="18" text-anchor="middle" fill="#0284C7" font-size="9.5" font-weight="bold">SƠ ĐỒ KIẾN TRÚC VON NEUMANN & TĂNG TỐC AI</text>
            <rect x="14" y="42" width="68" height="44" rx="6" fill="#334155"/>
            <text x="48" y="62" text-anchor="middle" fill="#FFF" font-size="8.5" font-weight="bold">INPUT</text>
            <text x="48" y="74" text-anchor="middle" fill="#CBD5E1" font-size="7">Phím, Chuột, Mic</text>
            <rect x="108" y="28" width="144" height="74" rx="8" fill="#EFF6FF" stroke="#0284C7" stroke-width="1.5"/>
            <text x="180" y="43" text-anchor="middle" fill="#0369A1" font-size="8.5" font-weight="bold">CPU (CU + ALU) & NPU</text>
            <rect x="118" y="50" width="124" height="20" rx="4" fill="#0284C7"/>
            <text x="180" y="63" text-anchor="middle" fill="#FFF" font-size="8">Bộ nhớ chính (RAM / ROM)</text>
            <rect x="118" y="74" width="124" height="20" rx="4" fill="#F37021"/>
            <text x="180" y="87" text-anchor="middle" fill="#FFF" font-size="8">Lưu trữ (SSD NVMe / HDD)</text>
            <rect x="278" y="42" width="68" height="44" rx="6" fill="#334155"/>
            <text x="312" y="62" text-anchor="middle" fill="#FFF" font-size="8.5" font-weight="bold">OUTPUT</text>
            <text x="312" y="74" text-anchor="middle" fill="#CBD5E1" font-size="7">Màn hình, Loa</text>
            <line x1="82" y1="64" x2="108" y2="64" stroke="#F37021" stroke-width="2"/>
            <line x1="252" y1="64" x2="278" y2="64" stroke="#F37021" stroke-width="2"/>
          </svg>
        `,
        references: [
          { label: "Google NotebookLM — Tóm tắt tài liệu kỹ thuật", url: "https://notebooklm.google.com" },
          { label: "Intel / ARM Architecture Overview", url: "https://www.arm.com/architecture" }
        ]
      },
      {
        id: 3,
        code: "Slide 3",
        title: "Hệ điều hành & Môi trường số đa nền tảng",
        badgeColor: "indigo",
        summary: "Hệ điều hành (OS) là lớp phần mềm trung gian quản lý tài nguyên phần cứng (Tiến trình CPU, Bộ nhớ RAM, Hệ thống tệp tin, Thiết bị I/O) và cung cấp giao diện cho người dùng. So sánh 3 kiến trúc hạt nhân (Kernel): Monolithic (Linux), Layered, và Microkernel. Tìm hiểu công nghệ Ảo hóa (Hypervisor), Container hóa (Docker) và xu hướng OS tích hợp trợ lý AI (Windows Copilot, macOS Apple Intelligence).",
        keyConcepts: ["Monolithic vs Microkernel", "Quản lý tiến trình & RAM", "Ảo hóa & Docker Container", "AI-Integrated OS"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="17" text-anchor="middle" fill="#4F46E5" font-size="9.5" font-weight="bold">KIẾN TRÚC PHÂN LỚP HỆ ĐIỀU HÀNH & CONTAINER</text>
            <rect x="24" y="25" width="312" height="22" rx="4" fill="#EEF2FF" stroke="#6366F1"/>
            <text x="180" y="39" text-anchor="middle" fill="#3730A3" font-size="8.5" font-weight="bold">Ứng dụng Người dùng (Web, Office, Docker Containers, AI Copilot)</text>
            <rect x="24" y="52" width="312" height="24" rx="4" fill="#4F46E5"/>
            <text x="180" y="67" text-anchor="middle" fill="#FFF" font-size="8.5" font-weight="bold">Hạt nhân Hệ điều hành (Kernel: Process • Memory • File • I/O)</text>
            <rect x="24" y="81" width="312" height="22" rx="4" fill="#1E293B"/>
            <text x="180" y="95" text-anchor="middle" fill="#E2E8F0" font-size="8.5" font-weight="bold">Phần cứng Vật lý / Hạ tầng Ảo hóa (CPU, RAM, Disk, Network)</text>
          </svg>
        `,
        references: [
          { label: "The Linux Kernel Documentation", url: "https://www.kernel.org/doc/html/latest/" },
          { label: "Docker Container Overview", url: "https://docs.docker.com/get-started/" }
        ]
      },
      {
        id: 4,
        code: "Slide 4",
        title: "Phần mềm, Điện toán đám mây & Nền tảng",
        badgeColor: "emerald",
        summary: "Định nghĩa phần mềm theo chuẩn IEEE (chương trình, thủ tục, tài liệu và dữ liệu đi kèm). Phân loại phần mềm hệ thống vs phần mềm ứng dụng; giấy phép bản quyền thương mại vs Mã nguồn mở (Open Source: MIT, GPL). Nắm vững 5 đặc trưng Điện toán đám mây chuẩn NIST, 3 mô hình dịch vụ (IaaS, PaaS, SaaS) và 4 mô hình triển khai (Public, Private, Hybrid, Multi-Cloud).",
        keyConcepts: ["Chuẩn phần mềm IEEE", "Open Source vs Thương mại", "5 đặc trưng Cloud NIST", "IaaS • PaaS • SaaS"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="18" text-anchor="middle" fill="#059669" font-size="9.5" font-weight="bold">MÔ HÌNH DỊCH VỤ ĐIỆN TOÁN ĐÁM MÂY (NIST CLOUD)</text>
            <rect x="18" y="28" width="100" height="74" rx="6" fill="#ECFDF5" stroke="#10B981"/>
            <text x="68" y="45" text-anchor="middle" fill="#047857" font-size="10" font-weight="bold">IaaS</text>
            <text x="68" y="60" text-anchor="middle" fill="#065F46" font-size="7.5">Hạ tầng máy chủ ảo</text>
            <text x="68" y="73" text-anchor="middle" fill="#065F46" font-size="7.5">AWS EC2, Google GCE</text>
            <text x="68" y="90" text-anchor="middle" fill="#059669" font-size="7" font-weight="bold">Quản trị: SysAdmin</text>

            <rect x="130" y="28" width="100" height="74" rx="6" fill="#EFF6FF" stroke="#3B82F6"/>
            <text x="180" y="45" text-anchor="middle" fill="#1D4ED8" font-size="10" font-weight="bold">PaaS</text>
            <text x="180" y="60" text-anchor="middle" fill="#1E40AF" font-size="7.5">Nền tảng triển khai App</text>
            <text x="180" y="73" text-anchor="middle" fill="#1E40AF" font-size="7.5">GitHub Pages, Vercel</text>
            <text x="180" y="90" text-anchor="middle" fill="#2563EB" font-size="7" font-weight="bold">Đối tượng: Developer</text>

            <rect x="242" y="28" width="100" height="74" rx="6" fill="#FFF7ED" stroke="#F97316"/>
            <text x="292" y="45" text-anchor="middle" fill="#C2410C" font-size="10" font-weight="bold">SaaS</text>
            <text x="292" y="60" text-anchor="middle" fill="#9A3412" font-size="7.5">Phần mềm dùng ngay</text>
            <text x="292" y="73" text-anchor="middle" fill="#9A3412" font-size="7.5">Google Docs, Notion</text>
            <text x="292" y="90" text-anchor="middle" fill="#EA580C" font-size="7" font-weight="bold">Đối tượng: End-User</text>
          </svg>
        `,
        references: [
          { label: "NIST Definition of Cloud Computing (SP 800-145)", url: "https://csrc.nist.gov/publications/detail/sp/800-145/final" },
          { label: "GitHub Pages Documentation", url: "https://docs.github.com/pages" }
        ]
      },
      {
        id: 5,
        code: "Slide 5",
        title: "Mạng máy tính, Internet & IoT",
        badgeColor: "cyan",
        summary: "Nắm vững kiến trúc truyền thông mạng theo mô hình tham chiếu OSI 7 tầng và bộ giao thức thực tế TCP/IP 4 tầng (HTTP/HTTPS, DNS, TCP/UDP, IP). Phân tích hệ sinh thái Internet vạn vật (IoT) gồm 3 lớp: Lớp cảm biến/thiết bị biên (Sensors/Actuators), Lớp truyền dẫn mạng (Wi-Fi 6, 5G, MQTT, LoRaWAN) và Lớp ứng dụng thông minh (Smart Home, Smart Agriculture tại Tây Nguyên).",
        keyConcepts: ["Mô hình OSI 7 tầng & TCP/IP", "HTTP/HTTPS • DNS • IP", "Kiến trúc IoT 3 lớp", "MQTT & Cảm biến thông minh"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="18" text-anchor="middle" fill="#0891B2" font-size="9.5" font-weight="bold">HỆ SINH THÁI MẠNG TCP/IP & KIẾN TRÚC IOT 3 LỚP</text>
            <rect x="18" y="32" width="96" height="64" rx="6" fill="#ECFEFF" stroke="#06B6D4"/>
            <text x="66" y="50" text-anchor="middle" fill="#0E7490" font-size="8.5" font-weight="bold">1. Cảm biến (Edge)</text>
            <text x="66" y="65" text-anchor="middle" fill="#155E75" font-size="7.5">Nhiệt độ, Độ ẩm đất</text>
            <text x="66" y="78" text-anchor="middle" fill="#155E75" font-size="7.5">ESP32 / Arduino</text>

            <rect x="132" y="32" width="96" height="64" rx="6" fill="#F0F9FF" stroke="#0284C7"/>
            <text x="180" y="50" text-anchor="middle" fill="#0369A1" font-size="8.5" font-weight="bold">2. Truyền dẫn Mạng</text>
            <text x="180" y="65" text-anchor="middle" fill="#075985" font-size="7.5">TCP/IP • Wi-Fi • 5G</text>
            <text x="180" y="78" text-anchor="middle" fill="#075985" font-size="7.5">Giao thức MQTT / HTTP</text>

            <rect x="246" y="32" width="96" height="64" rx="6" fill="#1E293B"/>
            <text x="294" y="50" text-anchor="middle" fill="#FFF" font-size="8.5" font-weight="bold">3. Ứng dụng Cloud</text>
            <text x="294" y="65" text-anchor="middle" fill="#CBD5E1" font-size="7.5">Dashboard giám sát</text>
            <text x="294" y="78" text-anchor="middle" fill="#38BDF8" font-size="7.5">Nông nghiệp thông minh</text>

            <line x1="114" y1="64" x2="132" y2="64" stroke="#F37021" stroke-width="2"/>
            <line x1="228" y1="64" x2="246" y2="64" stroke="#F37021" stroke-width="2"/>
          </svg>
        `,
        references: [
          { label: "Cloudflare Learning — What is TCP/IP & OSI?", url: "https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/" },
          { label: "MQTT IoT Protocol Specification", url: "https://mqtt.org" }
        ]
      },
      {
        id: 6,
        code: "Slide 6",
        title: "Tư duy tính toán & Tự động hóa CNTT",
        badgeColor: "amber",
        summary: "Rèn luyện phương pháp giải quyết vấn đề phức tạp qua 4 trụ cột của Tư duy tính toán (Computational Thinking): (1) Phân rã bài toán (Decomposition), (2) Nhận diện mẫu (Pattern Recognition), (3) Trừu tượng hóa (Abstraction), và (4) Thiết kế thuật toán (Algorithms). Ứng dụng vào Tự động hóa quy trình CNTT (IT Automation) với các công cụ Low-code/No-code (Zapier, Make, n8n, Power Automate).",
        keyConcepts: ["Decomposition (Phân rã)", "Pattern Recognition", "Abstraction (Trừu tượng hóa)", "Algorithms & Low-code"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="18" text-anchor="middle" fill="#D97706" font-size="9.5" font-weight="bold">4 TRỤ CỘT TƯ DUY TÍNH TOÁN (COMPUTATIONAL THINKING)</text>
            <rect x="14" y="28" width="78" height="72" rx="6" fill="#FFFBEB" stroke="#F59E0B"/>
            <text x="53" y="46" text-anchor="middle" fill="#B45309" font-size="8" font-weight="bold">1. Phân rã</text>
            <text x="53" y="58" text-anchor="middle" fill="#78350F" font-size="7">Decomposition</text>
            <text x="53" y="75" text-anchor="middle" fill="#92400E" font-size="6.8">Chia nhỏ việc lớn</text>

            <rect x="98" y="28" width="78" height="72" rx="6" fill="#FFF7ED" stroke="#F97316"/>
            <text x="137" y="46" text-anchor="middle" fill="#C2410C" font-size="8" font-weight="bold">2. Nhận diện mẫu</text>
            <text x="137" y="58" text-anchor="middle" fill="#7C2D12" font-size="7">Pattern Recog.</text>
            <text x="137" y="75" text-anchor="middle" fill="#9A3412" font-size="6.8">Tìm quy luật lặp</text>

            <rect x="182" y="28" width="78" height="72" rx="6" fill="#EFF6FF" stroke="#3B82F6"/>
            <text x="221" y="46" text-anchor="middle" fill="#1D4ED8" font-size="8" font-weight="bold">3. Trừu tượng</text>
            <text x="221" y="58" text-anchor="middle" fill="#1E3A8A" font-size="7">Abstraction</text>
            <text x="221" y="75" text-anchor="middle" fill="#1E40AF" font-size="6.8">Giữ cốt lõi, bỏ nhiễu</text>

            <rect x="266" y="28" width="80" height="72" rx="6" fill="#ECFDF5" stroke="#10B981"/>
            <text x="306" y="46" text-anchor="middle" fill="#047857" font-size="8" font-weight="bold">4. Thuật toán</text>
            <text x="306" y="58" text-anchor="middle" fill="#064E3B" font-size="7">Algorithms</text>
            <text x="306" y="75" text-anchor="middle" fill="#065F46" font-size="6.8">Bước giải từng bước</text>
          </svg>
        `,
        references: [
          { label: "Google for Education — Computational Thinking", url: "https://edu.google.com/resources/programs/exploring-computational-thinking/" },
          { label: "n8n Workflow Automation Docs", url: "https://docs.n8n.io" }
        ]
      },
      {
        id: 7,
        code: "Slide 7",
        title: "Trí tuệ nhân tạo, Dữ liệu & Ứng dụng",
        badgeColor: "purple",
        summary: "Phân biệt 3 cấp độ AI: ANI (AI hẹp), AGI (AI tổng quát), ASI (Siêu trí tuệ). Kiến trúc Transformer nền tảng của AI tạo sinh (GenAI: LLM, tạo ảnh/mã nguồn) và 4 đặc trưng Big Data (Volume, Velocity, Variety, Veracity). Làm chủ kỹ thuật Prompt Engineering (Zero-shot, Few-shot, Chain-of-Thought, Role-Context-Task-Format) cùng nhận diện rủi ro ảo giác (Hallucination).",
        keyConcepts: ["ANI vs AGI vs ASI", "Transformer & GenAI", "Big Data 4V", "Prompt Engineering (CoT)"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="18" text-anchor="middle" fill="#7C3AED" font-size="9.5" font-weight="bold">CÔNG THỨC PROMPT ENGINEERING CHUẨN & BIG DATA 4V</text>
            <rect x="16" y="28" width="76" height="42" rx="6" fill="#F3E8FF" stroke="#9333EA"/>
            <text x="54" y="46" text-anchor="middle" fill="#6B21A8" font-size="8.5" font-weight="bold">1. ROLE</text>
            <text x="54" y="59" text-anchor="middle" fill="#7E22CE" font-size="7">Vai trò chuyên gia</text>

            <rect x="100" y="28" width="76" height="42" rx="6" fill="#F3E8FF" stroke="#9333EA"/>
            <text x="138" y="46" text-anchor="middle" fill="#6B21A8" font-size="8.5" font-weight="bold">2. CONTEXT</text>
            <text x="138" y="59" text-anchor="middle" fill="#7E22CE" font-size="7">Bối cảnh & Dữ liệu</text>

            <rect x="184" y="28" width="76" height="42" rx="6" fill="#F3E8FF" stroke="#9333EA"/>
            <text x="222" y="46" text-anchor="middle" fill="#6B21A8" font-size="8.5" font-weight="bold">3. TASK</text>
            <text x="222" y="59" text-anchor="middle" fill="#7E22CE" font-size="7">Nhiệm vụ + CoT</text>

            <rect x="268" y="28" width="76" height="42" rx="6" fill="#F3E8FF" stroke="#9333EA"/>
            <text x="306" y="46" text-anchor="middle" fill="#6B21A8" font-size="8.5" font-weight="bold">4. FORMAT</text>
            <text x="306" y="59" text-anchor="middle" fill="#7E22CE" font-size="7">Bảng / JSON / Ý chính</text>

            <rect x="16" y="78" width="328" height="26" rx="5" fill="#1E293B"/>
            <text x="180" y="94" text-anchor="middle" fill="#E9D5FF" font-size="8.5" font-weight="bold">Kỹ thuật: Zero-shot • Few-shot • Chain-of-Thought (Suy luận từng bước)</text>
          </svg>
        `,
        references: [
          { label: "Google AI Studio — Thực hành Prompt Engineering", url: "https://aistudio.google.com" },
          { label: "Google Prompting Guide", url: "https://ai.google.dev/gemini-api/docs/prompting-intro" }
        ]
      },
      {
        id: 8,
        code: "Slide 8",
        title: "An toàn thông tin & Đạo đức số",
        badgeColor: "rose",
        summary: "Bảo vệ hệ thống thông tin dựa trên Tam giác CIA (Confidentiality - Bảo mật, Integrity - Toàn vẹn, Availability - Sẵn sàng) và tiêu chuẩn quản lý ISO/IEC 27000. Tuân thủ Đạo đức AI theo khuyến nghị của UNESCO/Microsoft/Google, phòng chống lừa đảo Deepfake và cập nhật hành lang pháp lý Việt Nam: Luật An ninh mạng 2018, Luật Trí tuệ nhân tạo (Luật số 134/2025/QH15) & Luật Bảo vệ dữ liệu cá nhân (Luật số 91/2025/QH15).",
        keyConcepts: ["Tam giác CIA & ISO 27000", "Đạo đức AI (UNESCO)", "Phòng chống Deepfake", "Luật AI 134/2025 & BVDLCN 91/2025"],
        svgDiagram: `
          <svg viewBox="0 0 360 115" class="w-full h-auto rounded-lg bg-slate-50 dark:bg-slate-950 border border-slate-200 dark:border-slate-800">
            <text x="180" y="18" text-anchor="middle" fill="#E11D48" font-size="9.5" font-weight="bold">TAM GIÁC BẢO MẬT CIA & KHUNG PHÁP LÝ SỐ VIỆT NAM</text>
            <rect x="16" y="28" width="102" height="42" rx="6" fill="#FFF1F2" stroke="#F43F5E"/>
            <text x="67" y="46" text-anchor="middle" fill="#BE123C" font-size="8.5" font-weight="bold">Confidentiality</text>
            <text x="67" y="59" text-anchor="middle" fill="#9F1239" font-size="7">Bảo mật • Mã hóa • 2FA</text>

            <rect x="129" y="28" width="102" height="42" rx="6" fill="#FFF1F2" stroke="#F43F5E"/>
            <text x="180" y="46" text-anchor="middle" fill="#BE123C" font-size="8.5" font-weight="bold">Integrity</text>
            <text x="180" y="59" text-anchor="middle" fill="#9F1239" font-size="7">Toàn vẹn • Hash • Chữ ký số</text>

            <rect x="242" y="28" width="102" height="42" rx="6" fill="#FFF1F2" stroke="#F43F5E"/>
            <text x="293" y="46" text-anchor="middle" fill="#BE123C" font-size="8.5" font-weight="bold">Availability</text>
            <text x="293" y="59" text-anchor="middle" fill="#9F1239" font-size="7">Sẵn sàng • Backup • Chống DDoS</text>

            <rect x="16" y="78" width="328" height="26" rx="5" fill="#1E293B"/>
            <text x="180" y="94" text-anchor="middle" fill="#FECDD3" font-size="8" font-weight="bold">Pháp lý VN: Luật ANM 2018 • Luật AI 134/2025/QH15 • Luật BVDLCN 91/2025/QH15</text>
          </svg>
        `,
        references: [
          { label: "UNESCO Recommendation on the Ethics of AI", url: "https://www.unesco.org/en/artificial-intelligence/recommendation-ethics" },
          { label: "Cổng Thông tin Điện tử Chính phủ (Văn bản Pháp luật)", url: "https://vanban.chinhphu.vn" }
        ]
      }
    ];

    // -------------------------------------------------------------------------
    // 2. PRE-POPULATED 12 STUDY NOTES ACROSS 6 DAYS (EXCEEDS 10 NOTES / 5 DAYS)
    // -------------------------------------------------------------------------
    const DEFAULT_NOTES = [
      {
        id: "note-01",
        title: "Phân biệt Tin học hóa (Digitization) và Chuyển đổi số (Digital Transformation)",
        slideId: 1,
        dayLabel: "Ngày 1",
        dateStr: "14/11/2026",
        tags: ["CMCN4.0", "ChuyểnĐổiSố", "TrụCộtCĐS"],
        content: "Tin học hóa là đưa quy trình thủ công truyền thống lên máy tính để tăng tốc độ xử lý. Trong khi đó, Chuyển đổi số (CĐS) là quá trình thay đổi tổng thể và toàn diện cách sống, cách làm việc và phương thức sản xuất dựa trên các công nghệ số (AI, Big Data, Cloud, IoT). Tại Việt Nam, CĐS tập trung vào 3 trụ cột chính: Chính phủ số (Cổng DVCQG, VNeID), Kinh tế số (thanh toán không tiền mặt, thương mại điện tử) và Xã hội số.",
        link: "https://dx.gov.vn",
        important: true
      },
      {
        id: "note-02",
        title: "Tổng hợp 4 cuộc Cách mạng công nghiệp từ 1.0 đến 4.0",
        slideId: 1,
        dayLabel: "Ngày 1",
        dateStr: "14/11/2026",
        tags: ["LịchSửCNTT", "CPS", "AIoT"],
        content: "CMCN 1.0 (cuối TK 18): Cơ khí hóa với động cơ hơi nước. CMCN 2.0 (cuối TK 19): Điện khí hóa và dây chuyền sản xuất hàng loạt. CMCN 3.0 (thập niên 1970): Tự động hóa nhờ máy tính điện tử và Internet. CMCN 4.0 (hiện nay): Hệ thống thực - ảo kết nối (Cyber-Physical Systems) xóa nhòa ranh giới giữa thế giới vật lý, số và sinh học thông qua AI, IoT và Dữ liệu lớn.",
        link: "https://dichvucong.gov.vn",
        important: false
      },
      {
        id: "note-03",
        title: "Kiến trúc máy tính Von Neumann & Vai trò của CPU, RAM, ROM",
        slideId: 2,
        dayLabel: "Ngày 2",
        dateStr: "15/11/2026",
        tags: ["VonNeumann", "PhầnCứng", "CPU_RAM"],
        content: "Kiến trúc Von Neumann lưu trữ cả dữ liệu và lệnh chương trình trong cùng một bộ nhớ chính. CPU gồm Khối điều khiển (CU), Khối tính toán số học & logic (ALU) và các Thanh ghi (Registers). RAM là bộ nhớ truy cập ngẫu nhiên khả biến (mất dữ liệu khi tắt nguồn), còn ROM lưu trữ Firmware/UEFI bất biến giúp khởi động phần cứng ban đầu.",
        link: "https://notebooklm.google.com",
        important: true
      },
      {
        id: "note-04",
        title: "Xu hướng phần cứng kỷ nguyên AI: Sự trỗi dậy của GPU và NPU",
        slideId: 2,
        dayLabel: "Ngày 2",
        dateStr: "15/11/2026",
        tags: ["NPU", "GPU", "AIComputer"],
        content: "Khác với CPU có ít nhân mạnh tối ưu cho tác vụ tuần tự, GPU sở hữu hàng nghìn nhân nhỏ xử lý song song ma trận cực nhanh. Hiện nay, các dòng chip laptop/smartphone mới đều tích hợp thêm NPU (Neural Processing Unit) chuyên trách suy luận các mô hình AI cục bộ (Local AI) giúp tiết kiệm pin và bảo mật dữ liệu ngay trên thiết bị.",
        link: "",
        important: false
      },
      {
        id: "note-05",
        title: "So sánh kiến trúc hạt nhân Hệ điều hành: Monolithic vs Microkernel",
        slideId: 3,
        dayLabel: "Ngày 3",
        dateStr: "16/11/2026",
        tags: ["HệĐiềuHành", "Kernel", "Linux"],
        content: "Hạt nhân nguyên khối (Monolithic Kernel như Linux) đặt toàn bộ trình điều khiển thiết bị, hệ thống tệp và quản lý bộ nhớ trong không gian nhân (Kernel Space) nên hiệu năng giao tiếp rất cao. Ngược lại, Microkernel chỉ giữ lại các dịch vụ tối thiểu nhất trong nhân, đưa driver ra không gian người dùng (User Space) để tăng tính ổn định và cô lập lỗi.",
        link: "https://www.kernel.org/doc/html/latest/",
        important: false
      },
      {
        id: "note-06",
        title: "Công nghệ Ảo hóa (VM) và Container hóa (Docker) trong triển khai ứng dụng",
        slideId: 3,
        dayLabel: "Ngày 3",
        dateStr: "16/11/2026",
        tags: ["Docker", "Virtualization", "DevOps"],
        content: "Máy ảo (Virtual Machine) mô phỏng toàn bộ phần cứng và chạy một hệ điều hành khách (Guest OS) riêng biệt nên tốn nhiều RAM/CPU. Container (như Docker) chia sẻ trực tiếp hạt nhân OS của máy chủ, chỉ đóng gói ứng dụng và thư viện phụ thuộc nên khởi động trong vài giây và nhẹ hơn gấp nhiều lần.",
        link: "https://docs.docker.com/get-started/",
        important: true
      },
      {
        id: "note-07",
        title: "Phân biệt 3 mô hình Điện toán đám mây IaaS, PaaS và SaaS theo chuẩn NIST",
        slideId: 4,
        dayLabel: "Ngày 4",
        dateStr: "17/11/2026",
        tags: ["CloudComputing", "NIST", "SaaS_PaaS_IaaS"],
        content: "Theo chuẩn NIST (5 đặc trưng: tự phục vụ theo nhu cầu, truy cập mạng rộng, dùng chung tài nguyên, co giãn nhanh, đo lường dịch vụ): IaaS cung cấp hạ tầng ảo (máy chủ, mạng); PaaS cung cấp môi trường chạy ứng dụng không cần lo quản trị OS (như GitHub Pages, Heroku); SaaS cung cấp phần mềm hoàn chỉnh trên trình duyệt (Google Workspace, Notion).",
        link: "https://csrc.nist.gov/publications/detail/sp/800-145/final",
        important: true
      },
      {
        id: "note-08",
        title: "Mô hình OSI 7 tầng, bộ giao thức TCP/IP và Hệ sinh thái IoT 3 lớp",
        slideId: 5,
        dayLabel: "Ngày 4",
        dateStr: "17/11/2026",
        tags: ["TCP_IP", "OSI", "IoT_MQTT"],
        content: "Bộ giao thức TCP/IP gồm 4 tầng (Ứng dụng, Giao vận, Internet, Truy cập mạng) vận hành toàn bộ Internet thông qua địa chỉ IP và hệ thống phân giải tên miền DNS. Trong IoT, giao thức nhẹ MQTT (cơ chế Publish/Subscribe) rất phù hợp để truyền dữ liệu từ cảm biến độ ẩm/nhiệt độ nông nghiệp tại Tây Nguyên lên Cloud.",
        link: "https://mqtt.org",
        important: false
      },
      {
        id: "note-09",
        title: "Ứng dụng 4 trụ cột Tư duy tính toán & Tự động hóa Low-code/No-code",
        slideId: 6,
        dayLabel: "Ngày 5",
        dateStr: "18/11/2026",
        tags: ["ComputationalThinking", "LowCode", "Automation"],
        content: "Khi xây dựng ứng dụng quản lý ghi chú này, mình áp dụng 4 bước Tư duy tính toán: (1) Phân rã thành các module Header, Thư viện 8 Slides, CRUD Ghi chú, AI Helper; (2) Nhận diện mẫu thẻ bài học lặp lại; (3) Trừu tượng hóa cấu trúc dữ liệu thành đối tượng JSON; (4) Thiết kế thuật toán lọc đa điều kiện và lưu LocalStorage.",
        link: "https://edu.google.com/resources/programs/exploring-computational-thinking/",
        important: false
      },
      {
        id: "note-10",
        title: "Kỹ thuật Prompt Engineering: Zero-shot, Few-shot và Chain-of-Thought",
        slideId: 7,
        dayLabel: "Ngày 5",
        dateStr: "18/11/2026",
        tags: ["PromptEngineering", "GenAI", "ChainOfThought"],
        content: "Để LLM (như Gemini, ChatGPT) trả lời chính xác và hạn chế ảo giác (Hallucination), cần viết Prompt đủ 4 yếu tố: Vai trò (Role) + Bối cảnh (Context) + Nhiệm vụ (Task) + Định dạng đầu ra (Format). Kỹ thuật Chain-of-Thought (yêu cầu AI suy luận từng bước) và Few-shot (cung cấp 2–3 ví dụ mẫu) cải thiện đáng kể độ chuẩn xác.",
        link: "https://aistudio.google.com",
        important: true
      },
      {
        id: "note-11",
        title: "Tam giác bảo mật thông tin CIA & Tiêu chuẩn quản lý ISO/IEC 27000",
        slideId: 8,
        dayLabel: "Ngày 6",
        dateStr: "19/11/2026",
        tags: ["AnToànThôngTin", "CIA_Triad", "ISO27000"],
        content: "Mọi hệ thống bảo mật đều xoay quanh Tam giác CIA: Confidentiality (Tính bảo mật — chỉ người có quyền mới xem được dữ liệu nhờ mã hóa & xác thực 2 yếu tố 2FA), Integrity (Tính toàn vẹn — dữ liệu không bị sửa đổi trái phép, kiểm chứng qua hàm băm Hash/Chữ ký số), và Availability (Tính sẵn sàng — hệ thống hoạt động liên tục, có sao lưu dự phòng).",
        link: "https://www.unesco.org/en/artificial-intelligence/recommendation-ethics",
        important: false
      },
      {
        id: "note-12",
        title: "Hành lang pháp lý số Việt Nam: Luật AI 134/2025/QH15 & Luật BVDLCN 91/2025/QH15",
        slideId: 8,
        dayLabel: "Ngày 6",
        dateStr: "19/11/2026",
        tags: ["PhápLuậtSố", "LuậtAI_2025", "BảoVệDữLiệuCáNhân"],
        content: "Sinh viên CNTT cần tuân thủ đạo đức số và pháp luật Việt Nam: Luật An ninh mạng 2018, Luật Bảo vệ dữ liệu cá nhân (Luật số 91/2025/QH15 — bảo vệ quyền đồng ý của chủ thể dữ liệu, chống mua bán trái phép thông tin cá nhân) và Luật Trí tuệ nhân tạo (Luật số 134/2025/QH15 — phân loại rủi ro hệ thống AI, minh bạch nội dung do AI tạo sinh, phòng chống lừa đảo Deepfake).",
        link: "https://vanban.chinhphu.vn",
        important: true
      }
    ];

    // -------------------------------------------------------------------------
    // 3. APPLICATION STATE & LOCALSTORAGE MANAGEMENT
    // -------------------------------------------------------------------------
    const STORAGE_KEY = "fpoly_cntt40_notes_P05007_v1";
    let notes = [];
    let selectedNoteId = null;
    let onlyImportantFilter = false;
    let currentAIMode = "summarize";

    function loadNotesFromStorage() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (raw) {
          const parsed = JSON.parse(raw);
          if (Array.isArray(parsed) && parsed.length > 0) {
            notes = parsed;
            return;
          }
        }
      } catch (err) {
        console.error("Lỗi đọc LocalStorage:", err);
      }
      // Fallback to 12 pre-populated notes
      notes = JSON.parse(JSON.stringify(DEFAULT_NOTES));
      saveNotesToStorage();
    }

    function saveNotesToStorage() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));
      } catch (err) {
        showToast("Không thể lưu vào trình duyệt (LocalStorage đầy).", "error");
      }
      updateStatsDashboard();
    }

    // -------------------------------------------------------------------------
    // 4. UI INITIALIZATION & STATS DASHBOARD
    // -------------------------------------------------------------------------
    function updateStatsDashboard() {
      const total = notes.length;
      const uniqueDays = new Set(notes.map(n => n.dayLabel)).size;
      const progressPct = Math.min(100, Math.round((total / 10) * 100));

      document.getElementById("statTotalNotes").textContent = total;
      document.getElementById("tabBadgeNotes").textContent = total;
      document.getElementById("statDaysSpan").textContent = `${uniqueDays} ngày học tập`;
      document.getElementById("statProgress").textContent = `${progressPct}%`;
      document.getElementById("statProgressBar").style.width = `${progressPct}%`;
    }

    function switchTab(tabName) {
      const tabs = ["notes", "library", "wireframe"];
      tabs.forEach(t => {
        const btn = document.getElementById(`tabBtn-${t}`);
        const content = document.getElementById(`tabContent-${t}`);
        if (t === tabName) {
          btn.classList.add("tab-active");
          btn.classList.remove("text-slate-600", "dark:text-slate-400");
          content.classList.remove("hidden");
        } else {
          btn.classList.remove("tab-active");
          btn.classList.add("text-slate-600", "dark:text-slate-400");
          content.classList.add("hidden");
        }
      });
    }

    function filterBySlideFromLibrary(slideId) {
      document.getElementById("slideFilter").value = String(slideId);
      document.getElementById("dayFilter").value = "all";
      document.getElementById("searchInput").value = "";
      onlyImportantFilter = false;
      updateStarFilterButtonUI();
      switchTab("notes");
      renderNotes();
      showToast(`Đã lọc ghi chú thuộc Slide ${slideId}`, "info");
    }

    // -------------------------------------------------------------------------
    // 5. RENDER NOTES LIST & FILTERING
    // -------------------------------------------------------------------------
    function toggleStarFilter() {
      onlyImportantFilter = !onlyImportantFilter;
      updateStarFilterButtonUI();
      renderNotes();
    }

    function updateStarFilterButtonUI() {
      const btn = document.getElementById("starFilterBtn");
      if (onlyImportantFilter) {
        btn.classList.remove("text-slate-400", "bg-slate-50", "dark:bg-slate-800");
        btn.classList.add("text-amber-500", "bg-amber-50", "dark:bg-amber-950/60", "border-amber-300", "dark:border-amber-700");
      } else {
        btn.classList.add("text-slate-400", "bg-slate-50", "dark:bg-slate-800");
        btn.classList.remove("text-amber-500", "bg-amber-50", "dark:bg-amber-950/60", "border-amber-300", "dark:border-amber-700");
      }
    }

    function getSlideMeta(slideId) {
      return SLIDES_DATA.find(s => s.id === Number(slideId)) || SLIDES_DATA[0];
    }

    function renderNotes() {
      const container = document.getElementById("notesListContainer");
      const query = document.getElementById("searchInput").value.trim().toLowerCase();
      const slideVal = document.getElementById("slideFilter").value;
      const dayVal = document.getElementById("dayFilter").value;

      const filtered = notes.filter(note => {
        const matchesSlide = slideVal === "all" || String(note.slideId) === slideVal;
        const matchesDay = dayVal === "all" || note.dayLabel === dayVal;
        const matchesStar = !onlyImportantFilter || note.important;
        const matchesQuery = !query ||
          note.title.toLowerCase().includes(query) ||
          note.content.toLowerCase().includes(query) ||
          (note.tags && note.tags.some(t => t.toLowerCase().includes(query)));
        return matchesSlide && matchesDay && matchesStar && matchesQuery;
      });

      document.getElementById("visibleNotesCount").textContent = filtered.length;

      if (filtered.length === 0) {
        container.innerHTML = `
          <div class="bg-white dark:bg-slate-900 rounded-2xl border border-dashed border-slate-300 dark:border-slate-700 p-8 text-center">
            <div class="text-3xl mb-2">🔍</div>
            <h4 class="font-bold text-slate-800 dark:text-slate-200">Không tìm thấy ghi chú phù hợp</h4>
            <p class="text-xs sm:text-sm text-slate-500 dark:text-slate-400 mt-1">Thử xóa bộ lọc tìm kiếm hoặc nhấn "+ Thêm ghi chú mới" để tạo nội dung.</p>
          </div>
        `;
        return;
      }

      // Ensure selectedNoteId is valid
      if (!selectedNoteId || !notes.some(n => n.id === selectedNoteId)) {
        selectedNoteId = filtered[0].id;
      }

      container.innerHTML = filtered.map(note => {
        const slideMeta = getSlideMeta(note.slideId);
        const isSelected = note.id === selectedNoteId;
        const tagsHtml = (note.tags || []).map(tag => `
          <span class="px-2 py-0.5 text-[11px] rounded-md bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300 font-medium">#${escapeHTML(tag)}</span>
        `).join("");

        return `
          <article onclick="selectNoteForAI('${note.id}')" class="cursor-pointer group bg-white dark:bg-slate-900 rounded-2xl border ${isSelected ? 'border-fpt-orange ring-2 ring-fpt-orange/20 shadow-md' : 'border-slate-200 dark:border-slate-800 hover:border-orange-300 dark:hover:border-slate-700 shadow-xs'} p-4 sm:p-5 transition-all">
            <div class="flex items-start justify-between gap-3">
              <div class="flex flex-wrap items-center gap-2">
                <span class="px-2.5 py-0.5 text-xs font-bold rounded-lg bg-orange-100 text-fpt-orange dark:bg-orange-950/70 dark:text-orange-300">
                  Slide ${note.slideId}
                </span>
                <span class="px-2.5 py-0.5 text-xs font-semibold rounded-lg bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-300">
                  📅 ${escapeHTML(note.dayLabel)} (${escapeHTML(note.dateStr)})
                </span>
                ${isSelected ? '<span class="px-2 py-0.5 text-[10px] font-bold rounded-md bg-emerald-100 text-emerald-700 dark:bg-emerald-950 dark:text-emerald-300">Đang chọn AI</span>' : ''}
              </div>

              <!-- Quick Actions -->
              <div class="flex items-center gap-1 shrink-0" onclick="event.stopPropagation()">
                <button onclick="toggleNoteImportant('${note.id}')" title="Đánh dấu quan trọng" class="p-1.5 rounded-lg hover:bg-slate-100 dark:hover:bg-slate-800 transition ${note.important ? 'text-amber-500' : 'text-slate-300 dark:text-slate-600 hover:text-amber-400'}">
                  <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"/></svg>
                </button>
                <button onclick="openNoteModal('${note.id}')" title="Chỉnh sửa ghi chú" class="p-1.5 rounded-lg text-slate-400 hover:text-sky-600 hover:bg-sky-50 dark:hover:bg-sky-950/50 transition">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/></svg>
                </button>
                <button onclick="confirmDeleteNote('${note.id}')" title="Xóa ghi chú" class="p-1.5 rounded-lg text-slate-400 hover:text-rose-600 hover:bg-rose-50 dark:hover:bg-rose-950/50 transition">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
                </button>
              </div>
            </div>

            <h3 class="mt-2.5 text-base font-bold text-slate-900 dark:text-white group-hover:text-fpt-orange transition">
              ${escapeHTML(note.title)}
            </h3>

            <p class="mt-1.5 text-xs sm:text-sm text-slate-600 dark:text-slate-300 leading-relaxed">
              ${escapeHTML(note.content)}
            </p>

            <div class="mt-3 pt-3 border-t border-slate-100 dark:border-slate-800/80 flex flex-wrap items-center justify-between gap-2">
              <div class="flex flex-wrap items-center gap-1.5">
                ${tagsHtml}
              </div>

              ${note.link ? `
                <a href="${escapeHTML(note.link)}" target="_blank" rel="noopener noreferrer" onclick="event.stopPropagation()" class="inline-flex items-center gap-1 text-xs font-semibold text-sky-600 dark:text-sky-400 hover:underline">
                  <span>Tài liệu tham chiếu</span>
                  <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"/></svg>
                </a>
              ` : `<span class="text-[11px] text-slate-400">${escapeHTML(slideMeta.title)}</span>`}
            </div>
          </article>
        `;
      }).join("");

      updateAIPanelContext();
    }

    // -------------------------------------------------------------------------
    // 6. CRUD OPERATIONS (CREATE, UPDATE, DELETE, TOGGLE STAR, RESET)
    // -------------------------------------------------------------------------
    function openNoteModal(noteId = null) {
      const modal = document.getElementById("noteModal");
      const titleEl = document.getElementById("modalTitle");
      const form = document.getElementById("noteForm");
      form.reset();

      if (noteId) {
        const existing = notes.find(n => n.id === noteId);
        if (!existing) return;
        titleEl.innerHTML = `<span class="w-2.5 h-2.5 rounded-full bg-sky-400"></span><span>Chỉnh Sửa Ghi Chú Học Tập</span>`;
        document.getElementById("noteIdInput").value = existing.id;
        document.getElementById("noteTitleInput").value = existing.title;
        document.getElementById("noteSlideInput").value = existing.slideId;
        document.getElementById("noteDayInput").value = `${existing.dayLabel}|${existing.dateStr}`;
        document.getElementById("noteTagsInput").value = (existing.tags || []).join(", ");
        document.getElementById("noteLinkInput").value = existing.link || "";
        document.getElementById("noteContentInput").value = existing.content;
        document.getElementById("noteImportantInput").checked = Boolean(existing.important);
      } else {
        titleEl.innerHTML = `<span class="w-2.5 h-2.5 rounded-full bg-fpt-orange"></span><span>Thêm Ghi Chú Học Tập Mới</span>`;
        document.getElementById("noteIdInput").value = "";
        document.getElementById("noteDayInput").value = "Ngày 6|19/11/2026";
      }

      modal.classList.remove("hidden");
      modal.classList.add("flex");
    }

    function closeNoteModal() {
      const modal = document.getElementById("noteModal");
      modal.classList.add("hidden");
      modal.classList.remove("flex");
    }

    function saveNoteForm(event) {
      event.preventDefault();
      const id = document.getElementById("noteIdInput").value.trim();
      const title = document.getElementById("noteTitleInput").value.trim();
      const slideId = Number(document.getElementById("noteSlideInput").value);
      const dayRaw = document.getElementById("noteDayInput").value.split("|");
      const dayLabel = dayRaw[0] || "Ngày 6";
      const dateStr = dayRaw[1] || "19/11/2026";
      const tagsRaw = document.getElementById("noteTagsInput").value;
      const tags = tagsRaw.split(",").map(t => t.trim().replace(/^#/, "")).filter(Boolean);
      const link = document.getElementById("noteLinkInput").value.trim();
      const content = document.getElementById("noteContentInput").value.trim();
      const important = document.getElementById("noteImportantInput").checked;

      if (!title || !content) {
        showToast("Vui lòng nhập đầy đủ Tiêu đề và Nội dung ghi chú!", "error");
        return;
      }

      if (id) {
        const idx = notes.findIndex(n => n.id === id);
        if (idx !== -1) {
          notes[idx] = { ...notes[idx], title, slideId, dayLabel, dateStr, tags, link, content, important };
          selectedNoteId = id;
          showToast("Đã cập nhật ghi chú thành công!", "success");
        }
      } else {
        const newId = "note-" + Date.now();
        notes.unshift({ id: newId, title, slideId, dayLabel, dateStr, tags, link, content, important });
        selectedNoteId = newId;
        showToast("Đã thêm ghi chú mới vào kho học tập!", "success");
      }

      saveNotesToStorage();
      closeNoteModal();
      renderNotes();
    }

    function toggleNoteImportant(id) {
      const idx = notes.findIndex(n => n.id === id);
      if (idx !== -1) {
        notes[idx].important = !notes[idx].important;
        saveNotesToStorage();
        renderNotes();
      }
    }

    function confirmDeleteNote(id) {
      const target = notes.find(n => n.id === id);
      if (!target) return;
      openConfirmModal(
        "Xóa ghi chú học tập?",
        `Bạn có chắc muốn xóa ghi chú "${target.title}"? Thao tác này không thể hoàn tác.`,
        () => {
          notes = notes.filter(n => n.id !== id);
          if (selectedNoteId === id) {
            selectedNoteId = notes.length > 0 ? notes[0].id : null;
          }
          saveNotesToStorage();
          renderNotes();
          showToast("Đã xóa ghi chú.", "info");
        }
      );
    }

    function openResetConfirmModal() {
      openConfirmModal(
        "Khôi phục 12 ghi chú chuẩn ban đầu?",
        "Toàn bộ thay đổi hiện tại sẽ được đặt lại về bộ dữ liệu mẫu 12 ghi chú trải dài 6 ngày học của sinh viên Trần Đãi Phong.",
        () => {
          notes = JSON.parse(JSON.stringify(DEFAULT_NOTES));
          selectedNoteId = notes[0].id;
          saveNotesToStorage();
          renderNotes();
          showToast("Đã khôi phục 12 ghi chú mẫu chuẩn!", "success");
        }
      );
    }

    // -------------------------------------------------------------------------
    // 7. SMART CLIENT-SIDE AI STUDY ASSISTANT (Y6 REQUIREMENT)
    // -------------------------------------------------------------------------
    function selectNoteForAI(noteId) {
      selectedNoteId = noteId;
      renderNotes();
      runAIAction(currentAIMode);
    }

    function updateAIPanelContext() {
      const note = notes.find(n => n.id === selectedNoteId) || notes[0];
      const titleEl = document.getElementById("aiSelectedNoteTitle");
      const metaEl = document.getElementById("aiSelectedNoteMeta");
      if (!note) {
        titleEl.textContent = "Chưa có ghi chú nào được chọn";
        metaEl.textContent = "";
        return;
      }
      const slideMeta = getSlideMeta(note.slideId);
      titleEl.textContent = note.title;
      metaEl.textContent = `Slide ${note.slideId}: ${slideMeta.title} • ${note.dayLabel} (${note.dateStr})`;
    }

    function runAIAction(mode) {
      currentAIMode = mode;
      const note = notes.find(n => n.id === selectedNoteId) || notes[0];
      const outputBox = document.getElementById("aiOutputBox");
      const headingEl = document.getElementById("aiOutputHeading");

      if (!note) {
        outputBox.innerHTML = `<p class="text-slate-400">Vui lòng chọn hoặc tạo một ghi chú để AI phân tích.</p>`;
        return;
      }

      const slideMeta = getSlideMeta(note.slideId);

      if (mode === "summarize") {
        headingEl.textContent = "✨ Tóm tắt ý chính (AI Key Takeaways)";
        // Split sentences smartly
        const sentences = note.content
          .split(/(?<=[.!?])\s+/)
          .map(s => s.trim())
          .filter(s => s.length > 10);

        const bullets = sentences.map((s, idx) => `
          <li class="flex items-start gap-2">
            <span class="w-4 h-4 rounded-full bg-orange-100 text-fpt-orange dark:bg-orange-950 dark:text-orange-300 text-[10px] font-bold flex items-center justify-center shrink-0 mt-0.5">${idx + 1}</span>
            <span>${escapeHTML(s)}</span>
          </li>
        `).join("");

        outputBox.innerHTML = `
          <div class="font-bold text-slate-900 dark:text-white">📌 Trọng tâm kiến thức — Slide ${note.slideId}:</div>
          <ul class="space-y-2 mt-1.5">${bullets}</ul>
          <div class="mt-3 pt-2.5 border-t border-slate-200 dark:border-slate-800 text-xs text-slate-500 dark:text-slate-400">
            <strong>Từ khóa cốt lõi bài học:</strong> ${slideMeta.keyConcepts.join(" • ")}
          </div>
        `;
      } else if (mode === "quiz") {
        headingEl.textContent = "🎯 Bộ 3 Câu hỏi Ôn tập Trắc nghiệm";
        const quizBank = getQuizForSlide(note.slideId, note);
        outputBox.innerHTML = `
          <div class="text-xs text-slate-500 dark:text-slate-400 mb-2">Tự kiểm tra kiến thức <strong>Slide ${note.slideId}</strong> (Nhấn vào đáp án để kiểm tra):</div>
          <div class="space-y-3">
            ${quizBank.map((q, qIdx) => `
              <div class="p-3 rounded-xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800">
                <div class="font-bold text-xs text-slate-900 dark:text-white mb-2">Câu ${qIdx + 1}: ${escapeHTML(q.question)}</div>
                <div class="space-y-1.5">
                  ${q.options.map((opt, oIdx) => `
                    <button type="button" onclick="checkQuizAnswer(this, ${oIdx === q.correctIndex})" class="w-full text-left px-2.5 py-1.5 rounded-lg text-xs border border-slate-200 dark:border-slate-700 hover:bg-slate-100 dark:hover:bg-slate-800 transition">
                      ${String.fromCharCode(65 + oIdx)}. ${escapeHTML(opt)}
                    </button>
                  `).join("")}
                </div>
              </div>
            `).join("")}
          </div>
        `;
      } else if (mode === "prompt") {
        headingEl.textContent = "🧠 Mẫu Prompt Engineering Chuẩn Slide 7";
        const promptTemplate = `[VAI TRÒ - ROLE]: Bạn là Giảng viên chuyên ngành Công nghệ thông tin 4.0 tại Cao đẳng FPT Polytechnic.\n\n[BỐI CẢNH - CONTEXT]: Tôi là sinh viên Trần Đãi Phong (MSSV: P05007, Lớp: F203) đang ôn tập chuyên đề "${slideMeta.title}" (Slide ${note.slideId}). Nội dung ghi chú của tôi: "${note.title} — ${note.content}"\n\n[NHIỆM VỤ - TASK]: Hãy áp dụng phương pháp suy luận từng bước (Chain-of-Thought) để:\n1. Giải thích sâu hơn các khái niệm cốt lõi (${slideMeta.keyConcepts.join(", ")}) bằng ví dụ thực tế tại Việt Nam.\n2. Chỉ ra 2 lỗi hiểu nhầm phổ biến của sinh viên khi học phần này.\n\n[ĐỊNH DẠNG - FORMAT]: Trình bày ngắn gọn bằng tiếng Việt dưới dạng bảng so sánh và gạch đầu dòng dễ nhớ.`;

        outputBox.innerHTML = `
          <div class="text-xs text-emerald-700 dark:text-emerald-300 font-medium mb-1.5">
            Áp dụng cấu trúc <strong>Role + Context + Task (CoT) + Format</strong>:
          </div>
          <pre class="whitespace-pre-wrap font-sans text-xs bg-white dark:bg-slate-900 p-3 rounded-xl border border-slate-200 dark:border-slate-800 text-slate-700 dark:text-slate-200 select-all">${escapeHTML(promptTemplate)}</pre>
        `;
      }
    }

    function checkQuizAnswer(btnEl, isCorrect) {
      const parent = btnEl.parentElement;
      Array.from(parent.children).forEach(b => {
        b.classList.remove("bg-emerald-100", "border-emerald-500", "text-emerald-800", "bg-rose-100", "border-rose-500", "text-rose-800");
      });
      if (isCorrect) {
        btnEl.classList.add("bg-emerald-100", "border-emerald-500", "text-emerald-800", "font-bold");
        showToast("Chính xác! 🎉 Bạn nắm vững kiến thức bài này.", "success");
      } else {
        btnEl.classList.add("bg-rose-100", "border-rose-500", "text-rose-800");
        showToast("Chưa chính xác, thử chọn đáp án khác nhé!", "error");
      }
    }

    function getQuizForSlide(slideId, note) {
      const bank = {
        1: [
          { question: "Đâu là 3 trụ cột chính của Chuyển đổi số quốc gia tại Việt Nam?", options: ["Phần cứng, Phần mềm, Mạng", "Chính phủ số, Kinh tế số, Xã hội số", "AI, Blockchain, Metaverse"], correctIndex: 1 },
          { question: "Điểm khác biệt cốt lõi của Chuyển đổi số so với Tin học hóa là gì?", options: ["Chỉ mua thêm máy tính mới", "Thay đổi toàn diện mô hình vận hành dựa trên dữ liệu số", "Chỉ chuyển văn bản giấy sang file PDF"], correctIndex: 1 },
          { question: "Đặc trưng nổi bật của CMCN 4.0 là gì?", options: ["Động cơ hơi nước", "Dây chuyền điện khí hóa", "Hệ thống thực - ảo kết nối (CPS) tích hợp AI, IoT, Big Data"], correctIndex: 2 }
        ],
        2: [
          { question: "Bộ nhớ nào bị mất toàn bộ dữ liệu khi ngắt nguồn điện máy tính?", options: ["RAM", "ROM", "SSD NVMe"], correctIndex: 0 },
          { question: "Trong kiến trúc Von Neumann, bộ phận nào thực hiện các phép tính toán số học & logic?", options: ["ALU (Arithmetic Logic Unit)", "GPU", "Card mạng NIC"], correctIndex: 0 },
          { question: "Bộ xử lý nào chuyên dụng để tăng tốc các tác vụ AI/Neural Network trên thiết bị hiện đại?", options: ["NPU (Neural Processing Unit)", "BIOS", "HDD"], correctIndex: 0 }
        ],
        3: [
          { question: "Kiến trúc hạt nhân (Kernel) nào được hệ điều hành Linux sử dụng?", options: ["Monolithic Kernel", "Không có Kernel", "Chỉ chạy trên trình duyệt"], correctIndex: 0 },
          { question: "Ưu điểm chính của Container (Docker) so với Máy ảo (VM) truyền thống là gì?", options: ["Chia sẻ hạt nhân OS máy chủ nên nhẹ và khởi động cực nhanh", "Mỗi container phải cài lại Windows từ đầu", "Không thể chạy trên Cloud"], correctIndex: 0 },
          { question: "Chức năng nào KHÔNG thuộc nhiệm vụ cốt lõi của Hệ điều hành?", options: ["Quản lý tiến trình CPU", "Quản lý bộ nhớ RAM & tệp tin", "Thiết kế bo mạch chủ vật lý"], correctIndex: 2 }
        ],
        4: [
          { question: "GitHub Pages thuộc mô hình dịch vụ điện toán đám mây nào?", options: ["PaaS (Platform as a Service)", "IaaS (Infrastructure as a Service)", "Phần cứng tại chỗ (On-premise)"], correctIndex: 0 },
          { question: "Theo chuẩn NIST, Điện toán đám mây có bao nhiêu đặc trưng cốt lõi?", options: ["3 đặc trưng", "5 đặc trưng", "10 đặc trưng"], correctIndex: 1 },
          { question: "Google Docs, Notion, Gmail là ví dụ điển hình của mô hình nào?", options: ["SaaS (Software as a Service)", "IaaS", "Firmware"], correctIndex: 0 }
        ],
        5: [
          { question: "Mô hình tham chiếu mạng OSI bao gồm bao nhiêu tầng?", options: ["4 tầng", "5 tầng", "7 tầng"], correctIndex: 2 },
          { question: "Giao thức truyền thông nhẹ nào phổ biến nhất cho các cảm biến IoT?", options: ["MQTT", "FTP", "SMTP"], correctIndex: 0 },
          { question: "Hệ thống phân giải tên miền sang địa chỉ IP được gọi là gì?", options: ["DNS", "DHCP", "MAC"], correctIndex: 0 }
        ],
        6: [
          { question: "Bốn trụ cột của Tư duy tính toán (Computational Thinking) gồm những gì?", options: ["Phân rã, Nhận diện mẫu, Trừu tượng hóa, Thuật toán", "HTML, CSS, JS, SQL", "CPU, RAM, ROM, SSD"], correctIndex: 0 },
          { question: "Kỹ thuật loại bỏ chi tiết rườm rà để tập trung vào bản chất cốt lõi của vấn đề gọi là gì?", options: ["Trừu tượng hóa (Abstraction)", "Phân rã (Decomposition)", "Mã hóa (Encryption)"], correctIndex: 0 },
          { question: "Công cụ nào sau đây hỗ trợ tự động hóa quy trình theo hướng Low-code/No-code?", options: ["n8n / Zapier / Make", "Trình biên dịch C++ thuần", "BIOS Setup"], correctIndex: 0 }
        ],
        7: [
          { question: "Cấp độ AI nào chỉ giải quyết xuất sắc một nhiệm vụ chuyên biệt (như nhận diện khuôn mặt, dịch thuật)?", options: ["ANI (Artificial Narrow Intelligence)", "AGI (Artificial General Intelligence)", "ASI (Artificial Superintelligence)"], correctIndex: 0 },
          { question: "Kỹ thuật Prompt nào yêu cầu AI suy luận giải thích từng bước trước khi đưa ra đáp án?", options: ["Chain-of-Thought (CoT)", "Zero-shot", "Copy-paste"], correctIndex: 0 },
          { question: "4V đặc trưng của Dữ liệu lớn (Big Data) là gì?", options: ["Volume, Velocity, Variety, Veracity", "Video, Voice, Virtual, Visual", "Virus, VPN, VLAN, VoIP"], correctIndex: 0 }
        ],
        8: [
          { question: "Ba yếu tố trong Tam giác bảo mật thông tin CIA là gì?", options: ["Confidentiality, Integrity, Availability", "Cloud, Internet, AI", "Code, Interface, App"], correctIndex: 0 },
          { question: "Văn bản luật nào của Quốc hội Việt Nam quy định trực tiếp về quản lý và phát triển Trí tuệ nhân tạo năm 2025?", options: ["Luật Trí tuệ nhân tạo số 134/2025/QH15", "Luật Giao thông đường bộ", "Luật Xây dựng"], correctIndex: 0 },
          { question: "Luật Bảo vệ dữ liệu cá nhân số 91/2025/QH15 bảo vệ quyền lợi nào của công dân?", options: ["Quyền được biết, đồng ý và bảo vệ dữ liệu cá nhân trên môi trường số", "Quyền miễn phí sử dụng mọi phần mềm bản quyền", "Quyền chia sẻ mật khẩu ngân hàng công khai"], correctIndex: 0 }
        ]
      };
      return bank[slideId] || bank[1];
    }

    function copyAIOutput() {
      const box = document.getElementById("aiOutputBox");
      const text = box.innerText || box.textContent;
      copyTextFallback(text, "Đã sao chép nội dung từ Trợ lý AI!");
    }

    // -------------------------------------------------------------------------
    // 8. RENDER 8 SLIDES DIGITAL LIBRARY (TAB 2 - Y1 & Y2)
    // -------------------------------------------------------------------------
    function renderSlidesLibrary() {
      const grid = document.getElementById("slidesLibraryGrid");
      grid.innerHTML = SLIDES_DATA.map(slide => {
        const linkedNotesCount = notes.filter(n => n.slideId === slide.id).length;
        const conceptsHtml = slide.keyConcepts.map(c => `
          <span class="px-2 py-0.5 text-[11px] rounded-md bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-300 font-medium border border-slate-200/70 dark:border-slate-700">${escapeHTML(c)}</span>
        `).join("");

        const refsHtml = slide.references.map(ref => `
          <a href="${escapeHTML(ref.url)}" target="_blank" rel="noopener noreferrer" class="inline-flex items-center gap-1 text-xs font-medium text-sky-600 dark:text-sky-400 hover:underline">
            <svg class="w-3.5 h-3.5 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1"/></svg>
            <span>${escapeHTML(ref.label)}</span>
          </a>
        `).join("");

        return `
          <div class="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 p-5 shadow-sm flex flex-col justify-between space-y-4">
            <div class="space-y-3">
              <!-- Slide Card Header -->
              <div class="flex items-start justify-between gap-2">
                <div>
                  <span class="inline-block px-2.5 py-0.5 rounded-md bg-orange-100 text-fpt-orange dark:bg-orange-950/70 dark:text-orange-300 text-xs font-extrabold uppercase">
                    ${escapeHTML(slide.code)}
                  </span>
                  <h3 class="text-base sm:text-lg font-extrabold text-slate-900 dark:text-white mt-1">
                    ${escapeHTML(slide.title)}
                  </h3>
                </div>
                <span class="px-2.5 py-1 rounded-lg bg-emerald-50 dark:bg-emerald-950/50 text-emerald-700 dark:text-emerald-300 text-xs font-bold border border-emerald-200 dark:border-emerald-800/60 shrink-0">
                  ${linkedNotesCount} ghi chú
                </span>
              </div>

              <!-- (a) Tài liệu học tập (Slide/PDF summary) -->
              <div>
                <div class="text-[11px] font-bold uppercase tracking-wider text-slate-400 mb-1">(a) Tóm tắt nội dung Slide bài học:</div>
                <p class="text-xs sm:text-sm text-slate-600 dark:text-slate-300 leading-relaxed">
                  ${escapeHTML(slide.summary)}
                </p>
                <div class="flex flex-wrap gap-1.5 mt-2">
                  ${conceptsHtml}
                </div>
              </div>

              <!-- (b) Minh họa / Sơ đồ trực quan SVG -->
              <div>
                <div class="text-[11px] font-bold uppercase tracking-wider text-slate-400 mb-1.5">(b) Sơ đồ / Infographic trực quan:</div>
                ${slide.svgDiagram}
              </div>
            </div>

            <!-- (c) Link tham chiếu & (d) Ghi chú cá nhân liên kết -->
            <div class="pt-3 border-t border-slate-100 dark:border-slate-800 space-y-2.5">
              <div>
                <div class="text-[11px] font-bold uppercase tracking-wider text-slate-400 mb-1">(c) Nguồn tham chiếu uy tín:</div>
                <div class="flex flex-col sm:flex-row sm:flex-wrap gap-x-4 gap-y-1">
                  ${refsHtml}
                </div>
              </div>

              <div class="flex items-center justify-between pt-2">
                <span class="text-[11px] font-semibold text-slate-500 dark:text-slate-400">(d) Liên kết ghi chú cá nhân:</span>
                <button onclick="filterBySlideFromLibrary(${slide.id})" class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-xl bg-fpt-navy hover:bg-slate-800 dark:bg-slate-800 dark:hover:bg-slate-700 text-white text-xs font-semibold transition">
                  <span>Xem ${linkedNotesCount} ghi chú của Slide ${slide.id}</span>
                  <span>→</span>
                </button>
              </div>
            </div>
          </div>
        `;
      }).join("");
    }

    // -------------------------------------------------------------------------
    // 9. EXPORT / IMPORT JSON BACKUP & UTILITIES
    // -------------------------------------------------------------------------
    function exportBackupJSON() {
      const payload = {
        app: "Trợ Lý Học Tập & Quản Lý Ghi Chú Cá Nhân — CNTT 4.0",
        student: { name: "Trần Đãi Phong", mssv: "P05007", class: "F203", campus: "FPT Polytechnic Tây Nguyên" },
        exportedAt: new Date().toISOString(),
        notes: notes
      };
      const blob = new Blob([JSON.stringify(payload, null, 2)], { type: "application/json" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = `CNTT40_GhiChu_TranDaiPhong_P05007.json`;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
      showToast("Đã xuất tệp sao lưu JSON thành công!", "success");
    }

    function importBackupJSON(event) {
      const file = event.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = function(e) {
        try {
          const parsed = JSON.parse(e.target.result);
          const importedNotes = Array.isArray(parsed) ? parsed : parsed.notes;
          if (!Array.isArray(importedNotes) || importedNotes.length === 0) {
            throw new Error("Cấu trúc file JSON không hợp lệ.");
          }
          notes = importedNotes;
          selectedNoteId = notes[0].id;
          saveNotesToStorage();
          renderNotes();
          renderSlidesLibrary();
          showToast(`Đã nhập thành công ${notes.length} ghi chú từ file JSON!`, "success");
        } catch (err) {
          showToast("Lỗi nhập file JSON: Định dạng không đúng chuẩn.", "error");
        }
        event.target.value = "";
      };
      reader.readAsText(file);
    }

    // Custom Confirmation Modal Logic
    let pendingConfirmCallback = null;
    function openConfirmModal(title, desc, onConfirm) {
      document.getElementById("confirmModalTitle").textContent = title;
      document.getElementById("confirmModalDesc").textContent = desc;
      pendingConfirmCallback = onConfirm;
      const modal = document.getElementById("confirmModal");
      modal.classList.remove("hidden");
      modal.classList.add("flex");
    }

    function closeConfirmModal() {
      const modal = document.getElementById("confirmModal");
      modal.classList.add("hidden");
      modal.classList.remove("flex");
      pendingConfirmCallback = null;
    }

    document.getElementById("confirmModalActionBtn").addEventListener("click", () => {
      if (typeof pendingConfirmCallback === "function") {
        pendingConfirmCallback();
      }
      closeConfirmModal();
    });

    // Toast Notification Helper
    function showToast(message, type = "info") {
      const container = document.getElementById("toastContainer");
      const toast = document.createElement("div");
      const colors = {
        success: "bg-emerald-600 text-white border-emerald-500",
        error: "bg-rose-600 text-white border-rose-500",
        info: "bg-fpt-navy text-white border-slate-700"
      };
      toast.className = `pointer-events-auto px-4 py-3 rounded-xl shadow-lg border text-xs sm:text-sm font-medium flex items-center justify-between gap-3 animate-fade-in ${colors[type] || colors.info}`;
      toast.innerHTML = `<span>${escapeHTML(message)}</span>`;
      container.appendChild(toast);
      setTimeout(() => {
        toast.style.opacity = "0";
        toast.style.transition = "opacity 0.25s ease";
        setTimeout(() => toast.remove(), 250);
      }, 3000);
    }

    // Safe Clipboard Copy (execCommand fallback for iframe/GitHub Pages)
    function copyTextFallback(text, successMsg) {
      const textarea = document.createElement("textarea");
      textarea.value = text;
      textarea.style.position = "fixed";
      textarea.style.opacity = "0";
      document.body.appendChild(textarea);
      textarea.select();
      try {
        document.execCommand("copy");
        showToast(successMsg, "success");
      } catch (err) {
        showToast("Không thể sao chép tự động, vui lòng bôi đen văn bản.", "error");
      }
      document.body.removeChild(textarea);
    }

    // Dark Mode Toggle
    function toggleDarkMode() {
      const htmlEl = document.documentElement;
      const isDark = htmlEl.classList.toggle("dark");
      localStorage.setItem("fpoly_theme_dark", isDark ? "1" : "0");
      syncThemeIcons(isDark);
    }

    function syncThemeIcons(isDark) {
      document.getElementById("sunIcon").classList.toggle("hidden", !isDark);
      document.getElementById("moonIcon").classList.toggle("hidden", isDark);
      document.getElementById("themeLabel").textContent = isDark ? "Sáng" : "Tối";
    }

    function escapeHTML(str) {
      if (str === null || str === undefined) return "";
      return String(str)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
    }

    // Boot Application
    window.addEventListener("DOMContentLoaded", () => {
      const savedDark = localStorage.getItem("fpoly_theme_dark");
      if (savedDark === "1" || (!savedDark && window.matchMedia("(prefers-color-scheme: dark)").matches)) {
        document.documentElement.classList.add("dark");
        syncThemeIcons(true);
      }

      loadNotesFromStorage();
      updateStatsDashboard();
      renderNotes();
      renderSlidesLibrary();
      if (notes.length > 0) {
        selectedNoteId = notes[0].id;
        runAIAction("summarize");
      }
    });
  </script>
</body>
</html>
