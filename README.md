<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v37.0 - SYSTÈME ABSOLU</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        /* --- CONFIGURATION DES COULEURS SUPER CLEAN --- */
        :root { 
            --primary: #0056b3; 
            --success: #2ecc71; 
            --danger: #ef233c; 
            --warning: #ff9f1c; 
            --dark: #1b2631; 
            --light: #f4f7f9; 
            --white: #ffffff;
            --radius: 20px;
        }

        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; }
        .hidden { display: none !important; }
        
        /* --- HEADER & IDENTITÉ --- */
        .header { background: var(--white); padding: 20px; text-align: center; border-bottom: 4px solid var(--primary); position: sticky; top: 0; z-index: 1000; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        .logo-main { width: 85px; height: 85px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 24px; margin-top: 10px; text-transform: uppercase; }
        .official-slogan { font-size: 13px; color: #555; font-style: italic; font-weight: 700; margin-top: 2px; }

        /* --- NAVIGATION BASSE SÉCURISÉE --- */
        .nav-bar { display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; padding: 12px; background: var(--white); position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000; }
        .nav-bar button { background: #f1f4f8; border: none; color: #7f8c8d; font-weight: 800; font-size: 7px; padding: 12px 1px; border-radius: 12px; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .nav-bar button.active { background: var(--primary); color: var(--white); transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 86, 179, 0.4); }

        /* --- COMPOSANTS INTERFACE --- */
        .card { background: var(--white); margin: 15px; padding: 25px; border-radius: var(--radius); box-shadow: 0 6px 20px rgba(0,0,0,0.06); border: 1px solid #eee; }
        h3 { margin-top: 0; font-size: 17px; color: var(--primary); font-weight: 900; text-transform: uppercase; border-bottom: 3px solid var(--light); padding-bottom: 12px; margin-bottom: 20px; }
        
        input, select, textarea { background: #f1f4f8; border: 2px solid transparent; padding: 16px; border-radius: 15px; margin-bottom: 12px; width: 100%; box-sizing: border-box; font-size: 15px; outline: none; transition: 0.3s; font-family: inherit; }
        input:focus { border-color: var(--primary); background: var(--white); }
        
        .btn-main { background: var(--primary); color: var(--white); border: none; padding: 20px; border-radius: 18px; font-weight: 900; width: 100%; cursor: pointer; text-transform: uppercase; letter-spacing: 1px; font-size: 14px; }
        .btn-wa { background: #25D366; color: var(--white); border: none; padding: 16px; border-radius: 15px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; display: flex; align-items: center; justify-content: center; font-size: 13px; }

        /* --- STATUTS ET BADGES --- */
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 15px; margin-bottom: 8px; border-left: 6px solid var(--primary); }
        .solde-badge { background: var(--danger); color: white; padding: 6px 12px; border-radius: 10px; font-size: 12px; font-weight: 900; display: inline-block; margin-bottom: 10px; }
        .status-pill { padding: 5px 12px; border-radius: 50px; font-size: 10px; font-weight: 900; text-transform: uppercase; }
        .pill-active { background: #dcfce7; color: #166534; }
        .pill-blocked { background: #fee2e2; color: #991b1b; }
        .retard-card { border: 2px solid var(--danger) !important; background: #fff5f5 !important; animation: blink-red 2s infinite; }
        @keyframes blink-red { 0%, 100% { opacity: 1; } 50% { opacity: 0.8; } }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 90%; max-width: 400px; text-align: center; padding: 50px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:110px; border-radius:50%; margin-bottom:20px; border: 5px solid #f1f4f8;">
            <h2 style="margin:0; letter-spacing:-1px;">SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:800; margin-bottom:35px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Identifiant (Login)">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">OUVRIR LA SESSION</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 10px 20px; font-size: 11px; display: flex; justify-content: space-between; align-items: center; font-weight: 600;">
        <span>👤 UTILISATEUR : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); color:white; border:none; padding:6px 12px; border-radius:8px; font-size:10px; font-weight:bold; cursor:pointer;">DÉCONNEXION</button>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 130px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION CLIENTS</h3>
                <input type="tel" id="tel-client" placeholder="WhatsApp Client (ex: 6...)" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <label style="font-size:11px; font-weight:800; color:#777; margin-left:5px;">DATE DE RETRAIT PRÉVUE :</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:20px; border-radius:18px; border:2px dashed #ddd; margin-top:10px;">
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">SÉLECTIONNER PRESTATION :</label>
                    <select id="select-article" onchange="majPrixInitial()"></select>
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">PRIX UNITAIRE APPLIQUÉ (F CFA) :</label>
                    <input type="number" id="prix-final" placeholder="Prix">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); margin-bottom:0;">+ AJOUTER AU TICKET</button>
                </div>
            </div>

            <div id="panier-liste" style="padding: 0 15px;"></div>

            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                    <span style="opacity:0.8; font-size: 14px;">TOTAL À PAYER :</span> 
                    <b id="total-caisse" style="font-size:30px; color:var(--success);">0 F</b>
                </div>
                <label style="font-size:11px; color:#aaa; font-weight:700;">MODE DE RÈGLEMENT :</label>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white;">
                    <option value="Cash">💵 Cash (Espèces)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN MoMo</option>
                </select>
                <label style="font-size:11px; color:#aaa; font-weight:700;">AVANCE ENCAISSÉE :</label>
                <input type="number" id="paye-caisse" placeholder="Montant" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning); font-size:18px;">Reste : 0 F</div>
            </div>

            <div style="padding: 0 15px;">
                <button onclick="validerCommande()" class="btn-main" style="height:80px; font-size:18px; box-shadow: 0 10px 20px rgba(0,86,179,0.3);">🖨️ VALIDER & IMPRIMER TICKET</button>
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
                <div id="rapport-details" style="margin-bottom: 25px;"></div>
                <button onclick="envoyerRapportWA()" class="btn-wa" style="height:70px; font-size:15px;">📲 ENVOYER LE BILAN AU GÉRANT</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE DES PERFORMANCES</h3>
                <div class="item-row"><span>Aujourd'hui</span> <b id="p-today">0 F</b></div>
                <div class="item-row"><span>Hebdomadaire (7j)</span> <b id="p-week">0 F</b></div>
                <div class="item-row"><span>Mensuel (Mois courant)</span> <b id="p-month">0 F</b></div>
                <div class="item-row"><span>Annuel (2026)</span> <b id="p-year">0 F</b></div>
                <div class="item-row" style="background:var(--success); color:white; padding:20px;">
                    <span style="font-weight:800;">BÉNÉFICE NET GLOBAL</span> 
                    <b id="p-net" style="font-size:22px;">0 F</b>
                </div>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px; font-weight: 900;">👕 ZONE TECHNIQUE - PRODUCTION</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3>🧼 INVENTAIRE DES CONSOMMABLES</h3>
                <div id="stock-display"></div>
                <hr style="margin:25px 0;">
                <h4>📦 AJUSTEMENT MANUEL DES STOCKS</h4>
                <select id="upd-stock-item"></select>
                <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter ou retirer (+/-)">
                <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">METTRE À JOUR L'INVENTAIRE</button>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 JOURNAL D'ACTIVITÉS (HISTORIQUE)</h3>
                <div id="logs-container" style="max-height:500px; overflow-y:auto; background:#f1f4f8; border-radius:15px; padding:15px; font-family:monospace; font-size:11px;"></div>
                <button onclick="clearLogs()" class="btn-main" style="background:var(--danger); margin-top:20px; font-size:11px; padding:10px;">EFFACER LE JOURNAL (ADMIN SEUL)</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card">
                <h3>👥 PERSONNEL & CONTRÔLE D'ACCÈS</h3>
                <div id="staff-list"></div>
                <hr style="margin:25px 0;">
                <h4 style="color:var(--primary);">➕ CRÉER UN NOUVEL ACCÈS PERSONNEL</h4>
                <input type="text" id="ns-nom" placeholder="Nom et Prénom">
                <input type="text" id="ns-log" placeholder="Identifiant (Login)">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Laveur">Laveur (Z.T)</option>
                    <option value="Repasseur (Z.T)">Repasseur</option>
                    <option value="Marketeur">Marketeur</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER ET ACTIVER LE COMPTE</button>
            </div>

            <div class="card">
                <h3>✨ AJOUTER UN NOUVEAU PRODUIT À LA GRILLE</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie (ex: ACCESSOIRES)">
                <input type="text" id="new-art-nom" placeholder="Nom du vêtement ou article">
                <input type="number" id="new-art-prix" placeholder="Prix standard F CFA">
                <button onclick="ajouterNouveauProduit()" class="btn-main" style="background:var(--primary);">AJOUTER À LA LISTE DE CAISSE</button>
            </div>
            
            <div class="card" style="text-align:center;">
                <h3>☁️ CLOUD BACKUP (DRIVE / ICLOUD)</h3>
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
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:75px; border-radius:50%;"><br>
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
// ==========================================
// 1. GRILLE TARIFAIRE INTÉGRALE DE L'IMAGE
// ==========================================
let GRILLE = JSON.parse(localStorage.getItem('sc_grille_v37')) || {
    "LES HAUTS": {
        "T-Shirt": 500, "Chemise": 500, "Polo": 500, "Pull Over": 700, "Blouson": 1500, "Démembré": 300, 
        "Veste 2 pièces": 1500, "Veste 3 pièces": 2000, "Pantalon simple": 500, "Pantalon jogging": 500, 
        "Pantalon jeans": 500, "Short": 500, "Jupe simple": 500, "Jupe plissée": 1000
    },
    "ENSEMBLES": {
        "Costume simple": 2000, "Costume de luxe": 2500, "Boubou femme": 1500, "Boubou homme 2P": 1500, 
        "Boubou homme 3P": 2500, "Pyjama 2 pièces": 1000, "Jogging": 1500
    },
    "LES ROBES": {
        "Robe simple": 1000, "Robe de soirée": 3000, "Kaba court": 1000, "Kaba long": 1500, "Toge (Magistrat/Avocat/Enseignant)": 3500
    },
    "LINGES DE LIT": {
        "1 Drap": 750, "Ensemble 1 drap+taie": 1500, "Ensemble 2 drap+taies": 2000, "Couvre lit / Couette": 2000, "Housse de couette": 2500, "Couverture": 4000
    },
    "VÊTEMENTS ENFANTS": {
        "Ensemble enfant": 1000, "Robe enfant": 500, "Haut enfant": 500, "Bas enfant": 500
    },
    "AMEUBLEMENT / BAIN": {
        "Rideau lourd": 1500, "Rideau léger": 1000, "Petite Serviette": 500, "Grande Serviette": 1000, "Peignoir": 2000
    },
    "ACCESSOIRES": {
        "Tennis": 500, "Cravate": 500
    }
};

// ==========================================
// 2. ÉTATS ET PERSISTANCE
// ==========================================
let db = JSON.parse(localStorage.getItem('sc_db_v37')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v37')) || [
    {nom: "Administrateur", login: "admin", pass: "0000", role: "ADMIN", actif: true}
];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v37')) || [
    {id:'savon', nom:'Savon Liquide', qte:100, unite:'L', mini:10},
    {id:'parfum', nom:'Parfum Textile', qte:30, unite:'L', mini:5},
    {id:'housses', nom:'Housses Plastiques', qte:500, unite:'pcs', mini:50}
];
let logs = JSON.parse(localStorage.getItem('sc_logs_v37')) || [];
let achats = JSON.parse(localStorage.getItem('sc_achats_v37')) || [];
let panier = [], currentUser = null;

// ==========================================
// 3. SÉCURITÉ, AUTH & LOGIN EXCLUSIF
// ==========================================
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return alert("🚫 ACCÈS RÉVOQUÉ : Votre compte est bloqué par l'administrateur.");
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        addLog("Connexion à la session");
        filtrerRoles();
        initDropdownArticles();
    } else alert("Identifiants incorrects !");
}

function filtrerRoles() {
    const r = currentUser.role;
    const allowsCaisse = (r === "ADMIN" || r === "Caissière");
    
    // Verrouillage Navigation
    document.getElementById('btn-caisse').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-rapport').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-performance').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-logs').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-admin').classList.toggle('hidden', r !== "ADMIN");
    
    tab(allowsCaisse ? 'section-caisse' : 'section-atelier');
}

