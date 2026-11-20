# qaz1wsx2
qazwsxedcrfv
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>鑽頭研磨攻角與成形刀輪廓 CAD 系統 (端面雙刃對稱修正版)</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Microsoft JhengHei", "PingFang TC", sans-serif;
      user-select: none;
    }

    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #1e293b;
    }
    ::-webkit-scrollbar-thumb {
      background: #475569;
      border-radius: 3px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #64748b;
    }

    .cad-grid-dark {
      background-color: #0f172a;
      background-image:
        radial-gradient(rgba(51, 65, 85, 0.45) 1px, transparent 1px),
        linear-gradient(to right, rgba(30, 41, 59, 0.55) 1px, transparent 1px),
        linear-gradient(to bottom, rgba(30, 41, 59, 0.55) 1px, transparent 1px);
      background-size: 20px 20px, 100px 100px, 100px 100px;
    }

    .cad-grid-light {
      background-color: #f8fafc;
      background-image:
        radial-gradient(rgba(203, 213, 225, 0.85) 1px, transparent 1px),
        linear-gradient(to right, rgba(226, 232, 240, 0.85) 1px, transparent 1px),
        linear-gradient(to bottom, rgba(226, 232, 240, 0.85) 1px, transparent 1px);
      background-size: 20px 20px, 100px 100px, 100px 100px;
    }

    .cad-grid-hidden {
      background-color: #0f172a;
    }

    .cad-grid-light.cad-grid-hidden {
      background-color: #f8fafc;
    }

    svg {
      cursor: grab;
      display: block;
    }
    svg:active {
      cursor: grabbing;
    }

    .tab-btn.active {
      border-bottom: 2px solid #10b981;
      color: #34d399;
      font-weight: 600;
      background-color: rgba(30, 41, 59, 0.75);
    }

    .view-btn.active {
      background-color: #2563eb;
      color: #ffffff;
      border-color: #3b82f6;
    }
  </style>
