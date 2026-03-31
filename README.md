<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING - GESTION PRO v21.0</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f8f9fa; --radius: 12px; --shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); }
        .hidden { display: none !important; }
        .card { background: white; padding: 15px; border-radius: var(--radius); box-shadow: var(--shadow); margin-bottom: 12px; border: 1px solid #eee; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 8px; border: 1px solid #ccc; font-size: 14px; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; }
        button:active { opacity: 0.8; }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 9px; height: 45px; padding: 2px; }
        .nav-bar button.active { background: var(--primary); color: white; }

        .logo-img { max-width: 120px; display: block; margin: 0 auto 10px; }
        .stock-item { display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px solid #eee; }
        .badge-admin { background: gold; color: black; padding: 2px 5px; border-radius: 4px; font-size: 9px; font-weight: bold; }

        @media print {
            body * { visibility: hidden; }
            #ticket-print, #ticket-print * { visibility: visible; }
            #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; }
        }
    </style>
</head>
<body>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-img">
    <h2 style="color:var(--primary); margin:0;">SUPER CLEAN</h2>
    <div style="width:280px; margin-top:20px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()">SE CONNECTER</button>
    </div>
</div>

<div id="app" class="hidden">
    <header style="text-align:center; margin-bottom:10px;">
        <div id="user-display" style="font-size:11px; font-weight:bold; color:var(--primary);"></div>
        <button onclick="logout()" style="width:auto; padding:5px 10px; background:var(--danger); font-size:10px; margin-top:5px;">Déconnexion</button>
    </header>

    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="nav-caisse" class="active">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="nav-atelier">👕 ATELIER</button>
        <button onclick="tab('section-stocks')" id="nav-stocks">🧼 STOCKS</button>
        <button onclick="tab('section-admin')" id="nav-admin">👑 ADMIN</button>
    </nav>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <input type="tel" id="tel-client" placeholder="WhatsApp" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom du Client">
            <input type="date" id="date-retrait">
            <select id="select-article"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary);">AJOUTER</button>
        </div>
        <div id="panier-liste" class="card"></div>
        <div class="card" style="background:var(--dark); color:white;">
            Total: <b id="total-caisse">0 F</b>
            <select id="mode-pay" style="margin-top:10px;"><option value="Cash">Cash</option><option value="OM">Orange Money</option><option value="Momo">Momo</option></select>
            <input type="number" id="paye-caisse" placeholder="Avance versée" oninput="calculerReste()">
            <div id="reste-caisse" style="font-size:12px; color:var(--warning);">Reste: 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:50px;">VALIDER & IMPRIMER</button>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 Gestion des Approvisionnements</h3>
            <div id="stock-display"></div>
            <hr>
            <h4>Ajouter du stock (Arrivage)</h4>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" style="background:var(--warning); color:black;">METTRE À JOUR LE STOCK</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card">
            <h3>🖨️ Configuration Imprimante</h3>
            <select id="printer-type">
                <option value="system">Système (Par défaut)</option>
                <option value="bluetooth">Bluetooth (Thermique)</option>
                <option value="wifi">Réseau / Wi-Fi</option>
            </select>
            <input type="text" id="printer-ip" placeholder="Adresse IP (si Wi-Fi)" class="hidden">
            <button onclick="savePrinter()" style="background:var(--secondary);">Enregistrer l'imprimante</button>
        </div>

        <div class="card">
            <h3>👥 Gestion du Personnel</h3>
            <div id="staff-list"></div>
            <hr>
            <h4>Ajouter un employé</h4>
            <input type="text" id="new-staff-nom" placeholder="Nom complet">
            <input type="text" id="new-staff-login" placeholder="Identifiant">
            <input type="password" id="new-staff-pass" placeholder="Mot de passe">
            <select id="new-staff-role">
                <option value="STAFF">Staff (Caisse/Atelier)</option>
                <option value="ADMIN">Administrateur</option>
            </select>
            <button onclick="ajouterEmploye()" style="background:var(--success);">CRÉER LE COMPTE</button>
        </div>
    </div>
</div>

<div id="ticket-print" class="hidden" style="padding:10px; font-family:monospace;">
    <center>
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:40mm;">
        <h4>SUPER CLEAN PRESSING</h4>
        <p>Nkounga, Immeuble Karche</p>
    </center>
    <div id="tk-content"></div>
</div>

<script>
// INITIALISATION DES DONNÉES
let employees = JSON.parse(localStorage.getItem('sc_staff')) || [
    {nom: "Admin Principal", login: "admin", pass: "0000", role: "ADMIN"}
];
let stocks = JSON.parse(localStorage.getItem('sc_stocks')) || [
    {id: 'savon', nom: "Savon Liquide", qte: 50, unite: "L"},
    {id: 'parfum', nom: "Parfum Linge", qte: 10, unite: "L"},
    {id: 'cintre', nom: "Cintres", qte: 100, unite: "pcs"},
    {id: 'housse', nom: "Housses Plastiques", qte: 100, unite: "pcs"}
];
let db = JSON.parse(localStorage.getItem('sc_db')) || [];
let current_user = null;
let panier = [];

const TARIFS = {
    "HAUTS": {"Chemise": 500, "Veste": 1500, "T-shirt": 500, "Blouson": 1500},
    "ROBES": {"Robe simple": 500, "Robe soirée": 3000, "Toge": 3500},
    "LIT": {"Drap": 750, "Couette": 2000, "Couverture": 4000}
};

