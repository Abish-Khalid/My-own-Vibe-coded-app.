# My-own-Vibe-coded-app.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WiseTrack</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <style>
    /* ---------- MINIMAL GREY / BLACK ---------- */
    :root {
      --bg: #111111;
      --surface: #1a1a1a;
      --surface-2: #222222;
      --border: #2e2e2e;
      --text: #e6e6e6;
      --text-dim: #8a8a8a;
      --text-faint: #555555;
      --accent: #b0b0b0;
      --radius: 12px;
      --shadow: 0 4px 12px rgba(0,0,0,0.5);
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }

    body {
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    /* ---------- HEADER ---------- */
    .header {
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      padding: 14px 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 50;
    }

    .logo {
      font-size: 20px;
      font-weight: 600;
      letter-spacing: -0.3px;
      color: #fff;
    }

    .hamburger {
      background: transparent;
      border: 1px solid var(--border);
      border-radius: 8px;
      width: 42px;
      height: 42px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 5px;
      cursor: pointer;
      transition: 0.15s;
    }

    .hamburger span {
      width: 22px;
      height: 1.5px;
      background: var(--text);
      border-radius: 2px;
      transition: 0.15s;
    }

    .hamburger:hover {
      background: var(--surface-2);
    }

    /* ---------- SIDE DRAWER ---------- */
    .drawer {
      position: fixed;
      top: 0;
      left: -320px;
      width: 300px;
      height: 100vh;
      background: var(--surface);
      border-right: 1px solid var(--border);
      box-shadow: 4px 0 20px rgba(0,0,0,0.7);
      transition: left 0.25s cubic-bezier(0.2, 0.9, 0.3, 1);
      z-index: 55;
      padding: 24px 18px;
      display: flex;
      flex-direction: column;
    }

    .drawer.open {
      left: 0;
    }

    .drawer h3 {
      font-size: 15px;
      font-weight: 500;
      margin-bottom: 20px;
      color: var(--text-dim);
      letter-spacing: 0.2px;
      text-transform: uppercase;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .drawer .class-list {
      flex: 1;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .drawer .class-pill {
      background: var(--surface-2);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px 14px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      transition: 0.15s;
      font-size: 14px;
    }

    .drawer .class-pill:hover {
      background: #2a2a2a;
      border-color: #3a3a3a;
    }

    .drawer .class-pill.active {
      background: #2a2a2a;
      border-color: #4a4a4a;
    }

    .drawer .class-pill .del {
      color: var(--text-faint);
      font-size: 16px;
      line-height: 1;
      padding: 0 4px;
      border-radius: 20px;
    }

    .drawer .class-pill .del:hover {
      color: #ff8a8a;
    }

    .drawer-footer {
      margin-top: 20px;
      border-top: 1px solid var(--border);
      padding-top: 18px;
    }

    .overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.6);
      backdrop-filter: blur(2px);
      z-index: 54;
      opacity: 0;
      pointer-events: none;
      transition: 0.2s;
    }

    .overlay.show {
      opacity: 1;
      pointer-events: auto;
    }

    /* ---------- MAIN ---------- */
    .container {
      max-width: 1300px;
      margin: 0 auto;
      padding: 24px 20px 60px;
      width: 100%;
    }

    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 22px;
    }

    @media (max-width: 800px) {
      .grid {
        grid-template-columns: 1fr;
      }
    }

    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 20px;
    }

    .card h2 {
      font-size: 15px;
      font-weight: 500;
      letter-spacing: -0.2px;
      margin-bottom: 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      color: #fff;
      text-transform: uppercase;
    }

    .flex-between {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .mb-16 { margin-bottom: 16px; }
    .mt-16 { margin-top: 16px; }

    /* ---------- BUTTONS ---------- */
    .btn {
      padding: 7px 14px;
      border-radius: 8px;
      border: 1px solid var(--border);
      background: var(--surface-2);
      color: var(--text);
      cursor: pointer;
      font-size: 12px;
      font-weight: 500;
      transition: 0.15s;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      letter-spacing: 0.2px;
    }

    .btn:hover {
      background: #2e2e2e;
      border-color: #4a4a4a;
    }

    .btn-primary {
      background: #fff;
      color: #111;
      border-color: #fff;
      font-weight: 600;
    }

    .btn-primary:hover {
      background: #d0d0d0;
      border-color: #d0d0d0;
    }

    .btn-ghost {
      background: transparent;
      border-color: var(--border);
      color: var(--text-dim);
    }

    .btn-ghost:hover {
      background: var(--surface-2);
      color: var(--text);
    }

    .btn-sm {
      padding: 4px 10px;
      font-size: 11px;
    }

    /* ---------- FORMS ---------- */
    .form-group {
      margin-bottom: 14px;
    }

    .form-group label {
      display: block;
      font-size: 11px;
      color: var(--text-dim);
      margin-bottom: 6px;
      font-weight: 500;
      letter-spacing: 0.3px;
      text-transform: uppercase;
    }

    input, select {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 13px;
      background: var(--surface-2);
      color: var(--text);
    }

    input:focus, select:focus {
      outline: 1px solid #5a5a5a;
      border-color: #5a5a5a;
    }

    input[type="color"] {
      padding: 3px;
      height: 42px;
      cursor: pointer;
    }

    /* ---------- SUBJECT LIST ---------- */
    .subject-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 12px 14px;
      border: 1px solid var(--border);
      border-radius: 10px;
      margin-bottom: 6px;
      cursor: pointer;
      transition: 0.15s;
      background: var(--surface-2);
    }

    .subject-item:hover {
      border-color: #4a4a4a;
      background: #2a2a2a;
    }

    .subject-item.active {
      border-color: #6a6a6a;
      background: #2a2a2a;
    }

    .subject-info {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .subject-color {
      width: 10px;
      height: 10px;
      border-radius: 20px;
    }

    .subject-name {
      font-weight: 500;
      font-size: 14px;
    }

    .subject-meta {
      font-size: 11px;
      color: var(--text-dim);
      margin-top: 2px;
    }

    .tag {
      display: inline-block;
      padding: 3px 8px;
      border-radius: 6px;
      font-size: 11px;
      font-weight: 600;
      background: #2a2a2a;
      color: var(--text-dim);
      letter-spacing: 0.2px;
    }

    .tag-done {
      background: #2a2a2a;
      color: #b0b0b0;
    }

    /* ---------- TABLE ---------- */
    .table-wrap {
      overflow-x: auto;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      table-layout: fixed;
    }

    th, td {
      padding: 10px 8px;
      text-align: left;
      border-bottom: 1px solid var(--border);
      font-size: 13px;
      vertical-align: middle;
    }

    th {
      font-weight: 500;
      color: var(--text-faint);
      font-size: 10px;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      white-space: nowrap;
    }

    tr:hover td {
      background: #1f1f1f;
    }

    .check-cell {
      text-align: center;
    }

    .check-cell input[type="checkbox"] {
      width: 16px;
      height: 16px;
      cursor: pointer;
      accent-color: #a0a0a0;
      filter: grayscale(1) brightness(1.3);
    }

    /* ---------- CHART ---------- */
    .chart-container {
      position: relative;
      height: 220px;
    }

    .chart-stats {
      display: flex;
      justify-content: space-around;
      margin-top: 14px;
      text-align: center;
    }

    .stat-num {
      font-size: 22px;
      font-weight: 600;
      letter-spacing: -0.5px;
    }

    .stat-label {
      font-size: 10px;
      color: var(--text-faint);
      text-transform: uppercase;
      letter-spacing: 0.4px;
    }

    .aura-legend {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px 14px;
      margin-top: 16px;
      font-size: 10px;
      color: var(--text-dim);
    }

    .aura-dot {
      display: inline-block;
      width: 8px;
      height: 8px;
      border-radius: 20px;
      margin-right: 5px;
    }

    /* ---------- MODAL ---------- */
    .modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.7);
      backdrop-filter: blur(4px);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 200;
    }

    .modal-overlay.show {
      display: flex;
    }

    .modal {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 24px;
      width: 90%;
      max-width: 420px;
      box-shadow: var(--shadow);
    }

    .modal h3 {
      margin-bottom: 18px;
      font-size: 17px;
      font-weight: 500;
      letter-spacing: -0.2px;
    }

    .modal-actions {
      display: flex;
      justify-content: flex-end;
      gap: 8px;
      margin-top: 20px;
    }

    .empty {
      text-align: center;
      color: var(--text-faint);
      padding: 30px 16px;
      font-size: 13px;
    }

    .muted {
      color: var(--text-dim);
    }

    .hidden { display: none; }
  </style>
