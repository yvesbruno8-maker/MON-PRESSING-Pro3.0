<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING - GESTION TOTALE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --radius: 12px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); font-size: 14px; }
        .hidden { display: none !important; }
        .card { background: white; padding: 15px; border-radius: var(--radius); box-shadow: 0 4px 12px rgba(0,0,0,0.08); margin-bottom: 12px; border: 1px solid #eee; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 8px; border: 1px solid #ccc; font-size: 14px; box-sizing: border-box; outline: none; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; transition: 0.2s; }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; position: sticky; top: 0; z-index: 100; background: var(--light); padding: 5px 0; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 8px; height: 50px; font-weight: 800; }
        .nav-bar button.active { background: var(--primary); color: white; }

        /* ATELIER & PROGRESSION */
        .progress-container { background: #eee; height: 12px; border-radius: 6px; margin: 10px 0; overflow: hidden; border: 1px solid #ddd; }
        .progress-fill { background: var(--success); height: 100%; transition: 0.5s; width: 0%; }
        .late-alert { background: var(--danger); color: white; padding: 4px; border-radius: 4px; font-size: 10px; font-weight: 800; animation: blink 1s infinite; text-align: center; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.3; } 100% { opacity: 1; } }

        /* ADMIN STATS */
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 10px; }
        .stat-box { padding: 10px; border-radius: 8px; text-align: center; color: white; font-size: 12px; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen" style="height:95vh; display:flex; flex-direction:column; align-items:center; justify-content:center;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="max-width:140px; border-radius:50%; margin-bottom:20px;">
    <div style="width:280px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()">CONNEXION SYSTÈME</button>
    </div>
</div>

<div id="app" class="hidden">
    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="nav-caisse" class="active">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="nav-atelier">👕 ATELIER</button>
        <button onclick="tab('section-stocks')" id="nav-stocks">🧼 STOCKS</button>
        <button onclick="tab('section-admin')" id="nav-admin">👑 ADMIN</button>
    </nav>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <input type="tel" id="tel-client" placeholder="WhatsApp (Recherche...)" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom du Client">
            <label style="font-size:11px; font-weight:800; color:var(--primary);">DATE DE RETRAIT PRÉVUE :</label>
            <input type="date" id="date-retrait">
            <select id="select-article"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary);">+ AJOUTER ARTICLE</button>
        </div>
        <div id="panier-liste"></div>
        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between; align-items:center;"><span>TOTAL:</span><b id="total-caisse" style="font-size:22px; color:var(--success);">0 F</b></div>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:8px; margin-top:10px;">
                <select id="mode-pay"><option value="Cash">💵 Cash</option><option value="OM">🍊 Orange</option><option value="Momo">🟡 Momo</option></select>
                <input type="number" id="paye-caisse" placeholder="Avance (F)" oninput="calculerReste()">
            </div>
            <div id="reste-caisse" style="text-align:right; font-size:12px; margin-top:5px; color:var(--warning);">Reste: 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:55px;">🖨️ ENCAISSER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <h3 style="margin-left:10px;">👕 Travaux en cours</h3>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 État des Consommables</h3>
            <div id="stock-display"></div>
            <hr>
            <h4>📦 Approvisionnement manuel</h4>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" style="background:var(--warning); color:black;">METTRE À JOUR LE STOCK</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card" style="border-top:5px solid var(--primary);">
            <h3>📊 Ventilation des Recettes</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:5px;">
                <button onclick="afficherStats('jour')" id="sj">JOUR</button>
                <button onclick="afficherStats('mois')" id="sm">MOIS</button>
                <button onclick="afficherStats('an')" id="sa">AN</button>
            </div>
            <div id="admin-stats-ui" style="margin-top:15px;"></div>
        </div>

        <div class="card">
            <h3>📡 Connectivité Imprimante</h3>
            <select id="printer-type">
                <option value="bt">Bluetooth (Thermique)</option>
                <option value="wifi">Wi-Fi (Réseau local)</option>
                <option value="usb">USB (Câblé)</option>
            </select>
            <button onclick="alert('Configuration Bluetooth/Wi-Fi mise à jour')">TESTER LA CONNEXION</button>
        </div>
        
        <div class="card">
            <h3>👥 Gestion du Personnel</h3>
            <div id="staff-list"></div>
            <hr>
            <input type="text" id="ns-nom" placeholder="Nom">
            <input type="text" id="ns-log" placeholder="Identifiant">
            <input type="password" id="ns-pass" placeholder="Mot de passe">
            <button onclick="ajouterEmploye()" style="background:var(--success);">+ AJOUTER EMPLOYÉ</button>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; width:80mm; padding:10px;">
    <center><b>SUPER CLEAN PRESSING</b><br>Nkounga, Immeuble Karche</center>
    <div id="tk-content" style="font-size:12px; margin-top:10px;"></div>
