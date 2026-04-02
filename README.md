<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING ERP v34.0 - L'INTÉGRALE ABSOLUE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        /* ==========================================
           1. CHARTE GRAPHIQUE SUPER CLEAN PRESSING
           ========================================== */
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

        body { 
            font-family: 'Inter', sans-serif; 
            margin: 0; 
            background-color: var(--light); 
            color: var(--dark); 
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        .hidden { display: none !important; }

        /* ==========================================
           2. HEADER & BRANDING
           ========================================== */
        .header { 
            background: var(--white); 
            padding: 20px; 
            text-align: center; 
            border-bottom: 3px solid var(--primary); 
            position: sticky; 
            top: 0; 
            z-index: 1000;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }
        .logo-main { width: 75px; height: 75px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 22px; margin-top: 10px; letter-spacing: -0.5px; }
        .official-slogan { font-size: 13px; color: #555; font-style: italic; font-weight: 600; margin-top: 2px; }

        /* ==========================================
           3. NAVIGATION BASSE (7 ONGLETS)
           ========================================== */
        .nav-bar { 
            display: grid; 
            grid-template-columns: repeat(7, 1fr); 
            gap: 2px; 
            padding: 10px; 
            background: var(--white); 
            position: fixed; 
            bottom: 0; 
            width: 100%; 
            border-top: 1px solid #ddd; 
            box-sizing: border-box; 
            z-index: 2000;
        }

        .nav-bar button { 
            background: #f8f9fa; 
            border: none; 
            color: #7f8c8d; 
            font-weight: 800; 
            font-size: 6px; 
            padding: 10px 1px; 
            border-radius: 10px; 
            cursor: pointer; 
            text-transform: uppercase;
            transition: all 0.3s ease;
        }

        .nav-bar button.active { 
            background: var(--primary); 
            color: var(--white); 
            transform: translateY(-3px);
            box-shadow: 0 5px 10px rgba(0, 86, 179, 0.3);
        }

        /* ==========================================
           4. COMPOSANTS UI (CARDS, BTN, INPUTS)
           ========================================== */
        .card { 
            background: var(--white); 
            margin: 12px; 
            padding: 22px; 
            border-radius: var(--radius); 
            box-shadow: 0 4px 15px rgba(0,0,0,0.06); 
            border: 1px solid #eee;
            position: relative;
        }

        h3 { margin-top: 0; font-size: 15px; color: var(--primary); font-weight: 800; text-transform: uppercase; border-bottom: 2px solid var(--light); padding-bottom: 10px; margin-bottom: 15px; }

        input, select, textarea { 
            background: #f1f4f8; 
            border: 2px solid transparent; 
            padding: 14px; 
            border-radius: 14px; 
            margin-bottom: 10px; 
            width: 100%; 
            box-sizing: border-box; 
            font-size: 14px; 
            font-family: inherit;
            outline: none;
            transition: 0.3s;
        }

        input:focus { border-color: var(--primary); background: var(--white); }

        .btn-main { 
            background: var(--primary); 
            color: var(--white); 
            border: none; 
            padding: 18px; 
            border-radius: 16px; 
            font-weight: 800; 
            width: 100%; 
            cursor: pointer; 
            text-transform: uppercase; 
            transition: 0.2s;
        }

        .btn-wa { 
            background: #25D366; 
            color: var(--white); 
            border: none; 
            padding: 15px; 
            border-radius: 15px; 
            font-weight: 800; 
            width: 100%; 
            cursor: pointer;
            text-transform: uppercase;
        }

        /* ==========================================
           5. BADGES & ETATS
           ========================================== */
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f8f9fa; border-radius: 12px; margin-bottom: 6px; border-left: 5px solid var(--primary); font-size: 13px; }
        .solde-badge { background: var(--danger); color: white; padding: 5px 10px; border-radius: 8px; font-size: 11px; font-weight: 900; }
        .stat-box { background: #f8fbff; padding: 15px; border-radius: 15px; border-left: 5px solid var(--primary); margin-bottom: 10px; }
        .log-row { font-size: 10px; padding: 8px; border-bottom: 1px solid #eee; font-family: monospace; background: #fafafa; }
        .retard-card { border: 2px solid var(--danger) !important; background: #fff5f5 !important; animation: blink-red 2s infinite; }
        @keyframes blink-red { 0%, 100% { opacity: 1; } 50% { opacity: 0.7; } }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 85%; max-width: 380px; text-align: center; padding: 40px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:100px; border-radius:50%; margin-bottom:15px; border: 4px solid #f1f4f8;">
            <h2 style="margin:0; letter-spacing:-1px;">SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:700; margin-bottom:25px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Identifiant Personnel">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">OUVRIR LA SESSION</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 8px 15px; font-size: 10px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 CONNECTÉ : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); color:white; border:none; padding:4px 8px; border-radius:5px; font-size:9px; cursor:pointer; font-weight:bold;">DECONNEXION</button>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 120px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION & ENCAISSEMENT</h3>
                <input type="tel" id="tel-client" placeholder="WhatsApp Client (ex: 6...)" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <label style="font-size:10px; font-weight:900; color:#777; margin-left:5px;">DATE DE RETRAIT PRÉVUE :</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:15px; border-radius:15px; border:1px dashed #ddd; margin-top:10px;">
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">ARTICLE À AJOUTER :</label>
                    <select id="select-article" onchange="majPrixSuggere()"></select>
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">PRIX À APPLIQUER (F CFA) :</label>
                    <input type="number" id="prix-final" placeholder="Prix">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); margin:0;">+ AJOUTER AU TICKET</button>
                </div>
            </div>

            <div id="panier-liste" style="padding: 0 15px;"></div>

            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                    <span style="opacity:0.8;">TOTAL PRESTATIONS :</span> <b id="total-caisse" style="font-size:26px; color:var(--success);">0 F</b>
                </div>
                <label style="font-size:10px; color:#aaa;">MODE DE RÈGLEMENT :</label>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white;">
                    <option value="Cash">💵 Cash (Espèces)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN MoMo</option>
                </select>
                <label style="font-size:10px; color:#aaa;">MONTANT DE L'AVANCE :</label>
                <input type="number" id="paye-caisse" placeholder="Avance versée" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning); font-size:16px;">Reste à percevoir : 0 F</div>
            </div>

            <div style="padding: 0 15px;">
                <button onclick="validerCommande()" class="btn-main" style="height:70px; font-size:16px;">🖨️ VALIDER & IMPRIMER TICKET</button>
            </div>

            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSES DE CAISSE (ACHATS)</h3>
                <input type="text" id="achat-motif" placeholder="Motif de la dépense (ex: Savon, Charbon...)">
                <input type="number" id="achat-montant" placeholder="Montant F CFA">
                <button onclick="enregistrerAchat()" class="btn-main" style="background:var(--danger);">DÉDUIRE DE LA RECETTE</button>
            </div>
        </div>

        <div id="section-rapport" class="tab-content hidden">
            <div class="card">
                <h3>📊 RAPPORT JOURNALIER WHATSAPP</h3>
                <div id="rapport-details" style="margin-bottom: 20px;"></div>
                <button onclick="envoyerRapportWA()" class="btn-wa">📲 ENVOYER LE BILAN AU GÉRANT</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE DES PERFORMANCES</h3>
                <div class="stat-box"><div class="stat-label">Aujourd'hui</div><div id="p-today" class="stat-value">0 F</div></div>
                <div class="stat-box"><div class="stat-label">Hebdomadaire (7 derniers jours)</div><div id="p-week" class="stat-value">0 F</div></div>
                <div class="stat-box"><div class="stat-label">Mensuel (Mois en cours)</div><div id="p-month" class="stat-value">0 F</div></div>
                <div class="stat-box"><div class="stat-label">Annuel (Année civile)</div><div id="p-year" class="stat-value">0 F</div></div>
                <div class="stat-box" style="border-left-color: var(--success); background: #f0fff4;">
                    <div class="stat-label">BÉNÉFICE NET GLOBAL</div>
                    <div id="p-net" class="stat-value" style="color:var(--success); font-size:24px;">0 F</div>
                </div>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px; font-weight: 800;">👕 ZONE TECHNIQUE - SUIVI PRODUCTION</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3>🧼 INVENTAIRE DES PRODUITS</h3>
                <div id="stock-display"></div>
                
                <div id="admin-stock-ui" class="hidden">
                    <hr style="margin:20px 0; border:0; border-top:1px solid #eee;">
                    <h4>📦 AJUSTEMENT MANUEL DES STOCKS</h4>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter (+/-)">
                    <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">METTRE À JOUR</button>
                </div>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 JOURNAL D'ACTIVITÉS (LOGS)</h3>
                <div id="logs-container" style="max-height:500px; overflow-y:auto; background:#f1f4f8; border-radius:10px; padding:10px;"></div>
                <button onclick="clearLogs()" class="btn-main" style="background:var(--danger); margin-top:15px; font-size:10px; padding:10px;">EFFACER LE JOURNAL (ADMIN)</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card">
                <h3>👥 GESTION DU PERSONNEL & ACCÈS</h3>
                <div id="staff-list"></div>
                <hr>
                <h4>CRÉER UN ACCÈS EMPLOYÉ</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Login">
                <input type="password" id="ns-pass" placeholder="Password">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Zone Technique">Zone Technique</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER ET ACTIVER LE COMPTE</button>
            </div>
            
            <div class="card" style="text-align:center;">
                <h3>☁️ CLOUD BACKUP</h3>
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 EXPORTER VERS DRIVE/ICLOUD</button>
                <input type="file" id="importFile" accept=".json" style="margin-top:20px;">
                <button onclick="importData()" class="btn-main" style="background:var(--success);">📥 RESTAURER SAUVEGARDE</button>
            </div>
        </div>
    </div>

    <nav class="nav-bar">
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
/* ==========================================
   8. GRILLE TARIFAIRE INTÉGRALE (ZÉRO SIMPLIFICATION)
   ========================================== */
const GRILLE_OFFICIELLE = {
    "LES HAUTS": {
        "T-Shirt": 500, "Chemise": 500, "Polo": 500, "Pull Over": 700, "Blouson": 1500, "Démembré": 300, "Veste 2 pièces": 1500, "Veste 3 pièces": 2000,
        "Pantalon simple": 500, "Pantalon jogging": 500, "Pantalon jeans": 500, "Short": 500, "Jupe simple": 500, "Jupe plissée": 1000
    },
    "ENSEMBLES": {
        "Costume simple": 2000, "Costume de luxe": 2500, "Boubou femme": 1500, "Boubou homme 2 pièces": 1500, "Boubou homme 3 pièces": 2500, "Pyjama 2 pièces": 1000, "Jogging": 1500
    },
    "LES ROBES": {
        "Robe simple": 1000, "Robe de soirée": 3000, "Kaba court": 1000, "Kaba long": 1500, "Toge (Magistrat/Avocat/Enseignant)": 3500
    },
    "LINGES DE LIT": {
        "1 Drap": 750, "Ensemble 1 drap+taies": 1500, "Ensemble 2 drap+taies": 2000, "Couvre lit ou Couette": 2000, "Housse de couette": 2500, "Couverture": 4000
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

/* ==========================================
   9. PERSISTANCE ET VARIABLES D'ÉTAT
   ========================================== */
let db = JSON.parse(localStorage.getItem('sc_db_v34')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v34')) || [
    {nom: "Administrateur", login: "admin", pass: "0000", role: "ADMIN", actif: true}
];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v34')) || [
    {id:'savon', nom:'Savon Liquide', qte:50, unite:'L', mini:5},
    {id:'parfum', nom:'Parfum Textile', qte:10, unite:'L', mini:2},
    {id:'housses', nom:'Housses Plastiques', qte:100, unite:'pcs', mini:10}
];
let logs = JSON.parse(localStorage.getItem('sc_logs_v34')) || [];
let achats = JSON.parse(localStorage.getItem('sc_achats_v34')) || [];
let panier = [], currentUser = null;

/* ==========================================
   10. SÉCURITÉ ET LOGIN
   ========================================== */
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return alert("🚫 ACCÈS RÉVOQUÉ PAR L'ADMIN.");
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        addLog("Connexion session");
        filtrerRoles();
        initDropdown();
    } else alert("Identifiants incorrects !");
}

