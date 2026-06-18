<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ranking Produk – Bengkel AA Garage</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Segoe UI', Arial, sans-serif;
      background: #f0f4f8;
      color: #1a202c;
      min-height: 100vh;
    }

    /* ── HEADER ── */
    header {
      background: linear-gradient(135deg, #1a365d 0%, #2b6cb0 100%);
      color: white;
      padding: 24px 32px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    }
    header h1 { font-size: 1.6rem; font-weight: 700; letter-spacing: 0.5px; }
    header p  { font-size: 0.85rem; opacity: 0.8; margin-top: 4px; }

    /* ── MAIN CONTAINER ── */
    .container { max-width: 1000px; margin: 0 auto; padding: 28px 20px; }

    /* ── CONTROL BAR ── */
    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 14px;
      align-items: flex-end;
      background: white;
      border-radius: 12px;
      padding: 20px 24px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
      margin-bottom: 24px;
    }

    .ctrl-group { display: flex; flex-direction: column; gap: 6px; }
    .ctrl-group label { font-size: 0.78rem; font-weight: 600; color: #4a5568; text-transform: uppercase; letter-spacing: 0.5px; }

    select, input[type="text"] {
      border: 1.5px solid #cbd5e0;
      border-radius: 8px;
      padding: 9px 14px;
      font-size: 0.92rem;
      color: #2d3748;
      background: #f7fafc;
      outline: none;
      transition: border-color 0.2s;
    }
    select:focus, input[type="text"]:focus { border-color: #3182ce; background: white; }

    input[type="text"] { width: 240px; }

    .btn-reset {
      background: #e53e3e;
      color: white;
      border: none;
      border-radius: 8px;
      padding: 9px 18px;
      font-size: 0.88rem;
      font-weight: 600;
      cursor: pointer;
      transition: background 0.2s;
    }
    .btn-reset:hover { background: #c53030; }

    /* ── CRITERIA PILLS ── */
    .criteria-pills {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 20px;
    }
    .pill {
      background: white;
      border: 1.5px solid #bee3f8;
      color: #2b6cb0;
      border-radius: 20px;
      padding: 5px 14px;
      font-size: 0.8rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s;
    }
    .pill:hover { background: #ebf8ff; }
    .pill.active { background: #2b6cb0; color: white; border-color: #2b6cb0; }

    /* ── SUMMARY CARDS ── */
    .summary {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(170px, 1fr));
      gap: 14px;
      margin-bottom: 24px;
    }
    .card {
      background: white;
      border-radius: 12px;
      padding: 18px 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.07);
      border-left: 4px solid #3182ce;
    }
    .card.gold   { border-left-color: #d69e2e; }
    .card.silver { border-left-color: #718096; }
    .card.bronze { border-left-color: #c05621; }
    .card-label { font-size: 0.72rem; color: #718096; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; }
    .card-value { font-size: 1.4rem; font-weight: 700; color: #1a202c; margin-top: 4px; }
    .card-sub   { font-size: 0.78rem; color: #4a5568; margin-top: 2px; }

    /* ── TABLE ── */
    .table-wrap {
      background: white;
      border-radius: 12px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
      overflow: hidden;
    }
    .table-header-bar {
      padding: 16px 20px;
      border-bottom: 1px solid #e2e8f0;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .table-header-bar h2 { font-size: 1rem; font-weight: 700; color: #2d3748; }
    .result-count { font-size: 0.82rem; color: #718096; }

    table {
      width: 100%;
      border-collapse: collapse;
    }
    thead tr { background: #edf2f7; }
    thead th {
      text-align: left;
      padding: 12px 16px;
      font-size: 0.78rem;
      font-weight: 700;
      color: #4a5568;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      white-space: nowrap;
    }
    thead th.num { text-align: center; }

    tbody tr {
      border-bottom: 1px solid #f0f4f8;
      transition: background 0.15s;
    }
    tbody tr:last-child { border-bottom: none; }
    tbody tr:hover { background: #f7fafc; }

    tbody td {
      padding: 13px 16px;
      font-size: 0.88rem;
      color: #2d3748;
      vertical-align: middle;
    }
    tbody td.num { text-align: center; }

    /* rank badge */
    .rank-badge {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 32px;
      height: 32px;
      border-radius: 50%;
      font-weight: 700;
      font-size: 0.85rem;
    }
    .rank-1  { background: #fef3c7; color: #92400e; border: 2px solid #f6c90e; }
    .rank-2  { background: #e2e8f0; color: #2d3748; border: 2px solid #a0aec0; }
    .rank-3  { background: #fde8d8; color: #744210; border: 2px solid #e07b39; }
    .rank-n  { background: #edf2f7; color: #4a5568; border: 2px solid #cbd5e0; }

    /* score bar */
    .score-bar-wrap { display: flex; align-items: center; gap: 10px; }
    .score-bar-bg {
      flex: 1;
      background: #e2e8f0;
      border-radius: 99px;
      height: 8px;
      overflow: hidden;
      min-width: 60px;
    }
    .score-bar-fill {
      height: 100%;
      border-radius: 99px;
      background: linear-gradient(90deg, #3182ce, #63b3ed);
    }
    .score-bar-fill.top1 { background: linear-gradient(90deg, #d69e2e, #f6e05e); }
    .score-bar-fill.top2 { background: linear-gradient(90deg, #718096, #a0aec0); }
    .score-bar-fill.top3 { background: linear-gradient(90deg, #c05621, #ed8936); }
    .score-val { font-weight: 600; min-width: 42px; text-align: right; font-size: 0.85rem; }

    /* no result */
    .no-result {
      text-align: center;
      padding: 48px 20px;
      color: #a0aec0;
      font-size: 0.95rem;
    }
    .no-result span { font-size: 2rem; display: block; margin-bottom: 10px; }

    /* ── FOOTER ── */
    footer {
      text-align: center;
      font-size: 0.76rem;
      color: #a0aec0;
      padding: 20px;
      margin-top: 8px;
    }

    @media (max-width: 600px) {
      .controls { flex-direction: column; }
      input[type="text"] { width: 100%; }
      header h1 { font-size: 1.2rem; }
    }
  </style>
</head>
<body>

<header>
  <h1>🏆 Ranking Produk Bengkel</h1>
  <p>Bengkel AA Garage &nbsp;|&nbsp; Berdasarkan Distance Score (nilai terkecil = paling laris)</p>
</header>

<div class="container">

  <!-- CONTROLS -->
  <div class="controls">
    <div class="ctrl-group">
      <label>Filter Kriteria</label>
      <select id="criteriaSelect">
        <option value="C1">C1 – Harga</option>
        <option value="C2">C2 – Kualitas</option>
        <option value="C3">C3 – Fitur</option>
        <option value="C4">C4 – Daya Tahan</option>
        <option value="C5">C5 – Populer</option>
        <option value="ALL">Semua Kriteria (rata-rata)</option>
      </select>
    </div>
    <div class="ctrl-group">
      <label>Cari Produk</label>
      <input type="text" id="searchInput" placeholder="Ketik nama produk…" />
    </div>
    <div class="ctrl-group">
      <label>&nbsp;</label>
      <button class="btn-reset" onclick="resetAll()">⟳ Reset</button>
    </div>
  </div>

  <!-- QUICK PILLS -->
  <div class="criteria-pills" id="pills"></div>

  <!-- SUMMARY CARDS -->
  <div class="summary" id="summaryCards"></div>

  <!-- TABLE -->
  <div class="table-wrap">
    <div class="table-header-bar">
      <h2 id="tableTitle">Ranking Produk</h2>
      <span class="result-count" id="resultCount"></span>
    </div>
    <table>
      <thead>
        <tr>
          <th class="num">Rank</th>
          <th>Nama Produk</th>
          <th>Skor</th>
          <th class="num">Kategori</th>
        </tr>
      </thead>
      <tbody id="tableBody"></tbody>
    </table>
    <div class="no-result" id="noResult" style="display:none">
      <span>🔍</span>Produk tidak ditemukan. Coba kata kunci lain.
    </div>
  </div>

</div>

<footer>Data bersumber dari file khairul.xlsx &nbsp;|&nbsp; Metode: Distance Score (nilai lebih kecil = lebih unggul)</footer>

<script>
  // ── RAW DATA ─────────────────────────────────────────────────────────────
  const rawData = [
    { name: "Filter Oli NGK",          C1:11.899, C2:10.924, C3: 9.361, C4: 8.986, C5: 6.156 },
    { name: "Busi Denso",              C1:15.077, C2:13.567, C3:14.049, C4:13.868, C5:12.547 },
    { name: "Kampas Rem Bendix",       C1: 4.769, C2: 4.960, C3: 6.563, C4: 5.280, C5: 4.234 },
    { name: "Aki Yuasa",               C1: 8.732, C2: 8.582, C3:11.019, C4: 7.582, C5: 8.732 },
    { name: "V-Belt Gates",            C1:15.874, C2:15.921, C3:14.049, C4:15.921, C5:15.921 },
    { name: "Rantai DID",              C1: 3.228, C2: 3.000, C3: 4.769, C4: 3.574, C5: 2.759 },
    { name: "Piston Kit Federal",      C1: 6.353, C2: 6.156, C3: 7.822, C4: 6.596, C5: 7.512 },
    { name: "Shock Absorber KYB",      C1:10.292, C2:10.924, C3: 9.361, C4:11.492, C5:11.173 },
    { name: "Kopling TDR",             C1:13.488, C2:12.547, C3:12.636, C4:12.739, C5:13.567 },
    { name: "Bearing SKF",             C1: 1.000, C2: 2.410, C3: 3.979, C4: 2.410, C5: 2.410 },
    { name: "Seal Kit AHM",            C1: 7.138, C2: 7.512, C3: 5.534, C4: 8.986, C5: 8.732 },
    { name: "Karburator Keihin",       C1:12.693, C2:15.138, C3:12.636, C4:13.868, C5:15.138 },
    { name: "CDI Rextor",              C1: 1.765, C2: 3.574, C3: 4.236, C4: 2.759, C5: 3.000 },
    { name: "Knalpot FMF",             C1: 9.521, C2: 9.596, C3: 9.361, C4:10.331, C5:10.331 },
    { name: "Air Filter K&N",          C1:14.283, C2:14.330, C3:14.049, C4:15.138, C5:14.330 },
    { name: "Radiator Denso",          C1: 3.979, C2: 4.538, C3: 4.769, C4: 4.234, C5: 4.960 },
    { name: "Timing Belt Continental", C1: 5.561, C2: 6.156, C3: 6.563, C4: 5.280, C5: 6.156 },
    { name: "Kampas Kopling Indopart", C1:11.096, C2:12.547, C3:11.019, C4:11.492, C5:12.547 },
    { name: "Throttle Body Mikuni",    C1: 2.410, C2: 2.759, C3: 4.051, C4: 2.759, C5: 3.574 },
    { name: "Roller Variator Kawahara",C1: 7.940, C2: 8.582, C3: 7.822, C4: 7.582, C5: 8.732 },
  ];

  const criteriaInfo = {
    C1:  { label: "C1 – Harga",       bobot: 0.25 },
    C2:  { label: "C2 – Kualitas",    bobot: 0.20 },
    C3:  { label: "C3 – Fitur",       bobot: 0.15 },
    C4:  { label: "C4 – Daya Tahan",  bobot: 0.20 },
    C5:  { label: "C5 – Populer",   bobot: 0.20 },
    ALL: { label: "Semua Kriteria",   bobot: null  },
  };

  let activeCriteria = "C1";
  let searchTerm = "";

  // ── INIT ──────────────────────────────────────────────────────────────────
  function init() {
    buildPills();
    document.getElementById("criteriaSelect").addEventListener("change", e => {
      activeCriteria = e.target.value;
      syncPills();
      render();
    });
    document.getElementById("searchInput").addEventListener("input", e => {
      searchTerm = e.target.value.toLowerCase().trim();
      render();
    });
    render();
  }

  // ── PILLS ─────────────────────────────────────────────────────────────────
  function buildPills() {
    const wrap = document.getElementById("pills");
    const keys = ["C1","C2","C3","C4","C5","ALL"];
    keys.forEach(k => {
      const p = document.createElement("span");
      p.className = "pill" + (k === activeCriteria ? " active" : "");
      p.textContent = criteriaInfo[k].label;
      p.onclick = () => {
        activeCriteria = k;
        document.getElementById("criteriaSelect").value = k;
        syncPills();
        render();
      };
      p.dataset.key = k;
      wrap.appendChild(p);
    });
  }

  function syncPills() {
    document.querySelectorAll(".pill").forEach(p => {
      p.classList.toggle("active", p.dataset.key === activeCriteria);
    });
  }

  // ── SCORE COMPUTATION ─────────────────────────────────────────────────────
  function getScore(item, criteria) {
    if (criteria === "ALL") {
      return (item.C1*0.25 + item.C2*0.20 + item.C3*0.15 + item.C4*0.20 + item.C5*0.20);
    }
    return item[criteria];
  }

  // ── RENDER ────────────────────────────────────────────────────────────────
  function render() {
    // Sort all data ascending (smallest score = rank 1 = most popular)
    const sorted = [...rawData].sort((a,b) => getScore(a, activeCriteria) - getScore(b, activeCriteria));

    // Assign global rank
    sorted.forEach((item, i) => item._rank = i + 1);

    // Filter by search
    const filtered = searchTerm
      ? sorted.filter(item => item.name.toLowerCase().includes(searchTerm))
      : sorted;

    // Summary cards (from full sorted, not filtered)
    renderCards(sorted);

    // Title
    document.getElementById("tableTitle").textContent =
      `Ranking berdasarkan ${criteriaInfo[activeCriteria].label}`;
    document.getElementById("resultCount").textContent =
      `Menampilkan ${filtered.length} dari ${sorted.length} produk`;

    // Table
    renderTable(filtered, sorted);
  }

  function renderCards(sorted) {
    const wrap = document.getElementById("summaryCards");
    wrap.innerHTML = "";
    const tops = sorted.slice(0,3);
    const classes = ["gold","silver","bronze"];
    const medals  = ["🥇","🥈","🥉"];
    tops.forEach((item,i) => {
      const c = document.createElement("div");
      c.className = `card ${classes[i]}`;
      c.innerHTML = `
        <div class="card-label">${medals[i]} Peringkat ${i+1}</div>
        <div class="card-value">${item.name.length > 20 ? item.name.substring(0,18)+"…" : item.name}</div>
        <div class="card-sub">Skor: ${getScore(item, activeCriteria).toFixed(3)}</div>`;
      wrap.appendChild(c);
    });
    // Total products card
    const tc = document.createElement("div");
    tc.className = "card";
    tc.innerHTML = `<div class="card-label">Total Produk</div>
      <div class="card-value">${sorted.length}</div>
      <div class="card-sub">Kriteria: ${criteriaInfo[activeCriteria].label}</div>`;
    wrap.appendChild(tc);
  }

  function renderTable(filtered, allSorted) {
    const tbody  = document.getElementById("tableBody");
    const noRes  = document.getElementById("noResult");
    tbody.innerHTML = "";

    if (filtered.length === 0) {
      noRes.style.display = "block";
      return;
    }
    noRes.style.display = "none";

    // Max score in filtered set for bar scaling
    const maxScore = Math.max(...allSorted.map(i => getScore(i, activeCriteria)));
    const minScore = Math.min(...allSorted.map(i => getScore(i, activeCriteria)));

    filtered.forEach(item => {
      const score = getScore(item, activeCriteria);
      const rank  = item._rank;
      // Bar: invert → lower score = wider bar (more popular)
      const barPct = ((maxScore - score) / (maxScore - minScore) * 100).toFixed(1);

      const rankClass = rank === 1 ? "rank-1" : rank === 2 ? "rank-2" : rank === 3 ? "rank-3" : "rank-n";
      const barClass  = rank === 1 ? "top1" : rank === 2 ? "top2" : rank === 3 ? "top3" : "";

      // Highlight search match
      let displayName = item.name;
      if (searchTerm) {
        const re = new RegExp(`(${escapeRe(searchTerm)})`, "gi");
        displayName = item.name.replace(re, `<mark style="background:#fefcbf;border-radius:2px;padding:0 2px">$1</mark>`);
      }

      // Category tag based on score range
      const cat = rank <= 5 ? "🔥 Terlaris" : rank <= 10 ? "✅ Populer" : rank <= 15 ? "📦 Standar" : "⬇️ Rendah";
      const catColor = rank <= 5 ? "#276749" : rank <= 10 ? "#2b6cb0" : rank <= 15 ? "#744210" : "#742a2a";

      const tr = document.createElement("tr");
      tr.innerHTML = `
        <td class="num"><span class="rank-badge ${rankClass}">${rank}</span></td>
        <td>${displayName}</td>
        <td>
          <div class="score-bar-wrap">
            <div class="score-bar-bg">
              <div class="score-bar-fill ${barClass}" style="width:${barPct}%"></div>
            </div>
            <span class="score-val">${score.toFixed(3)}</span>
          </div>
        </td>
        <td class="num"><span style="font-size:0.78rem;font-weight:600;color:${catColor}">${cat}</span></td>`;
      tbody.appendChild(tr);
    });
  }

  function resetAll() {
    activeCriteria = "C1";
    searchTerm = "";
    document.getElementById("criteriaSelect").value = "C1";
    document.getElementById("searchInput").value = "";
    syncPills();
    render();
  }

  function escapeRe(str) { return str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); }

  init();
</script>
</body>
</html>
