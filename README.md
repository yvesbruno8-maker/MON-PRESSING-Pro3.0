<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v38.0 - SOUVERAINETÉ TOTALE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --white: #ffffff;
            --radius: 20px; --shadow: 0 10px 30px rgba(0,0,0,0.1);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; }
        .hidden { display: none !important; }
        
        /* --- BRANDING --- */
        .header { background: var(--white); padding: 20px; text-align: center; border-bottom: 4px solid var(--primary); position: sticky; top: 0; z-index: 1000; }
        .logo-main { width: 80px; height: 80px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 24px; margin-top: 8px; }
        .official-slogan { font-size: 13px; color: #555; font-style: italic; font-weight: 700; margin-top: 2px; }

        /* --- NAVIGATION VERROUILLÉE --- */
        .nav-bar { display: grid; grid-template-columns: repeat(auto-fit, minmax(40px, 1fr)); gap: 2px; padding: 12px; background: var(--white); position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000; }
        .nav-bar button { background: #f1f4f8; border: none; color: #7f8c8d; font-weight: 800; font-size: 7px; padding: 12px 1px; border-radius: 12px; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .nav-bar button.active { background: var(--primary); color: var(--white); transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 86, 179, 0.4); }

        /* --- UI COMPONENTS --- */
        .card { background: var(--white); margin: 15px; padding: 25px; border-radius: var(--radius); box-shadow: var(--shadow); border: 1px solid #eee; }
        h3 { margin-top: 0; font-size: 16px; color: var(--primary); font-weight: 900; text-transform: uppercase; border-bottom: 3px solid var(--light); padding-bottom: 12px; margin-bottom: 20px; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 16px; border-radius: 15px; margin-bottom: 12px; width: 100%; box-sizing: border-box; font-size: 15px; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--primary); background: var(--white); }
        .btn-main { background: var(--primary); color: var(--white); border: none; padding: 20px; border-radius: 18px; font-weight: 900; width: 100%; cursor: pointer; text-transform: uppercase; font-size: 14px; }
        
        /* --- ADMIN EXCLUSIVE STYLES --- */
        .admin-only { border: 2px solid var(--primary) !important; background: #f0f7ff !important; }
        .status-pill { padding: 4px 10px; border-radius: 50px; font-size: 10px; font-weight: 900; }
        .pill-active { background: #dcfce7; color: #166534; }
        .pill-blocked { background: #fee2e2; color: #991b1b; }
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 12px; background: #f8f9fa; border-radius: 12px; margin-bottom: 8px; border-left: 5px solid var(--primary); font-size: 13px; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 90%; max-width: 400px; text-align: center; padding: 50px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:110px; border-radius:50%; margin-bottom:20px; border: 5px solid #f1f4f8;">
            <h2 style="margin:0;">SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:800; margin-bottom:35px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Identifiant (Login)">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">S'IDENTIFIER</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 10px 20px; font-size: 11px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 SESSION : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
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
                <h3>📥 RÉCEPTION CLIENT</h3>
                <input type="tel" id="tel-client" placeholder="WhatsApp Client (ex: 6...)" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <input type="date" id="date-retrait">
                <div style="background:#f8f9fa; padding:20px; border-radius:18px; border:1px dashed #ddd; margin-top:10px;">
                    <select id="select-article" onchange="majPrixInitial()"></select>
                    <input type="number" id="prix-final" placeholder="Prix ajusté">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark);">+ AJOUTER AU TICKET</button>
                </div>
            </div>
            <div id="panier-liste" style="padding: 0 15px;"></div>
            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between;"><span>TOTAL :</span> <b id="total-caisse" style="font-size:26px; color:var(--success);">0 F</b></div>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white; margin: 15px 0;"><option value="Cash">Cash</option><option value="OM">Orange Money</option><option value="Momo">MTN Momo</option></select>
                <input type="number" id="paye-caisse" placeholder="Avance reçue" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning);">Reste : 0 F</div>
            </div>
            <div style="padding: 0 15px;"><button onclick="validerCommande()" class="btn-main" style="height:80px;">🖨️ VALIDER & TICKET</button></div>
            
            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSE DE CAISSE</h3>
                <input type="text" id="achat-motif" placeholder="Motif de l'achat">
                <input type="number" id="achat-montant" placeholder="Montant">
                <button onclick="enregistrerAchat()" class="btn-main" style="background:var(--danger);">DÉDUIRE</button>
            </div>
        </div>

        <div id="section-rapport" class="tab-content hidden">
            <div class="card">
                <h3>📊 RAPPORT JOURNALIER</h3>
                <div id="rapport-details"></div>
                <button onclick="envoyerRapportWA()" class="btn-main" style="background:#25D366; margin-top:20px;">📲 ENVOYER PAR WHATSAPP</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE DES PERFORMANCES</h3>
                <div class="item-row"><span>Recette Aujourd'hui</span> <b id="p-today">0 F</b></div>
                <div class="item-row"><span>Derniers 7 Jours</span> <b id="p-week">0 F</b></div>
                <div class="item-row"><span>Mois en cours</span> <b id="p-month">0 F</b></div>
                <div class="item-row" style="background:var(--success); color:white;"><span>NET GLOBAL</span> <b id="p-net">0 F</b></div>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px;">👕 ZONE TECHNIQUE</h3>
            <div id="atelier-liste"></div>
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
                <h3>📜 JOURNAL D'AUDIT (LOGS)</h3>
                <div id="logs-container" style="max-height:500px; overflow-y:auto; background:#f1f4f8; padding:10px;"></div>
                <button onclick="clearLogs()" class="btn-main" style="background:var(--danger); margin-top:10px; font-size:10px;">Vider (Admin)</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card admin-only">
                <h3>👥 PERSONNEL & DROITS D'ACCÈS</h3>
                <div id="staff-list"></div>
                <hr style="margin:25px 0;">
                <h4 style="color:var(--primary);">➕ CRÉER UN NOUVEL EMPLOYÉ</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Identifiant (Login)">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Laveur">Laveur</option>
                    <option value="Repasseur">Repasseur</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">GÉNÉRER L'ACCÈS</button>
            </div>

            <div class="card admin-only">
                <h3>✨ AJOUTER UN ARTICLE À LA GRILLE</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie">
                <input type="text" id="new-art-nom" placeholder="Désignation">
                <input type="number" id="new-art-prix" placeholder="Prix de base">
                <button onclick="ajouterNouveauProduit()" class="btn-main">ACTUALISER LA GRILLE</button>
            </div>
            
            <div class="card" style="text-align:center;">
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 BACKUP DRIVE / ICLOUD</button>
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
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:75px; border-radius:50%;"><br>
    <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
    "Le bien être de vos vêtements"<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:13px;"></div>
</div>

<script>
// ==========================================
// 1. DONNÉES & GRILLE TARIFAIRE IMAGE
// ==========================================
let GRILLE = JSON.parse(localStorage.getItem('sc_grille_v38')) || {
    "HAUTS": {"Chemise":500,"Veste 2P":1500,"Pull":700,"T-Shirt":500,"Blouson":1500},
    "ENSEMBLES": {"Costume luxe":2500,"Boubou 3P":2500,"Jogging":1500},
    "ROBES": {"Robe simple":1000,"Robe soirée":3000,"Toge":3500},
    "LIT": {"1 Drap":750,"Ensemble Draps":2000,"Couverture":4000},
    "AUTRES": {"Tennis":500,"Cravate":500,"Peignoir":2000}
};

let db = JSON.parse(localStorage.getItem('sc_db_v38')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v38')) || [{nom:"Administrateur", login:"admin", pass:"0000", role:"ADMIN", actif:true}];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v38')) || [{id:'savon', nom:'Savon', qte:100, unite:'L', mini:5}, {id:'housses', nom:'Housses', qte:200, unite:'pcs', mini:10}];
let logs = JSON.parse(localStorage.getItem('sc_logs_v38')) || [];
let achats = JSON.parse(localStorage.getItem('sc_achats_v38')) || [];
let panier = [], currentUser = null;

// ==========================================
// 2. AUTHENTIFICATION & MATRICE DE SÉCURITÉ
// ==========================================
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return alert("🚫 ACCÈS BLOQUÉ : Contactez le gérant.");
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        addLog("Connexion");
        filtrerRoles(); initDropdown();
    } else alert("Erreur d'identifiants.");
}

