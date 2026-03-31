<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING - VERSION FINALE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --radius: 12px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); font-size: 14px; -webkit-tap-highlight-color: transparent; }
        .hidden { display: none !important; }
        .card { background: white; padding: 15px; border-radius: var(--radius); box-shadow: 0 4px 12px rgba(0,0,0,0.08); margin-bottom: 12px; border: 1px solid #eee; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 8px; border: 1px solid #ccc; font-size: 14px; box-sizing: border-box; outline: none; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; transition: 0.2s; }
        button:active { transform: scale(0.97); }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; position: sticky; top: 0; z-index: 100; background: var(--light); padding: 5px 0; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 9px; height: 45px; padding: 2px; }
        .nav-bar button.active { background: var(--primary); color: white; }

        .progress-bar { background: #eee; height: 8px; border-radius: 4px; margin: 10px 0; overflow: hidden; }
        .progress-fill { background: var(--success); height: 100%; transition: 0.3s; }
        .badge-late { background: var(--danger); color: white; padding: 2px 6px; border-radius: 4px; font-size: 10px; font-weight: 800; animation: blink 1s infinite; }
        @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }

        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 10px; }
        .stat-box { padding: 12px; border-radius: 8px; text-align: center; color: white; box-shadow: inset 0 -3px 0 rgba(0,0,0,0.1); }
        .panier-item { display: flex; justify-content: space-between; align-items: center; background: #f9f9f9; padding: 8px; border-radius: 6px; margin-bottom: 5px; border-left: 3px solid var(--secondary); }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; color: #000; } }
    </style>
</head>
<body>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; text-align:center;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="max-width:150px; border-radius:50%;">
    <h2 style="color:var(--primary); margin:10px 0 0;">SUPER CLEAN</h2>
    <p style="font-size:12px; color:#666;">Gestion Pressing v1.0</p>
    <div style="width:280px; margin-top:20px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--primary); height:50px; font-size:16px;">SE CONNECTER</button>
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
            <input type="tel" id="tel-client" placeholder="WhatsApp (ex: 699...)" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom du Client">
            <div style="font-size:11px; margin:8px 0 2px; color:var(--primary); font-weight:bold;">📅 DATE DE RETRAIT PRÉVUE</div>
            <input type="date" id="date-retrait">
            <hr style="border:0; border-top:1px solid #eee; margin:10px 0;">
            <select id="select-article"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary);">+ AJOUTER L'ARTICLE</button>
        </div>
        
        <div id="panier-liste"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <span>NET À PAYER :</span><b id="total-caisse" style="font-size:22px; color:var(--success);">0 F</b>
            </div>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:8px; margin-top:12px;">
                <select id="mode-pay" style="background:rgba(255,255,255,0.1); color:white; border:none;">
                    <option value="Cash" style="color:black;">💵 Cash</option>
                    <option value="OM" style="color:black;">🍊 Orange</option>
                    <option value="Momo" style="color:black;">🟡 Momo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Versé (F)" oninput="calculerReste()" style="font-weight:800;">
            </div>
            <div id="reste-caisse" style="text-align:right; font-size:13px; margin-top:8px; color:var(--warning); font-weight:bold;">Reste à percevoir: 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:55px; font-size:16px; border-bottom:4px solid #27ae60;">🖨️ VALIDER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <h3 style="margin:10px;">📋 Suivi des Travaux</h3>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 État des Consommables</h3>
            <div id="stock-display"></div>
            <hr style="margin:15px 0; border:0; border-top:1px solid #eee;">
            <h4>📦 Approvisionnement (Arrivage)</h4>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" style="background:var(--warning); color:black;">METTRE À JOUR LE STOCK</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card" style="border-top:5px solid var(--primary);">
            <h3 style="margin-top:0;">📊 Bilan Financier</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:5px;">
                <button onclick="afficherStats('jour')" id="sj">JOUR</button>
                <button onclick="afficherStats('mois')" id="sm">MOIS</button>
                <button onclick="afficherStats('an')" id="sa">AN</button>
            </div>
            <div id="admin-stats-ui" style="margin-top:15px;"></div>
        </div>
        
        <div class="card">
            <h3>👥 Gestion Personnel</h3>
            <div id="staff-list"></div>
            <div style="background:#f9f9f9; padding:10px; border-radius:8px; margin-top:10px;">
                <input type="text" id="new-staff-nom" placeholder="Nom complet">
                <input type="text" id="new-staff-login" placeholder="Login">
                <input type="password" id="new-staff-pass" placeholder="Mot de passe">
                <button onclick="ajouterEmploye()" style="background:var(--success);">+ AJOUTER EMPLOYÉ</button>
            </div>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; width:80mm; padding:5mm; background:white;">
    <center>
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:40mm;">
        <h3 style="margin:5px 0;">SUPER CLEAN</h3>
        <p style="font-size:12px; margin:0;">Nkounga, Immeuble Karche<br>Tél: 675 864 103 / 692 315 267</p>
    </center>
    <hr style="border:1px dashed #000;">
    <div id="tk-body" style="font-size:13px;"></div>
    <hr style="border:1px dashed #000;">
    <center><b id="tk-retrait" style="font-size:15px;"></b></center>
    <p style="font-size:10px; text-align:center; margin-top:10px;">Merci de votre confiance !</p>
