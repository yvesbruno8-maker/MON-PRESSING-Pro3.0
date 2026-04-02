<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v36.0 - SYSTÈME INTÉGRAL</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --white: #ffffff; --radius: 20px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; }
        .hidden { display: none !important; }
        
        /* --- BRANDING --- */
        .header { background: var(--white); padding: 20px; text-align: center; border-bottom: 3px solid var(--primary); position: sticky; top: 0; z-index: 1000; box-shadow: 0 4px 12px rgba(0,0,0,0.05); }
        .logo-main { width: 80px; height: 80px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 24px; margin-top: 10px; letter-spacing: -0.5px; }
        .slogan { font-size: 13px; color: #555; font-style: italic; font-weight: 600; margin-top: 2px; }

        /* --- NAVIGATION --- */
        .nav-bar { display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; padding: 10px; background: var(--white); position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000; }
        .nav-bar button { background: #f8f9fa; border: none; color: #7f8c8d; font-weight: 800; font-size: 6px; padding: 12px 1px; border-radius: 12px; cursor: pointer; text-transform: uppercase; transition: 0.2s; }
        .nav-bar button.active { background: var(--primary); color: var(--white); transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 86, 179, 0.3); }

        /* --- UI ELEMENTS --- */
        .card { background: var(--white); margin: 12px; padding: 22px; border-radius: var(--radius); box-shadow: 0 4px 15px rgba(0,0,0,0.06); border: 1px solid #eee; position: relative; }
        h3 { margin-top: 0; font-size: 16px; color: var(--primary); font-weight: 900; text-transform: uppercase; border-bottom: 2px solid var(--light); padding-bottom: 10px; margin-bottom: 15px; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 15px; border-radius: 14px; margin-bottom: 10px; width: 100%; box-sizing: border-box; font-size: 14px; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--primary); background: var(--white); }
        .btn-main { background: var(--primary); color: var(--white); border: none; padding: 18px; border-radius: 16px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; letter-spacing: 1px; }
        
        /* --- SPECIAL CLASSES --- */
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f8f9fa; border-radius: 12px; margin-bottom: 6px; border-left: 5px solid var(--primary); font-size: 13px; }
        .log-row { font-size: 10px; padding: 8px; border-bottom: 1px solid #eee; font-family: monospace; background: #fafafa; }
        .status-pill { padding: 4px 10px; border-radius: 20px; font-size: 10px; font-weight: 900; }
        .active-pill { background: #dcfce7; color: #166534; }
        .blocked-pill { background: #fee2e2; color: #991b1b; }
        .solde-badge { background: var(--danger); color: white; padding: 4px 10px; border-radius: 8px; font-size: 11px; font-weight: 900; display: inline-block; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 85%; max-width: 380px; text-align: center; padding: 45px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:100px; border-radius:50%; margin-bottom:15px; border: 4px solid #f1f4f8;">
            <h2 style="margin:0; letter-spacing:-1px;">SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:700; margin-bottom:30px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Identifiant">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">OUVRIR LA SESSION</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 8px 15px; font-size: 10px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 CONNECTÉ : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); color:white; border:none; padding:4px 8px; border-radius:5px; font-size:9px; font-weight:bold; cursor:pointer;">DÉCONNEXION</button>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 120px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION CLIENT</h3>
                <input type="tel" id="tel-client" placeholder="WhatsApp Client (ex: 6...)" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <label style="font-size:10px; font-weight:900; color:#777; margin-left:5px;">DATE DE RETRAIT PRÉVUE :</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:15px; border-radius:18px; border:1px dashed #ddd; margin-top:10px;">
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">SÉLECTIONNER L'ARTICLE :</label>
                    <select id="select-article" onchange="majPrixSuggere()"></select>
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">PRIX UNITAIRE APPLIQUÉ (F CFA) :</label>
                    <input type="number" id="prix-final" placeholder="Prix">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); margin-bottom:0;">+ AJOUTER AU TICKET</button>
                </div>
            </div>

            <div id="panier-liste" style="padding: 0 15px;"></div>

            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                    <span style="opacity:0.8;">NET À PERCEVOIR :</span> <b id="total-caisse" style="font-size:26px; color:var(--success);">0 F</b>
                </div>
                <label style="font-size:10px; color:#aaa;">MODE DE RÈGLEMENT :</label>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white;">
                    <option value="Cash">💵 Cash (Espèces)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN MoMo</option>
                </select>
                <label style="font-size:10px; color:#aaa;">AVANCE VERSÉE :</label>
                <input type="number" id="paye-caisse" placeholder="Montant encaissé" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning); font-size:16px;">Reste à payer : 0 F</div>
            </div>

            <div style="padding: 0 15px;">
                <button onclick="validerCommande()" class="btn-main" style="height:75px; font-size:16px;">🖨️ VALIDER & IMPRIMER TICKET</button>
            </div>

            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSES (SORTIES DE CAISSE)</h3>
                <input type="text" id="achat-motif" placeholder="Motif de la dépense (ex: Savon, Charbon...)">
                <input type="number" id="achat-montant" placeholder="Montant F CFA">
                <button onclick="enregistrerAchat()" class="btn-main" style="background:var(--danger);">DÉDUIRE DE LA RECETTE</button>
            </div>
        </div>

        <div id="section-rapport" class="tab-content hidden">
            <div class="card">
                <h3>📊 BILAN JOURNALIER WHATSAPP</h3>
                <div id="rapport-details" style="margin-bottom: 20px;"></div>
                <button onclick="envoyerRapportWA()" class="btn-main" style="background:#25D366;">📲 ENVOYER AU GÉRANT</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE DES PERFORMANCES</h3>
                <div class="item-row"><span>Aujourd'hui</span> <b id="p-today">0 F</b></div>
                <div class="item-row"><span>Hebdomadaire (7j)</span> <b id="p-week">0 F</b></div>
                <div class="item-row"><span>Mensuel</span> <b id="p-month">0 F</b></div>
                <div class="item-row"><span>Annuel</span> <b id="p-year">0 F</b></div>
                <div class="item-row" style="background:var(--success); color:white;"><span>Bénéfice Net Global</span> <b id="p-net">0 F</b></div>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px; font-weight: 800;">👕 ZONE TECHNIQUE - PRODUCTION</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3>🧼 INVENTAIRE DES PRODUITS</h3>
                <div id="stock-display"></div>
                <div id="admin-stock-ui" class="hidden">
                    <hr>
                    <h4>📦 APPROVISIONNEMENT MANUEL</h4>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Quantité (+/-)">
                    <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">AJOUTER</button>
                </div>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 JOURNAL D'ACTIVITÉS (AUDIT)</h3>
                <div id="logs-container" style="max-height:500px; overflow-y:auto; background:#f1f4f8; border-radius:10px; padding:10px;"></div>
                <button onclick="clearLogs()" class="btn-main" style="background:var(--danger); margin-top:15px; font-size:10px; padding:10px;">EFFACER LE JOURNAL (ADMIN)</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card">
                <h3>👥 PERSONNEL & ACCÈS (ADMIN SEUL)</h3>
                <div id="staff-list"></div>
                <hr>
                <h4>AJOUTER UN EMPLOYÉ</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Login">
                <input type="password" id="ns-pass" placeholder="Password">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Zone Technique">Zone Technique</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER ET ACTIVER</button>
            </div>

            <div class="card">
                <h3>✨ AJOUTER UN PRODUIT DYNAMIQUEMENT</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie (ex: ROBES)">
                <input type="text" id="new-art-nom" placeholder="Nom du produit">
                <input type="number" id="new-art-prix" placeholder="Prix F CFA">
                <button onclick="ajouterNouveauProduit()" class="btn-main">AJOUTER À LA GRILLE</button>
            </div>
            
            <div class="card" style="text-align:center;">
                <h3>☁️ CLOUD BACKUP</h3>
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 EXPORTER DRIVE/ICLOUD</button>
                <input type="file" id="importFile" accept=".json" style="margin-top:20px;">
                <button onclick="importData()" class="btn-main" style="background:var(--success);">📥 RESTAURER</button>
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
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:70px; border-radius:50%;"><br>
    <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
    "Le bien être de vos vêtements"<br>
    Nkounga, Immeuble Karche<br>
    Tél: 675 864 103 / 692 315 267<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:12px;"></div>
    --------------------------------<br>
    Merci de votre confiance !
</div>

<script>
// ==========================================
// 1. GRILLE TARIFAIRE ET DONNÉES
// ==========================================
let GRILLE = JSON.parse(localStorage.getItem('sc_grille_v36')) || {
    "LES HAUTS": {"T-Shirt": 500, "Chemise": 500, "Veste 2P": 1500, "Veste 3P": 2000, "Blouson": 1500, "Pull Over": 700},
    "ENSEMBLES": {"Costume luxe": 2500, "Boubou 3P": 2500, "Jogging": 1500, "Pyjama": 1000},
    "LES ROBES": {"Robe simple": 1000, "Robe soirée": 3000, "Kaba long": 1500, "Toge": 3500},
    "LIT & MAISON": {"1 Drap": 750, "Ensemble Draps": 2000, "Couverture": 4000, "Couette": 2000, "Rideau lourd": 1500},
    "ENFANTS": {"Ensemble enfant": 1000, "Robe enfant": 500},
    "AUTRES": {"Tennis": 500, "Cravate": 500, "Peignoir": 2000, "Serviette": 1000}
};

let db = JSON.parse(localStorage.getItem('sc_db_v36')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v36')) || [{nom:"Administrateur", login:"admin", pass:"0000", role:"ADMIN", actif:true}];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v36')) || [
    {id:'savon', nom:'Savon Liquide', qte:50, unite:'L', mini:5},
    {id:'parfum', nom:'Parfum Textile', qte:10, unite:'L', mini:2},
    {id:'housses', nom:'Housses Plastiques', qte:100, unite:'pcs', mini:10}
];
let logs = JSON.parse(localStorage.getItem('sc_logs_v36')) || [];
let achats = JSON.parse(localStorage.getItem('sc_achats_v36')) || [];
let panier = [], currentUser = null;

