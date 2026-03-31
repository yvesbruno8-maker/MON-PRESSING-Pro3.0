<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING - GESTION PRO</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f8f9fa; --radius: 16px; --shadow: 0 8px 30px rgba(0,0,0,0.1);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); }
        .hidden { display: none !important; }
        .card { background: white; padding: 18px; border-radius: var(--radius); box-shadow: var(--shadow); margin-bottom: 15px; border: 1px solid rgba(0,0,0,0.03); }
        input, select, button { width: 100%; padding: 13px; margin: 6px 0; border-radius: 10px; border: 2px solid #eee; font-size: 14px; outline: none; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; transition: 0.2s; }
        button:active { transform: scale(0.98); }
        
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 5px; margin-bottom: 15px; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 10px; height: 50px; padding: 2px; }
        .nav-bar button.active { background: var(--primary); color: white; }

        .logo-img { max-width: 140px; display: block; margin: 0 auto 10px; }
        .panier-item { display:flex; justify-content:space-between; background:#f9f9f9; padding:8px; border-radius:8px; margin-bottom:5px; font-size:13px; }
        
        .badge-late { background: var(--danger); color: white; padding: 3px 7px; border-radius: 5px; font-size: 10px; animation: flash 1s infinite; font-weight: bold; }
        @keyframes flash { 0%, 100% { opacity: 1; } 50% { opacity: 0.5; } }

        @media print {
            body * { visibility: hidden; }
            #ticket-print, #ticket-print * { visibility: visible; }
            #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; color: black; }
        }
    </style>
</head>
<body>

<div id="loader" class="hidden" style="position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(255,255,255,0.8);z-index:9999;display:flex;align-items:center;justify-content:center;">⚡ Calcul des stocks...</div>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-img">
    <h2 style="color:var(--primary); margin:0;">SUPER CLEAN</h2>
    <p style="font-size:11px; font-style:italic; margin-bottom:20px;">"Le bien-être de vos vêtements..."</p>
    <div style="width:280px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success);">SE CONNECTER</button>
    </div>
</div>

<div id="app" class="hidden">
    <header style="text-align:center; margin-bottom:15px;">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="max-width:60px;">
        <div id="user-info" style="font-size:10px; font-weight:bold;"></div>
    </header>

    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="nav-caisse" class="active">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="nav-atelier">👕 ATELIER</button>
        <button onclick="tab('section-stocks')" id="nav-stocks">🧼 STOCKS</button>
        <button onclick="tab('section-admin')" id="nav-admin">👑 ADMIN</button>
    </nav>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <input type="tel" id="tel-client" placeholder="WhatsApp (ex: 699...)" onkeyup="rechercherClient()">
            <input type="text" id="client" placeholder="Nom du Client">
            <div style="font-size:10px; font-weight:bold; color:var(--primary); margin: 8px 0 2px;">📅 DATE DE RETRAIT PRÉVUE</div>
            <input type="date" id="date-retrait">
            <hr style="border:0; border-top:1px solid #eee; margin:10px 0;">
            <select id="select-article"></select>
            <button onclick="ajouterAuPanier()" style="background:var(--secondary);">AJOUTER L'ARTICLE</button>
        </div>

        <div id="liste-panier"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <div style="display:flex; justify-content:space-between;"><span>TOTAL :</span><b id="total-val">0 F</b></div>
            
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:8px; margin-top:10px;">
                <select id="type-paiement" onchange="majAutoTotal()" style="background:rgba(255,255,255,0.1); color:white; border:none;">
                    <option value="Avance" style="color:black;">💰 Avance</option>
                    <option value="Totalité" style="color:black;">💎 Totalité</option>
                </select>
                <select id="mode-paiement" style="background:rgba(255,255,255,0.1); color:white; border:none;">
                    <option value="Cash" style="color:black;">💵 Cash</option>
                    <option value="OM" style="color:black;">🍊 Orange</option>
                    <option value="Momo" style="color:black;">🟡 Momo</option>
                </select>
            </div>
            <input type="number" id="montant-paye" placeholder="Somme versée" oninput="calculerReste()" style="margin-top:10px;">
            <div id="reste-label" style="text-align:right; color:var(--warning); font-size:12px; font-weight:bold;">Reste : 0 F</div>
        </div>
        <button onclick="validerCommande()" style="background:var(--success); height:55px; font-size:16px;">🖨️ ENCAISSER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div id="liste-atelier"></div>
    </div>

    <div id="section-stocks" class="tab-content hidden">
        <div class="card">
            <h4>📦 État des Consommables</h4>
            <div id="stock-list-ui"></div>
        </div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card" style="border-top:5px solid var(--primary);">
            <h3 style="margin:0 0 15px;">📊 Rapport Financier</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:5px; margin-bottom:15px;">
                <button onclick="afficherStats('jour')" id="s-j" style="font-size:10px; height:35px;">JOUR</button>
                <button onclick="afficherStats('mois')" id="s-m" style="font-size:10px; height:35px;">MOIS</button>
                <button onclick="afficherStats('an')" id="s-a" style="font-size:10px; height:35px;">AN</button>
            </div>
            <div id="stats-detail" style="background:var(--light); padding:10px; border-radius:10px; font-size:13px;">
                </div>
        </div>
        <div class="card">
            <h4>💸 Dépenses</h4>
            <input type="text" id="dep-motif" placeholder="Motif">
            <input type="number" id="dep-montant" placeholder="Montant">
            <button onclick="ajouterDepense()" style="background:var(--danger);">ENREGISTRER FRAIS</button>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:'Courier New', monospace; width:80mm; padding:5mm; background:white;">
    <center>
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:50mm;">
        <br><i>"Le bien-être de vos vêtements..."</i><br>
        <small>📍 Nkounga Immeuble de Karche</small><br>
        <small>📞 675 864 103 / 692 315 267</small>
    </center>
    <hr style="border:1px dashed #000;">
    <b>TICKET #<span id="tk-id"></span></b><br>
    Client: <span id="tk-cli"></span><br>
    Date: <span id="tk-date"></span>
    <hr style="border:1px dashed #000;">
    <div id="tk-items"></div>
    <hr style="border:1px dashed #000;">
    <div style="text-align:right;">
        TOTAL : <b id="tk-tot"></b> F<br>
        PAYÉ : <span id="tk-pay"></span> F (<span id="tk-mod"></span>)<br>
        <b>RESTE : <span id="tk-res"></span> F</b>
    </div>
    <hr style="border:1px dashed #000;">
    <center><b>RETRAIT PRÉVU LE : <span id="tk-ret"></span></b></center>
</div>

<script>
// --- DONNÉES TARIFS ---
const TARIFS = {
    "LES HAUTS": {"T-Shirt":500,"Chemise":500,"Polo":500,"Pull Over":700,"Blouson Std":1000,"Blouson Lourd":1500,"Démembré":300,"Veste 2p":1500,"Veste 3p":2000},
    "BAS & JUPES": {"Pantalon simple":500,"Pantalon jeans":500,"Short":500,"Jupe simple":500,"Jupe plissée":1000},
    "ENSEMBLES": {"Costume":2000,"Costume Luxe":2500,"Boubou 2p":1500,"Boubou 3p":2500,"Pyjama":1000,"Jogging":1500},
    "LES ROBES": {"Robe simple":500,"Robe soirée":3000,"Kaba court":1000,"Kaba long":1500,"Toge":3500},
    "ENFANTS": {"Ensemble":1000,"Robe/Haut/Bas":500},
    "LIT": {"1 Drap":750,"Ens. 1 Drap":1500,"Couette":2000,"Couverture Std":3000,"Couverture Lourde":4000},
    "AUTRES": {"Rideau":1500,"Serviette":500,"Tennis":500,"Cravate":500}
};

// --- INITIALISATION ---
let db = JSON.parse(localStorage.getItem('sc_db')) || [];
let depenses = JSON.parse(localStorage.getItem('sc_dep')) || [];
let stocks = JSON.parse(localStorage.getItem('sc_stock')) || [
    {nom: "Savon", qte: 50, unite: "L", seuil: 5},
    {nom: "Parfum", qte: 10, unite: "L", seuil: 1},
    {nom: "Cintre", qte: 200, unite: "pcs", seuil: 20},
    {nom: "Housse", qte: 100, unite: "pcs", seuil: 10}
];
let user = null;
let panier = [];

function login() {
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    if(l === "admin" && p === "0000") {
        user = {nom: "Directeur", role: "ADMIN"};
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('user-info').innerText = "Connecté : " + user.nom;
        initApp();
    } else { alert("Erreur identifiants"); }
}

function initApp() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Choisir Article --</option>';
    for(let cat in TARIFS) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in TARIFS[cat]) {
            let o = document.createElement('option'); o.value = TARIFS[cat][art];
            o.innerText = `${art} (${TARIFS[cat][art]}F)`; g.appendChild(o);
        }
        s.appendChild(g);
    }
    afficherStocks();
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('nav-'+id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') afficherAtelier();
    if(id === 'section-admin') afficherStats('jour');
}

