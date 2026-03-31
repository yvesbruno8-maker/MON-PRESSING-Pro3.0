<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN - SYSTEM v7.0</title>
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --accent: #00d2d3;
        }
        
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); }
        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 6px solid var(--primary); position: relative; }
        .logo-box { background: var(--primary); color: white; padding: 10px 15px; border-radius: 12px; font-weight: 900; font-size: 24px; display: inline-block; }
        
        .btn-switch { position: absolute; top: 10px; right: 10px; background: var(--dark); color: white; border: none; padding: 8px 12px; border-radius: 8px; font-size: 10px; cursor: pointer; }

        .card { background: white; padding: 15px; border-radius: 15px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); margin-bottom: 15px; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 10px; border: 1px solid #ddd; font-size: 14px; box-sizing: border-box; outline:none; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; transition: 0.3s; }
        button:active { opacity: 0.7; transform: scale(0.98); }
        
        .nav-bar { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 5px; margin-bottom: 15px; }
        .nav-bar button { font-size: 11px; padding: 12px 5px; }

        .atelier-card { border-left: 8px solid var(--primary); margin-bottom: 10px; position: relative; }
        .badge-price { background: var(--light); color: var(--primary); padding: 2px 5px; border-radius: 5px; font-weight: bold; font-size: 11px; }

        .hidden { display: none !important; }
        
        @media print { 
            body * { visibility: hidden; } 
            #ticket-print, #ticket-print * { visibility: visible; } 
            #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; font-size: 12px; } 
        }
    </style>
</head>
<body>

<div id="auth-screen" style="height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background:var(--primary); color:white; padding:20px;">
    <div class="logo-box" style="background:white; color:var(--primary);">SC</div>
    <h1 id="txt-welcome">SUPER CLEAN</h1>
    <div style="width: 100%; max-width: 320px;">
        <input type="text" id="u-login" placeholder="Identifiant">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); height:50px;">SE CONNECTER</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="header">
        <button class="btn-switch" onclick="logout()" id="btn-switch-txt">🔄 CHANGER UTILISATEUR</button>
        <div class="logo-box">SC</div>
        <h2 style="margin:5px; color:var(--primary);">SUPER CLEAN</h2>
        <p id="user-info" style="font-size:11px; font-weight:bold; color:var(--secondary); margin:0;"></p>
    </div>

    <div class="nav-bar">
        <button onclick="tab('section-reception')" id="btn-nav-caisse">📥 CAISSE</button>
        <button onclick="tab('section-atelier')" id="btn-nav-atelier">👕 ZONE TECHNIQUE</button>
        <button onclick="tab('section-params')" id="btn-nav-params" class="hidden" style="background:var(--dark)">⚙️ PARAMS</button>
    </div>

    <div id="section-reception" class="card hidden tab-content">
        <h4 id="txt-new-rec">📥 Nouveau Dépôt</h4>
        <input type="text" id="client" placeholder="Nom du client">
        <input type="date" id="date-livraison">
        
        <div style="background:#f8f9fa; padding:10px; border-radius:10px; margin-top:10px;">
            <label style="font-size:11px; font-weight:bold;">SÉLECTIONNER PRESTATION :</label>
            <select id="select-article"></select>
            <div style="display:flex; gap:5px;">
                <input type="number" id="qte" value="1" min="1" style="flex:1;">
                <button onclick="ajouterAuPanier()" style="flex:2; background:var(--accent)">+ AJOUTER</button>
            </div>
        </div>

        <div id="liste-panier" style="margin:15px 0; border-top:1px dashed #ccc; padding-top:10px;"></div>
        
        <div style="background:var(--dark); color:white; padding:10px; border-radius:10px; text-align:right; margin-bottom:10px;">
            TOTAL : <span id="total-panier" style="font-size:20px; font-weight:bold;">0</span> F
        </div>

        <div style="margin: 10px 0; padding: 10px; background: #fff3cd; border-radius: 10px; display: flex; align-items: center; gap: 10px;">
            <input type="checkbox" id="paye-avance" style="width: 20px; height: 20px;">
            <label for="paye-avance" style="font-weight: bold; color: #856404; font-size: 13px;">Paiement reçu à l'avance ?</label>
        </div>

        <button onclick="validerCommande()" style="background:var(--success); height:50px;">VALIDER & IMPRIMER</button>
    </div>

    <div id="section-atelier" class="tab-content hidden">
        <h3 style="color:var(--primary); margin-left:10px;">🛠️ ZONE TECHNIQUE</h3>
        <div id="atelier-grid" style="display: grid; gap: 10px;"></div>
    </div>

    <div id="section-params" class="card hidden tab-content">
        <h3 id="txt-params-title">📊 Tableau de Bord</h3>
        
        <div id="dashboard" style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 20px;">
            <div style="background: var(--success); color: white; padding: 15px; border-radius: 12px;">
                <small>RECETTE DU JOUR</small>
                <div id="stats-jour" style="font-size: 20px; font-weight: 900;">0 F</div>
            </div>
            <div style="background: var(--danger); color: white; padding: 15px; border-radius: 12px;">
                <small>TOTAL IMPAYÉS</small>
                <div id="stats-impayes" style="font-size: 20px; font-weight: 900;">0 F</div>
            </div>
        </div>

        <hr>
        <h4>⚙️ Configuration</h4>
        <select id="sys-lang" onchange="changeLang()">
            <option value="fr">Français</option>
            <option value="en">English (Tech Area)</option>
            <option value="bm">Bamoun (Shü-mom)</option>
        </select>
        
        <div style="background: #f1f2f6; padding: 10px; border-radius: 10px; margin-top: 15px;">
            <h4 style="margin-top:0;">👥 Ajouter Personnel</h4>
            <input type="text" id="st-nom" placeholder="Nom">
            <input type="text" id="st-login" placeholder="Login">
            <input type="password" id="st-pass" placeholder="Password">
            <select id="st-role">
                <option value="CAISSIERE">Caissière</option>
                <option value="LAVEUR">Laveur / Zone Tech</option>
                <option value="ADMIN">Administrateur</option>
            </select>
            <button onclick="ajouterEmploye()" style="background:var(--primary)">CRÉER ACCÈS</button>
            <div id="staff-list" style="margin-top:10px;"></div>
        </div>
    </div>
