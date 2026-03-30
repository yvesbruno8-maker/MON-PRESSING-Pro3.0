# MON-PRESSING-Pro3.0
MON PRESSING Pro3.0 est un logiciel application de gestion automatisée des pressings

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Super CLEAN - Comptabilité PRO</title>
    <style>
        :root { --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; --danger: #e74c3c; --warning: #f1c40f; --bg: #f5f6fa; }
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--bg); }
        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); margin-bottom: 15px; }
        .card { background: white; padding: 15px; border-radius: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); margin-bottom: 15px; }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; }
        .stat-card { background: var(--primary); color: white; padding: 15px; border-radius: 15px; text-align: center; }
        .stat-card.money { background: var(--success); }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 10px; border: 1px solid #ddd; font-size: 15px; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; }
        .commande-item { background: white; padding: 12px; margin-bottom: 10px; border-radius: 15px; border-left: 5px solid var(--primary); display: flex; flex-direction: column; }
        .badge { padding: 4px 8px; border-radius: 5px; font-size: 11px; font-weight: bold; color: white; display: inline-block; width: fit-content; }
        .actions-btn { display: flex; gap: 5px; margin-top: 10px; }
        .btn-wa { background: #25D366; font-size: 11px; }
        .btn-del { background: var(--danger); font-size: 11px; }
        @media print { .no-print { display: none; } #ticket-print { display: block; width: 100%; } }
    </style>
</head>
<body>

<div id="auth-screen" style="position:fixed; top:0; left:0; width:100%; height:100%; background:var(--primary); z-index:9999; display:flex; flex-direction:column; align-items:center; justify-content:center; color:white;">
    <h2>Super CLEAN PRO</h2>
    <input type="password" id="pin-input" placeholder="CODE PIN (2026)" style="width:200px; text-align:center;">
    <button onclick="verifierPIN()" style="width:200px;">OUVRIR LA CAISSE</button>
</div>

<div class="no-print">
    <div class="header">
        <h3 style="margin:0; color:var(--primary);">Super CLEAN Pressing</h3>
        <p style="font-size:12px; color:gray;">Horaires : Lun - Sam | 08h00 - 18h00</p>
    </div>

    <div class="stats-grid">
        <div class="stat-card money">
            <small>Recette du Jour (CFA)</small>
            <div id="recette-jour" style="font-size:22px; font-weight:bold;">0</div>
        </div>
        <div class="stat-card">
            <small>Commandes</small>
            <div id="nb-commandes" style="font-size:22px; font-weight:bold;">0</div>
        </div>
    </div>

    <div class="card">
        <h4>Nouvelle Commande</h4>
        <input type="text" id="client" placeholder="Nom du client">
        <input type="tel" id="telephone" placeholder="Numéro WhatsApp">
        <select id="article"></select>
        <input type="number" id="quantite" value="1">
        <button onclick="ajouterAuPanier()">+ AJOUTER</button>

        <div id="panier-zone" style="display:none; margin-top:10px; border-top:1px solid #eee;">
            <ul id="liste-panier" style="font-size:13px; padding-left:15px;"></ul>
            <select id="mode-paiement" onchange="toggleAvance()">
                <option value="Comptant">✅ Payé Comptant</option>
                <option value="Avance">💰 Avance versée</option>
                <option value="Retrait">🕒 Paye au retrait</option>
            </select>
            <input type="number" id="montant-avance" style="display:none;" placeholder="Montant reçu">
            <div style="text-align:right; font-size:18px; font-weight:bold; margin:10px 0;">TOTAL : <span id="total-final">0</span> F</div>
            <button onclick="validerCommande()" style="background:var(--success)">ENREGISTRER & FERMER</button>
        </div>
    </div>

    <div id="listeCommandes"></div>
</div>

<script>
    const PIN = "2026";
    let panier = [];
    const catalogue = [
        {n:"T-Shirt", p:500}, {n:"Chemise", p:500}, {n:"Costume", p:2500}, {n:"Robe", p:1000},
        {n:"Pantalon Jeans", p:500}, {n:"Drap", p:750}, {n:"Tennis", p:500}, {n:"Rideau", p:1500}
    ];

    function verifierPIN() { if(document.getElementById('pin-input').value === PIN) document.getElementById('auth-screen').style.display = 'none'; }

    function chargerCatalogue() {
        document.getElementById('article').innerHTML = catalogue.map(i => `<option value="${i.p}">${i.n} (${i.p}F)</option>`).join('');
    }

    function toggleAvance() {
        document.getElementById('montant-avance').style.display = (document.getElementById('mode-paiement').value === 'Avance') ? 'block' : 'none';
    }

    function ajouterAuPanier() {
        const s = document.getElementById('article');
        const n = s.options[s.selectedIndex].text.split(' (')[0];
        const p = parseInt(s.value);
        const q = parseInt(document.getElementById('quantite').value);
        panier.push({n, p, q, t: p*q});
        document.getElementById('panier-zone').style.display = 'block';
        document.getElementById('liste-panier').innerHTML = panier.map(i => `<li>${i.n} x${i.q}</li>`).join('');
        document.getElementById('total-final').innerText = panier.reduce((s, i) => s + i.t, 0);
    }

    function validerCommande() {
        const c = document.getElementById('client').value;
        const t = document.getElementById('telephone').value;
        const tf = parseInt(document.getElementById('total-final').innerText);
        const mode = document.getElementById('mode-paiement').value;
        
        let encaissée = 0, rest = tf, stat = "Non payé", cls = "background:var(--danger)";
        if(mode === "Comptant") { encaissée = tf; rest = 0; stat = "Payé"; cls = "background:var(--success)"; }
        else if(mode === "Avance") { encaissée = parseInt(document.getElementById('montant-avance').value) || 0; rest = tf - encaissée; stat = "Reste "+rest+"F"; cls = "background:var(--warning); color:black;"; }

        const cmd = { id: Date.now(), client: c, tel: t, tot: tf, enc: encaissée, rest: rest, stat: stat, cls: cls, date: new Date().toLocaleDateString('fr-FR') };
        
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        cmds.push(cmd);
        localStorage.setItem('sc_cmds', JSON.stringify(cmds));
        location.reload();
    }

    function calculerCompta() {
        const cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        const aujourdhui = new Date().toLocaleDateString('fr-FR');
        
        // On ne filtre que l'argent encaissé AUJOURD'HUI
        const recette = cmds.filter(c => c.date === aujourdhui).reduce((s, c) => s + c.enc, 0);
        const nb = cmds.filter(c => c.date === aujourdhui).length;

        document.getElementById('recette-jour').innerText = recette.toLocaleString();
        document.getElementById('nb-commandes').innerText = nb;
    }

    function afficherCommandes() {
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        document.getElementById('listeCommandes').innerHTML = '<h4>Commandes en cours</h4>' + cmds.reverse().map(c => `
            <div class="commande-item">
                <div style="display:flex; justify-content:space-between;">
                    <strong>${c.client}</strong>
                    <span class="badge" style="${c.cls}">${c.stat}</span>
                </div>
                <small>${c.date} | Total: ${c.tot}F</small>
                <div class="actions-btn">
                    <button class="btn-wa" onclick="window.open('https://wa.me/237${c.tel}')">WhatsApp</button>
                    <button class="btn-del" onclick="suppr(${c.id})">Livré</button>
                </div>
            </div>
        `).join('');
    }

    function suppr(id) { if(confirm("Marquer comme livré et archiver ?")) {
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')).filter(x => x.id !== id);
        localStorage.setItem('sc_cmds', JSON.stringify(cmds)); location.reload();
    }}

    window.onload = () => { chargerCatalogue(); calculerCompta(); afficherCommandes(); };
</script>
</body>
</html>
