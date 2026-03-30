<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN - GRILLE INTEGRALE</title>
    <style>
        :root { --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; --danger: #e74c3c; --warning: #f1c40f; --bg: #f5f6fa; }
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--bg); color: #2f3542; }
        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 5px solid var(--primary); }
        .card { background: white; padding: 15px; border-radius: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); margin-bottom: 15px; }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; }
        .stat-card { background: var(--primary); color: white; padding: 12px; border-radius: 15px; text-align: center; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 10px; border: 1px solid #ddd; font-size: 15px; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; }
        optgroup { font-weight: bold; color: var(--primary); background: #f9f9f9; }
        .commande-item { background: white; padding: 15px; margin-bottom: 10px; border-radius: 15px; border-left: 6px solid var(--primary); }
        .badge { padding: 4px 8px; border-radius: 5px; font-size: 11px; font-weight: bold; color: white; }
        .actions-btn { display: flex; gap: 5px; margin-top: 10px; }
        #ticket-print { display: none; }
        @media print { .no-print { display: none !important; } #ticket-print { display: block !important; width: 100%; font-family: monospace; } }
    </style>
</head>
<body>

<div id="auth-screen" style="position:fixed; top:0; left:0; width:100%; height:100%; background:var(--primary); z-index:9999; display:flex; flex-direction:column; align-items:center; justify-content:center; color:white;">
    <h2>SUPER CLEAN PRO</h2>
    <input type="password" id="pin-input" placeholder="CODE PIN (2026)" style="width:220px; text-align:center; color:black;">
    <button onclick="verifierPIN()" style="width:220px; background:var(--success);">OUVRIR SESSION</button>
</div>

<div class="no-print">
    <div class="header">
        <h3 style="margin:0; color:var(--primary);">Super CLEAN Pressing</h3>
        <p style="font-size:11px; margin:5px 0;">📍 Nkounga | 📞 675 864 103 / 692 315 267</p>
    </div>

    <div class="stats-grid">
        <div class="stat-card" style="background:var(--success)">
            <small>RECETTE DU JOUR</small>
            <div id="recette-jour" style="font-size:20px; font-weight:bold;">0</div>
        </div>
        <div class="stat-card">
            <small>SAVON (L)</small>
            <div id="stock-val" style="font-size:20px; font-weight:bold;">0.0</div>
        </div>
    </div>

    <div class="card">
        <h4 style="margin:0 0 10px 0;">📋 Nouvelle Commande</h4>
        <input type="text" id="client" placeholder="Nom du Client">
        <input type="tel" id="telephone" placeholder="Numéro WhatsApp">
        
        <label style="font-size:12px; font-weight:bold;">Sélectionner l'article :</label>
        <select id="article"></select>
        
        <input type="number" id="quantite" value="1" min="1">
        <button onclick="ajouterAuPanier()" style="background:var(--secondary)">+ AJOUTER AU PANIER</button>

        <div id="panier-zone" style="display:none; margin-top:15px; border-top:2px dashed #eee; padding-top:10px;">
            <ul id="liste-panier" style="font-size:14px;"></ul>
            <select id="mode-paiement" onchange="toggleAvance()">
                <option value="Comptant">✅ Payé Comptant</option>
                <option value="Avance">💰 Avance versée</option>
                <option value="Retrait">🕒 Paye au retrait</option>
            </select>
            <input type="number" id="montant-avance" style="display:none;" placeholder="Montant perçu">
            <div style="text-align:right; font-size:20px; font-weight:bold; color:var(--primary); margin:10px 0;">TOTAL: <span id="total-final">0</span> F</div>
            <button onclick="validerCommande()" style="background:var(--success)">VALIDER LA COMMANDE</button>
        </div>
    </div>

    <div id="listeCommandes"></div>
</div>

<div id="ticket-print"></div>

<script>
    const PIN = "2026";
    let panier = [];
    let stock = parseFloat(localStorage.getItem('sc_stock')) || 20;

    // TOUTE LA GRILLE TARIFAIRE PAR CATÉGORIE (De ton image)
    const grille = {
        "👕 LES HAUTS": [
            {n:"T-Shirt", p:500}, {n:"Chemise", p:500}, {n:"Polo", p:500}, {n:"Pull Over", p:700}, 
            {n:"Blouson", p:1000}, {n:"Démembré", p:300}, {n:"Veste 2 pièces", p:1500}, {n:"Veste 3 pièces", p:2000}
        ],
        "👖 LES BAS": [
            {n:"Pantalon simple", p:500}, {n:"Pantalon jogging", p:500}, {n:"Pantalon jeans", p:500}, 
            {n:"Short", p:500}, {n:"Jupe simple", p:500}, {n:"Jupe plissée", p:1000}
        ],
        "🤵 ENSEMBLES": [
            {n:"Costume simple", p:2000}, {n:"Costume de luxe", p:2500}, {n:"Boubou femme", p:1500}, 
            {n:"Boubou homme 2 pcs", p:1500}, {n:"Boubou homme 3 pcs", p:2500}, {n:"Pyjama 2 pcs", p:1000}, {n:"Jogging", p:1500}
        ],
        "👗 ROBES": [
            {n:"Robe simple", p:500}, {n:"Robe de soirée", p:3000}, {n:"Kaba court", p:1000}, 
            {n:"Kaba long", p:1500}, {n:"Toge (Magistrat/Avocat)", p:3500}
        ],
        "🛏️ LINGE DE LIT": [
            {n:"1 Drap", p:750}, {n:"Ensemble 1 drap+taies", p:1500}, {n:"Ensemble 2 draps+taies", p:2000}, 
            {n:"Couvre lit / Couette", p:2000}, {n:"Housse de couette", p:2500}, {n:"Couverture", p:3000}
        ],
        "🛁 BAIN & MAISON": [
            {n:"Rideau lourd", p:1500}, {n:"Rideau léger", p:1000}, {n:"Petite Serviette", p:500}, 
            {n:"Grande Serviette", p:1000}, {n:"Peignoir", p:2000}
        ],
        "👟 ACCESSOIRES": [
            {n:"Tennis (Paire)", p:500}, {n:"Cravate", p:500}, {n:"Ensemble Enfant", p:1000}
        ]
    };

    function verifierPIN() { if(document.getElementById('pin-input').value === PIN) document.getElementById('auth-screen').style.display = 'none'; }

    function chargerCatalogue() {
        let html = "";
        for (const [cat, items] of Object.entries(grille)) {
            html += `<optgroup label="${cat}">`;
            items.forEach(i => { html += `<option value="${i.p}">${i.n} (${i.p}F)</option>`; });
            html += `</optgroup>`;
        }
        document.getElementById('article').innerHTML = html;
    }

    function toggleAvance() { document.getElementById('montant-avance').style.display = (document.getElementById('mode-paiement').value === 'Avance') ? 'block' : 'none'; }

    function ajouterAuPanier() {
        const s = document.getElementById('article');
        const n = s.options[s.selectedIndex].text.split(' (')[0];
        const p = parseInt(s.value);
        const q = parseInt(document.getElementById('quantite').value);
        panier.push({n, p, q, t: p*q});
        MAJPanier();
    }

    function MAJPanier() {
        document.getElementById('panier-zone').style.display = 'block';
        document.getElementById('liste-panier').innerHTML = panier.map(i => `<li>${i.n} x${i.q}</li>`).join('');
        document.getElementById('total-final').innerText = panier.reduce((s, i) => s + i.t, 0);
    }

    function validerCommande() {
        const c = document.getElementById('client').value;
        const t = document.getElementById('telephone').value;
        const tf = parseInt(document.getElementById('total-final').innerText);
        const mode = document.getElementById('mode-paiement').value;
        if(!c || !t) return alert("Remplir Nom et Téléphone !");

        let enc = 0, rest = tf, stat = "Non Payé", cls = "background:var(--danger)";
        if(mode === "Comptant") { enc = tf; rest = 0; stat = "Payé"; cls = "background:var(--success)"; }
        else if(mode === "Avance") { enc = parseInt(document.getElementById('montant-avance').value) || 0; rest = tf - enc; stat = "Reste "+rest+"F"; cls = "background:var(--warning); color:black;"; }

        const cmd = { id: Date.now(), client: c, tel: t, arts: [...panier], tot: tf, enc: enc, rest: rest, stat: stat, cls: cls, date: new Date().toLocaleDateString('fr-FR') };
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        cmds.push(cmd);
        localStorage.setItem('sc_cmds', JSON.stringify(cmds));
        
        stock -= 0.1;
        localStorage.setItem('sc_stock', stock);
        location.reload();
    }

    function calculerCompta() {
        const cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        const aujourdhui = new Date().toLocaleDateString('fr-FR');
        const recette = cmds.filter(c => c.date === aujourdhui).reduce((s, c) => s + c.enc, 0);
        document.getElementById('recette-jour').innerText = recette.toLocaleString();
        document.getElementById('stock-val').innerText = stock.toFixed(1);
    }

    function afficherCommandes() {
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        document.getElementById('listeCommandes').innerHTML = '<h4 style="margin:10px 0;">📦 Commandes Actives</h4>' + cmds.reverse().map(c => `
            <div class="commande-item">
                <div style="display:flex; justify-content:space-between; align-items:start;">
                    <div><strong>${c.client}</strong><br><small>${c.date} | ${c.tot}F</small></div>
                    <span class="badge" style="${c.cls}">${c.stat}</span>
                </div>
                <div class="actions-btn">
                    <button onclick="notifierWhatsApp(${c.id})" style="background:#25D366; font-size:11px;">📲 WhatsApp</button>
                    <button onclick="imprimerTicket(${c.id})" style="background:var(--primary); font-size:11px;">🎫 Ticket</button>
                    <button onclick="suppr(${c.id})" style="background:var(--danger); font-size:11px;">🗑️ Livré</button>
                </div>
            </div>
        `).join('');
    }

    function notifierWhatsApp(id) {
        const c = JSON.parse(localStorage.getItem('sc_cmds')).find(x => x.id === id);
        let tel = c.tel; if(!tel.startsWith('237')) tel = '237' + tel;
        const msg = `Bonjour ${c.client}, c'est *Super CLEAN* ! 👋 Votre commande est prête.\n💰 Total: ${c.tot}F\n⚠️ Reste à payer: *${c.rest}F*. Merci !`;
        window.open(`https://wa.me/${tel}?text=${encodeURIComponent(msg)}`);
    }

    function imprimerTicket(id) {
        const c = JSON.parse(localStorage.getItem('sc_cmds')).find(x => x.id === id);
        document.getElementById('ticket-print').innerHTML = `<div style="text-align:center; padding:10px;"><h2>SUPER CLEAN</h2><hr><p>Client: ${c.client}</p><p>Total: ${c.tot}F</p><p>Payé: ${c.enc}F</p><strong>Reste: ${c.rest}F</strong></div>`;
        window.print();
    }

    function suppr(id) { if(confirm("Archiver ?")) {
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')).filter(x => x.id !== id);
        localStorage.setItem('sc_cmds', JSON.stringify(cmds)); location.reload();
    }}

    window.onload = () => { chargerCatalogue(); calculerCompta(); afficherCommandes(); };
</script>
</body>
</html>
