<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v10.0 - GESTION TOTALE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #4361ee; --success: #2ecc71; --danger: #f72585; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --radius: 16px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 0; background-color: var(--light); color: var(--dark); overflow-x: hidden; }
        .hidden { display: none !important; }
        
        /* --- NAVIGATION --- */
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; padding: 12px; background: white; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 15px rgba(0,0,0,0.05); }
        .nav-bar button { background: #f8f9fa; border: 1px solid #eee; color: #95a5a6; font-weight: 700; font-size: 9px; padding: 10px 2px; border-radius: 12px; display: flex; flex-direction: column; align-items: center; transition: 0.3s; cursor: pointer; }
        .nav-bar button.active { background: var(--primary); color: white; border-color: var(--primary); box-shadow: 0 4px 10px rgba(67, 97, 238, 0.3); }
        .nav-bar button i { font-size: 16px; margin-bottom: 4px; }

        /* --- AUTHENTIFICATION --- */
        #auth-screen { background: linear-gradient(135deg, #1b2631 0%, #4361ee 100%); height: 100vh; display: flex; align-items: center; justify-content: center; position: fixed; width: 100%; z-index: 2000; }
        .login-card { background: white; padding: 40px 30px; border-radius: 28px; box-shadow: 0 25px 50px rgba(0,0,0,0.3); width: 85%; max-width: 340px; text-align: center; }

        /* --- ÉLÉMENTS UI --- */
        .card { background: white; margin: 15px; padding: 20px; border-radius: var(--radius); box-shadow: 0 5px 20px rgba(0,0,0,0.04); border: 1px solid rgba(0,0,0,0.02); }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 14px; border-radius: 12px; margin-bottom: 12px; font-family: inherit; font-size: 14px; width: 100%; box-sizing: border-box; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--primary); background: white; }
        
        .btn-main { background: var(--primary); color: white; border: none; padding: 16px; border-radius: 14px; font-weight: 700; font-size: 15px; cursor: pointer; width: 100%; transition: 0.2s; }
        .btn-main:active { transform: scale(0.96); }

        .badge { font-size: 10px; padding: 4px 8px; border-radius: 6px; font-weight: 800; text-transform: uppercase; }
        .role-ADMIN { background: #ebf5ff; color: #1e40af; }
        .role-STAFF { background: #f0fdf4; color: #166534; }

        /* --- ATELIER --- */
        .order-card { border-left: 6px solid var(--primary); position: relative; }
        .progress-bg { background: #edf2f7; height: 8px; border-radius: 10px; margin: 15px 0; overflow: hidden; }
        .progress-bar { background: var(--success); height: 100%; transition: 0.5s; width: 0%; }
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px dashed #eee; font-size: 13px; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div class="login-card">
        <div style="font-size: 40px; margin-bottom: 10px;">🧼</div>
        <h2 style="margin:0; letter-spacing: -1px;">SUPER CLEAN</h2>
        <p style="font-size:12px; color:#64748b; margin-bottom:25px;">Logiciel de Gestion v10.0</p>
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" class="btn-main">ACCÉDER AU TABLEAU DE BORD</button>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background:white; padding:12px 20px; display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid #edf2f7;">
        <div>
            <div style="font-size:10px; color:#64748b; font-weight:700;">UTILISATEUR CONNECTÉ</div>
            <div style="font-size:13px; font-weight:800; color:var(--primary);" id="user-display">-</div>
        </div>
        <button onclick="location.reload()" style="background:#fff1f2; border:none; color:var(--danger); padding:8px 12px; border-radius:10px; font-size:11px; font-weight:800; cursor:pointer;">QUITTER</button>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="nav-caisse" class="active">📥<span>CAISSE</span></button>
        <button onclick="tab('section-atelier')" id="nav-atelier">👕<span>ATELIER</span></button>
        <button onclick="tab('section-stocks')" id="nav-stocks">🧼<span>STOCKS</span></button>
        <button onclick="tab('section-admin')" id="nav-admin">👑<span>GESTION</span></button>
    </nav>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <h3 style="margin-top:0;">Nouvelle Réception</h3>
            <input type="tel" id="tel-client" placeholder="N° WhatsApp Client" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom complet du client">
            <label style="font-size:11px; font-weight:800; color:#64748b;">DATE DE RETRAIT PRÉVUE</label>
            <input type="date" id="date-retrait">
            <select id="select-article"></select>
            <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark);">+ AJOUTER À LA LISTE</button>
        </div>

        <div id="panier-liste" style="padding: 0 15px;"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
                <span style="font-size:12px; font-weight:600;">TOTAL À PAYER</span>
                <b id="total-caisse" style="font-size:26px; color:var(--success);">0 F</b>
            </div>
            
            <label style="font-size:10px; color:#94a3b8;">MODE DE PAIEMENT</label>
            <select id="mode-pay" style="background:rgba(255,255,255,0.05); color:white; border:1px solid rgba(255,255,255,0.1); margin-top:5px;">
                <option value="Cash">💵 Espèces (Cash)</option>
                <option value="OM">🍊 Orange Money</option>
                <option value="Momo">🟡 MTN MoMo</option>
            </select>

            <label style="font-size:10px; color:#94a3b8;">AVANCE ENCAISSÉE</label>
            <input type="number" id="paye-caisse" placeholder="0" oninput="calculerReste()" style="background:rgba(255,255,255,0.05); color:white; border:1px solid rgba(255,255,255,0.1);">
            <div id="reste-caisse" style="text-align:right; font-weight:700; color:var(--warning); font-size:13px;">Reste : 0 F</div>
        </div>

        <div style="padding:15px;"><button onclick="validerCommande()" class="btn-main" id="btn-valider">CRÉER LE TICKET & ENREGISTRER</button></div>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>Niveaux des Consommables</h3>
            <div id="stock-display"></div>
            <div id="stock-admin-only" class="hidden">
                <hr style="border:0; border-top:1px solid #eee; margin:20px 0;">
                <h4>Réapprovisionnement</h4>
                <select id="upd-stock-item"></select>
                <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
                <button onclick="modifierStockManuel()" class="btn-main" style="background:var(--warning); color:black;">VALIDER L'ENTRÉE STOCK</button>
            </div>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card">
            <h3>Chiffre d'Affaires</h3>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px;">
                <button onclick="afficherStats('jour')" class="btn-main" style="font-size:12px;">JOURNÉE</button>
                <button onclick="afficherStats('mois')" class="btn-main" style="background:var(--dark); font-size:12px;">MENSUEL</button>
            </div>
            <div id="admin-stats-ui" style="margin-top:20px;"></div>
        </div>

        <div class="card">
            <h3>Gestion de l'Équipe & Rôles</h3>
            <div id="staff-list"></div>
            <hr style="margin:25px 0; border:0; border-top:1px solid #eee;">
            <h4>Créer un nouveau profil</h4>
            <input type="text" id="ns-nom" placeholder="Nom et Prénom">
            <input type="text" id="ns-log" placeholder="Nom d'utilisateur">
            <input type="password" id="ns-pass" placeholder="Mot de passe">
            <select id="ns-role" style="border:2px solid var(--primary); font-weight:700;">
                <option value="Caissière">Rôle : CAISSIÈRE</option>
                <option value="Laveur">Rôle : LAVEUR</option>
                <option value="Repasseur">Rôle : REPASSEUR</option>
                <option value="Marketeur">Rôle : MARKETEUR</option>
                <option value="Assistant">Rôle : ASSISTANT (Gestion)</option>
                <option value="ADMIN">Rôle : ADMINISTRATEUR (Propriétaire)</option>
            </select>
            <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">ENREGISTRER LE COMPTE</button>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; line-height:1.2;">
    <center>
        <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
        L'EXCELLENCE DU LINGE<br>
        Tél: (+237) XXX XXX XXX<br>
        --------------------------------
    </center>
    <div id="tk-content" style="font-size:14px; margin-top:10px;"></div>
    <center>
        --------------------------------<br>
        Merci de votre confiance !<br>
        Les articles non retirés après 30 jours<br>
        seront vendus.
    </center>
</div>

<script>
// ==========================================
// 1. BASE DE DONNÉES & TARIFS COMPLETS
// ==========================================
const GRILLE = {
    "HAUTS": {"Chemise": 500, "Chemise Amidon": 700, "Veste": 1500, "Polo": 500, "T-Shirt": 400, "Manteau": 2500, "Pull": 700},
    "BAS": {"Pantalon": 500, "Pantalon Lin": 800, "Jupe Simple": 500, "Jupe Plissée": 1000, "Short": 400},
    "TENUES": {"Costume 2P": 2500, "Costume 3P": 3500, "Robe Simple": 1000, "Robe Soirée": 3000, "Boubou Bazin": 3500, "Toge": 3000},
    "MAISON": {"Drap 1P": 800, "Drap 2P": 1200, "Couette": 2500, "Rideau": 1500, "Nappe": 700, "Serviette": 500},
    "ACCESSOIRES": {"Tennis": 1500, "Tennis Cuir": 2500, "Sac": 2500, "Cravate": 500, "Casquette": 500}
};

let db = JSON.parse(localStorage.getItem('sc_db_v10')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v10')) || [
    {id:'savon', nom:"Savon Liquide", qte:50, unite:"L"}, 
    {id:'parfum', nom:"Parfum Textile", qte:10, unite:"L"},
    {id:'housses', nom:"Housses Plastiques", qte:200, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff_v10')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [], currentUser = null;

// ==========================================
// 2. GESTION DES ACCÈS & RÔLES
// ==========================================
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const user = staff.find(s => s.login === l && s.pass === p);
    
    if(user) {
        currentUser = user;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('user-display').innerText = `${currentUser.nom} (${currentUser.role})`;
        
        // Restriction visuelle immédiate
        appliquerRestrictions();
        initApp();
    } else {
        alert("Identifiants incorrects !");
    }
}

function appliquerRestrictions() {
    const role = currentUser.role;
    // Navigation
    if(role === "Marketeur") document.getElementById('nav-atelier').classList.add('hidden');
    if(role === "Laveur" || role === "Repasseur") {
        document.getElementById('nav-caisse').classList.add('hidden');
        document.getElementById('nav-admin').classList.add('hidden');
    }
    if(role !== "ADMIN" && role !== "Assistant") {
        document.getElementById('nav-admin').classList.add('hidden');
    }
    // Stocks
    if(role === "ADMIN" || role === "Assistant") {
        document.getElementById('stock-admin-only').classList.remove('hidden');
    }
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Sélectionner l\'article --</option>';
    for(let cat in GRILLE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in GRILLE[cat]) {
            let o = document.createElement('option'); o.value = GRILLE[cat][art];
            o.innerText = `${art} (${GRILLE[cat][art]}F)`; g.appendChild(o);
        }
        s.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(x => `<option value="${x.id}">${x.nom}</option>`).join('');
    afficherStocks();
}

// ==========================================
// 3. MODULE CAISSE
// ==========================================
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    panier.push({ 
        uid: Date.now() + Math.random(), 
        nom: s.options[s.selectedIndex].text.split(' (')[0], 
        prix: parseInt(s.value), 
        pret: false, 
        done_by: "" 
    });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `
        <div style="background:white; padding:12px; border-radius:12px; margin-bottom:8px; display:flex; justify-content:space-between; align-items:center; border:1px solid #edf2f7;">
            <span><b>${x.nom}</b></span>
            <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()" style="background:none; border:none; color:var(--danger); font-weight:800; cursor:pointer; margin-left:10px;">✕</button></span>
        </div>`).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste à payer : ${Math.max(0, t - p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, dateR = document.getElementById('date-retrait').value;
    if(!nom || !dateR || panier.length === 0) return alert("Données manquantes !");
    
    const ticket = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: document.getElementById('tel-client').value,
        articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), 
        paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value,
        date_depot: new Date().toLocaleDateString('fr-FR'), retrait: dateR, 
        caissier: currentUser.nom, statut: "Atelier", timestamp: new Date().getTime()
    };

    // Consommation Stock (0.05L de savon par article, 1 housse)
    stocks[0].qte -= (panier.length * 0.05); 
    stocks[2].qte -= panier.length;
    localStorage.setItem('sc_stocks_v10', JSON.stringify(stocks));

    db.unshift(ticket); 
    localStorage.setItem('sc_db_v10', JSON.stringify(db));
    
    // Impression
    document.getElementById('tk-content').innerHTML = `
        TICKET #${ticket.id}<br>
        DATE: ${ticket.date_depot}<br>
        CLIENT: ${ticket.client.toUpperCase()}<br>
        CAISSIER: ${ticket.caissier}<br>
        --------------------------------<br>
        ${panier.map(a => `${a.nom.padEnd(18)} ${a.prix}F`).join('<br>')}<br>
        --------------------------------<br>
        TOTAL: ${ticket.total} F<br>
        PAYÉ (${ticket.mode}): ${ticket.paye} F<br>
        <b>RESTE: ${ticket.total - ticket.paye} F</b><br>
        RETRAIT LE: ${ticket.retrait}
    `;
    window.print(); 
    location.reload();
}

// ==========================================
// 4. MODULE ATELIER
// ==========================================
function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const orders = db.filter(t => t.statut !== "Livré");
    
    if(orders.length === 0) {
        list.innerHTML = `<div style="text-align:center; padding:40px; color:#94a3b8;">Aucun vêtement en cours...</div>`;
        return;
    }

    list.innerHTML = orders.map(t => {
        const fini = t.articles.filter(a => a.pret).length;
        const prog = Math.round((fini/t.articles.length)*100);
        return `
            <div class="card order-card" style="border-left-color: ${prog === 100 ? 'var(--success)' : 'var(--primary)'}">
                <div style="display:flex; justify-content:space-between;">
                    <b>#${t.id} - ${t.client}</b>
                    <small style="font-weight:700; color:var(--danger);">${t.retrait}</small>
                </div>
                <div class="progress-bg"><div class="progress-bar" style="width:${prog}%"></div></div>
                ${t.articles.map(art => `
                    <div class="item-row">
                        <span>${art.nom}</span>
                        ${!art.pret ? 
                            `<button onclick="marquerFini(${t.id}, ${art.uid})" style="background:var(--success); color:white; border:none; border-radius:8px; padding:6px 12px; font-weight:700; cursor:pointer;">PRÊT</button>` : 
                            `<span style="color:var(--success); font-weight:800;">✔ ${art.done_by}</span>`
                        }
                    </div>`).join('')}
                ${prog === 100 ? `<button onclick="livrer(${t.id})" class="btn-main" style="margin-top:15px; background:black;">MARQUER LIVRÉ & SOLDÉ</button>` : ''}
            </div>`;
    }).join('');
}