</head>
<body>

<!-- ========== HEADER ========== -->
<div class="header">
  <div class="hamburger" id="hamburgerBtn">
    <span></span><span></span><span></span>
  </div>
  <div class="logo">WiseTrack</div>
</div>

<!-- ========== SIDE DRAWER ========== -->
<div class="drawer" id="drawer">
  <h3>
    your classes
    <span style="font-size:16px; cursor:pointer; color: var(--text-dim);" id="closeDrawerBtn">✕</span>
  </h3>
  <div class="class-list" id="classListDrawer"></div>
  <div class="drawer-footer">
    <button class="btn btn-primary" style="width:100%; justify-content:center;" onclick="openModal('classModal')">
      + new class
    </button>
  </div>
</div>
<div class="overlay" id="overlay"></div>

<!-- ========== MAIN ========== -->
<div class="container">

  <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 18px;">
    <div style="font-size: 13px; color: var(--text-dim);" id="greeting">ready to track</div>
    <div style="display: flex; gap: 8px;">
      <button class="btn btn-ghost btn-sm" onclick="openModal('subjectModal')">+ subject</button>
    </div>
  </div>

  <div class="grid">
    <!-- Left: Subjects -->
    <div class="card">
      <div class="flex-between mb-16">
        <h2>subjects <span class="muted" style="font-size:12px; font-weight:400; text-transform:none;" id="subjectCount"></span></h2>
        <button class="btn btn-ghost btn-sm" onclick="openModal('subjectModal')">+ add</button>
      </div>
      <div id="subjectList"></div>
    </div>

    <!-- Right: Overall progress -->
    <div class="card">
      <h2>progress</h2>
      <div class="chart-container"><canvas id="overallChart"></canvas></div>
      <div class="chart-stats">
        <div><div class="stat-num" id="overallDone">0%</div><div class="stat-label">done</div></div>
        <div><div class="stat-num" id="overallTotal">0</div><div class="stat-label">chapters</div></div>
        <div><div class="stat-num" id="overallLeft">0</div><div class="stat-label">left</div></div>
      </div>
      <div class="aura-legend" id="auraLegend">
        <span><span class="aura-dot" style="background:#4a4a4a;"></span>0 rev</span>
        <span><span class="aura-dot" style="background:#8b5a2b;"></span>1 rev</span>
        <span><span class="aura-dot" style="background:#b0b0b0;"></span>2 rev</span>
        <span><span class="aura-dot" style="background:#d4af37;"></span>3 rev</span>
        <span><span class="aura-dot" style="background:#2e8b57;"></span>4 rev</span>
        <span><span class="aura-dot" style="background:#b22222;"></span>5+ rev</span>
      </div>
    </div>
  </div>

  <!-- Selected subject detail -->
  <div class="card mt-16" id="subjectDetail" style="display:none;">
    <div class="flex-between mb-16">
      <h2 id="detailTitle">Subject</h2>
      <div style="display: flex; gap: 6px;">
        <button class="btn btn-sm btn-ghost" onclick="openModal('columnModal')">+ column</button>
        <button class="btn btn-sm btn-ghost" onclick="openModal('chapterModal')">+ chapter</button>
        <button class="btn btn-sm btn-ghost" style="color:#ff8a8a; border-color:#4a2a2a;" onclick="deleteSubject()">delete</button>
      </div>
    </div>
    <div class="table-wrap">
      <table id="chapterTable">
        <thead><tr id="tableHead"></tr></thead>
        <tbody id="tableBody"></tbody>
      </table>
    </div>
  </div>