function addLog(action) {
    logs.unshift(`[${new Date().toLocaleString('fr-FR')}] ${currentUser.nom} : ${action}`);
    localStorage.setItem('sc_logs_v37', JSON.stringify(logs.slice(0, 1000)));
}

// ==========================================
// 4. LOGIQUE CAISSE & ACHATS
// ==========================================
function initDropdownArticles() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Choisir un Article --</option>';
    for(let cat in GRILLE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in GRILLE[cat]) {
            let o = document.createElement('option'); o.value = GRILLE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        sel.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(s => `<option value="${s.id}">${s.nom}</option>`).join('');
}

function majPrixInitial() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }

function ajouterAuPanier() {
    const s = document.getElementById('select-article'), p = document.getElementById('prix-final').value;
    if(!s.value || !p) return alert("Sélectionnez un produit.");
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p), pret: false });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b)=>a+b.prix, 0);
    document.getElementById('total-caisse').innerText = t + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`
        <div class="item-row">
            <b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none; font-weight:900; margin-left:10px;">✕</button></span>
        </div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `RESTE À PERCEVOIR : ${t - p} F CFA`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, dr = document.getElementById('date-retrait').value, tel = document.getElementById('tel-client').value;
    if(!nom || !dr || panier.length === 0) return alert("Fiche client incomplète.");
    
    // Déduction auto stocks (0.05 savon / 1 housse par article)
    stocks[0].qte -= (panier.length * 0.05);
    stocks[2].qte -= panier.length;
    localStorage.setItem('sc_stocks_v37', JSON.stringify(stocks));

    const cmd = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0,
        mode: document.getElementById('mode-pay').value, retrait: dr, agent: currentUser.nom,
        timestamp: new Date().getTime(), statut: "En cours"
    };

    db.unshift(cmd); localStorage.setItem('sc_db_v37', JSON.stringify(db));
    addLog(`Encaissement Ticket #${cmd.id} (${cmd.total}F)`);
    imprimerTicket(cmd);
    location.reload();
}

