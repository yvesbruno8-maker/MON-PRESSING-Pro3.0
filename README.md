<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN SYSTEM v5.0 - GESTION TOTALE & ACCESSOIRES</title>
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
        button:active { transform: scale(0.96); }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; position: sticky; top: 0; z-index: 1000; background: var(--light); padding: 5px 0; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 8px; height: 50px; font-weight: 800; padding: 2px; }
        .nav-bar button.active { background: var(--primary); color: white; box-shadow: 0 4px 10px rgba(30, 55, 153, 0.3); }

        .progress-container { background: #e0e0e0; height: 14px; border-radius: 20px; margin: 10px 0; overflow: hidden; border: 1px solid #ccc; }
        .progress-fill { background: linear-gradient(90deg, #2ecc71, #27ae60); height: 100%; transition: 0.6s ease-in-out; width: 0%; }
        .late-alert { background: var(--danger); color: white; padding: 6px; border-radius: 6px; font-size: 10px; font-weight: 800; animation: blink 0.8s infinite; text-align: center; margin-bottom: 10px; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }

        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 10px; }
        .stat-box { padding: 15px; border-radius: 10px; text-align: center; color: white; font-weight: bold; }
        .stat-val { font-size: 18px; font-weight: 800; display: block; }

        .item-line { display: flex; justify-content: space-between; align-items: center; padding: 10px; border-bottom: 1px solid #f1f1f1; }
        .item-ready { background: #f0fff4 !important; color: #27ae60; }
        .emp-badge { font-size: 9px; background: #eee; padding: 2px 5px; border-radius: 4px; color: #666; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="max-width:140px; border-radius:50%; border: 4px solid white; box-shadow: 0 10px 20px rgba(0,0,0,0.1);">
    <h1 style="color:var(--primary); font-weight:900; margin-top:15px;">SUPER CLEAN</h1>
    <div style="width:290px;">
        <input type="text" id="u-login" placeholder="Identifiant (admin)">
        <input type="password" id="u-pass" placeholder="Mot de passe (0000)">
        <button onclick="login()" style="height:55px; font-size:16px; margin-top:10px;">OUVRIR SESSION</button>
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
            <div id="user-display" style="font-size:10px; color:var(--primary); font-weight:bold; margin-bottom:5px;">Opérateur : -</div>
            <input type="tel" id="tel-client" placeholder="WhatsApp (Recherche automatique...)" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom Complet du Client">
            <label style="font-size:11px; font-weight:bold; color:var(--primary);">DATE DE RETRAIT PRÉVUE :</label>
            <input type="date" id="date-retrait">
            <select id="select-article" style="border: 2px solid var(--secondary); font-weight:bold;"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary); height:50px;">+ AJOUTER L'ARTICLE</button>
        </div>

        <div id="panier-liste"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <span>TOTAL À ENCAISSER :</span>
                <b id="total-caisse" style="font-size:24px; color:var(--success);">0 F</b>
            </div>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:8px; margin-top:10px;">
                <select id="mode-pay">
                    <option value="Cash">💵 Cash (Espèces)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN Momo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Avance" oninput="calculerReste()">
            </div>
            <div id="reste-caisse" style="text-align:right; font-size:12px; color:var(--warning); font-weight:800;">Reste : 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:65px; font-size:18px;">🖨️ VALIDER & ENCAISSER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <h3 style="margin-left:10px;">👕 Travaux & Journal d'Atelier</h3>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 État des Stocks Dynamiques</h3>
            <div id="stock-display"></div>
            <hr>
            <h4>📦 Approvisionnement manuel</h4>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" style="background:var(--warning); color:black;">METTRE À JOUR LE STOCK</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card">
            <h3>📊 Ventilation des Recettes</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:5px;">
                <button onclick="afficherStats('jour')" id="sj">JOUR</button>
                <button onclick="afficherStats('mois')" id="sm">MOIS</button>
                <button onclick="afficherStats('an')" id="sa">AN</button>
            </div>
            <div id="admin-stats-ui" style="margin-top:15px;"></div>
        </div>

        <div class="card">
            <h3>👥 Gestion du Personnel</h3>
            <div id="staff-list"></div>
            <hr>
            <h4>➕ Ajouter un employé</h4>
            <input type="text" id="ns-nom" placeholder="Nom">
            <input type="text" id="ns-log" placeholder="Login">
            <input type="password" id="ns-pass" placeholder="Mot de passe">
            <button onclick="ajouterEmploye()" style="background:var(--success);">CRÉER LE COMPTE</button>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; width:80mm; padding:10px;">
    <center><b style="font-size:16px;">SUPER CLEAN PRESSING</b><br>NKOUONGA, IMMEUBLE KARCHE<br>--------------------------------</center>
    <div id="tk-content" style="font-size:12px; margin-top:10px;"></div>
    <center><br>*** MERCI DE VOTRE CONFIANCE ***</center>
</div>

<script>
// --- GRILLE TARIFAIRE ENTIÈRE SANS SIMPLIFICATION ---
const GRILLE = {
    "HAUTS": {
        "Chemise Homme": 500,
        "Chemise Amidonnee": 700,
        "Veste Seule": 1500,
        "T-Shirt": 500,
        "Polo": 500,
        "Pull / Sweat": 700,
        "Manteau": 2500,
        "Blouson": 3500
    },
    "BAS": {
        "Pantalon Classique": 500,
        "Pantalon Lin": 800,
        "Short": 400,
        "Jupe Simple": 500,
        "Jupe Plissee": 1000,
        "Combinaison": 1500
    },
    "TENUES": {
        "Costume 2 Pieces": 2500,
        "Costume 3 Pieces": 3500,
        "Robe de Soiree": 3000,
        "Robe de Mariee": 15000,
        "Robe Simple": 1000,
        "Toge Graduation": 3500,
        "Boubou Simple": 2000,
        "Boubou Bazin Amidon": 3500
    },
    "MAISON": {
        "Drap": 1000,
        "Taie oreiller": 300,
        "Couette": 2500,
        "Couverture": 3000,
        "Rideau (m2)": 1500,
        "Nappe de table": 700,
        "Serviette de bain": 500
    },
    "ACCESSOIRES": {
        "Tennis": 1500,
        "Tennis Cuir": 2500,
        "Cravate": 500,
        "Casquette": 500,
        "Sac à main": 2500,
        "Echarpe": 500
    }
};

let db = JSON.parse(localStorage.getItem('sc_db_v5')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v5')) || [
    {id:'savon', nom:"Savon Liquide", qte:50, unite:"L"}, 
    {id:'parfum', nom:"Parfum", qte:10, unite:"L"},
    {id:'housses', nom:"Housses Plastiques", qte:200, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff_v5')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [];
let currentUser = null;

// --- LOGIQUE AUTH ---
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const user = staff.find(s => s.login === l && s.pass === p);
    if(user) {
        currentUser = user;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('user-display').innerText = `Opérateur : ${currentUser.nom}`;
        initApp();
    } else alert("Identifiants incorrects !");
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Sélectionner un article --</option>';
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

// --- LOGIQUE CAISSE ---
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    panier.push({
        uid: Date.now() + Math.random(),
        nom: s.options[s.selectedIndex].text.split(' (')[0],
        prix: parseInt(s.value),
        pret: false,
        loc: "",
        done_by: ""
    });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `
        <div class="item-line">
            <span>${x.nom}</span>
            <b>${x.prix} F <span onclick="panier.splice(${i},1);renderPanier()" style="color:var(--danger); cursor:pointer; margin-left:10px;">✕</span></b>
        </div>`).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    const p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${Math.max(0, t - p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value;
    const dateR = document.getElementById('date-retrait').value;
    if(!nom || !dateR || panier.length === 0) return alert("Veuillez remplir toutes les informations !");

    const ticket = {
        id: Math.floor(1000 + Math.random()*8999),
        client: nom,
        tel: document.getElementById('tel-client').value,
        articles: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0),
        paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value,
        date_depot: new Date().toLocaleDateString('fr-FR'),
        retrait: dateR,
        caissier: currentUser.nom,
        statut: "Atelier",
        timestamp: new Date().getTime()
    };

    // Déduction auto stocks dynamique
    stocks[0].qte -= (panier.length * 0.05); // Savon
    stocks[1].qte -= (panier.length * 0.01); // Parfum
    stocks[2].qte -= panier.length;         // Housses
    localStorage.setItem('sc_stocks_v5', JSON.stringify(stocks));

    db.unshift(ticket);
    localStorage.setItem('sc_db_v5', JSON.stringify(db));

    document.getElementById('tk-content').innerHTML = `
        TICKET #${ticket.id}<br>CAISSIER: ${ticket.caissier}<br>CLIENT: ${ticket.client}<br>RETRAIT LE: ${ticket.retrait}<hr>
        ${panier.map(a => `${a.nom} : ${a.prix}F`).join('<br>')}<hr>
        TOTAL: ${ticket.total}F | SOLDE: ${ticket.total - ticket.paye}F
    `;
    window.print();
    location.reload();
}

// --- LOGIQUE ATELIER & TRAÇABILITÉ ---
function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const orders = db.filter(t => t.statut !== "Livré");
    const today = new Date().toISOString().split('T')[0];

    list.innerHTML = orders.map(t => {
        const fini = t.articles.filter(a => a.pret).length;
        const total = t.articles.length;
        const prog = Math.round((fini/total)*100);
        const retard = t.retrait < today;

        return `
            <div class="card" style="border-left: 6px solid ${prog===100?'var(--success)':'var(--primary)'}">
                ${retard && prog < 100 ? '<div class="late-alert">⚠️ RETARD DE LIVRAISON</div>' : ''}
                <div style="display:flex; justify-content:space-between;"><b>#${t.id} - ${t.client}</b><small>Le: ${t.retrait}</small></div>
                <div class="progress-container"><div class="progress-fill" style="width:${prog}%"></div></div>
                <div style="font-size:10px; margin-bottom:5px;">Encaissé par : ${t.caissier}</div>
                ${t.articles.map(art => `
                    <div class="item-line ${art.pret ? 'item-ready' : ''}">
                        <span>${art.nom} ${art.loc ? `<span style="background:var(--dark); color:white; padding:2px 4px; border-radius:3px; font-size:9px;">📍 ${art.loc}</span>` : ''}</span>
                        ${!art.pret ? 
                            `<button onclick="marquerFini(${t.id}, ${art.uid})" style="width:auto; padding:4px 8px; font-size:10px; background:var(--success);">TERMINER</button>` : 
                            `<span class="emp-badge">Fait par: ${art.done_by}</span>`}
                    </div>`).join('')}
                ${prog === 100 ? `<button onclick="livrerFinal(${t.id})" style="background:#000; margin-top:10px;">📦 LIVRER (Solde: ${t.total-t.paye}F)</button>` : ''}
            </div>`;
    }).join('');
}

function marquerFini(tid, auid) {
    const loc = prompt("Emplacement (Casier/Rayon) ?");
    if(!loc) return;
    const ticket = db.find(x => x.id === tid);
    const art = ticket.articles.find(a => a.uid === auid);
    art.pret = true;
    art.loc = loc;
    art.done_by = currentUser.nom;
    localStorage.setItem('sc_db_v5', JSON.stringify(db));
    afficherAtelier();
}

function livrerFinal(id) {
    const t = db.find(x => x.id === id);
    if(confirm(`Confirmer la livraison finale ? Solde à percevoir : ${t.total - t.paye}F`)) {
        t.statut = "Livré";
        t.paye = t.total;
        localStorage.setItem('sc_db_v5', JSON.stringify(db));
        afficherAtelier();
    }
}

// --- STATS & ADMIN PERSONNEL ---
function afficherStats(periode) {
    if(currentUser.role !== "ADMIN") return alert("Accès Gérant uniquement !");
    const now = new Date();
    let cash = 0, om = 0, momo = 0;
    db.forEach(t => {
        const d = new Date(t.timestamp);
        let match = false;
        if(periode==='jour' && d.toDateString() === now.toDateString()) match = true;
        if(periode==='mois' && d.getMonth() === now.getMonth() && d.getFullYear() === now.getFullYear()) match = true;
        if(periode==='an' && d.getFullYear() === now.getFullYear()) match = true;
        if(match) {
            if(t.mode === "Cash") cash += t.paye;
            if(t.mode === "OM") om += t.paye;
            if(t.mode === "Momo") momo += t.paye;
        }
    });
    document.getElementById('admin-stats-ui').innerHTML = `
        <div class="stat-grid">
            <div class="stat-box" style="background:#2ecc71"><span class="stat-val">${cash} F</span>Cash</div>
            <div class="stat-box" style="background:#ff6600"><span class="stat-val">${om} F</span>Orange</div>
            <div class="stat-box" style="background:#f1c40f; color:black;"><span class="stat-val">${momo} F</span>Momo</div>
            <div class="stat-box" style="background:var(--dark); grid-column: span 2;"><span class="stat-val">${cash+om+momo} F</span>RECETTE TOTALE</div>
        </div>`;
}

function ajouterEmploye() {
    if(currentUser.role !== "ADMIN") return alert("Accès réservé !");
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-log').value, p = document.getElementById('ns-pass').value;
    if(n && l && p) {
        staff.push({nom:n, login:l, pass:p, role:"STAFF"});
        localStorage.setItem('sc_staff_v5', JSON.stringify(staff));
        afficherPersonnel();
        alert("Compte créé !");
    }
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e, i) => `
        <div class="item-line">
            <span><b>${e.nom}</b></span>
            ${i !== 0 ? `<button onclick="supprimerStaff(${i})" style="width:auto; padding:5px; background:var(--danger); font-size:10px;">Supprimer</button>` : '<i>(Gérant)</i>'}
        </div>`).join('');
}

function supprimerStaff(i) {
    if(confirm("Supprimer ce compte ?")) {
        staff.splice(i,1);
        localStorage.setItem('sc_staff_v5',JSON.stringify(staff));
        afficherPersonnel();
    }
}

function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div class="item-line"><span>${s.nom}</span><b>${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(!isNaN(q)) { stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks_v5', JSON.stringify(stocks)); afficherStocks(); }
}

// --- NAV ---
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
