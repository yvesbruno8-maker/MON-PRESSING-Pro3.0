<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING PRO v2.0</title>
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
        button:active { transform: scale(0.98); }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; position: sticky; top: 0; z-index: 100; background: var(--light); padding: 5px 0; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 9px; height: 45px; padding: 2px; }
        .nav-bar button.active { background: var(--primary); color: white; }

        .progress-bar { background: #eee; height: 10px; border-radius: 5px; margin: 10px 0; overflow: hidden; border: 1px solid #ddd; }
        .progress-fill { background: var(--success); height: 100%; transition: 0.5s; width: 0%; }
        .badge-late { background: var(--danger); color: white; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: 800; animation: blink 1s infinite; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.3; } 100% { opacity: 1; } }

        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 10px; }
        .stat-box { padding: 12px; border-radius: 8px; text-align: center; color: white; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="max-width:140px; border-radius:50%;">
    <h2 style="color:var(--primary); margin-top:10px;">SUPER CLEAN</h2>
    <div style="width:280px; margin-top:20px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()">CONNEXION</button>
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
            <div style="font-size:11px; font-weight:bold; color:var(--primary);">DATE DE RETRAIT PRÉVUE :</div>
            <input type="date" id="date-retrait">
            <select id="select-article"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary);">+ AJOUTER ARTICLE</button>
        </div>
        <div id="panier-liste"></div>
        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between;"><span>TOTAL:</span><b id="total-caisse" style="font-size:20px; color:var(--success);">0 F</b></div>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:8px; margin-top:10px;">
                <select id="mode-pay"><option value="Cash">💵 Cash</option><option value="OM">🍊 Orange</option><option value="Momo">🟡 Momo</option></select>
                <input type="number" id="paye-caisse" placeholder="Avance (F)" oninput="calculerReste()">
            </div>
            <div id="reste-caisse" style="text-align:right; font-size:12px; margin-top:5px; color:var(--warning);">Reste: 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:55px;">🖨️ VALIDER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <h3 style="margin-left:10px;">👕 Travaux & Progression</h3>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 État des Consommables</h3>
            <div id="stock-display"></div>
            <hr>
            <h4>📦 Approvisionnement (Arrivage)</h4>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" style="background:var(--warning); color:black;">METTRE À JOUR LE STOCK</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card" style="border-top:5px solid var(--primary);">
            <h3>📊 Bilan & Ventilation</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:5px;">
                <button onclick="afficherStats('jour')" id="sj">JOUR</button>
                <button onclick="afficherStats('mois')" id="sm">MOIS</button>
                <button onclick="afficherStats('an')" id="sa">AN</button>
            </div>
            <div id="admin-stats-ui" style="margin-top:15px;"></div>
        </div>
        
        <div class="card">
            <h3>🖨️ Imprimante</h3>
            <select id="printer-config">
                <option value="system">Système (Défaut)</option>
                <option value="bluetooth">Bluetooth Thermique</option>
                <option value="wifi">Wi-Fi (Réseau)</option>
            </select>
            <button onclick="alert('Configuration enregistrée')">Sauvegarder</button>
        </div>

        <div class="card">
            <h3>👥 Gestion du Personnel</h3>
            <div id="staff-list"></div>
            <hr>
            <input type="text" id="ns-nom" placeholder="Nom">
            <input type="text" id="ns-log" placeholder="Login">
            <input type="password" id="ns-pass" placeholder="Pass">
            <button onclick="ajouterEmploye()" style="background:var(--success);">+ Ajouter Employé</button>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; width:80mm; padding:10px;">
    <center><b>SUPER CLEAN PRESSING</b><br>Nkounga, Immeuble Karche</center>
    <div id="tk-content" style="font-size:12px; margin-top:10px;"></div>
</div>

<script>
let db = JSON.parse(localStorage.getItem('sc_db')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks')) || [
    {id:'savon', nom:"Savon", qte:20, unite:"L"}, {id:'parfum', nom:"Parfum", qte:5, unite:"L"},
    {id:'cintre', nom:"Cintres", qte:100, unite:"pcs"}, {id:'housse', nom:"Housses", qte:50, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [];
let user = null;

const TARIFS = {
    "HAUTS": {"Chemise": 500, "Veste": 1500, "Pull": 700},
    "BAS": {"Pantalon": 500, "Jupe": 500},
    "ROBES": {"Robe soirée": 3000, "Costume": 2500},
    "MAISON": {"Drap": 1000, "Couette": 2000}
};

function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const found = staff.find(s => s.login === l && s.pass === p);
    if(found) { user = found; document.getElementById('auth-screen').classList.add('hidden'); document.getElementById('app').classList.remove('hidden'); initApp(); }
    else alert("Accès refusé");
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Choisir Article --</option>';
    for(let cat in TARIFS) {
        for(let a in TARIFS[cat]) {
            let o = document.createElement('option'); o.value = TARIFS[cat][a]; o.innerText = `${a} (${TARIFS[cat][a]}F)`;
            s.appendChild(o);
        }
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(x => `<option value="${x.id}">${x.nom}</option>`).join('');
    afficherStocks();
}

function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    panier.push({nom: s.options[s.selectedIndex].text.split(' (')[0], prix: parseInt(s.value), fini: false});
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `<div class="card" style="padding:8px; margin-bottom:5px;">${x.nom} - ${x.prix}F <span onclick="panier.splice(${i},1);renderPanier()" style="color:red; float:right; cursor:pointer;">✕</span></div>`).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    const p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste: ${t-p} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value;
    const retrait = document.getElementById('date-retrait').value;
    if(!nom || !retrait || panier.length === 0) return alert("Infos manquantes !");

    const cmd = {
        id: Math.floor(1000+Math.random()*9000),
        client: nom,
        date: new Date().toLocaleDateString('fr-FR'),
        retrait: retrait,
        items: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0),
        paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value,
        status: "Atelier"
    };

    // Déduction stock
    stocks[0].qte -= (panier.length * 0.1);
    stocks[1].qte -= (panier.length * 0.02);
    localStorage.setItem('sc_stocks', JSON.stringify(stocks));

    db.unshift(cmd);
    localStorage.setItem('sc_db', JSON.stringify(db));
    alert("Encaissé avec succès !");
    window.print();
    location.reload();
}

