<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN - SYSTEM PRO v10.5</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>

    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f8f9fa; --accent: #00d2d3; --radius: 16px;
            --shadow: 0 8px 30px rgba(0,0,0,0.08);
        }
        body.dark-mode { --light: #121212; --dark: #f1f2f6; --primary: #546de5; background: #121212; color: #f1f2f6; }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 15px; background: var(--light); color: var(--dark); transition: 0.3s; }
        .hidden { display: none !important; }
        .card { background: white; padding: 20px; border-radius: var(--radius); box-shadow: var(--shadow); margin-bottom: 20px; border: 1px solid rgba(0,0,0,0.03); }
        body.dark-mode .card { background: #1e1e1e; border-color: #333; }
        input, select, button { width: 100%; padding: 14px; margin: 8px 0; border-radius: 12px; border: 2px solid #eee; font-size: 15px; outline: none; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; transition: 0.2s; }
        button:active { transform: scale(0.97); }
        .nav-bar { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 11px; }
        .nav-bar button.active { background: var(--primary); color: white; }
        .spinner { width: 40px; height: 40px; border: 4px solid #f3f3f3; border-top: 4px solid var(--primary); border-radius: 50%; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        #loader-container { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(255,255,255,0.8); z-index: 9999; display: flex; flex-direction: column; align-items: center; justify-content: center; backdrop-filter: blur(4px); }
        .theme-toggle { position: fixed; bottom: 20px; right: 20px; width: 50px; height: 50px; border-radius: 50%; background: var(--primary); color: white; display: flex; align-items: center; justify-content: center; z-index: 1000; box-shadow: 0 4px 15px rgba(0,0,0,0.3); }
        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
    </style>
</head>
<body>

<div id="loader-container" class="hidden"><div class="spinner"></div><p>Traitement...</p></div>
<div class="theme-toggle" onclick="toggleTheme()">🌓</div>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:20px;">
    <h1 style="color:var(--primary);">SUPER CLEAN PRO</h1>
    <div style="width: 100%; max-width: 320px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); height:55px;">CONNEXION</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="card" style="text-align:center; border-top: 5px solid var(--primary);">
        <h2 style="margin:0; color:var(--primary);">SUPER CLEAN</h2>
        <p id="user-info" style="font-size:11px; margin:5px 0; font-weight:bold;"></p>
        <button onclick="location.reload()" style="width:auto; padding:5px 15px; background:var(--danger); font-size:10px;">Quitter</button>
    </div>

    <div class="nav-bar">
        <button onclick="tab('section-reception')" id="btn-nav-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-nav-atelier">👕 ATELIER</button>
        <button onclick="tab('section-params')" id="btn-nav-params">⚙️ ADMIN</button>
    </div>

    <div id="section-reception" class="card hidden tab-content">
        <input type="tel" id="tel-client" placeholder="Téléphone" onkeyup="rechercherClient()">
        <input type="text" id="client" placeholder="Nom Client">
        <input type="date" id="date-livraison">
        <div style="background:var(--light); padding:10px; border-radius:12px; margin-top:10px;">
            <select id="select-article"></select>
            <input type="number" id="qte" value="1" min="1">
            <button onclick="ajouterAuPanier()" style="background:var(--accent)">➕ AJOUTER</button>
        </div>
        <div id="liste-panier" style="margin:15px 0; border-bottom: 1px solid #eee;"></div>
        <input type="number" id="remise-montant" value="0" oninput="renderPanier()" placeholder="Remise (F)">
        
        <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; margin:10px 0;">
            <div style="display:flex; justify-content:space-between;">NET À PAYER: <span id="total-panier">0</span> F</div>
        </div>

        <div style="background:rgba(0,0,0,0.03); padding:12px; border-radius:12px;">
            <label>Mode de Paiement</label>
            <select id="mode-paiement">
                <option value="Espèces">💵 Espèces</option>
                <option value="Mobile Money">📱 Mobile Money</option>
            </select>
            <div style="display:flex; gap:10px; align-items:center;">
                <div style="flex:1;"><small>Avance Versée</small><input type="number" id="montant-avance" value="0" oninput="calculerReste()"></div>
                <div style="flex:1; text-align:right;"><small>RESTE</small><div id="reste-display" style="font-weight:bold; color:var(--danger);">0 F</div></div>
            </div>
        </div>

        <button onclick="ouvrirDepenseRapide()" style="background:var(--danger); height:40px; font-size:12px; margin-top:15px;">💸 SORTIE DE CAISSE</button>
        <button onclick="validerCommande()" style="background:var(--success); height:55px;">✅ VALIDER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <input type="text" id="search-task" placeholder="Rechercher..." onkeyup="afficherZoneTechnique()">
        <div id="technique-grid"></div>
    </div>

    <div id="section-params" class="card hidden tab-content">
        <input type="month" id="stats-month-filter" onchange="mettreAJourStats()">
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin:10px 0;">
            <div style="background:var(--primary); color:white; padding:15px; border-radius:12px;">
                <small>RECETTE NETTE</small><div id="stats-mois-net" style="font-size:18px; font-weight:bold;">0 F</div>
            </div>
            <div style="background:var(--danger); color:white; padding:15px; border-radius:12px;">
                <small>IMPAYÉS</small><div id="stats-impayes" style="font-size:18px; font-weight:bold;">0 F</div>
            </div>
        </div>

        <div class="card">
            <div style="display:flex; justify-content:space-between; font-size:11px;"><span>🎯 OBJECTIF</span><span id="obj-percent">0%</span></div>
            <div style="background:#eee; height:8px; border-radius:10px; margin:5px 0;"><div id="obj-bar" style="background:var(--success); height:100%; transition:1s;"></div></div>
            <input type="number" id="input-obj-mensuel" oninput="sauvegarderObjectif()" placeholder="Fixer Objectif">
        </div>

        <button onclick="partagerRapportWhatsApp()" style="background:#25D366;">🟢 RAPPORT WHATSAPP</button>
        <button onclick="envoyerSauvegardeEmail()" style="background:var(--accent);">📧 SAUVEGARDE EMAIL</button>

        <div class="card"><h4>🏆 TOP CLIENTS</h4><div id="top-clients-list"></div></div>
        <div class="card"><h4>📦 STOCKS</h4><div id="stock-list"></div></div>
        <div class="card"><h4>👥 PERSONNEL</h4><div id="staff-list"></div></div>
        <div class="card"><h4>📜 LOGS</h4><div id="log-list" style="font-size:10px; height:100px; overflow-y:auto; font-family:monospace;"></div></div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; font-size:12px;">
    <center><h3>SUPER CLEAN</h3><p>📞 675 864 103</p></center>
    <hr>
    <p>Ticket: #<span id="t-id"></span><br>Client: <span id="t-client"></span></p>
    <div id="t-items"></div>
    <hr>
    <div style="text-align:right;">
        TOTAL: <span id="t-total"></span> F<br>
        AVANCE: <span id="t-avance"></span> F<br>
        <b>RESTE: <span id="t-reste"></span> F</b>
    </div>
    <p style="text-align:center; margin-top:20px;">Merci de votre confiance !</p>
</div>

<script>
    const GRILLE = { "Vêtements": {"T-Shirt":500,"Chemise":500,"Pantalon":500,"Veste":1500,"Costume":2500,"Robe":1500}, "Linge": {"Drap":1000,"Couette":3000} };
    let EMPLOYES = JSON.parse(localStorage.getItem('sc_staff')) || {"admin": {nom:"Directeur", pass:"0000", role:"ADMIN"}};
    let commandes = JSON.parse(localStorage.getItem('sc_data')) || [];
    let depenses = JSON.parse(localStorage.getItem('sc_expenses')) || [];
    let stocks = JSON.parse(localStorage.getItem('sc_stocks')) || [{nom:"Lessive", qte:10, alerte:2}];
    let logs = JSON.parse(localStorage.getItem('sc_logs')) || [];
    let panier = [], user = null;

    // EmailJS (A remplir)
    (function() { emailjs.init("VOTRE_PUBLIC_KEY"); })();

    function login() {
        const id = document.getElementById('u-login').value.toLowerCase().trim();
        const ps = document.getElementById('u-pass').value;
        if(EMPLOYES[id] && EMPLOYES[id].pass === ps) {
            user = EMPLOYES[id];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-info').innerText = `Connecté : ${user.nom} (${user.role})`;
            if(user.role !== "ADMIN") document.getElementById('btn-nav-params').classList.add('hidden');
            remplirSelect(); tab(user.role === "TECH" ? 'section-atelier' : 'section-reception');
            enregistrerLog("Connexion");
        } else alert("Accès refusé");
    }

    function tab(id) {
        document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
        document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
        document.getElementById(id).classList.remove('hidden');
        if(id === 'section-params') { mettreAJourStats(); afficherStock(); afficherPersonnel(); afficherLogs(); }
        if(id === 'section-atelier') afficherZoneTechnique();
        const btn = id === 'section-reception' ? 'btn-nav-caisse' : (id === 'section-atelier' ? 'btn-nav-atelier' : 'btn-nav-params');
        document.getElementById(btn).classList.add('active');
    }

    function remplirSelect() {
        const s = document.getElementById('select-article');
        for(let cat in GRILLE) {
            let g = document.createElement('optgroup'); g.label = cat;
            for(let a in GRILLE[cat]) { let o = document.createElement('option'); o.value = GRILLE[cat][a]; o.innerText = `${a} (${GRILLE[cat][a]}F)`; g.appendChild(o); }
            s.appendChild(g);
        }
    }

    function ajouterAuPanier() {
        const s = document.getElementById('select-article');
        const q = parseInt(document.getElementById('qte').value);
        panier.push({ nom: s.options[s.selectedIndex].text.split(' (')[0], prix: parseFloat(s.value), qte: q, st: parseFloat(s.value)*q });
        renderPanier();
    }

    function renderPanier() {
        let t = panier.reduce((a,b) => a+b.st, 0) - (parseFloat(document.getElementById('remise-montant').value) || 0);
        document.getElementById('total-panier').innerText = t;
        document.getElementById('liste-panier').innerHTML = panier.map((p,i) => `<div style="display:flex; justify-content:space-between; font-size:12px;"><span>${p.qte}x ${p.nom}</span><b>${p.st}F <span onclick="panier.splice(${i},1);renderPanier()" style="color:red">✕</span></b></div>`).join('');
        calculerReste();
    }

    function calculerReste() {
        const t = parseFloat(document.getElementById('total-panier').innerText);
        const a = parseFloat(document.getElementById('montant-avance').value) || 0;
        document.getElementById('reste-display').innerText = (t - a) + " F";
    }

    function validerCommande() {
        if(!document.getElementById('client').value || panier.length === 0) return alert("Champs vides !");
        montrerChargement();
        setTimeout(() => {
            const tot = parseFloat(document.getElementById('total-panier').innerText);
            const av = parseFloat(document.getElementById('montant-avance').value) || 0;
            const cmd = {
                id: Math.floor(1000 + Math.random()*9000), client: document.getElementById('client').value,
                tel: document.getElementById('tel-client').value, items: [...panier], total: tot,
                avance: av, reste: tot-av, paye: av >= tot, status: "En attente",
                dateEnreg: new Date().toISOString().split('T')[0], modePaiement: document.getElementById('mode-paiement').value
            };
            commandes.unshift(cmd); localStorage.setItem('sc_data', JSON.stringify(commandes));
            enregistrerLog(`Commande #${cmd.id} créée (Payé: ${av}F)`);
            imprimerTicket(cmd); cacherChargement(); location.reload();
        }, 800);
    }

    function ouvrirDepenseRapide() {
        const m = prompt("Motif ?"); const amt = parseFloat(prompt("Montant ?"));
        if(m && amt) {
            depenses.unshift({ id: Date.now(), date: new Date().toISOString().split('T')[0], motif: m, montant: amt });
            localStorage.setItem('sc_expenses', JSON.stringify(depenses));
            enregistrerLog(`Dépense: ${amt}F (${m})`); alert("Enregistré !");
        }
    }

    function afficherZoneTechnique() {
        const q = document.getElementById('search-task').value.toLowerCase();
        document.getElementById('technique-grid').innerHTML = commandes.filter(c => c.status !== 'Livré' && c.client.toLowerCase().includes(q)).map(c => `
            <div class="card" style="border-left: 8px solid ${c.status==='Prêt'?'#2ecc71':'#1e3799'}">
                <b>${c.client} (#${c.id})</b><br><small>Reste: ${c.reste}F | Status: ${c.status}</small>
                <div style="display:flex; gap:5px; margin-top:10px;">
                    <button onclick="prochaineEtape('${c.id}')" style="font-size:10px;">Étape Suivante</button>
                    <button onclick="livrer('${c.id}')" style="background:var(--dark); font-size:10px;">📦 Livrer</button>
                </div>
            </div>
        `).join('');
    }

    function prochaineEtape(id) {
        const steps = ["En attente", "Lavage", "Repassage", "Prêt"];
        const i = commandes.findIndex(c => c.id == id);
        let cur = steps.indexOf(commandes[i].status);
        if(cur < 3) { commandes[i].status = steps[cur+1]; localStorage.setItem('sc_data', JSON.stringify(commandes)); afficherZoneTechnique(); }
    }

    function livrer(id) {
        const i = commandes.findIndex(c => c.id == id);
        if(commandes[i].reste > 0) {
            if(confirm(`Solde de ${commandes[i].reste}F à payer. Règlement reçu ?`)) {
                commandes[i].avance += commandes[i].reste; commandes[i].reste = 0; commandes[i].paye = true;
            } else return;
        }
        commandes[i].status = "Livré"; localStorage.setItem('sc_data', JSON.stringify(commandes));
        enregistrerLog(`Commande #${id} livrée`); afficherZoneTechnique();
    }

    function mettreAJourStats() {
        const mv = document.getElementById('stats-month-filter').value || new Date().toISOString().slice(0,7);
        const obj = localStorage.getItem('sc_objectif_mensuel') || 500000;
        const cmdM = commandes.filter(c => c.dateEnreg.startsWith(mv));
        const depM = depenses.filter(d => d.date.startsWith(mv));
        
        const encaissé = cmdM.reduce((a,b) => a + b.avance, 0);
        const totalDep = depM.reduce((a,b) => a + b.montant, 0);
        const impayes = commandes.filter(c => !c.paye).reduce((a,b) => a + b.reste, 0);

        document.getElementById('stats-mois-net').innerHTML = `${(encaissé - totalDep).toLocaleString()} F <br><small style="font-weight:normal; font-size:10px;">Dépenses: -${totalDep}F</small>`;
        document.getElementById('stats-impayes').innerText = impayes.toLocaleString() + " F";
        
        const p = Math.min(100, Math.round(((encaissé-totalDep)/obj)*100));
        document.getElementById('obj-bar').style.width = p+"%";
        document.getElementById('obj-percent').innerText = p+"%";
    }

    function enregistrerLog(a) { logs.unshift({date: new Date().toLocaleString(), auteur: user.nom, action: a}); localStorage.setItem('sc_logs', JSON.stringify(logs)); }
    function afficherLogs() { document.getElementById('log-list').innerHTML = logs.map(l => `<div>[${l.date}] ${l.auteur}: ${l.action}</div>`).join(''); }
    function montrerChargement() { document.getElementById('loader-container').classList.remove('hidden'); }
    function cacherChargement() { document.getElementById('loader-container').classList.add('hidden'); }
    function toggleTheme() { document.body.classList.toggle('dark-mode'); }

    function imprimerTicket(c) {
        document.getElementById('t-id').innerText = c.id; document.getElementById('t-client').innerText = c.client;
        document.getElementById('t-total').innerText = c.total; document.getElementById('t-avance').innerText = c.avance;
        document.getElementById('t-reste').innerText = c.reste;
        document.getElementById('t-items').innerHTML = c.items.map(i => `<div>${i.qte}x ${i.nom}</div>`).join('');
        window.print();
    }

    // Fonctions simplifiées pour Stocks et Personnel
    function afficherStock() { document.getElementById('stock-list').innerHTML = stocks.map((s,i) => `<div style="font-size:12px; display:flex; justify-content:space-between;">${s.nom}: <b>${s.qte}</b> <span onclick="stocks[${i}].qte++; afficherStock()">➕</span></div>`).join(''); }
    function afficherPersonnel() { document.getElementById('staff-list').innerHTML = Object.keys(EMPLOYES).map(k => `<div style="font-size:12px;">${EMPLOYES[k].nom} (${EMPLOYES[k].role})</div>`).join(''); }
    function sauvegarderObjectif() { localStorage.setItem('sc_objectif_mensuel', document.getElementById('input-obj-mensuel').value); mettreAJourStats(); }
    function rechercherClient() {
        const tel = document.getElementById('tel-client').value;
        if(tel.length > 5) { const found = commandes.find(c => c.tel == tel); if(found) document.getElementById('client').value = found.client; }
    }
</script>
</body>
</html>