</div>

<!-- ========== MODALS ========== -->
<div class="modal-overlay" id="classModal">
  <div class="modal">
    <h3>new class</h3>
    <div class="form-group"><label>name</label><input id="className" placeholder="e.g. B.Tech CSE 3rd Year"></div>
    <div class="modal-actions">
      <button class="btn" onclick="closeModal('classModal')">cancel</button>
      <button class="btn btn-primary" onclick="addClass()">save</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="subjectModal">
  <div class="modal">
    <h3>new subject</h3>
    <div class="form-group"><label>subject name</label><input id="subjectName" placeholder="e.g. Data Structures"></div>
    <div class="form-group"><label>class</label><select id="subjectClass"></select></div>
    <div class="form-group"><label>color</label><input type="color" id="subjectColor" value="#6a6a6a"></div>
    <div class="modal-actions">
      <button class="btn" onclick="closeModal('subjectModal')">cancel</button>
      <button class="btn btn-primary" onclick="addSubject()">save</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="chapterModal">
  <div class="modal">
    <h3>new chapter</h3>
    <div class="form-group"><label>chapter name</label><input id="chapterName" placeholder="e.g. Arrays & Strings"></div>
    <div class="modal-actions">
      <button class="btn" onclick="closeModal('chapterModal')">cancel</button>
      <button class="btn btn-primary" onclick="addChapter()">save</button>
    </div>
  </div>