// --- LOGIQUE PANIER ---
function ajouterAuPanier() {
    const s = document.getElementById('select-article');
    if(!s.value) return;
    const n = s.options[s.selectedIndex].text.split(' (')[0];
    const p = parseInt(s.value);
    panier.push({nom: n, prix: p, pret: false});
    renderPanier();
}

function renderPanier() {
    const list = document.getElementById('liste-panier');
    const totVal = panier.reduce((a,b) => a+b.prix, 0);
    list.innerHTML = panier.map((x, i) => `<div class="panier-item"><span>${x.nom}</span><b>${x.prix}F <span onclick="panier.splice(${i},1);renderPanier()" style="color:red;margin-left:8px;">✕</span></b></div>`).join('');
    document.getElementById('total-val').innerText = totVal + " F";
    calculerReste();
}

function majAutoTotal() {
    if(document.getElementById('type-paiement').value === "Totalité") {
        document.getElementById('montant-paye').value = panier.reduce((a,b) => a+b.prix, 0);
    }
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b) => a+b.prix, 0);
    const p = parseInt(document.getElementById('montant-paye').value) || 0;
    document.getElementById('reste-label').innerText = "Reste : " + (t - p) + " F";
}

// --- LOGIQUE VALIDATION & STOCKS ---
function validerCommande() {
    const nom = document.getElementById('client').value;
    const tel = document.getElementById('tel-client').value;
    const retrait = document.getElementById('date-retrait').value;
    if(!nom || !retrait || panier.length === 0) return alert("Infos manquantes !");

    document.getElementById('loader').classList.remove('hidden');

    // Déduction Stocks
    let luxeCount = 0;
    panier.forEach(item => {
        const luxe = ["veste", "blouson", "robe", "costume", "toge"];
        if(luxe.some(l => item.nom.toLowerCase().includes(l))) luxeCount++;
    });

    const vTotal = panier.length;
    stocks.find(s => s.nom === "Savon").qte -= (vTotal * 0.1);
    stocks.find(s => s.nom === "Parfum").qte -= (vTotal * 0.02);
    stocks.find(s => s.nom === "Cintre").qte -= luxeCount;
    stocks.find(s => s.nom === "Housse").qte -= luxeCount;

    const tot = panier.reduce((a,b) => a+b.prix, 0);
    const pay = parseInt(document.getElementById('montant-paye').value) || 0;
    const mode = document.getElementById('mode-paiement').value;

    const cmd = {
        id: Math.floor(1000 + Math.random()*8999),
        client: nom, tel: tel, items: [...panier], total: tot,
        paye: pay, reste: tot-pay, mode: mode, retrait: retrait,
        date: new Date().toLocaleDateString('fr-FR'), status: "Atelier",
        dateBrute: new Date().toISOString().split('T')[0]
    };

    db.unshift(cmd);
    localStorage.setItem('sc_db', JSON.stringify(db));
    localStorage.setItem('sc_stock', JSON.stringify(stocks));

    // Ticket
    document.getElementById('tk-id').innerText = cmd.id;
    document.getElementById('tk-cli').innerText = cmd.client;
    document.getElementById('tk-date').innerText = cmd.date;
    document.getElementById('tk-tot').innerText = cmd.total;
    document.getElementById('tk-pay').innerText = cmd.paye;
    document.getElementById('tk-mod').innerText = cmd.mode;
    document.getElementById('tk-res').innerText = cmd.reste;
    document.getElementById('tk-ret').innerText = cmd.retrait;
    document.getElementById('tk-items').innerHTML = panier.map(x => `<div>- ${x.nom}</div>`).join('');

    setTimeout(() => {
        window.print();
        const msg = `*SUPER CLEAN PRESSING*%0ANouveau Ticket #${cmd.id}%0AClient: ${cmd.client}%0ATotal: ${cmd.total}F%0APayé: ${cmd.paye}F (${cmd.mode})%0ARetrait: ${cmd.retrait}`;
        window.open(`https://wa.me/237${tel}?text=${msg}`);
        location.reload();
    }, 1000);
}