function imprimerTicket(c) {
    document.getElementById('tk-body').innerHTML = `
        TICKET N° ${c.id}<br>DATE: ${new Date().toLocaleDateString()}<br>CLIENT: ${c.client.toUpperCase()}<br>TEL: ${c.tel}<hr>
        ${c.articles.map(a => `${a.nom.padEnd(18)} ${a.prix}F`).join('<br>')}<hr>
        TOTAL: ${c.total} F<br>PAYÉ: ${c.paye} F (${c.mode})<br><b>SOLDE: ${c.total - c.paye} F</b><hr>RDV: ${c.retrait}
    `;
    window.print();
}

function enregistrerAchat() {
    const motif = document.getElementById('achat-motif').value, mont = parseInt(document.getElementById('achat-montant').value);
    if(motif && mont) {
        achats.push({motif, mont, agent: currentUser.nom, timestamp: new Date().getTime()});
        localStorage.setItem('sc_achats_v37', JSON.stringify(achats));
        addLog(`SORTIE DE CAISSE : ${motif} (-${mont}F)`);
        alert("Dépense enregistrée et déduite.");
        location.reload();
    }
}

// ==========================================
// 5. RAPPORT WHATSAPP JOURNALIER
// ==========================================
function renderRapport() {
    const todayStr = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === todayStr);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === todayStr);
    
    let cash = 0, om = 0, momo = 0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    let totalAchats = ach.reduce((a,b)=>a+b.mont, 0);

    document.getElementById('rapport-details').innerHTML = `
        <div class="item-row"><span>Tickets créés aujourd'hui :</span> <b>${tks.length}</b></div>
        <div class="item-row"><span>Encaissement Cash :</span> <b>${cash} F</b></div>
        <div class="item-row"><span>Encaissement Orange Money :</span> <b>${om} F</b></div>
        <div class="item-row"><span>Encaissement MTN MoMo :</span> <b>${momo} F</b></div>
        <div class="item-row" style="color:var(--danger)"><span>Dépenses de caisse :</span> <b>-${totalAchats} F</b></div>
        <div class="item-row" style="background:var(--success); color:white; font-size:18px;"><span>NET À REMETTRE :</span> <b>${cash+om+momo-totalAchats} F CFA</b></div>
    `;
}

