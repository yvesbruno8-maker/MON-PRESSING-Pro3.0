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
            --light: #f8f9fa; --accent: #00d2d3; --radius: 16px;
            --shadow: 0 8px 30px rgba(0,0,0,0.08);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 15px; background: var(--light); color: var(--dark); overflow-x: hidden; }
        .hidden { display: none !important; }
        .card { background: white; padding: 20px; border-radius: var(--radius); box-shadow: var(--shadow); margin-bottom: 20px; border: 1px solid rgba(0,0,0,0.03); position: relative; }
        input, select, button { width: 100%; padding: 14px; margin: 8px 0; border-radius: 12px; border: 2px solid #eee; font-size: 15px; outline: none; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; transition: 0.2s; display: flex; align-items: center; justify-content: center; gap: 8px; }
        button:active { transform: scale(0.97); }
        .nav-bar { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 11px; box-shadow: none; height: 50px;}
        .nav-bar button.active { background: var(--primary); color: white; }
        
        .progress-container { background: #eee; height: 8px; border-radius: 10px; margin: 10px 0; overflow: hidden; position: relative; }
        .progress-fill { background: var(--success); height: 100%; transition: width 0.5s ease; width: 0%; }
        
        .header-logo { max-width: 140px; display: block; margin: 0 auto; }
        .header-slogan { font-size: 12px; font-style: italic; text-align:center; color: var(--primary); margin-top: -10px; margin-bottom: 15px; font-weight: 600; }
        
        .stat-box { padding: 15px; border-radius: 12px; color: white; text-align: center; }
        
        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="loader-container" class="hidden" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(255,255,255,0.9); z-index:9999; display:flex; flex-direction:column; align-items:center; justify-content:center;">
    <div style="width:40px; height:40px; border:4px solid #f3f3f3; border-top:4px solid var(--primary); border-radius:50%; animation: spin 1s linear infinite;"></div>
    <p>Traitement en cours...</p>
</div>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:20px;">
    <img src="https://i.ibb.co/Y76XvHh/logo-super-clean.png" alt="Super Clean Logo" class="header-logo" onerror="this.src='https://via.placeholder.com/150?text=SUPER+CLEAN'">
    <h2 style="color:var(--primary); margin-top:10px;">Connexion</h2>
    <div style="width: 100%; max-width: 320px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); height:55px; font-size:18px;">ENTRER</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="card" style="text-align:center; border-top: 5px solid var(--primary); padding-bottom: 10px;">
        <img src="https://i.ibb.co/Y76XvHh/logo-super-clean.png" alt="Logo" class="header-logo" style="max-width:80px;">
        <p class="header-slogan">Le bien-être de vos vêtements...</p>
        <div id="user-info" style="font-size:11px; background:var(--light); padding:5px; border-radius:20px; display:inline-block; padding: 5px 15px;"></div>
        <button onclick="location.reload()" style="width:auto; padding:4px 10px; background:var(--danger); font-size:10px; margin: 10px auto 0 auto; height: 25px;">Déconnexion</button>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-reception')" id="btn-nav-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-nav-atelier">👕 ATELIER</button>
        <button onclick="tab('section-params')" id="btn-nav-params">⚙️ ADMIN</button>
    </nav>

    <div id="section-reception" class="card hidden tab-content">
        <input type="tel" id="tel-client" placeholder="N° Téléphone" onkeyup="rechercherClient()">
        <input type="text" id="client" placeholder="Nom du Client">
        
        <div style="background:var(--light); padding:15px; border-radius:12px; margin-top:10px; border: 1px solid #ddd;">
            <label style="font-size:11px; font-weight:bold; color:var(--primary);">CHOISIR UN ARTICLE</label>
            <select id="select-article"></select>
            <div style="display:flex; gap:10px;">
                <input type="number" id="qte" value="1" min="1" style="flex:1">
                <button onclick="ajouterAuPanier()" style="background:var(--accent); flex:2">➕ AJOUTER</button>
            </div>
        </div>

        <div id="liste-panier" style="margin:15px 0;"></div>
        
        <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; text-align:right;">
            TOTAL : <span id="total-panier" style="font-size:22px; color:var(--success); font-weight:800;">0</span> F
        </div>

        <div style="margin-top:10px; background:rgba(0,0,0,0.03); padding:15px; border-radius:12px;">
            <input type="number" id="montant-avance" placeholder="Avance versée (F)" oninput="calculerReste()">
            <div id="reste-display" style="text-align:right; font-weight:bold; color:var(--danger); margin-top:5px;">Reste à payer : 0 F</div>
        </div>

        <button onclick="validerCommande()" style="background:var(--success); height:60px; margin-top:15px; font-size:16px;">✅ ENREGISTRER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <input type="text" id="search-task" placeholder="Rechercher ticket ou client..." onkeyup="afficherZoneTechnique()" style="margin-bottom:15px;">
        <div id="technique-grid"></div>
    </div>

    <div id="section-params" class="hidden tab-content">
        <div class="card" style="border-top: 5px solid var(--primary);">
            <h3 style="text-align:center; margin:0 0 15px 0;">📊 PERFORMANCE</h3>
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:10px;">
                <div class="stat-box" style="background:var(--primary);">
                    <small>AUJOURD'HUI</small><br><b id="stat-jour-ca" style="font-size:18px;">0 F</b>
                </div>
                <div class="stat-box" style="background:var(--success);">
                    <small>MOIS EN COURS</small><br><b id="stat-mois-ca" style="font-size:18px;">0 F</b>
                </div>
            </div>
            
            <div style="margin-top:20px;">
                <div style="display:flex; justify-content:space-between; font-size:11px;">
                    <b>🎯 OBJECTIF MENSUEL</b>
                    <span id="label-pourcent-objectif" style="font-weight:bold;">0%</span>
                </div>
                <div class="progress-container"><div id="barre-objectif-mois" class="progress-fill"></div></div>
                <input type="number" id="input-objectif" value="500000" onchange="calculerPerformance()" style="height:35px; font-size:12px; margin:0;">
            </div>
        </div>

        <div class="card">
            <h4>👥 GESTION DU PERSONNEL</h4>
            <div style="background: var(--light); padding: 10px; border-radius: 12px; margin-bottom: 10px;">
                <input type="text" id="new-staff-name" placeholder="Nom">
                <input type="text" id="new-staff-login" placeholder="Login">
                <input type="password" id="new-staff-pass" placeholder="Pass">
                <select id="new-staff-role">
                    <option value="CAISSE">📥 CAISSE</option>
                    <option value="TECH">👕 TECHNIQUE (ATELIER)</option>
                    <option value="ADMIN">👑 ADMIN</option>
                </select>
                <button onclick="ajouterPersonnel()" style="background:var(--success); height:40px;">➕ CRÉER COMPTE</button>
            </div>
            <div id="staff-list-detail"></div>
        </div>

        <div class="card">
            <h4>📜 HISTORIQUE DES LOGS</h4>
            <div id="log-list" style="font-size:10px; height:150px; overflow-y:auto; font-family:monospace; background:#eee; padding:10px; border-radius:8px;"></div>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; padding:10px; width: 80mm;">
    <center>
        <b style="font-size:16px;">SUPER CLEAN PRESSING</b><br>
        Le bien-être de vos vêtements...<br>
        📍 Nkounga Immeuble de Karche<br>
        📞 675 864 103 / 692 315 267
    </center>
    <hr>
    <p><b>Ticket #<span id="t-id"></span></b><br>Date: <span id="t-date"></span><br>Client: <span id="t-client"></span></p>
    <hr>
    <div id="t-items"></div>
    <hr>
    <div style="text-align:right;">
        TOTAL : <span id="t-total"></span> F<br>
        PAYÉ : <span id="t-avance"></span> F<br>
        <b>RESTE : <span id="t-reste"></span> F</b>
    </div>
    <hr>
    <center><small>Merci de votre confiance !</small></center>
</div>

<script>
    // 1. DONNÉES DE BASE
    const GRILLE = {
        "LES HAUTS": {"T-Shirt":500, "Chemise":500, "Polo":500, "Pull Over":700, "Blouson":1000, "Démembré":300, "Veste 2p":1500, "Veste 3p":2000, "Pantalon":500, "Short":500, "Jupe":500},
        "ENSEMBLES": {"Costume simple":2000, "Costume luxe":2500, "Boubou femme":1500, "Boubou homme":2000, "Jogging":1500, "Pyjama":1000},
        "ROBES": {"Robe simple":500, "Robe de soirée":3000, "Kaba":1000, "Toge":3500},
        "ENFANTS": {"Ensemble Enfant":1000, "Haut/Bas Enfant":500},
        "LINGE DE LIT": {"1 Drap":750, "Ensemble Draps":1500, "Couette/Couvre-lit":2000, "Housse couette":2500, "Couverture":3000},
        "BAIN/AUTRES": {"Petite Serviette":500, "Grande Serviette":1000, "Peignoir":2000, "Rideau lourd":1500, "Rideau léger":1000, "Tennis":500, "Cravate":500}
    };

    let EMPLOYES = JSON.parse(localStorage.getItem('sc_staff')) || {
        "admin": {nom:"Directeur", pass:"0000", role:"ADMIN"},
        "tech": {nom:"Laveur", pass:"1111", role:"TECH"}
    };
    let commandes = JSON.parse(localStorage.getItem('sc_data')) || [];
    let logs = JSON.parse(localStorage.getItem('sc_logs')) || [];
    let panier = [], user = null;

    // 2. AUTHENTIFICATION & NAVIGATION
    function login() {
        const id = document.getElementById('u-login').value.toLowerCase().trim();
        const ps = document.getElementById('u-pass').value;
        if(EMPLOYES[id] && EMPLOYES[id].pass === ps) {
            user = EMPLOYES[id];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-info').innerText = `${user.nom} [${user.role}]`;
            
            // Masquage Navigation selon rôle
            if(user.role === 'TECH') {
                document.getElementById('btn-nav-caisse').classList.add('hidden');
                document.getElementById('btn-nav-params').classList.add('hidden');
                tab('section-atelier');
            } else if(user.role === 'CAISSE') {
                document.getElementById('btn-nav-params').classList.add('hidden');
                tab('section-reception');
            } else {
                tab('section-reception');
            }
            remplirSelect();
        } else alert("Identifiants incorrects");
    }

    function tab(id) {
        if(id === 'section-reception' && user.role === 'TECH') return;
        if(id === 'section-params' && user.role !== 'ADMIN') return;

        document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
        document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
        document.getElementById(id).classList.remove('hidden');
        
        const btnId = id === 'section-reception' ? 'btn-nav-caisse' : (id === 'section-atelier' ? 'btn-nav-atelier' : 'btn-nav-params');
        document.getElementById(btnId).classList.add('active');

        if(id === 'section-atelier') afficherZoneTechnique();
        if(id === 'section-params') { calculerPerformance(); afficherPersonnel(); afficherLogs(); }
    }

    // 3. LOGIQUE CAISSE
    function remplirSelect() {
        const s = document.getElementById('select-article');
        s.innerHTML = "";
        for(let cat in GRILLE) {
            let g = document.createElement('optgroup'); g.label = cat;
            for(let a in GRILLE[cat]) {
                let o = document.createElement('option'); o.value = GRILLE[cat][a];
                o.innerText = `${a} (${GRILLE[cat][a]} F)`;
                g.appendChild(o);
            }
            s.appendChild(g);
        }
    }

    function ajouterAuPanier() {
        const s = document.getElementById('select-article');
        const q = parseInt(document.getElementById('qte').value);
        const nom = s.options[s.selectedIndex].text.split(' (')[0];
        panier.push({ nom, qte: q, prix: parseFloat(s.value), st: parseFloat(s.value)*q, pret: false });
        renderPanier();
    }

    function renderPanier() {
        const t = panier.reduce((a,b) => a+b.st, 0);
        document.getElementById('total-panier').innerText = t.toLocaleString();
        document.getElementById('liste-panier').innerHTML = panier.map((p,i) => `
            <div style="display:flex; justify-content:space-between; font-size:12px; background:white; padding:10px; border-radius:8px; margin-bottom:5px;">
                <span>${p.qte}x ${p.nom}</span>
                <b>${p.st} F <span onclick="panier.splice(${i},1);renderPanier()" style="color:red; margin-left:10px; cursor:pointer;">✕</span></b>
            </div>`).join('');
        calculerReste();
    }

    function calculerReste() {
        const t = panier.reduce((a,b) => a+b.st, 0);
        const av = parseFloat(document.getElementById('montant-avance').value) || 0;
        document.getElementById('reste-display').innerText = `Reste à payer : ${t - av} F`;
    }

    function validerCommande() {
        if(!document.getElementById('client').value || panier.length === 0) return alert("Complétez les informations !");
        document.getElementById('loader-container').classList.remove('hidden');
        
        setTimeout(() => {
            const tot = panier.reduce((a,b) => a+b.st, 0);
            const av = parseFloat(document.getElementById('montant-avance').value) || 0;
            const cmd = {
                id: Math.floor(1000 + Math.random()*9000),
                client: document.getElementById('client').value,
                tel: document.getElementById('tel-client').value,
                items: [...panier], total: tot, avance: av, reste: tot-av,
                status: "En attente", date: new Date().toLocaleDateString('fr-FR')
            };
            commandes.unshift(cmd);
            localStorage.setItem('sc_data', JSON.stringify(commandes));
            enregistrerLog(`Caisse: Commande #${cmd.id} (${cmd.client})`);
            imprimerTicket(cmd);
            location.reload();
        }, 800);
    }

    // 4. LOGIQUE ATELIER (PROGRESSION)
    function afficherZoneTechnique() {
        const q = document.getElementById('search-task').value.toLowerCase();
        const grid = document.getElementById('technique-grid');
        grid.innerHTML = "";

        commandes.filter(c => c.status !== "Livré").forEach(c => {
            if(c.client.toLowerCase().includes(q) || c.id.toString().includes(q)) {
                const articlesRestants = c.items.filter(i => !i.pret);
                const total = c.items.length;
                const fini = total - articlesRestants.length;
                const pourcent = Math.round((fini / total) * 100);

                grid.innerHTML += `
                    <div class="card" style="border-left: 6px solid ${pourcent===100?'var(--success)':'var(--primary)'}">
                        <div style="display:flex; justify-content:space-between; margin-bottom:5px;">
                            <b>${c.client} (#${c.id})</b>
                            <span style="color:var(--primary); font-weight:800;">${pourcent}%</span>
                        </div>
                        <div class="progress-container"><div class="progress-fill" style="width:${pourcent}%"></div></div>
                        <div style="margin-top:10px;">
                            ${articlesRestants.length > 0 ? 
                                articlesRestants.map(i => `
                                    <div style="display:flex; justify-content:space-between; background:#f0f2f5; padding:8px; border-radius:10px; margin:4px 0; font-size:13px;">
                                        <span>${i.qte}x ${i.nom}</span>
                                        <button onclick="validerVetement('${c.id}', '${i.nom}')" style="width:auto; padding:5px 12px; font-size:10px; background:var(--success); margin:0;">FINI</button>
                                    </div>
                                `).join('') : 
                                `<div style="text-align:center; padding:10px;"><button onclick="livrer('${c.id}')" style="background:var(--dark);">📦 LIVRER (SOLDE: ${c.reste}F)</button></div>`
                            }
                        </div>
                    </div>`;
            }
        });
    }

    function validerVetement(tid, nom) {
        const cmd = commandes.find(c => c.id == tid);
        const item = cmd.items.find(i => i.nom === nom && !i.pret);
        if(item) {
            item.pret = true;
            localStorage.setItem('sc_data', JSON.stringify(commandes));
            afficherZoneTechnique();
        }
    }

    function livrer(id) {
        const cmd = commandes.find(c => c.id == id);
        if(cmd.reste > 0 && !confirm(`Confirmer le règlement du reste (${cmd.reste} F) ?`)) return;
        cmd.status = "Livré"; cmd.avance += cmd.reste; cmd.reste = 0;
        localStorage.setItem('sc_data', JSON.stringify(commandes));
        enregistrerLog(`Caisse: Livraison effectuée #${id}`);
        afficherZoneTechnique();
    }

    // 5. ADMIN & PERFORMANCE
    function calculerPerformance() {
        const ajd = new Date().toLocaleDateString('fr-FR');
        const ceMois = new Date().getMonth();
        let caJour = 0, caMois = 0;

        commandes.forEach(c => {
            if(c.date === ajd) caJour += c.total;
            // Simplification pour le mois
            const d = c.date.split('/');
            if(parseInt(d[1])-1 === ceMois) caMois += c.total;
        });

        document.getElementById('stat-jour-ca').innerText = caJour.toLocaleString() + " F";
        document.getElementById('stat-mois-ca').innerText = caMois.toLocaleString() + " F";
        
        const obj = parseFloat(document.getElementById('input-objectif').value) || 1;
        const p = Math.min(100, Math.round((caMois / obj) * 100));
        document.getElementById('barre-objectif-mois').style.width = p + "%";
        document.getElementById('label-pourcent-objectif').innerText = p + "%";
    }

    function ajouterPersonnel() {
        const nom = document.getElementById('new-staff-name').value;
        const id = document.getElementById('new-staff-login').value.toLowerCase().trim();
        const ps = document.getElementById('new-staff-pass').value;
        const ro = document.getElementById('new-staff-role').value;
        if(!id || !ps) return;
        EMPLOYES[id] = {nom, pass:ps, role:ro};
        localStorage.setItem('sc_staff', JSON.stringify(EMPLOYES));
        afficherPersonnel();
        alert("Compte créé !");
    }

    function supprimerStaff(id) {
        if(id === 'admin') return;
        delete EMPLOYES[id];
        localStorage.setItem('sc_staff', JSON.stringify(EMPLOYES));
        afficherPersonnel();
    }

    function afficherPersonnel() {
        const list = document.getElementById('staff-list-detail');
        list.innerHTML = Object.keys(EMPLOYES).map(id => `
            <div style="display:flex; justify-content:space-between; padding:10px; border-bottom:1px solid #eee;">
                <span><b>${EMPLOYES[id].nom}</b> (${EMPLOYES[id].role})</span>
                ${id!=='admin'?`<button onclick="supprimerStaff('${id}')" style="width:auto; background:none; color:red; padding:0;">🗑️</button>`:''}
            </div>`).join('');
    }

    // 6. UTILS
    function enregistrerLog(msg) {
        logs.unshift(`[${new Date().toLocaleTimeString()}] ${user.nom}: ${msg}`);
        if(logs.length > 50) logs.pop();
        localStorage.setItem('sc_logs', JSON.stringify(logs));
    }
    function afficherLogs() { document.getElementById('log-list').innerHTML = logs.map(l => `<div>${l}</div>`).join(''); }
    function rechercherClient() {
        const t = document.getElementById('tel-client').value;
        if(t.length > 6) { const f = commandes.find(x => x.tel == t); if(f) document.getElementById('client').value = f.client; }
    }
    function imprimerTicket(c) {
        document.getElementById('t-id').innerText = c.id; document.getElementById('t-client').innerText = c.client;
        document.getElementById('t-date').innerText = c.date; document.getElementById('t-total').innerText = c.total;
        document.getElementById('t-avance').innerText = c.avance; document.getElementById('t-reste').innerText = c.reste;
        document.getElementById('t-items').innerHTML = c.items.map(i => `<div>${i.qte}x ${i.nom} (${i.st}F)</div>`).join('');
        window.print();
    }

    window.onload = () => { if(localStorage.getItem('sc_obj')) document.getElementById('input-objectif').value = localStorage.getItem('sc_obj'); };
</script>
</body>
</html>