// ==========================================
// 2. AUTHENTIFICATION & SÉCURITÉ ROLES EXCLUSIFS
// ==========================================
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return alert("🚫 VOTRE ACCÈS EST BLOQUÉ. CONTACTEZ LE GÉRANT.");
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        addLog("Connexion session");
        filtrerRoles(); initDropdown();
    } else alert("Identifiants incorrects !");
}

function filtrerRoles() {
    const r = currentUser.role;
    const allowsCaisse = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-rapport').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-performance').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-logs').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-admin').classList.toggle('hidden', r !== "ADMIN");
    if(r === "ADMIN") document.getElementById('admin-stock-ui').classList.remove('hidden');
    tab(allowsCaisse ? 'section-caisse' : 'section-atelier');
}

function addLog(action) {
    logs.unshift(`[${new Date().toLocaleString('fr-FR')}] ${currentUser.nom} : ${action}`);
    localStorage.setItem('sc_logs_v36', JSON.stringify(logs.slice(0, 1000)));
}

// ==========================================
// 3. LOGIQUE PRODUITS DYNAMIQUES
// ==========================================
function ajouterNouveauProduit() {
    const cat = document.getElementById('new-art-cat').value.toUpperCase();
    const nom = document.getElementById('new-art-nom').value;
    const prix = parseInt(document.getElementById('new-art-prix').value);
    if(!cat || !nom || !prix) return alert("Remplissez tout !");
    if(!GRILLE[cat]) GRILLE[cat] = {};
    GRILLE[cat][nom] = prix;
    localStorage.setItem('sc_grille_v36', JSON.stringify(GRILLE));
    alert("Produit ajouté !"); initDropdown(); addLog(`Nouveau produit: ${nom}`);
}