function filtrerRoles() {
    const r = currentUser.role;
    const allowsCaisse = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-rapport').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-performance').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-logs').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-admin').classList.toggle('hidden', r !== "ADMIN");
    if(r === "ADMIN") document.getElementById('admin-stock-control').classList.remove('hidden');
    tab(allowsCaisse ? 'section-caisse' : 'section-atelier');
}

function addLog(action) {
    logs.unshift(`[${new Date().toLocaleString()}] ${currentUser.nom} : ${action}`);
    localStorage.setItem('sc_logs_v38', JSON.stringify(logs.slice(0, 1000)));
}

// ==========================================
// 3. CAISSE & ACHATS
// ==========================================
function initDropdown() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Article --</option>';
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
    if(!s.value || !p) return;
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p), pret: false });
    renderPanier();
}
function renderPanier() {
    const t = panier.reduce((a,b)=>a+b.prix, 0);
    document.getElementById('total-caisse').innerText = t + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`<div class="item-row"><b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()">✕</button></span></div>`).join('');
    const p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${t - p} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, dr = document.getElementById('date-retrait').value;
    if(!nom || panier.length === 0) return alert("Incomplet");
    const cmd = { id: Math.floor(1000+Math.random()*8999), client: nom, tel: document.getElementById('tel-client').value, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, mode: document.getElementById('mode-pay').value, retrait: dr, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "Z.T" };
    db.unshift(cmd); localStorage.setItem('sc_db_v38', JSON.stringify(db));
    addLog(`Ticket créé #${cmd.id}`);
    document.getElementById('tk-body').innerHTML = `TICKET #${cmd.id}<br>CLIENT: ${cmd.client}<hr>TOTAL: ${cmd.total}F<br>RDV: ${cmd.retrait}`;
    window.print(); location.reload();
}