// --- AUTHENTIFICATION ---
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const user = employees.find(e => e.login === l && e.pass === p);
    
    if(user) {
        current_user = user;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('user-display').innerHTML = `${user.nom} ${user.role === 'ADMIN' ? '<span class="badge-admin">ADMIN</span>' : ''}`;
        initApp();
    } else { alert("Accès refusé"); }
}

function logout() { location.reload(); }

function initApp() {
    // Charger tarifs
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">Choisir Article...</option>';
    for(let cat in TARIFS) {
        for(let art in TARIFS[cat]) {
            let o = document.createElement('option');
            o.value = TARIFS[cat][art]; o.innerText = `${art} (${TARIFS[cat][art]}F)`;
            s.appendChild(o);
        }
    }
    // Charger liste stocks pour modif
    const sup = document.getElementById('upd-stock-item');
    sup.innerHTML = stocks.map(s => `<option value="${s.id}">${s.nom}</option>`).join('');
    afficherStocks();
}

// --- GESTION DU PERSONNEL (ADMIN UNIQUEMENT) ---
function ajouterEmploye() {
    if(current_user.role !== 'ADMIN') return alert("Action réservée à l'administrateur");
    const nom = document.getElementById('new-staff-nom').value;
    const login = document.getElementById('new-staff-login').value;
    const pass = document.getElementById('new-staff-pass').value;
    const role = document.getElementById('new-staff-role').value;

    if(!nom || !login || !pass) return alert("Remplissez tous les champs");
    employees.push({nom, login, pass, role});
    localStorage.setItem('sc_staff', JSON.stringify(employees));
    alert("Employé ajouté !");
    afficherPersonnel();
}

function supprimerEmploye(index) {
    if(current_user.role !== 'ADMIN') return alert("Action réservée à l'administrateur");
    if(confirm("Supprimer cet employé ?")) {
        employees.splice(index, 1);
        localStorage.setItem('sc_staff', JSON.stringify(employees));
        afficherPersonnel();
    }
}

function afficherPersonnel() {
    const list = document.getElementById('staff-list');
    list.innerHTML = employees.map((e, i) => `
        <div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;">
            <span>${e.nom} (<i>${e.role}</i>)</span>
            ${i !== 0 ? `<button onclick="supprimerEmploye(${i})" style="width:auto; background:red; padding:2px 10px;">Supprimer</button>` : '🔒'}
        </div>
    `).join('');
}

// --- GESTION STOCKS (APPROVISIONNEMENT) ---
function afficherStocks() {
    const div = document.getElementById('stock-display');
    div.innerHTML = stocks.map(s => `
        <div class="stock-item">
            <span>${s.nom}</span>
            <b>${parseFloat(s.qte.toFixed(2))} ${s.unite}</b>
        </div>
    `).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value;
    const qte = parseFloat(document.getElementById('upd-stock-qte').value);
    if(isNaN(qte)) return alert("Entrez une quantité valide");

    const item = stocks.find(s => s.id === id);
    item.qte += qte; // On ajoute à l'existant (Approvisionnement)
    localStorage.setItem('sc_stocks', JSON.stringify(stocks));
    afficherStocks();
    alert(`Stock mis à jour : ${item.nom} +${qte}`);
}

// --- CONFIGURATION IMPRIMANTE ---
function savePrinter() {
    const type = document.getElementById('printer-type').value;
    const ip = document.getElementById('printer-ip').value;
    localStorage.setItem('sc_printer', JSON.stringify({type, ip}));
    alert("Configuration imprimante enregistrée !");
}

document.getElementById('printer-type').addEventListener('change', function(){
    if(this.value === 'wifi') document.getElementById('printer-ip').classList.remove('hidden');
    else document.getElementById('printer-ip').classList.add('hidden');
});

// --- CAISSE & COMMANDE ---
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    const nom = s.options[s.selectedIndex].text.split(' (')[0];
    const prix = parseInt(s.value);
    if(!prix) return;
    panier.push({nom, prix});
    renderPanier();
}

function renderPanier() {
    const div = document.getElementById('panier-liste');
    const tot = panier.reduce((a,b) => a+b.prix, 0);
    div.innerHTML = panier.map((x,i) => `<div>${x.nom} - ${x.prix}F <small onclick="panier.splice(${i},1);renderPanier()">❌</small></div>`).join('');
    document.getElementById('total-caisse').innerText = tot + " F";
}

function validerCommande() {
    if(panier.length === 0) return alert("Panier vide");
    
    // Déduction auto des stocks (Consommation)
    const v = panier.length;
    stocks.find(s => s.id === 'savon').qte -= (v * 0.1);
    stocks.find(s => s.id === 'parfum').qte -= (v * 0.02);
    
    // Check articles luxe (Cintres/Housses)
    let luxe = 0;
    panier.forEach(p => { if(p.nom.match(/Veste|Robe|Toge|Blouson/i)) luxe++; });
    stocks.find(s => s.id === 'cintre').qte -= luxe;
    stocks.find(s => s.id === 'housse').qte -= luxe;

    localStorage.setItem('sc_stocks', JSON.stringify(stocks));
    
    // Impression (Simulée selon config)
    const config = JSON.parse(localStorage.getItem('sc_printer')) || {type: 'system'};
    alert(`Commande validée ! Impression via : ${config.type.toUpperCase()}`);
    
    window.print();
    location.reload();
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    if(id === 'section-admin') afficherPersonnel();
}
</script>
</body>
</html>