function envoyerRapportWA() {
    const todayStr = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === todayStr);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === todayStr);
    let cash=0, om=0, momo=0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    let totalAchats = ach.reduce((a,b)=>a+b.mont, 0);
    
    const msg = `📊 *RAPPORT JOURNALIER SUPER CLEAN*\n📅 Date : ${new Date().toLocaleDateString()}\n👤 Agent : ${currentUser.nom}\n------------------\n🎟️ Tickets : ${tks.length}\n💵 Cash : ${cash}F\n🍊 Orange : ${om}F\n🟡 MoMo : ${momo}F\n❌ Achats : -${totalAchats}F\n------------------\n💰 *NET À REMETTRE : ${cash+om+momo-totalAchats}F*`;
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
}

// ==========================================
// 6. ZONE TECHNIQUE & WHATSAPP CLIENT
// ==========================================
function renderAtelier() {
    const today = new Date().toISOString().split('T')[0];
    const isCaisse = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card ${c.retrait < today ? 'retard-card' : ''}">
            <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b> <small>${c.retrait}</small></div>
            <div class="solde-badge">À PERCEVOIR : ${c.total-c.paye} F</div><br>
            ${c.articles.map(a => `
                <div class="item-row">
                    <span>${a.nom}</span>
                    ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:5px 12px; border-radius:8px;">FINI</button>` : `<span style="font-weight:900; color:green;">📍 ${a.loc || 'PRÊT'}</span>`}
                </div>`).join('')}
            ${isCaisse ? `<button class="btn-wa" onclick="window.open('https://wa.me/237${c.tel}?text=Bonjour%20votre%20commande%20SUPER%20CLEAN%20est%20prete')">📲 WHATSAPP : DISPONIBLE</button>` : ''}
            <button onclick="livrer(${c.id})" class="btn-main" style="margin-top:10px; background:var(--dark);">CONFIRMER LIVRAISON FINALE</button>
        </div>`).join('');
}