function initDropdown() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Choisir Article --</option>';
    for(let cat in GRILLE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in GRILLE[cat]) {
            let o = document.createElement('option'); o.value = GRILLE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        sel.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(s => `<option value="${s.id}">${s.nom}</option>`).join('');
}

// ==========================================
// 4. LOGIQUE CAISSE & ACHATS
// ==========================================
function majPrixSuggere() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }
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

function validerCommande() {
    const nom = document.getElementById('client-nom').value, dr = document.getElementById('date-retrait').value, tel = document.getElementById('tel-client').value;
    if(!nom || panier.length === 0) return alert("Incomplet !");
    
    // Déduction auto stocks
    stocks[0].qte -= (panier.length * 0.05);
    localStorage.setItem('sc_stocks_v36', JSON.stringify(stocks));

    const cmd = { id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, mode: document.getElementById('mode-pay').value, retrait: dr, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "En cours" };
    db.unshift(cmd); localStorage.setItem('sc_db_v36', JSON.stringify(db));
    addLog(`Ticket créé #${cmd.id}`);
    document.getElementById('tk-body').innerHTML = `TICKET N° ${cmd.id}<br>CLIENT: ${cmd.client.toUpperCase()}<hr>TOTAL: ${cmd.total} F<br>SOLDE: ${cmd.total - cmd.paye} F<br>RDV: ${cmd.retrait}`;
    window.print(); location.reload();
}

function enregistrerAchat() {
    const motif = document.getElementById('achat-motif').value, mont = parseInt(document.getElementById('achat-montant').value);
    if(motif && mont) {
        achats.push({motif, mont, agent: currentUser.nom, timestamp: new Date().getTime()});
        localStorage.setItem('sc_achats_v36', JSON.stringify(achats));
        addLog(`ACHAT: ${motif} (-${mont}F)`);
        alert("Dépense enregistrée !"); location.reload();
    }
}

