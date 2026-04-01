<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v16.5 - ULTIMATE</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --radius: 20px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; padding-bottom: 80px; }
        .hidden { display: none !important; }
        
        /* --- UI COMPONENTS --- */
        .header { background: white; padding: 15px; text-align: center; border-bottom: 3px solid var(--primary); position: sticky; top: 0; z-index: 1000; }
        .logo-small { width: 45px; border-radius: 50%; vertical-align: middle; }
        .card { background: white; margin: 12px; padding: 18px; border-radius: var(--radius); box-shadow: 0 4px 15px rgba(0,0,0,0.06); border: 1px solid #eee; position: relative; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 14px; border-radius: 12px; margin-bottom: 10px; width: 100%; box-sizing: border-box; font-size: 14px; outline: none; }
        input:focus { border-color: var(--primary); background: white; }
        .btn-main { background: var(--primary); color: white; border: none; padding: 16px; border-radius: 14px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; transition: 0.2s; }
        .btn-main:active { transform: scale(0.98); opacity: 0.9; }
        
        /* --- NAVIGATION --- */
        .nav-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 4px; padding: 8px; background: #fff; position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 2000;}
        .nav-bar button { background: #f8f9fa; border: none; color: #7f8c8d; font-weight: 800; font-size: 9px; padding: 10px 2px; border-radius: 12px; cursor: pointer; }
        .nav-bar button.active { background: var(--primary); color: white; }

        /* --- SPECIAL STYLES --- */
        .badge-solde { background: var(--danger); color: white; padding: 4px 8px; border-radius: 6px; font-size: 10px; font-weight: 900; }
        .retard-tag { position: absolute; top: -10px; right: 10px; background: var(--danger); color: white; font-size: 10px; padding: 2px 10px; border-radius: 20px; font-weight: 900; }
        
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(239, 35, 60, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(239, 35, 60, 0); }
            100% { box-shadow: 0 0 0 0 rgba(239, 35, 60, 0); }
        }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height:100vh; display:flex; align-items:center; justify-content:center; background:linear-gradient(135deg, var(--primary), #00b4d8);">
        <div style="background:white; padding:40px; border-radius:30px; width:85%; max-width:340px; text-align:center; box-shadow: 0 20px 40px rgba(0,0,0,0.2);">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:100px; border-radius:50%; margin-bottom:15px;">
            <h2 style="margin:0; color:var(--dark);">SUPER CLEAN</h2>
            <p style="font-size:11px; color:var(--primary); font-weight:700; margin-bottom:25px;">L'EXCELLENCE AU SERVICE DE VOTRE IMAGE</p>
            <input type="text" id="u-login" placeholder="Identifiant">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">OUVRIR LA SESSION</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-small">
        <span style="font-weight:900; color:var(--primary); margin-left:10px;">SUPER CLEAN PRESSING</span>
    </div>

    <div id="section-caisse" class="tab-content">
        <div class="card">
            <h3>📝 NOUVELLE RÉCEPTION</h3>
            <input type="tel" id="tel-client" placeholder="WhatsApp Client" onkeyup="rechercherClient()">
            <input type="text" id="client-nom" placeholder="Nom du Client">
            <label style="font-size:10px; font-weight:900; color:#777;">DATE DE RETRAIT PRÉVUE</label>
            <input type="date" id="date-retrait">
            
            <div style="background:#f8f9fa; padding:12px; border-radius:15px; border:1px dashed #ddd;">
                <select id="select-article" onchange="majPrixSuggere()"></select>
                <input type="number" id="prix-final" placeholder="Prix final">
                <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark);">+ AJOUTER L'ARTICLE</button>
            </div>
        </div>

        <div id="panier-liste" style="padding:0 12px;"></div>

        <div class="card" style="background:var(--dark); color:white;">
            <button onclick="appliquerFidelite()" style="background:#f1c40f; color:#000; border:none; padding:8px; border-radius:8px; font-size:10px; font-weight:bold; width:100%; margin-bottom:10px;">🌟 APPLIQUER REMISE FIDÉLITÉ (-500 F)</button>
            <div style="display:flex; justify-content:space-between; margin-bottom:10px;">
                <span style="opacity:0.8;">NET À PAYER :</span> <b id="total-caisse" style="font-size:22px; color:var(--success);">0 F</b>
            </div>
            <input type="number" id="paye-caisse" placeholder="Avance reçue" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
            <div id="reste-caisse" style="text-align:right; font-weight:800; color:var(--warning);">Reste : 0 F</div>
        </div>
        <div style="padding:0 12px;"><button onclick="validerCommande()" class="btn-main">VALIDER & IMPRIMER TICKET</button></div>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div class="card"><input type="text" id="search-atelier" placeholder="🔍 Rechercher un nom ou N°..." onkeyup="filtrerAtelier()"></div>
        <div id="atelier-liste"></div>
    </div>

    <div id="section-admin" class="tab-content hidden">
        <div class="card" style="text-align:center;">
            <div style="display:flex; justify-content:center; align-items:center; gap:10px;">
                <h3 style="margin:0;">🎯 OBJECTIF DU JOUR</h3>
                <button onclick="modifierObjectif()" style="border:none; background:none; cursor:pointer; font-size:18px;">⚙️</button>
            </div>
            <div style="background: #eee; border-radius: 20px; height: 25px; width: 100%; position: relative; overflow: hidden; margin-top:10px;">
                <div id="progress-bar" style="background: var(--success); width: 0%; height: 100%; transition: width 0.5s;"></div>
                <span id="progress-text" style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); font-size: 11px; font-weight: 900;">0%</span>
            </div>
            <p id="objectif-status" style="font-size: 10px; margin-top: 5px; font-weight: 600;"></p>
        </div>

        <div class="card" style="background:var(--primary); color:white; text-align:center;">
            <div id="admin-stats-ui" style="font-weight:900;">0 F CFA</div>
            <button onclick="genererRapportCloture()" style="background:#25D366; color:white; border:none; padding:10px; border-radius:10px; margin-top:10px; width:100%; font-weight:bold;">📤 RAPPORT DE CLÔTURE (WA)</button>
        </div>

        <div class="card">
            <h3>💸 DÉPENSES</h3>
            <input type="text" id="exp-motif" placeholder="Motif">
            <input type="number" id="exp-montant" placeholder="Montant">
            <button onclick="ajouterDepense()" class="btn-main" style="background:var(--danger);">VALIDER DÉPENSE</button>
            <div id="liste-depenses-ui" style="margin-top:15px; font-size:12px;"></div>
        </div>

        <div class="card">
            <h3>📦 STOCKS</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:5px;">
                <input type="text" id="stk-nom" placeholder="Produit">
                <input type="number" id="stk-qty" placeholder="Quantité">
                <input type="text" id="stk-unite" placeholder="L, Kg, Pcs">
                <input type="number" id="stk-seuil" placeholder="Alerte">
            </div>
            <button onclick="ajouterStock()" class="btn-main" style="font-size:12px;">MAJ STOCK</button>
            <div id="stk-liste" style="margin-top:10px;"></div>
        </div>

        <div class="card" style="text-align:center; background:#f4f7f9;">
            <h3>☁️ CLOUD BACKUP</h3>
            <button onclick="exportData()" class="btn-main" style="background:#6c5ce7; margin-bottom:10px;">📤 SAUVEGARDER TOUT</button>
            <input type="file" id="importFile" accept=".json" style="font-size:10px;">
            <button onclick="importData()" class="btn-main" style="background:var(--success);">📥 RESTAURER</button>
        </div>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-caisse')" id="btn-caisse" class="active">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-atelier">👕 ATELIER</button>
        <button onclick="tab('section-admin')" id="btn-admin">👑 ADMIN</button>
        <button onclick="location.reload()" style="color:var(--danger)">🔴 QUITTER</button>
    </nav>