function filtrerRoles() {
    const r = currentUser.role;
    const hasCaisse = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').classList.toggle('hidden', !hasCaisse);
    document.getElementById('btn-rapport').classList.toggle('hidden', !hasCaisse);
    document.getElementById('btn-performance').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-logs').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-admin').classList.toggle('hidden', r !== "ADMIN");
    if(r === "ADMIN") document.getElementById('admin-stock-ui').classList.remove('hidden');
    tab(hasCaisse ? 'section-caisse' : 'section-atelier');
}

function addLog(action) {
    logs.unshift(`[${new Date().toLocaleString('fr-FR')}] ${currentUser.nom} : ${action}`);
    localStorage.setItem('sc_logs_v34', JSON.stringify(logs.slice(0, 1000)));
}

/* ==========================================
   11. LOGIQUE CAISSE & ACHATS
   ========================================== */
function initDropdown() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Choisir Article --</option>';
    for(let cat in GRILLE_OFFICIELLE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in GRILLE_OFFICIELLE[cat]) {
            let o = document.createElement('option'); o.value = GRILLE_OFFICIELLE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        sel.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(s => `<option value="${s.id}">${s.nom}</option>`).join('');
}

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
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`
        <div class="item-row">
            <b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none; font-weight:900; margin-left:10px;">✕</button></span>
        </div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste à percevoir : ${t - p} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, dr = document.getElementById('date-retrait').value, tel = document.getElementById('tel-client').value;
    if(!nom || !dr || panier.length === 0) return alert("Champs client incomplets !");
    
    // Déduction auto stocks (0.05 savon par vêtement)
    stocks[0].qte -= (panier.length * 0.05);
    localStorage.setItem('sc_stocks_v34', JSON.stringify(stocks));

    const cmd = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0,
        mode: document.getElementById('mode-pay').value, retrait: dr, agent: currentUser.nom,
        timestamp: new Date().getTime(), statut: "En cours"
    };

    db.unshift(cmd); localStorage.setItem('sc_db_v34', JSON.stringify(db));
    addLog(`Encaissement Ticket #${cmd.id} (${cmd.total}F)`);
    imprimerTicket(cmd);
    location.reload();
}

