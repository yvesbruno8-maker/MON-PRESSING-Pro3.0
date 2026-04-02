<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v43.0 - SYSTÈME DE GESTION INTÉGRAL</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        /* ==========================================================================
           CHARTE GRAPHIQUE ET VARIABLES
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
           AUTHENTIFICATION (LOGIN)
           ========================================================================== */
        #auth-screen { 
            height: 100vh; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%); 
        }

        .login-card { 
            background: var(--white); 
            padding: 40px; 
            border-radius: 30px; 
            width: 90%; 
            max-width: 400px; 
            text-align: center; 
            box-shadow: 0 20px 50px rgba(0,0,0,0.2); 
        }

        .login-card img { 
            width: 110px; 
            height: 110px; 
            border-radius: 50%; 
            margin-bottom: 20px; 
            border: 5px solid var(--light); 
            object-fit: cover;
        }

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
           NAVIGATION BASSE (7 ONGLETS)
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
           CONTENEURS ET CARTES
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
           ELEMENTS DE LISTE ET BADGES
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
            font-size: 14px; 
        }

        .solde-badge { 
            background: var(--danger); 
            color: white; 
            padding: 6px 12px; 
            border-radius: 10px; 
            font-size: 12px; 
            font-weight: 900; 
            display: inline-block; 
            margin-bottom: 10px; 
        }

        .status-pill { 
            padding: 5px 12px; 
            border-radius: 50px; 
            font-size: 10px; 
            font-weight: 900; 
            text-transform: uppercase; 
        }
        .pill-active { background: #dcfce7; color: #166534; }
        .pill-blocked { background: #fee2e2; color: #991b1b; }

        .log-row { 
            font-size: 10px; 
            padding: 8px; 
            border-bottom: 1px solid #eee; 
            font-family: monospace; 
            background: #fafafa; 
            color: #555; 
        }

        .retard-card { 
            border: 2px solid var(--danger) !important; 
            background: #fff5f5 !important; 
            animation: blink-red 2s infinite; 
        }
        @keyframes blink-red { 0%, 100% { opacity: 1; } 50% { opacity: 0.8; } }

        @media print { 
            body * { visibility: hidden; } 
            #ticket-print, #ticket-print * { visibility: visible; } 
            #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } 
        }
    </style>
</head>
<body>

<div id="auth-screen">
    <div class="login-card">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" alt="Logo">
        <h2 style="margin:0; letter-spacing:-1px;">SUPER CLEAN</h2>
        <p style="font-size:12px; color:var(--primary); font-weight:800; margin-bottom:35px; text-transform:uppercase;">Le bien être de vos vêtements</p>
        <input type="text" id="u-login" placeholder="Identifiant (Login)">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" class="btn-main">OUVRIR LA SESSION</button>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 10px 20px; font-size: 11px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 UTILISATEUR : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); border:none; color:white; padding:6px 12px; border-radius:8px; font-size:10px; font-weight:bold; cursor:pointer;">DÉCONNEXION</button>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 130px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION & ENCAISSEMENT</h3>
                <input type="tel" id="tel-client" placeholder="N° WhatsApp Client (ex: 6...)" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <label style="font-size:11px; font-weight:800; color:#777; margin-left:5px;">DATE DE RETRAIT PRÉVUE :</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:20px; border-radius:18px; border:1px dashed #ddd; margin-top:10px;">
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">ARTICLE :</label>
                    <select id="select-article" onchange="majPrixInitial()"></select>
                    <label style="font-size:11px; font-weight:800; color:var(--primary);">PRIX (F CFA) :</label>
                    <input type="number" id="prix-final" placeholder="Prix ajusté">
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
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning); font-size:18px;">Reste à percevoir : 0 F</div>
            </div>

            <div style="padding: 0 15px;">
                <button onclick="validerCommande()" class="btn-main" style="height:80px; font-size:18px;">🖨️ VALIDER & IMPRIMER TICKET</button>
            </div>

            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSE DE CAISSE (ACHATS)</h3>
                <input type="text" id="achat-motif" placeholder="Motif (ex: Savon, Charbon...)">
                <input type="number" id="achat-montant" placeholder="Montant F CFA">
                <button onclick="enregistrerAchat()" class="btn-main" style="background:var(--danger);">DÉDUIRE DE LA RECETTE</button>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px; font-weight: 900;">👕 ZONE TECHNIQUE - PRODUCTION</h3>
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
                <div class="item-row"><span>Recette Aujourd'hui</span> <b id="p-today">0 F</b></div>
                <div class="item-row"><span>Derniers 7 Jours (Hebdo)</span> <b id="p-week">0 F</b></div>
                <div class="item-row"><span>Mois en cours</span> <b id="p-month">0 F</b></div>
                <div class="item-row"><span>Année 2026</span> <b id="p-year">0 F</b></div>
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
                    <hr style="margin:25px 0;">
                    <h4>📦 AJUSTEMENT MANUEL DES STOCKS</h4>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Quantité (+/-)">
                    <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">METTRE À JOUR</button>
                </div>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 JOURNAL D'ACTIVITÉS (LOGS)</h3>
                <div id="logs-container" style="max-height:500px; overflow-y:auto; background:#f1f4f8; border-radius:15px; padding:15px; font-family:monospace; font-size:11px;"></div>
                <button onclick="clearLogs()" class="btn-main" style="background:var(--danger); margin-top:20px; font-size:11px; padding:10px;">EFFACER LE JOURNAL (ADMIN SEUL)</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card" style="border: 2px solid var(--primary);">
                <h3>👑 PERSONNEL & CONTRÔLE D'ACCÈS</h3>
                <div id="staff-list"></div>
                <hr style="margin:25px 0;">
                <h4 style="color:var(--primary);">➕ CRÉER UN NOUVEL ACCÈS PERSONNEL</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Identifiant (Login)">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Zone Technique">Zone Technique</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">GÉNÉRER L'ACCÈS</button>
            </div>

            <div class="card">
                <h3>✨ AJOUTER UN ARTICLE À LA GRILLE</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie (ex: ROBES)">
                <input type="text" id="new-art-nom" placeholder="Désignation de l'article">
                <input type="number" id="new-art-prix" placeholder="Prix standard F CFA">
                <button onclick="ajouterNouveauProduit()" class="btn-main" style="background:var(--primary);">ACTUALISER LA GRILLE</button>
            </div>
            
            <div class="card" style="text-align:center;">
                <h3>☁️ SAUVEGARDE CLOUD</h3>
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 EXPORTER TOUTE LA BASE (.JSON)</button>
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
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:80px; border-radius:50%;"><br>
    <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
    "Le bien être de vos vêtements"<br>
    Tél: 675 864 103 / 692 315 267<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:13px; line-height:1.5;"></div>
    --------------------------------<br>
    Merci de votre confiance !
</div>

<script>
/* ==========================================================================
   GRILLE TARIFAIRE INTÉGRALE (DE L'IMAGE)
   ========================================================================== */
let GRILLE = JSON.parse(localStorage.getItem('sc_grille_v43')) || {
    "LES HAUTS": {
        "T-Shirt": 500, "Chemise": 500, "Polo": 500, "Pull Over": 700, "Blouson": 1500, "Démembré": 300, 
        "Veste 2 pièces": 1500, "Veste 3 pièces": 2000, "Pantalon simple": 500, "Pantalon jeans": 500, 
        "Short": 500, "Jupe simple": 500, "Jupe plissée": 1000
    },
    "ENSEMBLES": {
        "Costume simple": 2000, "Costume de luxe": 2500, "Boubou femme": 1500, "Boubou homme 2P": 1500, 
        "Boubou homme 3P": 2500, "Pyjama 2 pièces": 1000, "Jogging": 1500
    },
    "LES ROBES": {
        "Robe simple": 1000, "Robe de soirée": 3000, "Kaba court": 1000, "Kaba long": 1500, "Toge": 3500
    },
    "LINGES DE LIT": {
        "1 Drap": 750, "Ensemble 1 drap+taie": 1500, "Ensemble 2 drap+taie": 2000, "Couvre lit / Couette": 2000, "Couverture": 4000
    },
    "VÊTEMENTS ENFANTS": {
        "Ensemble enfant": 1000, "Robe enfant": 500, "Haut enfant": 500, "Bas enfant": 500
    },
    "AMEUBLEMENT / BAIN": {
        "Rideau lourd": 1500, "Rideau léger": 1000, "Petite Serviette": 500, "Grande Serviette": 1000, "Peignoir": 2000
    },
    "ACCESSOIRES": { "Tennis": 500, "Cravate": 500 }
};

/* ==========================================================================
   INITIALISATION DES ÉTATS
   ========================================================================== */
let db = JSON.parse(localStorage.getItem('sc_db_v43')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v43')) || [
    {nom: "Administrateur", login: "admin", pass: "0000", role: "ADMIN", actif: true}
];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v43')) || [
    {id:'savon', nom:'Savon Liquide', qte:100, unite:'L', mini:10},
    {id:'parfum', nom:'Parfum Textile', qte:30, unite:'L', mini:5},
    {id:'housses', nom:'Housses Plastiques', qte:500, unite:'pcs', mini:50}
];
let logs = JSON.parse(localStorage.getItem('sc_logs_v43')) || [];
let achats = JSON.parse(localStorage.getItem('sc_achats_v43')) || [];
let panier = [], currentUser = null;

