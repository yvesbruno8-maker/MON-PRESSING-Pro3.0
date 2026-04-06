<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v45.0 - SOUVERAINETÉ & SYNCHRO CLOUD</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --white: #ffffff;
            --radius: 20px; --shadow: 0 10px 30px rgba(0,0,0,0.1);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; -webkit-tap-highlight-color: transparent; }
        .hidden { display: none !important; }
        
        /* --- BRANDING & HEADER --- */
        .header { background: var(--white); padding: 25px; text-align: center; border-bottom: 4px solid var(--primary); position: sticky; top: 0; z-index: 1000; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        .logo-main { width: 90px; height: 90px; border-radius: 50%; border: 4px solid var(--primary); object-fit: cover; margin-bottom: 10px; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 26px; text-transform: uppercase; letter-spacing: -1px; }
        .official-slogan { font-size: 14px; color: #555; font-style: italic; font-weight: 700; margin-top: 5px; }

        /* --- NAVIGATION 7 ONGLETS --- */
        .nav-bar { display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; padding: 12px; background: var(--white); position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000; }
        .nav-bar button { background: #f1f4f8; border: none; color: #7f8c8d; font-weight: 800; font-size: 7px; padding: 12px 1px; border-radius: 12px; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .nav-bar button.active { background: var(--primary); color: var(--white); transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 86, 179, 0.4); }

        /* --- UI COMPONENTS --- */
        .card { background: var(--white); margin: 15px; padding: 25px; border-radius: var(--radius); box-shadow: var(--shadow); border: 1px solid #eee; }
        h3 { margin-top: 0; font-size: 17px; color: var(--primary); font-weight: 900; text-transform: uppercase; border-bottom: 3px solid var(--light); padding-bottom: 12px; margin-bottom: 20px; }
        input, select, textarea { background: #f1f4f8; border: 2px solid transparent; padding: 18px; border-radius: 15px; margin-bottom: 15px; width: 100%; box-sizing: border-box; font-size: 15px; outline: none; transition: 0.3s; font-family: inherit; }
        input:focus { border-color: var(--primary); background: var(--white); }
        .btn-main { background: var(--primary); color: var(--white); border: none; padding: 22px; border-radius: 18px; font-weight: 900; width: 100%; cursor: pointer; text-transform: uppercase; font-size: 15px; letter-spacing: 1px; }
        .btn-wa { background: #25D366; color: var(--white); border: none; padding: 18px; border-radius: 18px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; font-size: 14px; }
        
        /* --- STATUS & ALERTS --- */
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 15px; margin-bottom: 8px; border-left: 6px solid var(--primary); font-size: 14px; }
        .solde-badge { background: var(--danger); color: white; padding: 6px 12px; border-radius: 10px; font-size: 12px; font-weight: 900; display: inline-block; margin-bottom: 10px; }
        .retard-card { border: 2px solid var(--danger) !important; background: #fff5f5 !important; animation: blink-red 2s infinite; }
        @keyframes blink-red { 0%, 100% { opacity: 1; } 50% { opacity: 0.8; } }
        .status-pill { padding: 5px 12px; border-radius: 50px; font-size: 10px; font-weight: 900; text-transform: uppercase; }
        .pill-active { background: #dcfce7; color: #166534; }
        .pill-blocked { background: #fee2e2; color: #991b1b; }
        .sync-tag { position: fixed; top: 10px; right: 10px; font-size: 9px; padding: 4px 8px; border-radius: 10px; z-index: 3000; font-weight: 900; box-shadow: var(--shadow); }
        .sync-ok { background: var(--success); color: white; }
        .sync-ko { background: var(--danger); color: white; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="sync-indicator" class="sync-tag sync-ko">CLOUD DÉCONNECTÉ</div>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 90%; max-width: 420px; text-align: center; padding: 60px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
            <h2 style="margin:0; letter-spacing:-1px;">SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:800; margin-bottom:40px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Votre Identifiant">
            <input type="password" id="u-pass" placeholder="Votre Mot de passe">
            <button onclick="login()" class="btn-main" style="box-shadow: 0 10px 20px rgba(0,0,0,0.2);">ENTRER DANS LA SESSION</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 12px 20px; font-size: 11px; display: flex; justify-content: space-between; align-items: center; font-weight: 600;">
        <span>👤 CONNECTÉ : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); border:none; color:white; padding:6px 12px; border-radius:8px; font-size:10px; font-weight:bold; cursor:pointer;">DÉCONNEXION</button>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:60px; height:60px; border-radius:50%; border:2px solid var(--primary);">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 140px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION & ENCAISSEMENT</h3>
                <input type="tel" id="tel-client" placeholder="N° WhatsApp Client" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <label style="font-size:11px; font-weight:800; color:#777; margin-left:5px;">DATE DE RETRAIT PRÉVUE :</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:20px; border-radius:20px; border:2px dashed #ddd; margin-top:10px;">
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">ARTICLE À AJOUTER :</label>
                    <select id="select-article" onchange="majPrixInitial()"></select>
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">PRIX UNITAIRE APPLIQUÉ (F CFA) :</label>
                    <input type="number" id="prix-final" placeholder="Prix">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); margin-bottom:0;">+ AJOUTER AU TICKET</button>
                </div>
            </div>

            <div id="panier-liste" style="padding: 0 15px;"></div>

            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                    <span style="opacity:0.8;">TOTAL PRESTATIONS :</span> 
                    <b id="total-caisse" style="font-size:32px; color:var(--success);">0 F</b>
                </div>
                <label style="font-size:11px; color:#aaa; font-weight:700;">MODE DE RÈGLEMENT :</label>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white; margin-bottom:15px;">
                    <option value="Cash">💵 Cash (Espèces)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN MoMo</option>
                </select>
                <label style="font-size:11px; color:#aaa; font-weight:700;">MONTANT DE L'AVANCE :</label>
                <input type="number" id="paye-caisse" placeholder="Avance versée" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning); font-size:18px;">Reste à percevoir : 0 F</div>
            </div>

            <div style="padding: 0 15px;">
                <button onclick="validerCommande()" class="btn-main" style="height:85px; font-size:18px; box-shadow: 0 10px 20px rgba(0,86,179,0.3);">🖨️ VALIDER & IMPRIMER TICKET</button>
            </div>

            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSES DE CAISSE (ACHATS)</h3>
                <input type="text" id="achat-motif" placeholder="Motif de la dépense (ex: Savon, Facture...)">
                <input type="number" id="achat-montant" placeholder="Montant F CFA">
                <button onclick="enregistrerAchat()" class="btn-main" style="background:var(--danger);">DÉDUIRE DE LA RECETTE</button>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 25px; font-weight: 900;">👕 ZONE TECHNIQUE - SUIVI PRODUCTION</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-rapport" class="tab-content hidden">
            <div class="card">
                <h3>📊 BILAN JOURNALIER WHATSAPP</h3>
                <div id="rapport-details" style="margin-bottom: 25px;"></div>
                <button onclick="envoyerRapportWA()" class="btn-wa" style="height:75px;">📲 ENVOYER LE BILAN AU GÉRANT</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE DES PERFORMANCES</h3>
                <div class="item-row"><span>Recette Aujourd'hui</span> <b id="p-today">0 F</b></div>
                <div class="item-row"><span>Hebdomadaire (7 derniers jours)</span> <b id="p-week">0 F</b></div>
                <div class="item-row"><span>Mensuel (Mois courant)</span> <b id="p-month">0 F</b></div>
                <div class="item-row"><span>Annuel (Année civile)</span> <b id="p-year">0 F</b></div>
                <div class="item-row" style="background:var(--success); color:white; padding:25px; margin-top:20px;">
                    <span style="font-weight:800; font-size: 16px;">BÉNÉFICE NET GLOBAL</span> 
                    <b id="p-net" style="font-size:24px;">0 F</b>
                </div>
            </div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3>🧼 INVENTAIRE DES CONSOMMABLES</h3>
                <div id="stock-display"></div>
                <div id="admin-stock-ui" class="hidden">
                    <hr style="margin:25px 0;">
                    <h4>📦 AJUSTEMENT MANUEL DES STOCKS</h4>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter (+/-)">
                    <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">METTRE À JOUR</button>
                </div>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 JOURNAL D'ACTIVITÉS (AUDIT)</h3>
                <div id="logs-container" style="max-height:500px; overflow-y:auto; background:#f1f4f8; border-radius:15px; padding:15px; font-family:monospace; font-size:11px;"></div>
                <button onclick="clearLogs()" class="btn-main" style="background:var(--danger); margin-top:20px; font-size:11px; padding:10px;">EFFACER LE JOURNAL (ADMIN SEUL)</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card" style="background:var(--warning); color:black;">
                <h3>⚙️ CONFIGURATION CLOUD SYNCHRO</h3>
                <p style="font-size:12px; font-weight:700;">URL FIREBASE :</p>
                <input type="text" id="fb-url" placeholder="https://votre-projet.firebaseio.com/">
                <button onclick="saveFBConfig()" class="btn-main" style="background:var(--dark);">ACTIVER LA SYNCHRO TEMPS RÉEL</button>
            </div>

            <div class="card" style="border: 4px solid var(--primary);">
                <h3>👥 PERSONNEL & DROITS D'ACCÈS</h3>
                <div id="staff-list"></div>
                <hr style="margin:30px 0;">
                <h4 style="color:var(--primary);">➕ CRÉER UN NOUVEL ACCÈS PERSONNEL</h4>
                <input type="text" id="ns-nom" placeholder="Nom et Prénom">
                <input type="text" id="ns-log" placeholder="Identifiant (Login)">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role">
                    <option value="Caissière">Caissière (Caisse + Rapport)</option>
                    <option value="Zone Technique">Zone Technique (Lavage/Repassage)</option>
                    <option value="ADMIN">ADMINISTRATEUR (Directeur)</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER ET ACTIVER LE COMPTE</button>
            </div>

            <div class="card">
                <h3>✨ GESTION DYNAMIQUE DE LA GRILLE</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie (ex: ROBES)">
                <input type="text" id="new-art-nom" placeholder="Désignation du vêtement">
                <input type="number" id="new-art-prix" placeholder="Prix standard F CFA">
                <button onclick="ajouterNouveauProduit()" class="btn-main">ACTUALISER LA LISTE DE VENTE</button>
            </div>
            
            <div class="card" style="text-align:center;">
                <h3>☁️ SAUVEGARDE DE SÉCURITÉ</h3>
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 EXPORTER TOUTE LA BASE (.JSON)</button>
                <input type="file" id="importFile" accept=".json" style="margin-top:20px;">
                <button onclick="importData()" class="btn-main" style="background:var(--success);">📥 RESTAURER UNE SAUVEGARDE</button>
            </div>
        </div>
    </div>

    <nav class="nav-bar" id="main-nav">
        <button onclick="tab('section-caisse')" id="btn-caisse">📥 CAISSE</button>
        <button onclick="tab('section-rapport')" id="btn-rapport">📊 RAPPORT</button>
        <button onclick="tab('section-performance')" id="btn-performance">📈 PERF</button>
        <button onclick="tab('section-atelier')" id="btn-atelier">👕 Z.T</button>
        <button onclick="tab('section-stocks')" id="btn-stocks">🧼 STOCKS</button>
        <button onclick="tab('section-logs')" id="btn-logs">📜 LOGS</button>
        <button onclick="tab('section-admin')" id="btn-admin">👑 ADMIN</button>
    </nav>
</div>

<div id="ticket-print" style="display:none; text-align:center; font-family:monospace; padding:10px;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:80px; border-radius:50%;"><br>
    <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
    "Le bien être de vos vêtements"<br>
    Nkounga, Immeuble Karche<br>
    Tél: 675 864 103 / 692 315 267<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:13px; line-height:1.5;"></div>
    --------------------------------<br>
    Merci de votre confiance !
</div>

<script>
/* ==========================================================================
   CONFIGURATION CLOUD & ÉTATS
   ========================================================================== */
let CLOUD_URL = localStorage.getItem('sc_fb_url') || "";
let GRILLE = {
    "LES HAUTS": {"T-Shirt": 500, "Chemise": 500, "Veste 2P": 1500, "Veste 3P": 2000, "Blouson": 1500, "Pull Over": 700},
    "ENSEMBLES": {"Costume luxe": 2500, "Boubou 3P": 2500, "Jogging": 1500, "Pyjama": 1000},
    "LES ROBES": {"Robe simple": 1000, "Robe soirée": 3000, "Kaba long": 1500, "Toge": 3500},
    "LIT & MAISON": {"1 Drap": 750, "Ensemble Draps": 2000, "Couverture": 4000},
    "ACCESSOIRES": {"Tennis": 500, "Cravate": 500, "Peignoir": 2000}
};
let db = [], staff = [], stocks = [], logs = [], achats = [];
let panier = [], currentUser = null;

// Fonctions de Synchronisation Cloud
async function syncPush(path, data) {
    if(!CLOUD_URL) return;
    try {
        await fetch(`${CLOUD_URL}/${path}.json`, { method: 'PUT', body: JSON.stringify(data) });
        document.getElementById('sync-indicator').className = "sync-tag sync-ok";
        document.getElementById('sync-indicator').innerText = "CLOUD SYNCHRONISÉ";
    } catch(e) { 
        document.getElementById('sync-indicator').className = "sync-tag sync-ko";
        document.getElementById('sync-indicator').innerText = "ERREUR RÉSEAU";
    }
}

async function syncPull() {
    if(!CLOUD_URL) return;
    try {
        const res = await fetch(`${CLOUD_URL}/.json`);
        const data = await res.json();
        if(data) {
            db = data.db || []; staff = data.staff || [{nom: "Directeur", login: "admin", pass: "0000", role: "ADMIN", actif: true}];
            stocks = data.stocks || [{id:'savon', nom:'Savon', qte:100, unite:'L'}, {id:'housses', nom:'Housses', qte:500, unite:'pcs'}];
            logs = data.logs || []; achats = data.achats || [];
            if(data.GRILLE) GRILLE = data.GRILLE;
            document.getElementById('sync-indicator').className = "sync-tag sync-ok";
        }
    } catch(e) { }
}

// Rafraîchissement automatique toutes les 8 secondes
setInterval(() => { if(currentUser) syncPull(); }, 8000);

/* ==========================================================================
   SÉCURITÉ & AUTHENTIFICATION
   ========================================================================== */
async function login() {
    await syncPull();
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return alert("🚫 ACCÈS RÉVOQUÉ : Votre compte est bloqué.");
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        document.getElementById('fb-url').value = CLOUD_URL;
        addLog("Ouverture de session");
        filtrerRoles();
        initDropdown();
    } else alert("Identifiants incorrects !");
}

function saveFBConfig() {
    let url = document.getElementById('fb-url').value;
    if(url.endsWith('/')) url = url.slice(0, -1);
    localStorage.setItem('sc_fb_url', url);
    CLOUD_URL = url;
    syncPush('staff', staff); // Initialisation
    alert("Cloud Super Clean Actif !");
    location.reload();
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

function addLog(action) {
    logs.unshift(`[${new Date().toLocaleString('fr-FR')}] ${currentUser.nom} : ${action}`);
    syncPush('logs', logs.slice(0, 500));
}

/* ==========================================================================
   LOGIQUE CAISSE & ACHATS
   ========================================================================== */
function initDropdown() {
    const sel = document.getElementById('select-article'); sel.innerHTML = '<option value="">-- Article --</option>';
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
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`<div class="item-row"><b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()">✕</button></span></div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${t - p} F`;
}

async function validerCommande() {
    const nom = document.getElementById('client-nom').value, dr = document.getElementById('date-retrait').value, tel = document.getElementById('tel-client').value;
    if(!nom || panier.length === 0) return alert("Données Client Incomplètes !");
    
    const cmd = { id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, mode: document.getElementById('mode-pay').value, retrait: dr, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "En cours" };
    
    db.unshift(cmd); 
    addLog(`Encaissement Ticket #${cmd.id} (${cmd.total}F)`);
    await syncPush('db', db);
    
    // Déduction auto stocks
    stocks[0].qte -= (panier.length * 0.05); 
    await syncPush('stocks', stocks);

    imprimerTicket(cmd);
    location.reload();
}

function imprimerTicket(c) {
    document.getElementById('tk-body').innerHTML = `TICKET N° ${c.id}<br>CLIENT: ${c.client.toUpperCase()}<br>TEL: ${c.tel}<hr>${c.articles.map(a=>`${a.nom.padEnd(15)} ${a.prix}F`).join('<br>')}<hr>TOTAL: ${c.total}F<br>PAYÉ: ${c.paye}F<br>SOLDE: ${c.total-c.paye}F<hr>RDV: ${c.retrait}`;
    window.print();
}

async function enregistrerAchat() {
    const m = document.getElementById('achat-motif').value, mont = parseInt(document.getElementById('achat-montant').value);
    if(m && mont) {
        achats.push({motif: m, mont, agent: currentUser.nom, timestamp: new Date().getTime()});
        addLog(`ACHAT CAISSE: ${m} (-${mont}F)`);
        await syncPush('achats', achats);
        alert("Dépense Enregistrée"); location.reload();
    }
}

/* ==========================================================================
   ZONE TECHNIQUE (ÉTANCHE FINANCIÈREMENT)
   ========================================================================== */
function renderAtelier() {
    const canSeeMoney = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card">
            <b>TICKET #${c.id} - ${c.client}</b><br>
            <small>RDV: ${c.retrait}</small><br>
            ${canSeeMoney ? `<div class="solde-badge">À PAYER : ${c.total-c.paye} F</div>` : ''}
            <div style="margin-top:10px;">
                ${c.articles.map(a => `<div class="item-row"><span>${a.nom}</span> ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:8px 15px; border-radius:10px;">PRÊT</button>` : `<b style="color:green;">📍 FINI</b>`}</div>`).join('')}
            </div>
            ${canSeeMoney ? `<button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark); margin-top:10px;">LIVRER & SOLDE</button>` : `<div style="text-align:center; font-size:11px; color:#666;">Travaillez bien, Super Clean !</div>`}
        </div>`).join('');
}

async function finirArt(cid, auid) {
    const c = db.find(x => x.id === cid), a = c.articles.find(x => x.uid === auid);
    a.pret = true; addLog(`Article prêt: ${a.nom} (#${cid})`);
    await syncPush('db', db); renderAtelier();
}

async function livrer(id) {
    const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
    addLog(`Livraison Ticket #${id}`); await syncPush('db', db); renderAtelier();
}

/* ==========================================================================
   RAPPORTS & ANALYTICS
   ========================================================================== */
function renderRapport() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === today);
    let cash = 0, om = 0, momo = 0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    let totalAchats = ach.reduce((a,b)=>a+b.mont, 0);

    document.getElementById('rapport-details').innerHTML = `<div class="item-row"><span>Recettes Brutes :</span> <b>${cash+om+momo} F</b></div><div class="item-row" style="color:red"><span>Dépenses :</span> <b>-${totalAchats} F</b></div><div class="item-row" style="background:var(--success); color:white;"><span>NET EN CAISSE :</span> <b>${cash+om+momo-totalAchats} F CFA</b></div>`;
}

function envoyerRapportWA() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === today);
    let tot = tks.reduce((a,b)=>a+b.paye, 0), spent = ach.reduce((a,b)=>a+b.mont, 0);
    const msg = `📊 *RAPPORT JOURNALIER SUPER CLEAN*\nAgent: ${currentUser.nom}\nRecettes: ${tot}F\nAchats: ${spent}F\n💰 *NET: ${tot-spent}F*`;
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
}

function calculerPerformance() {
    const now = new Date();
    let d=0, w=0, m=0, y=0;
    db.forEach(t => {
        const dt = new Date(t.timestamp);
        if(dt.toDateString() === now.toDateString()) d+=t.paye;
        if(now - dt <= 7*24*3600000) w+=t.paye;
        if(dt.getMonth() === now.getMonth()) m+=t.paye;
        if(dt.getFullYear() === now.getFullYear()) y+=t.paye;
    });
    document.getElementById('p-today').innerText = d+" F"; document.getElementById('p-week').innerText = w+" F";
    document.getElementById('p-month').innerText = m+" F"; document.getElementById('p-year').innerText = y+" F";
    document.getElementById('p-net').innerText = (db.reduce((a,b)=>a+b.paye,0)-achats.reduce((a,b)=>a+b.mont,0))+" F CFA";
}

/* ==========================================================================
   ADMINISTRATION & GRILLE
   ========================================================================== */
function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `<div class="item-row" style="background:${s.actif?'':'#fff1f2'}"><div><b>${s.nom}</b> (${s.role})<br><span class="status-pill ${s.actif?'pill-active':'pill-blocked'}">${s.actif?'ACTIF':'BLOQUÉ'}</span></div> ${i!==0?`<button onclick="toggleAccess(${i})">BASCULER ACCÈS</button>`:''}</div>`).join('');
}
async function toggleAccess(i) { staff[i].actif = !staff[i].actif; await syncPush('staff', staff); renderAdmin(); }

async function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif:true}); await syncPush('staff', staff); renderAdmin(); }
}
async function ajouterNouveauProduit() {
    const c=document.getElementById('new-art-cat').value.toUpperCase(), n=document.getElementById('new-art-nom').value, p=parseInt(document.getElementById('new-art-prix').value);
    if(c&&n&&p) { if(!GRILLE[c])GRILLE[c]={}; GRILLE[c][n]=p; await syncPush('GRILLE', GRILLE); initDropdown(); alert("Produit ajouté !"); }
}

