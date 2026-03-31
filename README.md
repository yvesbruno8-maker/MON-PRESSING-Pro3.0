<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN v6.0 - MULTI-UTILISATEURS</title>
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
        
        /* Navigation */
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; position: sticky; top: 0; z-index: 1000; background: var(--light); padding: 5px 0; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 8px; height: 50px; font-weight: 800; padding: 2px; }
        .nav-bar button.active { background: var(--primary); color: white; box-shadow: 0 4px 10px rgba(30, 55, 153, 0.3); }

        /* Header Session */
        .session-info { display: flex; justify-content: space-between; align-items: center; background: var(--dark); color: white; padding: 8px 12px; border-radius: 8px; margin-bottom: 10px; font-size: 11px; }
        .btn-logout { width: auto; padding: 5px 10px; background: var(--danger); font-size: 10px; margin: 0; }

        /* Atelier & Alertes */
        .progress-container { background: #e0e0e0; height: 14px; border-radius: 20px; margin: 10px 0; overflow: hidden; border: 1px solid #ccc; }
        .progress-fill { background: linear-gradient(90deg, #2ecc71, #27ae60); height: 100%; transition: 0.6s ease-in-out; width: 0%; }
        .late-alert { background: var(--danger); color: white; padding: 6px; border-radius: 6px; font-size: 10px; font-weight: 800; animation: blink 0.8s infinite; text-align: center; margin-bottom: 10px; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }

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
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="height:55px; font-size:16px; margin-top:10px;">SE CONNECTER</button>
    </div>
</div>

<div id="app" class="hidden">
    <div class="session-info">
        <span>👤 <b id="user-display">-</b></span>
        <button class="btn-logout" onclick="logout()">CHANGER D'UTILISATEUR</button>
    </div>

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
            <label style="font-size:11px; font-weight:bold; color:var(--primary);">DATE DE RETRAIT :</label>
            <input type="date" id="date-retrait">
            <select id="select-article" style="border: 2px solid var(--secondary);"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary);">+ AJOUTER AU PANIER</button>
        </div>

        <div id="panier-liste"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <span>TOTAL :</span>
                <b id="total-caisse" style="font-size:24px; color:var(--success);">0 F</b>
            </div>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:8px; margin-top:10px;">
                <select id="mode-pay">
                    <option value="Cash">💵 Cash</option>
                    <option value="OM">🍊 OM</option>
                    <option value="Momo">🟡 Momo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Avance" oninput="calculerReste()">
            </div>
            <div id="reste-caisse" style="text-align:right; font-size:12px; color:var(--warning); font-weight:800;">Reste : 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:65px; font-size:18px;">🖨️ VALIDER TICKET</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <h3 style="margin-left:10px;">👕 Journal d'Atelier</h3>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 Stocks</h3>
            <div id="stock-display"></div>
            <hr>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" style="background:var(--warning); color:black;">AJOUTER AU STOCK</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card">
            <h3>📊 Recettes</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:5px;">
                <button onclick="afficherStats('jour')" id="sj">JOUR</button>
                <button onclick="afficherStats('mois')" id="sm">MOIS</button>
                <button onclick="afficherStats('an')" id="sa">AN</button>
            </div>
            <div id="admin-stats-ui" style="margin-top:15px;"></div>
        </div>
        <div class="card">
            <h3>👥 Personnel</h3>
            <div id="staff-list"></div>
            <hr>
            <input type="text" id="ns-nom" placeholder="Nom">
            <input type="text" id="ns-log" placeholder="Login">
            <input type="password" id="ns-pass" placeholder="Mot de passe">
            <button onclick="ajouterEmploye()" style="background:var(--success);">CRÉER COMPTE</button>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; width:80mm; padding:10px;">
    <center><b style="font-size:16px;">SUPER CLEAN PRESSING</b><hr></center>
    <div id="tk-content" style="font-size:12px;"></div>
</div>

<script>
// --- GRILLE TARIFAIRE COMPLÈTE ---
const GRILLE = {
    "HAUTS": {"Chemise Homme": 500, "Chemise Amidonnee": 700, "Veste Seule": 1500, "T-Shirt": 500, "Polo": 500, "Pull": 700, "Manteau": 2500, "Blouson": 3500},
    "BAS": {"Pantalon": 500, "Pantalon Lin": 800, "Short": 400, "Jupe Simple": 500, "Jupe Plissee": 1000, "Combinaison": 1500},
    "TENUES": {"Costume 2P": 2500, "Costume 3P": 3500, "Robe Soiree": 3000, "Robe Mariee": 15000, "Robe Simple": 1000, "Toge": 3500, "Boubou Simple": 2000, "Boubou Bazin": 3500},
    "MAISON": {"Drap": 1000, "Taie": 300, "Couette": 2500, "Couverture": 3000, "Rideau": 1500, "Nappe": 700, "Serviette": 500},
    "ACCESSOIRES": {"Tennis": 1500, "Tennis Cuir": 2500, "Cravate": 500, "Casquette": 500, "Sac": 2500, "Echarpe": 500}
};

let db = JSON.parse(localStorage.getItem('sc_db_v6')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v6')) || [
    {id:'savon', nom:"Savon Liquide", qte:50, unite:"L"}, 
    {id:'parfum', nom:"Parfum", qte:10, unite:"L"},
    {id:'housses', nom:"Housses", qte:200, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff_v6')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [];
let currentUser = null;

// --- GESTION UTILISATEURS (VERROUILLÉ) ---
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const user = staff.find(s => s.login === l && s.pass === p);
    if(user) {
        currentUser = user;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('user-display').innerText = currentUser.nom;
        initApp();
    } else alert("Erreur d'accès !");
}

function logout() {
    if(confirm("Voulez-vous changer d'utilisateur ?")) {
        currentUser = null;
        panier = [];
        document.getElementById('app').classList.add('hidden');
        document.getElementById('auth-screen').classList.remove('hidden');
        document.getElementById('u-login').value = "";
        document.getElementById('u-pass').value = "";
    }
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Article --</option>';
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

// --- CAISSE ---
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    panier.push({ uid: Date.now()+Math.random(), nom: s.options[s.selectedIndex].text.split(' (')[0], prix: parseInt(s.value), pret: false, loc: "", done_by: "" });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `<div class="item-line"><span>${x.nom}</span><b>${x.prix}F <span onclick="panier.splice(${i},1);renderPanier()" style="color:var(--danger)">✕</span></b></div>`).join('');
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
    if(!nom || !dateR || panier.length === 0) return alert("Incomplet !");

    const ticket = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: document.getElementById('tel-client').value,
        articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value, date_depot: new Date().toLocaleDateString('fr-FR'), retrait: dateR,
        caissier: currentUser.nom, statut: "Atelier", timestamp: new Date().getTime()
    };

    stocks[0].qte -= (panier.length * 0.05); stocks[1].qte -= (panier.length * 0.01); stocks[2].qte -= panier.length;
    localStorage.setItem('sc_stocks_v6', JSON.stringify(stocks));
    db.unshift(ticket);
    localStorage.setItem('sc_db_v6', JSON.stringify(db));
    
    document.getElementById('tk-content').innerHTML = `TICKET #${ticket.id}<br>CAISSIER: ${ticket.caissier}<br>CLIENT: ${ticket.client}<hr>${panier.map(a => `${a.nom}: ${a.prix}F`).join('<br>')}<hr>SOLDE: ${ticket.total - ticket.paye}F`;
    window.print();
    location.reload();
}

// --- ATELIER ---
function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const orders = db.filter(t => t.statut !== "Livré");
    const today = new Date().toISOString().split('T')[0];

    list.innerHTML = orders.map(t => {
        const fini = t.articles.filter(a => a.pret).length;
        const prog = Math.round((fini/t.articles.length)*100);
        return `
            <div class="card" style="border-left: 6px solid ${prog===100?'var(--success)':'var(--primary)'}">
                ${t.retrait < today && prog < 100 ? '<div class="late-alert">⚠️ RETARD</div>' : ''}
                <b>#${t.id} - ${t.client}</b>
                <div class="progress-container"><div class="progress-fill" style="width:${prog}%"></div></div>
                ${t.articles.map(art => `
                    <div class="item-line ${art.pret ? 'item-ready' : ''}">
                        <span>${art.nom}</span>
                        ${!art.pret ? `<button onclick="marquerFini(${t.id}, ${art.uid})" style="width:auto; padding:4px; font-size:10px; background:var(--success);">FINI</button>` : `<span class="emp-badge">Par: ${art.done_by}</span>`}
                    </div>`).join('')}
                ${prog === 100 ? `<button onclick="livrer(${t.id})" style="background:#000;">LIVRER (Solde: ${t.total-t.paye}F)</button>` : ''}
            </div>`;
    }).join('');
}