function imprimerTicket(c) {
    document.getElementById('tk-body').innerHTML = `
        TICKET N° ${c.id}<br>DATE: ${new Date().toLocaleDateString()}<br>CLIENT: ${c.client.toUpperCase()}<hr>
        ${c.articles.map(a => `${a.nom.padEnd(18)} ${a.prix}F`).join('<br>')}<hr>
        TOTAL: ${c.total} F<br>PAYÉ: ${c.paye} F (${c.mode})<br><b>SOLDE: ${c.total - c.paye} F</b><hr>RDV: ${c.retrait}
    `;
    window.print();
}

function enregistrerAchat() {
    const motif = document.getElementById('achat-motif').value, mont = parseInt(document.getElementById('achat-montant').value);
    if(motif && mont) {
        achats.push({motif, mont, agent: currentUser.nom, timestamp: new Date().getTime()});
        localStorage.setItem('sc_achats_v34', JSON.stringify(achats));
        addLog(`SORTIE CAISSE : ${motif} (-${mont}F)`);
        alert("Dépense enregistrée !"); location.reload();
    }
}

/* ==========================================
   12. RAPPORT WHATSAPP JOURNALIER
   ========================================== */
function renderRapport() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === today);
    
    let cash = 0, om = 0, momo = 0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    let totalAchats = ach.reduce((a,b)=>a+b.mont, 0);

    document.getElementById('rapport-details').innerHTML = `
        <div class="item-row"><span>Tickets créés :</span> <b>${tks.length}</b></div>
        <div class="item-row"><span>Recette Cash :</span> <b>${cash} F</b></div>
        <div class="item-row"><span>Recette Orange Money :</span> <b>${om} F</b></div>
        <div class="item-row"><span>Recette MTN Momo :</span> <b>${momo} F</b></div>
        <div class="item-row" style="color:red"><span>Total Dépenses :</span> <b>-${totalAchats} F</b></div>
        <div class="item-row" style="background:var(--success); color:white;"><span>NET EN CAISSE :</span> <b>${cash+om+momo-totalAchats} F</b></div>
    `;
}

