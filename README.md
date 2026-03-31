<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PREMIUM v7.0</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #4361ee; --primary-dark: #3f37c9;
            --secondary: #4cc9f0; --success: #4cc9f0; 
            --danger: #f72585; --warning: #ff9f1c; --dark: #212529; 
            --bg: #f8f9fa; --card-bg: #ffffff; --radius: 16px;
        }
        
        body { 
            font-family: 'Inter', sans-serif; margin: 0; padding: 0; 
            background-color: var(--bg); color: var(--dark); 
            line-height: 1.5; -webkit-tap-highlight-color: transparent;
        }

        /* --- LOGIN SCREEN LUXE --- */
        #auth-screen {
            background: linear-gradient(135deg, var(--primary-dark) 0%, var(--primary) 100%);
            height: 100vh; display: flex; align-items: center; justify-content: center;
        }
        .login-card {
            background: white; padding: 40px 30px; border-radius: 24px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2); width: 100%; max-width: 340px; text-align: center;
        }
        .login-card h1 { font-weight: 800; color: var(--primary); margin-bottom: 5px; letter-spacing: -1px; }
        .login-card p { font-size: 13px; color: #6c757d; margin-bottom: 25px; }

        /* --- APP LAYOUT --- */
        .header-session {
            background: white; padding: 15px 20px; display: flex; justify-content: space-between;
            align-items: center; border-bottom: 1px solid #e9ecef;
        }
        .user-tag { background: #eef2ff; color: var(--primary); padding: 6px 12px; border-radius: 20px; font-weight: 600; font-size: 12px; }

        .nav-bar {
            display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px;
            padding: 15px; background: white; position: sticky; top: 0; z-index: 100;
        }
        .nav-bar button {
            background: transparent; border: none; color: #adb5bd; font-weight: 700;
            font-size: 10px; display: flex; flex-direction: column; align-items: center; transition: 0.3s;
        }
        .nav-bar button.active { color: var(--primary); transform: translateY(-2px); }
        .nav-bar button.active::after {
            content: ''; width: 20px; height: 4px; background: var(--primary);
            border-radius: 10px; margin-top: 5px;
        }

        /* --- ELEMENTS DESIGN --- */
        .card {
            background: var(--card-bg); margin: 15px; padding: 20px; border-radius: var(--radius);
            box-shadow: 0 4px 15px rgba(0,0,0,0.05); border: 1px solid rgba(0,0,0,0.03);
        }
        input, select {
            background: #f1f3f5; border: 2px solid transparent; padding: 14px;
            border-radius: 12px; margin-bottom: 12px; font-family: inherit; font-size: 14px; width: 100%;
            transition: 0.3s; box-sizing: border-box;
        }
        input:focus { border-color: var(--primary); background: white; outline: none; }
        
        button.btn-main {
            background: var(--primary); color: white; border: none; padding: 16px;
            border-radius: 12px; font-weight: 700; font-size: 15px; cursor: pointer;
            box-shadow: 0 8px 20px rgba(67, 97, 238, 0.25); transition: 0.2s;
        }
        button.btn-main:active { transform: scale(0.95); }

        /* --- ATELIER & PROGRESS --- */
        .progress-bar { background: #e9ecef; height: 8px; border-radius: 10px; margin: 12px 0; overflow: hidden; }
        .progress-fill { background: var(--primary); height: 100%; transition: 0.8s cubic-bezier(0.17, 0.67, 0.83, 0.67); }
        
        .item-row {
            display: flex; justify-content: space-between; align-items: center;
            padding: 12px 0; border-bottom: 1px solid #f8f9fa;
        }
        .badge-done { background: #dcfce7; color: #15803d; padding: 4px 8px; border-radius: 6px; font-size: 11px; font-weight: 600; }

        .total-banner {
            background: var(--dark); color: white; padding: 20px; border-radius: var(--radius);
            margin: 15px; display: flex; justify-content: space-between; align-items: center;
        }

        .hidden { display: none !important; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div class="login-card">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width: 100px; height: 100px; border-radius: 50%; border: 5px solid #f1f3f5; margin-bottom: 15px;">
        <h1>SUPER CLEAN</h1>
        <p>Gestion Professionnelle de Pressing</p>
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" class="btn-main" style="width: 100%;">SE CONNECTER</button>
    </div>
</div>

<div id="app" class="hidden">
    <div class="header-session">
        <div class="user-tag">👤 <span id="user-display">Chargement...</span></div>
        <button onclick="logout()" style="background:none; border:none; color:var(--danger); font-size:11px; font-weight:bold;">DÉCONNEXION</button>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="nav-caisse" class="active">📥<span>CAISSE</span></button>
        <button onclick="tab('section-atelier')" id="nav-atelier">👕<span>ATELIER</span></button>
        <button onclick="tab('section-stocks')" id="nav-stocks">🧼<span>STOCKS</span></button>
        <button onclick="tab('section-admin')" id="nav-admin">👑<span>ADMIN</span></button>
    </nav>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <input type="tel" id="tel-client" placeholder="N° WhatsApp Client" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom du Client">
            <label style="font-size:11px; font-weight:bold; color:var(--primary); margin-left: 5px;">DATE DE RETRAIT PRÉVUE</label>
            <input type="date" id="date-retrait">
            <select id="select-article" style="background-color: #eef2ff; border-left: 5px solid var(--primary);"></select>
            <button onclick="ajouterAuPanier()" class="btn-main" style="background: var(--dark); width: 100%;">+ AJOUTER AU PANIER</button>
        </div>

        <div id="panier-liste" style="margin: 0 15px;"></div>

        <div class="total-banner">
            <div><span style="font-size: 12px; opacity: 0.8;">NET À PAYER</span><br><b id="total-caisse" style="font-size: 24px;">0 F</b></div>
            <div style="text-align: right;">
                <input type="number" id="paye-caisse" placeholder="Avance" oninput="calculerReste()" style="margin:0; width: 100px; background: rgba(255,255,255,0.1); color: white; border: 1px solid rgba(255,255,255,0.2);">
                <div id="reste-caisse" style="font-size: 11px; margin-top: 5px; color: var(--secondary);">Reste: 0 F</div>
            </div>
        </div>
        <div style="padding: 15px;">
            <select id="mode-pay" style="margin-bottom: 10px;">
                <option value="Cash">💵 Espèces (Cash)</option>
                <option value="OM">🍊 Orange Money</option>
                <option value="Momo">🟡 MTN MoMo</option>
            </select>
            <button onclick="validerCommande()" class="btn-main" style="width: 100%; height: 60px; background: var(--success);">🖨️ VALIDER ET IMPRIMER</button>
        </div>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3 style="margin-top:0;">📊 Niveaux de Stock</h3>
            <div id="stock-display"></div>
            <hr style="border: 0; border-top: 1px solid #eee; margin: 20px 0;">
            <h4>Réapprovisionnement</h4>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" class="btn-main" style="width:100%;">ENREGISTRER</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card">
            <h3 style="margin-top:0;">💰 Rapport Financier</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:10px; margin-bottom:20px;">
                <button onclick="afficherStats('jour')" class="btn-main" style="padding:10px; font-size:12px;">JOUR</button>
                <button onclick="afficherStats('mois')" class="btn-main" style="padding:10px; font-size:12px; background:var(--dark);">MOIS</button>
                <button onclick="afficherStats('an')" class="btn-main" style="padding:10px; font-size:12px; background:var(--dark);">AN</button>
            </div>
            <div id="admin-stats-ui"></div>
        </div>
        <div class="card">
            <h3>👥 Équipe</h3>
            <div id="staff-list"></div>
            <hr style="margin:20px 0; border:0; border-top:1px solid #eee;">
            <input type="text" id="ns-nom" placeholder="Nom complet">
            <input type="text" id="ns-log" placeholder="Identifiant">
            <input type="password" id="ns-pass" placeholder="Mot de passe">
            <button onclick="ajouterEmploye()" class="btn-main" style="width:100%; background:var(--success);">AJOUTER COLLABORATEUR</button>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; padding: 20px;">
    <center>
        <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
        Expert en Blanchisserie<br>
        -----------------------------
    </center>
    <div id="tk-content" style="font-family:monospace; font-size:14px; margin-top:15px;"></div>
</div>

<script>
// --- DONNÉES ET TARIFS (LOGIQUE INTACTE) ---
const GRILLE = {
    "HAUTS": {"Chemise": 500, "Veste": 1500, "T-Shirt": 500, "Manteau": 2500, "Blouson": 3500},
    "BAS": {"Pantalon": 500, "Pantalon Lin": 800, "Jupe Simple": 500, "Jupe Plissée": 1000},
    "TENUES": {"Costume 2P": 2500, "Costume 3P": 3500, "Robe Soirée": 3000, "Boubou Bazin": 3500},
    "MAISON": {"Drap": 1000, "Couette": 2500, "Rideau": 1500},
    "ACCESSOIRES": {"Tennis": 1500, "Cravate": 500, "Sac à main": 2500}
};

let db = JSON.parse(localStorage.getItem('sc_db_v7')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v7')) || [
    {id:'savon', nom:"Savon Liquide", qte:50, unite:"L"}, 
    {id:'parfum', nom:"Parfum", qte:10, unite:"L"},
    {id:'housses', nom:"Housses", qte:200, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff_v7')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [];
let currentUser = null;

// --- FONCTIONS SYSTÈME ---
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
    } else alert("Accès refusé");
}

function logout() {
    location.reload();
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">📦 Sélectionner un article...</option>';
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

// --- LOGIQUE MÉTIER ---
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    panier.push({ uid: Date.now()+Math.random(), nom: s.options[s.selectedIndex].text.split(' (')[0], prix: parseInt(s.value), pret: false, done_by: "" });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `
        <div class="card" style="margin: 5px 0; padding: 12px; display:flex; justify-content:space-between; align-items:center;">
            <span><b>${x.nom}</b></span>
            <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()" style="background:none; border:none; color:var(--danger); font-weight:bold; margin-left:10px;">✕</button></span>
        </div>`).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    const p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste à payer : ${Math.max(0, t - p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value;
    const dateR = document.getElementById('date-retrait').value;
    if(!nom || !dateR || panier.length === 0) return alert("Formulaire incomplet !");

    const ticket = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: document.getElementById('tel-client').value,
        articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value, date_depot: new Date().toLocaleDateString('fr-FR'), retrait: dateR,
        caissier: currentUser.nom, statut: "Atelier", timestamp: new Date().getTime()
    };

    stocks[0].qte -= (panier.length * 0.05); stocks[1].qte -= (panier.length * 0.01); stocks[2].qte -= panier.length;
    localStorage.setItem('sc_stocks_v7', JSON.stringify(stocks));
    db.unshift(ticket);
    localStorage.setItem('sc_db_v7', JSON.stringify(db));
    
    document.getElementById('tk-content').innerHTML = `
        TICKET N° ${ticket.id}<br>DATE: ${ticket.date_depot}<br>CLIENT: ${ticket.client.toUpperCase()}<br>CAISSIER: ${ticket.caissier}<hr>
        ${panier.map(a => `${a.nom}: ${a.prix}F`).join('<br>')}<hr>
        TOTAL: ${ticket.total}F<br>PAYÉ: ${ticket.paye}F<br><b>RESTE: ${ticket.total - ticket.paye}F</b><br>RETRAIT PRÉVU LE: ${ticket.retrait}
    `;
    window.print();
    location.reload();
}

function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const orders = db.filter(t => t.statut !== "Livré");
    list.innerHTML = orders.map(t => {
        const fini = t.articles.filter(a => a.pret).length;
        const prog = Math.round((fini/t.articles.length)*100);
        return `
            <div class="card">
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <b style="color:var(--primary);">#${t.id} — ${t.client}</b>
                    <small style="font-weight:700;">${t.retrait}</small>
                </div>
                <div class="progress-bar"><div class="progress-fill" style="width:${prog}%"></div></div>
                ${t.articles.map(art => `
                    <div class="item-row">
                        <span>${art.nom}</span>
                        ${!art.pret ? `<button onclick="marquerFini(${t.id}, ${art.uid})" class="btn-main" style="padding:6px 12px; font-size:10px; background:var(--success); box-shadow:none;">PRÊT</button>` : `<span class="badge-done">✔ ${art.done_by}</span>`}
                    </div>`).join('')}
                ${prog === 100 ? `<button onclick="livrer(${t.id})" class="btn-main" style="width:100%; margin-top:15px; background:black;">MARQUER COMME LIVRÉ</button>` : ''}
            </div>`;
    }).join('');
}

function marquerFini(tid, auid) {
    const loc = prompt("Emplacement (Casier) ?");
    if(!loc) return;
    const t = db.find(x => x.id === tid);
    const art = t.articles.find(a => a.uid === auid);
    art.pret = true; art.done_by = currentUser.nom;
    localStorage.setItem('sc_db_v7', JSON.stringify(db));
    afficherAtelier();
}

function livrer(id) {
    const t = db.find(x => x.id === id);
    if(confirm("Confirmer la livraison ?")) { t.statut = "Livré"; t.paye = t.total; localStorage.setItem('sc_db_v7', JSON.stringify(db)); afficherAtelier(); }
}

function afficherStats(periode) {
    if(currentUser.role !== "ADMIN") return;
    const now = new Date(); let cash = 0, om = 0, momo = 0;
    db.forEach(t => {
        const d = new Date(t.timestamp); let match = false;
        if(periode==='jour' && d.toDateString() === now.toDateString()) match = true;
        if(periode==='mois' && d.getMonth() === now.getMonth()) match = true;
        if(match) { if(t.mode === "Cash") cash += t.paye; if(t.mode === "OM") om += t.paye; if(t.mode === "Momo") momo += t.paye; }
    });
    document.getElementById('admin-stats-ui').innerHTML = `
        <div class="stat-grid">
            <div class="stat-box" style="background:var(--success); padding:15px; border-radius:12px;">${cash} F<br><small>Espèces</small></div>
            <div class="stat-box" style="background:#ff9f1c; padding:15px; border-radius:12px;">${om} F<br><small>Orange</small></div>
            <div class="stat-box" style="background:#ffcc33; padding:15px; border-radius:12px; color:black;">${momo} F<br><small>MTN</small></div>
            <div class="stat-box" style="background:var(--dark); grid-column: span 2; padding:15px; border-radius:12px;">${cash+om+momo} F<br><small>TOTAL</small></div>
        </div>`;
}

function ajouterEmploye() {
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-log').value, p = document.getElementById('ns-pass').value;
    if(n && l && p) { staff.push({nom:n, login:l, pass:p, role:"STAFF"}); localStorage.setItem('sc_staff_v7', JSON.stringify(staff)); afficherPersonnel(); alert("Employé ajouté !"); }
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e, i) => `
        <div class="item-row">
            <span><b>${e.nom}</b></span>
            ${i!==0 ? `<button onclick="staff.splice(${i},1);localStorage.setItem('sc_staff_v7',JSON.stringify(staff));afficherPersonnel()" style="color:var(--danger); background:none; border:none; font-weight:bold;">Supprimer</button>` : '<i>(Gérant)</i>'}
        </div>`).join('');
}

function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div class="item-row"><span>${s.nom}</span><b style="color:var(--primary);">${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(q) { stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks_v7', JSON.stringify(stocks)); afficherStocks(); }
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
