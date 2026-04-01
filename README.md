<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING ERP v18.0 - TITANIUM</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #004182; --secondary: #00b4d8; --success: #2ecc71; 
            --danger: #ef233c; --warning: #ff9f1c; --dark: #0a192f; 
            --light: #f0f7ff; --radius: 20px;
        }
        body.dark-mode { --light: #0a192f; --dark: #f0f7ff; background-color: #0a192f; }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; padding-bottom: 90px; transition: 0.3s; overflow-x: hidden; }
        .hidden { display: none !important; }
        
        /* --- ELEMENTS UI --- */
        .header { background: white; padding: 12px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 4px solid var(--secondary); position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        body.dark-mode .header, body.dark-mode .card, body.dark-mode .nav-bar { background: #112240; color: white; border-color: #1d2d50; }
        .logo-small { width: 40px; border-radius: 50%; border: 2px solid var(--secondary); }
        .card { background: white; margin: 12px; padding: 18px; border-radius: var(--radius); box-shadow: 0 8px 20px rgba(0, 65, 130, 0.05); border: 1px solid rgba(0, 65, 130, 0.05); position: relative; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 14px; border-radius: 12px; margin-bottom: 10px; width: 100%; box-sizing: border-box; font-size: 14px; outline: none; transition: 0.2s; }
        input:focus { border-color: var(--secondary); background: white; }
        body.dark-mode input, body.dark-mode select { background: #1d2d50; color: white; }
        .btn-main { background: linear-gradient(135deg, var(--primary), var(--secondary)); color: white; border: none; padding: 16px; border-radius: 14px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; transition: 0.2s; box-shadow: 0 4px 12px rgba(0, 65, 130, 0.2); }
        .btn-main:active { transform: scale(0.95); }

        /* --- NAVIGATION --- */
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px; padding: 8px; background: #fff; position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; z-index: 2000; box-sizing: border-box; }
        .nav-bar button { background: #f8f9fa; border: none; color: #7f8c8d; font-weight: 800; font-size: 9px; padding: 12px 2px; border-radius: 12px; transition: 0.2s; }
        body.dark-mode .nav-bar button { background: #1d2d50; color: #8892b0; }
        .nav-bar button.active { background: var(--primary); color: white; transform: translateY(-5px); }

        /* --- ANIMATIONS & BADGES --- */
        .badge-solde { background: var(--danger); color: white; padding: 4px 8px; border-radius: 6px; font-size: 10px; font-weight: 900; }
        .retard-tag { position: absolute; top: -10px; right: 10px; background: var(--danger); color: white; font-size: 10px; padding: 2px 10px; border-radius: 20px; font-weight: 900; box-shadow: 0 4px 8px rgba(239, 35, 60, 0.3); }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(239, 35, 60, 0.4); } 70% { box-shadow: 0 0 0 10px rgba(239, 35, 60, 0); } 100% { box-shadow: 0 0 0 0 rgba(239, 35, 60, 0); } }
        
        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height:100vh; display:flex; align-items:center; justify-content:center; background:linear-gradient(135deg, var(--primary), var(--secondary));">
        <div style="background:white; padding:40px; border-radius:30px; width:85%; max-width:340px; text-align:center; box-shadow: 0 20px 50px rgba(0,0,0,0.3);">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:100px; border-radius:50%; border: 4px solid var(--light); margin-bottom:15px;">
            <h2 style="margin:0; color:var(--primary); font-weight:900;">SUPER CLEAN PRESSING</h2>
            <p style="font-size:11px; color:var(--secondary); font-weight:700; margin-bottom:25px; letter-spacing: 1px;">LE BIEN-ÊTRE DE VOS VÊTEMENTS</p>
            <input type="text" id="u-login" placeholder="Identifiant">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">OUVRIR LA SESSION</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div class="header">
        <div style="display:flex; align-items:center;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-small">
            <span style="font-weight:900; color:var(--primary); margin-left:10px; font-size:14px;">SUPER CLEAN PRESSING</span>
        </div>
        <button onclick="toggleDarkMode()" style="background:none; border:none; font-size:20px; cursor:pointer;">🌙</button>
    </div>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <input type="tel" id="tel-client" placeholder="Numéro WhatsApp" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom du Client">
            <input type="date" id="date-retrait">
            
            <div style="background: #fff8e1; padding: 10px; border-radius: 12px; border: 1px solid #ffe082; margin-bottom: 15px; display: flex; align-items: center; justify-content: space-between;">
                <b style="font-size: 11px; color: #f57f17;">⚡ SERVICE EXPRESS (+50%)</b>
                <input type="checkbox" id="check-express" onchange="renderPanier()" style="width: 22px; height: 22px;">
            </div>

            <div style="background:#f8f9fa; padding:12px; border-radius:15px; border:1px dashed #ddd;">
                <select id="select-article" onchange="majPrixSuggere()"></select>
                <input type="number" id="prix-final" placeholder="Prix final">
                <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); font-size:12px;">+ AJOUTER L'ARTICLE</button>
            </div>
        </div>

        <div id="panier-liste" style="padding:0 12px;"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <button onclick="appliquerFidelite()" style="background:#f1c40f; color:#000; border:none; padding:8px; border-radius:8px; font-size:11px; font-weight:bold; width:100%; margin-bottom:12px;">🌟 RÉCOMPENSE FIDÉLITÉ (-500 F)</button>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:8px;">
                <select id="mode-payement" style="background:#2c3e50; color:white; font-size:11px;">
                    <option value="Espèces">💵 ESPÈCES</option>
                    <option value="Orange Money">🍊 ORANGE MONEY</option>
                    <option value="MTN MoMo">🟡 MTN MOMO</option>
                </select>
                <select id="type-payement" style="background:#2c3e50; color:white; font-size:11px;">
                    <option value="Avance">⏳ AVANCE</option>
                    <option value="Totalité">✅ TOTALITÉ</option>
                </select>
            </div>
            <div style="display:flex; justify-content:space-between; margin:10px 0;">
                <span style="opacity:0.8;">NET À PAYER :</span> <b id="total-caisse" style="font-size:24px; color:var(--success);">0 F</b>
            </div>
            <input type="number" id="paye-caisse" placeholder="Avance reçue" oninput="calculerReste()" style="background:#2c3e50; color:white; border:none;">
            <div id="reste-caisse" style="text-align:right; font-weight:800; color:var(--warning); font-size:14px;">Reste : 0 F</div>
        </div>
        <div style="padding:0 12px;"><button onclick="validerCommande()" class="btn-main">VALIDER & IMPRIMER TICKET</button></div>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div class="card"><input type="text" id="search-atelier" placeholder="🔍 Rechercher client ou N°..." onkeyup="filtrerAtelier()"></div>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card">
            <h3 style="margin-top:0; font-size:14px; color:var(--primary);">📈 PERFORMANCE COMPARATIVE</h3>
            <div id="stats-comparative-ui"></div>
        </div>

        <div class="card" style="background:var(--primary); color:white; text-align:center; padding:25px;">
            <small style="opacity:0.8;">NET EN CAISSE (RECETTES - DÉPENSES)</small>
            <div id="admin-stats-ui" style="font-weight:900; font-size:28px; margin:10px 0;">0 F CFA</div>
            <button onclick="genererRapportCloture()" style="background:#25D366; color:white; border:none; padding:15px; border-radius:12px; width:100%; font-weight:bold; cursor:pointer;">📤 ENVOYER RAPPORT WHATSAPP</button>
        </div>

        <div class="card">
            <h3>👥 GESTION DU PERSONNEL</h3>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:5px;">
                <input type="text" id="ns-nom" placeholder="Nom">
                <input type="text" id="ns-login" placeholder="ID">
                <input type="password" id="ns-pass" placeholder="Pass">
                <select id="ns-role">
                    <option value="CAISSIERE">CAISSIERE</option>
                    <option value="LAVEUR">LAVEUR</option>
                    <option value="REPASSEUR">REPASSEUR</option>
                    <option value="ADMIN">ADMIN</option>
                </select>
            </div>
            <button onclick="ajouterPersonnel()" class="btn-main" style="font-size:11px;">+ CRÉER COMPTE</button>
            <div id="staff-list-ui" style="margin-top:10px;"></div>
        </div>

        <div class="card">
            <h3>💸 DÉPENSES JOUR</h3>
            <input type="text" id="exp-motif" placeholder="Motif">
            <input type="number" id="exp-montant" placeholder="Montant">
            <button onclick="ajouterDepense()" class="btn-main" style="background:var(--danger);">ENREGISTRER CHARGE</button>
            <div id="liste-depenses-ui" style="margin-top:10px; font-size:11px;"></div>
        </div>

        <div class="card">
            <h3>📦 CONSOMMABLES & STOCK</h3>
            <div id="stk-liste"></div>
            <hr>
            <small>Ajouter/MAJ Produit :</small>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:5px; margin-top:5px;">
                <input type="text" id="stk-nom" placeholder="Produit">
                <input type="number" id="stk-qty" placeholder="Qté">
                <input type="text" id="stk-unite" placeholder="Unité">
                <input type="number" id="stk-seuil" placeholder="Seuil">
            </div>
            <button onclick="ajouterStock()" class="btn-main" style="font-size:11px;">+ METTRE À JOUR STOCK</button>
        </div>

        <div class="card">
            <h3>📜 JOURNAL D'ACTIVITÉ</h3>
            <div id="journal-ui" style="max-height:150px; overflow-y:auto; font-size:10px; background:#f9f9f9; padding:10px;"></div>
        </div>

        <div class="card" style="text-align:center;">
            <button onclick="exportData()" class="btn-main" style="background:#6c5ce7; margin-bottom:10px;">📤 CLOUD BACKUP (.JSON)</button>
            <input type="file" id="importFile" accept=".json" style="font-size:10px;">
            <button onclick="importData()" class="btn-main" style="background:var(--success);">📥 RESTAURER DONNÉES</button>
        </div>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="btn-caisse" class="active">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-atelier">👕 ATELIER</button>
        <button onclick="tab('section-admin')" id="btn-admin">👑 ADMIN</button>
        <button onclick="location.reload()" style="color:var(--danger)">🔴 QUITTER</button>
    </nav>
</div>

<div id="ticket-print" style="display:none; text-align:center; font-family:monospace; padding:15px;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:70px; border-radius:50%;"><br>
    <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
    Nkounga, Immeuble Karche<br>
    Tél: 675 864 103 / 692 315 267<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:12px;"></div>
    --------------------------------<br>
    <img id="tk-qrcode" src="" style="width:120px;"><br>
    <small>Scannez pour suivi WhatsApp</small><br>
    --------------------------------<br>
    "Le bien être de vos vêtements"<br>
    Merci de votre confiance !
</div>

<script>
// --- DONNÉES TARIFAIRES RÉELLES ---
const TARIFAIRE = {
    "LES HAUTS": {"T-Shirt":500, "Chemise":500, "Polo":500, "Pull Over":700, "Blouson Simple":1000, "Blouson Lourd":1500, "Démembré":300, "Veste 2P":1500, "Veste 3P":2000, "Pantalon simple":500, "Pantalon jeans":500, "Short":500, "Jupe simple":500, "Jupe plissée":1000},
    "ENSEMBLES": {"Costume simple":2000, "Costume luxe":2500, "Boubou":1500, "Boubou 3P":2500, "Pyjama 2P":1000, "Jogging":1500},
    "ROBES & TOGES": {"Robe simple":500, "Robe soirée":3000, "Kaba court":1000, "Kaba long":1500, "Toges (Avocats/Magi)":3500},
    "LINGES DE LIT": {"1 Drap":750, "Ensemble 1 Drap":1500, "Ensemble 2 Draps":2000, "Couette":2000, "Housse couette":2500, "Couverture P":3000, "Couverture G":4000},
    "BAIN & ACCESSOIRES": {"Serviette P":500, "Serviette G":1000, "Peignoir":2000, "Tennis":500, "Cravate":500, "Rideau lourd":1500, "Rideau léger":1000}
};

let db = JSON.parse(localStorage.getItem('sc_db_v18')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v18')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let depenses = JSON.parse(localStorage.getItem('sc_expenses_v18')) || [];
let journal = JSON.parse(localStorage.getItem('sc_logs_v18')) || [];
let stock = JSON.parse(localStorage.getItem('sc_stock_v18')) || [
    {nom:"SAVON LIQUIDE", qty:20, unite:"L", seuil:5},
    {nom:"CINTRES", qty:100, unite:"Pcs", seuil:20},
    {nom:"HOUSSES EMBALLAGE", qty:200, unite:"Pcs", seuil:50},
    {nom:"PARFUM", qty:5, unite:"L", seuil:1}
];
let panier = [], currentUser = null;

// --- AUTH & NAV ---
if(localStorage.getItem('sc_darkmode') === 'enabled') document.body.classList.add('dark-mode');

function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        initApp();
        enregistrerAction("A ouvert une session");
    } else alert("Identifiants incorrects !");
}

function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
    localStorage.setItem('sc_darkmode', document.body.classList.contains('dark-mode') ? 'enabled' : 'disabled');
}

function initApp() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Sélectionner l\'article --</option>';
    for(let cat in TARIFAIRE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in TARIFAIRE[cat]) {
            let o = document.createElement('option'); o.value = TARIFAIRE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        sel.appendChild(g);
    }
}