// --- ATELIER ---
function afficherAtelier() {
    const cont = document.getElementById('liste-atelier');
    const today = new Date().toISOString().split('T')[0];
    cont.innerHTML = db.filter(c => c.status !== "Livré").map(c => {
        const estRetard = c.retrait < today;
        const finiCount = c.items.filter(i => i.pret).length;
        const perc = Math.round((finiCount/c.items.length)*100);
        return `
            <div class="card" style="border-left:5px solid ${perc===100?'green':'blue'}">
                <div style="display:flex; justify-content:space-between;">
                    <b>#${c.id} - ${c.client}</b>
                    <span>${perc}%</span>
                </div>
                ${estRetard ? '<div class="badge-late">⚠️ RETARD</div>' : ''}
                <div style="font-size:11px; margin:5px 0;">Retrait: ${c.retrait}</div>
                <div style="background:#eee; height:6px; border-radius:3px;"><div style="width:${perc}%; background:green; height:100%;"></div></div>
                <div style="margin-top:10px;">
                    ${c.items.map((item, idx) => `
                        <div style="font-size:12px; display:flex; justify-content:space-between; align-items:center; margin-bottom:4px;">
                            <span style="${item.pret?'text-decoration:line-through;opacity:0.5':''}">${item.nom}</span>
                            ${!item.pret ? `<button onclick="finirArticle(${c.id}, ${idx})" style="width:auto; padding:2px 8px; font-size:10px;">PRÊT</button>` : '✅'}
                        </div>
                    `).join('')}
                </div>
                ${perc === 100 ? `<button onclick="livrer(${c.id})" style="background:black; margin-top:10px;">📦 LIVRER (Solde: ${c.reste}F)</button>` : ''}
            </div>
        `;
    }).join('');
}