function enregistrerAchat() {
    const m = document.getElementById('achat-motif').value, mont = parseInt(document.getElementById('achat-montant').value);
    if(m && mont) {
        achats.push({motif: m, mont, agent: currentUser.nom, timestamp: new Date().getTime()});
        localStorage.setItem('sc_achats_v38', JSON.stringify(achats));
        addLog(`Achat caisse : ${m} (-${mont}F)`);
        alert("Dépense enregistrée."); location.reload();
    }
}

// ==========================================
// 4. ZONE TECHNIQUE & STOCKS
// ==========================================
function renderAtelier() {
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card">
            <b>#${c.id} - ${c.client}</b><br>
            <div class="solde-badge">À PAYER : ${c.total-c.paye} F</div><br>
            <button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark);">LIVRER</button>
        </div>`).join('');
}
function livrer(id) {
    const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
    addLog(`Livraison #${id}`); localStorage.setItem('sc_db_v38', JSON.stringify(db)); renderAtelier();
}

function majStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    stocks.find(s => s.id === id).qte += q;
    localStorage.setItem('sc_stocks_v38', JSON.stringify(stocks));
    addLog(`Stock manuel ${id} (${q})`); renderStocks();
}
function renderStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-row"><span>${s.nom}</span> <b>${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}

// ==========================================
// 5. RAPPORTS & ADMIN EXCLUSIF
// ==========================================
function envoyerRapportWA() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    const ach = achats.filter(a => new Date(a.timestamp).toDateString() === today);
    let tot = tks.reduce((a,b)=>a+b.paye, 0);
    let spent = ach.reduce((a,b)=>a+b.mont, 0);
    const msg = `📊 *RAPPORT SUPER CLEAN*\nAgent: ${currentUser.nom}\nRecettes: ${tot}F\nAchats: ${spent}F\n💰 *NET: ${tot-spent}F*`;
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
}

function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `
        <div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;">
            <span>${s.nom} (${s.role}) <span class="status-pill ${s.actif?'pill-active':'pill-blocked'}">${s.actif?'ACTIF':'BLOC'}</span></span>
            ${i!==0?`<button onclick="basculer(${i})">${s.actif?'BLOQUER':'ACTIVER'}</button>`:''}
        </div>`).join('');
}

function basculer(i) {
    if(i===0) return;
    staff[i].actif = !staff[i].actif;
    localStorage.setItem('sc_staff_v38', JSON.stringify(staff));
    renderAdmin(); addLog(`Accès changé pour ${staff[i].nom}`);
}

function ajouterNouveauProduit() {
    const cat = document.getElementById('new-art-cat').value.toUpperCase();
    const nom = document.getElementById('new-art-nom').value;
    const prix = parseInt(document.getElementById('new-art-prix').value);
    if(cat && nom && prix) {
        if(!GRILLE[cat]) GRILLE[cat] = {};
        GRILLE[cat][nom] = prix;
        localStorage.setItem('sc_grille_v38', JSON.stringify(GRILLE));
        alert("Grille mise à jour."); initDropdown();
    }
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]);
    if(btn) btn.classList.add('active');
    if(id==='section-atelier') renderAtelier();
    if(id==='section-admin') renderAdmin();
    if(id==='section-stocks') renderStocks();
    if(id==='section-performance') {
        const total = db.reduce((a,b)=>a+b.paye, 0);
        document.getElementById('p-net').innerText = total + " F";
    }
}

function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif:true}); localStorage.setItem('sc_staff_v38', JSON.stringify(staff)); renderAdmin(); }
}

function exportData() {
    const b = new Blob([JSON.stringify({db, staff, stocks, achats, logs, GRILLE})], {type:"application/json"});
    const a = document.createElement('a'); a.href = URL.createObjectURL(b); a.download=`SuperClean_Backup.json`; a.click();
}

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length>=8) { const f = db.find(x => x.tel && x.tel.includes(t)); if(f) document.getElementById('client-nom').value = f.client; }
}
</script>
</body>
</html>
