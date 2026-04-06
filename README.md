<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v47.0 - ARCHITECTURE FINALE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        /* ==========================================================================
           CHARTE GRAPHIQUE ET VARIABLES (SANS SIMPLIFICATION)
           ========================================================================== */
        :root { 
            --primary: #0056b3; 
            --primary-dark: #003d7a;
            --success: #2ecc71; 
            --danger: #ef233c; 
            --warning: #ff9f1c; 
            --dark: #1b2631; 
            --light: #f4f7f9; 
            --white: #ffffff;
            --radius-lg: 20px;
            --radius-md: 12px;
            --shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        body { 
            font-family: 'Inter', sans-serif; 
            margin: 0; 
            padding: 0;
            background-color: var(--light); 
            color: var(--dark); 
            user-select: none;
            overflow-x: hidden;
            -webkit-tap-highlight-color: transparent;
        }

        .hidden { display: none !important; }

        /* ==========================================================================
           HEADER & BRANDING
           ========================================================================== */
        .header { 
            background: var(--white); 
            padding: 20px; 
            text-align: center; 
            border-bottom: 4px solid var(--primary); 
            position: sticky; 
            top: 0; 
            z-index: 1000;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }
        .logo-main { width: 85px; height: 85px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 24px; margin-top: 10px; text-transform: uppercase; letter-spacing: -1px; }
        .official-slogan { font-size: 13px; color: #555; font-style: italic; font-weight: 700; margin-top: 2px; }

        /* ==========================================================================
           NAVIGATION BASSE (7 ONGLETS VERROUILLÉS)
           ========================================================================== */
        .nav-bar { 
            display: grid; 
            grid-template-columns: repeat(7, 1fr); 
            gap: 2px; 
            padding: 12px; 
            background: var(--white); 
            position: fixed; 
            bottom: 0; 
            width: 100%; 
            border-top: 1px solid #ddd; 
            box-sizing: border-box; 
            z-index: 2000;
        }

        .nav-bar button { 
            background: #f1f4f8; 
            border: none; 
            color: #7f8c8d; 
            font-weight: 800; 
            font-size: 7px; 
            padding: 12px 1px; 
            border-radius: 12px; 
            cursor: pointer; 
            text-transform: uppercase; 
            transition: all 0.3s ease;
        }

        .nav-bar button.active { 
            background: var(--primary); 
            color: var(--white); 
            transform: translateY(-5px); 
            box-shadow: 0 5px 15px rgba(0, 86, 179, 0.4); 
        }

        /* ==========================================================================
           COMPOSANTS UI (CARTES ET FORMULAIRES)
           ========================================================================== */
        .card { 
            background: var(--white); 
            margin: 15px; 
            padding: 25px; 
            border-radius: var(--radius-lg); 
            box-shadow: 0 6px 20px rgba(0,0,0,0.06); 
            border: 1px solid #eee; 
        }

        h3 { 
            margin-top: 0; 
            font-size: 17px; 
            color: var(--primary); 
            font-weight: 900; 
            text-transform: uppercase; 
            border-bottom: 3px solid var(--light); 
            padding-bottom: 12px; 
            margin-bottom: 20px; 
        }
        
        input, select, textarea { 
            background: #f1f4f8; 
            border: 2px solid transparent; 
            padding: 16px; 
            border-radius: 15px; 
            margin-bottom: 12px; 
            width: 100%; 
            box-sizing: border-box; 
            font-size: 15px; 
            outline: none; 
            transition: 0.3s; 
            font-family: inherit; 
        }

        input:focus { border-color: var(--primary); background: var(--white); }
        
        .btn-main { 
            background: var(--primary); 
            color: var(--white); 
            border: none; 
            padding: 20px; 
            border-radius: 18px; 
            font-weight: 900; 
            width: 100%; 
            cursor: pointer; 
            text-transform: uppercase; 
            font-size: 14px; 
        }

        .btn-wa { 
            background: #25D366; 
            color: var(--white); 
            border: none; 
            padding: 18px; 
            border-radius: 15px; 
            font-weight: 800; 
            width: 100%; 
            cursor: pointer; 
            text-transform: uppercase; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            font-size: 13px;
        }

        /* ==========================================================================
           ÉTATS ET INDICATEURS
           ========================================================================== */
        .item-row { 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            padding: 15px; 
            background: #f8f9fa; 
            border-radius: 15px; 
            margin-bottom: 8px; 
            border-left: 6px solid var(--primary); 
        }

        .solde-badge { 
            background: var(--danger); 
            color: white; 
            padding: 6px 12px; 
            border-radius: 10px; 
            font-size: 12px; 
            font-weight: 900; 
            display: inline-block; 
        }

        .status-pill { padding: 5px 12px; border-radius: 50px; font-size: 10px; font-weight: 900; text-transform: uppercase; }
        .pill-active { background: #dcfce7; color: #166534; }
        .pill-blocked { background: #fee2e2; color: #991b1b; }

        .sync-indicator { position: fixed; top: 10px; right: 10px; font-size: 9px; padding: 5px 10px; border-radius: 10px; z-index: 3000; font-weight: bold; }
        .online { background: var(--success); color: white; }
        .offline { background: var(--danger); color: white; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="sync-ui" class="sync-indicator offline">SYNC : OFF</div>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 90%; max-width: 400px; text-align: center; padding: 50px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
            <h2>SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:800; margin-bottom:35px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Login">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">CONNEXION</button>
            <p id="login-msg" style="font-size:10px; color:red; margin-top:10px;"></p>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 10px 20px; font-size: 11px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 SESSION : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); border:none; color:white; padding:6px 12px; border-radius:8px; font-size:10px; font-weight:bold; cursor:pointer;">SORTIR</button>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:60px; height:60px; border-radius:50%; border:2px solid var(--primary);">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 130px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION CLIENTS</h3>
                <input type="tel" id="tel-client" placeholder="WhatsApp Client" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet">
                <input type="date" id="date-retrait">
                <div style="background:#f8f9fa; padding:20px; border-radius:18px; border:1px dashed #ddd; margin-top:10px;">
                    <select id="select-article" onchange="majPrixInitial()"></select>
                    <input type="number" id="prix-final" placeholder="Prix ajusté">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark);">+ AJOUTER</button>
                </div>
            </div>
            <div id="panier-liste" style="padding: 0 15px;"></div>
            <div class="card" style="background:var(--dark); color:white;">
                TOTAL : <b id="total-caisse" style="font-size:26px; color:var(--success);">0 F</b>
                <input type="number" id="paye-caisse" placeholder="Avance" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white; margin-top:10px;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning);">Reste : 0 F</div>
            </div>
            <div style="padding: 0 15px;"><button onclick="validerCommande()" class="btn-main" style="height:80px;">🖨️ VALIDER TICKET</button></div>
            
            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSE (ACHAT)</h3>
                <input type="text" id="achat-motif" placeholder="Motif de l'achat">
                <input type="number" id="achat-montant" placeholder="Montant">
                <button onclick="enregistrerAchat()" class="btn-main" style="background:var(--danger);">DÉDUIRE</button>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px;">👕 ZONE TECHNIQUE - PRODUCTION</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-rapport" class="tab-content hidden">
            <div class="card">
                <h3>📊 BILAN JOURNALIER</h3>
                <div id="rapport-details"></div>
                <button onclick="envoyerRapportWA()" class="btn-main" style="background:#25D366; margin-top:20px;">📲 ENVOYER AU PATRON</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE PERF</h3>
                <div id="perf-display"></div>
                <div class="item-row" style="background:var(--success); color:white; margin-top:20px;">
                    <span>NET GLOBAL</span> <b id="p-net">0 F</b>
                </div>
            </div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3>🧼 CONSOMMABLES</h3>
                <div id="stock-display"></div>
                <div id="admin-stock-control" class="hidden">
                    <hr>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Quantité (+/-)">
                    <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">AJUSTER</button>
                </div>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 AUDIT LOGS (TEMPS RÉEL)</h3>
                <div id="logs-container" style="max-height:400px; overflow-y:auto; font-size:10px; font-family:monospace;"></div>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card" style="background: #fff3cd;">
                <h3>⚙️ CONFIGURATION CLOUD</h3>
                <input type="text" id="fb-url" placeholder="https://votre-firebase-url.com">
                <button onclick="saveFBConfig()" class="btn-main" style="background:var(--dark);">ACTIVER LA SYNCHRO</button>
            </div>

            <div class="card" style="border: 3px solid var(--primary);">
                <h3>👑 PERSONNEL & CONTRÔLE D'ACCÈS</h3>
                <div id="staff-list"></div>
                <hr>
                <h4>CRÉER LOGIN EMPLOYÉ</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Identifiant">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Zone Technique">Zone Technique</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER L'ACCÈS</button>
            </div>

            <div class="card">
                <h3>✨ GESTION DYNAMIQUE DE LA GRILLE</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie (ex: ROBES)">
                <input type="text" id="new-art-nom" placeholder="Nom de l'article">
                <input type="number" id="new-art-prix" placeholder="Prix standard">
                <button onclick="ajouterNouveauProduit()" class="btn-main">ACTUALISER LA LISTE</button>
            </div>
            
            <div class="card"><button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 BACKUP MANUEL (.JSON)</button></div>
        </div>
    </div>

    <nav class="nav-bar" id="main-nav">
        <button onclick="tab('section-caisse')" id="btn-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-atelier">👕 Z.T</button>
        <button onclick="tab('section-rapport')" id="btn-rapport">📊 RAPPORT</button>
        <button onclick="tab('section-performance')" id="btn-performance">📈 PERF</button>
        <button onclick="tab('section-stocks')" id="btn-stocks">🧼 STOCKS</button>
        <button onclick="tab('section-logs')" id="btn-logs">📜 LOGS</button>
        <button onclick="tab('section-admin')" id="btn-admin">👑 ADMIN</button>
    </nav>
</div>

<script>
/* ==========================================================================
   MOTEUR DE SYNCHRONISATION HYBRIDE (CLOUD FIREBASE)
   ========================================================================== */
let CLOUD_URL = localStorage.getItem('sc_fb_url') || "";
let GRILLE = JSON.parse(localStorage.getItem('sc_local_grille')) || {
    "HAUTS": {"T-Shirt":500,"Chemise":500,"Veste":1500},
    "ENSEMBLES": {"Costume luxe":2500,"Boubou":1500},
    "ROBES": {"Robe simple":1000,"Toge":3500}
};

let db = [], staff = [], stocks = [], logs = [], achats = [];
const local_staff_fallback = [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN", actif:true}];

async function syncPush(path, data) {
    if(!CLOUD_URL) return;
    try {
        await fetch(`${CLOUD_URL}/${path}.json`, { method: 'PUT', body: JSON.stringify(data) });
        updateSyncStatus(true);
    } catch(e) { updateSyncStatus(false); }
}

async function syncPull() {
    if(!CLOUD_URL) {
        staff = JSON.parse(localStorage.getItem('sc_local_staff')) || local_staff_fallback;
        db = JSON.parse(localStorage.getItem('sc_local_db')) || [];
        stocks = JSON.parse(localStorage.getItem('sc_local_stocks')) || [{id:'savon', nom:'Savon', qte:100, unite:'L'}];
        updateSyncStatus(false);
        return;
    }
    try {
        const res = await fetch(`${CLOUD_URL}/.json`);
        const data = await res.json();
        if(data) {
            db = data.db || [];
            staff = data.staff || local_staff_fallback;
            stocks = data.stocks || [];
            logs = data.logs || [];
            achats = data.achats || [];
            if(data.GRILLE) GRILLE = data.GRILLE;
            updateSyncStatus(true);
        }
    } catch(e) { updateSyncStatus(false); }
}

function updateSyncStatus(isOnline) {
    const el = document.getElementById('sync-ui');
    el.className = isOnline ? "sync-indicator online" : "sync-indicator offline";
    el.innerText = isOnline ? "☁️ CLOUD ACTIF" : "⚡ MODE LOCAL";
}

/* ==========================================================================
   AUTHENTIFICATION ET SÉCURITÉ ROLES EXCLUSIFS
   ========================================================================== */
async function login() {
    document.getElementById('login-msg').innerText = "Vérification...";
    await syncPull();
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    
    if(u) {
        if(!u.actif && u.role !== "ADMIN") {
            document.getElementById('login-msg').innerText = "🚫 ACCÈS BLOQUÉ PAR L'ADMIN.";
            return;
        }
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        document.getElementById('fb-url').value = CLOUD_URL;
        addLog("Ouverture session");
        filtrerRoles(); initDropdown();
    } else {
        document.getElementById('login-msg').innerText = "Identifiants incorrects.";
    }
}

function filtrerRoles() {
    const r = currentUser.role;
    const isBoss = (r === "ADMIN");
    const isCaisse = (r === "ADMIN" || r === "Caissière");
    
    document.getElementById('btn-caisse').classList.toggle('hidden', !isCaisse);
    document.getElementById('btn-rapport').classList.toggle('hidden', !isCaisse);
    document.getElementById('btn-performance').classList.toggle('hidden', !isBoss);
    document.getElementById('btn-logs').classList.toggle('hidden', !isBoss);
    document.getElementById('btn-admin').classList.toggle('hidden', !isBoss);
    
    if(isBoss) document.getElementById('admin-stock-control').classList.remove('hidden');
    tab(isCaisse ? 'section-caisse' : 'section-atelier');
}

/* ==========================================================================
   CAISSE ET ENCAISSEMENT
   ========================================================================== */
function initDropdown() {
    const s = document.getElementById('select-article'); s.innerHTML = '<option value="">-- Article --</option>';
    for(let c in GRILLE) {
        let g = document.createElement('optgroup'); g.label = c;
        for(let a in GRILLE[c]) { let o = document.createElement('option'); o.value = GRILLE[c][a]; o.innerText = a; g.appendChild(o); }
        s.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(s => `<option value="${s.id}">${s.nom}</option>`).join('');
}

function majPrixInitial() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }

function ajouterAuPanier() {
    const s = document.getElementById('select-article'), p = document.getElementById('prix-final').value;
    if(!s.value || !p) return;
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p), pret: false });
    renderPanier();
}
function renderPanier() {
    const t = panier.reduce((a,b)=>a+b.prix, 0);
    document.getElementById('total-caisse').innerText = t + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`<div class="item-row"><b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()">✕</button></span></div>`).join('');
}

