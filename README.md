
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v28.0 - LE BIEN-ÊTRE DE VOS VÊTEMENTS</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --radius: 20px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; -webkit-tap-highlight-color: transparent; }
        .hidden { display: none !important; }
        
        /* --- BRANDING STYLE --- */
        .header { background: white; padding: 15px; text-align: center; border-bottom: 3px solid var(--primary); position: sticky; top: 0; z-index: 1000; box-shadow: 0 4px 10px rgba(0,0,0,0.05); }
        .logo-main { width: 60px; height: 60px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 20px; margin: 5px 0 0 0; letter-spacing: -0.5px; }
        .official-slogan { font-size: 11px; color: #555; font-style: italic; font-weight: 600; margin-bottom: 5px; }

        /* --- NAVIGATION --- */
        .nav-bar { display: grid; grid-template-columns: repeat(5, 1fr); gap: 4px; padding: 10px; background: #fff; position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000; }
        .nav-bar button { background: #f8f9fa; border: none; color: #7f8c8d; font-weight: 800; font-size: 7px; padding: 12px 2px; border-radius: 12px; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .nav-bar button.active { background: var(--primary); color: white; transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 86, 179, 0.3); }

        /* --- UI CARDS --- */
        .card { background: white; margin: 12px; padding: 20px; border-radius: var(--radius); box-shadow: 0 4px 15px rgba(0,0,0,0.06); border: 1px solid #eee; position: relative; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 14px; border-radius: 12px; margin-bottom: 10px; width: 100%; box-sizing: border-box; font-size: 14px; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--primary); background: white; }
        
        .btn-main { background: var(--primary); color: white; border: none; padding: 16px; border-radius: 14px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; transition: 0.2s; }
        .btn-wa { background: #25D366; color: white; border: none; padding: 12px; border-radius: 12px; font-size: 11px; font-weight: 700; width: 100%; margin: 4px 0; cursor: pointer; display: flex; align-items: center; justify-content: center; }
        .btn-report { background: #075E54; margin-top: 10px; }

        /* --- STOCKS & STATUS --- */
        .stock-item { display: flex; justify-content: space-between; padding: 12px; border-bottom: 1px solid #eee; font-size: 13px; }
        .low-stock { color: var(--danger); font-weight: 900; animation: blink 1s infinite; }
        @keyframes blink { 50% { opacity: 0.5; } }
        
        .retard-card { border: 2px solid var(--danger) !important; background: #fff5f5 !important; }
        .solde-badge { background: var(--danger); color: white; padding: 4px 10px; border-radius: 8px; font-size: 11px; font-weight: 900; }
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 8px; background: #f8f9fa; border-radius: 8px; margin-bottom: 5px; font-size: 12px; border-left: 3px solid var(--primary); }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div style="background: white; padding: 40px; border-radius: 30px; width: 85%; max-width: 360px; text-align: center; box-shadow: 0 20px 40px rgba(0,0,0,0.2);">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:100px; border-radius:50%; margin-bottom:15px; border: 4px solid #f1f4f8;">
            <h2 style="margin:0; color:var(--dark);">SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:700; margin-bottom:25px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Identifiant">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">ACCÉDER AU SYSTÈME</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 8px 15px; font-size: 10px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 <b id="u-name"></b> (<span id="u-role"></span>)</span>
        <span onclick="location.reload()" style="text-decoration: underline; cursor: pointer;">Déconnexion</span>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
        <div style="font-size:9px; color:#666;">Situé à Nkounga Immeuble de Karche | 675 864 103</div>
    </div>

    <div id="main-content" style="padding-bottom: 110px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3 style="margin-top:0; color:var(--primary);">📥 Réception & Marquage</h3>
                <input type="tel" id="tel-client" placeholder="N° WhatsApp Client" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet du Client">
                <label style="font-size:10px; font-weight:900; color:#777; margin-left:5px;">RETRAIT PRÉVU LE :</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:15px; border-radius:15px; border:2px dashed #ddd;">
                    <select id="select-article" onchange="majPrixSuggere()"></select>
                    <input type="number" id="prix-final" placeholder="Prix Unitaire (F CFA)">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); margin:0;">+ AJOUTER AU TICKET</button>
                </div>
            </div>

            <div id="panier-liste" style="padding: 0 15px;"></div>

            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px;">
                    <span style="opacity:0.8;">NET À PAYER :</span> <b id="total-caisse" style="font-size:26px; color:var(--success);">0 F</b>
                </div>
                <label style="font-size:10px; color:#aaa;">MODE DE RÈGLEMENT :</label>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white;">
                    <option value="Cash">💵 Espèces (Cash)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN MoMo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Montant de l'avance" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning);">Reste du : 0 F</div>
            </div>
            
            <div style="padding: 0 15px;">
                <button onclick="validerCommande()" class="btn-main" style="height:70px; font-size:16px;">🖨️ VALIDER & IMPRIMER</button>
                <button id="btn-report-wa" onclick="envoyerRapportJournalier()" class="btn-wa btn-report">📊 RAPPORT JOURNALIER WHATSAPP</button>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px; font-weight: 800;">👕 ZONE TECHNIQUE - SUIVI</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3 style="margin-top:0;">🧼 GESTION DES CONSOMMABLES</h3>
                <div id="stock-display"></div>
                <div id="admin-stock-control" class="hidden">
                    <hr style="margin:20px 0; border:0; border-top:1px solid #eee;">
                    <h4>📦 Approvisionnement Manuel</h4>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Quantité à ajouter">
                    <button onclick="majStock()" class="btn-main" style="background:var(--warning); color:black;">METTRE À JOUR LE STOCK</button>
                </div>
            </div>
        </div>

        <div id="section-cloud" class="tab-content hidden">
            <div class="card" style="text-align:center;">
                <h3>☁️ SAUVEGARDE DRIVE / ICLOUD</h3>
                <p style="font-size:12px; color:#666;">Exportez votre base de données pour la sécuriser sur vos comptes cloud.</p>
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 EXPORTER LA BASE (.JSON)</button>
                <hr style="margin:30px 0; border:0; border-top:1px solid #eee;">
                <input type="file" id="importFile" accept=".json">
                <button onclick="importData()" class="btn-main" style="background:var(--success);">📥 RESTAURER UNE SAUVEGARDE</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card" style="background:var(--primary); color:white; text-align:center;">
                <small>CHIFFRE D'AFFAIRES GLOBAL</small>
                <div id="admin-stats-ui" style="font-size:28px; font-weight:900;">0 F CFA</div>
            </div>
            <div class="card">
                <h3 style="margin-top:0;">👥 CONTRÔLE DES ACCÈS PERSONNEL</h3>
                <div id="staff-list"></div>
                <hr>
                <h4>Créer un nouveau profil</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Login">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Laveur">Laveur</option>
                    <option value="Repasseur">Repasseur</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER ET ACTIVER</button>
            </div>
        </div>
    </div>

    <nav class="nav-bar" id="main-nav">
        <button onclick="tab('section-caisse')" id="btn-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-atelier">👕 TECHNIQUE</button>
        <button onclick="tab('section-stocks')" id="btn-stocks">🧼 STOCKS</button>
        <button onclick="tab('section-cloud')" id="btn-cloud">☁️ CLOUD</button>
        <button onclick="tab('section-admin')" id="btn-admin">👑 ADMIN</button>
    </nav>
</div>

<div id="ticket-print" style="display:none; text-align:center; font-family:monospace; padding:10px;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:70px; border-radius:50%;"><br>
    <b style="font-size:18px;">SUPER CLEAN PRESSING</b><br>
    "Le bien être de vos vêtements"<br>
    Nkounga, Immeuble de Karche<br>
    Tél: 675 864 103 / 692 315 267<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:12px;"></div>
    --------------------------------<br>
    Merci de votre confiance !
</div>

<script>
// ==========================================
// 1. GRILLE TARIFAIRE INTÉGRALE (ZÉRO SIMPLIFICATION)
// ==========================================
const GRILLE_OFFICIELLE = {
    "LES HAUTS": {"T-Shirt":500,"Chemise":500,"Polo":500,"Pull Over":700,"Blouson":1500,"Démembré":300,"Veste 2P":1500,"Veste 3P":2000,"Pantalon simple":500,"Pantalon jeans":500,"Short":500,"Jupe simple":500,"Jupe plissée":1000},
    "ENSEMBLES": {"Costume simple":2000,"Costume luxe":2500,"Boubou femme":1500,"Boubou 3P":2500,"Jogging":1500,"Pyjama 2P":1000},
    "LES ROBES": {"Robe simple":1000,"Robe soirée":3000,"Kaba court":1000,"Kaba long":1500,"Toge":3500},
    "LINGES DE LIT": {"1 Drap":750,"Ensemble 1 drap+taie":1500,"Ensemble 2 drap+taie":2000,"Couvre lit / Couette":2000,"Housse de couette":2500,"Couverture":4000},
    "VÊTEMENTS ENFANTS": {"Ensemble enfant":1000,"Robe enfant":500,"Haut enfant":500,"Bas enfant":500},
    "AMEUBLEMENT / BAIN": {"Rideau lourd":1500,"Rideau léger":1000,"Petite Serviette":500,"Grande Serviette":1000,"Peignoir":2000},
    "ACCESSOIRES": {"Tennis": 500, "Cravate": 500}
};

let db = JSON.parse(localStorage.getItem('sc_db_v28')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v28')) || [{nom:"Patron", login:"admin", pass:"0000", role:"ADMIN", actif: true}];
let stocks = JSON.parse(localStorage.getItem('sc_stocks_v28')) || [
    {id:'savon', nom:'Savon Liquide', qte:50, unite:'L', mini:5},
    {id:'parfum', nom:'Parfum Textile', qte:10, unite:'L', mini:2},
    {id:'housses', nom:'Housses Plastiques', qte:100, unite:'pcs', mini:10}
];
let panier = [], currentUser = null;

// ==========================================
// 2. AUTHENTIFICATION & SÉCURITÉ
// ==========================================
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return alert("🚫 VOTRE ACCÈS EST BLOQUÉ. VOIR LE PATRON.");
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name').innerText = u.nom;
        document.getElementById('u-role').innerText = u.role;
        filtrerRoles();
        initDropdown();
    } else alert("Identifiants incorrects !");
}

function filtrerRoles() {
    const r = currentUser.role;
    const allowsCaisse = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-report-wa').classList.toggle('hidden', !allowsCaisse);
    document.getElementById('btn-admin').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-cloud').classList.toggle('hidden', r !== "ADMIN");
    if(r === "ADMIN") document.getElementById('admin-stock-control').classList.remove('hidden');
    tab(allowsCaisse ? 'section-caisse' : 'section-atelier');
}

function initDropdown() {
    const s = document.getElementById('select-article');
    s.innerHTML = '<option value="">-- Choisir Prestation --</option>';
    for(let cat in GRILLE_OFFICIELLE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in GRILLE_OFFICIELLE[cat]) {
            let o = document.createElement('option'); o.value = GRILLE_OFFICIELLE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        s.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(s => `<option value="${s.id}">${s.nom}</option>`).join('');
}

// ==========================================
// 3. LOGIQUE CAISSE & STOCKS
// ==========================================
function majPrixSuggere() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }

function ajouterAuPanier() {
    const s = document.getElementById('select-article'), p = document.getElementById('prix-final').value;
    if(!s.value || !p) return;
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p), pret: false });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b)=>a+b.prix, 0);
    document.getElementById('total-caisse').innerText = t + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`
        <div style="display:flex; justify-content:space-between; background:white; padding:10px; border-radius:10px; margin-bottom:5px; border-left:4px solid var(--primary);">
            <b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none; font-weight:bold;">✕</button></span>
        </div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste à percevoir : ${Math.max(0, t - p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, tel = document.getElementById('tel-client').value, dr = document.getElementById('date-retrait').value;
    if(!nom || !tel || panier.length === 0) return alert("Incomplet !");
    
    // Déduction auto stocks (Industriel)
    stocks[0].qte -= (panier.length * 0.05); // Savon
    stocks[1].qte -= (panier.length * 0.01); // Parfum
    stocks[2].qte -= panier.length; // Housses
    localStorage.setItem('sc_stocks_v28', JSON.stringify(stocks));

    const cmd = { id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, mode: document.getElementById('mode-pay').value, retrait: dr, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "En cours" };
    db.unshift(cmd); localStorage.setItem('sc_db_v28', JSON.stringify(db));
    
    document.getElementById('tk-body').innerHTML = `Ticket: #${cmd.id}<br>Client: ${cmd.client.toUpperCase()}<br>Tel: ${cmd.tel}<hr>${cmd.articles.map(a=>`${a.nom}: ${a.prix}F`).join('<br>')}<hr>TOTAL: ${cmd.total}F<br>PAYÉ: ${cmd.paye}F (${cmd.mode})<br><b>SOLDE: ${cmd.total-cmd.paye}F</b><br>RDV: ${cmd.retrait}`;
    window.print(); location.reload();
}

// ==========================================
// 4. ZONE TECHNIQUE & WHATSAPP
// ==========================================
function renderAtelier() {
    const today = new Date().toISOString().split('T')[0];
    const isBoss = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card ${c.retrait < today ? 'retard-card' : ''}">
            <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b><small>${c.retrait}</small></div>
            <div class="solde-badge">À PERCEVOIR : ${c.total-c.paye} F</div><br>
            ${c.articles.map(a => `
                <div class="item-row">
                    <span>${a.nom}</span>
                    ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:4px 8px; border-radius:5px;">OK</button>` : `<span style="font-weight:700; color:green;">📍 ${a.loc || 'PRÊT'}</span>`}
                </div>`).join('')}
            ${isBoss ? `<button class="btn-wa" onclick="window.open('https://wa.me/237${c.tel}?text=Bonjour%20votre%20commande%20SUPER%20CLEAN%20est%20prete')">📲 WHATSAPP : DISPONIBLE</button>` : ''}
            <button onclick="livrer(${c.id})" class="btn-main" style="margin-top:10px; background:var(--dark);">LIVRER ET FERMER LE TICKET</button>
        </div>`).join('');
}

function finirArt(cid, auid) {
    const loc = prompt("Emplacement (Casier) ?");
    const c = db.find(x => x.id === cid), a = c.articles.find(x => x.uid === auid);
    a.pret = true; a.loc = loc;
    localStorage.setItem('sc_db_v28', JSON.stringify(db)); renderAtelier();
}

function livrer(id) {
    if(confirm("Confirmer la livraison et l'encaissement du solde ?")) {
        const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
        localStorage.setItem('sc_db_v28', JSON.stringify(db)); renderAtelier();
    }
}

// ==========================================
// 5. RAPPORT JOURNALIER (WA)
// ==========================================
function envoyerRapportJournalier() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    let cash=0, om=0, momo=0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    const msg = `📊 *RAPPORT JOURNALIER SUPER CLEAN*\n📅 Date : ${new Date().toLocaleDateString()}\n👤 Agent : ${currentUser.nom}\n------------------\n💵 Cash : ${cash}F\n🍊 Orange Money : ${om}F\n🟡 MTN Momo : ${momo}F\n💰 *TOTAL : ${cash+om+momo}F*`;
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
}