function marquerFini(tid, auid) {
    const t = db.find(x => x.id === tid), art = t.articles.find(a => a.uid === auid);
    art.pret = true; 
    art.done_by = currentUser.nom;
    localStorage.setItem('sc_db_v10', JSON.stringify(db)); 
    afficherAtelier();
}

function livrer(id) {
    const t = db.find(x => x.id === id);
    if(confirm("Confirmer la livraison au client ? Le solde sera marqué comme payé.")) {
        t.statut = "Livré"; 
        t.paye = t.total; // Encaissement du reste
        localStorage.setItem('sc_db_v10', JSON.stringify(db)); 
        afficherAtelier();
    }
}

// ==========================================
// 5. MODULE ADMIN & STATS
// ==========================================
function afficherStats(periode) {
    const now = new Date(); 
    let cash = 0, om = 0, momo = 0;
    
    db.forEach(t => {
        const d = new Date(t.timestamp);
        let match = false;
        if(periode==='jour' && d.toDateString() === now.toDateString()) match = true;
        if(periode==='mois' && d.getMonth() === now.getMonth() && d.getFullYear() === now.getFullYear()) match = true;
        
        if(match) {
            if(t.mode === "Cash") cash += t.paye;
            if(t.mode === "OM") om += t.paye;
            if(t.mode === "Momo") momo += t.paye;
        }
    });

    document.getElementById('admin-stats-ui').innerHTML = `
        <div style="display:grid; grid-template-columns: 1fr; gap:10px;">
            <div style="background:#f8fafc; padding:15px; border-radius:12px; border-left:5px solid #2ecc71;">CASH : <b>${cash} F</b></div>
            <div style="background:#f8fafc; padding:15px; border-radius:12px; border-left:5px solid #ff9f1c;">ORANGE : <b>${om} F</b></div>
            <div style="background:#f8fafc; padding:15px; border-radius:12px; border-left:5px solid #4361ee;">MTN MOMO : <b>${momo} F</b></div>
            <div style="background:var(--dark); color:white; padding:20px; border-radius:12px; text-align:center;">
                <small>CHIFFRE D'AFFAIRES TOTAL (${periode})</small>
                <div style="font-size:24px; font-weight:900;">${cash+om+momo} F CFA</div>
            </div>
        </div>`;
}