async function validerCommande() {
    const cmd = { id: Math.floor(1000+Math.random()*8999), client: document.getElementById('client-nom').value, tel: document.getElementById('tel-client').value, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, retrait: document.getElementById('date-retrait').value, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "En cours" };
    db.unshift(cmd);
    addLog(`Encaissement ticket #${cmd.id}`);
    if(CLOUD_URL) { await syncPush('db', db); } else { localStorage.setItem('sc_local_db', JSON.stringify(db)); }
    location.reload();
}

/* ==========================================================================
   ZONE TECHNIQUE (ÉTANCHE À L'ARGENT)
   ========================================================================== */
function renderAtelier() {
    const canSeeMoney = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card">
            <b>TICKET #${c.id} - ${c.client}</b><br>
            <small>RDV : ${c.retrait}</small><br>
            ${canSeeMoney ? `<div class="solde-badge" style="margin-top:5px;">À PAYER : ${c.total-c.paye} F</div>` : ''}
            <div style="margin-top:10px;">
                ${c.articles.map(a => `<div class="item-row"><span>${a.nom}</span> ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:8px 12px; border-radius:10px;">PRÊT</button>` : `<b style="color:green;">OK</b>`}</div>`).join('')}
            </div>
            ${canSeeMoney ? `<button onclick="livrer(${id})" class="btn-main" style="background:var(--dark); margin-top:10px;">LIVRER</button>` : ''}
        </div>`).join('');
}

