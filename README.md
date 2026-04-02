<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN ERP v19.0 - SECURED GOLD</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #0056b3; --success: #2ecc71; --danger: #ef233c; 
            --warning: #ff9f1c; --dark: #1b2631; --light: #f4f7f9; --radius: 20px;
        }
        body { font-family: 'Inter', sans-serif; margin: 0; background-color: var(--light); color: var(--dark); user-select: none; }
        .hidden { display: none !important; }
        
        /* --- HEADER --- */
        .header { background: white; padding: 15px; text-align: center; border-bottom: 3px solid var(--primary); position: sticky; top: 0; z-index: 1000; }
        .logo-small { width: 45px; border-radius: 50%; vertical-align: middle; }
        
        /* --- NAVIGATION --- */
        .nav-bar { display: grid; grid-template-columns: repeat(auto-fit, minmax(50px, 1fr)); gap: 4px; padding: 8px; background: #fff; position: fixed; bottom: 0; width: 100%; border-top: 1px solid #ddd; box-sizing: border-box; z-index: 1000; }
        .nav-bar button { background: #f8f9fa; border: none; color: #7f8c8d; font-weight: 800; font-size: 8px; padding: 10px 2px; border-radius: 12px; cursor: pointer; text-transform: uppercase; }
        .nav-bar button.active { background: var(--primary); color: white; }

        /* --- UI COMPONENTS --- */
        .card { background: white; margin: 12px; padding: 18px; border-radius: var(--radius); box-shadow: 0 4px 15px rgba(0,0,0,0.06); border: 1px solid #eee; position: relative; }
        input, select { background: #f1f4f8; border: 2px solid transparent; padding: 14px; border-radius: 12px; margin-bottom: 10px; width: 100%; box-sizing: border-box; font-size: 14px; outline: none; }
        input:focus { border-color: var(--primary); background: white; }
        
        .btn-main { background: var(--primary); color: white; border: none; padding: 16px; border-radius: 14px; font-weight: 800; width: 100%; cursor: pointer; text-transform: uppercase; transition: 0.2s; margin-bottom: 5px; }
        .btn-main:active { transform: scale(0.98); opacity: 0.9; }
        
        .btn-wa { background: #25D366; color: white; border: none; padding: 12px; border-radius: 12px; font-size: 11px; font-weight: 700; width: 100%; margin: 4px 0; cursor: pointer; text-transform: uppercase; }
        .btn-report { background: #075E54; margin-top: 10px; box-shadow: 0 4px 10px rgba(7,94,84,0.3); }

        .badge-solde { background: var(--danger); color: white; padding: 4px 8px; border-radius: 6px; font-size: 10px; font-weight: 900; }
        .retard-card { border: 2px solid var(--danger) !important; background: #fff5f5 !important; }
        .retard-tag { position: absolute; top: -10px; right: 10px; background: var(--danger); color: white; font-size: 10px; padding: 2px 10px; border-radius: 20px; font-weight: 900; }

        .user-info-bar { background: var(--dark); color: white; padding: 5px 15px; font-size: 10px; display: flex; justify-content: space-between; align-items: center; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="height:100vh; display:flex; align-items:center; justify-content:center; background:linear-gradient(135deg, var(--primary), #00b4d8);">
        <div style="background:white; padding:40px; border-radius:30px; width:85%; max-width:340px; text-align:center; box-shadow: 0 20px 40px rgba(0,0,0,0.2);">
            <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:100px; border-radius:50%; margin-bottom:15px;">
            <h2 style="margin:0; color:var(--dark);">SUPER CLEAN</h2>
            <p style="font-size:11px; color:var(--primary); font-weight:700; margin-bottom:25px;">SÉCURITÉ & PERFORMANCE</p>
            <input type="text" id="u-login" placeholder="Identifiant">
            <input type="password" id="u-pass" placeholder="Mot de passe">
            <button onclick="login()" class="btn-main">OUVRIR LA SESSION</button>
        </div>
    </div>
</div>

<div id="app" class="hidden">
    <div class="user-info-bar">
        <span>👤 <span id="display-user-name"></span> (<span id="display-user-role"></span>)</span>
        <span onclick="location.reload()" style="text-decoration: underline; cursor: pointer;">Déconnexion</span>
    </div>
    <div class="header">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" class="logo-small">
        <span style="font-weight:900; color:var(--primary); margin-left:10px;">SUPER CLEAN PRESSING</span>
    </div>

    <div id="content" style="padding-bottom: 100px;">
        
        <div id="section-caisse" class="tab-content">
            <div class="card">
                <h3>📥 RÉCEPTION CLIENT</h3>
                <input type="tel" id="tel-client" placeholder="Numéro WhatsApp (ex: 6...)" onkeyup="rechercherClient()">
                <input type="text" id="client-nom" placeholder="Nom du Client">
                <label style="font-size:10px; font-weight:900; color:#777;">DATE DE RETRAIT PRÉVUE</label>
                <input type="date" id="date-retrait">
                
                <div style="background:#f8f9fa; padding:12px; border-radius:15px; border:1px dashed #ddd;">
                    <select id="select-article" onchange="majPrixSuggere()"></select>
                    <input type="number" id="prix-final" placeholder="Prix final">
                    <button onclick="ajouterAuPanier()" class="btn-main" style="background:var(--dark);">+ AJOUTER AU TICKET</button>
                </div>
            </div>

            <div id="panier-liste" style="padding:0 12px;"></div>

            <div class="card" style="background:var(--dark); color:white;">
                <div style="display:flex; justify-content:space-between; margin-bottom:10px;">
                    <span style="opacity:0.8;">NET À PAYER :</span> <b id="total-caisse" style="font-size:22px; color:var(--success);">0 F</b>
                </div>
                <label style="font-size:10px; color:#aaa;">MODE DE RÈGLEMENT</label>
                <select id="mode-pay" style="background:#2c3e50; border:none; color:white; margin-bottom: 10px;">
                    <option value="Cash">💵 Cash (Espèces)</option>
                    <option value="OM">🍊 Orange Money</option>
                    <option value="Momo">🟡 MTN MoMo</option>
                </select>
                <input type="number" id="paye-caisse" placeholder="Avance reçue" oninput="calculerReste()" style="background:#2c3e50; border:none; color:white;">
                <div id="reste-caisse" style="text-align:right; font-weight:800; color:var(--warning);">Reste : 0 F</div>
            </div>
            <div style="padding:0 12px;">
                <button onclick="validerCommande()" class="btn-main">VALIDER & IMPRIMER TICKET</button>
                <button id="btn-rapport-wa" onclick="envoyerRapportJournalier()" class="btn-main btn-report">📊 RAPPORT JOURNALIER WHATSAPP</button>
            </div>
        </div>

        <div id="section-atelier" class="tab-content hidden">
            <h3 style="margin: 15px;">👕 ZONE TECHNIQUE - SUIVI</h3>
            <div id="atelier-liste"></div>
        </div>

        <div id="section-cloud" class="tab-content hidden">
            <div class="card" style="text-align:center;">
                <h3>☁️ SAUVEGARDE DRIVE / ICLOUD</h3>
                <button onclick="exportData()" class="btn-main" style="background:#6c5ce7;">📤 EXPORTER (.JSON)</button>
                <hr style="margin:30px 0; border:0; border-top:1px solid #eee;">
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
                <h3>👥 GESTION DU PERSONNEL</h3>
                <div id="staff-list"></div>
                <hr>
                <h4>Ajouter un profil</h4>
                <input type="text" id="ns-nom" placeholder="Nom">
                <input type="text" id="ns-log" placeholder="Login">
                <input type="password" id="ns-pass" placeholder="Password">
                <select id="ns-role">
                    <option value="Caissière">Caissière</option>
                    <option value="Laveur">Laveur</option>
                    <option value="Repasseur">Repasseur</option>
                    <option value="ADMIN">Administrateur</option>
                </select>
                <button onclick="ajouterEmploye()" class="btn-main" style="background:var(--success);">CRÉER LE COMPTE</button>
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
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" style="width:60px;"><br>
    <b>SUPER CLEAN PRESSING</b><br>
    --------------------------------<br>
    <div id="tk-body" style="text-align:left; font-size:12px;"></div>
    --------------------------------<br>
    Merci de votre confiance !
</div>

<script>
// --- DONNÉES ---
const TARIFAIRE = {
    "HAUTS": {"Chemise": 500, "Veste 2P": 1500, "Costume luxe": 2500, "Pull Over": 700, "T-Shirt": 500, "Blouson": 1500},
    "BAS": {"Pantalon jeans": 500, "Jupe plissée": 1000, "Pantalon simple": 500, "Short": 500},
    "LIT & MAISON": {"1 Drap": 750, "Ensemble Draps": 2000, "Couette": 2000, "Housse couette": 2500, "Couverture": 4000},
    "ACCESSOIRES": {"Tennis": 500, "Cravate": 500}
};

let db = JSON.parse(localStorage.getItem('sc_db_v19')) || [];
let staff = JSON.parse(localStorage.getItem('sc_staff_v19')) || [
    {nom:"Directeur", login:"admin", pass:"0000", role:"ADMIN"},
    {nom:"Caisse 1", login:"caisse", pass:"1234", role:"Caissière"}
];
let panier = [], currentUser = null;

// --- AUTH & FILTRAGE ---
function login() {
    const l = document.getElementById('u-login').value, p = document.getElementById('u-pass').value;
    const u = staff.find(x => x.login === l && x.pass === p);
    if(u) {
        currentUser = u;
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('display-user-name').innerText = u.nom;
        document.getElementById('display-user-role').innerText = u.role;
        
        filtrerMenuParRole();
        initApp();
    } else alert("Identifiants incorrects !");
}

function filtrerMenuParRole() {
    const r = currentUser.role;
    // Visibilité Boutons Nav
    const hasCaisseAccess = (r === "ADMIN" || r === "Caissière");
    document.getElementById('btn-caisse').style.display = hasCaisseAccess ? "block" : "none";
    document.getElementById('btn-admin').style.display = (r === "ADMIN") ? "block" : "none";
    document.getElementById('btn-cloud').style.display = (r === "ADMIN") ? "block" : "none";
    
    // Visibilité Rapport WA
    document.getElementById('btn-rapport-wa').classList.toggle('hidden', !hasCaisseAccess);

    // Redirection automatique si pas accès caisse
    if(!hasCaisseAccess) tab('section-atelier');
    else tab('section-caisse');
}

function initApp() {
    const sel = document.getElementById('select-article');
    sel.innerHTML = '<option value="">-- Sélectionner Article --</option>';
    for(let cat in TARIFAIRE) {
        let g = document.createElement('optgroup'); g.label = cat;
        for(let art in TARIFAIRE[cat]) {
            let o = document.createElement('option'); o.value = TARIFAIRE[cat][art]; o.innerText = art; g.appendChild(o);
        }
        sel.appendChild(g);
    }
}

// --- LOGIQUE MÉTIER ---
function majPrixSuggere() { document.getElementById('prix-final').value = document.getElementById('select-article').value; }

function ajouterAuPanier() {
    const s = document.getElementById('select-article'), p = document.getElementById('prix-final').value;
    if(!s.value || !p) return;
    panier.push({ uid: Date.now(), nom: s.options[s.selectedIndex].text, prix: parseInt(p) });
    renderPanier();
}

function renderPanier() {
    const t = panier.reduce((a,b)=>a+b.prix, 0);
    document.getElementById('total-caisse').innerText = t + " F";
    document.getElementById('panier-liste').innerHTML = panier.map((x,i)=>`
        <div style="display:flex; justify-content:space-between; background:white; padding:10px; border-radius:10px; margin-bottom:5px; font-size:13px; border-left:4px solid var(--primary);">
            <b>${x.nom}</b> <span>${x.prix}F <button onclick="panier.splice(${i},1);renderPanier()" style="color:red; border:none; background:none; font-weight:bold;">✕</button></span>
        </div>`).join('');
    calculerReste();
}

function calculerReste() {
    const t = panier.reduce((a,b)=>a+b.prix, 0), p = parseInt(document.getElementById('paye-caisse').value) || 0;
    document.getElementById('reste-caisse').innerText = `Reste : ${Math.max(0, t - p)} F`;
}

function validerCommande() {
    const nom = document.getElementById('client-nom').value, tel = document.getElementById('tel-client').value, dr = document.getElementById('date-retrait').value;
    if(!nom || !tel || !dr || panier.length === 0) return alert("Champs incomplets");
    
    const cmd = {
        id: Math.floor(1000 + Math.random()*8999), client: nom, tel: tel, articles: [...panier],
        total: panier.reduce((a,b)=>a+b.prix,0), paye: parseInt(document.getElementById('paye-caisse').value) || 0,
        mode: document.getElementById('mode-pay').value, retrait: dr, caissier: currentUser.nom, 
        timestamp: new Date().getTime(), statut: "Atelier"
    };

    db.unshift(cmd); localStorage.setItem('sc_db_v19', JSON.stringify(db));
    
    document.getElementById('tk-body').innerHTML = `TICKET #${cmd.id}<br>CLIENT: ${cmd.client.toUpperCase()}<br>TEL: ${cmd.tel}<hr>${cmd.articles.map(a => `${a.nom}: ${a.prix}F`).join('<br>')}<hr>TOTAL: ${cmd.total} F<br>SOLDE: ${cmd.total - cmd.paye} F<br>RETRAIT: ${cmd.retrait}`;
    window.print(); location.reload();
}

// --- ZONE TECHNIQUE ---
function renderAtelier() {
    const today = new Date().toISOString().split('T')[0];
    const list = document.getElementById('atelier-liste');
    const isCaisseOrAdmin = (currentUser.role === "ADMIN" || currentUser.role === "Caissière");
    
    list.innerHTML = db.filter(c => c.statut !== "Livré").map(c => {
        const estRetard = c.retrait < today;
        const reste = c.total - c.paye;
        return `
            <div class="card ${estRetard ? 'retard-card' : ''}">
                ${estRetard ? '<span class="retard-tag">⚠️ RETARD</span>' : ''}
                <div style="display:flex; justify-content:space-between; margin-bottom:10px;">
                    <b>#${c.id} - ${c.client}</b>
                    <small style="font-weight:900;">${c.retrait}</small>
                </div>
                <div class="solde-badge">${reste > 0 ? 'SOLDE À PERCEVOIR : ' + reste + ' F' : 'PAYÉ ✔'}</div><br>
                
                ${isCaisseOrAdmin ? `
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:5px;">
                    <button class="btn-wa" onclick="envoyerWA('${c.tel}', '${c.client}', '${c.id}', 'pret')">📲 DISPO</button>
                    <button class="btn-wa" style="background:var(--danger)" onclick="envoyerWA('${c.tel}', '${c.client}', '${c.id}', 'retard')">📲 RETARD</button>
                </div>` : '<p style="font-size:11px; color:#666;">(Messages réservés à la caisse)</p>'}
                
                <button onclick="livrer(${c.id})" class="btn-main" style="margin-top:10px; background:var(--dark);">LIVRER & ARCHIVER</button>
            </div>`;
    }).join('');
}

function envoyerWA(tel, client, id, type) {
    const msg = type === 'pret' 
        ? `Bonjour ${client}, votre commande SUPER CLEAN #${id} est prête. Merci !` 
        : `Bonjour ${client}, nous avons un léger retard sur votre commande #${id}. Toutes nos excuses.`;
    window.open(`https://wa.me/237${tel}?text=${encodeURIComponent(msg)}`);
}

function livrer(id) {
    if(confirm("Encaisser le solde et livrer ?")) {
        const c = db.find(x => x.id === id); c.statut = "Livré"; c.paye = c.total;
        localStorage.setItem('sc_db_v19', JSON.stringify(db)); renderAtelier();
    }
}

// --- RAPPORT JOURNALIER ---
function envoyerRapportJournalier() {
    const todayStr = new Date().toDateString();
    const ticketsToday = db.filter(t => new Date(t.timestamp).toDateString() === todayStr);
    
    let cash = 0, om = 0, momo = 0, total = 0;
    ticketsToday.forEach(t => {
        if(t.mode === "Cash") cash += t.paye;
        if(t.mode === "OM") om += t.paye;
        if(t.mode === "Momo") momo += t.paye;
        total += t.paye;
    });

    const msg = `📊 *RAPPORT JOURNALIER SUPER CLEAN*\n` +
                `📅 Date : ${new Date().toLocaleDateString()}\n` +
                `👤 Responsable : ${currentUser.nom}\n` +
                `----------------------------------\n` +
                `🎟️ Nouveaux tickets : ${ticketsToday.length}\n\n` +
                `💵 Cash : ${cash} F\n` +
                `🍊 Orange Money : ${om} F\n` +
                `🟡 MTN MoMo : ${momo} F\n` +
                `----------------------------------\n` +
                `💰 *TOTAL ENCAISSÉ : ${total} F*`;

    window.open(`https://wa.me/237675864103?text=${encodeURIComponent(msg)}`);
}

// --- ADMIN & CLOUD ---
function exportData() {
    const data = { db, staff };
    const blob = new Blob([JSON.stringify(data)], { type: "application/json" });
    const a = document.createElement('a'); a.href = URL.createObjectURL(blob);
    a.download = `SuperClean_Backup_${new Date().toLocaleDateString()}.json`; a.click();
}

function importData() {
    const reader = new FileReader();
    reader.onload = (e) => {
        const d = JSON.parse(e.target.result);
        localStorage.setItem('sc_db_v19', JSON.stringify(d.db));
        localStorage.setItem('sc_staff_v19', JSON.stringify(d.staff));
        location.reload();
    };
    reader.readAsText(document.getElementById('importFile').files[0]);
}

function ajouterEmploye() {
    const n = document.getElementById('ns-nom').value, l = document.getElementById('ns-log').value, p = document.getElementById('ns-pass').value, r = document.getElementById('ns-role').value;
    if(n && l && p) {
        staff.push({nom:n, login:l, pass:p, role:r});
        localStorage.setItem('sc_staff_v19', JSON.stringify(staff));
        tab('section-admin');
    }
}

function tab(id) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
    document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
    document.getElementById(id).classList.remove('hidden');
    const btn = document.getElementById('btn-' + id.split('-')[1]);
    if(btn) btn.classList.add('active');
    if(id === 'section-atelier') renderAtelier();
    if(id === 'section-admin') {
        document.getElementById('admin-stats-ui').innerText = db.reduce((a,b)=>a+b.paye,0) + " F CFA";
        document.getElementById('staff-list').innerHTML = staff.map((s,i) => `<div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;"><span>${s.nom} (${s.role})</span> ${i!==0?`<button onclick="staff.splice(${i},1);localStorage.setItem('sc_staff_v19',JSON.stringify(staff));tab('section-admin')" style="color:red; border:none; background:none;">✕</button>`:''}</div>`).join('');
    }
}

function rechercherClient() {
    const t = document.getElementById('tel-client').value;
    if(t.length >= 8) {
        const f = db.find(x => x.tel && x.tel.includes(t));
        if(f) document.getElementById('client-nom').value = f.client;
    }
}
</script>
</body>
</html>