function marquerFini(tid, auid) {
    const loc = prompt("Emplacement ?"); if(!loc) return;
    const t = db.find(x => x.id === tid); const art = t.articles.find(a => a.uid === auid);
    art.pret = true; art.loc = loc; art.done_by = currentUser.nom;
    localStorage.setItem('sc_db_v6', JSON.stringify(db));
    afficherAtelier();
}

function livrer(id) {
    const t = db.find(x => x.id === id);
    if(confirm("Confirmer la livraison ?")) { t.statut = "Livré"; t.paye = t.total; localStorage.setItem('sc_db_v6', JSON.stringify(db)); afficherAtelier(); }
}

// --- ADMIN ---
function afficherStats(periode) {
    if(currentUser.role !== "ADMIN") return alert("Accès réservé !");
    const now = new Date(); let cash = 0, om = 0, momo = 0;
    db.forEach(t => {
        const d = new Date(t.timestamp); let match = false;
        if(periode==='jour' && d.toDateString() === now.toDateString()) match = true;
        if(periode==='mois' && d.getMonth() === now.getMonth()) match = true;
        if(match) { if(t.mode === "Cash") cash += t.paye; if(t.mode === "OM") om += t.paye; if(t.mode === "Momo") momo += t.paye; }
    });
    document.getElementById('admin-stats-ui').innerHTML = `<div class="stat-grid"><div class="stat-box" style="background:#2ecc71">${cash} F<br>Cash</div><div class="stat-box" style="background:#ff6600">${om} F<br>OM</div><div class="stat-box" style="background:#f1c40f; color:black;">${momo} F<br>Momo</div></div>`;
}

function ajouterEmploye() {
    if(currentUser.role !== "ADMIN") return;
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-log').value, p = document.getElementById('ns-pass').value;
    if(n && l && p) { staff.push({nom:n, login:l, pass:p, role:"STAFF"}); localStorage.setItem('sc_staff_v6', JSON.stringify(staff)); afficherPersonnel(); }
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e, i) => `<div class="item-line"><span>${e.nom}</span>${i!==0?`<button onclick="staff.splice(${i},1);localStorage.setItem('sc_staff_v6',JSON.stringify(staff));afficherPersonnel()" style="width:auto; padding:4px; background:var(--danger); font-size:10px;">Suppr</button>`:''}</div>`).join('');
}

function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-line"><span>${s.nom}</span><b>${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(q) { stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks_v6', JSON.stringify(stocks)); afficherStocks(); }
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

function rechercherClient() {
    const tel = document.getElementById('tel-client').value;
    if(tel.length >= 8) { const found = db.find(t => t.tel && t.tel.includes(tel)); if(found) document.getElementById('client-nom').value = found.client; }
}
</script>
</body>
</html>
