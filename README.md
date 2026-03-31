<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN - GESTION ATELIER v6.4</title>
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --accent: #00d2d3;
        }
        
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); }
        
        @keyframes pulse-red { 0% { box-shadow: 0 0 0 0 rgba(231, 76, 60, 0.7); } 70% { box-shadow: 0 0 0 10px rgba(231, 76, 60, 0); } 100% { box-shadow: 0 0 0 0 rgba(231, 76, 60, 0); } }
        .en-retard { border: 2px solid var(--danger) !important; animation: pulse-red 2s infinite; background: #fff5f5 !important; }

        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 6px solid var(--primary); }
        .logo-box { background: var(--primary); color: white; padding: 10px 15px; border-radius: 12px; font-weight: 900; font-size: 24px; display: inline-block; }
        
        .card { background: white; padding: 15px; border-radius: 15px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); margin-bottom: 15px; }
        
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 10px; border: 1px solid #ddd; font-size: 14px; box-sizing: border-box; outline:none; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; }

        /* Style spécifique Atelier */
        .atelier-container { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 15px; }
        .ticket-atelier { background: white; border-top: 5px solid var(--accent); padding: 15px; border-radius: 12px; position: relative; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        .vêtement-item { background: #f9f9f9; margin: 3px 0; padding: 5px 10px; border-radius: 5px; font-size: 13px; border-left: 3px solid var(--secondary); }
        
        .badge-status { font-size: 10px; padding: 3px 8px; border-radius: 20px; color: white; font-weight: bold; text-transform: uppercase; }
        .hidden { display: none !important; }
        
        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; } }
        #ticket-print { display: none; padding: 10px; font-family: 'Courier New', monospace; }
    </style>
</head>
<body>

<div id="auth-screen" style="height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background:var(--primary); color:white; padding:20px;">
    <div class="logo-box" style="background:white; color:var(--primary);">SC</div>
    <h1 style="margin-top:10px;">SUPER CLEAN</h1>
    <div style="width: 100%; max-width: 320px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); height:50px; margin-top:10px;">ACCÉDER</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="header">
        <div class="logo-box">SC</div>
        <h2 style="margin:5px; color:var(--primary);">SUPER CLEAN</h2>
        <p id="user-info" style="font-size:11px; font-weight:bold; color:var(--secondary); margin:0;"></p>
    </div>

    <div style="display:flex; gap:5px; margin-bottom:15px;">
        <button onclick="tab('section-reception')" id="btn-recep" style="flex:1; background:var(--primary);">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" style="flex:1; background:var(--accent);">👕 ATELIER</button>
    </div>

    <div id="section-reception" class="card hidden tab-content">
        <h4 style="margin:0 0 10px 0;">📥 Nouvelle Réception</h4>
        <input type="text" id="client" placeholder="Nom du Client">
        <input type="tel" id="tel" placeholder="WhatsApp">
        <input type="date" id="date-livraison">

        <div style="background:#f8f9fa; padding:10px; border-radius:15px; margin:10px 0;">
            <select id="select-article"></select>
            <div style="display:flex; gap:5px;">
                <input type="number" id="qte" value="1" min="1" style="flex:1;">
                <button onclick="ajouterAuPanier()" style="flex:1; background:var(--accent);">+ AJOUTER</button>
            </div>
        </div>
        <div id="liste-panier" style="margin-bottom:10px;"></div>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:5px;">
            <select id="mode-p"><option>Espèces</option><option>Mobile Money</option></select>
            <select id="type-p" onchange="toggleAvance()"><option value="Totalité">Payé Total</option><option value="Avance">Avance</option></select>
        </div>
        <input type="number" id="m-avance" placeholder="Montant perçu" style="display:none;">
        <button onclick="validerCommande()" style="background:var(--success); margin-top:10px;">ENREGISTRER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content">
        <h3 style="color:var(--dark); border-left:5px solid var(--accent); padding-left:10px;">📦 VÊTEMENTS EN COURS DE TRAITEMENT</h3>
        <p style="font-size:11px; color:#666;">(Les tickets disparaissent automatiquement après livraison)</p>
        <div id="atelier-grid" class="atelier-container">
            </div>
    </div>

    <div class="card" style="background:var(--dark); margin-top:20px;">
        <input type="text" id="recherche" onkeyup="afficherAtelier()" placeholder="🔍 Chercher un client..." style="margin:0;">
    </div>
</div>

<div id="ticket-print">
    <center><h2>SUPER CLEAN</h2><p>Nkounga, Immeuble de Karche</p></center>
    <hr>
    <p>ID: <span id="t-id"></span> | Client: <span id="t-client"></span></p>
    <p>LIVRAISON: <span id="t-date-liv"></span></p>
    <div id="t-items"></div>
    <hr>
    <p>Reste: <span id="t-res"></span> F</p>
</div>