</div>

<script>
// --- INITIALISATION DES DONNÉES ---
let db = JSON.parse(localStorage.getItem('sc_db')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks')) || [
    {id:'savon', nom:"Savon Liquide", qte:50, unite:"L"}, 
    {id:'parfum', nom:"Parfum", qte:10, unite:"L"},
    {id:'cintre', nom:"Cintres", qte:200, unite:"pcs"}, 
    {id:'housse', nom:"Housses Plastiques", qte:150, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [];
let user = null;

const TARIFS = {
    "HAUTS": {"T-Shirt":500,"Chemise":500,"Polo":500,"Pull Over":700,"Blouson Std":1000,"Blouson Lourd":1500,"Veste 2p":1500,"Veste 3p":2000},
    "BAS/JUPES": {"Pantalon Jeans":500,"Pantalon Jogging":500,"Short":500,"Jupe simple":500,"Jupe plissée":1000},
    "ROBES/ENSEMBLES": {"Robe simple":500,"Robe de soirée":3000,"Kaba long":1500,"Costume":2500,"Toge":3500,"Boubou 3p":2500},
    "LINGE DE MAISON": {"Drap 1p":750,"Drap 2p":1000,"Couette":2000,"Couverture Std":3000,"Couverture Lourde":4000,"Rideau":1500}
};

// --- AUTHENTIFICATION ---
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const found = staff.find(s => s.login === l && s.pass === p);
    if(found) {
        user = found;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        initApp();
    } else alert("❌ Identifiants incorrects");
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Sélectionner l\'article --</option>';
    for(let cat in TARIFS) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let a in TARIFS[cat]) {
            let o = document.createElement('option'); o.value = TARIFS[cat][a]; o.innerText = `${a} (${TARIFS[cat][a]}F)`;
            g.appendChild(o);
        }
        s.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(x => `<option value="${x.id}">${x.nom}</option>`).join('');
    afficherStocks();
}