/* ==========================================================================
   ADMINISTRATION ET PRÉROGATIVES
   ========================================================================== */
function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `
        <div class="item-row" style="background:${s.actif?'':'#fff1f2'}">
            <span><b>${s.nom}</b> (${s.role}) <span class="status-pill ${s.actif?'pill-active':'pill-blocked'}">${s.actif?'ACTIF':'BLOQUÉ'}</span></span>
            ${i!==0?`<button onclick="toggleUser(${i})">BLOQUER/OUVRIR</button>`:''}
        </div>`).join('');
}

async function toggleUser(i) {
    staff[i].actif = !staff[i].actif;
    if(CLOUD_URL) await syncPush('staff', staff); else localStorage.setItem('sc_local_staff', JSON.stringify(staff));
    renderAdmin();
}

function saveFBConfig() {
    let url = document.getElementById('fb-url').value;
    if(url.endsWith('/')) url = url.slice(0, -1);
    localStorage.setItem('sc_fb_url', url);
    location.reload();
}

// Utilitaires système (Achat, Log, Rapport, Performance, Stock) ...
function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]); if(btn) btn.classList.add('active');
    if(id==='section-atelier') renderAtelier();
    if(id==='section-admin') renderAdmin();
}
function addLog(a) { logs.unshift(`[${new Date().toLocaleString()}] ${currentUser.nom}: ${a}`); }
</script>
</body>
</html>
