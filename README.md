<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Leoo v3 Ultimate</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
    :root {
        --primary: #6C63FF; /* Roxo Moderno */
        --primary-hover: #5a52d5;
        --secondary: #FF6584; /* Rosa para destaque */
        --bg-body: #f0f2f5;
        --bg-sidebar: rgba(255, 255, 255, 0.85);
        --bg-card: #ffffff;
        --text-main: #2d3436;
        --text-muted: #636e72;
        --shadow: 0 10px 30px rgba(0,0,0,0.08);
        --glass-border: 1px solid rgba(255, 255, 255, 0.3);
        --radius: 16px;
        --sidebar-width: 260px;
    }

    body.dark-mode {
        --primary: #8c85ff;
        --bg-body: #121212;
        --bg-sidebar: rgba(30, 30, 30, 0.85);
        --bg-card: #1e1e1e;
        --text-main: #dfe6e9;
        --text-muted: #b2bec3;
        --shadow: 0 10px 30px rgba(0,0,0,0.3);
        --glass-border: 1px solid rgba(255, 255, 255, 0.05);
    }

    * { margin:0; padding:0; box-sizing:border-box; font-family: 'Poppins', sans-serif; outline: none; }

    body {
        background: var(--bg-body);
        color: var(--text-main);
        display: flex;
        height: 100vh;
        overflow: hidden;
        transition: background 0.4s ease;
    }

    /* --- Sidebar Glassmorphism --- */
    .sidebar {
        width: var(--sidebar-width);
        background: var(--bg-sidebar);
        backdrop-filter: blur(12px);
        -webkit-backdrop-filter: blur(12px);
        border-right: var(--glass-border);
        padding: 40px 25px;
        display: flex;
        flex-direction: column;
        z-index: 10;
        transition: 0.3s;
    }

    .brand { margin-bottom: 50px; display: flex; align-items: center; gap: 10px; }
    .brand i { font-size: 28px; color: var(--primary); }
    .brand h1 { font-size: 24px; font-weight: 700; letter-spacing: -0.5px; }
    
    .nav-btn {
        background: transparent;
        border: none;
        color: var(--text-muted);
        padding: 14px 20px;
        margin-bottom: 10px;
        text-align: left;
        border-radius: 12px;
        cursor: pointer;
        font-size: 15px;
        font-weight: 500;
        transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
        display: flex;
        align-items: center;
        gap: 12px;
    }

    .nav-btn:hover { background: rgba(108, 99, 255, 0.1); color: var(--primary); transform: translateX(5px); }
    .nav-btn.active {
        background: var(--primary);
        color: white;
        box-shadow: 0 5px 15px rgba(108, 99, 255, 0.3);
    }

    /* --- Main Area --- */
    .main { flex: 1; padding: 40px 50px; display: flex; flex-direction: column; overflow: hidden; position: relative; }
    
    /* Greeting Header */
    .header-section { display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px; }
    .greeting h2 { font-size: 32px; font-weight: 700; }
    .greeting p { color: var(--text-muted); font-size: 14px; margin-top: 5px; }

    .add-btn {
        background: var(--primary); color: white; border: none;
        padding: 12px 25px; border-radius: 50px; cursor: pointer;
        font-weight: 600; box-shadow: 0 4px 15px rgba(108, 99, 255, 0.4);
        transition: 0.3s; display: flex; align-items: center; gap: 8px;
    }
    .add-btn:hover { transform: translateY(-2px) scale(1.05); background: var(--primary-hover); }

    /* --- List Items --- */
    #content { flex: 1; overflow-y: auto; padding-right: 10px; padding-bottom: 20px; }
    
    /* Scrollbar Customization */
    #content::-webkit-scrollbar { width: 6px; }
    #content::-webkit-scrollbar-thumb { background: #ccc; border-radius: 10px; }
    body.dark-mode #content::-webkit-scrollbar-thumb { background: #444; }

    .item {
        background: var(--bg-card);
        padding: 20px;
        border-radius: var(--radius);
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 15px;
        box-shadow: var(--shadow);
        border: var(--glass-border);
        transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
        animation: slideIn 0.4s ease forwards;
        opacity: 0;
        transform: translateY(20px);
        position: relative;
        overflow: hidden;
    }

    .item:hover { transform: translateY(-3px) scale(1.01); box-shadow: 0 15px 35px rgba(0,0,0,0.1); }
    
    /* Priority Stripe */
    .item::before {
        content: ''; position: absolute; left: 0; top: 0; bottom: 0; width: 5px;
        background: #ccc;
    }
    .p-high::before { background: #ff4757; }
    .p-medium::before { background: #ffa502; }
    .p-low::before { background: #2ed573; }

    .item-info { display: flex; align-items: center; gap: 15px; }
    .check-circle {
        width: 24px; height: 24px; border: 2px solid var(--text-muted);
        border-radius: 50%; cursor: pointer; transition: 0.2s; position: relative;
    }
    .check-circle:hover { border-color: var(--primary); }
    .check-circle.checked { background: var(--primary); border-color: var(--primary); }
    .check-circle.checked::after {
        content: '\f00c'; font-family: 'Font Awesome 6 Free'; font-weight: 900;
        color: white; font-size: 12px; position: absolute; top: 50%; left: 50%;
        transform: translate(-50%, -50%);
    }

    .item-text h4 { font-size: 16px; margin-bottom: 2px; }
    .item-text span { font-size: 12px; color: var(--text-muted); display: flex; align-items: center; gap: 5px; }
    
    .actions { display: flex; gap: 10px; opacity: 0; transition: 0.2s; }
    .item:hover .actions { opacity: 1; }
    
    .icon-btn {
        background: transparent; border: none; color: var(--text-muted);
        cursor: pointer; transition: 0.2s; font-size: 16px;
    }
    .icon-btn:hover { color: var(--primary); transform: scale(1.2); }
    .icon-btn.delete:hover { color: #ff4757; }

    @keyframes slideIn { to { opacity: 1; transform: translateY(0); } }

    /* --- Stats & Settings --- */
    .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
    .chart-box { background: var(--bg-card); padding: 25px; border-radius: var(--radius); box-shadow: var(--shadow); height: 300px; }
    
    .settings-row {
        background: var(--bg-card); padding: 20px; border-radius: 12px;
        display: flex; justify-content: space-between; align-items: center;
        margin-bottom: 15px; box-shadow: 0 2px 10px rgba(0,0,0,0.02);
    }

    /* --- Modal --- */
    .modal-overlay {
        position: fixed; inset: 0; background: rgba(0,0,0,0.5); backdrop-filter: blur(5px);
        display: none; justify-content: center; align-items: center; z-index: 100; opacity: 0; transition: opacity 0.3s;
    }
    .modal-overlay.open { display: flex; opacity: 1; }
    
    .modal {
        background: var(--bg-card); padding: 30px; border-radius: 20px; width: 400px;
        box-shadow: 0 20px 50px rgba(0,0,0,0.2); transform: scale(0.8); transition: 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    }
    .modal-overlay.open .modal { transform: scale(1); }

    .modal h3 { margin-bottom: 20px; font-size: 22px; }
    .inp-group { margin-bottom: 15px; }
    .inp-group label { display: block; font-size: 12px; font-weight: 600; margin-bottom: 5px; color: var(--text-muted); }
    .inp-style {
        width: 100%; padding: 12px; border-radius: 8px; border: 1px solid #ddd;
        background: var(--bg-body); color: var(--text-main); transition: 0.2s;
    }
    .inp-style:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(108, 99, 255, 0.1); }

    .priority-select { display: flex; gap: 10px; margin-bottom: 20px; }
    .p-btn {
        flex: 1; padding: 8px; border: 1px solid #ddd; border-radius: 8px; background: transparent;
        cursor: pointer; font-size: 13px; color: var(--text-muted); transition: 0.2s;
    }
    .p-btn.selected { color: white; border-color: transparent; }
    .p-btn[data-val="high"].selected { background: #ff4757; }
    .p-btn[data-val="medium"].selected { background: #ffa502; }
    .p-btn[data-val="low"].selected { background: #2ed573; }

    .modal-footer { display: flex; justify-content: flex-end; gap: 10px; margin-top: 10px; }
    .btn-sec { background: #ddd; color: #333; padding: 10px 20px; border-radius: 8px; border: none; cursor: pointer; }
    
</style>
</head>

<body>

<div class="sidebar">
    <div class="brand">
        <i class="fa-solid fa-layer-group"></i>
        <h1>Leoo<span style="color:var(--primary)">.v3</span></h1>
    </div>
    
    <button class="nav-btn active" onclick="switchWorkspace('tasks')" id="btn-tasks">
        <i class="fa-solid fa-list-check"></i> <span data-i18n="tasks">Tarefas</span>
    </button>
    <button class="nav-btn" onclick="switchWorkspace('goals')" id="btn-goals">
        <i class="fa-solid fa-bullseye"></i> <span data-i18n="goals">Objetivos</span>
    </button>
    <button class="nav-btn" onclick="switchWorkspace('stats')" id="btn-stats">
        <i class="fa-solid fa-chart-pie"></i> <span data-i18n="stats">Estatísticas</span>
    </button>
    <div style="flex:1"></div>
    <button class="nav-btn" onclick="switchWorkspace('settings')" id="btn-settings">
        <i class="fa-solid fa-sliders"></i> <span data-i18n="settings">Ajustes</span>
    </button>
</div>

<div class="main">
    <div class="header-section">
        <div class="greeting">
            <h2 id="greeting-text">Olá!</h2>
            <p id="date-display">Carregando data...</p>
        </div>
        <button class="add-btn" id="newBtn">
            <i class="fa-solid fa-plus"></i> <span data-i18n="new">Novo Item</span>
        </button>
    </div>

    <div id="content">
        </div>
</div>

<div class="modal-overlay" id="modalOverlay">
    <div class="modal">
        <h3 id="modalTitle">Novo Item</h3>
        <input type="hidden" id="editIndex">
        
        <div class="inp-group">
            <label data-i18n="nameLabel">Nome</label>
            <input type="text" id="itemName" class="inp-style" placeholder="Ex: Estudar JavaScript">
        </div>
        
        <div class="inp-group" id="dateGroup">
            <label data-i18n="dateLabel">Data Limite</label>
            <input type="date" id="itemDate" class="inp-style">
        </div>

        <div class="inp-group">
            <label data-i18n="priorityLabel">Prioridade</label>
            <div class="priority-select">
                <button class="p-btn" data-val="low" onclick="selectPriority(this)">Baixa</button>
                <button class="p-btn selected" data-val="medium" onclick="selectPriority(this)">Média</button>
                <button class="p-btn" data-val="high" onclick="selectPriority(this)">Alta</button>
            </div>
        </div>

        <div class="modal-footer">
            <button class="btn-sec" onclick="closeModal()" data-i18n="cancel">Cancelar</button>
            <button class="add-btn" onclick="saveItem()" id="saveBtn" data-i18n="save">Salvar</button>
        </div>
    </div>
</div>

<script>
// --- CONFIG E DADOS ---
const i18n = {
    pt: {
        tasks: "Tarefas", goals: "Objetivos", stats: "Estatísticas", settings: "Ajustes",
        new: "Novo", save: "Salvar", cancel: "Cancelar",
        empty: "Tudo limpo por aqui!", overdue: "Atrasado", today: "Hoje", days: "dias",
        greetingMorning: "Bom dia", greetingAfternoon: "Boa tarde", greetingNight: "Boa noite",
        nameLabel: "O que precisa ser feito?", dateLabel: "Pra quando?", priorityLabel: "Nível de Urgência",
        deleteConfirm: "Tem certeza que deseja excluir?"
    },
    en: {
        tasks: "Tasks", goals: "Goals", stats: "Statistics", settings: "Settings",
        new: "New", save: "Save", cancel: "Cancel",
        empty: "Everything is clean!", overdue: "Overdue", today: "Today", days: "days",
        greetingMorning: "Good morning", greetingAfternoon: "Good afternoon", greetingNight: "Good evening",
        nameLabel: "What needs to be done?", dateLabel: "Due Date", priorityLabel: "Urgency Level",
        deleteConfirm: "Are you sure you want to delete?"
    }
};

let state = {
    workspace: 'tasks',
    lang: JSON.parse(localStorage.getItem("leoo_settings"))?.lang || 'pt',
    theme: JSON.parse(localStorage.getItem("leoo_settings"))?.theme || 'light',
    user: JSON.parse(localStorage.getItem("leoo_settings"))?.user || 'Leo',
    tasks: JSON.parse(localStorage.getItem("tasks")) || [],
    goals: JSON.parse(localStorage.getItem("goals")) || [],
    completed: JSON.parse(localStorage.getItem("completed")) || [], // Unified completed history
    currentPriority: 'medium'
};

// --- CORE ---
function init() {
    applyTheme();
    updateGreeting();
    switchWorkspace(state.workspace);
    setInterval(updateGreeting, 60000); // Atualiza saudação a cada minuto
}

function saveState() {
    localStorage.setItem("tasks", JSON.stringify(state.tasks));
    localStorage.setItem("goals", JSON.stringify(state.goals));
    localStorage.setItem("completed", JSON.stringify(state.completed));
    localStorage.setItem("leoo_settings", JSON.stringify({ theme: state.theme, lang: state.lang, user: state.user }));
}

function updateGreeting() {
    const now = new Date();
    const hr = now.getHours();
    const t = i18n[state.lang];
    let greet = t.greetingMorning;
    if(hr >= 12) greet = t.greetingAfternoon;
    if(hr >= 18) greet = t.greetingNight;
    
    document.getElementById("greeting-text").innerText = `${greet}, ${state.user}`;
    
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    document.getElementById("date-display").innerText = now.toLocaleDateString(state.lang === 'pt' ? 'pt-BR' : 'en-US', options);
}

// --- NAVEGAÇÃO ---
function switchWorkspace(type) {
    state.workspace = type;
    
    // Atualiza Menu
    document.querySelectorAll(".nav-btn").forEach(b => b.classList.remove("active"));
    const btnId = `btn-${type}`;
    if(document.getElementById(btnId)) document.getElementById(btnId).classList.add("active");

    // Botão novo some em stats/settings
    document.getElementById("newBtn").style.display = (type === 'stats' || type === 'settings') ? 'none' : 'flex';

    const content = document.getElementById("content");
    content.style.opacity = 0;
    
    setTimeout(() => {
        content.innerHTML = "";
        if(type === 'stats') renderStats();
        else if (type === 'settings') renderSettings();
        else renderList();
        content.style.opacity = 1;
    }, 200);
}

// --- RENDER LISTA ---
function renderList() {
    const list = state.workspace === 'tasks' ? state.tasks : state.goals;
    const content = document.getElementById("content");
    const t = i18n[state.lang];

    if (list.length === 0) {
        content.innerHTML = `
            <div style="text-align:center; margin-top:80px; color:var(--text-muted); opacity:0.6;">
                <i class="fa-solid fa-clipboard-check" style="font-size:48px; margin-bottom:15px;"></i>
                <h3>${t.empty}</h3>
            </div>`;
        return;
    }

    // Ordenar por data
    list.sort((a,b) => new Date(a.date) - new Date(b.date));

    list.forEach((item, index) => {
        const today = new Date(); today.setHours(0,0,0,0);
        const due = new Date(item.date + "T00:00:00");
        const diff = Math.ceil((due - today) / (1000 * 60 * 60 * 24));
        
        let dateText = diff < 0 ? `<span style="color:#ff4757">${t.overdue}</span>` : 
                       diff === 0 ? `<span style="color:#ffa502">${t.today}</span>` : 
                       diff === 1 ? t.tomorrow : `${diff} ${t.days}`;

        const el = document.createElement("div");
        el.className = `item p-${item.priority}`;
        el.style.animationDelay = `${index * 0.05}s`; // Efeito cascata
        el.innerHTML = `
            <div class="item-info">
                <div class="check-circle" onclick="completeItem(${index})"></div>
                <div class="item-text">
                    <h4>${item.name}</h4>
                    <span><i class="fa-regular fa-clock"></i> ${dateText}</span>
                </div>
            </div>
            <div class="actions">
                <button class="icon-btn" onclick="openModal(${index})"><i class="fa-solid fa-pen"></i></button>
                <button class="icon-btn delete" onclick="deleteItem(${index})"><i class="fa-solid fa-trash"></i></button>
            </div>
        `;
        content.appendChild(el);
    });
}

// --- AÇÕES ---
function completeItem(index) {
    const list = state.workspace === 'tasks' ? state.tasks : state.goals;
    const item = list[index];
    
    // Animação de saída
    const els = document.querySelectorAll(".item");
    if(els[index]) {
        els[index].querySelector(".check-circle").classList.add("checked");
        els[index].style.transform = "scale(0.95)";
        els[index].style.opacity = "0";
    }

    setTimeout(() => {
        state.completed.push({ ...item, type: state.workspace, completedDate: new Date() });
        list.splice(index, 1);
        saveState();
        renderList();
    }, 300);
}

function deleteItem(index) {
    if(confirm(i18n[state.lang].deleteConfirm)) {
        const list = state.workspace === 'tasks' ? state.tasks : state.goals;
        list.splice(index, 1);
        saveState();
        renderList();
    }
}

// --- MODAL & FORM ---
const modalOverlay = document.getElementById("modalOverlay");
const dateInput = document.getElementById("itemDate");
const nameInput = document.getElementById("itemName");
const editIndexInput = document.getElementById("editIndex");

document.getElementById("newBtn").onclick = () => openModal();

function openModal(index = null) {
    state.currentPriority = 'medium';
    updatePriorityUI();
    
    if (index !== null) {
        // Edit Mode
        const list = state.workspace === 'tasks' ? state.tasks : state.goals;
        const item = list[index];
        nameInput.value = item.name;
        dateInput.value = item.date;
        state.currentPriority = item.priority;
        editIndexInput.value = index;
        updatePriorityUI();
    } else {
        // New Mode
        nameInput.value = "";
        dateInput.value = new Date().toISOString().split('T')[0];
        editIndexInput.value = "";
    }
    
    modalOverlay.classList.add("open");
    nameInput.focus();
}

function closeModal() {
    modalOverlay.classList.remove("open");
}

function selectPriority(btn) {
    state.currentPriority = btn.dataset.val;
    updatePriorityUI();
}

function updatePriorityUI() {
    document.querySelectorAll(".p-btn").forEach(b => {
        b.classList.toggle("selected", b.dataset.val === state.currentPriority);
    });
}

function saveItem() {
    const name = nameInput.value;
    const date = dateInput.value;
    const idx = editIndexInput.value;
    
    if(!name) return;

    const newItem = { name, date, priority: state.currentPriority };
    const list = state.workspace === 'tasks' ? state.tasks : state.goals;

    if (idx !== "") {
        list[idx] = newItem;
    } else {
        list.push(newItem);
    }

    saveState();
    closeModal();
    renderList();
}

// --- ESTATÍSTICAS ---
function renderStats() {
    const content = document.getElementById("content");
    content.innerHTML = `
        <div class="stats-grid">
            <div class="chart-box"><canvas id="barChart"></canvas></div>
            <div class="chart-box"><canvas id="pieChart"></canvas></div>
        </div>
    `;

    // Processar dados
    const tasksDone = state.completed.filter(i => i.type === 'tasks').length;
    const goalsDone = state.completed.filter(i => i.type === 'goals').length;
    
    // Chart 1: Barras
    new Chart(document.getElementById("barChart"), {
        type: 'bar',
        data: {
            labels: ['Tarefas', 'Objetivos'],
            datasets: [{
                label: 'Concluídos',
                data: [tasksDone, goalsDone],
                backgroundColor: ['#6C63FF', '#FF6584'],
                borderRadius: 8
            }]
        },
        options: { responsive: true, maintainAspectRatio: false, plugins: { legend: {display: false} } }
    });

    // Chart 2: Prioridade (Distribuição)
    const pCounts = { high: 0, medium: 0, low: 0 };
    [...state.tasks, ...state.goals].forEach(i => pCounts[i.priority]++);

    new Chart(document.getElementById("pieChart"), {
        type: 'doughnut',
        data: {
            labels: ['Alta', 'Média', 'Baixa'],
            datasets: [{
                data: [pCounts.high, pCounts.medium, pCounts.low],
                backgroundColor: ['#ff4757', '#ffa502', '#2ed573'],
                borderWidth: 0
            }]
        },
        options: { responsive: true, maintainAspectRatio: false, cutout: '70%' }
    });
}

// --- SETTINGS ---
function renderSettings() {
    const content = document.getElementById("content");
    content.innerHTML = `
        <div class="settings-row">
            <div>
                <h4>Modo Escuro</h4>
                <span style="font-size:12px; opacity:0.7">Alterne entre temas claro e escuro</span>
            </div>
            <button onclick="toggleTheme()" class="btn-sec">${state.theme === 'light' ? '🌙' : '☀️'}</button>
        </div>
        <div class="settings-row">
            <div>
                <h4>Seu Nome</h4>
                <span style="font-size:12px; opacity:0.7">Para a saudação personalizada</span>
            </div>
            <input type="text" value="${state.user}" onchange="updateName(this.value)" class="inp-style" style="width:150px">
        </div>
        <div class="settings-row">
            <div>
                <h4>Idioma / Language</h4>
            </div>
            <div style="display:flex; gap:10px">
                <button class="btn-sec" onclick="setLang('pt')" ${state.lang==='pt'?'style="background:var(--primary);color:white"':''}>PT</button>
                <button class="btn-sec" onclick="setLang('en')" ${state.lang==='en'?'style="background:var(--primary);color:white"':''}>EN</button>
            </div>
        </div>
        <div class="settings-row" style="color:#ff4757; cursor:pointer" onclick="resetData()">
            <div>
                <h4>Resetar Dados</h4>
                <span style="font-size:12px; opacity:0.7">Apagar tudo e começar do zero</span>
            </div>
            <i class="fa-solid fa-triangle-exclamation"></i>
        </div>
    `;
}

function toggleTheme() {
    state.theme = state.theme === 'light' ? 'dark' : 'light';
    applyTheme(); saveState(); renderSettings();
}

function applyTheme() {
    document.body.className = state.theme === 'dark' ? 'dark-mode' : '';
}

function updateName(val) {
    state.user = val; saveState(); updateGreeting();
}

function setLang(l) {
    state.lang = l; saveState(); location.reload();
}

function resetData() {
    if(confirm("Resetar tudo? Essa ação não pode ser desfeita.")) {
        localStorage.clear(); location.reload();
    }
}

// Iniciar
init();

</script>
</body>
</html>