function envoyerRapportWA() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === today);
    let cash=0, om=0, momo=0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    let totalAchats = ach.reduce((a,b)=>a+b.mont, 0);
    
    const msg = `📊 *RAPPORT JOURNALIER SUPER CLEAN*\n📅 Date : ${new Date().toLocaleDateString()}\n👤 Agent : ${currentUser.nom}\n------------------\n🎟️ Tickets : ${tks.length}\n💵 Cash : ${cash}F\n🍊 OM : ${om}F\n🟡 MoMo : ${momo}F\n❌ Achats : -${totalAchats}F\n------------------\n💰 *NET : ${cash+om+momo-totalAchats}F*`;
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
    addLog("Envoi rapport WhatsApp");
}

/* ==========================================
   13. ANALYSE PERFORMANCE (STATISTIQUES)
   ========================================== */
function calculerPerformance() {
    const now = new Date();
    const oneDay = 24 * 60 * 60 * 1000;
    let totalToday = 0, totalWeek = 0, totalMonth = 0, totalYear = 0;
    
    db.forEach(t => {
        const d = new Date(t.timestamp);
        if(d.toDateString() === now.toDateString()) totalToday += t.paye;
        if(now - d <= 7 * oneDay) totalWeek += t.paye;
        if(d.getMonth() === now.getMonth() && d.getFullYear() === now.getFullYear()) totalMonth += t.paye;
        if(d.getFullYear() === now.getFullYear()) totalYear += t.paye;
    });

    document.getElementById('p-today').innerText = totalToday + " F";
    document.getElementById('p-week').innerText = totalWeek + " F";
    document.getElementById('p-month').innerText = totalMonth + " F";
    document.getElementById('p-year').innerText = totalYear + " F";
    document.getElementById('p-net').innerText = (db.reduce((a,b)=>a+b.paye,0) - achats.reduce((a,b)=>a+b.mont,0)) + " F CFA";
}

