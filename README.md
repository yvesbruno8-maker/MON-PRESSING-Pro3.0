<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v23.0 - SYSTÈME INTÉGRAL</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --radius: 20px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; }
        .hidden { display: none !important; }
        
        /* --- BRANDING --- */
        .header { background: white; padding: 15px; text-align: center; border-bottom: 3px solid var(--primary); position: sticky; top: 0; z-index: 1000; }
        .logo-small { width: 50px; border-radius: 50%; vertical-align: middle; border: 2px solid var(--primary); }
        .brand-text { font-weight: 900; color: var(--primary); font-size: 18px; margin-left: 10px; }

        /* --- BARRE DE NAVIGATION DYNAMIQUE --- */
        .nav-bar { display: grid; grid-template-columns: repeat(auto-fit, minmax(50px, 1fr)); gap: 4px; padding: 10px; background: #fff; position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 1000; }
        .nav-bar button { background: #f8f9fa; border: none; color: #7f8c8d; font-weight: 800; font-size: 8px; padding: 12px 2px; border-radius: 14px; cursor: pointer; transition: 0.2s; text-transform: uppercase; }
        .nav-bar button.active { background: var(--primary); color: white; transform: translateY(-3px); box-shadow: 0 4px 10px rgba(0, 86, 179, 0.3); }

        /* --- UI COMPONENTS --- */
        .card { background: white; margin: 12px; padding: 18px; border-radius: var(--radius); box-shadow: 0 4px 15px rgba(0,0,0,0.06); border: 1px solid #eee; position: relative; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 14px; border-radius: 12px; margin-bottom: 10px; width: 100%; box-sizing: border-box; font-size: 14px; outline: none; transition: 0.3s; }
        input:focus { border-color: var(--primary); background: white; }
        
        .btn-main { background: var(--primary); color: white; border: none; padding: 16px; border-radius: 14px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; margin-bottom: 10px; }
        .btn-wa { background: #25D366; color: white; border: none; padding: 12px; border-radius: 12px; font-size: 11px; font-weight: 700; width: 100%; margin: 4px 0; cursor: pointer; display: flex; align-items: center; justify-content: center; }
        .btn-report { background: #075E54; margin-top: 10px; box-shadow: 0 4px 10px rgba(7, 94, 84, 0.3); }

        .solde-badge { background: var(--danger); color: white; padding: 4px 10px; border-radius: 8px; font-size: 11px; font-weight: 900; display: inline-block; margin-bottom: 10px; }
        .retard-card { border: 2px solid var(--danger) !important; background: #fff5f5 !important; }
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 8px; background: #f8f9fa; border-radius: 8px; margin-bottom: 5px; font-size: 12px; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--primary), #00b4d8);">
        <div style="background: white; padding: 40px; border-radius: 30px; width: 85%; max-width: 350px; text-align: center; box-shadow: 0 20px 40px rgba(0,0,0,0.2);">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:100px; border-radius:50%; margin-bottom:15px; border: 3px solid #f1f4f8;">
            <h2 style="margin:0; color:var(--dark);">SUPER CLEAN</h2>
            <p style="font-size:11px; color:var(--primary); font-weight:700; margin-bottom:25px;">SÉCURITÉ & EXCELLENCE</p>
            <input type="text" id="u-login" placeholder="Identifiant">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">ACCÉDER</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div style="background: var(--dark); color: white; padding: 5px 15px; font-size: 10px; display: flex; justify-content: space-between;">
        <span>👤 <span id="u-name"></span> (<span id="u-role"></span>)</span>
        <span onclick="location.reload()" style="text-decoration: underline; cursor: pointer;">Déconnexion</span>
    </div>

    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-small">
        <span class="brand-text">SUPER CLEAN PRESSING</span>
    </div>

    <div id="content" style="padding-bottom: 100px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <input type="tel" id="tel-client" placeholder="WhatsApp Client (6...)" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom du Client">
                <label style="font-size:10px; font-weight:900; color:#777;">DATE DE RETRAIT PRÉVUE</label>
                <input type="date" id="date-retrait">
                <div style="background:#f8f9fa; padding:15px; border-radius:15px; border:1px dashed #ddd;">
                    <select id="select-article" onchange="document.getElementById('prix-final').value = this.value"></select>
                    <input type="number" id="prix-final" placeholder="Prix final">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark); margin:0;">+ AJOUTER</button>
                </div>
            </div>
            <div id="panier-liste" style="padding:0 12px;"></div>
            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; margin-bottom:10px;">
                    <span style="opacity:0.8;">NET À PAYER :</span> <b id="total-caisse" style="font-size:24px; color:var(--success);">0 F</b>
                </div>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white; margin: 10px 0;">
                    <option value="Cash">Cash (Espèces)</option>
                    <option value="OM">Orange Money</option>
                    <option value="Momo">MTN MoMo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Avance" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:900; color:var(--warning);">Reste : 0 F</div>
            </div>
            <div style="padding:0 15px;">
                <button onclick="validerCommande()" class="btn-main">VALIDER & TICKET</button>
                <button id="wa-report-btn" onclick="envoyerRapportJournalier()" class="btn-wa btn-report">📊 RAPPORT JOURNALIER (WA)</button>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 15px;">👕 ZONE TECHNIQUE - SUIVI</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-cloud" class="tab-content hidden">
            <div class="card" style="text-align:center;">
                <h3>☁️ CLOUD BACKUP (DRIVE/ICLOUD)</h3>
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 EXPORTER (.JSON)</button>
                <hr style="margin:25px 0; border:0; border-top:1px solid #eee;">
                <input type="file" id="importFile" accept=".json">
                <button onclick="importData()" class="btn-main" style="background:var(--success);">📥 RESTAURER</button>
            </div>
        </div>

        <div id="section-admin" class="tab-content hidden">
            <div class="card" style="background:var(--primary); color:white; text-align:center;">
                <small>CHIFFRE D'AFFAIRES GLOBAL</small>
                <div id="admin-stats-ui" style="font-size:28px; font-weight:900;">0 F CFA</div>
            </div>
            <div class="card">
                <h3>👥 PERSONNEL</h3>
                <div id="staff-list"></div>
                <hr>
                <input type="text" id="ns-nom" placeholder="Nom">
                <input type="text" id="ns-log" placeholder="Login">
                <input type="password" id="ns-pass" placeholder="Pass">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Laveur">Laveur</option>
                    <option value="Repasseur">Repasseur</option>
                    <option value="ADMIN">ADMIN</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">AJOUTER</button>
            </div>
        </div>
    </div>

    <nav class="nav-bar" id="main-nav">
        <button onclick="tab('section-caisse')" id="btn-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-atelier">👕 ZONE TECHNIQUE</button>
        <button onclick="tab('section-cloud')" id="btn-cloud">☁️ CLOUD</button>
        <button onclick="tab('section-admin')" id="btn-admin">👑 ADMIN</button>
    </nav>
</div>

<div id="ticket-print" style="display:none; text-align:center; font-family:monospace; padding:10px;">
    <b>SUPER CLEAN PRESSING</b><br>
    Nkounga, Immeuble Karche<br>
    675 864 103 / 692 315 267<br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:12px;"></div>
    --------------------------------<br>
    Merci de votre confiance !
</div>

<script>
// ==========================================
// 1. GRILLE TARIFAIRE INTÉGRALE
// ==========================================
const GRILLE_OFFICIELLE = {
    "HAUTS": {"T-Shirt":500,"Chemise":500,"Polo":500,"Pull Over":700,"Blouson":1500,"Démembré":300,"Veste 2P":1500,"Veste 3P":2000,"Pantalon simple":500,"Pantalon jeans":500,"Short":500,"Jupe simple":500,"Jupe plissée":1000},
    "ENSEMBLES": {"Costume simple":2000,"Costume luxe":2500,"Boubou femme":1500,"Boubou 3P":2500,"Jogging":1500,"Pyjama 2P":1000},
    "ROBES": {"Robe simple":1000,"Robe soirée":3000,"Kaba court":1000,"Kaba long":1500,"Toge":3500},
    "LIT": {"1 Drap":750,"Ensemble Draps":2000,"Couette":2000,"Housse couette":2500,"Couverture":4000},
    "ENFANTS": {"Ensemble enfant":1000,"Robe enfant":500,"Haut enfant":500},
    "AUTRES": {"Tennis":500,"Cravate":500,"Rideau lourd":1500,"Serviette":1000,"Peignoir":2000}
};

let db = JSON.parse(localStorage.getItem('sc_db_v23')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v23')) || [{nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"}];
let panier = [], currentUser = null;

// ==========================================
// 2. SÉCURITÉ ET FILTRAGE
// ==========================================
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('u-name').innerText = u.nom;
        document.getElementById('u-role').innerText = u.role;
        filtrerMenu();
        initApp();
    } else alert("Accès refusé !");
}

function filtrerMenu() {
    const r = currentUser.role;
    const isCaisse = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').classList.toggle('hidden', !isCaisse);
    document.getElementById('btn-admin').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('btn-cloud').classList.toggle('hidden', r !== "ADMIN");
    document.getElementById('wa-report-btn').classList.toggle('hidden', !isCaisse);
    tab(isCaisse ? 'section-caisse' : 'section-atelier');
}

function initApp() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Article --</option>';
    for(let cat in GRILLE_OFFICIELLE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in GRILLE_OFFICIELLE[cat]) {
            let o = document.createElement('option'); o.value = GRILLE_OFFICIELLE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        sel.appendChild(g);
    }
}

// ==========================================
// 3. LOGIQUE CAISSE
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
        <div style="display:flex; justify-content:space-between; background:white; padding:10px; margin-bottom:5px; border-radius:10px; border-left:4px solid var(--primary);">
            <b>${x.nom}</b> <span>${x.prix} F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none; font-weight:bold;">✕</button></span>
        </div>`).join('');
    calculerReste();
}
function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${Math.max(0, t - p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, tel = document.getElementById('tel-client').value, dr = document.getElementById('date-retrait').value;
    if(!nom || !tel || !dr || panier.length === 0) return alert("Incomplet");
    const cmd = { id: Math.floor(1000+Math.random()*8999), client: nom, tel: tel, articles: [...panier], total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value)||0, mode: document.getElementById('mode-pay').value, retrait: dr, timestamp: new Date().getTime(), agent: currentUser.nom, statut: "En cours" };
    db.unshift(cmd); localStorage.setItem('sc_db_v23', JSON.stringify(db));
    imprimerTicket(cmd);
    location.reload();
}

function imprimerTicket(c) {
    document.getElementById('tk-body').innerHTML = `Ticket #${c.id}<br>Client: ${c.client.toUpperCase()}<br>Tel: ${c.tel}<hr>${c.articles.map(a=>`${a.nom}: ${a.prix}F`).join('<br>')}<hr>TOTAL: ${c.total}F<br>SOLDE: ${c.total-c.paye}F<br>RDV: ${c.retrait}`;
    window.print();
}

// ==========================================
// 4. ZONE TECHNIQUE ET RAPPORT WA
// ==========================================
function renderAtelier() {
    const today = new Date().toISOString().split('T')[0];
    const isCaisse = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    document.getElementById('atelier-liste').innerHTML = db.filter(c => c.statut !== "Livré").map(c => `
        <div class="card ${c.retrait < today ? 'retard-card' : ''}">
            <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b><small>${c.retrait}</small></div>
            <div class="solde-badge">À PERCEVOIR : ${c.total-c.paye} F</div><br>
            ${isCaisse ? `<button class="btn-wa" onclick="envoyerWA('${c.tel}', '${c.client}', '${c.id}', 'pret')">📲 WHATSAPP : PRÊT</button>` : ''}
            <button onclick="livrer(${c.id})" class="btn-main" style="margin-top:10px; background:var(--dark);">LIVRER</button>
        </div>`).join('');
}

function envoyerWA(tel, client, id, type) {
    const msg = type === 'pret' ? `Bonjour ${client}, votre commande SUPER CLEAN #${id} est prête.` : `Bonjour, retard sur #${id}.`;
    window.open(`https://wa.me/237${tel}?text=${encodeURIComponent(msg)}`);
}

function envoyerRapportJournalier() {
    const today = new Date().toDateString();
    const tks = db.filter(t => new Date(t.timestamp).toDateString() === today);
    let cash=0, om=0, momo=0;
    tks.forEach(t => { if(t.mode==="Cash") cash+=t.paye; if(t.mode==="OM") om+=t.paye; if(t.mode==="Momo") momo+=t.paye; });
    const msg = `📊 *RAPPORT SUPER CLEAN*\n📅 ${new Date().toLocaleDateString()}\n👤 Agent : ${currentUser.nom}\n------------------\n💵 Cash : ${cash}F\n🍊 OM : ${om}F\n🟡 Momo : ${momo}F\n💰 *TOTAL : ${cash+om+momo}F*`;
    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
}

function livrer(id) {
    if(confirm("Confirmer livraison ?")) {
        const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
        localStorage.setItem('sc_db_v23', JSON.stringify(db)); renderAtelier();
    }
}

// ==========================================
// 5. ADMIN & CLOUD
// ==========================================
function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]);
    if(btn) btn.classList.add('active');
    if(id === 'section-atelier') renderAtelier();
    if(id === 'section-admin') {
        document.getElementById('admin-stats-ui').innerText = db.reduce((a,b)=>a+b.paye,0) + " F";
        document.getElementById('staff-list').innerHTML = staff.map((s,i) => `<div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;"><span>${s.nom} (${s.role})</span> ${i!==0?`<button onclick="staff.splice(${i},1);localStorage.setItem('sc_staff_v23',JSON.stringify(staff));tab('section-admin')" style="color:red; border:none; background:none;">✕</button>`:''}</div>`).join('');
    }
}

function exportData() {
    const blob = new Blob([JSON.stringify({db, staff})], {type:"application/json"});
    const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download=`SuperClean_Backup.json`; a.click();
}

function importData() {
    const reader = new FileReader();
    reader.onload = (e) => {
        const data = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v23', JSON.stringify(data.db));
        localStorage.setItem('sc_staff_v23', JSON.stringify(data.staff));
        location.reload();
    };
    reader.readAsText(document.getElementById('importFile').files[0]);
}

function ajouterEmploye() {
    const n=document.getElementById('ns-nom').value, l=document.getElementById('ns-log').value, p=document.getElementById('ns-pass').value, r=document.getElementById('ns-role').value;
    if(n&&l&&p) { staff.push({nom:n, login:l, pass:p, role:r}); localStorage.setItem('sc_staff_v23', JSON.stringify(staff)); tab('section-admin'); }
}

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length>=8) { const f = db.find(x => x.tel && x.tel.includes(t)); if(f) document.getElementById('client-nom').value = f.client; }
}
</script>
</body>
</html>