function tab(id) {
    if(id === 'section-admin' && currentUser.role !== 'ADMIN') return alert("Accès réservé !");
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('btn-' + id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') renderAtelier();
    if(id === 'section-admin') { updateAdminStats(); renderStaffList(); renderJournal(); renderStock(); calculerPerformance(); }
}

// --- GESTION CAISSE ---
function majPrixSuggere() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length >= 8) {
        const h = db.filter(x => x.tel && x.tel.includes(t));
        if(h.length > 0) {
            document.getElementById('client-nom').value = h[0].client;
            const totalA = h.reduce((a,b)=>a+b.articles.length,0);
            if(totalA % 10 === 0 && totalA > 0) alert("🎁 CLIENT FIDÈLE : Pensez à appliquer la remise !");
        }
    }
}

function ajouterAuPanier() {
    const s = document.getElementById('select-article'), p = document.getElementById('prix-final').value;
    if(!s.value || !p) return;
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p) });
    renderPanier();
}

function appliquerFidelite() { panier.push({ uid:Date.now(), nom:"🎁 REMISE FIDÉLITÉ", prix:-500 }); renderPanier(); }

function renderPanier() {
    let total = panier.reduce((a,b)=>a+b.prix, 0);
    if(document.getElementById('check-express').checked) total = Math.round(total * 1.5);
    document.getElementById('total-caisse').innerText = total + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`<div style="display:flex; justify-content:space-between; background:white; padding:10px; border-radius:10px; margin-bottom:5px; font-size:12px; border-left:4px solid var(--primary);"><b>${x.nom}</b> <span>${x.prix}F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none;">✕</button></span></div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = parseInt(document.getElementById('total-caisse').innerText), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${Math.max(0, t - p)} F CFA`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, tel = document.getElementById('tel-client').value, isEx = document.getElementById('check-express').checked;
    if(!nom || !tel || panier.length === 0) return alert("Données manquantes !");
    
    const totalF = parseInt(document.getElementById('total-caisse').innerText);
    const cmd = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier], express: isEx,
        total: totalF, paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-payement').value, type: document.getElementById('type-payement').value,
        retrait: document.getElementById('date-retrait').value, time: new Date().getTime(), statut: "Atelier"
    };

    // Déduction Consommables
    const nbA = panier.length;
    const s1 = stock.findIndex(p => p.nom.includes("CINTRE")), s2 = stock.findIndex(p => p.nom.includes("HOUSSE")), s3 = stock.findIndex(p => p.nom.includes("SAVON"));
    if(s1 !== -1) stock[s1].qty -= nbA; if(s2 !== -1) stock[s2].qty -= nbA; if(s3 !== -1) stock[s3].qty -= (nbA * 0.05);

    db.unshift(cmd); localStorage.setItem('sc_db_v18', JSON.stringify(db));
    localStorage.setItem('sc_stock_v18', JSON.stringify(stock));
    enregistrerAction(`Nouvelle commande #${cmd.id} (${cmd.total}F)`);

    // Ticket & QR
    const msgWA = `Bonjour SUPER CLEAN PRESSING, suivi commande #${cmd.id} de ${cmd.client}`;
    document.getElementById('tk-qrcode').src = `https://chart.googleapis.com/chart?chs=120x120&cht=qr&chl=${encodeURIComponent('https://wa.me/237675864103?text='+msgWA)}`;
    document.getElementById('tk-body').innerHTML = `TICKET #${cmd.id} ${isEx ? '[⚡EXPRESS]':''}<br>CLIENT: ${cmd.client}<br>TEL: ${cmd.tel}<br>--------------------------------<br>${panier.map(a=>`${a.nom}: ${a.prix}F`).join('<br>')}<br>--------------------------------<br>TOTAL: ${cmd.total} F<br>PAYÉ (${cmd.mode}): ${cmd.paye} F<br><b>SOLDE: ${cmd.total-cmd.paye} F</b><br>RDV: ${cmd.retrait}`;
    
    setTimeout(() => { window.print(); location.reload(); }, 600);
}