</div>

<script>
// INITIALISATION DES DONNÉES
let db = JSON.parse(localStorage.getItem('sc_db')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks')) || [
    {id:'savon', nom:"Savon Liquide", qte:20, unite:"L"}, 
    {id:'parfum', nom:"Parfum", qte:5, unite:"L"},
    {id:'cintres', nom:"Cintres", qte:100, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [];
let currentUser = null;

const GRILLE = {
    "HAUTS": {"Chemise": 500, "Veste": 1500, "T-Shirt": 500, "Pull": 700},
    "BAS": {"Pantalon": 500, "Jupe": 500, "Jupe plissée": 1000},
    "TENUES": {"Costume 2p": 2500, "Robe soirée": 3000, "Boubou": 2000},
    "MAISON": {"Drap": 1000, "Couette": 2000, "Rideau": 1500}
};

// 1. GESTION ADMIN & CONNEXION
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const found = staff.find(s => s.login === l && s.pass === p);
    if(found) { 
        currentUser = found; 
        document.getElementById('auth-screen').classList.add('hidden'); 
        document.getElementById('app').classList.remove('hidden'); 
        initApp(); 
    } else alert("Accès refusé");
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Choisir Article --</option>';
    for(let cat in GRILLE) {
        let group = document.createElement('optgroup'); group.label = cat;
        for(let art in GRILLE[cat]) {
            let o = document.createElement('option'); o.value = GRILLE[cat][art];
            o.innerText = `${art} (${GRILLE[cat][art]}F)`; group.appendChild(o);
        }
        s.appendChild(group);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(x => `<option value="${x.id}">${x.nom}</option>`).join('');
    afficherStocks();
}

// 2. STOCKS DYNAMIQUES (DEDUCTION & MODIF MANUELLE)
function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div style="display:flex; justify-content:space-between; padding:5px 0; border-bottom:1px solid #eee;">
            <span>${s.nom}</span><b style="${s.qte < 2 ? 'color:red' : ''}">${s.qte.toFixed(2)} ${s.unite}</b>
        </div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value;
    const q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(q) { 
        stocks.find(s => s.id === id).qte += q; 
        localStorage.setItem('sc_stocks', JSON.stringify(stocks)); 
        afficherStocks();
        alert("Stock mis à jour !");
    }
}

// 3. CAISSE
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    panier.push({uid: Date.now()+Math.random(), nom: s.options[s.selectedIndex].text.split(' (')[0], prix: parseInt(s.value), pret: false, loc: ""});
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `
        <div class="card" style="padding:10px; margin-bottom:5px; display:flex; justify-content:space-between;">
            <span>${x.nom}</span><b>${x.prix}F <span onclick="panier.splice(${i},1);renderPanier()" style="color:red; cursor:pointer;">✕</span></b>
        </div>`).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    const p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste à payer: ${t-p} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value;
    const dateR = document.getElementById('date-retrait').value;
    if(!nom || !dateR || panier.length === 0) return alert("Données manquantes");

    const ticket = {
        id: Math.floor(1000 + Math.random()*8999),
        client: nom, date_depot: new Date().toLocaleDateString('fr-FR'),
        retrait: dateR, articles: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0),
        paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value, statut: "Atelier", timestamp: new Date().getTime()
    };

    // DEDUCTION AUTO STOCKS
    stocks[0].qte -= 0.05; // Savon
    stocks[1].qte -= 0.01; // Parfum
    localStorage.setItem('sc_stocks', JSON.stringify(stocks));

    db.unshift(ticket);
    localStorage.setItem('sc_db', JSON.stringify(db));
    alert("Ticket #" + ticket.id + " Enregistré !");
    location.reload();
}