</div>

<div id="ticket-print" style="display:none; text-align:center; font-family:monospace; padding:10px;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:60px; border-radius:50%;"><br>
    <b style="font-size:16px;">SUPER CLEAN PRESSING</b><br>
    Nkounga, Immeuble Karche<br>
    Tél: 675 864 103 / 692 315 267<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:12px;"></div>
    --------------------------------<br>
    "L'excellence au service de votre image"<br>
    Merci de votre confiance !
</div>

<script>
// --- DONNÉES ---
const TARIFAIRE = {
    "HAUTS": {"Chemise": 500, "Veste": 1500, "Costume 2P": 2000, "Veste 3P": 2500, "T-Shirt": 500, "Pull": 700, "Blouson": 1500},
    "BAS": {"Pantalon Jeans": 500, "Pantalon simple": 500, "Short": 500, "Jupe simple": 500, "Jupe plissée": 1000},
    "LIT & MAISON": {"1 Drap": 750, "Ensemble Draps": 2000, "Couette": 2000, "Housse couette": 2500, "Rideau": 1500, "Couverture": 4000},
    "ENFANTS": {"Ensemble enfant": 1000, "Robe enfant": 500, "Haut enfant": 500},
    "ACCESSOIRES": {"Tennis": 500, "Cravate": 500, "Peignoir": 2000, "Serviette": 1000}
};