// --- GESTION CAISSE ---
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    const n = s.options[s.selectedIndex].text.split(' (')[0];
    panier.push({nom: n, prix: parseInt(s.value), fini: false});
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `
        <div class="panier-item">
            <span>${x.nom}</span>
            <b>${x.prix} F <span onclick="panier.splice(${i},1);renderPanier()" style="color:var(--danger); cursor:pointer; margin-left:10px;">✕</span></b>
        </div>
    `).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    const v = parseInt(document.getElementById('paye-caisse').value) || 0;
    const r = Math.max(0, t - v);
    document.getElementById('reste-caisse').innerText = `Reste à percevoir: ${r} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value;
    const tel = document.getElementById('tel-client').value;
    const retrait = document.getElementById('date-retrait').value;
    if(!nom || !retrait || panier.length === 0) return alert("⚠️ Complétez les infos (Client, Date, Panier) !");

    // 1. Déduction intelligente des Stocks
    const vCount = panier.length;
    let luxeCount = 0;
    panier.forEach(p => { if(p.nom.match(/Veste|Robe soirée|Toge|Costume|Blouson|Boubou/i)) luxeCount++; });

    stocks.find(s => s.id==='savon').qte -= (vCount * 0.1);
    stocks.find(s => s.id==='parfum').qte -= (vCount * 0.02);
    stocks.find(s => s.id==='cintre').qte -= luxeCount;
    stocks.find(s => s.id==='housse').qte -= luxeCount;
    localStorage.setItem('sc_stocks', JSON.stringify(stocks));

    // 2. Enregistrement DB
    const total = panier.reduce((a,b)=>a+b.prix, 0);
    const paye = parseInt(document.getElementById('paye-caisse').value) || 0;
    const cmd = {
        id: Math.floor(1000 + Math.random()*8999),
        client: nom, tel: tel, items: [...panier],
        total: total, paye: paye, reste: total - paye,
        mode: document.getElementById('mode-pay').value,
        retrait: retrait, date: new Date().toLocaleDateString('fr-FR'),
        status: "Atelier"
    };
    db.unshift(cmd);
    localStorage.setItem('sc_db', JSON.stringify(db));

    // 3. Impression Ticket
    document.getElementById('tk-body').innerHTML = `
        <b>REÇU #${cmd.id}</b><br>Client: ${cmd.client}<br>Date: ${cmd.date}<hr>
        ${panier.map(x => `<div>• ${x.nom} : ${x.prix}F</div>`).join('')}<hr>
        <b>TOTAL: ${total} F</b><br>Payé: ${paye} F (${cmd.mode})<br>
        <b>RESTE: ${cmd.total - cmd.paye} F</b>
    `;
    document.getElementById('tk-retrait').innerText = "À RETIRER LE : " + cmd.retrait;
    window.print();
    
    // 4. Reset & WhatsApp
    const msg = `*SUPER CLEAN PRESSING*%0A*Ticket #${cmd.id}*%0AClient: ${cmd.client}%0AArticles: ${vCount}%0APayé: ${cmd.paye} F%0AReste: ${cmd.reste} F%0A*Retrait prévu: ${cmd.retrait}*`;
    window.open(`https://wa.me/237${tel}?text=${msg}`);
    location.reload();
}

// --- SUIVI ATELIER ---
function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const today = new Date().toISOString().split('T')[0];
    
    if(db.filter(c => c.status !== "Livré").length === 0) {
        list.innerHTML = '<p style="text-align:center; opacity:0.5;">Aucun travail en cours.</p>';
        return;
    }

    list.innerHTML = db.filter(c => c.status !== "Livré").map(c => {
        const finiCount = c.items.filter(i => i.fini).length;
        const perc = Math.round((finiCount / c.items.length) * 100);
        const estRetard = c.retrait < today;

        return `
            <div class="card" style="border-left: 5px solid ${perc===100?'var(--success)':'var(--primary)'}">
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <b>#${c.id} - ${c.client}</b>
                    ${estRetard ? '<span class="badge-late">⚠️ RETARD</span>' : ''}
                </div>
                <div class="progress-bar"><div class="progress-fill" style="width:${perc}%"></div></div>
                <div style="font-size:11px; margin-bottom:10px; color:#666;">Livraison le: <b>${c.retrait}</b> (${perc}%)</div>
                ${c.items.map((item, idx) => `
                    <div style="display:flex; justify-content:space-between; padding:5px 0; border-top:1px solid #f2f2f2; font-size:12px;">
                        <span style="${item.fini?'text-decoration:line-through;opacity:0.4':''}">${item.nom}</span>
                        ${!item.fini ? `<button onclick="marquerPret(${c.id}, ${idx})" style="width:auto; padding:3px 10px; font-size:10px; margin:0;">PRÊT</button>` : '✅'}
                    </div>
                `).join('')}
                ${perc === 100 ? `<button onclick="livrerCommande(${c.id})" style="background:#000; margin-top:10px;">📦 LIVRER (Solde: ${c.total-c.paye}F)</button>` : ''}
            </div>
        `;
    }).join('');
}

