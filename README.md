<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PREMIUM v8.0 - RÔLES ADMIN</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #4361ee; --primary-dark: #3f37c9;
            --secondary: #4cc9f0; --success: #2ecc71; 
            --danger: #f72585; --warning: #ff9f1c; --dark: #212529; 
            --bg: #f8f9fa; --card-bg: #ffffff; --radius: 16px;
        }
        
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 0; background-color: var(--bg); color: var(--dark); -webkit-tap-highlight-color: transparent; }

        /* --- LOGIN --- */
        #auth-screen { background: linear-gradient(135deg, var(--primary-dark) 0%, var(--primary) 100%); height: 100vh; display: flex; align-items: center; justify-content: center; }
        .login-card { background: white; padding: 40px 30px; border-radius: 24px; box-shadow: 0 20px 40px rgba(0,0,0,0.2); width: 100%; max-width: 320px; text-align: center; }

        /* --- HEADER & NAV --- */
        .header-session { background: white; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #e9ecef; }
        .user-tag { background: #eef2ff; color: var(--primary); padding: 6px 12px; border-radius: 20px; font-weight: 700; font-size: 11px; }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; padding: 15px; background: white; position: sticky; top: 0; z-index: 100; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        .nav-bar button { background: transparent; border: none; color: #adb5bd; font-weight: 700; font-size: 10px; display: flex; flex-direction: column; align-items: center; transition: 0.3s; }
        .nav-bar button.active { color: var(--primary); }
        .nav-bar button.active::after { content: ''; width: 15px; height: 3px; background: var(--primary); border-radius: 10px; margin-top: 4px; }

        /* --- CARDS & FORMS --- */
        .card { background: var(--card-bg); margin: 15px; padding: 20px; border-radius: var(--radius); box-shadow: 0 4px 15px rgba(0,0,0,0.05); border: 1px solid rgba(0,0,0,0.03); }
        input, select { background: #f1f3f5; border: 2px solid transparent; padding: 14px; border-radius: 12px; margin-bottom: 10px; font-family: inherit; font-size: 14px; width: 100%; box-sizing: border-box; }
        input:focus { border-color: var(--primary); background: white; outline: none; }
        
        button.btn-main { background: var(--primary); color: white; border: none; padding: 16px; border-radius: 12px; font-weight: 700; font-size: 14px; cursor: pointer; transition: 0.2s; }
        button.btn-main:active { transform: scale(0.96); }

        .role-badge { font-size: 9px; padding: 2px 6px; border-radius: 4px; font-weight: 800; text-transform: uppercase; margin-left: 5px; }
        .role-admin { background: #fee2e2; color: #ef4444; }
        .role-staff { background: #dcfce7; color: #15803d; }

        .hidden { display: none !important; }
        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div class="login-card">
        <h2 style="color:var(--primary); margin:0;">SUPER CLEAN</h2>
        <p style="font-size:12px; color:#666; margin-bottom:20px;">Connexion Sécurisée</p>
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" class="btn-main" style="width:100%;">ENTRER</button>
    </div>
</div>

<div id="app" class="hidden">
    <div class="header-session">
        <div class="user-tag">👤 <span id="user-display">-</span></div>
        <button onclick="location.reload()" style="background:none; border:none; color:var(--danger); font-size:11px; font-weight:800;">DÉCONNEXION</button>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="nav-caisse" class="active">📥<span>CAISSE</span></button>
        <button onclick="tab('section-atelier')" id="nav-atelier">👕<span>ATELIER</span></button>
        <button onclick="tab('section-stocks')" id="nav-stocks">🧼<span>STOCKS</span></button>
        <button onclick="tab('section-admin')" id="nav-admin">👑<span>ADMIN</span></button>
    </nav>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <input type="tel" id="tel-client" placeholder="WhatsApp Client" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom du Client">
            <input type="date" id="date-retrait">
            <select id="select-article"></select>
            <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); width:100%;">+ AJOUTER</button>
        </div>
        <div id="panier-liste" style="padding:0 15px;"></div>
        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <span style="font-size:12px;">TOTAL</span>
                <b id="total-caisse" style="font-size:22px; color:var(--success);">0 F</b>
            </div>
            <input type="number" id="paye-caisse" placeholder="Avance versée" oninput="calculerReste()" style="margin-top:10px; background:rgba(255,255,255,0.1); color:white;">
            <div id="reste-caisse" style="font-size:11px; text-align:right; color:var(--secondary);">Reste: 0 F</div>
        </div>
        <div style="padding:15px;"><button onclick="validerCommande()" class="btn-main" style="width:100%; background:var(--primary);">VALIDER & IMPRIMER</button></div>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 État des Produits</h3>
            <div id="stock-display"></div>
            <hr style="border:0; border-top:1px solid #eee; margin:15px 0;">
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" class="btn-main" style="width:100%;">METTRE À JOUR</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div id="admin-lock-msg" class="card hidden" style="text-align:center; color:var(--danger); font-weight:800;">
            🚫 ACCÈS REFUSÉ<br><small style="font-weight:400; color:#666;">Seul un administrateur peut voir cette section.</small>
        </div>
        <div id="admin-content">
            <div class="card">
                <h3>💰 Recettes Financières</h3>
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px;">
                    <button onclick="afficherStats('jour')" class="btn-main" style="padding:10px;">JOUR</button>
                    <button onclick="afficherStats('mois')" class="btn-main" style="padding:10px; background:var(--dark);">MOIS</button>
                </div>
                <div id="admin-stats-ui" style="margin-top:15px;"></div>
            </div>
            <div class="card">
                <h3>👥 Gestion de l'Équipe</h3>
                <div id="staff-list"></div>
                <hr style="margin:20px 0; border:0; border-top:1px solid #eee;">
                <h4>Ajouter un Utilisateur</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Identifiant (Login)">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role" style="border:2px solid var(--primary);">
                    <option value="STAFF">Rôle : EMPLOYÉ (Staff)</option>
                    <option value="ADMIN">Rôle : ADMINISTRATEUR (Gérant)</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="width:100%; background:var(--success);">ENREGISTRER L'UTILISATEUR</button>
            </div>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; padding:20px;">
    <center><b>SUPER CLEAN PRESSING</b><hr></center>
    <div id="tk-content"></div>
</div>

<script>
const GRILLE = {
    "HAUTS": {"Chemise": 500, "Veste": 1500, "Manteau": 2500},
    "BAS": {"Pantalon": 500, "Jupe": 500},
    "TENUES": {"Costume 2P": 2500, "Robe": 1500, "Boubou": 2500},
    "MAISON": {"Drap": 1000, "Couette": 2500},
    "ACCESSOIRES": {"Tennis": 1500, "Sac": 2500}
};

let db = JSON.parse(localStorage.getItem('sc_db_v8')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v8')) || [
    {id:'savon', nom:"Savon Liquide", qte:50, unite:"L"}, 
    {id:'parfum', nom:"Parfum", qte:10, unite:"L"},
    {id:'housses', nom:"Housses", qte:200, unite:"pcs"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff_v8')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [], currentUser = null;

function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const user = staff.find(s => s.login === l && s.pass === p);
    if(user) {
        currentUser = user;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('user-display').innerText = `${currentUser.nom} (${currentUser.role})`;
        initApp();
    } else alert("Identifiants incorrects");
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">Choisir Article...</option>';
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

function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    panier.push({ uid: Date.now()+Math.random(), nom: s.options[s.selectedIndex].text.split(' (')[0], prix: parseInt(s.value), pret: false, done_by: "" });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `
        <div style="background:white; padding:10px; border-radius:8px; margin-bottom:5px; display:flex; justify-content:space-between; font-size:13px; border:1px solid #eee;">
            <span>${x.nom}</span><b>${x.prix}F <span onclick="panier.splice(${i},1);renderPanier()" style="color:var(--danger); margin-left:10px;">✕</span></b>
        </div>`).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${Math.max(0, t - p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, dateR = document.getElementById('date-retrait').value;
    if(!nom || !dateR || panier.length === 0) return alert("Incomplet !");
    const ticket = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: document.getElementById('tel-client').value,
        articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        date_depot: new Date().toLocaleDateString('fr-FR'), retrait: dateR, caissier: currentUser.nom, statut: "Atelier", timestamp: new Date().getTime()
    };
    stocks[0].qte -= (panier.length * 0.05); stocks[2].qte -= panier.length;
    localStorage.setItem('sc_stocks_v8', JSON.stringify(stocks));
    db.unshift(ticket); localStorage.setItem('sc_db_v8', JSON.stringify(db));
    document.getElementById('tk-content').innerHTML = `TICKET #${ticket.id}<br>CLIENT: ${ticket.client}<br>TOTAL: ${ticket.total}F<br>RESTE: ${ticket.total-ticket.paye}F<br>RETRAIT: ${ticket.retrait}`;
    window.print(); location.reload();
}

function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    list.innerHTML = db.filter(t => t.statut !== "Livré").map(t => {
        const fini = t.articles.filter(a => a.pret).length;
        const prog = Math.round((fini/t.articles.length)*100);
        return `
            <div class="card">
                <b>#${t.id} - ${t.client}</b>
                <div style="background:#eee; height:6px; border-radius:10px; margin:10px 0;"><div style="background:var(--primary); width:${prog}%; height:100%; border-radius:10px;"></div></div>
                ${t.articles.map(art => `
                    <div style="display:flex; justify-content:space-between; padding:8px 0; border-bottom:1px solid #f9f9f9; font-size:12px;">
                        <span>${art.nom}</span>
                        ${!art.pret ? `<button onclick="marquerFini(${t.id}, ${art.uid})" style="background:var(--success); color:white; border:none; border-radius:5px; padding:4px 8px;">FINI</button>` : `<span style="color:var(--success); font-weight:700;">✔ ${art.done_by}</span>`}
                    </div>`).join('')}
                ${prog === 100 ? `<button onclick="livrer(${t.id})" class="btn-main" style="width:100%; margin-top:10px; background:black;">LIVRER</button>` : ''}
            </div>`;
    }).join('');
}

function marquerFini(tid, auid) {
    const t = db.find(x => x.id === tid), art = t.articles.find(a => a.uid === auid);
    art.pret = true; art.done_by = currentUser.nom;
    localStorage.setItem('sc_db_v8', JSON.stringify(db)); afficherAtelier();
}

function livrer(id) {
    const t = db.find(x => x.id === id);
    if(confirm("Confirmer livraison ?")) { t.statut = "Livré"; t.paye = t.total; localStorage.setItem('sc_db_v8', JSON.stringify(db)); afficherAtelier(); }
}

function afficherStats(periode) {
    const now = new Date(); let total = 0;
    db.forEach(t => {
        const d = new Date(t.timestamp);
        if(periode==='jour' && d.toDateString() === now.toDateString()) total += t.paye;
        if(periode==='mois' && d.getMonth() === now.getMonth()) total += t.paye;
    });
    document.getElementById('admin-stats-ui').innerHTML = `<div class="card" style="margin:0; background:var(--primary); color:white; text-align:center;"><h3>${total} F CFA</h3><small>Recette ${periode}</small></div>`;
}

function ajouterEmploye() {
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-log').value, p = document.getElementById('ns-pass').value, r = document.getElementById('ns-role').value;
    if(n && l && p) {
        staff.push({nom:n, login:l, pass:p, role:r});
        localStorage.setItem('sc_staff_v8', JSON.stringify(staff));
        afficherPersonnel(); alert("Utilisateur Ajouté !");
    }
}

function afficherPersonnel() {
    document.getElementById('staff-list').innerHTML = staff.map((e, i) => `
        <div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;">
            <span>${e.nom} <span class="role-badge role-${e.role.toLowerCase()}">${e.role}</span></span>
            ${i!==0 ? `<button onclick="staff.splice(${i},1);localStorage.setItem('sc_staff_v8',JSON.stringify(staff));afficherPersonnel()" style="color:var(--danger); border:none; background:none; font-weight:800;">✕</button>` : ''}
        </div>`).join('');
}

function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `<div style="display:flex; justify-content:space-between; padding:8px 0;"><span>${s.nom}</span><b>${s.qte.toFixed(2)} ${s.unite}</b></div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(q) { stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks_v8', JSON.stringify(stocks)); afficherStocks(); }
}

function tab(id) {
    if(id === 'section-admin') {
        if(currentUser.role !== "ADMIN") {
            document.getElementById('admin-content').classList.add('hidden');
            document.getElementById('admin-lock-msg').classList.remove('hidden');
        } else {
            document.getElementById('admin-content').classList.remove('hidden');
            document.getElementById('admin-lock-msg').classList.add('hidden');
            afficherStats('jour'); afficherPersonnel();
        }
    }
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') afficherAtelier();
    if(id === 'section-stocks') afficherStocks();
}

function rechercherClient() {
    const tel = document.getElementById('tel-client').value;
    if(tel.length >= 8) { const found = db.find(t => t.tel && t.tel.includes(tel)); if(found) document.getElementById('client-nom').value = found.client; }
}
</script>
</body>
</html>