/* ==========================================================================
   SYSTEM NAVIGATION
   ========================================================================== */
function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]); if(btn) btn.classList.add('active');
    if(id==='section-atelier') renderAtelier();
    if(id==='section-admin') renderAdmin();
    if(id==='section-performance') calculerPerformance();
    if(id==='section-rapport') renderRapport();
    if(id==='section-stocks') renderStocks();
    if(id==='section-logs') document.getElementById('logs-container').innerHTML = logs.map(l => `<div class="log-row">${l}</div>`).join('');
}

function renderStocks() { document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-row"><span>${s.nom}</span> <b>${s.qte} ${s.unite}</b></div>`).join(''); }
async function majStockManuel() { const i=document.getElementById('upd-stock-item').value, q=parseFloat(document.getElementById('upd-stock-qte').value); stocks.find(x=>x.id===i).qte+=q; await syncPush('stocks', stocks); renderStocks(); }
function rechercherClient() { const t=document.getElementById('tel-client').value; if(t.length>=8){ const f=db.find(x=>x.tel&&x.tel.includes(t)); if(f) document.getElementById('client-nom').value=f.client; } }
function exportData() { const b=new Blob([JSON.stringify({db, staff, stocks, achats, logs, GRILLE})], {type:"application/json"}); const a=document.createElement('a'); a.href=URL.createObjectURL(b); a.download="SuperClean_FullBackup.json"; a.click(); }
function importData() { const r=new FileReader(); r.onload=(e)=>{ const d=JSON.parse(e.target.result); syncPush('db', d.db); syncPush('staff', d.staff); location.reload(); }; r.readAsText(document.getElementById('importFile').files[0]); }
function clearLogs() { if(confirm("Vider ?")) { logs=[]; syncPush('logs', []); tab('section-logs'); } }
</script>
</body>
</html>