/* ==========================================================================
   AUTHENTIFICATION & SÉCURITÉ ROLES
   ========================================================================== */
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return alert("🚫 ACCÈS RÉVOQUÉ : Votre compte est bloqué par le gérant.");
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        addLog("Ouverture de session réussie");
        filtrerRoles();
        initDropdownArticles();
    } else alert("Identifiants incorrects !");
}

function filtrerRoles() {
    const r = currentUser.role;
    const isBoss = (r === "ADMIN");
    const isCaisse = (r === "ADMIN" || r === "Caissière");
    
    // Verrouillage Physique des boutons
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
    localStorage.setItem('sc_logs_v43', JSON.stringify(logs.slice(0, 1000)));
}

/* ==========================================================================
   LOGIQUE CAISSE & ACHATS
   ========================================================================== */
function initDropdownArticles() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Sélectionner l\'Article --</option>';
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
    if(!s.value || !p) return alert("Veuillez choisir un article.");
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
    document.getElementById('reste-caisse').innerText = `Reste à percevoir : ${t - p} F CFA`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, dr = document.getElementById('date-retrait').value, tel = document.getElementById('tel-client').value;
    if(!nom || !dr || panier.length === 0) return alert("Données Client Incomplètes !");
    
    // Déduction auto stocks (0.05 savon par vêtement)
    stocks[0].qte -= (panier.length * 0.05);
    localStorage.setItem('sc_stocks_v43', JSON.stringify(stocks));

    const cmd = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0,
        mode: document.getElementById('mode-pay').value, retrait: dr, agent: currentUser.nom,
        timestamp: new Date().getTime(), statut: "En cours"
    };

    db.unshift(cmd); localStorage.setItem('sc_db_v43', JSON.stringify(db));
    addLog(`Création Ticket #${cmd.id} - Total : ${cmd.total}F`);
    imprimerTicket(cmd);
    location.reload();
}

