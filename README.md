<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN SYSTEM v2.1</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --radius: 12px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); font-size: 14px; user-select: none; }
        .hidden { display: none !important; }
        .card { background: white; padding: 15px; border-radius: var(--radius); box-shadow: 0 4px 12px rgba(0,0,0,0.08); margin-bottom: 12px; border: 1px solid #eee; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 8px; border: 1px solid #ccc; font-size: 14px; box-sizing: border-box; outline: none; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; }
        button:active { opacity: 0.7; transform: scale(0.98); }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; position: sticky; top: 0; z-index: 100; background: var(--light); padding: 5px 0; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 9px; height: 45px; padding: 2px; }
        .nav-bar button.active { background: var(--primary); color: white; }

        .progress-bar { background: #eee; height: 10px; border-radius: 5px; margin: 10px 0; overflow: hidden; }
        .progress-fill { background: var(--success); height: 100%; transition: 0.5s; width: 0%; }

        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 10px; border-bottom: 1px solid #f0f0f0; }
        .item-row.ready { background: #f0fff4; color: #27ae60; text-decoration: line-through; }
        .badge-loc { background: var(--dark); color: white; padding: 2px 6px; border-radius: 4px; font-size: 10px; margin-left: 5px; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="max-width:130px; border-radius:50%;">
    <h2 style="color:var(--primary); margin-top:15px;">SUPER CLEAN</h2>
    <div style="width:280px; margin-top:10px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="height:50px;">OUVRIR LA SESSION</button>
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
            <div style="font-size:11px; font-weight:800; color:var(--primary);">DATE DE RETRAIT :</div>
            <input type="date" id="date-retrait">
            <select id="select-article" style="margin-top:10px; border:2px solid var(--primary);"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary);">+ AJOUTER L'ARTICLE</button>
        </div>

        <div id="panier-liste"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <span>NET À PAYER :</span>
                <b id="total-caisse" style="font-size:22px; color:var(--success);">0 F</b>
            </div>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:10px;">
                <select id="mode-pay">
                    <option value="Cash">💵 Cash</option>
                    <option value="OM">🍊 Orange</option>
                    <option value="Momo">🟡 Momo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Avance (F)" oninput="calculerReste()">
            </div>
            <div id="reste-caisse" style="text-align:right; font-size:13px; color:var(--warning);">Reste: 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:60px; font-size:18px;">🖨️ VALIDER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <h3 style="margin-left:10px;">👕 Suivi Pièce par Pièce</h3>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h3>🧼 État des Stocks</h3>
            <div id="stock-display"></div>
            <hr>
            <select id="upd-stock-item"></select>
            <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
            <button onclick="modifierStockManuel()" style="background:var(--warning); color:black;">AJOUTER AU STOCK</button>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card">
            <h3>📊 Recettes du Jour</h3>
            <div id="admin-stats-ui"></div>
        </div>
        <div class="card">
            <h3>👥 Personnel</h3>
            <div id="staff-list"></div>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; width:80mm; padding:5mm; font-family:monospace;">
    <center><b>SUPER CLEAN PRESSING</b><br>Nkounga, Immeuble Karche</center>
    <div id="tk-body" style="font-size:12px; margin-top:10px;"></div>
</div>

<script>
// --- CONFIGURATION ---
const GRILLE = {
    "HAUTS": {"Chemise": 500, "Veste": 1500, "T-Shirt": 500, "Polo": 500, "Pull": 700, "Blouson": 1500},
    "BAS": {"Pantalon": 500, "Short": 500, "Jupe": 500, "Jupe plissée": 1000},
    "TENUES": {"Costume 2p": 2500, "Robe soirée": 3000, "Boubou": 2000, "Toge": 3500},
    "MAISON": {"Drap": 1000, "Couette": 2000, "Rideau": 1500}
};

let db = JSON.parse(localStorage.getItem('sc_db')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stocks')) || [
    {id:'savon', nom:"Savon Liquide", qte:20, unite:"L"}, {id:'parfum', nom:"Parfum", qte:5, unite:"L"}
];
let staff = JSON.parse(localStorage.getItem('sc_staff')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [];

// --- FONCTIONS ---
function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    const found = staff.find(s => s.login === l && s.pass === p);
    if(found) { 
        document.getElementById('auth-screen').classList.add('hidden'); 
        document.getElementById('app').classList.remove('hidden'); 
        initApp(); 
    } else alert("Erreur d'accès");
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Choisir Article --</option>';
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
    panier.push({ uid: Date.now()+Math.random(), nom: s.options[s.selectedIndex].text.split(' (')[0], prix: parseInt(s.value), pret: false, loc: "" });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    document.getElementById('panier-liste').innerHTML = panier.map((x,i) => `
        <div class="item-row"><span>${x.nom}</span><b>${x.prix}F <span onclick="panier.splice(${i},1);renderPanier()" style="color:red; cursor:pointer;">✕</span></b></div>`).join('');
    document.getElementById('total-caisse').innerText = t + " F";
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    const p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste à payer: ${Math.max(0, t-p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value;
    const dateR = document.getElementById('date-retrait').value;
    if(!nom || !dateR || panier.length === 0) return alert("Complétez le client, la date et les articles !");

    const ticket = {
        id: Math.floor(1000 + Math.random()*8999),
        client: nom, tel: document.getElementById('tel-client').value,
        articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0),
        paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value,
        date_depot: new Date().toLocaleDateString('fr-FR'), date_retrait: dateR, statut: "Atelier"
    };

    db.unshift(ticket);
    localStorage.setItem('sc_db', JSON.stringify(db));
    stocks[0].qte -= 0.1; // Déduction auto savon
    localStorage.setItem('sc_stocks', JSON.stringify(stocks));

    document.getElementById('tk-body').innerHTML = `TICKET #${ticket.id}<br>Client: ${ticket.client}<br>Dépôt: ${ticket.date_depot}<br>Retrait: ${ticket.date_retrait}<hr>` + 
        panier.map(a => `${a.nom} : ${a.prix}F`).join('<br>') + `<hr>TOTAL: ${ticket.total}F<br>PAYÉ: ${ticket.paye}F`;
    
    window.print();
    location.reload();
}

function afficherAtelier() {
    const list = document.getElementById('atelier-liste');
    const encours = db.filter(t => t.statut !== "Livré");
    
    list.innerHTML = encours.map(t => {
        const fini = t.articles.filter(a => a.pret).length;
        const prog = Math.round((fini / t.articles.length) * 100) || 0;

        return `
            <div class="card" style="border-left: 5px solid ${prog === 100 ? 'var(--success)' : 'var(--primary)'}">
                <div style="display:flex; justify-content:space-between;"><b>#${t.id} - ${t.client}</b> <small>${t.date_retrait}</small></div>
                <div class="progress-bar"><div class="progress-fill" style="width:${prog}%"></div></div>
                ${t.articles.map(art => `
                    <div class="item-row ${art.pret ? 'ready' : ''}">
                        <span>${art.nom} ${art.loc ? `<span class="badge-loc">📍 ${art.loc}</span>` : ''}</span>
                        ${!art.pret ? `<button onclick="finirPiece(${t.id}, ${art.uid})" style="width:auto; padding:5px; font-size:10px; background:var(--success);">PRÊT</button>` : '✅'}
                    </div>`).join('')}
                ${prog === 100 ? `<button onclick="livrer(${t.id})" style="background:black; margin-top:10px;">LIVRER (Solde: ${t.total - t.paye}F)</button>` : ''}
            </div>`;
    }).join('');
}

function finirPiece(tid, auid) {
    const loc = prompt("Emplacement (Ex: Casier A1) ?");
    if(!loc) return;
    const t = db.find(x => x.id === tid);
    const a = t.articles.find(x => x.uid === auid);
    a.pret = true; a.loc = loc;
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

function livrer(id) {
    const t = db.find(x => x.id === id);
    if(confirm(`Livrer et encaisser le solde de ${t.total - t.paye}F ?`)) {
        t.statut = "Livré"; t.paye = t.total;
        localStorage.setItem('sc_db', JSON.stringify(db));
        afficherAtelier();
    }
}

function afficherStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div style="display:flex; justify-content:space-between; padding:5px 0; border-bottom:1px solid #eee;">
            <span>${s.nom}</span><b>${s.qte.toFixed(2)} ${s.unite}</b>
        </div>`).join('');
}

function modifierStockManuel() {
    const id = document.getElementById('upd-stock-item').value;
    const q = parseFloat(document.getElementById('upd-stock-qte').value);
    if(q) { stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks', JSON.stringify(stocks)); afficherStocks(); }
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') afficherAtelier();
    if(id === 'section-admin') {
        const j = new Date().toLocaleDateString('fr-FR');
        let tot = 0; db.forEach(t => { if(t.date_depot === j) tot += t.paye; });
        document.getElementById('admin-stats-ui').innerHTML = `<h1 style="text-align:center; color:var(--success);">${tot} F</h1><p style="text-align:center;">Total encaissé aujourd'hui</p>`;
        document.getElementById('staff-list').innerHTML = staff.map(e => `<div>• ${e.nom} (${e.role})</div>`).join('');
    }
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
