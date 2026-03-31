<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN - SYSTEM v10.0</title>
    
    <link rel="manifest" href='data:application/json,{"name":"Super Clean Pressing","short_name":"SuperClean","start_url":"index.html","display":"standalone","background_color":"#1e3799","theme_color":"#1e3799","icons":[{"src":"https://cdn-icons-png.flaticon.com/512/2975/2975175.png","sizes":"192x192","type":"image/png"}]}'>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/2975/2975175.png">
    <meta name="theme-color" content="#1e3799">

    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --accent: #00d2d3;
        }

        /* MODE SOMBRE AUTOMATIQUE */
        body.dark-mode {
            --light: #121212; --dark: #f1f2f6; --primary: #546de5;
            background-color: #121212; color: #f1f2f6;
        }
        
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); transition: 0.3s; overflow-x: hidden; }
        
        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 6px solid var(--primary); position: relative; }
        body.dark-mode .header, body.dark-mode .card { background: #1e1e1e !important; color: white; }

        .logo-box { background: var(--primary); color: white; padding: 10px 15px; border-radius: 12px; font-weight: 900; font-size: 24px; display: inline-block; }
        
        .card { background: white; padding: 15px; border-radius: 15px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); margin-bottom: 15px; }
        
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 10px; border: 1px solid #ddd; font-size: 14px; box-sizing: border-box; outline:none; }
        body.dark-mode input, body.dark-mode select { background: #2d2d2d; color: white; border-color: #444; }
        
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }
        button:active { transform: scale(0.96); opacity: 0.8; }
        
        .nav-bar { display: grid; grid-template-columns: 1fr 1.3fr 1fr; gap: 5px; margin-bottom: 15px; }
        .nav-bar button { font-size: 10px; padding: 12px 2px; background: var(--dark); }

        /* ZONE TECHNIQUE STYLES */
        .technique-card { border-left: 10px solid var(--primary); position: relative; transition: 0.3s; }
        .badge-urgent { background: var(--danger); color: white; padding: 3px 8px; border-radius: 8px; font-size: 10px; font-weight: bold; animation: pulse 1.5s infinite; }
        
        @keyframes pulse { 0% { opacity: 1; transform: scale(1); } 50% { opacity: 0.6; transform: scale(1.05); } 100% { opacity: 1; transform: scale(1); } }
        
        .hidden { display: none !important; }
        .suggestion-box { position:absolute; background:white; width:100%; z-index:100; border-radius:0 0 10px 10px; box-shadow:0 5px 15px rgba(0,0,0,0.1); max-height:150px; overflow-y:auto; color: #333; }

        .theme-toggle { position: fixed; bottom: 20px; right: 20px; width: 50px; height: 50px; border-radius: 50%; background: var(--primary); color: white; display: flex; align-items: center; justify-content: center; font-size: 20px; cursor: pointer; box-shadow: 0 4px 10px rgba(0,0,0,0.3); z-index: 1000; }

        @media print { body * { visibility: hidden; } #ticket-print, #ticket-print * { visibility: visible; } #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; font-size: 12px; } }
    </style>
</head>
<body>

<div class="theme-toggle" onclick="toggleTheme()">🌓</div>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:20px;">
    <div class="logo-box">SC</div>
    <h1 style="color:var(--primary); margin: 10px 0;">SUPER CLEAN</h1>
    <p style="opacity: 0.7;">Gestion de Pressing Pro v10.0</p>
    <div style="width: 100%; max-width: 320px; margin-top: 20px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); margin-top:15px; height:55px; font-size: 16px;">SE CONNECTER</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="header">
        <div class="logo-box">SC</div>
        <h2 style="margin:5px; color:var(--primary);">SUPER CLEAN</h2>
        <p id="user-info" style="font-size:11px; margin:0; opacity:0.8; font-weight: bold;"></p>
        <button onclick="location.reload()" style="width:auto; padding:5px 10px; font-size:9px; background:var(--danger); position:absolute; top:10px; right:10px; border-radius: 5px;">DECONNEXION</button>
    </div>

    <div class="nav-bar">
        <button onclick="tab('section-reception')" id="btn-nav-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-nav-atelier">👕 ZONE TECHNIQUE</button>
        <button onclick="tab('section-params')" id="btn-nav-params" class="hidden">⚙️ ADMIN</button>
    </div>

    <div id="section-reception" class="card hidden tab-content">
        <h4 style="margin-top: 0;">📥 Nouveau Dépôt</h4>
        <div style="position:relative;">
            <input type="tel" id="tel-client" placeholder="N° Téléphone (Recherche...)" onkeyup="rechercherClient()" autocomplete="off">
            <div id="suggestions-client" class="suggestion-box hidden"></div>
        </div>
        <input type="text" id="client" placeholder="Nom du Client">
        <div id="info-fidelite" class="hidden" style="font-size:12px; color:var(--warning); font-weight: bold; padding: 5px 0;"></div>
        <input type="date" id="date-livraison">
        
        <div style="background:rgba(0,0,0,0.04); padding:10px; border-radius:12px; margin-top:10px;">
            <select id="select-article"></select>
            <input type="number" id="qte" value="1" min="1">
            
            <div style="background:#fff5f5; padding:8px; border-radius:8px; border:1px dashed var(--danger); margin:8px 0;">
                <label style="font-size:10px; font-weight:bold; color:var(--danger);">📸 PHOTO PREUVE (Optionnel) :</label>
                <input type="file" id="photo-vêtement" accept="image/*" capture="environment" style="font-size:10px;">
                <div id="preview-photo" class="hidden"><img id="img-mini" src="" style="width:60px; height:60px; object-fit:cover; margin-top:5px; border-radius:8px;"></div>
            </div>
            
            <button onclick="ajouterAuPanier()" style="background:var(--accent)">➕ AJOUTER L'ARTICLE</button>
        </div>

        <div id="liste-panier" style="margin:15px 0; border-top: 1px solid #eee; padding-top: 10px;"></div>
        
        <div style="background:rgba(30,55,153,0.08); padding:12px; border-radius:12px; margin-bottom:10px;">
            <div style="display:flex; align-items:center; gap:10px;">
                <label style="font-size:12px; font-weight:bold;">🎁 REMISE (F) :</label>
                <input type="number" id="remise-montant" value="0" oninput="calculerTotalFinal()" style="margin:0;">
            </div>
            <div style="margin-top:10px; display:flex; align-items:center; gap:10px; color:var(--danger);">
                <input type="checkbox" id="is-urgent" style="width:22px; height:22px;"> <b style="font-size: 14px;">🚨 COMMANDE URGENTE</b>
            </div>
        </div>

        <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; text-align:right;">
            <small>BRUT : <span id="total-brut">0</span> F</small><br>
            NET À PAYER : <span id="total-panier" style="font-size:26px; font-weight:bold; color: var(--success);">0</span> F
        </div>

        <div style="margin: 15px 0; font-weight: bold;">
            <input type="checkbox" id="paye-avance"> <label for="paye-avance">💰 Déjà Payé</label>
        </div>

        <button onclick="validerCommande()" style="background:var(--success); height:60px; font-size:18px;">✅ VALIDER ET IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div class="card" style="padding: 10px;">
            <input type="text" id="search-task" placeholder="🔍 Rechercher (Nom, Tel, Ticket...)" onkeyup="afficherZoneTechnique()">
        </div>
        <div id="technique-grid" style="display: grid; gap: 15px;"></div>
    </div>

    <div id="section-params" class="card hidden tab-content">
        <h3>📊 Gestion Financière</h3>
        <div id="dashboard" style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
            <div style="background: var(--success); color: white; padding: 15px; border-radius: 12px;">
                <small>NET JOURNÉE</small>
                <div id="stats-jour" style="font-size:20px; font-weight:bold;">0 F</div>
            </div>
            <div style="background: var(--danger); color: white; padding: 15px; border-radius: 12px;">
                <small>IMPAYÉS</small>
                <div id="stats-impayes" style="font-size:20px; font-weight:bold;">0 F</div>
            </div>
        </div>

        <div style="background:#fff3e0; padding:15px; border-radius:12px; margin-top:20px; border:1px solid #ffb74d;">
            <h4 style="margin:0; color:#e65100;">💸 SORTIE DE CAISSE</h4>
            <input type="text" id="exp-motif" placeholder="Raison de la dépense">
            <input type="number" id="exp-montant" placeholder="Montant">
            <button onclick="ajouterDepense()" style="background:#e65100; margin-top: 10px;">ENREGISTRER</button>
        </div>

        <button onclick="genererRapportJour()" style="background:var(--secondary); margin-top: 20px;">📋 GÉNÉRER RAPPORT DU JOUR</button>
        <div id="zone-rapport" class="hidden" style="background:#fff; color:#000; border:1px solid #ddd; padding:15px; margin-top:15px; font-family:monospace;">
            <div id="contenu-rapport"></div>
            <button onclick="window.print()">🖨️ IMPRIMER RAPPORT</button>
        </div>

        <hr style="margin: 20px 0;">
        <div style="background:var(--light); padding:15px; border-radius:15px; border:2px dashed var(--primary); color: #333;">
            <h4 style="margin:0;">☁️ SAUVEGARDE CLOUD</h4>
            <p style="font-size: 10px;">Exportez vos données pour Drive / iCloud</p>
            <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:10px;">
                <button onclick="exporterDonnees()">📤 EXPORTER</button>
                <button onclick="document.getElementById('import-file').click()" style="background:var(--dark)">📥 IMPORTER</button>
            </div>
            <input type="file" id="import-file" class="hidden" onchange="importerDonnees(event)">
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; padding:10px; color:#000;">
    <center>
        <h2 style="margin:0;">SUPER CLEAN</h2>
        <p style="font-size:10px;">📍 Nkounga, Immeuble de Karche<br>📞 675 864 103</p>
    </center>
    <hr style="border:0.5px dashed #000">
    <p><b>N° TICKET: #<span id="t-id"></span></b><br>Client: <span id="t-client"></span><br>Tél: <span id="t-tel"></span></p>
    <p>Le: <span id="t-enreg"></span><br><b>RETRAIT PRÉVU: <span id="t-date"></span></b></p>
    <div id="t-items" style="border-top:1px solid #000; padding-top:5px;"></div>
    <p id="t-remise" style="text-align:right; margin:5px 0;"></p>
    <div style="text-align:right; font-weight:bold; font-size:18px;">TOTAL NET: <span id="t-total"></span> F</div>
    <p id="t-paye" style="text-align:center; border:1px solid #000; padding:8px; margin-top:10px; font-weight: bold;"></p>
    <center><p style="font-size:9px;">Merci ! Les vêtements non récupérés après 3 mois seront offerts aux orphelinats.</p></center>
</div>

<script>
    // --- DATA & CONFIG ---
    const GRILLE_PRIX = {
        "HAUTS": { "T-Shirt": 500, "Chemise": 500, "Veste 2p": 1500, "Pull": 700, "Blouson": 1500, "Costume 3p": 2500 },
        "BAS": { "Pantalon": 500, "Short": 400, "Jupe": 500, "Robe simple": 1000, "Robe Soirée": 2500 },
        "LIT": { "Drap": 1000, "Couette": 3000, "Couverture": 2500, "Rideau": 1500, "Nappe": 1000 }
    };

    let EMPLOYES = JSON.parse(localStorage.getItem('sc_staff')) || {"admin": { pass: "0000", role: "ADMIN", nom: "Manager" }};
    let commandes = JSON.parse(localStorage.getItem('sc_data')) || [];
    let depenses = JSON.parse(localStorage.getItem('sc_expenses')) || [];
    let panier = [];
    let user = null;
    let imageTemp = null;

    // --- LOGIQUE AUTH ---
    function login() {
        const id = document.getElementById('u-login').value.toLowerCase().trim();
        const ps = document.getElementById('u-pass').value;
        if(EMPLOYES[id] && EMPLOYES[id].pass === ps) {
            user = EMPLOYES[id];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-info').innerText = `Opérateur : ${user.nom} (${user.role})`;
            if(user.role === "ADMIN") document.getElementById('btn-nav-params').classList.remove('hidden');
            initSystem();
        } else alert("Erreur d'accès !");
    }

    function initSystem() {
        if (localStorage.getItem('sc_theme') === 'dark') document.body.classList.add('dark-mode');
        remplirArticles();
        afficherZoneTechnique();
        tab(user.role === "ADMIN" ? 'section-reception' : 'section-atelier');
    }

    function tab(id) {
        document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
        document.getElementById(id).classList.remove('hidden');
        if(id === 'section-params') mettreAJourStats();
    }

    function toggleTheme() {
        document.body.classList.toggle('dark-mode');
        localStorage.setItem('sc_theme', document.body.classList.contains('dark-mode') ? 'dark' : 'light');
    }

    // --- CAISSE LOGIQUE ---
    function remplirArticles() {
        const s = document.getElementById('select-article');
        s.innerHTML = "";
        for(let cat in GRILLE_PRIX) {
            let g = document.createElement('optgroup'); g.label = cat;
            for(let art in GRILLE_PRIX[cat]) {
                let o = document.createElement('option'); o.value = GRILLE_PRIX[cat][art];
                o.innerText = `${art} (${GRILLE_PRIX[cat][art]} F)`;
                g.appendChild(o);
            }
            s.appendChild(g);
        }
    }

    document.getElementById('photo-vêtement').addEventListener('change', function(e) {
        const file = e.target.files[0];
        if (file) {
            const reader = new FileReader();
            reader.onload = (ev) => { 
                imageTemp = ev.target.result; 
                document.getElementById('img-mini').src = imageTemp;
                document.getElementById('preview-photo').classList.remove('hidden');
            };
            reader.readAsDataURL(file);
        }
    });

    function rechercherClient() {
        const val = document.getElementById('tel-client').value;
        const list = document.getElementById('suggestions-client');
        if (val.length < 3) { list.classList.add('hidden'); return; }
        
        const uniques = []; const map = new Map();
        for (const c of commandes) { if(!map.has(c.tel)){ map.set(c.tel, true); uniques.push(c); } }
        
        const filtres = uniques.filter(c => c.tel.includes(val));
        if (filtres.length > 0) {
            list.innerHTML = filtres.map(c => `<div onclick="setProfil('${c.client}', '${c.tel}')" style="padding:12px; border-bottom:1px solid #eee;">📱 ${c.tel} - ${c.client}</div>`).join('');
            list.classList.remove('hidden');
        } else list.classList.add('hidden');
    }

    function setProfil(nom, tel) {
        document.getElementById('client').value = nom;
        document.getElementById('tel-client').value = tel;
        document.getElementById('suggestions-client').classList.add('hidden');
        
        const depenseTotale = commandes.filter(c => c.tel === tel && c.status === 'Livré').reduce((s,c) => s+c.total, 0);
        const nbFidelite = commandes.filter(c => c.tel === tel && c.typeRemise === 'FIDELITE').length;
        const points = Math.max(0, Math.floor(depenseTotale/1000) - (nbFidelite*10));
        
        const info = document.getElementById('info-fidelite');
        info.innerHTML = `⭐ Fidélité : ${points} points (10 pts = -500 F)`;
        info.classList.remove('hidden');
        
        if(points >= 10 && confirm("Appliquer le bonus Fidélité de 500 F ?")) {
            document.getElementById('remise-montant').value = 500;
            document.getElementById('remise-montant').dataset.type = "FIDELITE";
            calculerTotalFinal();
        }
    }

    function ajouterAuPanier() {
        const s = document.getElementById('select-article');
        const q = parseInt(document.getElementById('qte').value);
        panier.push({ 
            nom: s.options[s.selectedIndex].text.split(' (')[0], 
            prix: parseFloat(s.value), 
            qte: q, 
            st: parseFloat(s.value) * q,
            photo: imageTemp 
        });
        imageTemp = null;
        document.getElementById('preview-photo').classList.add('hidden');
        renderPanier();
    }

    function renderPanier() {
        let brut = 0;
        document.getElementById('liste-panier').innerHTML = panier.map((i, idx) => {
            brut += i.st;
            return `<div style="display:flex; justify-content:space-between; margin:6px 0; border-bottom:1px solid rgba(0,0,0,0.05); padding-bottom:5px;">
                <span>${i.qte}x ${i.nom} ${i.photo ? '📸' : ''}</span>
                <span style="font-weight:bold;">${i.st} F <span onclick="panier.splice(${idx},1);renderPanier()" style="color:red; margin-left:10px;">✕</span></span>
            </div>`;
        }).join('');
        document.getElementById('total-brut').innerText = brut;
        calculerTotalFinal();
    }

    function calculerTotalFinal() {
        const b = parseFloat(document.getElementById('total-brut').innerText) || 0;
        const r = parseFloat(document.getElementById('remise-montant').value) || 0;
        document.getElementById('total-panier').innerText = Math.max(0, b - r);
    }

    function validerCommande() {
        const cmd = {
            id: Date.now().toString().slice(-5),
            client: document.getElementById('client').value,
            tel: document.getElementById('tel-client').value,
            date: document.getElementById('date-livraison').value,
            dateEnreg: new Date().toISOString().split('T')[0],
            items: [...panier],
            total: parseFloat(document.getElementById('total-panier').innerText),
            remise: parseFloat(document.getElementById('remise-montant').value) || 0,
            typeRemise: document.getElementById('remise-montant').dataset.type || "NORMALE",
            status: 'En attente',
            urgent: document.getElementById('is-urgent').checked,
            paye: document.getElementById('paye-avance').checked,
            caissier: user.nom
        };
        if(!cmd.client || !cmd.tel || panier.length === 0) return alert("Infos manquantes !");
        
        commandes.unshift(cmd);
        localStorage.setItem('sc_data', JSON.stringify(commandes));
        imprimerTicket(cmd);
        location.reload();
    }

    // --- ZONE TECHNIQUE LOGIQUE ---
    function afficherZoneTechnique() {
        const grid = document.getElementById('technique-grid');
        const q = document.getElementById('search-task').value.toLowerCase();
        grid.innerHTML = "";
        
        const filtrés = commandes.filter(c => c.status !== 'Livré' && (c.id.includes(q) || c.client.toLowerCase().includes(q) || c.tel.includes(q)));
        
        filtrés.forEach(c => {
            const etapes = ["En attente", "Lavage", "Repassage", "Prêt"];
            const color = ["#e74c3c", "#3498db", "#f1c40f", "#2ecc71"][etapes.indexOf(c.status)];
            const urgentStyle = c.urgent ? "border: 2px solid var(--danger); background: #fffdfd;" : "";

            let itemsHtml = "";
            c.items.forEach((item, idx) => {
                for(let i=1; i<=item.qte; i++) {
                    itemsHtml += `<div style="display:flex; align-items:center; padding:10px; background:white; border:1px solid #eee; border-radius:10px; margin-top:5px;">
                        <input type="checkbox" id="item-${c.id}-${idx}-${i}" style="width:24px; height:24px; margin-right:12px;">
                        <span style="flex:1; font-size:14px; font-weight:600;">${item.nom} <small style="color:gray;">(#${c.id}-${i})</small></span>
                        ${item.photo ? `<img src="${item.photo}" onclick="alert('SIGNALEMENT TACHE SUR CE VÊTEMENT')" style="width:40px; height:40px; border-radius:6px; object-fit:cover; border:2px solid var(--danger);">` : ''}
                    </div>`;
                }
            });

            grid.innerHTML += `
            <div class="card technique-card" style="border-left-color:${color}; ${urgentStyle}">
                <div style="display:flex; justify-content:space-between;">
                    <div>
                        <b style="font-size:17px;">${c.client.toUpperCase()}</b><br>
                        <small>Ticket #${c.id} | Prévu: <b>${new Date(c.date).toLocaleDateString()}</b></small>
                    </div>
                    ${c.urgent ? '<span class="badge-urgent">🚨 EXPRESS</span>' : ''}
                </div>
                <div style="margin:12px 0;">${itemsHtml}</div>
                <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px;">
                    <button onclick="progression('${c.id}')" style="background:${color}; font-size:12px;">➡️ ÉTAPE SUIVANTE</button>
                    <button onclick="livrer('${c.id}')" style="background:var(--dark); font-size:12px;">📦 LIVRER</button>
                </div>
                ${c.status === 'Prêt' ? `<button onclick="window.open('https://wa.me/${c.tel}?text=Bonjour ${c.client}, votre vêtement est prêt chez Super Clean !','_blank')" style="background:#25D366; margin-top:10px;">📱 WHATSAPP</button>` : ''}
            </div>`;
        });
    }

    function progression(id) {
        const i = commandes.findIndex(c => c.id === id);
        const etapes = ["En attente", "Lavage", "Repassage", "Prêt"];
        let actuel = etapes.indexOf(commandes[i].status);
        if(actuel < 3) {
            commandes[i].status = etapes[actuel+1];
            commandes[i].staff = user.nom;
            localStorage.setItem('sc_data', JSON.stringify(commandes));
            afficherZoneTechnique();
        }
    }

    function livrer(id) {
        const i = commandes.findIndex(c => c.id === id);
        if(!commandes[i].paye && !confirm("⚠️ IMPAYÉ ! Livrer quand même ?")) return;
        
        const checks = document.querySelectorAll(`[id^="item-${id}"]`);
        if(!Array.from(checks).every(cb => cb.checked) && !confirm("Toutes les pièces ne sont pas cochées. Continuer ?")) return;

        commandes[i].status = 'Livré';
        commandes[i].dateLivraisonEffectuee = new Date().toISOString().split('T')[0];
        localStorage.setItem('sc_data', JSON.stringify(commandes));
        afficherZoneTechnique();
        alert("Livraison validée !");
    }

    // --- ADMIN LOGIQUE ---
    function ajouterDepense() {
        const m = document.getElementById('exp-motif').value;
        const amt = parseFloat(document.getElementById('exp-montant').value);
        if(!m || !amt) return alert("Champs vides !");
        depenses.push({ motif: m, montant: amt, date: new Date().toISOString().split('T')[0], user: user.nom });
        localStorage.setItem('sc_expenses', JSON.stringify(depenses));
        document.getElementById('exp-motif').value = ""; document.getElementById('exp-montant').value = "";
        mettreAJourStats();
        alert("Dépense notée !");
    }

    function mettreAJourStats() {
        const ajd = new Date().toISOString().split('T')[0];
        const encaissé = commandes.filter(c => c.dateEnreg === ajd && c.paye).reduce((a,b) => a+b.total, 0);
        const dettes = commandes.filter(c => !c.paye).reduce((a,b) => a+b.total, 0);
        const couts = depenses.filter(d => d.date === ajd).reduce((a,b) => a+b.montant, 0);

        document.getElementById('stats-jour').innerText = (encaissé - couts).toLocaleString() + " F";
        document.getElementById('stats-impayes').innerText = dettes.toLocaleString() + " F";
    }

    function genererRapportJour() {
        const ajd = new Date().toISOString().split('T')[0];
        const v = commandes.filter(c => c.dateEnreg === ajd);
        const d = depenses.filter(ex => ex.date === ajd);
        
        let html = `<h2 style="text-align:center">RAPPORT DU ${new Date().toLocaleDateString()}</h2>`;
        html += `<table style="width:100%; text-align:left; border-collapse:collapse;">
            <tr style="background:#eee;"><th>Client</th><th>Total</th><th>Payé</th></tr>`;
        
        let totalRecu = 0;
        v.forEach(c => {
            html += `<tr style="border-bottom:1px solid #ddd;"><td>${c.client}</td><td>${c.total}F</td><td>${c.paye?'OUI':'NON'}</td></tr>`;
            if(c.paye) totalRecu += c.total;
        });
        
        let totalOut = d.reduce((a,b) => a+b.montant, 0);
        html += `</table><div style="margin-top:20px; font-weight:bold;">
            <p>RECETTES : ${totalRecu} F</p>
            <p>DÉPENSES : ${totalOut} F</p>
            <p style="font-size:18px; color:green;">BILAN NET : ${totalRecu - totalOut} F</p>
        </div>`;
        
        document.getElementById('contenu-rapport').innerHTML = html;
        document.getElementById('zone-rapport').classList.remove('hidden');
    }

    function exporterDonnees() {
        const data = { cmd: commandes, exp: depenses, staff: EMPLOYES };
        const blob = new Blob([JSON.stringify(data)], {type:'application/json'});
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob);
        a.download = `SUPER_CLEAN_BACKUP.json`;
        a.click();
    }

    function importerDonnees(e) {
        const reader = new FileReader();
        reader.onload = (ev) => {
            const data = JSON.parse(ev.target.result);
            localStorage.setItem('sc_data', JSON.stringify(data.cmd || []));
            localStorage.setItem('sc_expenses', JSON.stringify(data.exp || []));
            localStorage.setItem('sc_staff', JSON.stringify(data.staff || {}));
            location.reload();
        };
        reader.readAsText(e.target.files[0]);
    }

    function imprimerTicket(cmd) {
        document.getElementById('t-id').innerText = cmd.id;
        document.getElementById('t-client').innerText = cmd.client;
        document.getElementById('t-tel').innerText = cmd.tel;
        document.getElementById('t-enreg').innerText = new Date().toLocaleDateString();
        document.getElementById('t-date').innerText = new Date(cmd.date).toLocaleDateString();
        document.getElementById('t-items').innerHTML = cmd.items.map(i => `<div style="display:flex; justify-content:space-between;"><span>${i.qte}x ${i.nom}</span><span>${i.st} F</span></div>`).join('');
        document.getElementById('t-remise').innerText = cmd.remise > 0 ? `Remise: -${cmd.remise} F` : "";
        document.getElementById('t-total').innerText = cmd.total;
        document.getElementById('t-paye').innerText = cmd.paye ? "--- ✅ PAYÉ ---" : "--- ⚠️ SOLDE À PAYER ---";
        if(cmd.urgent) document.getElementById('t-paye').innerHTML += "<br><span style='color:red;'>🚨 COMMANDE EXPRESS 🚨</span>";
        window.print();
    }
</script>
</body>
</html>