function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const today = new Date().toISOString().split('T')[0];
    list.innerHTML = db.filter(c => c.status !== "Livré").map(c => {
        const finiCount = c.items.filter(i => i.fini).length;
        const perc = Math.round((finiCount / c.items.length) * 100);
        const retard = c.retrait < today;
        return `
            <div class="card" style="border-left: 5px solid ${perc===100?'var(--success)':'var(--primary)'}">
                <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b> ${retard?'<span class="badge-late">⚠️ RETARD</span>':''}</div>
                <div class="progress-bar"><div class="progress-fill" style="width:${perc}%"></div></div>
                <div style="font-size:11px;">Retrait: ${c.retrait} | Prêt à ${perc}%</div>
                <hr>
                ${c.items.map((it, idx) => `<div style="font-size:12px;">${it.nom} ${it.fini ? '✅' : `<button onclick="marquerPret(${c.id},${idx})" style="width:auto; padding:2px 5px; font-size:10px; margin:0;">FINIR</button>`}</div>`).join('')}
                ${perc === 100 ? `<button onclick="livrer(${c.id})" style="background:black; margin-top:10px;">📦 LIVRER MAINTENANT</button>` : ''}
            </div>`;
    }).join('');
}

function marquerPret(cid, idx) {
    const c = db.find(x => x.id === cid);
    c.items[idx].fini = true;
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

function livrer(cid) {
    db.find(x => x.id === cid).status = "Livré";
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

function afficherStats(periode) {
    const now = new Date();
    const dStr = now.toLocaleDateString('fr-FR');
    const mStr = (now.getMonth()+1).toString().padStart(2,'0');
    const yStr = now.getFullYear().toString();
    let cash = 0, om = 0, momo = 0;

    db.forEach(c => {
        const [dd, mm, yy] = c.date.split('/');
        let match = false;
        if(periode === 'jour' && c.date === dStr) match = true;
        if(periode === 'mois' && mm === mStr && yy === yStr) match = true;
        if(periode === 'an' && yy === yStr) match = true;
        if(match) {
            if(c.mode === "Cash") cash += c.paye;
            else if(c.mode === "OM") om += c.paye;
            else if(c.mode === "Momo") momo += c.paye;
        }
    });

    document.getElementById('admin-stats-ui').innerHTML = `
        <div class="stat-grid">
            <div class="stat-box" style="background:#2ecc71">💵 Cash<br><b>${cash} F</b></div>
            <div class="stat-box" style="background:#ff6600">🍊 OM<br><b>${om} F</b></div>
            <div class="stat-box" style="background:#f1c40f; color:black">🟡 Momo<br><b>${momo} F</b></div>
            <div class="stat-box" style="background:var(--dark)">💎 TOTAL<br><b>${cash+om+momo} F</b></div>
        </div>`;
}

function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div style="display:flex; justify-content:space-between; padding:5px 0; border-bottom:1px solid #eee;"><span>${s.nom}</span><b>${parseFloat(s.qte.toFixed(2))} ${s.unite}</b></div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value;
    const q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(q) { stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks', JSON.stringify(stocks)); afficherStocks(); }
}

function ajouterEmploye() {
    if(user.role !== 'ADMIN') return alert("Action réservée à l'Admin");
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-log').value, p = document.getElementById('ns-pass').value;
    if(n && l && p) { staff.push({nom:n, login:l, pass:p, role:"STAFF"}); localStorage.setItem('sc_staff', JSON.stringify(staff)); afficherPersonnel(); }
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e,i) => `<div style="display:flex; justify-content:space-between; padding:5px; background:#f9f9f9; margin-bottom:2px;"><span>${e.nom}</span> ${i!==0?`<span onclick="staff.splice(${i},1);localStorage.setItem('sc_staff',JSON.stringify(staff));afficherPersonnel()" style="color:red; cursor:pointer;">Suppr.</span>`:''}</div>`).join('');
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') afficherAtelier();
    if(id === 'section-admin') { afficherStats('jour'); afficherPersonnel(); }
}

function rechercherClient() {
    const tel = document.getElementById('tel-client').value;
    if(tel.length >= 8) {
        const found = db.find(c => c.tel && c.tel.includes(tel));
        if(found) document.getElementById('client-nom').value = found.client;
    }
}
</script>
</body>
</html>
