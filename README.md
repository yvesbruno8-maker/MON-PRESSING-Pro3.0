<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v55.0 - TEMPS RÉEL TOTAL</title>
    
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --white: #ffffff;
            --radius: 20px; --shadow: 0 10px 30px rgba(0,0,0,0.1);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; -webkit-tap-highlight-color: transparent; }
        .hidden { display: none !important; }
        
        /* --- UI DESIGN --- */
        .header { background: var(--white); padding: 20px; text-align: center; border-bottom: 4px solid var(--primary); position: sticky; top: 0; z-index: 1000; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        .logo-main { width: 85px; height: 85px; border-radius: 50%; border: 3px solid var(--primary); object-fit: cover; }
        .brand-name { font-weight: 900; color: var(--primary); font-size: 24px; margin-top: 10px; text-transform: uppercase; }
        .official-slogan { font-size: 13px; color: #555; font-style: italic; font-weight: 700; }

        .nav-bar { display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; padding: 12px; background: var(--white); position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000; }
        .nav-bar button { background: #f1f4f8; border: none; color: #7f8c8d; font-weight: 800; font-size: 7px; padding: 12px 1px; border-radius: 12px; cursor: pointer; text-transform: uppercase; }
        .nav-bar button.active { background: var(--primary); color: var(--white); transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0, 86, 179, 0.4); }

        .card { background: var(--white); margin: 15px; padding: 25px; border-radius: var(--radius); box-shadow: var(--shadow); border: 1px solid #eee; }
        h3 { margin-top: 0; font-size: 16px; color: var(--primary); font-weight: 900; text-transform: uppercase; border-bottom: 3px solid var(--light); padding-bottom: 12px; margin-bottom: 20px; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 16px; border-radius: 15px; margin-bottom: 12px; width: 100%; box-sizing: border-box; font-size: 15px; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--primary); background: var(--white); }
        .btn-main { background: var(--primary); color: var(--white); border: none; padding: 20px; border-radius: 18px; font-weight: 900; width: 100%; cursor: pointer; text-transform: uppercase; }
        
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 15px; margin-bottom: 8px; border-left: 6px solid var(--primary); font-size: 14px; }
        .solde-badge { background: var(--danger); color: white; padding: 6px 12px; border-radius: 10px; font-size: 12px; font-weight: 900; display: inline-block; }
        .status-pill { padding: 4px 10px; border-radius: 50px; font-size: 10px; font-weight: 900; text-transform: uppercase; }
        .pill-active { background: #dcfce7; color: #166534; }
        .pill-blocked { background: #fee2e2; color: #991b1b; }

        .sync-tag { position: fixed; top: 10px; right: 10px; font-size: 9px; padding: 5px 10px; border-radius: 10px; z-index: 3000; font-weight: bold; }
        .online { background: var(--success); color: white; }
        .offline { background: var(--danger); color: white; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="sync-status" class="sync-tag offline">SYNC : OFF</div>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary) 0%, #00b4d8 100%);">
        <div class="card" style="width: 90%; max-width: 400px; text-align: center; padding: 50px;">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-main">
            <h2>SUPER CLEAN</h2>
            <p style="font-size:12px; color:var(--primary); font-weight:800; margin-bottom:35px; text-transform:uppercase;">Le bien être de vos vêtements</p>
            <input type="text" id="u-login" placeholder="Identifiant">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">CONNEXION</button>
            <div id="login-error" style="color:var(--danger); font-size:11px; margin-top:15px; font-weight:bold;"></div>
            <p onclick="resetConfig()" style="font-size:9px; color:#aaa; margin-top:25px; cursor:pointer;">Réinitialiser URL Cloud</p>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 10px 20px; font-size: 11px; display: flex; justify-content: space-between; align-items: center;">
        <span>👤 SESSION : <b id="u-name-top"></b> (<span id="u-role-top"></span>)</span>
        <button onclick="location.reload()" style="background:var(--danger); border:none; color:white; padding:6px 12px; border-radius:8px; font-size:10px; cursor:pointer;">SORTIR</button>
    </div>

    <div class="header">
        <div class="brand-name">SUPER CLEAN PRESSING</div>
        <div class="official-slogan">Le bien être de vos vêtements</div>
    </div>

    <div id="main-content" style="padding-bottom: 130px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION & ENCAISSEMENT</h3>
                <input type="tel" id="tel-client" placeholder="WhatsApp Client" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom Complet">
                <input type="date" id="date-retrait">
                <div style="background:#f8f9fa; padding:20px; border-radius:18px; border:1px dashed #ddd; margin-top:10px;">
                    <select id="select-article" onchange="majPrixInitial()"></select>
                    <input type="number" id="prix-final" placeholder="Prix ajusté">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark);">+ AJOUTER</button>
                </div>
            </div>
            <div id="panier-liste" style="padding: 0 15px;"></div>
            <div class="card" style="background:var(--dark); color:white;">
                TOTAL : <b id="total-caisse" style="font-size:26px; color:var(--success);">0 F</b>
                <input type="number" id="paye-caisse" placeholder="Avance" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white; margin-top:10px;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning);">Reste : 0 F</div>
            </div>
            <div style="padding: 0 15px;"><button onclick="validerCommande()" class="btn-main" style="height:80px;">🖨️ VALIDER TICKET</button></div>
            
            <div class="card" style="border: 2px solid var(--danger);">
                <h3>💸 DÉPENSE</h3>
                <input type="text" id="achat-motif" placeholder="Motif">
                <input type="number" id="achat-montant" placeholder="Montant">
                <button onclick="enregistrerAchat()" class="btn-main" style="background:var(--danger);">DÉDUIRE</button>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 20px;">👕 ZONE TECHNIQUE - PRODUCTION</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-rapport" class="tab-content hidden">
            <div class="card">
                <h3>📊 BILAN JOURNALIER</h3>
                <div id="rapport-details"></div>
                <button onclick="envoyerRapportWA()" class="btn-main" style="background:#25D366; margin-top:20px;">📲 ENVOYER AU PATRON</button>
            </div>
        </div>

        <div id="section-performance" class="tab-content hidden">
            <div class="card">
                <h3>📈 ANALYSE PERF</h3>
                <div id="perf-display"></div>
                <div class="item-row" style="background:var(--success); color:white;"><span>NET GLOBAL</span> <b id="p-net">0 F</b></div>
            </div>
        </div>

        <div id="section-stocks" class="tab-content hidden">
            <div class="card">
                <h3>🧼 INVENTAIRE DES PRODUITS</h3>
                <div id="stock-display"></div>
                <div id="admin-stock-control" class="hidden">
                    <hr>
                    <select id="upd-stock-item"></select>
                    <input type="number" id="upd-stock-qte" placeholder="Quantité">
                    <button onclick="majStockManuel()" class="btn-main" style="background:var(--warning); color:black;">AJUSTER</button>
                </div>
            </div>
        </div>

        <div id="section-logs" class="tab-content hidden">
            <div class="card">
                <h3>📜 LOGS AUDIT</h3>
                <div id="logs-container" style="max-height:450px; overflow-y:auto; font-size:10px; font-family:monospace;"></div>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card" style="background: #fff3cd; border: 1px solid #ffeeba;">
                <h3>⚙️ CONFIGURATION CLOUD</h3>
                <input type="text" id="fb-url-config" placeholder="https://votre-projet.firebaseio.com">
                <button onclick="saveFirebaseURL()" class="btn-main" style="background:var(--dark);">DÉPLOYER LA SYNCHRO</button>
            </div>
            <div class="card" style="border: 2px solid var(--primary);">
                <h3>👑 PERSONNEL & ACCÈS</h3>
                <div id="staff-list"></div>
                <hr>
                <h4>CRÉER UN COMPTE</h4>
                <input type="text" id="ns-nom" placeholder="Nom Complet">
                <input type="text" id="ns-log" placeholder="Login">
                <input type="password" id="ns-pass" placeholder="Mot de passe">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Zone Technique">Zone Technique</option>
                    <option value="ADMIN">ADMINISTRATEUR</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER ET ACTIVER</button>
            </div>
            <div class="card">
                <h3>✨ AJOUTER ARTICLE GRILLE</h3>
                <input type="text" id="new-art-cat" placeholder="Catégorie">
                <input type="text" id="new-art-nom" placeholder="Désignation">
                <input type="number" id="new-art-prix" placeholder="Prix standard">
                <button onclick="ajouterNouveauProduit()" class="btn-main">ACTUALISER LA LISTE</button>
            </div>
        </div>
    </div>

    <nav class="nav-bar" id="main-nav">
        <button onclick="tab('section-caisse')" id="btn-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-atelier">👕 Z.T</button>
        <button onclick="tab('section-rapport')" id="btn-rapport">📊 RAPPORT</button>
        <button onclick="tab('section-performance')" id="btn-performance">📈 PERF</button>
        <button onclick="tab('section-stocks')" id="btn-stocks">🧼 STOCKS</button>
        <button onclick="tab('section-logs')" id="btn-logs">📜 LOGS</button>
        <button onclick="tab('section-admin')" id="btn-admin">👑 ADMIN</button>
    </nav>
</div>

<script>
/* ==========================================
   1. GRILLE TARIFAIRE INTÉGRALE
   ========================================== */
const GRILLE_OFFICIELLE = {
    "LES HAUTS": { "T-Shirt": 500, "Chemise": 500, "Polo": 500, "Pull Over": 700, "Blouson": 1500, "Démembré": 300, "Veste 2 pièces": 1500, "Veste 3 pièces": 2000, "Pantalon jeans": 500, "Jupe plissée": 1000 },
    "ENSEMBLES": { "Costume luxe": 2500, "Boubou femme": 1500, "Boubou homme 3P": 2500, "Jogging": 1500, "Pyjama 2P": 1000 },
    "LES ROBES": { "Robe simple": 1000, "Robe de soirée": 3000, "Kaba court": 1000, "Kaba long": 1500, "Toge": 3500 },
    "LINGES DE LIT": { "1 Drap": 750, "Ensemble Draps": 2000, "Couverture": 4000, "Couette": 2000 },
    "VÊTEMENTS ENFANTS": { "Ensemble enfant": 1000, "Robe enfant": 500 },
    "AMEUBLEMENT / BAIN": { "Rideau lourd": 1500, "Serviette": 1000, "Peignoir": 2000 },
    "ACCESSOIRES": { "Tennis": 500, "Cravate": 500 }
};

/* ==========================================
   2. CONFIGURATION & MOTEUR TEMPS RÉEL
   ========================================== */
const MASTER_ADMIN = {nom: "Directeur", login: "admin", pass: "0000", role: "ADMIN", actif: true};
let CLOUD_URL = localStorage.getItem('sc_fb_url_v55') || "";
let GRILLE = GRILLE_OFFICIELLE;
let dbRef = null;

let orders = [], staff = [MASTER_ADMIN], stocks = [{id:'savon', nom:'Savon', qte:100, unite:'L'}, {id:'housses', nom:'Housses', qte:500, unite:'pcs'}], logs = [], achats = [];
let currentUser = null, panier = [];

// Connexion SDK Firebase avec Ecouteur Temps Réel
if(CLOUD_URL) {
    try {
        firebase.initializeApp({ databaseURL: CLOUD_URL });
        dbRef = firebase.database();
        document.getElementById('sync-status').className = "sync-tag online";
        document.getElementById('sync-status').innerText = "SYNCHRO : LIVE";
        
        // ECOUTEUR TEMPS REEL : Chaque changement sur le Cloud met à jour tous les téléphones
        dbRef.ref('/').on('value', (snap) => {
            const data = snap.val();
            if(data) {
                orders = data.orders || [];
                staff = data.staff || [MASTER_ADMIN];
                stocks = data.stocks || stocks;
                logs = data.logs || [];
                achats = data.achats || [];
                if(data.GRILLE) GRILLE = data.GRILLE;
                
                // Rafraîchir l'écran actuel si l'utilisateur est connecté
                if(currentUser) {
                    renderAtelier();
                    renderStocks();
                    renderAdmin();
                }
            }
        });
    } catch(e) { console.error("Firebase Init Fail"); }
}

/* ==========================================
   3. AUTHENTIFICATION & SÉCURITÉ
   ========================================== */
async function login() {
    const errorEl = document.getElementById('login-error');
    errorEl.innerText = "Connexion...";
    
    const l = document.getElementById('u-login').value;
    const p = document.getElementById('u-pass').value;
    
    // Vérification Maître d'abord
    let u = (l === "admin" && p === "0000") ? MASTER_ADMIN : staff.find(x => x.login === l && x.pass === p);
    
    if(u) {
        if(!u.actif && u.role !== "ADMIN") return errorEl.innerText = "COMPTE BLOQUÉ !";
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name-top').innerText = u.nom;
        document.getElementById('u-role-top').innerText = u.role;
        document.getElementById('fb-url-config').value = CLOUD_URL;
        
        filtrerRoles(); initDropdown(); renderStocks();
    } else errorEl.innerText = "Identifiants incorrects.";
}

function filtrerRoles() {
    const r = currentUser.role;
    const isBoss = (r === "ADMIN"), isCaisse = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').classList.toggle('hidden', !isCaisse);
    document.getElementById('btn-rapport').classList.toggle('hidden', !isCaisse);
    document.getElementById('btn-performance').classList.toggle('hidden', !isBoss);
    document.getElementById('btn-logs').classList.toggle('hidden', !isBoss);
    document.getElementById('btn-admin').classList.toggle('hidden', !isBoss);
    if(isBoss) document.getElementById('admin-stock-control').classList.remove('hidden');
    tab(isCaisse ? 'section-caisse' : 'section-atelier');
}

/* ==========================================
   4. CAISSE & TECHNIQUE
   ========================================== */
function initDropdown() {
    const s = document.getElementById('select-article'); s.innerHTML = '<option value="">-- Article --</option>';
    for(let c in GRILLE) {
        let g = document.createElement('optgroup'); g.label = c;
        for(let a in GRILLE[c]) { let o = document.createElement('option'); o.value = GRILLE[c][a]; o.innerText = a; g.appendChild(o); }
        s.appendChild(g);
    }
    document.getElementById('upd-stock-item').innerHTML = stocks.map(s => `<option value="${s.id}">${s.nom}</option>`).join('');
}

function ajouterAuPanier() {
    const s = document.getElementById('select-article'), p = document.getElementById('prix-final').value;
    if(!s.value || !p) return;
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p), pret: false });
    renderPanier();
}
function renderPanier() {
    const t = panier.reduce((a,b)=>a+b.prix, 0);
    document.getElementById('total-caisse').innerText = t + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`<div class="item-row"><b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()">✕</button></span></div>`).join('');
    calculerReste();
}
function calculerReste() { const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0; document.getElementById('reste-caisse').innerText = `Reste : ${t - p} F`; }

