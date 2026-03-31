<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN - PRO SYSTEM v13.0</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f8f9fa; --accent: #00d2d3; --radius: 16px;
            --shadow: 0 8px 30px rgba(0,0,0,0.08);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 15px; background: var(--light); color: var(--dark); }
        .hidden { display: none !important; }
        .card { background: white; padding: 20px; border-radius: var(--radius); box-shadow: var(--shadow); margin-bottom: 20px; border: 1px solid rgba(0,0,0,0.03); }
        input, select, button { width: 100%; padding: 14px; margin: 8px 0; border-radius: 12px; border: 2px solid #eee; font-size: 15px; outline: none; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; transition: 0.2s; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .nav-bar { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 11px; }
        .nav-bar button.active { background: var(--primary); color: white; }
        .progress-container { background: #eee; height: 6px; border-radius: 10px; margin: 10px 0; overflow: hidden; }
        .progress-fill { background: var(--success); height: 100%; transition: width 0.5s ease; width: 0%; }
        .header-logo { max-width: 150px; display: block; margin: 0 auto; }
        .header-slogan { font-size: 12px; font-style: italic; text-align:center; color: var(--primary); margin-bottom: 15px; }
        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="loader-container" class="hidden" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(255,255,255,0.8); z-index:9999; display:flex; align-items:center; justify-content:center;">Attente...</div>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:20px;">
    <img src="https://i.ibb.co/Y76XvHh/logo-super-clean.png" alt="Logo" class="header-logo" onerror="this.src='https://via.placeholder.com/150?text=SUPER+CLEAN'">
    <div style="width: 100%; max-width: 320px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); height:55px;">CONNEXION</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="card" style="text-align:center;">
        <img src="https://i.ibb.co/Y76XvHh/logo-super-clean.png" alt="Logo" class="header-logo" style="max-width:100px;">
        <p class="header-slogan">Le bien-être de vos vêtements...</p>
        <p id="user-info" style="font-size:10px; font-weight:bold;"></p>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-reception')" id="btn-nav-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-nav-atelier">👕 ATELIER</button>
        <button onclick="tab('section-params')" id="btn-nav-params">⚙️ ADMIN</button>
    </nav>

    <div id="section-reception" class="card hidden tab-content">
        <input type="tel" id="tel-client" placeholder="Téléphone Client" onkeyup="rechercherClient()">
        <input type="text" id="client" placeholder="Nom du Client">
        
        <div style="background:var(--light); padding:10px; border-radius:12px; margin-top:10px;">
            <label style="font-size:11px; font-weight:bold;">SÉLECTIONNER PRESTATION</label>
            <select id="select-article"></select>
            <div style="display:flex; gap:10px;">
                <input type="number" id="qte" value="1" min="1" style="flex:1">
                <button onclick="ajouterAuPanier()" style="background:var(--accent); flex:2">➕ AJOUTER</button>
            </div>
        </div>

        <div id="liste-panier" style="margin:15px 0;"></div>
        
        <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; text-align:right;">
            NET À PAYER : <span id="total-panier" style="font-size:20px; color:var(--success);">0</span> F
        </div>

        <div style="margin-top:10px; background:rgba(0,0,0,0.03); padding:10px; border-radius:12px;">
            <select id="mode-paiement"><option>Espèces</option><option>Mobile Money</option></select>
            <input type="number" id="montant-avance" placeholder="Montant versé (Avance)" oninput="calculerReste()">
            <div id="reste-display" style="text-align:right; font-weight:bold; color:var(--danger); margin-top:5px;">Reste: 0 F</div>
        </div>

        <button onclick="validerCommande()" style="background:var(--success); height:55px; margin-top:15px;">✅ VALIDER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div id="technique-grid"></div>
    </div>

    <div id="section-params" class="card hidden tab-content">
        <h3>Statistiques & Logs</h3>
        <div id="log-list" style="font-size:10px; height:200px; overflow-y:auto; background:#eee; padding:10px; border-radius:8px;"></div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; font-size:12px;">
    <center>
        <b>SUPER CLEAN PRESSING</b><br>
        Le bien-être de vos vêtements...<br>
        📍 Nkounga Immeuble de Karche<br>
        📞 675 864 103 / 692 315 267
    </center>
    <hr>
    <p>Ticket #<span id="t-id"></span> | Client: <span id="t-client"></span></p>
    <div id="t-items"></div>
    <hr>
    <div style="text-align:right;">
        TOTAL : <span id="t-total"></span> F<br>
        AVANCE : <span id="t-avance"></span> F<br>
        <b>RESTE : <span id="t-reste"></span> F</b>
    </div>
    <hr>
    <center><small>Service Express Disponible</small></center>
</div>

<script>
    // GRILLE TARIFAIRE EXTRAITE DE L'IMAGE
    const GRILLE_OFFICIELLE = {
        "LES HAUTS": {
            "T-Shirt": 500, "Chemise": 500, "Polo": 500, "Pull Over": 700, 
            "Blouson": 1000, "Démembré": 300, "Veste 2 pièces": 1500, "Veste 3 pièces": 2000,
            "Pantalon simple": 500, "Pantalon jogging": 500, "Pantalon jeans": 500, "Short": 500,
            "Jupe simple": 500, "Jupe plissée": 1000
        },
        "ENSEMBLES": {
            "Costume simple": 2000, "Costume de luxe": 2500, "Boubou femme": 1500,
            "Boubou homme 2 pièces": 1500, "Boubou homme 3 pièces": 2500,
            "Pyjama 2 pièces": 1000, "Jogging": 1500
        },
        "LES ROBES": {
            "Robe simple": 500, "Robe de soirée": 3000, "Kaba court": 1000, 
            "Kaba long": 1500, "Toge (Magistrat/Avocat/Enseignant)": 3500
        },
        "VÊTEMENTS ENFANTS": {
            "Ensemble Enfant": 1000, "Robe Enfant": 500, "Haut Enfant": 500, "Bas Enfant": 500
        },
        "LINGES DE LIT": {
            "1 Drap": 750, "Ensemble 1 drap + taies": 1500, "Ensemble 2 drap + taies": 2000,
            "Couvre lit ou Couette": 2000, "Housse de couette": 2500, "Couverture": 3000
        },
        "AMEUBLEMENT / BAIN": {
            "Rideau lourd": 1500, "Rideau léger": 1000, "Petite Serviette": 500, 
            "Grande Serviette": 1000, "Peignoir": 2000
        },
        "LES ACCESSOIRES": {
            "Tennis": 500, "Cravate": 500
        }
    };

    let EMPLOYES = {"admin":{nom:"Directeur", pass:"0000", role:"ADMIN"}, "tech":{nom:"Laveur", pass:"1111", role:"TECH"}};
    let commandes = JSON.parse(localStorage.getItem('sc_data')) || [];
    let logs = JSON.parse(localStorage.getItem('sc_logs')) || [];
    let panier = [], user = null;

    function login() {
        const id = document.getElementById('u-login').value.toLowerCase().trim();
        const ps = document.getElementById('u-pass').value;
        if(EMPLOYES[id] && EMPLOYES[id].pass === ps) {
            user = EMPLOYES[id];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-info').innerText = `Opérateur : ${user.nom} (${user.role})`;
            remplirSelect(); tab(user.role === 'TECH' ? 'section-atelier' : 'section-reception');
        } else alert("Erreur identifiants");
    }

    function tab(id) {
        if(id === 'section-reception' && user.role === 'TECH') return;
        document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
        document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
        document.getElementById(id).classList.remove('hidden');
        if(id === 'section-atelier') afficherZoneTechnique();
        if(id === 'section-params') afficherLogs();
    }

    function remplirSelect() {
        const s = document.getElementById('select-article');
        s.innerHTML = "";
        for(let cat in GRILLE_OFFICIELLE) {
            let g = document.createElement('optgroup'); g.label = cat;
            for(let a in GRILLE_OFFICIELLE[cat]) {
                let o = document.createElement('option');
                o.value = GRILLE_OFFICIELLE[cat][a];
                o.innerText = `${a} - ${GRILLE_OFFICIELLE[cat][a]} F`;
                g.appendChild(o);
            }
            s.appendChild(g);
        }
    }

    function ajouterAuPanier() {
        const s = document.getElementById('select-article');
        const q = parseInt(document.getElementById('qte').value);
        const nom = s.options[s.selectedIndex].text.split(' - ')[0];
        panier.push({ nom, qte: q, prix: parseFloat(s.value), st: parseFloat(s.value)*q, pret: false });
        renderPanier();
    }

    function renderPanier() {
        const t = panier.reduce((a,b) => a+b.st, 0);
        document.getElementById('total-panier').innerText = t;
        document.getElementById('liste-panier').innerHTML = panier.map((p,i) => `
            <div style="display:flex; justify-content:space-between; margin:5px 0; font-size:12px; background:white; padding:8px; border-radius:8px;">
                <span>${p.qte}x ${p.nom}</span>
                <b>${p.st} F <span onclick="panier.splice(${i},1);renderPanier()" style="color:red; margin-left:10px; cursor:pointer;">✕</span></b>
            </div>`).join('');
        calculerReste();
    }

    function calculerReste() {
        const t = parseFloat(document.getElementById('total-panier').innerText);
        const a = parseFloat(document.getElementById('montant-avance').value) || 0;
        document.getElementById('reste-display').innerText = `Reste à percevoir : ${t - a} F`;
    }

    function validerCommande() {
        if(!document.getElementById('client').value || panier.length === 0) return alert("Complétez les champs");
        const tot = parseFloat(document.getElementById('total-panier').innerText);
        const av = parseFloat(document.getElementById('montant-avance').value) || 0;
        const cmd = {
            id: Math.floor(1000 + Math.random()*9000),
            client: document.getElementById('client').value,
            tel: document.getElementById('tel-client').value,
            items: [...panier], total: tot, avance: av, reste: tot-av,
            status: "En attente", date: new Date().toLocaleDateString()
        };
        commandes.unshift(cmd);
        localStorage.setItem('sc_data', JSON.stringify(commandes));
        enregistrerLog(`Commande #${cmd.id} créée pour ${cmd.client}`);
        imprimerTicket(cmd);
        location.reload();
    }

    function afficherZoneTechnique() {
        const grid = document.getElementById('technique-grid');
        grid.innerHTML = commandes.filter(c => c.status !== "Livré").map(c => {
            const fini = c.items.filter(i => i.pret).length;
            const pourcent = Math.round((fini / c.items.length) * 100);
            return `
            <div class="card">
                <div style="display:flex; justify-content:space-between;"><b>#${c.id} - ${c.client}</b> <span>${pourcent}%</span></div>
                <div class="progress-container"><div class="progress-fill" style="width:${pourcent}%"></div></div>
                ${c.items.filter(i => !i.pret).map(i => `
                    <div style="display:flex; justify-content:space-between; font-size:12px; margin:5px 0; background:#f0f2f5; padding:8px; border-radius:8px;">
                        <span>${i.qte}x ${i.nom}</span>
                        <button onclick="validerVetement('${c.id}', '${i.nom}')" style="width:auto; padding:5px 10px; font-size:10px; background:var(--success); margin:0;">FINI</button>
                    </div>
                `).join('')}
                ${pourcent === 100 ? `<button onclick="livrer('${c.id}')" style="background:var(--dark); margin-top:10px;">📦 LIVRER (SOLDE: ${c.reste}F)</button>` : ''}
            </div>`;
        }).join('');
    }

    function validerVetement(tid, nom) {
        const c = commandes.find(x => x.id == tid);
        const item = c.items.find(i => i.nom === nom && !i.pret);
        if(item) { item.pret = true; localStorage.setItem('sc_data', JSON.stringify(commandes)); afficherZoneTechnique(); }
    }

    function livrer(id) {
        const c = commandes.find(x => x.id == id);
        if(c.reste > 0 && !confirm(`Confirmer paiement du solde (${c.reste}F) ?`)) return;
        c.status = "Livré"; c.avance += c.reste; c.reste = 0;
        localStorage.setItem('sc_data', JSON.stringify(commandes));
        afficherZoneTechnique();
    }

    function enregistrerLog(msg) { logs.unshift(`${new Date().toLocaleString()} - ${user.nom}: ${msg}`); localStorage.setItem('sc_logs', JSON.stringify(logs)); }
    function afficherLogs() { document.getElementById('log-list').innerHTML = logs.map(l => `<div>${l}</div>`).join(''); }
    function rechercherClient() {
        const t = document.getElementById('tel-client').value;
        if(t.length > 5) { const f = commandes.find(x => x.tel == t); if(f) document.getElementById('client').value = f.client; }
    }
    function imprimerTicket(c) {
        document.getElementById('t-id').innerText = c.id; document.getElementById('t-client').innerText = c.client;
        document.getElementById('t-total').innerText = c.total; document.getElementById('t-avance').innerText = c.avance;
        document.getElementById('t-reste').innerText = c.reste;
        document.getElementById('t-items').innerHTML = c.items.map(i => `<div>${i.qte}x ${i.nom} (${i.st}F)</div>`).join('');
        window.print();
    }
</script>
</body>
</html>