function ajouterEmploye() {
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-log').value, p = document.getElementById('ns-pass').value, r = document.getElementById('ns-role').value;
    if(n && l && p) {
        staff.push({nom:n, login:l, pass:p, role:r});
        localStorage.setItem('sc_staff_v10', JSON.stringify(staff));
        afficherPersonnel();
        alert("Compte " + r + " créé avec succès !");
        document.getElementById('ns-nom').value = ""; document.getElementById('ns-log').value = ""; document.getElementById('ns-pass').value = "";
    } else {
        alert("Remplissez tous les champs !");
    }
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e, i) => `
        <div style="display:flex; justify-content:space-between; align-items:center; padding:12px; border-bottom:1px solid #f1f5f9;">
            <span><b>${e.nom}</b> <span class="badge role-${e.role === 'ADMIN' ? 'ADMIN' : 'STAFF'}">${e.role}</span></span>
            ${i !== 0 ? `<button onclick="supprimerUser(${i})" style="color:var(--danger); border:none; background:none; font-weight:800; cursor:pointer;">✕</button>` : '<i>(Chef)</i>'}
        </div>`).join('');
}

function supprimerUser(i) {
    if(confirm("Supprimer ce compte ?")) {
        staff.splice(i, 1);
        localStorage.setItem('sc_staff_v10', JSON.stringify(staff));
        afficherPersonnel();
    }
}

// ==========================================
// 6. MODULE STOCKS & RECHERCHE
// ==========================================
function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div style="display:flex; justify-content:space-between; padding:12px 0; border-bottom:1px solid #f1f5f9;">
            <span>${s.nom}</span>
            <b style="color: ${s.qte < 5 ? 'var(--danger)' : 'var(--primary)'}">${s.qte.toFixed(2)} ${s.unite}</b>
        </div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(q) {
        stocks.find(s => s.id === id).qte += q;
        localStorage.setItem('sc_stocks_v10', JSON.stringify(stocks));
        afficherStocks();
        document.getElementById('upd-stock-qte').value = "";
    }
}

function rechercherClient() {
    const tel = document.getElementById('tel-client').value;
    if(tel.length >= 8) {
        const found = db.find(t => t.tel && t.tel.includes(tel));
        if(found) document.getElementById('client-nom').value = found.client;
    }
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    
    if(id === 'section-atelier') afficherAtelier();
    if(id === 'section-admin') { afficherStats('jour'); afficherPersonnel(); }
    if(id === 'section-stocks') afficherStocks();
}
</script>

</body>
</html>