let db = JSON.parse(localStorage.getItem('sc_db_v16')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v16')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let depenses = JSON.parse(localStorage.getItem('sc_expenses_v16')) || [];
let stock = JSON.parse(localStorage.getItem('sc_stock_v16')) || [];
let objectifJournalier = parseInt(localStorage.getItem('sc_objectif_v16')) || 50000;
let panier = [], currentUser = null;

// --- LOGIN ---
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        initApp();
    } else alert("Accès refusé !");
}

function initApp() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Article --</option>';
    for(let cat in TARIFAIRE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in TARIFAIRE[cat]) {
            let o = document.createElement('option'); o.value = TARIFAIRE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        sel.appendChild(g);
    }
}

// --- LOGIQUE CAISSE ---
function majPrixSuggere() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length >= 8) {
        const histo = db.filter(x => x.tel && x.tel.includes(t));
        if(histo.length > 0) {
            document.getElementById('client-nom').value = histo[0].client;
            const totalArt = histo.reduce((a,b) => a + b.articles.length, 0);
            if(totalArt % 10 === 0 && totalArt > 0) alert("🌟 FIDÉLITÉ : Ce client a droit à une remise !");
        }
    }
}

function ajouterAuPanier() {
    const s = document.getElementById('select-article'), p = document.getElementById('prix-final').value;
    if(!s.value || !p) return;
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p) });
    renderPanier();
}

function appliquerFidelite() {
    panier.push({ uid: Date.now(), nom: "🎁 REMISE FIDÉLITÉ", prix: -500 });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b)=>a+b.prix, 0);
    document.getElementById('total-caisse').innerText = t + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`
        <div style="display:flex; justify-content:space-between; background:white; padding:10px; border-radius:10px; margin-bottom:5px; font-size:13px; border-left:4px solid var(--primary);">
            <b>${x.nom}</b> <span>${x.prix}F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none;">✕</button></span>
        </div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${Math.max(0, t - p)} F CFA`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, tel = document.getElementById('tel-client').value, dr = document.getElementById('date-retrait').value;
    if(!nom || !tel || !dr || panier.length === 0) return alert("Complétez les infos !");
    
    const cmd = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        retrait: dr, caissier: currentUser.nom, time: new Date().getTime(), statut: "Atelier"
    };

    db.unshift(cmd); localStorage.setItem('sc_db_v16', JSON.stringify(db));
    
    document.getElementById('tk-body').innerHTML = `TICKET #${cmd.id}<br>CLIENT: ${cmd.client.toUpperCase()}<br>TEL: ${cmd.tel}<br>--------------------------------<br>${cmd.articles.map(a => `${a.nom}: ${a.prix}F`).join('<br>')}<br>--------------------------------<br>TOTAL: ${cmd.total} F<br>PAYÉ: ${cmd.paye} F<br>SOLDE: ${cmd.total - cmd.paye} F<br>RDV RETRAIT: ${cmd.retrait}`;
    window.print(); location.reload();
}