function finirArticle(cid, idx) {
    const c = db.find(x => x.id === cid);
    c.items[idx].pret = true;
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

function livrer(cid) {
    const c = db.find(x => x.id === cid);
    if(c.reste > 0) { if(!confirm(`Paiement du solde de ${c.reste}F reçu ?`)) return; }
    c.status = "Livré";
    localStorage.setItem('sc_db', JSON.stringify(db));
    afficherAtelier();
}

// --- ADMIN & STATS ---
function afficherStats(per) {
    const now = new Date();
    const j = now.toLocaleDateString('fr-FR');
    const m = (now.getMonth()+1).toString().padStart(2,'0');
    const a = now.getFullYear().toString();

    let csh=0, o=0, mo=0, f=0;
    db.forEach(c => {
        const [dd, mm, yy] = c.date.split('/');
        let ok = false;
        if(per==='jour' && c.date === j) ok=true;
        if(per==='mois' && mm === m && yy === a) ok=true;
        if(per==='an' && yy === a) ok=true;
        if(ok) {
            if(c.mode==="Cash") csh += c.paye;
            if(c.mode==="OM") o += c.paye;
            if(c.mode==="Momo") mo += c.paye;
        }
    });

    depenses.forEach(d => {
        const [dd, mm, yy] = d.date.split('/');
        if(per==='jour' && d.date === j) f += d.montant;
        if(per==='mois' && mm === m && yy === a) f += d.montant;
        if(per==='an' && yy === a) f += d.montant;
    });

    document.getElementById('stats-detail').innerHTML = `
        <b>Bilan ${per.toUpperCase()}</b><br>
        💵 Cash: ${csh} F<br>
        🍊 Orange: ${o} F<br>
        🟡 Momo: ${mo} F<hr>
        💰 Total Recettes: ${csh+o+mo} F<br>
        💸 Dépenses: ${f} F<br>
        💎 NET: ${(csh+o+mo)-f} F
    `;
}

function ajouterDepense() {
    const m = document.getElementById('dep-motif').value;
    const v = parseInt(document.getElementById('dep-montant').value);
    if(!m || !v) return;
    depenses.push({motif: m, montant: v, date: new Date().toLocaleDateString('fr-FR')});
    localStorage.setItem('sc_dep', JSON.stringify(depenses));
    alert("Dépense enregistrée");
    afficherStats('jour');
}

function afficherStocks() {
    document.getElementById('stock-list-ui').innerHTML = stocks.map(s => `
        <div style="display:flex; justify-content:space-between; padding:8px; border-bottom:1px solid #eee; ${s.qte<=s.seuil?'color:red;font-weight:bold':''}">
            <span>${s.nom}</span>
            <span>${parseFloat(s.qte.toFixed(2))} ${s.unite} ${s.qte<=s.seuil?'⚠️':''}</span>
        </div>
    `).join('');
}

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length >= 8) {
        const found = db.find(c => c.tel && c.tel.includes(t));
        if(found) document.getElementById('client').value = found.client;
    }
}
</script>
</body>
</html>
