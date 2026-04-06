<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v50.0 - VERSION FINALE INTÉGRALE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --primary-dark: #003d7a; --success: #2ecc71; 
            --danger: #ef233c; --warning: #ff9f1c; --dark: #1b2631; 
            --light: #f4f7f9; --white: #ffffff; --radius: 20px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; -webkit-tap-highlight-color: transparent; }
        .hidden { display: none !important; }
        
        /* --- HEADER & BRANDING --- */
        .header { background: var(--white); padding: 20px; text-align: center; border-bottom: 4px solid var(--primary); position: sticky; top: 0; z-index: 1000; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        .logo-main { width: 85px; height: 85px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 24px; margin-top: 10px; text-transform: uppercase; letter-spacing: -1px; }
        .official-slogan { font-size: 13px; color: #555; font-style: italic; font-weight: 700; margin-top: 2px; }

        /* --- NAVIGATION 7 ONGLETS --- */
        .nav-bar { display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; padding: 12px; background: var(--white); position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000; }
        .nav-bar button { background: #f1f4f8; border: none; color: #7f8c8d; font-weight: 800; font-size: 7px; padding: 12px 1px; border-radius: 12px; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .nav-bar button.active { background: var(--primary); color: var(--white); transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 86, 179, 0.4); }

        /* --- UI COMPONENTS --- */
        .card { background: var(--white); margin: 15px; padding: 25px; border-radius: var(--radius); box-shadow: 0 6px 20px rgba(0,0,0,0.06); border: 1px solid #eee; }
        h3 { margin-top: 0; font-size: 17px; color: var(--primary); font-weight: 900; text-transform: uppercase; border-bottom: 3px solid var(--light); padding-bottom: 12px; margin-bottom: 20px; }
        input, select, textarea { background: #f1f4f8; border: 2px solid transparent; padding: 16px; border-radius: 15px; margin-bottom: 12px; width: 100%; box-sizing: border-box; font-size: 15px; outline: none; transition: 0.3s; font-family: inherit; }
        input:focus { border-color: var(--primary); background: var(--white); }
        .btn-main { background: var(--primary); color: var(--white); border: none; padding: 20px; border-radius: 18px; font-weight: 900; width: 100%; cursor: pointer; text-transform: uppercase; font-size: 14px; }
        .btn-wa { background: #25D366; color: var(--white); border: none; padding: 16px; border-radius: 15px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; font-size: 13px; }
        
        /* --- LISTS & BADGES --- */
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 15px; margin-bottom: 8px; border-left: 6px solid var(--primary); font-size: 14px; }
        .solde-badge { background: var(--danger); color: white; padding: 6px 12px; border-radius: 10px; font-size: 12px; font-weight: 900; display: inline-block; }
        .status-pill { padding: 5px 12px; border-radius: 50px; font-size: 10px; font-weight: 900; text-transform: uppercase; }
        .pill-active { background: #dcfce7; color: #166534; }
        .pill-blocked { background: #fee2e2; color: #991b1b; }

        .sync-tag { position: fixed; top: 10px; right: 10px; font-size: 9px; padding: 5px 10px; border-radius: 10px; z-index: 3000; font-weight: bold; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        .online { background: var(--success); color: white; }
        .offline { background: var(--danger); color: white; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="sync-status" class="sync-tag offline">LANCEMENT...</div>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 90%; max-width: 420px; text-align: center; padding: 50px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
            <h2 style="letter-spacing: -1px;">SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:800; margin-bottom:35px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Identifiant Personnel">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">ACCÉDER À L'INTERFACE</button>
            <div id="login-error" style="color:var(--danger); font-size:11px; margin-top:15px; font-weight:bold;"></div>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 10px 20px; font-size: 11px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 CONNECTÉ : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); border:none; color:white; padding:6px 12px; border-radius:8px; font-size:10px; font-weight:bold; cursor:pointer;">QUITTER</button>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:60px; height:60px; border-radius:50%; border:2px solid var(--primary);">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 130px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION & ENCAISSEMENT</h3>
                <input type="tel" id="tel-client" placeholder="WhatsApp Client" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <label style="font-size:11px; font-weight:800; color:#777; margin-left:5px;">DATE DE RETRAIT PRÉVUE :</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:20px; border-radius:18px; border:1px dashed #ddd; margin-top:10px;">
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">SÉLECTIONNER L'ARTICLE :</label>
                    <select id="select-article" onchange="majPrixInitial()"></select>
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">PRIX UNITAIRE (F CFA) :</label>
                    <input type="number" id="prix-final" placeholder="Prix ajusté">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); margin-bottom:0;">+ AJOUTER AU TICKET</button>
                </div>
            </div>

            <div id="panier-liste" style="padding: 0 15px;"></div>

            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                    <span style="opacity:0.8;">NET À PAYER :</span> 
                    <b id="total-caisse" style="font-size:30px; color:var(--success);">0 F</b>
                </div>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white;">
                    <option value="Cash">💵 Cash (Espèces)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN MoMo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Montant Avance" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white; margin-top:10px;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning); font-size:18px;">Reste : 0 F</div>
            </div>

            <div style="padding: 0 15px;">
                <button onclick="validerCommande()" class="btn-main" style="height:80px; font-size:18px;">🖨️ VALIDER & TICKET</button>
            </div>

            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSE DE CAISSE</h3>
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
                <h3>📊 BILAN JOURNALIER WHATSAPP</h3>
                <div id="rapport-details" style="margin-bottom: 25px;"></div>
                <button onclick="envoyerRapportWA()" class="btn-wa" style="height:70px;">📲 ENVOYER AU GÉRANT</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE DES PERFORMANCES</h3>
                <div id="perf-display"></div>
                <div class="item-row" style="background:var(--success); color:white; padding:20px; margin-top:20px;">
                    <span style="font-weight:800;">RECETTE NETTE GLOBALE</span> 
                    <b id="p-net" style="font-size:22px;">0 F</b>
                </div>
            </div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3>🧼 INVENTAIRE DES PRODUITS</h3>
                <div id="stock-display"></div>
                <div id="admin-stock-control" class="hidden">
                    <hr style="margin:20px 0;">
                    <h4>📦 AJUSTEMENT DES STOCKS</h4>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Qté +/-">
                    <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">METTRE À JOUR</button>
                </div>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 JOURNAL D'ACTIVITÉS</h3>
                <div id="logs-container" style="max-height:500px; overflow-y:auto; background:#f1f4f8; border-radius:15px; padding:15px; font-family:monospace; font-size:11px;"></div>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card" style="background: #fff3cd;">
                <h3>⚙️ CONFIGURATION CLOUD</h3>
                <input type="text" id="fb-url" placeholder="https://votre-firebase.firebaseio.com">
                <button onclick="saveFBConfig()" class="btn-main" style="background:var(--dark);">ACTIVER LA SYNCHRONISATION</button>
            </div>

            <div class="card" style="border: 2px solid var(--primary);">
                <h3>👑 PERSONNEL & ACCÈS</h3>
                <div id="staff-list"></div>
                <hr>
                <h4>CRÉER UN COMPTE</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Login">
                <input type="password" id="ns-pass" placeholder="Password">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Zone Technique">Zone Technique</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">GÉNÉRER L'ACCÈS</button>
            </div>

            <div class="card">
                <h3>✨ AJOUTER ARTICLE À LA GRILLE</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie (ex: ROBES)">
                <input type="text" id="new-art-nom" placeholder="Désignation">
                <input type="number" id="new-art-prix" placeholder="Prix standard">
                <button onclick="ajouterNouveauProduit()" class="btn-main">ACTUALISER LA LISTE</button>
            </div>
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

<div id="ticket-print" style="display:none; text-align:center; font-family:monospace; padding:10px;">
    <b>SUPER CLEAN PRESSING</b><br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:12px;"></div>
</div>

<script>
/* ==========================================================================
   1. GRILLE TARIFAIRE INTÉGRALE (ZÉRO SIMPLIFICATION)
   ========================================================================== */
const GRILLE_OFFICIELLE = {
    "LES HAUTS": { "T-Shirt": 500, "Chemise": 500, "Polo": 500, "Pull Over": 700, "Blouson": 1500, "Démembré": 300, "Veste 2 pièces": 1500, "Veste 3 pièces": 2000, "Pantalon jeans": 500, "Jupe simple": 500, "Jupe plissée": 1000 },
    "ENSEMBLES": { "Costume simple": 2000, "Costume luxe": 2500, "Boubou femme": 1500, "Boubou homme 3P": 2500, "Jogging": 1500, "Pyjama 2P": 1000 },
    "LES ROBES": { "Robe simple": 1000, "Robe de soirée": 3000, "Kaba court": 1000, "Kaba long": 1500, "Toge": 3500 },
    "LINGES DE LIT": { "1 Drap": 750, "Ensemble Draps": 2000, "Couverture": 4000, "Couette": 2000 },
    "VÊTEMENTS ENFANTS": { "Ensemble enfant": 1000, "Robe enfant": 500, "Haut enfant": 500 },
    "AMEUBLEMENT / BAIN": { "Rideau lourd": 1500, "Petite Serviette": 500, "Grande Serviette": 1000, "Peignoir": 2000 },
    "ACCESSOIRES": { "Tennis": 500, "Cravate": 500 }
};

/* ==========================================================================
   2. ÉTATS ET MOTEUR CLOUD HYBRIDE
   ========================================================================== */
let CLOUD_URL = localStorage.getItem('sc_fb_url') || "";
let GRILLE = JSON.parse(localStorage.getItem('sc_local_grille')) || GRILLE_OFFICIELLE;
let db = [], staff = [], stocks = [], logs = [], achats = [];
let panier = [], currentUser = null;

const MASTER_ADMIN = {nom: "Directeur", login: "admin", pass: "0000", role: "ADMIN", actif: true};

async function syncPush(path, data) {
    if(!CLOUD_URL) return;
    try {
        await fetch(`${CLOUD_URL}/${path}.json`, { method: 'PUT', body: JSON.stringify(data) });
        updateSyncUI(true);
    } catch(e) { updateSyncUI(false); }
}

async function syncPull() {
    if(!CLOUD_URL) {
        db = JSON.parse(localStorage.getItem('sc_db')) || [];
        staff = JSON.parse(localStorage.getItem('sc_staff')) || [MASTER_ADMIN];
        stocks = JSON.parse(localStorage.getItem('sc_stocks')) || [{id:'savon', nom:'Savon', qte:100, unite:'L'},{id:'housses', nom:'Housses', qte:500, unite:'pcs'}];
        logs = JSON.parse(localStorage.getItem('sc_logs')) || [];
        achats = JSON.parse(localStorage.getItem('sc_achats')) || [];
        updateSyncUI(false);
        return true;
    }
    try {
        const controller = new AbortController();
        const timeoutId = setTimeout(() => controller.abort(), 3500);
        const res = await fetch(`${CLOUD_URL}/.json`, { signal: controller.signal });
        const data = await res.json();
        if(data) {
            db = data.db || []; staff = data.staff || [MASTER_ADMIN];
            stocks = data.stocks || [{id:'savon', nom:'Savon', qte:100, unite:'L'},{id:'housses', nom:'Housses', qte:500, unite:'pcs'}]; 
            logs = data.logs || []; achats = data.achats || [];
            if(data.GRILLE) GRILLE = data.GRILLE;
            updateSyncUI(true);
        }
        return true;
    } catch(e) {
        updateSyncUI(false);
        db = JSON.parse(localStorage.getItem('sc_db')) || [];
        staff = JSON.parse(localStorage.getItem('sc_staff')) || [MASTER_ADMIN];
        return true;
    }
}

function updateSyncUI(online) {
    const el = document.getElementById('sync-status');
    el.className = online ? "sync-tag online" : "sync-tag offline";
    el.innerText = online ? "☁️ CLOUD : ON" : "⚡ LOCAL : ON";
}

/* ==========================================================================
   3. AUTHENTIFICATION
   ========================================================================== */
async function login() {
    document.getElementById('login-error').innerText = "Vérification...";
    await syncPull();
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    
    if(u) {
        if(!u.actif && u.role !== "ADMIN") { document.getElementById('login-error').innerText = "🚫 COMPTE BLOQUÉ."; return; }
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        document.getElementById('fb-url').value = CLOUD_URL;
        addLog("Ouverture de session");
        filtrerRoles(); initDropdownArticles();
    } else { document.getElementById('login-error').innerText = "Identifiants incorrects."; }
}

function filtrerRoles() {
    const r = currentUser.role;
    const isBoss = (r === "ADMIN"), isCaisse = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').classList.toggle('hidden', !isCaisse);
    document.getElementById('btn-rapport').classList.toggle('hidden', !isCaisse);
    document.getElementById('btn-performance').classList.toggle('hidden', !isBoss);
    document.getElementById('btn-logs').classList.toggle('hidden', !isBoss);
    document.getElementById('btn-admin').classList.toggle('hidden', !isBoss);
    if(isBoss) document.getElementById('admin-stock-control').classList.remove('hidden');
    tab(isCaisse ? 'section-caisse' : 'section-atelier');
}

function addLog(action) {
    logs.unshift(`[${new Date().toLocaleString()}] ${currentUser.nom}: ${action}`);
    if(CLOUD_URL) syncPush('logs', logs.slice(0, 300));
    else localStorage.setItem('sc_logs', JSON.stringify(logs.slice(0, 300)));
}

/* ==========================================================================
   4. CAISSE & TECHNIQUE
   ========================================================================== */
function initDropdownArticles() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Choisir Article --</option>';
    for(let cat in GRILLE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in GRILLE[cat]) { let o = document.createElement('option'); o.value = GRILLE[cat][art]; o.innerText = art; g.appendChild(o); }
        sel.appendChild(g);
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
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`<div class="item-row"><b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none;">✕</button></span></div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${t - p} F`;
}

async function validerCommande() {
    const nom = document.getElementById('client-nom').value, dr = document.getElementById('date-retrait').value, tel = document.getElementById('tel-client').value;
    if(!nom || panier.length === 0) return alert("Champs incomplets.");
    const cmd = { id: Math.floor(1000+Math.random()*8999), client: nom, tel: tel, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, mode: document.getElementById('mode-pay').value, retrait: dr, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "En cours" };
    db.unshift(cmd);
    addLog(`Nouveau ticket #${cmd.id}`);
    if(CLOUD_URL) await syncPush('db', db); else localStorage.setItem('sc_db', JSON.stringify(db));
    location.reload();
}

function renderAtelier() {
    const canSeeMoney = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card">
            <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b> <small>${c.retrait}</small></div>
            ${canSeeMoney ? `<div class="solde-badge" style="margin-top:5px;">À PAYER : ${c.total-c.paye} F</div>` : ''}
            <div style="margin-top:10px;">
                ${c.articles.map(a => `<div class="item-row"><span>${a.nom}</span> ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:5px 12px; border-radius:10px;">FINI</button>` : `<b style="color:green;">PRÊT</b>`}</div>`).join('')}
            </div>
            ${canSeeMoney ? `<button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark); margin-top:10px;">LIVRER</button>` : ''}
        </div>`).join('');
}

async function finirArt(cid, auid) {
    const c = db.find(x => x.id === cid); c.articles.find(a => a.uid === auid).pret = true;
    if(CLOUD_URL) await syncPush('db', db); else localStorage.setItem('sc_db', JSON.stringify(db));
    renderAtelier();
}

async function livrer(id) {
    const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
    addLog(`Livraison #${id}`);
    if(CLOUD_URL) await syncPush('db', db); else localStorage.setItem('sc_db', JSON.stringify(db));
    renderAtelier();
}

/* ==========================================================================
   5. STOCKS, ADMIN, NAVIGATION
   ========================================================================== */
function renderStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-row"><span>${s.nom}</span> <b>${s.qte.toFixed(1)} ${s.unite}</b></div>`).join('');
}

async function majStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    stocks.find(x => x.id === id).qte += q;
    if(CLOUD_URL) await syncPush('stocks', stocks); else localStorage.setItem('sc_stocks', JSON.stringify(stocks));
    renderStocks();
}

function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `<div class="item-row"><span><b>${s.nom}</b> (${s.role}) <span class="status-pill ${s.actif?'pill-active':'pill-blocked'}">${s.actif?'Actif':'Bloqué'}</span></span> ${i!==0?`<button onclick="toggleUser(${i})">BASCULER</button>`:''}</div>`).join('');
}

async function toggleUser(i) {
    staff[i].actif = !staff[i].actif;
    if(CLOUD_URL) await syncPush('staff', staff); else localStorage.setItem('sc_staff', JSON.stringify(staff));
    renderAdmin();
}

async function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif:true}); if(CLOUD_URL) await syncPush('staff', staff); else localStorage.setItem('sc_staff', JSON.stringify(staff)); renderAdmin(); }
}