// --- ATELIER ---
function renderAtelier() {
    const today = new Date(); today.setHours(0,0,0,0);
    const list = document.getElementById('atelier-liste');
    list.innerHTML = db.filter(c => c.statut !== "Livré").map(c => {
        const dR = new Date(c.retrait);
        const diff = Math.floor((today - dR) / (1000*60*60*24));
        let style = "", tag = "";
        if(diff > 0) {
            style = diff >= 3 ? "border:3px solid #800000; animation:pulse 2s infinite;" : "border:2px solid var(--danger);";
            tag = `<span class="retard-tag">${diff >= 3 ? '🚨 CRITIQUE' : '⚠️ RETARD'} ${diff}J</span>`;
        }
        return `<div class="card" style="${style}">${tag}<b>#${c.id} - ${c.client}</b><br><small>RDV: ${c.retrait}</small><br><span class="badge-solde">Solde: ${c.total-c.paye}F</span><br><div style="display:grid; grid-template-columns:1fr 1fr; gap:5px; margin-top:10px;"><button class="btn-main" style="background:#25D366; font-size:10px; padding:10px;" onclick="envoyerWA('${c.tel}', '${c.client}', '${c.id}')">📲 RAPPEL</button><button onclick="livrer(${c.id})" class="btn-main" style="background:var(--dark); font-size:10px; padding:10px;">LIVRER</button></div></div>`;
    }).join('') || "<p style='text-align:center;'>Atelier vide.</p>";
}

function livrer(id) {
    const c = db.find(x => x.id === id);
    if(confirm(`Livrer #${id} ? Solde à encaisser : ${c.total - c.paye} F`)) {
        c.statut = "Livré"; c.paye = c.total;
        localStorage.setItem('sc_db_v16', JSON.stringify(db)); renderAtelier();
    }
}

function envoyerWA(t, c, id) { window.open(`https://wa.me/237${t}?text=Bonjour ${c}, votre commande #${id} est prête chez SUPER CLEAN.`); }

function filtrerAtelier() {
    const q = document.getElementById('search-atelier').value.toLowerCase();
    document.querySelectorAll('#atelier-liste .card').forEach(card => {
        card.style.display = card.innerText.toLowerCase().includes(q) ? "block" : "none";
    });
}

// --- ADMIN ---
function modifierObjectif() {
    const n = prompt("Nouvel objectif (F CFA) :", objectifJournalier);
    if(n) { objectifJournalier = parseInt(n); localStorage.setItem('sc_objectif_v16', objectifJournalier); updateAdminStats(); }
}

function updateAdminStats() {
    const todayStr = new Date().toISOString().split('T')[0];
    const recJour = db.filter(c => new Date(c.time).toISOString().split('T')[0] === todayStr).reduce((a,b)=>a+b.paye,0);
    const pourc = Math.min(Math.round((recJour/objectifJournalier)*100), 100);
    
    document.getElementById('progress-bar').style.width = pourc + "%";
    document.getElementById('progress-text').innerText = pourc + "%";
    document.getElementById('objectif-status').innerText = `${recJour.toLocaleString()} / ${objectifJournalier.toLocaleString()} F`;

    const totalR = db.reduce((a,b)=>a+b.paye,0), totalD = depenses.reduce((a,b)=>a+b.montant,0);
    document.getElementById('admin-stats-ui').innerHTML = `NET EN CAISSE : ${(totalR - totalD).toLocaleString()} F<br><small>Recettes: ${totalR}F | Dépenses: ${totalD}F</small>`;
    
    document.getElementById('liste-depenses-ui').innerHTML = depenses.map((d,i)=>`<div style="display:flex; justify-content:space-between;"><span>${d.motif} (${d.montant}F)</span><button onclick="supprimerDepense(${i})" style="color:red; border:none; background:none;">✕</button></div>`).join('');
    renderStock();
}