// ==========================================
// 6. ADMIN & STOCKS
// ==========================================
function renderAdmin() {
    document.getElementById('admin-stats-ui').innerText = db.reduce((a,b)=>a+b.paye,0) + " F CFA";
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `
        <div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;">
            <span><b>${s.nom}</b> (${s.role}) - ${s.actif?'Actif':'Bloqué'}</span>
            ${i!==0?`<button onclick="basculer(${i})" style="font-size:10px; padding:5px; border-radius:5px;">BASCULER</button>`:''}
        </div>`).join('');
}

function renderStocks() {
    document.getElementById('stock-display').innerHTML = stocks.map(s => `
        <div class="stock-item">
            <span>${s.nom}</span> 
            <b class="${s.qte < s.mini ? 'low-stock':''}">${s.qte.toFixed(2)} ${s.unite}</b>
        </div>`).join('');
}

function majStock() {
    const id = document.getElementById('upd-stock-item').value, q = parseFloat(document.getElementById('upd-stock-qte').value);
    stocks.find(s => s.id === id).qte += q; localStorage.setItem('sc_stocks_v28', JSON.stringify(stocks)); renderStocks();
}

// ==========================================
// 7. UTILS & NAVIGATION
// ==========================================
function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]);
    if(btn) btn.classList.add('active');
    if(id==='section-atelier') renderAtelier();
    if(id==='section-stocks') renderStocks();
    if(id==='section-admin') renderAdmin();
}

function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif: true}); localStorage.setItem('sc_staff_v28', JSON.stringify(staff)); renderAdmin(); }
}

function basculer(i) { staff[i].actif = !staff[i].actif; localStorage.setItem('sc_staff_v28', JSON.stringify(staff)); renderAdmin(); }
function rechercherClient() { const t = document.getElementById('tel-client').value; if(t.length>=8){ const f=db.find(x=>x.tel.includes(t)); if(f) document.getElementById('client-nom').value=f.client; } }

function exportData() {
    const blob = new Blob([JSON.stringify({db, staff, stocks})], {type:"application/json"});
    const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download=`SuperClean_Backup.json`; a.click();
}

function importData() {
    const reader = new FileReader();
    reader.onload = (e) => {
        const data = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v28', JSON.stringify(data.db));
        localStorage.setItem('sc_staff_v28', JSON.stringify(data.staff));
        localStorage.setItem('sc_stocks_v28', JSON.stringify(data.stocks));
        location.reload();
    };
    reader.readAsText(document.getElementById('importFile').files[0]);
}
</script>
</body>
</html>