</div>

<div id="ticket-print" style="display:none;">
    <center>
        <h2 style="margin:0;">SUPER CLEAN</h2>
        <p style="font-size:10px;">📍 Nkounga, Immeuble de Karche<br>📞 675 864 103 / 692 315 267</p>
    </center>
    <hr>
    <p><b>Ticket :</b> #<span id="t-id"></span><br><b>Client :</b> <span id="t-client"></span></p>
    <p><b>Retrait le :</b> <span id="t-date"></span></p>
    <div id="t-items" style="border-bottom:1px solid #000; padding-bottom:5px; margin-bottom:5px;"></div>
    <div style="text-align:right; font-weight:bold; font-size:14px;">TOTAL : <span id="t-total"></span> F</div>
    <p id="t-paye" style="font-weight:bold; text-align:center; border:1px solid #000; padding:5px;"></p>
    <hr>
    <center><p style="font-size:9px;">Merci de votre confiance !</p></center>
</div>

<script>
    const GRILLE = {
        "LES HAUTS": { "T-Shirt": 500, "Chemise": 500, "Polo": 500, "Pull Over": 700, "Blouson": 1000, "Démembré": 300, "Veste 2 pièces": 1500, "Veste 3 pièces": 2000, "Pantalon simple": 500, "Pantalon jogging": 500, "Pantalon jeans": 500, "Short": 500, "Jupe simple": 500, "Jupe plissée": 1000 },
        "ENSEMBLES": { "Costume simple": 2000, "Costume de luxe": 2500, "Boubou femme": 1500, "Boubou homme 2 p": 1500, "Boubou homme 3 p": 2500, "Pyjama 2 pièces": 1000, "Jogging": 1500 },
        "LES ROBES": { "Robe simple": 500, "Robe de soirée": 3000, "Kaba court": 1000, "Kaba long": 1500, "Toges": 3500 },
        "LINGES DE LIT": { "1 Drap": 750, "Ensemble 1 drap+taies": 1500, "Ensemble 2 drap+taies": 2000, "Couvre lit / Couette": 2000, "Housse de couette": 2500, "Couverture": 3000 },
        "AMEUBLEMENT": { "Rideau lourd": 1500, "Rideau léger": 1000, "Petite Serviette": 500, "Peignoir": 2000 },
        "ACCESSOIRES": { "Tennis": 500, "Cravate": 500 }
    };

    const TRANSLATIONS = {
        fr: { welcome: "BIENVENUE", caisse: "📥 CAISSE", atelier: "👕 ZONE TECHNIQUE", params: "⚙️ PARAMS", switch: "🔄 CHANGER UTILISATEUR" },
        en: { welcome: "WELCOME", caisse: "📥 CASHIER", atelier: "👕 TECH AREA", params: "⚙️ SETTINGS", switch: "🔄 SWITCH USER" },
        bm: { welcome: "MÄ PUU", caisse: "📥 NTU NKÙ", atelier: "👕 LI'Ì GBÈ", params: "⚙️ NTU GBÈ", switch: "🔄 KÜ KÀ' PÀ FÂ" }
    };

    let CONFIG = JSON.parse(localStorage.getItem('sc_cfg')) || { lang: 'fr' };
    let EMPLOYES = JSON.parse(localStorage.getItem('sc_staff')) || {"admin": { pass: "0000", role: "ADMIN", nom: "Le Responsable" }};
    let commandes = JSON.parse(localStorage.getItem('sc_data')) || [];
    let panier = [];
    let user = null;

    function login() {
        const id = document.getElementById('u-login').value.toLowerCase().trim();
        const ps = document.getElementById('u-pass').value;
        if(EMPLOYES[id] && EMPLOYES[id].pass === ps) {
            user = EMPLOYES[id];
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            applyLang();
            document.getElementById('user-info').innerText = `${user.nom} (${user.role})`;
            if(user.role === "ADMIN") document.getElementById('btn-nav-params').classList.remove('hidden');
            initialiserTarifs(); 
            afficherAtelier(); 
            tab(user.role === "LAVEUR" ? 'section-atelier' : 'section-reception');
        } else alert("Erreur d'accès !");
    }

    function logout() { user = null; location.reload(); }

    function tab(id) {
        if(id === 'section-params' && user.role !== 'ADMIN') {
            alert("Accès réservé à l'administration !"); return;
        }
        document.querySelectorAll('.tab-content').forEach(c => c.classList.add('hidden'));
        document.getElementById(id).classList.remove('hidden');
        if(id === 'section-params') { mettreAJourStats(); afficherListeStaff(); }
        if(id === 'section-atelier') afficherAtelier();
    }

    function initialiserTarifs() {
        const s = document.getElementById('select-article');
        s.innerHTML = "";
        for(let cat in GRILLE) {
            let g = document.createElement('optgroup'); g.label = cat;
            for(let art in GRILLE[cat]) {
                let o = document.createElement('option');
                o.value = GRILLE[cat][art];
                o.innerText = `${art} (${GRILLE[cat][art]} F)`;
                g.appendChild(o);
            }
            s.appendChild(g);
        }
    }

    function ajouterAuPanier() {
        const s = document.getElementById('select-article');
        const q = parseInt(document.getElementById('qte').value);
        const nom = s.options[s.selectedIndex].text.split(' (')[0];
        panier.push({ nom: nom, prix: parseFloat(s.value), qte: q, st: parseFloat(s.value) * q });
        renderPanier();
    }

    function renderPanier() {
        const div = document.getElementById('liste-panier');
        let tot = 0;
        div.innerHTML = panier.map((i, idx) => {
            tot += i.st;
            return `<div style="display:flex; justify-content:space-between; margin:5px 0; font-size:13px;">
                <span>${i.qte}x ${i.nom}</span>
                <span><b>${i.st} F</b> <button onclick="supprItem(${idx})" style="width:auto; padding:2px 5px; background:red;">x</button></span>
            </div>`;
        }).join('');
        document.getElementById('total-panier').innerText = tot;
    }

    function supprItem(idx) { panier.splice(idx, 1); renderPanier(); }

    function validerCommande() {
        const client = document.getElementById('client').value;
        const dateL = document.getElementById('date-livraison').value;
        if(!client || !dateL || panier.length === 0) return alert("Veuillez remplir toutes les informations");
        
        const total = panier.reduce((a,b) => a + b.st, 0);
        const estPaye = document.getElementById('paye-avance').checked;
        
        const cmd = { 
            id: Date.now().toString().slice(-5), 
            client: client, 
            date: dateL, 
            dateEnreg: new Date().toISOString().split('T')[0],
            items: [...panier], 
            total: total, 
            status: 'En attente',
            paye: estPaye 
        };
        
        commandes.unshift(cmd);
        localStorage.setItem('sc_data', JSON.stringify(commandes));
        imprimerTicket(cmd);
        location.reload();
    }

    function imprimerTicket(cmd) {
        document.getElementById('t-id').innerText = cmd.id;
        document.getElementById('t-client').innerText = cmd.client;
        document.getElementById('t-date').innerText = new Date(cmd.date).toLocaleDateString();
        document.getElementById('t-total').innerText = cmd.total;
        document.getElementById('t-paye').innerText = cmd.paye ? "✅ PAYÉ" : "⚠️ À PAYER À LA RÉCEPTION";
        document.getElementById('t-items').innerHTML = cmd.items.map(i => `<div>${i.qte}x ${i.nom} .... ${i.st}F</div>`).join('');
        window.print();
    }

    function afficherAtelier() {
        const grid = document.getElementById('atelier-grid');
        grid.innerHTML = "";
        const actifs = commandes.filter(c => c.status !== 'Livré');
        
        if(actifs.length === 0) {
            grid.innerHTML = "<center style='padding:20px; color:gray;'>Aucun travail en Zone Technique.</center>";
            return;
        }

        actifs.forEach(c => {
            const etapes = ["En attente", "Lavage", "Repassage", "Prêt"];
            let color = "#e74c3c"; // Rouge
            if(c.status === "Lavage") color = "#3498db";
            if(c.status === "Repassage") color = "#f1c40f";
            if(c.status === "Prêt") color = "#2ecc71";

            grid.innerHTML += `
                <div class="card atelier-card" style="border-left: 8px solid ${color}">
                    <div style="display:flex; justify-content:space-between;">
                        <b>${c.client.toUpperCase()}</b>
                        <span class="badge-price">#${c.id}</span>
                    </div>
                    <div style="font-size:11px; margin:5px 0;">
                        ${c.paye ? '<span style="color:green;">✅ PAYÉ</span>' : '<span style="color:red; font-weight:bold;">⚠️ À PAYER</span>'}
                        | Livraison : ${new Date(c.date).toLocaleDateString()}
                    </div>
                    <div style="font-weight:bold; color:${color}; font-size:12px;">Étape : ${c.status}</div>
                    <hr>
                    <div style="font-size:13px;">${c.items.map(i => `• ${i.qte}x ${i.nom}`).join('<br>')}</div>
                    <div style="display:grid; grid-template-columns:1fr 1fr; gap:5px; margin-top:10px;">
                        <button onclick="changerStatut('${c.id}')" style="background:${color}; font-size:11px;">➡️ ÉTAPE SUIVANTE</button>
                        <button onclick="finaliserLivraison('${c.id}')" style="background:var(--dark); font-size:11px;">📦 LIVRER</button>
                    </div>
                </div>`;
        });
    }

    function changerStatut(id) {
        const i = commandes.findIndex(c => c.id === id);
        const etapes = ["En attente", "Lavage", "Repassage", "Prêt"];
        let actuel = etapes.indexOf(commandes[i].status);
        if(actuel < etapes.length - 1) {
            commandes[i].status = etapes[actuel + 1];
            localStorage.setItem('sc_data', JSON.stringify(commandes));
            afficherAtelier();
        }
    }

    function finaliserLivraison(id) {
        const i = commandes.findIndex(c => c.id === id);
        if(!commandes[i].paye) {
            if(!confirm("⚠️ ATTENTION: Cette commande n'est pas payée. Confirmer la livraison quand même ?")) return;
        }
        commandes[i].status = 'Livré';
        localStorage.setItem('sc_data', JSON.stringify(commandes));
        afficherAtelier();
    }

    function mettreAJourStats() {
        const aujourdhui = new Date().toISOString().split('T')[0];
        const recetteJour = commandes
            .filter(c => c.dateEnreg === aujourdhui && c.paye === true)
            .reduce((sum, c) => sum + c.total, 0);
        const totalImpayes = commandes
            .filter(c => c.paye === false)
            .reduce((sum, c) => sum + c.total, 0);

        document.getElementById('stats-jour').innerText = recetteJour.toLocaleString() + " F";
        document.getElementById('stats-impayes').innerText = totalImpayes.toLocaleString() + " F";
    }

    function ajouterEmploye() {
        const nom = document.getElementById('st-nom').value;
        const loginId = document.getElementById('st-login').value.toLowerCase().trim();
        const pass = document.getElementById('st-pass').value;
        const role = document.getElementById('st-role').value;
        if(!nom || !loginId || !pass) return alert("Remplissez tous les champs");
        EMPLOYES[loginId] = { nom: nom, pass: pass, role: role };
        localStorage.setItem('sc_staff', JSON.stringify(EMPLOYES));
        afficherListeStaff();
    }

    function afficherListeStaff() {
        const listDiv = document.getElementById('staff-list');
        listDiv.innerHTML = "<h5>Personnel actif :</h5>";
        for(let id in EMPLOYES) {
            listDiv.innerHTML += `<div style="font-size:11px; padding:5px; border-bottom:1px solid #ddd;"><b>${id}</b> - ${EMPLOYES[id].nom} (${EMPLOYES[id].role})</div>`;
        }
    }

    function changeLang() {
        CONFIG.lang = document.getElementById('sys-lang').value;
        localStorage.setItem('sc_cfg', JSON.stringify(CONFIG));
        applyLang();
    }

    function applyLang() {
        const l = TRANSLATIONS[CONFIG.lang];
        document.getElementById('txt-welcome').innerText = l.welcome;
        document.getElementById('btn-nav-caisse').innerText = l.caisse;
        document.getElementById('btn-nav-atelier').innerText = l.atelier;
        document.getElementById('btn-nav-params').innerText = l.params;
        document.getElementById('btn-switch-txt').innerText = l.switch;
    }
</script>
</body>
</html>