function ajouterDepense() {
    const m = document.getElementById('exp-motif').value, v = parseInt(document.getElementById('exp-montant').value);
    if(m && v) { depenses.push({date: new Date().toISOString().split('T')[0], motif: m, montant: v}); localStorage.setItem('sc_expenses_v16', JSON.stringify(depenses)); updateAdminStats(); }
}

function supprimerDepense(i) { if(confirm("Supprimer ?")) { depenses.splice(i,1); localStorage.setItem('sc_expenses_v16', JSON.stringify(depenses)); updateAdminStats(); } }

function genererRapportCloture() {
    const today = new Date().toISOString().split('T')[0];
    const vJ = db.filter(c => new Date(c.time).toISOString().split('T')[0] === today);
    const dJ = depenses.filter(d => d.date === today);
    const enc = vJ.reduce((a,b)=>a+b.paye, 0), dep = dJ.reduce((a,b)=>a+b.montant, 0);
    let msg = `*📊 RAPPORT SUPER CLEAN*\n*Cash:* ${enc}F\n*Dépenses:* ${dep}F\n*NET:* ${enc-dep}F\n*Commandes:* ${vJ.length}`;
    window.open(`https://wa.me/?text=${encodeURIComponent(msg)}`);
}

// --- STOCK ---
function ajouterStock() {
    const n = document.getElementById('stk-nom').value.toUpperCase(), q = parseFloat(document.getElementById('stk-qty').value), u = document.getElementById('stk-unite').value, s = parseFloat(document.getElementById('stk-seuil').value);
    const idx = stock.findIndex(p => p.nom === n);
    if(idx !== -1) { stock[idx].qty = q; stock[idx].seuil = s; } else { stock.push({nom:n, qty:q, unite:u, seuil:s}); }
    localStorage.setItem('sc_stock_v16', JSON.stringify(stock)); renderStock();
}

function renderStock() {
    document.getElementById('stk-liste').innerHTML = stock.map((p,i)=>`<div style="display:flex; justify-content:space-between; padding:5px; border-bottom:1px solid #eee; ${p.qty <= p.seuil ? 'color:red; font-weight:bold;' : ''}"><span>${p.nom}: ${p.qty} ${p.unite}</span><div><button onclick="ajustStk(${i},-1)">-</button><button onclick="ajustStk(${i},1)">+</button></div></div>`).join('');
}
function ajustStk(i, d) { stock[i].qty = Math.max(0, stock[i].qty + d); localStorage.setItem('sc_stock_v16', JSON.stringify(stock)); renderStock(); }

// --- NAV & BACKUP ---
function tab(id) {
    if(id === 'section-admin' && currentUser.role !== 'ADMIN') return alert("Admin seul !");
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    document.getElementById('btn-' + id.split('-')[1]).classList.add('active');
    if(id === 'section-atelier') renderAtelier();
    if(id === 'section-admin') updateAdminStats();
}

function exportData() {
    const data = { db, staff, expenses: depenses, stock, version: "16.5" };
    const blob = new Blob([JSON.stringify(data)], { type: "application/json" });
    const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = `SUPER_CLEAN_FULL.json`; a.click();
}

function importData() {
    const f = document.getElementById('importFile').files[0];
    if(!f) return;
    const r = new FileReader();
    r.onload = (e) => {
        const c = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v16', JSON.stringify(c.db));
        localStorage.setItem('sc_expenses_v16', JSON.stringify(c.expenses));
        localStorage.setItem('sc_stock_v16', JSON.stringify(c.stock));
        location.reload();
    };
    r.readAsText(f);
}
</script>
</body>
</html>