// --- ATELIER & ADMIN ---
function renderAtelier() {
    const t = new Date(); t.setHours(0,0,0,0);
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => {
        const dR = new Date(c.retrait); const diff = Math.floor((t - dR) / (1000*60*60*24));
        let st = "", tg = "";
        if(diff > 0) { st = diff >= 3 ? "border:3px solid #800000; animation:pulse 2s infinite;" : "border:2px solid red;"; tg = `<span class="retard-tag">${diff >= 3 ? '🚨 CRITIQUE' : '⚠️ RETARD'} ${diff}J</span>`; }
        return `<div class="card" style="${st}">${tg}<b>#${c.id} - ${c.client}</b><br><small>RDV: ${c.retrait} | Solde: ${c.total-c.paye}F</small><div style="display:grid; grid-template-columns:1fr 1fr; gap:5px; margin-top:10px;"><button class="btn-main" style="background:#25D366; font-size:10px; padding:10px;" onclick="window.open('https://wa.me/237${c.tel}')">📲 RAPPEL</button><button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark); font-size:10px; padding:10px;">LIVRER</button></div></div>`;
    }).join('') || "<p style='text-align:center;'>Aucune commande à l'atelier.</p>";
}

function livrer(id) {
    const c = db.find(x => x.id === id);
    if(confirm(`Livrer #${id}? Encaisser solde: ${c.total - c.paye}F`)) {
        c.statut = "Livré"; c.paye = c.total; localStorage.setItem('sc_db_v18', JSON.stringify(db));
        enregistrerAction(`Livraison commande #${id}`); renderAtelier();
    }
}