</head>
<body class="bg-slate-950 text-slate-100 h-screen flex flex-col overflow-hidden">

  <!-- 頂部導航列 -->
  <header class="bg-slate-900 border-b border-slate-800 px-4 py-2 flex flex-wrap items-center justify-between gap-2 z-20 shadow-md shrink-0">
    <div class="flex items-center gap-2.5 min-w-0">
      <div class="bg-gradient-to-br from-emerald-600 to-blue-600 text-white px-2 py-1 rounded-lg shadow-sm font-black text-xs tracking-wider shrink-0">
        GRIND / CAD
      </div>
      <div class="min-w-0">
        <h1 class="text-sm font-bold text-slate-100 leading-tight truncate">鑽頭研磨攻角與成形刀輪廓系統 (雙刃對稱修正)</h1>
        <p class="text-[11px] text-slate-400 truncate">嚴格 180° 點對稱前刀面・切點長度基準・DIN 1412 幾何</p>
      </div>
    </div>

    <!-- 主畫面視圖切換器 -->
    <div class="flex items-center bg-slate-950 p-1 rounded-lg border border-slate-800 gap-1 text-xs flex-wrap">
      <button type="button" id="viewBtnProfile" class="view-btn px-2.5 py-1.5 rounded-md transition border border-transparent text-slate-300 hover:bg-slate-800 active" onclick="switchMainView('profile')">
        成形刀攻角視圖
      </button>
      <button type="button" id="viewBtnEndFace" class="view-btn px-2.5 py-1.5 rounded-md transition border border-transparent text-slate-300 hover:bg-slate-800" onclick="switchMainView('endFace')">
        端面視圖 (主畫面)
      </button>
      <button type="button" id="viewBtnDrillSide" class="view-btn px-2.5 py-1.5 rounded-md transition border border-transparent text-slate-300 hover:bg-slate-800" onclick="switchMainView('drillSide')">
        鑽頭主視圖
      </button>
      <button type="button" id="viewBtnSplit" class="view-btn px-2.5 py-1.5 rounded-md transition border border-transparent text-slate-300 hover:bg-slate-800 flex items-center gap-1" onclick="switchMainView('split')">
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17V7m6 10V7m-9 10h12a2 2 0 002-2V7a2 2 0 00-2-2H6a2 2 0 00-2 2v10a2 2 0 002 2z"/></svg>
        雙視圖分割
      </button>
    </div>

    <!-- 右側功能按鈕 -->
    <div class="flex items-center gap-1.5 shrink-0">
      <button type="button" id="toggleThemeBtn" class="px-2.5 py-1.5 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded border border-slate-700 text-xs transition">
        🌓 主題
      </button>
      <button type="button" id="exportSvgBtn" class="px-3 py-1.5 bg-emerald-600 hover:bg-emerald-500 text-white font-medium rounded text-xs transition shadow flex items-center gap-1">
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"/></svg>
        匯出 SVG
      </button>
      <button type="button" id="toggleSidebarBtn" class="px-2.5 py-1.5 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded border border-slate-700 text-xs transition">
        ☰ 側邊欄
      </button>
    </div>
  </header>

  <!-- 主內容容器 -->
  <div class="flex flex-1 overflow-hidden relative">

    <!-- 左側控制側邊欄 -->
    <aside id="sidebar" class="w-96 bg-slate-900 border-r border-slate-800 flex flex-col transition-all duration-300 z-10 shrink-0">

      <!-- 研磨攻角即時儀表板 -->
      <div class="grid grid-cols-3 gap-1.5 p-2.5 bg-slate-950/80 border-b border-slate-800">
        <div class="bg-slate-900 border border-slate-800 rounded-md px-2 py-1.5">
          <div class="text-[10px] text-slate-400 font-medium">法向前角 γn</div>
          <div id="valNormalRake" class="text-xs font-bold font-mono text-emerald-400 mt-0.5">+0.0°</div>
        </div>
        <div class="bg-slate-900 border border-slate-800 rounded-md px-2 py-1.5">
          <div class="text-[10px] text-slate-400 font-medium">工作前角 γw</div>
          <div id="valWorkingRake" class="text-xs font-bold font-mono text-emerald-400 mt-0.5">+25.7°</div>
        </div>
        <div class="bg-slate-900 border border-slate-800 rounded-md px-2 py-1.5">
          <div class="text-[10px] text-slate-400 font-medium">修磨前角 γch</div>
          <div id="valThinningRake" class="text-xs font-bold font-mono text-emerald-400 mt-0.5">+0.0°</div>
        </div>
      </div>

      <!-- 側邊欄分頁標籤 -->
      <div class="flex border-b border-slate-800 bg-slate-950 text-xs text-slate-400 shrink-0">
        <button type="button" id="tabBtnProfile" class="tab-btn flex-1 py-2 text-center transition hover:text-slate-200 active" onclick="switchSidebarTab('profile')">
          研磨輪廓與攻角
        </button>
        <button type="button" id="tabBtnDrill" class="tab-btn flex-1 py-2 text-center transition hover:text-slate-200" onclick="switchSidebarTab('drill')">
          鑽頭 (DIN 1412)
        </button>
        <button type="button" id="tabBtnView" class="tab-btn flex-1 py-2 text-center transition hover:text-slate-200" onclick="switchSidebarTab('view')">
          工藝預設與標註
        </button>
      </div>

      <!-- 分頁內容區域 -->
      <div class="flex-1 overflow-y-auto p-3.5 space-y-3.5 text-xs text-slate-300">

        <!-- ================= TAB 1: 研磨輪廓與攻角參數 ================= -->
        <div id="tabContentProfile" class="space-y-3.5">
          <div class="bg-emerald-950/35 border border-emerald-800/50 text-emerald-200 p-2.5 rounded-md leading-relaxed text-[11px]">
            <span class="font-bold text-emerald-400">鑽頭端面雙刃對稱保證：</span><br>
            • 端面兩側前刀面嚴格遵循 <strong>180° 中心點旋轉對稱</strong>。<br>
            • 外凸斷屑槽圓角與修磨刃口方向已校正，切削前刀面同步朝向前進旋轉方向。
          </div>

          <!-- 全域旋轉 -->
          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200 flex justify-between">
              <span>整體旋轉 (繞原點 P2)</span>
              <span id="globalRotVal" class="text-emerald-400 font-mono">0°</span>
            </div>
            <input type="range" id="globalRot" min="-180" max="180" value="0" step="1" class="w-full accent-emerald-500 cursor-pointer" />
            <div class="grid grid-cols-4 gap-1.5 pt-0.5">
              <button type="button" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-slate-300 text-[11px]" onclick="setProfileParam('globalRot', -30)">-30°</button>
              <button type="button" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-slate-300 text-[11px]" onclick="setProfileParam('globalRot', -15)">-15°</button>
              <button type="button" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-slate-300 text-[11px]" onclick="setProfileParam('globalRot', 15)">+15°</button>
              <button type="button" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-slate-300 text-[11px]" onclick="setProfileParam('globalRot', 0)">歸零 0°</button>
            </div>
          </div>

          <!-- 基準線：第 2 段 -->
          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200">第 2 段：芯厚修磨基準 (T2 → 原點 P2)</div>
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="text-slate-400 block mb-1">修磨段長 L2 (mm)</label>
                <input type="number" id="L2" value="15" min="0" step="0.5" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">修磨基準角 A2 (°)</label>
                <input type="number" id="A2" value="0" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none" />
              </div>
            </div>
          </div>

          <!-- 第 1 段線 -->
          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200">第 1 段：主切削前刀面 (P0 → 切點 T1)</div>
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="text-slate-400 block mb-1">前刀面長 L1 (mm)</label>
                <input type="number" id="L1" value="35" min="0" step="0.5" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">與第 2 段夾角 θ1 (°)</label>
                <input type="number" id="theta1" value="90" min="1" max="179" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none" />
              </div>
            </div>
            <div>
              <label class="text-slate-400 block mb-1">相對側向</label>
              <select id="side1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none">
                <option value="left" selected>基準線左側 (+90° 側)</option>
                <option value="right">基準線右側 (-90° 側)</option>
              </select>
            </div>
          </div>

          <!-- 外凸圓角與第 3 段線 -->
          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200">斷屑圓角 R (T1 ⌒ T2) 與讓刀段 L3 (P2 → P3)</div>
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="text-slate-400 block mb-1">圓角半徑 R (mm)</label>
                <input type="number" id="R" value="10" min="0" step="0.5" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">讓刀段長 L3 (mm)</label>
                <input type="number" id="L3" value="20" min="0" step="0.5" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none" />
              </div>
            </div>
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="text-slate-400 block mb-1">夾角 θ2 (°)</label>
                <input type="number" id="theta2" value="90" min="1" max="179" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">第 3 段側向</label>
                <select id="side2" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-emerald-500 outline-none">
                  <option value="right" selected>基準線右側 (-90° 側)</option>
                  <option value="left">基準線左側 (+90° 側)</option>
                </select>
              </div>
            </div>
          </div>

          <!-- 座標數據表格 -->
          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200">關鍵節點座標 (原點 P2 = 0, 0)</div>
            <div class="overflow-x-auto">
              <table class="w-full text-left text-[11px] border-collapse">
                <thead>
                  <tr class="border-b border-slate-800 text-slate-400">
                    <th class="py-1 px-1.5">節點</th>
                    <th class="py-1 px-1.5 text-right">X (mm)</th>
                    <th class="py-1 px-1.5 text-right">Y (mm)</th>
                  </tr>
                </thead>
                <tbody id="coordTableBody" class="divide-y divide-slate-800/60 font-mono text-slate-300"></tbody>
              </table>
            </div>
          </div>

        </div>

        <!-- ================= TAB 2: 鑽頭幾何 (DIN 1412) ================= -->
        <div id="tabContentDrill" class="space-y-3.5 hidden">
          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <label class="font-bold text-slate-200 block">DIN 1412 鑽尖修銳型式</label>
            <select id="drillFormType" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1.5 text-slate-100 focus:border-blue-500 outline-none">
              <option value="N" selected>Type N (標準鑽尖 - Standard Point)</option>
              <option value="A">Type A (修薄橫刃 - Chisel Thinning)</option>
              <option value="B">Type B (校正弧刃 - Split Point)</option>
              <option value="C">Type C (十字修薄鑽尖 - Cross Thinning)</option>
            </select>
          </div>

          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200">主尺寸與柄部</div>
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="text-slate-400 block mb-1">外徑 D (mm)</label>
                <input type="number" id="drillD" value="10.0" min="0.5" max="100" step="0.5" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">溝槽長 L (mm)</label>
                <input type="number" id="drillL" value="65.0" min="5" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">柄長 Ls (mm)</label>
                <input type="number" id="drillLs" value="35.0" min="0" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">芯厚 w (mm)</label>
                <input type="number" id="drillW" value="1.8" min="0.1" step="0.1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
            </div>
          </div>

          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200">幾何角度設定</div>
            <div class="grid grid-cols-2 gap-2">
              <div>
                <label class="text-slate-400 block mb-1">鑽尖頂角 2θ (°)</label>
                <input type="number" id="drill2Theta" value="118" min="60" max="160" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">螺旋角 β (°)</label>
                <input type="number" id="drillBeta" value="30" min="0" max="60" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">橫刃斜角 ψ (°)</label>
                <input type="number" id="drillPsi" value="125" min="90" max="150" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
              <div>
                <label class="text-slate-400 block mb-1">主要後角 α (°)</label>
                <input type="number" id="drillAlpha" value="12" min="1" max="30" step="1" class="w-full bg-slate-900 border border-slate-700 rounded px-2 py-1 text-slate-100 focus:border-blue-500 outline-none" />
              </div>
            </div>
          </div>

          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-1.5">
            <div class="font-bold text-slate-200 mb-1">推導計算數據 (Calculated)</div>
            <div class="flex justify-between text-[11px]">
              <span class="text-slate-400">螺旋溝槽導程 Lead P:</span>
              <span id="calcLead" class="font-mono text-emerald-400 font-bold">54.41 mm</span>
            </div>
            <div class="flex justify-between text-[11px]">
              <span class="text-slate-400">芯厚百分比 Web Ratio (w/D):</span>
              <span id="calcWebRatio" class="font-mono text-emerald-400 font-bold">18.0 %</span>
            </div>
            <div class="flex justify-between text-[11px]">
              <span class="text-slate-400">鑽尖高度 Point Height h:</span>
              <span id="calcPointHeight" class="font-mono text-emerald-400 font-bold">2.90 mm</span>
            </div>
          </div>
        </div>

        <!-- ================= TAB 3: 工藝預設與標註設定 ================= -->
        <div id="tabContentView" class="space-y-3.5 hidden">
          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2.5">
            <div class="font-bold text-slate-200">繪圖輔助與尺寸標註</div>
            <label class="flex items-center gap-2 cursor-pointer">
              <input type="checkbox" id="chkShowCoords" checked class="rounded border-slate-700 accent-emerald-600" />
              <span>顯示圖形關鍵點、座標與攻角標籤</span>
            </label>
            <label class="flex items-center gap-2 cursor-pointer">
              <input type="checkbox" id="chkShowDimensions" checked class="rounded border-slate-700 accent-emerald-600" />
              <span>顯示工程尺寸與退縮量標註 (Dimensions)</span>
            </label>
            <label class="flex items-center gap-2 cursor-pointer">
              <input type="checkbox" id="chkShowCenterlines" checked class="rounded border-slate-700 accent-emerald-600" />
              <span>顯示中心線與旋轉軸線 (Centerlines)</span>
            </label>
            <label class="flex items-center gap-2 cursor-pointer">
              <input type="checkbox" id="chkShowGrid" checked class="rounded border-slate-700 accent-emerald-600" />
              <span>顯示 CAD 背景參考網格</span>
            </label>
          </div>

          <div class="bg-slate-950/60 p-3 rounded-lg border border-slate-800 space-y-2">
            <div class="font-bold text-slate-200">鑽頭研磨工藝快速預設</div>
            <button type="button" onclick="presetSteelStandard()" class="w-full bg-slate-800 hover:bg-slate-700 py-1.5 rounded text-slate-200 font-medium transition text-center">
              鋼件標準研磨 (γn = 0.0°)
            </button>
            <button type="button" onclick="presetNonFerrousPositive()" class="w-full bg-emerald-600 hover:bg-emerald-500 py-1.5 rounded text-white font-medium transition text-center">
              鋁合金/非鐵金屬正前角 (γn = +10.0°)
            </button>
            <button type="button" onclick="presetHardMaterialNegative()" class="w-full bg-amber-700 hover:bg-amber-600 py-1.5 rounded text-white font-medium transition text-center">
              難削材/高硬度耐磨防崩 (γn = -6.0°)
            </button>
            <button type="button" onclick="presetDrillDIN1412()" class="w-full bg-blue-600 hover:bg-blue-500 py-1.5 rounded text-white font-medium transition text-center">
              鑽頭 DIN 1412-C 十字修薄預設
            </button>
          </div>
        </div>

      </div>
    </aside>

    <!-- 右側 CAD 工作繪圖區域 -->
    <main class="flex-1 flex flex-col relative overflow-hidden cad-grid-dark" id="mainCanvasArea">

      <!-- 頂部視窗資訊列 -->
      <div class="absolute top-3 left-4 z-10 flex items-center gap-3 bg-slate-900/90 border border-slate-800 backdrop-blur px-3 py-1.5 rounded-lg shadow text-xs">
        <span class="inline-block w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
        <span id="canvasStatusText" class="font-medium text-slate-200">視圖：成形刀攻角視圖 ｜ 法向前角 γn = +0.0°</span>
        <span class="text-slate-600">|</span>
        <span id="cursorCoordText" class="text-slate-400 font-mono">X: 0.00 , Y: 0.00</span>
      </div>

      <!-- 右上角 View Controls (以中心點縮放) -->
      <div class="absolute top-3 right-4 z-10 flex items-center gap-1.5 bg-slate-900/90 border border-slate-800 backdrop-blur p-1 rounded-lg shadow text-xs">
        <button type="button" onclick="zoomCanvasAtCenter(1.2)" class="px-2 py-1 bg-slate-800 hover:bg-slate-700 rounded text-slate-200 font-bold">+</button>
        <button type="button" onclick="zoomCanvasAtCenter(0.8)" class="px-2 py-1 bg-slate-800 hover:bg-slate-700 rounded text-slate-200 font-bold">-</button>
        <button type="button" onclick="resetCanvasView()" class="px-2.5 py-1 bg-slate-800 hover:bg-slate-700 rounded text-slate-300">重置視角</button>
        <span id="zoomScaleText" class="px-2 text-slate-400 font-mono border-l border-slate-700">100%</span>
      </div>

      <!-- 主 SVGs 容器 -->
      <div id="svgContainer" class="w-full h-full flex flex-row relative overflow-hidden">
        <!-- Viewport 1 -->
        <div id="viewport1Wrapper" class="w-full h-full relative border-r border-slate-800/80">
          <svg id="svgViewport1" class="w-full h-full">
            <g id="gViewport1"></g>
          </svg>
          <div id="v1Label" class="absolute bottom-2 left-3 text-[11px] font-bold text-slate-500 pointer-events-none select-none">
            MAIN VIEWPORT
          </div>
        </div>

        <!-- Viewport 2 (Split 時開啟) -->
        <div id="viewport2Wrapper" class="w-1/2 h-full relative hidden bg-slate-950/40">
          <svg id="svgViewport2" class="w-full h-full">
            <g id="gViewport2"></g>
          </svg>
          <div id="v2Label" class="absolute bottom-2 left-3 text-[11px] font-bold text-slate-500 pointer-events-none select-none">
            END FACE VIEWPORT (端面研磨攻角投影)
          </div>
        </div>
      </div>

      <!-- 右下角浮動卡片 Picture-in-Picture Mini View -->
      <div id="pipMiniCard" class="absolute bottom-4 right-4 z-20 w-64 h-52 bg-slate-900/95 border border-slate-700 rounded-xl shadow-2xl flex flex-col overflow-hidden backdrop-blur transition-all duration-200 hover:border-emerald-500">
        <div class="bg-slate-950 px-3 py-1.5 border-b border-slate-800 flex items-center justify-between text-[11px]">
          <span id="pipTitle" class="font-bold text-emerald-400 flex items-center gap-1">
            <span class="w-1.5 h-1.5 rounded-full bg-emerald-400"></span> 端面攻角截面預覽
          </span>
          <div class="flex items-center gap-1.5">
            <button type="button" id="pipPromoteBtn" onclick="promotePipToMain()" class="px-1.5 py-0.5 bg-slate-800 hover:bg-blue-600 text-slate-200 hover:text-white rounded text-[10px] transition">
              切至主畫面
            </button>
            <button type="button" onclick="togglePipExpand()" class="text-slate-400 hover:text-white" title="切換放大/縮小">
              ⤢
            </button>
          </div>
        </div>
        <div class="flex-1 relative cad-grid-dark">
          <svg id="svgPip" class="w-full h-full">
            <g id="gPip"></g>
          </svg>
        </div>
      </div>

      <!-- 底部操作指示 -->
      <div class="absolute bottom-2 left-4 z-10 text-[11px] text-slate-500 pointer-events-none">
        滑鼠滾輪：縮放 ｜ 左鍵拖曳：平移視角 ｜ 雙擊畫布：重置置中
      </div>
    </main>

  </div>

  <script>
    let currentViewMode = 'profile';
    let activeSidebarTab = 'profile';
    let isDarkMode = true;

    // Viewport 1
    const viewState = {
      scale: 4.5,
      baseScale: 4.5,
      panX: 0,
      panY: 0,
      isInitialized: false,
      isDragging: false,
      dragStart: { eX: 0, eY: 0, panX: 0, panY: 0 }
    };

    // Viewport 2 (Split Mode)
    const viewState2 = {
      scale: 6.0,
      baseScale: 6.0,
      panX: 0,
      panY: 0,
      isInitialized: false,
      isDragging: false,
      dragStart: { eX: 0, eY: 0, panX: 0, panY: 0 }
    };

    const dom = {
      sidebar: document.getElementById('sidebar'),
      mainCanvasArea: document.getElementById('mainCanvasArea'),
      svgContainer: document.getElementById('svgContainer'),
      viewport1Wrapper: document.getElementById('viewport1Wrapper'),
      viewport2Wrapper: document.getElementById('viewport2Wrapper'),
      svgV1: document.getElementById('svgViewport1'),
      gV1: document.getElementById('gViewport1'),
      svgV2: document.getElementById('svgViewport2'),
      gV2: document.getElementById('gViewport2'),
      pipMiniCard: document.getElementById('pipMiniCard'),
      svgPip: document.getElementById('svgPip'),
      gPip: document.getElementById('gPip'),
      pipTitle: document.getElementById('pipTitle'),
      pipPromoteBtn: document.getElementById('pipPromoteBtn'),
      canvasStatusText: document.getElementById('canvasStatusText'),
      cursorCoordText: document.getElementById('cursorCoordText'),
      zoomScaleText: document.getElementById('zoomScaleText'),
      coordTableBody: document.getElementById('coordTableBody'),
      valNormalRake: document.getElementById('valNormalRake'),
      valWorkingRake: document.getElementById('valWorkingRake'),
      valThinningRake: document.getElementById('valThinningRake'),
      globalRot: document.getElementById('globalRot'),
      globalRotVal: document.getElementById('globalRotVal'),
      L1: document.getElementById('L1'),
      L2: document.getElementById('L2'),
      L3: document.getElementById('L3'),
      R: document.getElementById('R'),
      A2: document.getElementById('A2'),
      theta1: document.getElementById('theta1'),
      theta2: document.getElementById('theta2'),
      side1: document.getElementById('side1'),
      side2: document.getElementById('side2'),
      drillFormType: document.getElementById('drillFormType'),
      drillD: document.getElementById('drillD'),
      drillL: document.getElementById('drillL'),
      drillLs: document.getElementById('drillLs'),
      drillW: document.getElementById('drillW'),
      drill2Theta: document.getElementById('drill2Theta'),
      drillBeta: document.getElementById('drillBeta'),
      drillPsi: document.getElementById('drillPsi'),
      drillAlpha: document.getElementById('drillAlpha'),
      chkShowCoords: document.getElementById('chkShowCoords'),
      chkShowDimensions: document.getElementById('chkShowDimensions'),
      chkShowCenterlines: document.getElementById('chkShowCenterlines'),
      chkShowGrid: document.getElementById('chkShowGrid')
    };

    const deg2rad = (deg) => (deg * Math.PI) / 180;
    const rad2deg = (rad) => (rad * 180) / Math.PI;
    const fmt = (n) => (Math.abs(n) < 1e-7 ? 0 : n).toFixed(3);
    const fmtAngle = (deg) => `${deg >= 0 ? "+" : ""}${deg.toFixed(1)}°`;

    function rotatePoint(pt, angleDeg) {
      const rad = deg2rad(angleDeg);
      const cos = Math.cos(rad);
      const sin = Math.sin(rad);
      return {
        x: pt.x * cos - pt.y * sin,
        y: pt.x * sin + pt.y * cos
      };
    }

    function calcFormToolGeometry() {
      const L1_actual = Math.max(0, parseFloat(dom.L1.value) || 0);
      const L2_actual = Math.max(0, parseFloat(dom.L2.value) || 0);
      const L3 = Math.max(0, parseFloat(dom.L3.value) || 0);
      const R = Math.max(0, parseFloat(dom.R.value) || 0);
      const theta1 = Math.min(179, Math.max(1, parseFloat(dom.theta1.value) || 90));
      const theta2 = Math.min(179, Math.max(1, parseFloat(dom.theta2.value) || 90));
      const side1 = dom.side1.value;
      const side2 = dom.side2.value;
      const A2 = parseFloat(dom.A2.value) || 0;
      const globalRot = parseFloat(dom.globalRot.value) || 0;
      const totalRot = A2 + globalRot;

      const beta = Math.max(0, Math.min(60, parseFloat(dom.drillBeta.value) || 0));
      const tangentDist = R > 0 ? R / Math.tan(deg2rad(theta1 / 2)) : 0;

      const P2_local = { x: 0, y: 0 };
      const T2_local = { x: -L2_actual, y: 0 };
      const P1_local = { x: -(L2_actual + tangentDist), y: 0 };

      const s1Sign = side1 === "left" ? 1 : -1;
      const dir1Rad = deg2rad(s1Sign * theta1);
      const u1 = { x: Math.cos(dir1Rad), y: Math.sin(dir1Rad) };

      const T1_local = {
        x: P1_local.x + u1.x * tangentDist,
        y: P1_local.y + u1.y * tangentDist
      };

      const P0_local = {
        x: T1_local.x + u1.x * L1_actual,
        y: T1_local.y + u1.y * L1_actual
      };

      const C_local = {
        x: T2_local.x,
        y: T2_local.y + s1Sign * R
      };

      const s2Sign = side2 === "left" ? 1 : -1;
      const dir3Rad = deg2rad(s2Sign * (180 - theta2));
      const P3_local = {
        x: P2_local.x + L3 * Math.cos(dir3Rad),
        y: P2_local.y + L3 * Math.sin(dir3Rad)
      };

      const pts = {
        P0: rotatePoint(P0_local, totalRot),
        P1: rotatePoint(P1_local, totalRot),
        T1: rotatePoint(T1_local, totalRot),
        T2: rotatePoint(T2_local, totalRot),
        C: rotatePoint(C_local, totalRot),
        P2: P2_local,
        P3: rotatePoint(P3_local, totalRot)
      };

      const normalRake = (90 - theta1) * s1Sign + A2;
      const workingRake = rad2deg(
        Math.atan(Math.tan(deg2rad(beta)) * Math.cos(deg2rad(normalRake)))
      );
      const thinningRake = A2;

      return {
        pts,
        R,
        L1_actual,
        L2_actual,
        L3,
        theta1,
        theta2,
        s1Sign,
        A2,
        totalRot,
        tangentDist,
        normalRake,
        workingRake,
        thinningRake
      };
    }

    function calcDrillGeometry() {
      const D = Math.max(0.5, parseFloat(dom.drillD.value) || 10);
      const L = Math.max(5, parseFloat(dom.drillL.value) || 65);
      const Ls = Math.max(0, parseFloat(dom.drillLs.value) || 35);
      const w = Math.max(0.1, parseFloat(dom.drillW.value) || 1.8);
      const twoTheta = parseFloat(dom.drill2Theta.value) || 118;
      const beta = parseFloat(dom.drillBeta.value) || 30;
      const psi = parseFloat(dom.drillPsi.value) || 125;
      const alpha = parseFloat(dom.drillAlpha.value) || 12;
      const formType = dom.drillFormType.value;

      const halfThetaRad = deg2rad(twoTheta / 2);
      const pointHeight = (D / 2) / Math.tan(halfThetaRad);
      const leadP = beta > 0 ? (Math.PI * D) / Math.tan(deg2rad(beta)) : Infinity;
      const webRatio = (w / D) * 100;

      document.getElementById('calcLead').textContent = isFinite(leadP) ? `${leadP.toFixed(2)} mm` : '∞ (直槽)';
      document.getElementById('calcWebRatio').textContent = `${webRatio.toFixed(1)} %`;
      document.getElementById('calcPointHeight').textContent = `${pointHeight.toFixed(2)} mm`;

      return { D, L, Ls, w, twoTheta, beta, psi, alpha, formType, pointHeight, leadP, webRatio };
    }

    function updateRakeDashboard(fGeo) {
      dom.valNormalRake.textContent = fmtAngle(fGeo.normalRake);
      dom.valWorkingRake.textContent = fmtAngle(fGeo.workingRake);
      dom.valThinningRake.textContent = fmtAngle(fGeo.thinningRake);

      dom.valNormalRake.classList.toggle('text-rose-400', fGeo.normalRake < 0);
      dom.valNormalRake.classList.toggle('text-emerald-400', fGeo.normalRake >= 0);

      dom.valWorkingRake.classList.toggle('text-rose-400', fGeo.workingRake < 0);
      dom.valWorkingRake.classList.toggle('text-emerald-400', fGeo.workingRake >= 0);

      dom.valThinningRake.classList.toggle('text-rose-400', fGeo.thinningRake < 0);
      dom.valThinningRake.classList.toggle('text-emerald-400', fGeo.thinningRake >= 0);
    }

    function renderFormToolSVG(gElem, vs, fGeo) {
      gElem.innerHTML = '';
      const { pts, R, s1Sign, tangentDist, normalRake } = fGeo;

      const toSvg = (p) => ({
        x: vs.panX + p.x * vs.scale,
        y: vs.panY - p.y * vs.scale
      });

      const sP0 = toSvg(pts.P0);
      const sP1 = toSvg(pts.P1);
      const sT1 = toSvg(pts.T1);
      const sT2 = toSvg(pts.T2);
      const sC  = toSvg(pts.C);
      const sP2 = toSvg(pts.P2);
      const sP3 = toSvg(pts.P3);
      const rPx = R * vs.scale;

      const sweepFlag = s1Sign === 1 ? 0 : 1;

      let pathD = `M ${sP0.x} ${sP0.y} L ${sT1.x} ${sT1.y} `;
      if (rPx > 0.01) {
        pathD += `A ${rPx} ${rPx} 0 0 ${sweepFlag} ${sT2.x} ${sT2.y} `;
      } else {
        pathD += `L ${sT2.x} ${sT2.y} `;
      }
      pathD += `L ${sP2.x} ${sP2.y} L ${sP3.x} ${sP3.y}`;

      let html = '';

      if (dom.chkShowCenterlines.checked) {
        html += `
          <line x1="${sP2.x - 1400}" y1="${sP2.y}" x2="${sP2.x + 1400}" y2="${sP2.y}" stroke="#475569" stroke-width="1" stroke-dasharray="6,4" />
          <line x1="${sP2.x}" y1="${sP2.y - 1400}" x2="${sP2.x}" y2="${sP2.y + 1400}" stroke="#475569" stroke-width="1" stroke-dasharray="6,4" />
        `;
      }

      html += `<polyline points="${sT1.x},${sT1.y} ${sP1.x},${sP1.y} ${sT2.x},${sT2.y}" fill="none" stroke="#64748b" stroke-width="1.5" stroke-dasharray="4,4" />`;
      if (rPx > 0.01) {
        html += `<polyline points="${sT1.x},${sT1.y} ${sC.x},${sC.y} ${sT2.x},${sT2.y}" fill="none" stroke="#3b82f6" stroke-width="1.1" stroke-dasharray="3,3" />`;
      }

      html += `<path d="${pathD}" fill="none" stroke="#38bdf8" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round" />`;
      html += `<line x1="${sP0.x}" y1="${sP0.y}" x2="${sT1.x}" y2="${sT1.y}" stroke="#34d399" stroke-width="4" stroke-linecap="round" />`;

      if (dom.chkShowCoords.checked) {
        const drawNode = (sPt, label, color, dx, dy) => `
          <circle cx="${sPt.x}" cy="${sPt.y}" r="4.5" fill="${color}" stroke="#0f172a" stroke-width="1.5" />
          <text x="${sPt.x + dx}" y="${sPt.y + dy}" font-size="12" font-weight="600" fill="#cbd5e1" font-family="monospace">${label}</text>
        `;

        html += drawNode(sP0, `P0 (${fmt(pts.P0.x)}, ${fmt(pts.P0.y)})`, "#34d399", -30, -10);
        html += drawNode(sT1, `T1`, "#3b82f6", -25, -8);
        html += drawNode(sP1, `P1`, "#94a3b8", -25, 18);
        html += drawNode(sT2, `T2`, "#3b82f6", -10, 18);
        if (rPx > 0.01) html += drawNode(sC, `C`, "#a855f7", 10, 4);
        html += drawNode(sP2, `P2 (0,0)`, "#f97316", 10, -8);
        html += drawNode(sP3, `P3 (${fmt(pts.P3.x)}, ${fmt(pts.P3.y)})`, "#ef4444", 10, 14);
      }

      if (dom.chkShowDimensions.checked) {
        const mid1 = { x: (sP0.x + sT1.x) / 2, y: (sP0.y + sT1.y) / 2 };
        const mid2 = { x: (sT2.x + sP2.x) / 2, y: (sT2.y + sP2.y) / 2 };
        html += `
          <text x="${mid1.x - 10}" y="${mid1.y - 12}" font-size="11.5" fill="#34d399" font-weight="bold">γn=${fmtAngle(normalRake)} | L1=${fmt(fGeo.L1_actual)}</text>
          <text x="${mid2.x}" y="${mid2.y + 18}" font-size="11" fill="#38bdf8" font-weight="bold">L2=${fmt(fGeo.L2_actual)}</text>
          <text x="${sP1.x}" y="${sP1.y + 32}" font-size="10" fill="#94a3b8">d=${fmt(tangentDist)}</text>
        `;
      }

      gElem.innerHTML = html;
      updateCoordTable(pts);
    }

    function renderDrillSideSVG(gElem, vs, dGeo) {
      gElem.innerHTML = '';
      const { D, L, Ls, w, twoTheta, leadP, pointHeight, formType } = dGeo;

      const toSvg = (x, y) => ({
        x: vs.panX + x * vs.scale,
        y: vs.panY - y * vs.scale
      });

      const r = D / 2;
      const totalLen = L + Ls;

      const tip = toSvg(0, 0);
      const pTopRadius = toSvg(pointHeight, r);
      const pBotRadius = toSvg(pointHeight, -r);
      const pFluteEndTop = toSvg(L, r);
      const pFluteEndBot = toSvg(L, -r);
      const pShankEndTop = toSvg(totalLen, r);
      const pShankEndBot = toSvg(totalLen, -r);

      let html = '';

      if (dom.chkShowCenterlines.checked) {
        const cStart = toSvg(-10, 0);
        const cEnd = toSvg(totalLen + 15, 0);
        html += `<line x1="${cStart.x}" y1="${cStart.y}" x2="${cEnd.x}" y2="${cEnd.y}" stroke="#ef4444" stroke-width="1.2" stroke-dasharray="8,4" />`;
      }

      html += `
        <path d="M ${pFluteEndTop.x} ${pFluteEndTop.y} L ${pShankEndTop.x} ${pShankEndTop.y} L ${pShankEndBot.x} ${pShankEndBot.y} L ${pFluteEndBot.x} ${pFluteEndBot.y} Z" fill="#1e293b" stroke="#64748b" stroke-width="2" />
        <line x1="${pFluteEndTop.x}" y1="${pFluteEndTop.y}" x2="${pFluteEndBot.x}" y2="${pFluteEndBot.y}" stroke="#475569" stroke-width="1.5" />
      `;

      const bodyPath = `M ${tip.x} ${tip.y} L ${pTopRadius.x} ${pTopRadius.y} L ${pFluteEndTop.x} ${pFluteEndTop.y} L ${pFluteEndBot.x} ${pFluteEndBot.y} L ${pBotRadius.x} ${pBotRadius.y} Z`;
      html += `<path d="${bodyPath}" fill="#0f172a" stroke="#f59e0b" stroke-width="2.5" stroke-linejoin="round" />`;

      if (isFinite(leadP) && leadP > 0) {
        const numTurns = (L - pointHeight) / (leadP / 2);
        const steps = 40;
        let flutePath1 = "";
        let flutePath2 = "";

        for (let i = 0; i <= steps; i++) {
          const t = i / steps;
          const xVal = pointHeight + t * (L - pointHeight);
          const angle = t * numTurns * Math.PI * 2;
          const yVal1 = r * Math.sin(angle);
          const yVal2 = r * Math.sin(angle + Math.PI);
          const pt1 = toSvg(xVal, yVal1);
          const pt2 = toSvg(xVal, yVal2);
          flutePath1 += (i === 0 ? `M ${pt1.x} ${pt1.y} ` : `L ${pt1.x} ${pt1.y} `);
          flutePath2 += (i === 0 ? `M ${pt2.x} ${pt2.y} ` : `L ${pt2.x} ${pt2.y} `);
        }

        html += `<path d="${flutePath1}" fill="none" stroke="#fbbf24" stroke-width="1.8" stroke-dasharray="2,2" />`;
        html += `<path d="${flutePath2}" fill="none" stroke="#d97706" stroke-width="1.8" />`;
      }

      if (formType === 'C' || formType === 'A') {
        const thinPt1 = toSvg(pointHeight * 0.4, w * 0.6);
        const thinPt2 = toSvg(pointHeight * 0.4, -w * 0.6);
        html += `<line x1="${tip.x}" y1="${tip.y}" x2="${thinPt1.x}" y2="${thinPt1.y}" stroke="#38bdf8" stroke-width="2" />`;
        html += `<line x1="${tip.x}" y1="${tip.y}" x2="${thinPt2.x}" y2="${thinPt2.y}" stroke="#38bdf8" stroke-width="2" />`;
      }

      if (dom.chkShowDimensions.checked) {
        const dDimX = pFluteEndTop.x + 15;
        html += `
          <line x1="${pFluteEndTop.x}" y1="${pFluteEndTop.y}" x2="${dDimX + 10}" y2="${pFluteEndTop.y}" stroke="#94a3b8" stroke-width="1" />
          <line x1="${pFluteEndBot.x}" y1="${pFluteEndBot.y}" x2="${dDimX + 10}" y2="${pFluteEndBot.y}" stroke="#94a3b8" stroke-width="1" />
          <line x1="${dDimX}" y1="${pFluteEndTop.y}" x2="${dDimX}" y2="${pFluteEndBot.y}" stroke="#38bdf8" stroke-width="1.2" />
          <text x="${dDimX + 5}" y="${tip.y + 4}" font-size="11" font-weight="bold" fill="#38bdf8">Ø ${D.toFixed(1)}</text>
          <text x="${tip.x - 42}" y="${tip.y + 4}" font-size="11" font-weight="bold" fill="#f59e0b">2θ=${twoTheta}°</text>
        `;
      }

      gElem.innerHTML = html;
    }

    // 核心端面渲染：修正第二刃點對稱旋轉與圓弧方向
    function renderEndFaceSVG(gElem, vs, fGeo, dGeo) {
      gElem.innerHTML = '';
      const { D, w, psi, formType } = dGeo;
      const { pts, R, s1Sign, L1_actual, L2_actual, normalRake, workingRake } = fGeo;

      const toSvg = (x, y) => ({
        x: vs.panX + x * vs.scale,
        y: vs.panY - y * vs.scale
      });

      const center = toSvg(0, 0);
      const rPx = (D / 2) * vs.scale;
      const wPx = (w / 2) * vs.scale;

      let html = '';

      if (dom.chkShowCenterlines.checked) {
        html += `
          <line x1="${center.x - rPx - 20}" y1="${center.y}" x2="${center.x + rPx + 20}" y2="${center.y}" stroke="#475569" stroke-width="1" stroke-dasharray="6,4" />
          <line x1="${center.x}" y1="${center.y - rPx - 20}" x2="${center.x}" y2="${center.y + rPx + 20}" stroke="#475569" stroke-width="1" stroke-dasharray="6,4" />
        `;
      }

      html += `
        <circle cx="${center.x}" cy="${center.y}" r="${rPx}" fill="#0f172a" stroke="#38bdf8" stroke-width="2.2" />
        <circle cx="${center.x}" cy="${center.y}" r="${wPx}" fill="none" stroke="#f59e0b" stroke-width="1.1" stroke-dasharray="3,3" />
      `;

      // 投影比例：將成形刀輪廓縮放至鑽頭半徑空間
      const denom = Math.max(1, L1_actual + L2_actual);
      const mapScale = (rPx * 0.85) / denom;
      const rMapPx = R * mapScale;

      // 局部原點向量 (P2 = 0,0)
      const mapPt = (p) => ({
        dx: p.x * mapScale,
        dy: -p.y * mapScale
      });

      const dP0 = mapPt(pts.P0);
      const dT1 = mapPt(pts.T1);
      const dT2 = mapPt(pts.T2);

      // SVG sweep-flag 判定：
      // 從 T2 到 T1 繪製外凸圓弧
      const sweepFlagLip1 = s1Sign === 1 ? 1 : 0;
      // 點對稱旋轉 180° 後，在 SVG 坐標系中的順逆時針方向保持一致：
      const sweepFlagLip2 = sweepFlagLip1;

      // 構建刃口 1 (Main Lip 1: P2 -> T2 -> T1 -> P0)
      let lip1 = `M ${center.x} ${center.y} L ${center.x + dT2.dx} ${center.y + dT2.dy} `;
      if (rMapPx > 0.01) {
        lip1 += `A ${rMapPx} ${rMapPx} 0 0 ${sweepFlagLip1} ${center.x + dT1.dx} ${center.y + dT1.dy} `;
      } else {
        lip1 += `L ${center.x + dT1.dx} ${center.y + dT1.dy} `;
      }
      lip1 += `L ${center.x + dP0.dx} ${center.y + dP0.dy}`;

      // 構建刃口 2 (Main Lip 2: 繞 center 嚴格 180° 點對稱: dx' = -dx, dy' = -dy)
      let lip2 = `M ${center.x} ${center.y} L ${center.x - dT2.dx} ${center.y - dT2.dy} `;
      if (rMapPx > 0.01) {
        lip2 += `A ${rMapPx} ${rMapPx} 0 0 ${sweepFlagLip2} ${center.x - dT1.dx} ${center.y - dT1.dy} `;
      } else {
        lip2 += `L ${center.x - dT1.dx} ${center.y - dT1.dy} `;
      }
      lip2 += `L ${center.x - dP0.dx} ${center.y - dP0.dy}`;

      html += `<path d="${lip1}" fill="none" stroke="#34d399" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round" />`;
      html += `<path d="${lip2}" fill="none" stroke="#34d399" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round" />`;

      // 橫刃與修薄
      const psiRad = deg2rad(psi);
      const chiselLen = wPx * 1.1;
      const c1 = {
        x: center.x + chiselLen * Math.cos(psiRad),
        y: center.y - chiselLen * Math.sin(psiRad)
      };
      const c2 = {
        x: center.x - chiselLen * Math.cos(psiRad),
        y: center.y + chiselLen * Math.sin(psiRad)
      };
      html += `<line x1="${c1.x}" y1="${c1.y}" x2="${c2.x}" y2="${c2.y}" stroke="#ef4444" stroke-width="2.5" />`;

      if (formType === 'C') {
        html += `
          <path d="M ${c1.x} ${c1.y} L ${center.x + wPx * 0.8} ${center.y - wPx * 0.8} L ${center.x} ${center.y}" fill="none" stroke="#38bdf8" stroke-width="1.8" />
          <path d="M ${c2.x} ${c2.y} L ${center.x - wPx * 0.8} ${center.y + wPx * 0.8} L ${center.x} ${center.y}" fill="none" stroke="#38bdf8" stroke-width="1.8" />
        `;
      }

      if (dom.chkShowDimensions.checked) {
        html += `
          <text x="${center.x}" y="${center.y + rPx + 16}" font-size="11" font-weight="bold" fill="#34d399" text-anchor="middle">
            γn ${fmtAngle(normalRake)} ｜ γw ${fmtAngle(workingRake)} ｜ Ø${D}
          </text>
        `;
      }

      gElem.innerHTML = html;
    }

    function updateCoordTable(pts) {
      const nodeRows = [
        { name: "P0 (起點)", pt: pts.P0 },
        { name: "T1 (切點1)", pt: pts.T1 },
        { name: "P1 (交點)", pt: pts.P1 },
        { name: "T2 (切點2)", pt: pts.T2 },
        { name: "C (圓心)", pt: pts.C },
        { name: "P2 (原點)", pt: pts.P2 },
        { name: "P3 (終點)", pt: pts.P3 }
      ];

      dom.coordTableBody.innerHTML = nodeRows
        .map(
          (r) => `
          <tr class="hover:bg-slate-800/40">
            <td class="py-1 px-1.5 font-bold text-slate-300">${r.name}</td>
            <td class="py-1 px-1.5 text-right font-mono text-emerald-400">${fmt(r.pt.x)}</td>
            <td class="py-1 px-1.5 text-right font-mono text-emerald-400">${fmt(r.pt.y)}</td>
          </tr>`
        )
        .join("");
    }

    function autoCenterView(vs, svgElem, isDrillSide = false) {
      const rect = svgElem.getBoundingClientRect();
      const w = rect.width || 800;
      const h = rect.height || 600;
      vs.panX = isDrillSide ? w * 0.25 : w / 2;
      vs.panY = h / 2;
    }

    function getPipState(svgElem, targetMode) {
      const rect = svgElem.getBoundingClientRect();
      const w = rect.width || 240;
      const h = rect.height || 160;
      if (targetMode === 'drillSide') {
        return { scale: 1.5, panX: w * 0.15, panY: h * 0.5 };
      }
      return { scale: 5.5, panX: w * 0.5, panY: h * 0.5 };
    }

    function syncGridClass() {
      const baseGrid = isDarkMode ? "cad-grid-dark" : "cad-grid-light";
      const hiddenClass = dom.chkShowGrid.checked ? "" : "cad-grid-hidden";
      dom.mainCanvasArea.className = `flex-1 flex flex-col relative overflow-hidden ${baseGrid} ${hiddenClass}`.trim();
    }

    function renderAll() {
      syncGridClass();
      dom.globalRotVal.textContent = `${dom.globalRot.value}°`;
      const fGeo = calcFormToolGeometry();
      const dGeo = calcDrillGeometry();
      updateRakeDashboard(fGeo);

      if (!viewState.isInitialized) {
        autoCenterView(viewState, dom.svgV1, currentViewMode === 'drillSide');
        viewState.isInitialized = true;
      }
      if (!viewState2.isInitialized) {
        autoCenterView(viewState2, dom.svgV2, false);
        viewState2.isInitialized = true;
      }

      if (currentViewMode === 'profile') {
        renderFormToolSVG(dom.gV1, viewState, fGeo);
        renderEndFaceSVG(dom.gPip, getPipState(dom.svgPip, 'endFace'), fGeo, dGeo);
        dom.pipTitle.innerHTML = `<span class="w-1.5 h-1.5 rounded-full bg-emerald-400"></span> 端面攻角截面預覽`;
        dom.pipPromoteBtn.textContent = "切至主畫面";
        dom.canvasStatusText.textContent = `視圖：成形刀攻角視圖 ｜ γn = ${fmtAngle(fGeo.normalRake)}`;
      } else if (currentViewMode === 'endFace') {
        renderEndFaceSVG(dom.gV1, viewState, fGeo, dGeo);
        renderFormToolSVG(dom.gPip, getPipState(dom.svgPip, 'profile'), fGeo);
        dom.pipTitle.innerHTML = `<span class="w-1.5 h-1.5 rounded-full bg-blue-400"></span> 成形刀輪廓預覽`;
        dom.pipPromoteBtn.textContent = "切至主畫面";
        dom.canvasStatusText.textContent = `視圖：端面攻角截面 (Ø${dGeo.D.toFixed(1)}mm) ｜ γw = ${fmtAngle(fGeo.workingRake)}`;
      } else if (currentViewMode === 'drillSide') {
        renderDrillSideSVG(dom.gV1, viewState, dGeo);
        renderEndFaceSVG(dom.gPip, getPipState(dom.svgPip, 'endFace'), fGeo, dGeo);
        dom.pipTitle.innerHTML = `<span class="w-1.5 h-1.5 rounded-full bg-emerald-400"></span> 端面攻角截面預覽`;
        dom.pipPromoteBtn.textContent = "切至主畫面";
        dom.canvasStatusText.textContent = `視圖：鑽頭 DIN 1412 主視圖 ｜ 2θ=${dGeo.twoTheta}°`;
      } else if (currentViewMode === 'split') {
        renderFormToolSVG(dom.gV1, viewState, fGeo);
        renderEndFaceSVG(dom.gV2, viewState2, fGeo, dGeo);
        dom.canvasStatusText.textContent = `視圖：雙視圖分割並排 (攻角輪廓 & 端面投影)`;
      }

      dom.zoomScaleText.textContent = `${Math.round((viewState.scale / viewState.baseScale) * 100)}%`;
    }

    function switchMainView(mode) {
      currentViewMode = mode;

      document.querySelectorAll('.view-btn').forEach(b => b.classList.remove('active'));
      if (mode === 'profile') document.getElementById('viewBtnProfile').classList.add('active');
      if (mode === 'endFace') document.getElementById('viewBtnEndFace').classList.add('active');
      if (mode === 'drillSide') document.getElementById('viewBtnDrillSide').classList.add('active');
      if (mode === 'split') document.getElementById('viewBtnSplit').classList.add('active');

      if (mode === 'split') {
        dom.viewport1Wrapper.className = "w-1/2 h-full relative border-r border-slate-800";
        dom.viewport2Wrapper.classList.remove('hidden');
        dom.pipMiniCard.classList.add('hidden');
      } else {
        dom.viewport1Wrapper.className = "w-full h-full relative";
        dom.viewport2Wrapper.classList.add('hidden');
        dom.pipMiniCard.classList.remove('hidden');
      }

      viewState.isInitialized = false;
      viewState2.isInitialized = false;
      renderAll();
    }

    function promotePipToMain() {
      if (currentViewMode === 'endFace') {
        switchMainView('profile');
      } else {
        switchMainView('endFace');
      }
    }

    function switchSidebarTab(tab) {
      activeSidebarTab = tab;
      document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
      document.getElementById('tabContentProfile').classList.add('hidden');
      document.getElementById('tabContentDrill').classList.add('hidden');
      document.getElementById('tabContentView').classList.add('hidden');

      if (tab === 'profile') {
        document.getElementById('tabBtnProfile').classList.add('active');
        document.getElementById('tabContentProfile').classList.remove('hidden');
      } else if (tab === 'drill') {
        document.getElementById('tabBtnDrill').classList.add('active');
        document.getElementById('tabContentDrill').classList.remove('hidden');
      } else if (tab === 'view') {
        document.getElementById('tabBtnView').classList.add('active');
        document.getElementById('tabContentView').classList.remove('hidden');
      }
    }

    function setProfileParam(param, val) {
      if (dom[param]) {
        dom[param].value = val;
        renderAll();
      }
    }

    function zoomCanvasAtCenter(factor) {
      const zoomViewport = (vs, svgEl) => {
        const rect = svgEl.getBoundingClientRect();
        const cx = rect.width / 2;
        const cy = rect.height / 2;
        const nextScale = Math.min(Math.max(vs.scale * factor, 0.2), 150);
        vs.panX = cx - (cx - vs.panX) * (nextScale / vs.scale);
        vs.panY = cy - (cy - vs.panY) * (nextScale / vs.scale);
        vs.scale = nextScale;
      };

      zoomViewport(viewState, dom.svgV1);
      if (currentViewMode === 'split') {
        zoomViewport(viewState2, dom.svgV2);
      }
      renderAll();
    }

    function resetCanvasView() {
      viewState.isInitialized = false;
      viewState2.isInitialized = false;
      renderAll();
    }

    function togglePipExpand() {
      dom.pipMiniCard.classList.toggle('w-64');
      dom.pipMiniCard.classList.toggle('h-52');
      dom.pipMiniCard.classList.toggle('w-96');
      dom.pipMiniCard.classList.toggle('h-72');
      setTimeout(renderAll, 220);
    }

    function presetSteelStandard() {
      dom.L1.value = 35;
      dom.L2.value = 15;
      dom.L3.value = 20;
      dom.R.value = 10;
      dom.theta1.value = 90;
      dom.theta2.value = 90;
      dom.A2.value = 0;
      dom.globalRot.value = 0;
      dom.side1.value = "left";
      dom.side2.value = "right";
      switchMainView('profile');
    }

    function presetNonFerrousPositive() {
      dom.L1.value = 35;
      dom.L2.value = 15;
      dom.L3.value = 20;
      dom.R.value = 12;
      dom.theta1.value = 80;
      dom.theta2.value = 90;
      dom.A2.value = 0;
      dom.globalRot.value = 0;
      dom.side1.value = "left";
      dom.side2.value = "right";
      switchMainView('profile');
    }

    function presetHardMaterialNegative() {
      dom.L1.value = 35;
      dom.L2.value = 15;
      dom.L3.value = 20;
      dom.R.value = 8;
      dom.theta1.value = 96;
      dom.theta2.value = 90;
      dom.A2.value = 0;
      dom.globalRot.value = 0;
      dom.side1.value = "left";
      dom.side2.value = "right";
      switchMainView('profile');
    }

    function presetDrillDIN1412() {
      dom.drillD.value = 12.0;
      dom.drillL.value = 65.0;
      dom.drillW.value = 2.1;
      dom.drill2Theta.value = 118;
      dom.drillBeta.value = 30;
      dom.drillPsi.value = 125;
      dom.drillFormType.value = "C";
      switchMainView('drillSide');
      switchSidebarTab('drill');
    }

    const allInputs = [
      dom.globalRot, dom.L1, dom.L2, dom.L3, dom.R, dom.A2, dom.theta1, dom.theta2, dom.side1, dom.side2,
      dom.drillFormType, dom.drillD, dom.drillL, dom.drillLs, dom.drillW, dom.drill2Theta, dom.drillBeta, dom.drillPsi, dom.drillAlpha,
      dom.chkShowCoords, dom.chkShowDimensions, dom.chkShowCenterlines, dom.chkShowGrid
    ];

    allInputs.forEach(input => {
      if (input) {
        input.addEventListener('input', renderAll);
        input.addEventListener('change', renderAll);
      }
    });

    function setupViewportInteraction(svgEl, vs) {
      svgEl.addEventListener('mousedown', (e) => {
        vs.isDragging = true;
        vs.dragStart = { eX: e.clientX, eY: e.clientY, panX: vs.panX, panY: vs.panY };
      });

      svgEl.addEventListener('wheel', (e) => {
        e.preventDefault();
        const zoomFactor = e.deltaY < 0 ? 1.12 : 1 / 1.12;
        const rect = svgEl.getBoundingClientRect();
        const mouseX = e.clientX - rect.left;
        const mouseY = e.clientY - rect.top;

        const newScale = Math.min(Math.max(vs.scale * zoomFactor, 0.2), 150);
        vs.panX = mouseX - (mouseX - vs.panX) * (newScale / vs.scale);
        vs.panY = mouseY - (mouseY - vs.panY) * (newScale / vs.scale);
        vs.scale = newScale;
        renderAll();
      }, { passive: false });

      svgEl.addEventListener('dblclick', resetCanvasView);
    }

    setupViewportInteraction(dom.svgV1, viewState);
    setupViewportInteraction(dom.svgV2, viewState2);

    window.addEventListener('mousemove', (e) => {
      const rect = dom.svgV1.getBoundingClientRect();
      const mouseX = e.clientX - rect.left;
      const mouseY = e.clientY - rect.top;
      const cadX = (mouseX - viewState.panX) / viewState.scale;
      const cadY = -(mouseY - viewState.panY) / viewState.scale;
      dom.cursorCoordText.textContent = `X: ${cadX.toFixed(2)} , Y: ${cadY.toFixed(2)}`;

      if (viewState.isDragging) {
        viewState.panX = viewState.dragStart.panX + (e.clientX - viewState.dragStart.eX);
        viewState.panY = viewState.dragStart.panY + (e.clientY - viewState.dragStart.eY);
        renderAll();
      } else if (viewState2.isDragging) {
        viewState2.panX = viewState2.dragStart.panX + (e.clientX - viewState2.dragStart.eX);
        viewState2.panY = viewState2.dragStart.panY + (e.clientY - viewState2.dragStart.eY);
        renderAll();
      }
    });

    window.addEventListener('mouseup', () => {
      viewState.isDragging = false;
      viewState2.isDragging = false;
    });

    document.getElementById('toggleSidebarBtn').addEventListener('click', () => {
      dom.sidebar.classList.toggle('w-96');
      dom.sidebar.classList.toggle('w-0');
      dom.sidebar.classList.toggle('overflow-hidden');
      setTimeout(() => {
        viewState.isInitialized = false;
        viewState2.isInitialized = false;
        renderAll();
      }, 300);
    });

    document.getElementById('toggleThemeBtn').addEventListener('click', () => {
      isDarkMode = !isDarkMode;
      syncGridClass();
    });

    document.getElementById('exportSvgBtn').addEventListener('click', () => {
      const clone = dom.svgV1.cloneNode(true);
      clone.setAttribute("xmlns", "http://www.w3.org/2000/svg");
      if (!clone.getAttribute("viewBox")) {
        const rect = dom.svgV1.getBoundingClientRect();
        clone.setAttribute("viewBox", `0 0 ${rect.width || 800} ${rect.height || 600}`);
      }
      const serializer = new XMLSerializer();
      const source = serializer.serializeToString(clone);
      const blob = new Blob([source], { type: "image/svg+xml;charset=utf-8" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = `Grinding_CAD_${currentViewMode}_export.svg`;
      a.click();
      URL.revokeObjectURL(url);
    });

    window.onload = () => {
      renderAll();
    };
  </script>
</body>
</html>