async function ajouterNouveauProduit() {
    const c=document.getElementById('new-art-cat').value.toUpperCase(), n=document.getElementById('new-art-nom').value, p=parseInt(document.getElementById('new-art-prix').value);
    if(c&&n&&p) { if(!GRILLE[c]) GRILLE[c]={}; GRILLE[c][n]=p; if(CLOUD_URL) await syncPush('GRILLE', GRILLE); else localStorage.setItem('sc_local_grille', JSON.stringify(GRILLE)); initDropdownArticles(); alert("Ajouté !"); }
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]); if(btn) btn.classList.add('active');
    if(id==='section-atelier') renderAtelier();
    if(id==='section-admin') renderAdmin();
    if(id==='section-stocks') renderStocks();
    if(id==='section-logs') document.getElementById('logs-container').innerHTML = logs.map(l => `<div style="font-size:10px; padding:5px; border-bottom:1px solid #eee;">${l}</div>`).join('');
}

function saveFBConfig() {
    let url = document.getElementById('fb-url').value; if(url.endsWith('/')) url = url.slice(0, -1);
    localStorage.setItem('sc_fb_url', url); location.reload();
}

function rechercherClient() { const t=document.getElementById('tel-client').value; if(t.length>=8){ const f=db.find(x=>x.tel&&x.tel.includes(t)); if(f) document.getElementById('client-nom').value=f.client; } }
async function enregistrerAchat() { const m=document.getElementById('achat-motif').value, mt=parseInt(document.getElementById('achat-montant').value); if(m&&mt){achats.push({motif:m,mont:mt,agent:currentUser.nom,timestamp:new Date().getTime()}); if(CLOUD_URL) await syncPush('achats', achats); else localStorage.setItem('sc_achats', JSON.stringify(achats)); location.reload();}}
</script>
</body>
</html>