function finirArt(cid, auid) {
    const loc = prompt("📍 Emplacement (Casier) ?");
    const c = db.find(x => x.id === cid), a = c.articles.find(x => x.uid === auid);
    a.pret = true; a.loc = loc;
    addLog(`Vêtement prêt : ${a.nom} (#${cid})`);
    localStorage.setItem('sc_db_v37', JSON.stringify(db)); renderAtelier();
}

function livrer(id) {
    if(confirm("Confirmer la livraison et l'encaissement total ?")) {
        const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
        addLog(`Livraison finale Ticket #${id}`);
        localStorage.setItem('sc_db_v37', JSON.stringify(db)); renderAtelier();
    }
}

// ==========================================
// 7. PERFORMANCE & ANALYTICS
// ==========================================
function calculerPerformance() {
    const now = new Date();
    const oneDay = 24 * 60 * 60 * 1000;
    let d=0, w=0, m=0, y=0;
    db.forEach(t => {
        const dt = new Date(t.timestamp);
        if(dt.toDateString() === now.toDateString()) d+=t.paye;
        if(now - dt <= 7*oneDay) w+=t.paye;
        if(dt.getMonth() === now.getMonth() && dt.getFullYear() === now.getFullYear()) m+=t.paye;
        if(dt.getFullYear() === now.getFullYear()) y+=t.paye;
    });
    document.getElementById('p-today').innerText = d+" F";
    document.getElementById('p-week').innerText = w+" F";
    document.getElementById('p-month').innerText = m+" F";
    document.getElementById('p-year').innerText = y+" F";
    document.getElementById('p-net').innerText = (db.reduce((a,b)=>a+b.paye,0) - achats.reduce((a,b)=>a+b.mont,0)) + " F CFA";
}