</div>

<!-- Column modal with revision toggle -->
<div class="modal-overlay" id="columnModal">
  <div class="modal">
    <h3>add column</h3>
    <div class="form-group">
      <label>column name</label>
      <input id="columnName" placeholder="e.g. Revised, Practiced, Notes">
    </div>
    <div class="form-group" style="display: flex; align-items: center; gap: 10px; margin-top: 4px;">
      <input type="checkbox" id="isRevisionCol" style="width: 16px; height: 16px; accent-color: #a0a0a0; filter: grayscale(1) brightness(1.3);">
      <label for="isRevisionCol" style="margin: 0; text-transform: none; font-size: 12px; color: var(--text-dim);">mark as revision column (affects aura)</label>
    </div>
    <div class="form-group">
      <label>applies to</label>
      <select id="columnScope">
        <option value="all">all chapters</option>
        <option value="specific">a specific chapter</option>
      </select>
    </div>
    <div class="form-group hidden" id="chapterSelectWrapper">
      <label>select chapter</label>
      <select id="columnChapterSelect"></select>
    </div>
    <div class="modal-actions">
      <button class="btn" onclick="closeModal('columnModal')">cancel</button>
      <button class="btn btn-primary" onclick="addColumn()">save</button>
    </div>
  </div>
</div>