function marquerPret(cid, idx) {
    const c = db.find(x => x.id === cid);
    c.items[idx].fini = true;
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

function livrerCommande(cid) {
    const c = db.find(x => x.id === cid);
    if(c.total - c.paye > 0) {
        if(!confirm(`Confirmez-vous avoir reçu le reste de ${c.total - c.paye} F ?`)) return;
        c.paye = c.total;
    }
    c.status = "Livré";
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

// --- STATS & ADMIN ---
function afficherStats(periode) {
    const now = new Date();
    const j = now.toLocaleDateString('fr-FR');
    const m = (now.getMonth()+1).toString().padStart(2,'0');
    const a = now.getFullYear().toString();

    let cash=0, om=0, momo=0;
    db.forEach(c => {
        const [dd, mm, yy] = c.date.split('/');
        let ok = false;
        if(periode==='jour' && c.date === j) ok=true;
        if(periode==='mois' && mm === m && yy === a) ok=true;
        if(periode==='an' && yy === a) ok=true;
        
        if(ok) {
            if(c.mode==="Cash") cash += c.paye;
            if(c.mode==="OM") om += c.paye;
            if(c.mode==="Momo") momo += c.paye;
        }
    });

    document.getElementById('admin-stats-ui').innerHTML = `
        <div class="stat-grid">
            <div class="stat-box" style="background:var(--success)">💵 Cash<br><b>${cash.toLocaleString()} F</b></div>
            <div class="stat-box" style="background:#ff6600">🍊 Orange<br><b>${om.toLocaleString()} F</b></div>
            <div class="stat-box" style="background:#f1c40f; color:black">🟡 Momo<br><b>${momo.toLocaleString()} F</b></div>
            <div class="stat-box" style="background:var(--dark)">💎 TOTAL<br><b>${(cash+om+momo).toLocaleString()} F</b></div>
        </div>
    `;
}

// --- STOCK & STAFF ---
function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value;
    const qte = parseFloat(document.getElementById('upd-stock-qte').value);
    if(!qte) return alert("Entrez une quantité");
    stocks.find(s => s.id === id).qte += qte;
    localStorage.setItem('sc_stocks', JSON.stringify(stocks));
    afficherStocks();
    alert("Stock mis à jour !");
}

function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div style="display:flex; justify-content:space-between; padding:8px 0; border-bottom:1px solid #f0f0f0;">
            <span>${s.nom}</span>
            <b style="${s.qte < 2 ? 'color:var(--danger)' : ''}">${parseFloat(s.qte.toFixed(2))} ${s.unite}</b>
        </div>
    `).join('');
}

function ajouterEmploye() {
    const n = document.getElementById('new-staff-nom').value;
    const l = document.getElementById('new-staff-login').value;
    const p = document.getElementById('new-staff-pass').value;
    if(!n || !l || !p) return;
    staff.push({nom:n, login:l, pass:p, role:"STAFF"});
    localStorage.setItem('sc_staff', JSON.stringify(staff));
    afficherPersonnel();
    alert("Employé ajouté");
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e,i) => `
        <div style="display:flex; justify-content:space-between; font-size:12px; margin-bottom:5px; background:#f9f9f9; padding:5px; border-radius:4px;">
            <span>${e.nom} (${e.role})</span>
            ${i !== 0 ? `<span onclick="staff.splice(${i},1);localStorage.setItem('sc_staff',JSON.stringify(staff));afficherPersonnel()" style="color:red">Suppr.</span>` : ''}
        </div>
    `).join('');
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') afficherAtelier();
    if(id === 'section-admin') { afficherStats('jour'); afficherPersonnel(); }
    if(id === 'section-stocks') { afficherStocks(); }
}

function rechercherClient() {
    const tel = document.getElementById('tel-client').value;
    if(tel.length >= 8) {
        const found = db.find(c => c.tel && c.tel.includes(tel));
        if(found) {
            document.getElementById('client-nom').value = found.client;
            document.getElementById('client-nom').style.background = "#e8f5e9";
        }
    }
}
</script>
</body>
</html>