// ==========================================
// 8. ADMIN EXCLUSIF & PERSONNEL
// ==========================================
function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `
        <div style="display:flex; justify-content:space-between; padding:15px; border-bottom:1px solid #eee;">
            <span><b>${s.nom}</b> (${s.role}) <span class="status-tag ${s.actif?'pill-active':'pill-blocked'}">${s.actif?'ACTIF':'BLOQUÉ'}</span></span>
            ${i!==0 ? `<button onclick="toggleAccess(${i})" style="font-size:10px; padding:8px; border-radius:8px; background:var(--dark); color:white; border:none;">BASCULER ACCÈS</button>` : ''}
        </div>`).join('');
}

function toggleAccess(i) { staff[i].actif = !staff[i].actif; localStorage.setItem('sc_staff_v37', JSON.stringify(staff)); renderAdmin(); addLog(`Changement statut accès pour ${staff[i].nom}`); }

function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif:true}); localStorage.setItem('sc_staff_v37', JSON.stringify(staff)); addLog(`Création compte : ${n}`); renderAdmin(); }
}

function ajouterNouveauProduit() {
    const cat = document.getElementById('new-art-cat').value.toUpperCase();
    const nom = document.getElementById('new-art-nom').value;
    const prix = parseInt(document.getElementById('new-art-prix').value);
    if(cat && nom && prix) {
        if(!GRILLE[cat]) GRILLE[cat] = {};
        GRILLE[cat][nom] = prix;
        localStorage.setItem('sc_grille_v37', JSON.stringify(GRILLE));
        alert("Produit ajouté !"); initDropdownArticles();
    }
}

// ==========================================
// 9. STOCKS & NAVIGATION
// ==========================================
function renderStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-row"><span>${s.nom}</span> <b>${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}
function majStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks_v37', JSON.stringify(stocks)); renderStocks();
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
    if(id==='section-logs') document.getElementById('logs-container').innerHTML = logs.map(l => `<div>${l}</div>`).join('');
}

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length>=8) { const f = db.find(x => x.tel && x.tel.includes(t)); if(f) document.getElementById('client-nom').value = f.client; }
}

function clearLogs() { if(confirm("Effacer ?")) { logs=[]; localStorage.setItem('sc_logs_v37', "[]"); tab('section-logs'); } }

function exportData() {
    const b = new Blob([JSON.stringify({db, staff, stocks, achats, logs, GRILLE})], {type:"application/json"});
    const a = document.createElement('a'); a.href = URL.createObjectURL(b); a.download=`SUPER_CLEAN_ABSOLUTE_BACKUP.json`; a.click();
}

function importData() {
    const reader = new FileReader(); reader.onload = (e) => {
        const d = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v37', JSON.stringify(d.db));
        localStorage.setItem('sc_staff_v37', JSON.stringify(d.staff));
        localStorage.setItem('sc_stocks_v37', JSON.stringify(d.stocks));
        localStorage.setItem('sc_grille_v37', JSON.stringify(d.GRILLE));
        location.reload();
    }; reader.readAsText(document.getElementById('importFile').files[0]);
}
</script>
</body>
</html>