async function validerCommande() {
    const nom = document.getElementById('client-nom').value;
    if(!nom || panier.length === 0) return alert("Incomplet !");
    const cmd = { id: Math.floor(1000+Math.random()*8999), client: nom, tel: document.getElementById('tel-client').value, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, mode: document.getElementById('mode-pay').value, retrait: document.getElementById('date-retrait').value, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "En cours" };
    orders.unshift(cmd);
    if(dbRef) await dbRef.ref('orders').set(orders);
    location.reload();
}

function renderAtelier() {
    const canSeeMoney = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    document.getElementById('atelier-liste').innerHTML = orders.filter(c => c.statut !== "Livré").map(c => `
        <div class="card">
            <b>TICKET #${c.id} - ${c.client}</b><br><small>RDV: ${c.retrait}</small><br>
            ${canSeeMoney ? `<div class="solde-badge" style="margin-top:5px;">À PAYER: ${c.total-c.paye} F</div>` : ''}
            <div style="margin-top:10px;">
                ${c.articles.map(a => `<div class="item-row"><span>${a.nom}</span> ${!a.pret ? `<button onclick="finirArt(${c.id}, ${a.uid})" style="background:var(--success); color:white; border:none; padding:5px 12px; border-radius:8px;">OK</button>` : `<b style="color:green;">PRÊT</b>`}</div>`).join('')}
            </div>
            ${canSeeMoney ? `<button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark); margin-top:10px;">LIVRER</button>` : ''}
        </div>`).join('');
}

async function finirArt(cid, auid) {
    const c = orders.find(x => x.id === cid); c.articles.find(a => a.uid === auid).pret = true;
    if(dbRef) await dbRef.ref('orders').set(orders);
}

async function livrer(id) {
    const c = orders.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
    if(dbRef) await dbRef.ref('orders').set(orders);
}

/* ==========================================
   5. ADMIN & SYSTEM
   ========================================== */
function renderStocks() { document.getElementById('stock-display').innerHTML = stocks.map(s => `<div class="item-row"><span>${s.nom}</span> <b>${s.qte} ${s.unite}</b></div>`).join(''); }
async function majStockManuel() { const i=document.getElementById('upd-stock-item').value, q=parseFloat(document.getElementById('upd-stock-qte').value); stocks.find(x=>x.id===i).qte+=q; if(dbRef) await dbRef.ref('stocks').set(stocks); }

function renderAdmin() {
    document.getElementById('staff-list').innerHTML = staff.map((s,i) => `<div class="item-row"><span><b>${s.nom}</b> (${s.role}) <span class="status-pill ${s.actif?'pill-active':'pill-blocked'}">${s.actif?'Actif':'Bloqué'}</span></span> ${i!==0?`<button onclick="toggleUser(${i})">BASCULER</button>`:''}</div>`).join('');
}
async function toggleUser(i) { staff[i].actif = !staff[i].actif; if(dbRef) await dbRef.ref('staff').set(staff); }

async function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r, actif:true}); if(dbRef) await dbRef.ref('staff').set(staff); }
}

async function ajouterNouveauProduit() {
    const c=document.getElementById('new-art-cat').value.toUpperCase(), n=document.getElementById('new-art-nom').value, p=parseInt(document.getElementById('new-art-prix').value);
    if(c&&n&&p) { if(!GRILLE[c]) GRILLE[c]={}; GRILLE[c][n]=p; if(dbRef) await dbRef.ref('GRILLE').set(GRILLE); }
}

function saveFirebaseURL() { localStorage.setItem('sc_fb_url_v55', document.getElementById('fb-url-config').value); location.reload(); }
function resetConfig() { localStorage.removeItem('sc_fb_url_v55'); location.reload(); }

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]); if(btn) btn.classList.add('active');
}
function majPrixInitial() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }
function rechercherClient() { const t=document.getElementById('tel-client').value; if(t.length>=8){ const f=orders.find(x=>x.tel&&x.tel.includes(t)); if(f) document.getElementById('client-nom').value=f.client; } }
</script>
</body>
</html>