function updateAdminStats() {
    const rec = db.reduce((a,b)=>a+b.paye,0), dep = depenses.reduce((a,b)=>a+b.montant,0);
    document.getElementById('admin-stats-ui').innerText = (rec - dep).toLocaleString() + " F CFA";
    document.getElementById('liste-depenses-ui').innerHTML = depenses.map((d,i)=>`<div style="display:flex; justify-content:space-between;"><span>${d.motif} (-${d.montant}F)</span><button onclick="depenses.splice(${i},1);localStorage.setItem('sc_expenses_v18',JSON.stringify(depenses));updateAdminStats()" style="color:red;border:none;background:none;">✕</button></div>`).join('');
}

function calculerPerformance() {
    const now = new Date(), today = new Date(now.setHours(0,0,0,0));
    const rAuj = db.filter(c => new Date(c.time) >= today).reduce((a,b)=>a+b.paye, 0);
    const p = Math.min(Math.round((rAuj/50000)*100), 100);
    document.getElementById('stats-comparative-ui').innerHTML = `<div style="font-size:11px;"><b>AUJOURD'HUI :</b> ${rAuj.toLocaleString()} F / 50 000 F</div><div style="background:#eee; height:8px; border-radius:5px; margin-top:5px; overflow:hidden;"><div style="width:${p}%; background:var(--success); height:100%;"></div></div>`;
}