function imprimerTicket(c) {
    document.getElementById('tk-body').innerHTML = `
        TICKET N° ${c.id}<br>DATE: ${new Date().toLocaleDateString()}<br>CLIENT: ${c.client.toUpperCase()}<br>TEL: ${c.tel}<hr>
        ${c.articles.map(a => `${a.nom.padEnd(18)} ${a.prix}F`).join('<br>')}<hr>
        TOTAL: ${c.total} F<br>PAYÉ: ${c.paye} F (${c.mode})<br><b>SOLDE: ${c.total - c.paye} F</b><hr>RDV RETRAIT: ${c.retrait}
    `;
    window.print();
}

function enregistrerAchat() {
    const motif = document.getElementById('achat-motif').value, mont = parseInt(document.getElementById('achat-montant').value);
    if(motif && mont) {
        achats.push({motif, mont, agent: currentUser.nom, timestamp: new Date().getTime()});
        localStorage.setItem('sc_achats_v43', JSON.stringify(achats));
        addLog(`SORTIE DE CAISSE : ${motif} (-${mont}F)`);
        alert("Dépense enregistrée et déduite de la caisse.");
        location.reload();
    }
}

/* ==========================================================================
   ZONE TECHNIQUE (SÉCURISÉE SANS ARGENT)
   ========================================================================== */
