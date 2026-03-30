<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN PRESSING - GESTION OFFICIELLE</title>
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --starch: #9b59b6; 
        }
        
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); }
        
        /* Animations Alertes */
        @keyframes blink { 50% { opacity: 0.4; } }
        .retard-urgent { background: #fff5f5 !important; border: 2px solid var(--danger) !important; animation: blink 1.2s infinite; }
        .today-urgent { border-left: 10px solid var(--warning) !important; background: #fffcf0 !important; }

        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 6px solid var(--primary); }
        .card { background: white; padding: 15px; border-radius: 20px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); margin-bottom: 15px; }
        
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; margin-bottom: 15px; }
        .stat-card { background: white; padding: 10px; border-radius: 15px; text-align: center; border-bottom: 4px solid var(--primary); }
        .stat-card b { font-size: 11px; color: var(--primary); display:block; }
        .stat-card span { font-size: 8px; font-weight: bold; color: gray; text-transform: uppercase; }

        .stock-list { display: flex; flex-wrap: wrap; gap: 5px; margin-top: 10px; }
        .stock-item { background: #fff; border: 1px solid #ddd; padding: 4px 8px; border-radius: 8px; font-size: 10px; font-weight: bold; }
        .starch-item { background: #f3e5f5; border-color: var(--starch); color: var(--starch); }

        input, select, button { width: 100%; padding: 12px; margin: 6px 0; border-radius: 12px; border: 1px solid #ddd; font-size: 14px; box-sizing: border-box; outline:none; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; transition: 0.2s; }
        button:active { transform: scale(0.98); }

        .commande-card { background: white; padding: 15px; margin-bottom: 12px; border-radius: 18px; border-left: 6px solid var(--primary); position: relative; box-shadow: 0 3px 6px rgba(0,0,0,0.05); }
        .badge-id { position: absolute; top: 12px; right: 12px; font-size: 9px; background: #eee; padding: 3px 8px; border-radius: 5px; font-weight: bold; }
        .status-badge { font-size: 10px; font-weight: bold; padding: 4px 8px; border-radius: 6px; display: inline-block; margin: 8px 0; }
        
        .bg-red { background: var(--danger); color: white; }
        .bg-orange { background: var(--warning); color: white; }
        .bg-gray { background: #f0f0f0; color: #666; }

        .hidden { display: none !important; }
        #auth-screen { position:fixed; top:0; left:0; width:100%; height:100%; background:var(--primary); z-index:9999; display:flex; flex-direction:column; align-items:center; justify-content:center; color:white; padding:20px; text-align:center;}
    </style>
</head>
<body>

<div id="auth-screen">
    <div style="background:white; padding:15px; border-radius:50%; margin-bottom:20px;">
        <svg width="40" height="40" viewBox="0 0 24 24" fill="#1e3799"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 3c1.66 0 3 1.34 3 3s-1.34 3-3 3-3-1.34-3-3 1.34-3 3-3zm0 14.2c-2.5 0-4.71-1.28-6-3.22.03-1.99 4-3.08 6-3.08 1.99 0 5.97 1.09 6 3.08-1.29 1.94-3.5 3.22-6 3.22z"/></svg>
    </div>
    <h2 style="margin:0;">SUPER CLEAN</h2>
    <p style="font-size:12px; opacity:0.8;">Système de Gestion Nkounga</p>
    <div style="width: 100%; max-width: 300px; margin-top:20px;">
        <input type="tel" id="u-phone" placeholder="Numéro de téléphone">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); margin-top:10px; height:50px;">SE CONNECTER</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="header">
        <h3 style="margin:0; color:var(--primary);">Super CLEAN Pressing</h3>
        <p id="user-role" style="font-size:11px; margin:5px 0; font-weight:bold; color: #666;"></p>
    </div>

    <div class="stats-grid" id="finance-grid">
        <div class="stat-card" style="border-color:var(--success)"><span>JOUR</span><b id="sj">0 F</b></div>
        <div id="ws" class="stat-card"><span>SEMAINE</span><b id="ss">0 F</b></div>
        <div id="wm" class="stat-card"><span>MOIS</span><b id="sm">0 F</b></div>
    </div>

    <div class="card" id="stock-panel">
        <h5 style="margin:0 0 8px 0; color:var(--secondary); font-size: 11px;">🧺 LINGE EN ATELIER</h5>
        <div id="stock-display" class="stock-list"></div>
    </div>

    <div class="card" id="reception-form">
        <h4 style="margin-top:0;">📋 Réception Client</h4>
        <input type="text" id="client" placeholder="Nom du Client">
        <input type="tel" id="tel" placeholder="WhatsApp (ex: 675...)">
        
        <label style="font-size:11px; font-weight:bold;">📅 Date de Retrait :</label>
        <input type="date" id="date_liv">

        <select id="article-list"></select>
        <select id="article-note">
            <option value="Normal">Soin Normal</option>
            <option value="Amidonné">🌾 Amidonné</option>
            <option value="Taché">⚠️ Taché</option>
            <option value="Luxe">💎 Luxe</option>
        </select>
        <button onclick="ajouterPanier()" style="background:var(--secondary)">+ AJOUTER L'ARTICLE</button>
        
        <div id="panier-zone" style="display:none; margin-top:15px; border-top: 1px dashed #ccc; padding-top:10px;">
            <div id="panier-items" style="font-size:12px; margin-bottom:10px;"></div>
            <div style="text-align:right; font-size:18px; font-weight:bold; color:var(--primary);">TOTAL: <span id="total-calc">0</span> F</div>
            <button onclick="valider()" style="background:var(--success); margin-top:10px;">VALIDER LA COMMANDE</button>
        </div>
    </div>

    <div id="main-list"></div>
    
    <button onclick="logout()" style="background:var(--danger); margin-top:30px; opacity:0.6; font-size:12px;">DÉCONNEXION</button>
</div>

<script>
    const USERS = [
        {t:"675864103", p:"admin26", r:"ADMIN", n:"Gestionnaire"},
        {t:"692315267", p:"cont26", r:"CONTROLEUR", n:"Contrôleur"},
        {t:"600000000", p:"cais26", r:"CAISSIERE", n:"Caisse"},
        {t:"655555555", p:"lav26", r:"LAVEUR", n:"Atelier"}
    ];

    const GRILLE = {
        "LES HAUTS": [
            {n:"T-Shirt", p:500}, {n:"Chemise", p:500}, {n:"Polo", p:500}, {n:"Pull Over", p:700},
            {n:"Blouson", p:1000}, {n:"Démembré", p:300}, {n:"Veste 2 pièces", p:1500}, {n:"Veste 3 pièces", p:2000}
        ],
        "LES BAS": [
            {n:"Pantalon simple", p:500}, {n:"Pantalon jogging", p:500}, {n:"Pantalon jeans", p:500}, 
            {n:"Short", p:500}, {n:"Jupe simple", p:500}, {n:"Jupe plissée", p:1000}
        ],
        "ENSEMBLES": [
            {n:"Costume simple", p:2000}, {n:"Costume de luxe", p:2500}, {n:"Boubou femme", p:1500},
            {n:"Boubou homme 2 pièces", p:1500}, {n:"Boubou homme 3 pièces", p:2500}, {n:"Pyjama 2 pièces", p:1000}, {n:"Jogging", p:1500}
        ],
        "LES ROBES": [
            {n:"Robe simple", p:500}, {n:"Robe de soirée", p:3000}, {n:"Kaba court", p:1000}, 
            {n:"Kaba long", p:1500}, {n:"Toges (Prof/Avocats)", p:3500}
        ],
        "VÊTEMENTS ENFANTS": [
            {n:"Ensemble Enfant", p:1000}, {n:"Robe Enfant", p:500}, {n:"Haut Enfant", p:500}, {n:"Bas Enfant", p:500}
        ],
        "LINGES DE LIT": [
            {n:"1 Drap", p:750}, {n:"Ensemble 1 drap+taies", p:1500}, {n:"Ensemble 2 drap+taies", p:2000},
            {n:"Couvre lit/Couette", p:2000}, {n:"Housse couette", p:2500}, {n:"Couverture", p:3000}
        ],
        "AMEUBLEMENT": [ {n:"Rideau lourd", p:1500}, {n:"Rideau léger", p:1000} ],
        "LINGE DE BAIN": [ {n:"Petite Serviette", p:500}, {n:"Grande Serviette", p:1000}, {n:"Peignoir", p:2000} ],
        "ACCESSOIRES": [ {n:"Tennis", p:500}, {n:"Cravate", p:500} ]
    };

    let user = null; let panier = [];

    function login() {
        const t = document.getElementById('u-phone').value;
        const p = document.getElementById('u-pass').value;
        user = USERS.find(x => x.t === t && x.p === p);
        if(user) { sessionStorage.setItem('sc_u', JSON.stringify(user)); init(); }
        else alert("Accès refusé.");
    }

    function init() {
        document.getElementById('auth-screen').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
        document.getElementById('user-role').innerText = `${user.n} | Profil: ${user.r}`;
        
        if(user.r === "LAVEUR") {
            document.getElementById('finance-grid').classList.add('hidden');
            document.getElementById('reception-form').classList.add('hidden');
        }

        let h = ""; 
        for(let cat in GRILLE) {
            h += `<optgroup label="${cat}">`;
            GRILLE[cat].forEach(i => h += `<option value="${i.p}">${i.n} (${i.p}F)</option>`);
            h += `</optgroup>`;
        }
        document.getElementById('article-list').innerHTML = h;
        document.getElementById('date_liv').valueAsDate = new Date(Date.now() + 86400000);
        update();
    }

    function ajouterPanier() {
        const s = document.getElementById('article-list');
        panier.push({ n: s.options[s.selectedIndex].text.split(' (')[0], p: parseInt(s.value), note: document.getElementById('article-note').value });
        document.getElementById('panier-zone').style.display = 'block';
        document.getElementById('panier-items').innerHTML = panier.map(i => `• ${i.n} [${i.note}]`).join('<br>');
        document.getElementById('total-calc').innerText = panier.reduce((s, i) => s + i.p, 0);
    }

    function valider() {
        const c = document.getElementById('client').value, t = document.getElementById('tel').value, dl = document.getElementById('date_liv').value;
        if(!c || !t || !dl) return alert("Champs vides !");
        const cmd = { uid: "SC-"+Date.now().toString().slice(-4), c, t, items: [...panier], tot: parseInt(document.getElementById('total-calc').innerText), ts: Date.now(), liv: dl, date_rec: new Date().toLocaleDateString('fr-FR') };
        let db = JSON.parse(localStorage.getItem('sc_db')) || [];
        db.push(cmd); localStorage.setItem('sc_db', JSON.stringify(db));
        location.reload();
    }

    function update() {
        const db = JSON.parse(localStorage.getItem('sc_db')) || [];
        const isoToday = new Date().toISOString().split('T')[0];
        
        // Finances
        if(user.r !== "LAVEUR") {
            let rJ = 0; db.forEach(x => { if(x.date_rec === new Date().toLocaleDateString('fr-FR')) rJ += x.tot; });
            document.getElementById('sj').innerText = rJ.toLocaleString() + " F";
        }

        // Stock
        let st = {}; let am = 0;
        db.forEach(c => c.items.forEach(i => { st[i.n] = (st[i.n] || 0) + 1; if(i.note==='Amidonné') am++; }));
        let sH = am > 0 ? `<div class="stock-item starch-item">🌾 AMIDON: ${am}</div>` : "";
        for(let a in st) sH += `<div class="stock-item">${a}: ${st[a]}</div>`;
        document.getElementById('stock-display').innerHTML = sH || "Atelier vide";

        // Liste
        document.getElementById('main-list').innerHTML = `<h4>📦 Liste de Production</h4>` + db.reverse().map(x => {
            let uC = "", bC = "bg-gray", lbl = "Sortie: " + new Date(x.liv).toLocaleDateString('fr-FR'), late = false;
            if (x.liv < isoToday) { uC = "retard-urgent"; bC = "bg-red"; lbl = "⚠️ RETARD"; late = true; }
            else if (x.liv === isoToday) { uC = "today-urgent"; bC = "bg-orange"; lbl = "🔥 AUJOURD'HUI"; late = true; }

            return `
            <div class="commande-card ${uC}">
                <span class="badge-id">${x.uid}</span>
                <strong>${x.c}</strong><br>
                <div class="status-badge ${bC}">${lbl}</div>
                <div style="font-size:11px; margin: 8px 0;">${x.items.map(i => `• ${i.n} [${i.note}]`).join('<br>')}</div>
                <div style="display:flex; gap:5px;">
                   ${user.r !== 'LAVEUR' ? `<button onclick="window.open('https://wa.me/237${x.t}')" style="background:#25D366; font-size:10px; width:auto; padding:8px 12px;">WhatsApp</button>` : ''}
                   ${late && user.r !== 'LAVEUR' ? `<button onclick="rappel('${x.c}','${x.t}')" style="background:var(--warning); font-size:10px; width:auto; padding:8px 12px;">Rappel</button>` : ''}
                   <button onclick="finish('${x.uid}')" style="background:var(--success); font-size:10px; width:auto; padding:8px 12px;">LIVRÉ ✅</button>
                </div>
            </div>`;
        }).join('');
    }

    function rappel(n, t) { window.open(`https://wa.me/237${t}?text=${encodeURIComponent("Bonjour "+n+", Super Clean Pressing vous informe que votre linge est prêt. Merci !")}`); }
    function finish(id) { if(confirm("Sortir le linge du système ?")) { let db = JSON.parse(localStorage.getItem('sc_db')).filter(x => x.uid !== id); localStorage.setItem('sc_db', JSON.stringify(db)); location.reload(); } }
    function logout() { sessionStorage.clear(); location.reload(); }
    window.onload = () => { if(sessionStorage.getItem('sc_u')) { user = JSON.parse(sessionStorage.getItem('sc_u')); init(); } };
</script>
</body>
</html>
