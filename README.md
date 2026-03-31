<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SUPER CLEAN PRESSING - GESTION PRO v18.0</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f8f9fa; --whatsapp: #25D366; --radius: 16px;
            --shadow: 0 8px 30px rgba(0,0,0,0.08);
        }
        body { font-family: 'Inter', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); overflow-x: hidden; }
        .hidden { display: none !important; }
        .card { background: white; padding: 20px; border-radius: var(--radius); box-shadow: var(--shadow); margin-bottom: 20px; border: 1px solid rgba(0,0,0,0.03); position: relative; }
        
        /* Boutons et Inputs Modernes */
        input, select, button { width: 100%; padding: 14px; margin: 8px 0; border-radius: 12px; border: 2px solid #eee; font-size: 15px; outline: none; box-sizing: border-box; transition: 0.2s; }
        button { background: var(--primary); color: white; border: none; font-weight: 600; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }
        button:active { transform: scale(0.97); }
        
        /* Navigation */
        .nav-bar { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .nav-bar button { background: white; color: var(--primary); border: 1px solid var(--primary); font-size: 11px; box-shadow: none; height: 50px;}
        .nav-bar button.active { background: var(--primary); color: white; }
        
        /* Barre de Progression Atelier */
        .progress-container { background: #eee; height: 8px; border-radius: 10px; margin: 10px 0; overflow: hidden; position: relative; }
        .progress-fill { background: var(--success); height: 100%; transition: width 0.5s ease; width: 0%; }
        
        /* Alertes Retard */
        .badge-late { background: var(--danger); color: white; padding: 3px 8px; border-radius: 5px; font-size: 10px; animation: flash 1s infinite; font-weight: 800; display: inline-block; }
        @keyframes flash { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }
        
        /* Logo Styles */
        .app-logo { max-width: 150px; height: auto; display: block; margin: 0 auto 15px auto; }
        .ticket-logo { max-width: 70mm; height: auto; display: block; margin: 0 auto 5px auto; }
        
        /* Panier Express */
        .panier-item { display:flex; justify-content:space-between; align-items:center; font-size:12px; background:white; padding:10px; border-radius:10px; margin-bottom:5px; border: 1px solid #eee; }
        .panier-delete { color:red; margin-left:10px; cursor:pointer; font-weight:bold; }

        @media print { 
            body * { visibility: hidden; } 
            #ticket-print, #ticket-print * { visibility: visible; } 
            #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; padding: 5mm; background: white !important; color: black !important; } 
        }
    </style>
</head>
<body>

<div id="loader-container" class="hidden" style="position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(255,255,255,0.9); z-index:10000; display:flex; align-items:center; justify-content:center;">⚡ Traitement...</div>

<div id="auth-screen" style="height:90vh; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:20px;">
    <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" alt="Super Clean Logo" class="app-logo">
    <h2 style="color:var(--primary); margin:0;">SUPER CLEAN PRO</h2>
    <p style="font-size:12px; opacity:0.6; margin-bottom:20px;">Le bien-être de vos vêtements...</p>
    <div style="width: 100%; max-width: 320px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); height:55px; font-size:16px;">SE CONNECTER</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="card" style="text-align:center; border-top: 5px solid var(--primary); padding-bottom: 10px;">
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" alt="Logo" class="app-logo" style="max-width:80px; margin-bottom: 5px;">
        <p id="user-info" style="font-size:11px; background:var(--light); padding:5px; border-radius:20px; display:inline-block; font-weight:bold;"></p>
        <button onclick="location.reload()" style="width:auto; height:25px; padding:0 10px; background:var(--danger); font-size:10px; margin: 10px auto 0 auto;">Quitter</button>
    </div>

    <nav class="nav-bar">
        <button onclick="tab('section-reception')" id="btn-nav-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-nav-atelier">👕 ATELIER</button>
        <button onclick="tab('section-admin')" id="btn-nav-admin">👑 ADMIN</button>
    </nav>

    <div id="section-reception" class="card hidden tab-content">
        <input type="tel" id="tel-client" placeholder="N° WhatsApp (Cameroun, ex: 699...)" onkeyup="rechercherClient()">
        <input type="text" id="client" placeholder="Nom complet du client">
        
        <div style="background:var(--light); padding:10px; border-radius:12px; margin-top:10px; border: 1px solid #ddd;">
            <select id="select-article"></select>
            <div style="display:flex; gap:10px; align-items:center;">
                <input type="number" id="qte" value="1" min="1" style="flex:1">
                <button onclick="ajouterAuPanier()" style="background:var(--accent); flex:2; height:48px;">➕ AJOUTER</button>
            </div>
        </div>

        <div id="liste-panier" style="margin:15px 0;"></div>
        
        <div style="background:var(--dark); color:white; padding:15px; border-radius:12px; text-align:right;">
            TOTAL : <span id="total-panier" style="font-size:20px; color:var(--success); font-weight:800;">0 F</span>
        </div>

        <div style="margin-top:10px; background:rgba(0,0,0,0.03); padding:15px; border-radius:12px;">
            <input type="number" id="montant-avance" placeholder="Avance versée (F)" oninput="calculerReste()">
            <div id="reste-display" style="text-align:right; font-weight:bold; color:var(--danger); margin-top:5px;">Reste à percevoir : 0 F</div>
        </div>

        <button onclick="validerCommande()" style="background:var(--success); height:60px; margin-top:15px; font-size:16px;">✅ ENCAISSER & IMPRIMER TICKET</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <div id="alert-global-retard" class="hidden" style="background:var(--danger); color:white; padding:12px; border-radius:12px; text-align:center; font-weight:bold; margin-bottom:15px; font-size:12px;">
            🚨 ATTENTION : <span id="count-retard">0</span> VÊTEMENTS EN RETARD !
        </div>
        <input type="text" id="search-task" placeholder="Rechercher ticket ou client..." onkeyup="afficherZoneTechnique()" style="margin-bottom:15px;">
        <div id="technique-grid"></div>
    </div>

    <div id="section-admin" class="card hidden tab-content">
        <h3>📊 Tableau de Bord</h3>
        <div style="display:grid; grid-template-columns: 1fr 1fr; gap:10px;">
            <div style="padding:15px; border-radius:12px; background:var(--primary); color:white; text-align:center;">
                <small>ENCAISSÉ (AVANCES)</small><br><b id="stat-encaissé" style="font-size:20px;">0 F</b>
            </div>
            <div style="padding:15px; border-radius:12px; background:var(--danger); color:white; text-align:center;">
                <small>FRAIS DE FONCTIONNEMENT</small><br><b id="stat-frais" style="font-size:20px;">0 F</b>
            </div>
        </div>

        <div class="card" style="margin-top:15px;">
            <h4>👥 GESTION DU PERSONNEL</h4>
            <div style="background: var(--light); padding: 10px; border-radius: 12px; margin-bottom: 10px;">
                <input type="text" id="new-staff-name" placeholder="Nom complet">
                <input type="text" id="new-staff-login" placeholder="Identifiant (Login)">
                <input type="password" id="new-staff-pass" placeholder="Mot de passe">
                <select id="new-staff-role">
                    <option value="CAISSE">📥 CAISSE</option>
                    <option value="TECH">👕 TECHNIQUE (ATELIER)</option>
                    <option value="ADMIN">👑 ADMIN</option>
                </select>
                <button onclick="ajouterPersonnel()" style="background:var(--success); height:40px;">➕ CRÉER COMPTE</button>
            </div>
            <div id="staff-list-detail"></div>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none; font-family:monospace; padding:10px;">
    <center>
        <img src="https://i.postimg.cc/kMj5ffyz/Whats-App-Image-2025-07-02-a-05-55-51-f9e5b9e3.jpg" alt="Logo Ticket" class="ticket-logo">
        <small>📍 Nkounga Immeuble de Karche</small><br>
        <small>📞 675 864 103 / 692 315 267</small>
    </center>
    <hr style="border: 1px dashed black;">
    <p><b>Ticket #<span id="t-id"></span></b><br>Date: <span id="t-date"></span><br>Client: <span id="t-client"></span></p>
    <hr style="border: 1px dashed black;">
    <div id="t-items"></div>
    <hr style="border: 1px dashed black;">
    <div style="text-align:right; font-size:13px;">
        TOTAL : <span id="t-total"></span> F<br>
        PAYÉ : <span id="t-avance"></span> F<br>
        <b>RESTE : <span id="t-reste"></span> F</b>
    </div>
    <hr style="border: 1px dashed black;">
    <center><small>Aucun vêtement n'est retiré sans ticket.<br>Merci de votre confiance !</small></center>
</div>

<script>
    // 1. DONNÉES DE BASE (TARIFS & EMPLOYÉS DÉFAUT)
    const TARIFS = {
        "HAUTS": {"T-Shirt":500,"Chemise":500,"Polo":500,"Veste":1500,"Pull Over":700,"Blouson":1000},
        "BAS": {"Pantalon":500,"Jupe":500,"Short":500},
        "ROBES": {"Robe simple":700,"Robe soirée":3000,"Kaba":1000},
        "LIT": {"1 Drap":700,"Couette":2500,"Couverture":3000}
    };

    let staff_list = JSON.parse(localStorage.getItem('superclean_staff')) || {
        "admin": {nom:"Directeur", pass:"0000", role:"ADMIN"},
        "tech": {nom:"Laveur", pass:"1111", role:"TECH"}
    };
    let commandes = JSON.parse(localStorage.getItem('superclean_data')) || [];
    let depenses = JSON.parse(localStorage.getItem('superclean_dep')) || [];
    let logs = JSON.parse(localStorage.getItem('superclean_logs')) || [];
    let panier = [], current_user = null;

    // 2. AUTHENTIFICATION & NAVIGATION
    function login() {
        const id = document.getElementById('u-login').value.toLowerCase().trim();
        const ps = document.getElementById('u-pass').value;
        if(staff_list[id] && staff_list[id].pass === ps) {
            current_user = staff_list[id];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            document.getElementById('user-info').innerText = `${current_user.nom} [${current_user.role}]`;
            
            // Masquage selon rôle
            if(current_user.role === 'TECH') {
                document.getElementById('btn-nav-caisse').classList.add('hidden');
                document.getElementById('btn-nav-admin').classList.add('hidden');
                tab('section-atelier');
            } else if(current_user.role === 'CAISSE') {
                document.getElementById('btn-nav-admin').classList.add('hidden');
                tab('section-reception');
            } else {
                tab('section-reception');
            }
            remplirSelectTarifs();
        } else { alert("Identifiants incorrects, réessayez."); }
    }

    function tab(id) {
        // Sécurité Navigation
        if(id === 'section-reception' && current_user.role === 'TECH') return;
        if(id === 'section-admin' && current_user.role !== 'ADMIN') return;

        document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
        document.querySelectorAll('.nav-bar button').forEach(b => b.classList.remove('active'));
        document.getElementById(id).classList.remove('hidden');
        const btnId = id === 'section-reception' ? 'btn-nav-caisse' : (id === 'section-atelier' ? 'btn-nav-atelier' : 'btn-nav-admin');
        document.getElementById(btnId).classList.add('active');
        if(id === 'section-atelier') zoneTechniqueProgressive();
        if(id === 'section-admin') { afficherPersonnel(); calculerStats(); }
    }

    // 3. LOGIQUE CAISSE
    function remplirSelectTarifs() {
        const s = document.getElementById('select-article');
        s.innerHTML = "";
        for(let cat in TARIFS) {
            let g = document.createElement('optgroup'); g.label = cat;
            for(let a in TARIFS[cat]) { let o = document.createElement('option'); o.value = TARIFS[cat][a]; o.innerText = `${a} (${TARIFS[cat][a]}F)`; g.appendChild(o); }
            s.appendChild(g);
        }
    }

    function ajouterAuPanier() {
        const s = document.getElementById('select-article');
        const q = parseInt(document.getElementById('qte').value);
        const nom = s.options[s.selectedIndex].text.split(' (')[0];
        const prix = parseFloat(s.value);
        panier.push({nom, qte: q, prix: prix, st: prix*q, pret: false});
        renderPanier();
    }

    function renderPanier() {
        const t = panier.reduce((a,b) => a+b.st, 0);
        document.getElementById('total-panier').innerText = t.toLocaleString() + " F";
        document.getElementById('liste-panier').innerHTML = panier.map((p,i) => `
            <div class="panier-item">
                <span>${p.qte}x ${p.nom}</span>
                <b>${p.st} F <span onclick="panier.splice(${i},1);renderPanier()" class="panier-delete">✕</span></b>
            </div>`).join('');
        calculerReste();
    }

    function calculerReste() {
        const t = panier.reduce((a,b) => a+b.st, 0);
        const av = parseFloat(document.getElementById('montant-avance').value) || 0;
        document.getElementById('reste-display').innerText = `Reste à percevoir : ${(t - av).toLocaleString()} F`;
    }

    function validerCommande() {
        if(!document.getElementById('client').value || panier.length === 0) return alert("Nom du client ou panier vide !");
        document.getElementById('loader-container').classList.remove('hidden');
        
        setTimeout(() => {
            const tot = panier.reduce((a,b) => a+b.st, 0);
            const av = parseFloat(document.getElementById('montant-avance').value) || 0;
            const cmd = {
                id: Math.floor(1000 + Math.random()*9000), client: document.getElementById('client').value,
                tel: document.getElementById('tel-client').value, items: [...panier], total: tot,
                avance: av, reste: tot-av, status: "En attente", date: new Date().toLocaleDateString('fr-FR'),
                dateObj: new Date().toISOString().split('T')[0] // Pour les retards
            };
            commandes.unshift(cmd);
            localStorage.setItem('superclean_data', JSON.stringify(commandes));
            imprimerTicket(cmd);
            enregistrerLog(`Caisse: Nouveau ticket #${cmd.id}`);
            document.getElementById('loader-container').classList.add('hidden');
            location.reload(); // Refresh pour reset
        }, 800);
    }

    // 4. LOGIQUE ATELIER (VÊTEMENT PAR VÊTEMENT)
    function zoneTechniqueProgressive() {
        const q = document.getElementById('search-task').value.toLowerCase();
        const grid = document.getElementById('technique-grid');
        grid.innerHTML = "";
        const today = new Date().toISOString().split('T')[0];

        let retardGlobal = 0;

        commandes.filter(c => c.status !== "Livré").forEach(c => {
            if(c.client.toLowerCase().includes(q) || c.id.toString().includes(q)) {
                // Vérif Retard
                const estRetard = (c.dateObj < today && c.status !== "Prêt");
                if(estRetard) retardGlobal++;

                const articlesFini = c.items.filter(item => item.pret).length;
                const totalArticles = c.items.length;
                const pourcent = Math.round((articlesFini / totalArticles) * 100);

                // articles restants
                let htmlArticles = c.items.filter(item => !item.pret).map(item => `
                    <div style="display:flex; justify-content:space-between; align-items:center; background:#f0f2f5; padding:8px; border-radius:10px; margin:5px 0; font-size:13px;">
                        <span>${item.qte}x <b>${item.nom}</b></span>
                        <button onclick="cloreVetement('${c.id}', '${item.nom}')" style="width:auto; padding:5px 12px; font-size:10px; background:var(--success); margin:0;">FINI</button>
                    </div>
                `).join('');

                // Si tout est fini, bouton Livraison
                let boutonFinal = "";
                if(pourcent === 100) {
                    boutonFinal = `<button onclick="livrer('${c.id}')" style="background:var(--dark); margin-top:10px;">📦 LIVRER (Reste: ${c.reste}F)</button>`;
                    htmlArticles = `<center style="color:var(--success); font-weight:bold; padding:10px;">✓ TOUT EST PRÊT</center>`;
                }

                grid.innerHTML += `
                    <div class="card" style="border-left: 6px solid ${estRetard ? 'var(--danger)' : (pourcent===100?'var(--success)':'var(--primary)')}">
                        <div style="display:flex; justify-content:space-between; align-items:center;">
                            <b>#${c.id} - ${c.client}</b>
                            <span style="color:var(--primary); font-weight:800; font-size:16px;">${pourcent}%</span>
                        </div>
                        ${estRetard ? `<div class="badge-late">⚠️ RETARD (depuis ${c.date})</div>` : `<small style="opacity:0.6;">Déposé le: ${c.date}</small>`}
                        <div class="progress-container"><div class="progress-fill" style="width:${pourcent}%"></div></div>
                        <div style="margin-top:10px;">${htmlArticles}</div>
                        ${boutonFinal}
                    </div>
                `;
            }
        });

        // Alerte Global Retard
        const alertBox = document.getElementById('alert-global-retard');
        if(retardGlobal > 0) {
            alertBox.classList.remove('hidden');
            document.getElementById('count-retard').innerText = retardGlobal;
        } else {
            alertBox.classList.add('hidden');
        }
    }

    function cloreVetement(tid, nomItem) {
        const c = commandes.find(x => x.id == tid);
        const item = c.items.find(i => i.nom === nomItem && !i.pret);
        if(item) {
            item.pret = true;
            localStorage.setItem('superclean_data', JSON.stringify(commandes));
            enregistrerLog(`Atelier: ${nomItem} terminé (#${tid})`);
            
            // Si c'est le dernier vêtement, on propose d'appeler/sms si Tel existe
            const articlesFini = c.items.filter(item => item.pret).length;
            if(articlesFini === c.items.length && c.tel) {
                if(confirm(`${c.client} : Tout est prêt. Notifier par WhatsApp ?`)) {
                    const msg = encodeURIComponent(`Bonjour ${c.client}, votre linge est prêt chez SUPER CLEAN (#${c.id}). Reste à payer: ${c.reste}F.`);
                    window.open(`https://wa.me/237${c.tel}?text=${msg}`);
                }
            }
            zoneTechniqueProgressive();
        }
    }

    function livrer(id) {
        const c = commandes.find(x => x.id == id);
        if(c.reste > 0) {
            if(!confirm(`Reste à payer : ${c.reste} F. Confirmer paiement du solde ?`)) return;
            c.avance += c.reste; c.reste = 0;
        }
        c.status = "Livré";
        localStorage.setItem('superclean_data', JSON.stringify(commandes));
        enregistrerLog(`Caisse: Commande #${id} livrée`);
        zoneTechniqueProgressive();
    }

    // 5. ADMIN & UTILS
    function calculerStats() {
        document.getElementById('stat-encaissé').innerText = commandes.reduce((a,b) => a + b.avance, 0).toLocaleString() + " F";
        document.getElementById('stat-frais').innerText = depenses.reduce((a,b) => a + b.montant, 0).toLocaleString() + " F";
    }

    function enregistrerLog(msg) {
        logs.unshift({date: new Date().toLocaleString(), auteur: current_user.nom, msg});
        if(logs.length > 50) logs.pop(); // Limite 50 logs
        localStorage.setItem('superclean_logs', JSON.stringify(logs));
    }

    function rechercherClient() {
        const t = document.getElementById('tel-client').value;
        if(t.length >= 8) {
            const f = commandes.find(c => c.tel.includes(t));
            if(f) document.getElementById('client').value = f.client;
        }
    }

    function imprimerTicket(c) {
        document.getElementById('t-id').innerText = c.id;
        document.getElementById('t-date').innerText = c.date;
        document.getElementById('t-client').innerText = c.client;
        document.getElementById('t-total').innerText = c.total.toLocaleString();
        document.getElementById('t-avance').innerText = c.avance.toLocaleString();
        document.getElementById('t-reste').innerText = c.reste.toLocaleString();
        document.getElementById('t-items').innerHTML = c.items.map(i => `
            <div style="display:flex; justify-content:space-between; font-size:12px; margin:2px 0;">
                <span>${i.qte}x ${i.nom}</span>
                <b>${i.st} F</b>
            </div>`).join('');
        window.print();
    }

    // Fonctions simplifiées pour le Personnel
    function afficherPersonnel() {
        document.getElementById('staff-list-detail').innerHTML = Object.keys(staff_list).map(k => `
            <div style="display:flex; justify-content:space-between; padding:8px; border-bottom:1px solid #eee;">
                <span>${staff_list[k].nom} (<b>${staff_list[k].role}</b>)</span>
                ${k !== 'admin' ? `<button onclick="deleteStaff('${k}')" style="width:auto; background:none; color:red; padding:0;">🗑️</button>` : '🔒'}
            </div>`).join('');
    }
    function ajouterPersonnel() {
        const n = document.getElementById('new-staff-name').value;
        const id = document.getElementById('new-staff-login').value.toLowerCase().trim();
        const ps = document.getElementById('new-staff-pass').value;
        const ro = document.getElementById('new-staff-role').value;
        if(!n || !id || !ps) return alert("Remplissez tout !");
        staff_list[id] = {nom: n, pass: ps, role: ro};
        localStorage.setItem('superclean_staff', JSON.stringify(staff_list));
        enregistrerLog(`Admin: Création compte personnel ${id}`);
        location.reload();
    }
    function deleteStaff(id) {
        if(confirm("Supprimer ce personnel ?")) {
            delete staff_list[id];
            localStorage.setItem('superclean_staff', JSON.stringify(staff_list));
            location.reload();
        }
    }
</script>
</body>
</html>