function renderAtelier() {
    const today = new Date().toISOString().split('T')[0];
    const canSeeMoney = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card ${c.retrait < today ? 'retard-card' : ''}">
            <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b><small>${c.retrait}</small></div>
            
            ${canSeeMoney ? `<div class="solde-badge">À PERCEVOIR : ${c.total-c.paye} F</div>` : ''}
            
            <div style="margin-top:10px;">
                ${c.articles.map(a => `
                    <div class="item-row">
                        <span>${a.nom}</span>
                        ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:5px 12px; border-radius:8px;">TERMINER</button>` : `<span style="font-weight:900; color:green;">📍 PRÊT</span>`}
                    </div>`).join('')}
            </div>
            
            ${canSeeMoney ? `<button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark); margin-top:10px;">LIVRER ET FERMER LE TICKET</button>` : ''}
        </div>`).join('');
}

function finirArt(cid, auid) {
    const c = db.find(x => x.id === cid), a = c.articles.find(x => x.uid === auid);
    a.pret = true;
    addLog(`Vêtement traité : ${a.nom} (Ticket #${cid})`);
    localStorage.setItem('sc_db_v43', JSON.stringify(db)); renderAtelier();
}

function livrer(id) {
    if(confirm("Confirmer la livraison finale et l'encaissement du solde ?")) {
        const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
        addLog(`LIVRAISON : Ticket #${id}`);
        localStorage.setItem('sc_db_v43', JSON.stringify(db)); renderAtelier();
    }
}

/* ==========================================================================
   ANALYSE PERFORMANCE & RAPPORTS
   ========================================================================== */
function calculerPerformance() {
    const now = new Date();
    const oneDay = 24 * 60 * 60 * 1000;
    let d=0, w=0, m=0, y=0;
    db.forEach(t => {
        const dt = new Date(t.timestamp);
        if(dt.toDateString() === now.toDateString()) d+=t.paye;
        if(now - dt <= 7 * oneDay) w+=t.paye;
        if(dt.getMonth() === now.getMonth() && dt.getFullYear() === now.getFullYear()) m+=t.paye;
        if(dt.getFullYear() === now.getFullYear()) y+=t.paye;
    });
    const spentTotal = achats.reduce((a,b)=>a+b.mont, 0);
    const incomeTotal = db.reduce((a,b)=>a+b.paye, 0);
    
    document.getElementById('p-today').innerText = d + " F";
    document.getElementById('p-week').innerText = w + " F";
    document.getElementById('p-month').innerText = m + " F";
    document.getElementById('p-year').innerText = y + " F";
    document.getElementById('p-net').innerText = (incomeTotal - spentTotal) + " F CFA";
}

function envoyerRapportWA() {
    const todayStr = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === todayStr);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === todayStr);
    let cash=0, om=0, momo=0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    let totalAchats = ach.reduce((a,b)=>a+b.mont, 0);
    
    const msg = `📊 *BILAN JOURNALIER SUPER CLEAN*\n📅 Date : ${new Date().toLocaleDateString()}\n👤 Agent : ${currentUser.nom}\n------------------\n🎟️ Tickets : ${tks.length}\n💵 Cash : ${cash}F\n🍊 OM : ${om}F\n🟡 MoMo : ${momo}F\n❌ Achats : -${totalAchats}F\n------------------\n💰 *NET À REMETTRE : ${cash+om+momo-totalAchats}F*`;
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
}

function renderRapport() {
    const todayStr = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === todayStr);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === todayStr);
    let cash=0, om=0, momo=0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    let totalAchats = ach.reduce((a,b)=>a+b.mont, 0);

    document.getElementById('rapport-details').innerHTML = `
        <div class="item-row"><span>Tickets créés :</span> <b>${tks.length}</b></div>
        <div class="item-row"><span>Recette Cash :</span> <b>${cash} F</b></div>
        <div class="item-row"><span>Recette Orange Money :</span> <b>${om} F</b></div>
        <div class="item-row"><span>Recette MTN MoMo :</span> <b>${momo} F</b></div>
        <div class="item-row" style="color:var(--danger)"><span>Achats effectués :</span> <b>-${totalAchats} F</b></div>
        <div class="item-row" style="background:var(--success); color:white;"><span>TOTAL NET EN CAISSE :</span> <b>${cash+om+momo-totalAchats} F CFA</b></div>
    `;
}