/* ==========================================
   14. ZONE TECHNIQUE & STOCKS
   ========================================== */
function renderAtelier() {
    const today = new Date().toISOString().split('T')[0];
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card ${c.retrait < today ? 'retard-card' : ''}">
            <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b><small>${c.retrait}</small></div>
            <div class="solde-badge">À PERCEVOIR : ${c.total-c.paye} F</div><br>
            ${c.articles.map(a => `
                <div class="item-row">
                    <span>${a.nom}</span>
                    ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:4px 8px; border-radius:5px;">OK</button>` : `<span style="font-weight:900; color:green;">📍 ${a.loc || 'PRÊT'}</span>`}
                </div>`).join('')}
            <button onclick="livrer(${c.id})" class="btn-main" style="margin-top:10px; background:var(--dark);">LIVRER LA COMMANDE</button>
        </div>`).join('');
}

function finirArt(cid, auid) {
    const loc = prompt("📍 Emplacement du vêtement (ex: Casier A1) ?");
    const c = db.find(x => x.id === cid), a = c.articles.find(x => x.uid === auid);
    a.pret = true; a.loc = loc;
    localStorage.setItem('sc_db_v34', JSON.stringify(db)); renderAtelier();
}

function livrer(id) {
    if(confirm("Confirmer la livraison et le solde payé ?")) {
        const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
        addLog(`LIVRAISON : Ticket #${id}`);
        localStorage.setItem('sc_db_v34', JSON.stringify(db)); renderAtelier();
    }
}

function renderStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div class="item-row" style="border-left-color: ${s.qte < s.mini ? 'red' : 'green'}">
            <span>${s.nom}</span> <b>${s.qte.toFixed(2)} ${s.unite}</b>
        </div>`).join('');
}

function majStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    const s = stocks.find(x => x.id === id); s.qte += q;
    addLog(`AJUSTEMENT STOCK : ${s.nom} (${q})`);
    localStorage.setItem('sc_stocks_v34', JSON.stringify(stocks)); renderStocks();
}

/* ==========================================
   15. ADMIN & UTILITAIRES
   ========================================= */
function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btnId = 'btn-' + id.split('-')[1];
    if(document.getElementById(btnId)) document.getElementById(btnId).classList.add('active');
    
    if(id==='section-atelier') renderAtelier();
    if(id==='section-rapport') renderRapport();
    if(id==='section-performance') calculerPerformance();
    if(id==='section-stocks') renderStocks();
    if(id==='section-logs') document.getElementById('logs-container').innerHTML = logs.map(l => `<div class="log-row">${l}</div>`).join('');
    if(id==='section-admin') renderAdmin();
}

function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `
        <div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;">
            <span><b>${s.nom}</b> (${s.role}) - ${s.actif?'ACTIF':'BLOQUÉ'}</span>
            ${i!==0 ? `<button onclick="toggleAccess(${i})" style="font-size:10px; padding:5px; background:var(--dark); color:white; border:none; border-radius:5px;">BASCULER ACCÈS</button>` : ''}
        </div>`).join('');
}

function toggleAccess(i) { staff[i].actif = !staff[i].actif; localStorage.setItem('sc_staff_v34', JSON.stringify(staff)); renderAdmin(); }

function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif:true}); localStorage.setItem('sc_staff_v34', JSON.stringify(staff)); addLog(`Création compte : ${n}`); renderAdmin(); }
}

function exportData() {
    const blob = new Blob([JSON.stringify({db, staff, stocks, achats, logs})], {type:"application/json"});
    const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download=`SuperClean_Full_Backup.json`; a.click();
}

function importData() {
    const reader = new FileReader();
    reader.onload = (e) => {
        const d = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v34', JSON.stringify(d.db));
        localStorage.setItem('sc_staff_v34', JSON.stringify(d.staff));
        localStorage.setItem('sc_stocks_v34', JSON.stringify(d.stocks));
        localStorage.setItem('sc_logs_v34', JSON.stringify(d.logs));
        location.reload();
    };
    reader.readAsText(document.getElementById('importFile').files[0]);
}

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length>=8) { const f = db.find(x => x.tel && x.tel.includes(t)); if(f) document.getElementById('client-nom').value = f.client; }
}

function clearLogs() { if(confirm("Effacer tout le journal d'activités ?")) { logs=[]; localStorage.setItem('sc_logs_v34', "[]"); tab('section-logs'); } }
</script>
</body>
</html>