// --- STOCKS & LOGS ---
function ajouterStock() {
    const n = document.getElementById('stk-nom').value.toUpperCase(), q = parseFloat(document.getElementById('stk-qty').value), u = document.getElementById('stk-unite').value, s = parseFloat(document.getElementById('stk-seuil').value);
    if(!n || isNaN(q)) return;
    const idx = stock.findIndex(p => p.nom === n);
    if(idx !== -1) { stock[idx].qty = q; stock[idx].seuil = s; } else { stock.push({nom:n, qty:q, unite:u, seuil:s}); }
    localStorage.setItem('sc_stock_v18', JSON.stringify(stock)); renderStock();
}

function renderStock() {
    document.getElementById('stk-liste').innerHTML = stock.map((p,i)=>`<div style="display:flex; justify-content:space-between; padding:8px; border-bottom:1px solid #eee; ${p.qty<=p.seuil?'color:red;font-weight:bold;':''}"><span>${p.nom}: ${p.qty.toFixed(1)} ${p.unite}</span><div><button onclick="ajustStk(${i},-1)">-</button><button onclick="ajustStk(${i},1)">+</button></div></div>`).join('');
}
function ajustStk(i, d) { stock[i].qty = Math.max(0, stock[i].qty + d); localStorage.setItem('sc_stock_v18', JSON.stringify(stock)); renderStock(); }