/* ==========================================================================
   ADMINISTRATION & PRÉROGATIVES EXCLUSIVES
   ========================================================================== */
function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `
        <div class="item-row" style="background:${s.actif?'':'#fff1f2'}">
            <div>
                <b>${s.nom}</b> <small>(${s.role})</small><br>
                <span class="status-pill ${s.actif?'pill-active':'pill-blocked'}">${s.actif?'ACCÈS AUTORISÉ':'ACCÈS RÉVOQUÉ'}</span>
            </div>
            ${i!==0 ? `<button onclick="basculerAcces(${i})" style="font-size:10px; padding:8px; border-radius:8px; background:var(--dark); color:white; border:none; cursor:pointer;">BASCULER STATUT</button>` : ''}
        </div>`).join('');
}

function basculerAcces(i) {
    staff[i].actif = !staff[i].actif;
    localStorage.setItem('sc_staff_v43', JSON.stringify(staff));
    renderAdmin();
    addLog(`Statut de compte modifié pour : ${staff[i].nom}`);
}

function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) {
        staff.push({nom:n, login:l, pass:p, role:r, actif:true});
        localStorage.setItem('sc_staff_v43', JSON.stringify(staff));
        addLog(`Création de compte employé : ${n} (${r})`);
        renderAdmin();
    } else alert("Veuillez remplir tous les champs !");
}

function ajouterNouveauProduit() {
    const cat = document.getElementById('new-art-cat').value.toUpperCase();
    const nom = document.getElementById('new-art-nom').value;
    const prix = parseInt(document.getElementById('new-art-prix').value);
    if(cat && nom && prix) {
        if(!GRILLE[cat]) GRILLE[cat] = {};
        GRILLE[cat][nom] = prix;
        localStorage.setItem('sc_grille_v43', JSON.stringify(GRILLE));
        alert("Grille tarifaire mise à jour !");
        initDropdownArticles();
    }
}

/* ==========================================================================
   NAVIGATION & SYSTEM UTILS
   ========================================================================== */
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
    if(id==='section-admin') renderAdmin();
    if(id==='section-logs') document.getElementById('logs-container').innerHTML = logs.map(l => `<div class="log-row">${l}</div>`).join('');
}

function renderStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-row"><span>${s.nom}</span> <b>${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}

function majStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    const s = stocks.find(x => x.id === id); s.qte += q;
    addLog(`AJUSTEMENT STOCK : ${s.nom} (${q > 0 ? '+':''}${q})`);
    localStorage.setItem('sc_stocks_v43', JSON.stringify(stocks)); renderStocks();
}

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length>=8) {
        const found = db.find(x => x.tel && x.tel.includes(t));
        if(found) document.getElementById('client-nom').value = found.client;
    }
}

function clearLogs() { 
    if(confirm("Confirmer la suppression de l'historique ?")) {
        logs=[]; localStorage.setItem('sc_logs_v43', "[]"); tab('section-logs');
    }
}

function exportData() {
    const blob = new Blob([JSON.stringify({db, staff, stocks, achats, logs, GRILLE})], {type:"application/json"});
    const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download=`SuperClean_Full_Backup.json`; a.click();
}

function importData() {
    const reader = new FileReader(); reader.onload = (e) => {
        const d = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v43', JSON.stringify(d.db));
        localStorage.setItem('sc_staff_v43', JSON.stringify(d.staff));
        localStorage.setItem('sc_stocks_v43', JSON.stringify(d.stocks));
        localStorage.setItem('sc_grille_v43', JSON.stringify(d.GRILLE));
        location.reload();
    }; reader.readAsText(document.getElementById('importFile').files[0]);
}
</script>
</body>
</html>