// 4. SUIVI ATELIER (PROGRESSION & ALERTES RETARD)
function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const encours = db.filter(t => t.statut !== "Livré");
    const today = new Date().toISOString().split('T')[0];

    list.innerHTML = encours.map(t => {
        const fini = t.articles.filter(a => a.pret).length;
        const prog = Math.round((fini / t.articles.length) * 100);
        const estRetard = t.retrait < today;

        return `
            <div class="card" style="border-left: 5px solid ${prog===100?'var(--success)':'var(--primary)'}">
                ${estRetard ? '<div class="late-alert">⚠️ CLIENT ATTEND (RETARD)</div>' : ''}
                <div style="display:flex; justify-content:space-between; margin-top:5px;">
                    <b>#${t.id} - ${t.client}</b>
                    <small>Retrait: ${t.retrait}</small>
                </div>
                <div class="progress-container"><div class="progress-fill" style="width:${prog}%"></div></div>
                <div style="font-size:11px; margin-bottom:10px;">Progression: ${prog}% (${fini}/${t.articles.length} pièces)</div>
                ${t.articles.map(art => `
                    <div style="display:flex; justify-content:space-between; padding:5px; border-bottom:1px solid #f0f0f0; font-size:12px;">
                        <span>${art.nom} ${art.loc ? `(📍 ${art.loc})`:''}</span>
                        ${!art.pret ? `<button onclick="finirPiece(${t.id}, ${art.uid})" style="width:auto; padding:3px 8px; font-size:10px; background:var(--success);">FINI</button>` : '✅'}
                    </div>`).join('')}
                ${prog === 100 ? `<button onclick="livrer(${t.id})" style="background:black; margin-top:10px;">📦 LIVRER (Solde: ${t.total - t.paye}F)</button>` : ''}
            </div>`;
    }).join('');
}

function finirPiece(tid, auid) {
    const loc = prompt("Emplacement de rangement (ex: Casier A1) ?");
    if(!loc) return;
    const t = db.find(x => x.id === tid);
    const a = t.articles.find(x => x.uid === auid);
    a.pret = true; a.loc = loc;
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

function livrer(id) {
    db.find(x => x.id === id).statut = "Livré";
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

// 5. TABLEAU DE BORD (VENTILATION PAIEMENTS & PERSONNEL)
function afficherStats(periode) {
    const now = new Date();
    let cash = 0, om = 0, momo = 0;

    db.forEach(t => {
        const d = new Date(t.timestamp);
        let match = false;
        if(periode === 'jour' && d.toDateString() === now.toDateString()) match = true;
        if(periode === 'mois' && d.getMonth() === now.getMonth() && d.getFullYear() === now.getFullYear()) match = true;
        if(periode === 'an' && d.getFullYear() === now.getFullYear()) match = true;

        if(match) {
            if(t.mode === "Cash") cash += t.paye;
            if(t.mode === "OM") om += t.paye;
            if(t.mode === "Momo") momo += t.paye;
        }
    });

    document.getElementById('admin-stats-ui').innerHTML = `
        <div class="stat-grid">
            <div class="stat-box" style="background:#2ecc71">💵 Cash<br><b>${cash} F</b></div>
            <div class="stat-box" style="background:#ff6600">🍊 Orange<br><b>${om} F</b></div>
            <div class="stat-box" style="background:#f1c40f; color:black">🟡 Momo<br><b>${momo} F</b></div>
            <div class="stat-box" style="background:var(--dark)">💎 TOTAL<br><b>${cash+om+momo} F</b></div>
        </div>`;
}

function ajouterEmploye() {
    const n = document.getElementById('ns-nom').value;
    const l = document.getElementById('ns-log').value;
    const p = document.getElementById('ns-pass').value;
    if(n && l && p) {
        staff.push({nom:n, login:l, pass:p, role:"STAFF"});
        localStorage.setItem('sc_staff', JSON.stringify(staff));
        afficherPersonnel();
        alert("Employé ajouté !");
    }
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e, i) => `
        <div style="display:flex; justify-content:space-between; padding:8px; border-bottom:1px solid #eee;">
            <span>${e.nom} (${e.role})</span>
            ${i !== 0 ? `<button onclick="staff.splice(${i},1);localStorage.setItem('sc_staff',JSON.stringify(staff));afficherPersonnel()" style="width:auto; padding:2px 8px; background:var(--danger); font-size:10px;">Suppr.</button>` : ''}
        </div>`).join('');
}

// NAVIGATION
function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') afficherAtelier();
    if(id === 'section-admin') { afficherStats('jour'); afficherPersonnel(); }
    if(id === 'section-stocks') afficherStocks();
}

function rechercherClient() {
    const tel = document.getElementById('tel-client').value;
    if(tel.length >= 8) {
        const found = db.find(t => t.tel && t.tel.includes(tel));
        if(found) document.getElementById('client-nom').value = found.client;
    }
}
</script>
</body>
</html>