<script>
  // ============ STATE ============
  let state = {
    classes: [],
    subjects: [],
    selectedSubject: null
  };

  // ---------- PERSISTENCE ----------
  function save() { localStorage.setItem('wiseTrack', JSON.stringify(state)); }
  function load() {
    const d = localStorage.getItem('wiseTrack');
    if (d) { try { state = JSON.parse(d); } catch(e) {} }
  }

  // ---------- HELPERS ----------
  const uid = () => Math.random().toString(36).substr(2, 9);
  const $ = (id) => document.getElementById(id);

  function openModal(id) { $(id).classList.add('show'); }
  function closeModal(id) { $(id).classList.remove('show'); }

  // ---------- DRAWER ----------
  const drawer = $('drawer');
  const overlay = $('overlay');
  const hamburger = $('hamburgerBtn');
  const closeDrawer = $('closeDrawerBtn');

  function toggleDrawer(open) {
    drawer.classList.toggle('open', open);
    overlay.classList.toggle('show', open);
  }

  hamburger.addEventListener('click', () => toggleDrawer(true));
  closeDrawer.addEventListener('click', () => toggleDrawer(false));
  overlay.addEventListener('click', () => toggleDrawer(false));

  // ---------- CLASSES ----------
  function addClass() {
    const name = $('className').value.trim();
    if (!name) return;
    state.classes.push({ id: uid(), name });
    $('className').value = '';
    closeModal('classModal');
    save(); render();
  }

  function renderClasses() {
    const el = $('classListDrawer');
    if (state.classes.length === 0) {
      el.innerHTML = '<div class="muted" style="padding:16px 0; text-align:center; font-size:13px;">no classes yet</div>';
      return;
    }
    el.innerHTML = state.classes.map(c => `
      <div class="class-pill" onclick="selectClass('${c.id}')">
        <span>${c.name}</span>
        <span class="del" onclick="event.stopPropagation(); deleteClass('${c.id}')">✕</span>
      </div>
    `).join('');
  }

  function selectClass(id) {
    $('subjectClass').value = id;
    toggleDrawer(false);
    // highlight active class in drawer
    document.querySelectorAll('.class-pill').forEach(el => el.classList.remove('active'));
    event.currentTarget.classList.add('active');
  }

  function deleteClass(id) {
    if (!confirm('delete this class and all its subjects?')) return;
    state.classes = state.classes.filter(c => c.id !== id);
    state.subjects = state.subjects.filter(s => s.classId !== id);
    if (state.selectedSubject && !state.subjects.find(s => s.id === state.selectedSubject)) state.selectedSubject = null;
    save(); render();
  }

  // ---------- SUBJECTS ----------
  function renderSubjectClassOptions() {
    $('subjectClass').innerHTML = state.classes.length
      ? state.classes.map(c => `<option value="${c.id}">${c.name}</option>`).join('')
      : '<option value="">create a class first</option>';
  }

  function addSubject() {
    const name = $('subjectName').value.trim();
    const classId = $('subjectClass').value;
    const color = $('subjectColor').value;
    if (!name || !classId) { alert('enter a name and select a class'); return; }
    state.subjects.push({
      id: uid(), name, classId, color,
      columns: ['Done', 'Revised'],
      chapters: []
    });
    $('subjectName').value = '';
    closeModal('subjectModal');
    save(); render();
  }

  // ---------- REVISION COUNT (detects any column with 'rev' in name, plus the dedicated flag) ----------
  function getRevisionCount(chapter) {
    let rev = 0;
    if (chapter.checks['Done'] !== true) return 0; // only count revisions if done
    Object.keys(chapter.checks).forEach(col => {
      if (col.toLowerCase().includes('rev') && chapter.checks[col] === true) rev++;
    });
    return rev;
  }

  function getAuraColor(revCount) {
    if (revCount === 0) return '#4a4a4a';
    if (revCount === 1) return '#8b5a2b';
    if (revCount === 2) return '#b0b0b0';
    if (revCount === 3) return '#d4af37';
    if (revCount === 4) return '#2e8b57';
    return '#b22222';
  }

  function renderSubjects() {
    const el = $('subjectList');
    $('subjectCount').textContent = state.subjects.length ? `(${state.subjects.length})` : '';
    if (state.subjects.length === 0) {
      el.innerHTML = '<div class="empty">no subjects yet</div>';
      return;
    }
    el.innerHTML = state.subjects.map(s => {
      const total = s.chapters.length;
      const done = s.chapters.filter(ch => ch.checks['Done']).length;
      const pct = total ? Math.round(done/total*100) : 0;
      const cls = state.classes.find(c => c.id === s.classId);
      return `
        <div class="subject-item ${state.selectedSubject === s.id ? 'active' : ''}" onclick="selectSubject('${s.id}')">
          <div class="subject-info">
            <div class="subject-color" style="background:${s.color}"></div>
            <div>
              <div class="subject-name">${s.name}</div>
              <div class="subject-meta">${cls ? cls.name : ''} · ${total} chapters · ${pct}% done</div>
            </div>
          </div>
          <div class="tag ${pct === 100 ? 'tag-done' : ''}">${pct}%</div>
        </div>
      `;
    }).join('');
  }

  function selectSubject(id) {
    state.selectedSubject = id;
    save(); render();
  }

  function deleteSubject() {
    if (!state.selectedSubject) return;
    if (!confirm('delete this subject and all its chapters?')) return;
    state.subjects = state.subjects.filter(s => s.id !== state.selectedSubject);
    state.selectedSubject = null;
    save(); render();
  }

  // ---------- CHAPTERS & COLUMNS ----------
  function getSelected() { return state.subjects.find(s => s.id === state.selectedSubject); }

  function addChapter() {
    const s = getSelected();
    if (!s) return;
    const name = $('chapterName').value.trim();
    if (!name) return;
    const checks = {};
    s.columns.forEach(c => checks[c] = false);
    s.chapters.push({ id: uid(), name, checks });
    $('chapterName').value = '';
    closeModal('chapterModal');
    save(); render();
  }

  function addColumn() {
    const s = getSelected();
    if (!s) { alert('select a subject first'); return; }
    const name = $('columnName').value.trim();
    if (!name) return;
    if (s.columns.includes(name)) { alert('column already exists'); return; }

    const isRevision = $('isRevisionCol').checked;
    const scope = $('columnScope').value;
    const chapterId = scope === 'specific' ? $('columnChapterSelect').value : null;

    // add column to subject
    s.columns.push(name);

    // initialize checks for all chapters
    s.chapters.forEach(ch => {
      if (scope === 'all' || ch.id === chapterId) {
        ch.checks[name] = false;
      } else {
        ch.checks[name] = undefined; // not applicable
      }
    });

    $('columnName').value = '';
    $('isRevisionCol').checked = false;
    $('columnScope').value = 'all';
    $('chapterSelectWrapper').classList.add('hidden');
    closeModal('columnModal');
    save(); render();
  }

  function toggleCheck(chapterId, colName) {
    const s = getSelected();
    if (!s) return;
    const ch = s.chapters.find(c => c.id === chapterId);
    if (ch) { ch.checks[colName] = !ch.checks[colName]; }
    save(); render();
  }

  function deleteChapter(chapterId) {
    const s = getSelected();
    if (!s) return;
    s.chapters = s.chapters.filter(c => c.id !== chapterId);
    save(); render();
  }

  function renderDetail() {
    const s = getSelected();
    const detail = $('subjectDetail');
    if (!s) { detail.style.display = 'none'; return; }
    detail.style.display = 'block';
    $('detailTitle').textContent = s.name;

    // build header
    let head = '<th style="width:36px;">#</th><th style="min-width:120px;">chapter</th>';
    s.columns.forEach(c => head += `<th class="check-cell" style="width:70px;">${c}</th>`);
    head += '<th style="width:40px;"></th>';
    $('tableHead').innerHTML = head;

    if (s.chapters.length === 0) {
      $('tableBody').innerHTML = '<tr><td colspan="' + (s.columns.length + 3) + '" class="empty">no chapters yet</td></tr>';
      return;
    }

    $('tableBody').innerHTML = s.chapters.map((ch, i) => {
      const revCount = getRevisionCount(ch);
      const aura = getAuraColor(revCount);

      let cells = `<td class="muted" style="font-size:12px;">${i+1}</td>`;
      cells += `<td style="font-size:13px; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;"><span style="display:inline-block; width:8px; height:8px; border-radius:20px; background:${aura}; margin-right:8px; vertical-align:middle;"></span>${ch.name}</td>`;

      s.columns.forEach(c => {
        const val = ch.checks[c];
        if (val === undefined) {
          // column not applicable to this chapter
          cells += `<td class="check-cell" style="color:var(--text-faint); font-size:12px;">—</td>`;
        } else {
          cells += `<td class="check-cell"><input type="checkbox" ${val ? 'checked' : ''} onchange="toggleCheck('${ch.id}','${c}')"></td>`;
        }
      });
      cells += `<td class="check-cell"><button class="btn btn-sm btn-ghost" style="color:#ff8a8a; padding:2px 6px; font-size:11px;" onclick="deleteChapter('${ch.id}')">✕</button></td>`;
      return `<tr>${cells}</tr>`;
    }).join('');
  }

  // ---------- CHART with AURA ----------
  let overallChart = null;

  function renderCharts() {
    let allChapters = [];
    state.subjects.forEach(s => {
      s.chapters.forEach(ch => {
        allChapters.push({ chapter: ch, subject: s });
      });
    });

    const totalChapters = allChapters.length;
    const doneChapters = allChapters.filter(({chapter}) => chapter.checks['Done']).length;
    const pct = totalChapters ? Math.round(doneChapters / totalChapters * 100) : 0;

    $('overallDone').textContent = pct + '%';
    $('overallTotal').textContent = totalChapters;
    $('overallLeft').textContent = totalChapters - doneChapters;

    const segments = allChapters.map(({chapter}) => {
      if (chapter.checks['Done'] !== true) {
        return { color: '#2c2c2c' };
      }
      const rev = getRevisionCount(chapter);
      return { color: getAuraColor(rev) };
    });

    if (segments.length === 0) {
      segments.push({ color: '#2c2c2c' });
    }

    const ctx = $('overallChart').getContext('2d');
    if (overallChart) overallChart.destroy();
    overallChart = new Chart(ctx, {
      type: 'doughnut',
      data: {
        labels: [],
        datasets: [{
          data: segments.map(() => 1),
          backgroundColor: segments.map(s => s.color),
          borderWidth: 0,
          hoverOffset: 4,
          borderRadius: 4,
        }]
      },
      options: {
        cutout: '72%',
        plugins: {
          legend: { display: false },
          tooltip: {
            callbacks: {
              label: (ctx) => {
                const idx = ctx.dataIndex;
                const item = allChapters[idx];
                if (!item) return 'no data';
                const rev = getRevisionCount(item.chapter);
                const status = item.chapter.checks['Done'] ? `done · ${rev} rev` : 'not done';
                return `${item.chapter.name}: ${status}`;
              }
            }
          }
        },
        responsive: true,
        maintainAspectRatio: false,
      }
    });

    // update greeting
    const greet = $('greeting');
    if (pct === 100) greet.textContent = 'syllabus cleared';
    else if (pct > 70) greet.textContent = 'almost there';
    else if (pct > 40) greet.textContent = 'steady progress';
    else greet.textContent = 'ready to track';
  }

  // ---------- RENDER ALL ----------
  function render() {
    renderClasses();
    renderSubjectClassOptions();
    renderSubjects();
    renderDetail();
    renderCharts();

    // populate chapter select for column modal
    const s = getSelected();
    if (s) {
      $('columnChapterSelect').innerHTML = s.chapters.map(ch => `<option value="${ch.id}">${ch.name}</option>`).join('');
    }

    // toggle chapter select visibility based on scope
    $('columnScope').addEventListener('change', function() {
      $('chapterSelectWrapper').classList.toggle('hidden', this.value !== 'specific');
    });
  }

  // ---------- INIT ----------
  load();
  if (!localStorage.getItem('wiseTrack')) {
    const demoClass = { id: uid(), name: 'B.Tech CSE 3rd Year' };
    const demoSubject = {
      id: uid(), name: 'Data Structures', classId: demoClass.id, color: '#6a6a6a',
      columns: ['Done', 'Revised', 'Revised2', 'Revised3'],
      chapters: [
        { id: uid(), name: 'Arrays & Strings', checks: { 'Done': true, 'Revised': true, 'Revised2': true, 'Revised3': false } },
        { id: uid(), name: 'Linked Lists', checks: { 'Done': true, 'Revised': true, 'Revised2': false, 'Revised3': false } },
        { id: uid(), name: 'Stacks & Queues', checks: { 'Done': false, 'Revised': false, 'Revised2': false, 'Revised3': false } },
        { id: uid(), name: 'Trees & Graphs', checks: { 'Done': true, 'Revised': true, 'Revised2': true, 'Revised3': true } }
      ]
    };
    state.classes = [demoClass];
    state.subjects = [demoSubject];
    save();
  }
  render();

  // close drawer on escape
  window.addEventListener('keydown', (e) => { if (e.key === 'Escape') toggleDrawer(false); });

  // expose functions globally
  window.addClass = addClass;
  window.selectClass = selectClass;
  window.deleteClass = deleteClass;
  window.addSubject = addSubject;
  window.selectSubject = selectSubject;
  window.deleteSubject = deleteSubject;
  window.addChapter = addChapter;
  window.addColumn = addColumn;
  window.toggleCheck = toggleCheck;
  window.deleteChapter = deleteChapter;
  window.openModal = openModal;
  window.closeModal = closeModal;
</script>
</body>
</html>
