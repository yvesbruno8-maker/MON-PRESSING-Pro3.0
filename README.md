# MON-PRESSING-Pro3.0
MON PRESSING Pro 3.0 est un logiciel application de gestion automatisée des pressings

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN PRESSING - GESTION PROFESSIONNELLE</title>
    <style>
        :root { --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; --danger: #e74c3c; --warning: #f1c40f; --bg: #f5f6fa; }
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--bg); color: #2f3542; }
        .no-print { display: block; }
        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 5px solid var(--primary); }
        .card { background: white; padding: 15px; border-radius: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); margin-bottom: 15px; }
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; }
        .stat-card { background: var(--primary); color: white; padding: 12px; border-radius: 15px; text-align: center; }
        .stat-card.money { background: var(--success); }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 10px; border: 1px solid #ddd; font-size: 15px; box-sizing: border-box; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; transition: 0.3s; }
        button:active { transform: scale(0.98); }
        .commande-item { background: white; padding: 15px; margin-bottom: 10px; border-radius: 15px; border-left: 6px solid var(--primary); box-shadow: 0 2px 4px rgba(0,0,0,0.05); }
        .badge { padding: 4px 8px; border-radius: 5px; font-size: 11px; font-weight: bold; color: white; text-transform: uppercase; }
        .actions-btn { display: flex; gap: 5px; margin-top: 10px; }
        .btn-wa { background: #25D366; } .btn-del { background: var(--danger); }
        #ticket-print { display: none; }
        @media print { .no-print { display: none !important; } #ticket-print { display: block !important; width: 100%; font-family: monospace; } }
    </style>
</head>
<body>

<div id="auth-screen" style="position:fixed; top:0; left:0; width:100%; height:100%; background:var(--primary); z-index:9999; display:flex; flex-direction:column; align-items:center; justify-content:center; color:white;">
    <div style="background:white; padding:10px; border-radius:50%; margin-bottom:15px;">
        <svg width="60" height="60" viewBox="0 0 100 100"><circle cx="50" cy="50" r="48" fill="#1e3799"/><text x="50" y="42" text-anchor="middle" font-weight="bold" font-size="20" fill="white">Super</text><text x="50" y="68" text-anchor="middle" font-weight="bold" font-size="26" fill="white">Clean</text></svg>
    </div>
    <h2>LOGICIEL DE CAISSE</h2>
    <input type="password" id="pin-input" placeholder="CODE PIN (2026)" style="width:220px; text-align:center; color:black;">
    <button onclick="verifierPIN()" style="width:220px; background:var(--success);">OUVRIR LA SESSION</button>
</div>

<div class="no-print">
    <div class="header">
        <h3 style="margin:0; color:var(--primary);">Super CLEAN Pressing</h3>
        <p style="font-size:11px; margin:5px 0;">📍 Nkounga Immeuble de Karche | 📧 supercleanpres@gmail.com</p>
        <p style="font-size:12px; font-weight:bold; color:var(--secondary);">📞 675 864 103 / 692 315 267</p>
    </div>

    <div class="stats-grid">
        <div class="stat-card money">
            <small>CAISSE DU JOUR (CFA)</small>
            <div id="recette-jour" style="font-size:20px; font-weight:bold;">0</div>
        </div>
        <div class="stat-card">
            <small>STOCK LESSIVE (L)</small>
            <div id="stock-val" style="font-size:20px; font-weight:bold;">0.0</div>
        </div>
    </div>

    <div class="card">
        <h4 style="margin-top:0;">📋 Nouvelle Réception</h4>
        <input type="text" id="client" placeholder="Nom du Client">
        <input type="tel" id="telephone" placeholder="Numéro WhatsApp (9 chiffres)" oninput="verifierFidelite(this.value)">
        
        <div id="info-fidelite" style="display:none; background:#e8f8f5; padding:8px; border-radius:8px; margin-bottom:10px; border:1px solid var(--success); font-size:13px;">
            ⭐ Fidélité: <strong id="pts-client">0</strong> pts | <button onclick="utiliserPoints()" style="width:auto; padding:2px 8px; font-size:10px; background:var(--warning); color:black;">Cadeau -500F</button>
        </div>

        <select id="article"></select>
        <input type="number" id="quantite" value="1" min="1">
        <button onclick="ajouterAuPanier()" style="background:var(--secondary)">+ AJOUTER L'ARTICLE</button>

        <div id="panier-zone" style="display:none; margin-top:15px; border-top:2px dashed #eee; padding-top:10px;">
            <ul id="liste-panier" style="font-size:14px; padding-left:20px;"></ul>
            <select id="mode-paiement" onchange="toggleAvance()">
                <option value="Comptant">✅ Payé Comptant</option>
                <option value="Avance">💰 Avance versée</option>
                <option value="Retrait">🕒 Paye au retrait</option>
            </select>
            <input type="number" id="montant-avance" style="display:none;" placeholder="Montant perçu (F)">
            <div style="text-align:right; font-size:20px; font-weight:bold; color:var(--primary); margin:10px 0;">TOTAL: <span id="total-final">0</span> F</div>
            <button onclick="validerCommande()" style="background:var(--success); font-size:18px;">ENREGISTRER LA COMMANDE</button>
        </div>
    </div>

    <div class="card" style="background:#eee;">
        <input type="number" id="add-stock-val" placeholder="Ajouter Litres de Savon" style="width:60%;">
        <button onclick="majStock()" style="width:35%; float:right;">STOCK +</button>
    </div>

    <div id="listeCommandes"></div>
</div>

<div id="ticket-print"></div>

<script>
    const PIN = "2026";
    let panier = [];
    let remiseFidelite = 0;
    let stock = parseFloat(localStorage.getItem('sc_stock')) || 20;
    let fidelite = JSON.parse(localStorage.getItem('sc_fid')) || {};

    const catalogue = [
        {n:"T-Shirt / Chemise / Polo", p:500}, {n:"Pull Over / Gilet", p:700}, {n:"Veste 2 pièces", p:1500},
        {n:"Veste 3 pièces", p:2000}, {n:"Costume Simple", p:2000}, {n:"Costume de Luxe", p:2500},
        {n:"Pantalon Simple/Jeans", p:500}, {n:"Robe Simple", p:500}, {n:"Robe de Soirée", p:3000},
        {n:"Boubou Homme 2 pcs", p:1500}, {n:"Boubou Homme 3 pcs", p:2500}, {n:"Kaba Long", p:1500},
        {n:"1 Drap", p:750}, {n:"Couvre lit / Couette", p:2000}, {n:"Rideau Lourd", p:1500},
        {n:"Tennis (Paire)", p:500}, {n:"Peignoir", p:2000}, {n:"Grande Serviette", p:1000}
    ];

    function verifierPIN() { if(document.getElementById('pin-input').value === PIN) document.getElementById('auth-screen').style.display = 'none'; }

    function chargerCatalogue() {
        document.getElementById('article').innerHTML = catalogue.map(i => `<option value="${i.p}">${i.n} (${i.p}F)</option>`).join('');
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
        let total = panier.reduce((s, i) => s + i.t, 0) - remiseFidelite;
        document.getElementById('total-final').innerText = total < 0 ? 0 : total;
    }

    function verifierFidelite(tel) {
        const pts = fidelite[tel] || 0;
        document.getElementById('info-fidelite').style.display = pts > 0 ? 'block' : 'none';
        document.getElementById('pts-client').innerText = pts;
    }

    function utiliserPoints() {
        remiseFidelite = 500;
        alert("Remise fidélité appliquée !");
        MAJPanier();
    }

    function validerCommande() {
        const c = document.getElementById('client').value;
        const t = document.getElementById('telephone').value;
        const tf = parseInt(document.getElementById('total-final').innerText);
        const mode = document.getElementById('mode-paiement').value;
        if(!c || !t) return alert("Nom et Téléphone obligatoires !");

        let enc = 0, rest = tf, stat = "Non Payé", cls = "background:var(--danger)";
        if(mode === "Comptant") { enc = tf; rest = 0; stat = "Payé"; cls = "background:var(--success)"; }
        else if(mode === "Avance") { enc = parseInt(document.getElementById('montant-avance').value) || 0; rest = tf - enc; stat = "Reste "+rest+"F"; cls = "background:var(--warning); color:black;"; }

        const cmd = { id: Date.now(), client: c, tel: t, arts: [...panier], tot: tf, enc: enc, rest: rest, stat: stat, cls: cls, date: new Date().toLocaleDateString('fr-FR') };
        
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        cmds.push(cmd);
        localStorage.setItem('sc_cmds', JSON.stringify(cmds));
        
        // MAJ FIDELITE & STOCK
        fidelite[t] = (fidelite[t] || 0) + Math.floor(tf/1000);
        localStorage.setItem('sc_fid', JSON.stringify(fidelite));
        stock -= 0.1;
        localStorage.setItem('sc_stock', stock);

        alert("Commande enregistrée avec succès !");
        location.reload();
    }

    function calculerCompta() {
        const cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        const aujourdhui = new Date().toLocaleDateString('fr-FR');
        const recette = cmds.filter(c => c.date === aujourdhui).reduce((s, c) => s + c.enc, 0);
        document.getElementById('recette-jour').innerText = recette.toLocaleString();
        document.getElementById('stock-val').innerText = stock.toFixed(1);
    }

    function notifierWhatsApp(id) {
        const c = JSON.parse(localStorage.getItem('sc_cmds')).find(x => x.id === id);
        let tel = c.tel; if(!tel.startsWith('237')) tel = '237' + tel;
        const msg = `Bonjour ${c.client}, c'est *Super CLEAN Pressing* ! 👋\nVotre linge est prêt.\n💰 Total: ${c.tot}F\n✅ Payé: ${c.enc}F\n⚠️ Reste: *${c.rest}F*.\nMerci !`;
        window.open(`https://wa.me/${tel}?text=${encodeURIComponent(msg)}`);
    }

    function imprimerTicket(id) {
        const c = JSON.parse(localStorage.getItem('sc_cmds')).find(x => x.id === id);
        document.getElementById('ticket-print').innerHTML = `
            <div style="text-align:center; padding:20px; border:1px solid #000;">
                <h2 style="margin:0;">SUPER CLEAN</h2><p>Nkounga, Immeuble de Karche</p><hr>
                <p style="text-align:left;">Client: ${c.client}<br>Date: ${c.date}</p><hr>
                ${c.arts.map(a => `<div style="display:flex; justify-content:space-between;"><span>${a.n} x${a.q}</span><span>${a.t}F</span></div>`).join('')}<hr>
                <h3 style="text-align:right;">TOTAL: ${c.tot} F</h3>
                <p style="text-align:left;">Payé: ${c.enc} F<br><strong>RESTE À PAYER: ${c.rest} F</strong></p>
                <p>Merci de votre confiance !</p>
            </div>`;
        window.print();
    }

    function afficherCommandes() {
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
        document.getElementById('listeCommandes').innerHTML = '<h4 style="margin-bottom:10px;">📦 Commandes en cours</h4>' + cmds.reverse().map(c => `
            <div class="commande-item">
                <div style="display:flex; justify-content:space-between; align-items:flex-start;">
                    <div><strong>${c.client}</strong><br><small>${c.date} | ${c.tot}F</small></div>
                    <span class="badge" style="${c.cls}">${c.stat}</span>
                </div>
                <div class="actions-btn">
                    <button class="btn-wa" onclick="notifierWhatsApp(${c.id})">WhatsApp</button>
                    <button onclick="imprimerTicket(${c.id})">Ticket</button>
                    <button class="btn-del" onclick="suppr(${c.id})">Livré</button>
                </div>
            </div>
        `).join('');
    }

    function majStock() {
        const v = parseFloat(document.getElementById('add-stock-val').value) || 0;
        stock += v; localStorage.setItem('sc_stock', stock); location.reload();
    }

    function suppr(id) { if(confirm("Marquer comme livré ?")) {
        let cmds = JSON.parse(localStorage.getItem('sc_cmds')).filter(x => x.id !== id);
        localStorage.setItem('sc_cmds', JSON.stringify(cmds)); location.reload();
    }}

    window.onload = () => { chargerCatalogue(); calculerCompta(); afficherCommandes(); };
</script>
</body>
</html>