<script>
    const TARIFS = {"HAUTS": {"Chemise":500, "Veste":1500}, "LIT": {"Drap":750, "Couette":2000}, "ROBES": {"Simple":500, "Toge":3500}}; // etc.
    let EMPLOYES = JSON.parse(localStorage.getItem('sc_staff_v64')) || {"admin": { pass: "0000", role: "ADMIN", nom: "Responsable" }};
    let commandes = JSON.parse(localStorage.getItem('sc_data_v64')) || [];
    let panier = [];
    let user = null;

    function login() {
        const id = document.getElementById('u-login').value.toLowerCase().trim();
        const ps = document.getElementById('u-pass').value;
        if(EMPLOYES[id] && EMPLOYES[id].pass === ps) {
            user = EMPLOYES[id];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-info').innerText = `${user.nom} (${user.role})`;
            tab('section-atelier'); // Par défaut on montre l'atelier
            initialiserTarifs(); 
            reglerDateAuto();
            afficherAtelier();
        } else alert("Erreur de login");
    }

    function tab(id) {
        document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
        document.getElementById(id).classList.remove('hidden');
    }

    function initialiserTarifs() {
        const s = document.getElementById('select-article');
        for(let c in TARIFS) {
            let g = document.createElement('optgroup'); g.label = c;
            for(let a in TARIFS[c]) {
                let o = document.createElement('option'); o.value = TARIFS[c][a]; o.innerText = `${a} (${TARIFS[c][a]}F)`;
                g.appendChild(o);
            }
            s.appendChild(g);
        }
    }

    function reglerDateAuto() {
        const d = new Date(); d.setDate(d.getDate() + 2);
        document.getElementById('date-livraison').value = d.toISOString().split('T')[0];
    }

    function toggleAvance() { document.getElementById('m-avance').style.display = (document.getElementById('type-p').value === 'Avance') ? 'block' : 'none'; }

    function ajouterAuPanier() {
        const s = document.getElementById('select-article');
        const q = parseInt(document.getElementById('qte').value);
        panier.push({ des: s.options[s.selectedIndex].text.split(' (')[0], qte: q, px: parseFloat(s.value), st: parseFloat(s.value)*q });
        document.getElementById('liste-panier').innerHTML = panier.map(i=>`<div>${i.qte}x ${i.des}</div>`).join('');
    }

    function validerCommande() {
        const cmd = {
            id: "SC-" + Date.now().toString().slice(-4),
            client: document.getElementById('client').value,
            date_liv: document.getElementById('date-livraison').value,
            items: [...panier],
            tot: panier.reduce((s,i)=>s+i.st,0),
            status: 'En attente',
            date: new Date().toISOString()
        };
        commandes.unshift(cmd);
        localStorage.setItem('sc_data_v64', JSON.stringify(commandes));
        location.reload();
    }

    // --- FONCTIONNALITÉ ATELIER EXCLUSIVE ---
    function afficherAtelier() {
        const grid = document.getElementById('atelier-grid');
        const txt = document.getElementById('recherche').value.toLowerCase();
        const auj = new Date().toISOString().split('T')[0];
        
        grid.innerHTML = "";
        
        // FILTRE : On affiche seulement si le status n'est PAS "Livré"
        const enCours = commandes.filter(c => c.status !== 'Livré' && c.client.toLowerCase().includes(txt));

        enCours.forEach(c => {
            const retard = (c.date_liv < auj) ? 'en-retard' : '';
            const statusColor = c.status === 'Prêt' ? 'var(--success)' : 'var(--primary)';
            
            grid.innerHTML += `
                <div class="ticket-atelier ${retard}">
                    <span class="badge-status" style="background:${statusColor}">${c.status}</span>
                    <h3 style="margin:10px 0 5px 0;">${c.client.toUpperCase()}</h3>
                    <small>N° ${c.id} | Livraison : <b>${new Date(c.date_liv).toLocaleDateString()}</b></small>
                    <div style="margin-top:10px;">
                        ${c.items.map(i => `<div class="vêtement-item">${i.qte}x <b>${i.des}</b></div>`).join('')}
                    </div>
                    <div style="margin-top:15px; display:flex; gap:5px;">
                        <button onclick="changerStatusAtelier('${c.id}', 'Prêt')" style="background:var(--success); font-size:11px;">MARQUER PRÊT</button>
                        <button onclick="changerStatusAtelier('${c.id}', 'Livré')" style="background:var(--dark); font-size:11px;">LIVRER (SORTIR)</button>
                    </div>
                </div>
            `;
        });

        if(enCours.length === 0) grid.innerHTML = "<p>Aucun vêtement en cours à l'atelier.</p>";
    }

    function changerStatusAtelier(id, st) {
        const i = commandes.findIndex(c => c.id === id);
        commandes[i].status = st;
        localStorage.setItem('sc_data_v64', JSON.stringify(commandes));
        afficherAtelier(); // Rafraîchit la vue
    }
</script>
</body>
</html>