// ==========================================
// 5. ZONE TECHNIQUE & WHATSAPP
// ==========================================
function renderAtelier() {
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card ${c.retrait < today ? 'retard-card' : ''}">
            <b>#${c.id} - ${c.client}</b><br>
            <div class="solde-badge">À PERCEVOIR : ${c.total-c.paye} F</div><br>
            <button onclick="window.open('https://wa.me/237${c.tel}?text=Commande%20Prete')" style="background:#25D366; color:white; border:none; padding:8px; border-radius:10px; font-size:10px; margin-bottom:5px;">WHATSAPP PRÊT</button>
            <button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark);">LIVRER</button>
        </div>`).join('');
}
function livrer(id) {
    const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
    addLog(`Livraison #${id}`); localStorage.setItem('sc_db_v36', JSON.stringify(db)); renderAtelier();
}

// ==========================================
// 6. RAPPORTS & PERFORMANCE
// ==========================================
function renderRapport() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === today);
    let tot = tks.reduce((a,b)=>a+b.paye, 0), spent = ach.reduce((a,b)=>a+b.mont, 0);
    document.getElementById('rapport-details').innerHTML = `<div class="item-row"><span>Recettes :</span> <b>${tot} F</b></div><div class="item-row"><span>Achats :</span> <b>-${spent} F</b></div><div class="item-row" style="background:var(--success); color:white;"><span>NET :</span> <b>${tot-spent} F</b></div>`;
}

function envoyerRapportWA() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === today);
    let tot = tks.reduce((a,b)=>a+b.paye, 0), spent = ach.reduce((a,b)=>a+b.mont, 0);
    const msg = `📊 *RAPPORT SUPER CLEAN*\nAgent: ${currentUser.nom}\nTickets: ${tks.length}\nRecettes: ${tot}F\nAchats: ${spent}F\n💰 *NET: ${tot-spent}F*`;
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
    const spent = achats.reduce((a,b)=>a+b.mont, 0);
    document.getElementById('p-today').innerText = d+" F";
    document.getElementById('p-week').innerText = w+" F";
    document.getElementById('p-month').innerText = m+" F";
    document.getElementById('p-year').innerText = y+" F";
    document.getElementById('p-net').innerText = (db.reduce((a,b)=>a+b.paye,0)-spent)+" F";
}

// ==========================================
// 7. ADMIN, STOCKS & PERSISTANCE
// ==========================================
function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `
        <div style="display:flex; justify-content:space-between; align-items:center; padding:10px; border-bottom:1px solid #eee;">
            <span><b>${s.nom}</b> (${s.role}) <span class="status-pill ${s.actif?'active-pill':'blocked-pill'}">${s.actif?'ACTIF':'BLOC'}</span></span>
            ${i!==0?`<button onclick="staff[${i}].actif=!staff[${i}].actif;localStorage.setItem('sc_staff_v36',JSON.stringify(staff));renderAdmin()">${s.actif?'BLOQUER':'OUVRIR'}</button>`:''}
        </div>`).join('');
}
function renderStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-row"><span>${s.nom}</span> <b>${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}
function majStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks_v36', JSON.stringify(stocks)); renderStocks();
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]);
    if(btn) btn.classList.add('active');
    if(id==='section-atelier') renderAtelier();
    if(id==='section-rapport') renderRapport();
    if(id==='section-performance') calculerPerformance();
    if(id==='section-stocks') renderStocks();
    if(id==='section-admin') renderAdmin();
    if(id==='section-logs') document.getElementById('logs-container').innerHTML = logs.map(l => `<div class="log-row">${l}</div>`).join('');
}

function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif:true}); localStorage.setItem('sc_staff_v36', JSON.stringify(staff)); renderAdmin(); }
}
function exportData() {
    const b = new Blob([JSON.stringify({db, staff, stocks, achats, logs, GRILLE})], {type:"application/json"});
    const a = document.createElement('a'); a.href = URL.createObjectURL(b); a.download=`SuperClean_FullBackup.json`; a.click();
}
function importData() {
    const reader = new FileReader(); reader.onload = (e) => {
        const d = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v36', JSON.stringify(d.db));
        localStorage.setItem('sc_staff_v36', JSON.stringify(d.staff));
        localStorage.setItem('sc_stocks_v36', JSON.stringify(d.stocks));
        localStorage.setItem('sc_grille_v36', JSON.stringify(d.GRILLE));
        location.reload();
    }; reader.readAsText(document.getElementById('importFile').files[0]);
}
function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length>=8) { const f = db.find(x => x.tel && x.tel.includes(t)); if(f) document.getElementById('client-nom').value = f.client; }
}
function clearLogs() { if(confirm("Effacer ?")) { logs=[]; localStorage.setItem('sc_logs_v36', "[]"); tab('section-logs'); } }
</script>
</body>
</html>