function ajouterDepense() {
    const m = document.getElementById('exp-motif').value, v = parseInt(document.getElementById('exp-montant').value);
    if(m && v) { depenses.push({date: new Date().toISOString().split('T')[0], motif: m, montant: v}); localStorage.setItem('sc_expenses_v18', JSON.stringify(depenses)); updateAdminStats(); }
}

function enregistrerAction(m) {
    journal.unshift({date: new Date().toLocaleTimeString(), user: currentUser ? currentUser.nom : "Système", act: m});
    if(journal.length > 50) journal.pop(); localStorage.setItem('sc_logs_v18', JSON.stringify(journal));
}
function renderJournal() { document.getElementById('journal-ui').innerHTML = journal.map(l => `<div>[${l.date}] <b>${l.user}</b>: ${l.act}</div>`).join(''); }

function genererRapportCloture() {
    const today = new Date().toISOString().split('T')[0];
    const vJ = db.filter(c => new Date(c.time).toISOString().split('T')[0] === today);
    const al = stock.filter(p => p.qty <= p.seuil);
    const enc = vJ.reduce((a,b)=>a+b.paye, 0);
    let msg = `*📊 RAPPORT CLÔTURE SUPER CLEAN PRESSING*\n*Recette Cash:* ${enc} F\n*Commandes:* ${vJ.length}\n`;
    if(al.length > 0) { msg += `\n*⚠️ ALERTES STOCK :*\n`; al.forEach(p => msg += `- ${p.nom} : ${p.qty.toFixed(1)} ${p.unite}\n`); }
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg + "\n_Le bien-être de vos vêtements_")}`);
}

function ajouterPersonnel() {
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-login').value, p = document.getElementById('ns-pass').value, r = document.getElementById('ns-role').value;
    if(n && l) { staff.push({nom:n, login:l, pass:p, role:r}); localStorage.setItem('sc_staff_v18', JSON.stringify(staff)); renderStaffList(); }
}
function renderStaffList() {
    document.getElementById('staff-list-ui').innerHTML = staff.map((s,i) => `<div style="display:flex; justify-content:space-between; padding:5px; border-bottom:1px solid #eee;"><span>${s.nom} (${s.role})</span>${i>0 ? `<button onclick="staff.splice(${i},1);renderStaffList()" style="color:red;border:none;background:none;">✕</button>` : ''}</div>`).join('');
}

function exportData() {
    const d = { db, staff, expenses: depenses, stock, journal, version: "18.0" };
    const b = new Blob([JSON.stringify(d)], { type: "application/json" });
    const a = document.createElement('a'); a.href = URL.createObjectURL(b); a.download = `SUPER_CLEAN_FULL.json`; a.click();
}
function importData() {
    const f = document.getElementById('importFile').files[0]; if(!f) return;
    const r = new FileReader(); r.onload = (e) => {
        const c = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v18', JSON.stringify(c.db)); localStorage.setItem('sc_stock_v18', JSON.stringify(c.stock));
        location.reload();
    }; r.readAsText(f);
}
</script>
</body>
</html>
