# MON-PRESSING-Pro3.0
MON PRESSING Pro3.0 est un logiciel application de gestion automatisée des pressings

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN PRESSING - SYSTEM PRO</title>
    <style>
        :root { --primary: #1e3799; --secondary: #4a69bd; --success: #38ada9; --danger: #eb2f06; --warning: #f1c40f; --bg: #f1f2f6; --card-bg: #ffffff; }
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--bg); color: #2f3542; }
        .header { text-align: center; background: var(--card-bg); padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 5px solid var(--primary); }
        .card { background: var(--card-bg); padding: 15px; border-radius: 20px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); margin-bottom: 15px; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 10px; border: 2px solid #edeff2; font-size: 15px; }
        button { background: var(--success); color: white; border: none; font-weight: bold; cursor: pointer; }
        .badge { padding: 4px 8px; border-radius: 5px; font-size: 11px; font-weight: bold; color: white; }
        .bg-paye { background: var(--success); } .bg-reste { background: var(--warning); color: #333; } .bg-nonpaye { background: var(--danger); }
        .commande-item { border-left: 8px solid var(--primary); display: flex; justify-content: space-between; align-items: center; padding: 12px; margin-bottom: 10px; background: white; border-radius: 10px; }
        @media print { .no-print { display: none !important; } #ticket-print { display: block !important; width: 80mm; font-family: monospace; } }
        #ticket-print { display: none; }
    </style>
</head>
<body>

    <div id="auth-screen" style="position:fixed; top:0; left:0; width:100%; height:100%; background:var(--primary); z-index:9999; display:flex; flex-direction:column; align-items:center; justify-content:center; color:white;">
        <h2 style="margin-bottom:20px;">SUPER CLEAN PRO</h2>
        <input type="password" id="pin-input" placeholder="CODE PIN (2026)" style="width:200px; text-align:center;">
        <button onclick="verifierPIN()" style="width:200px;">ENTRER</button>
    </div>

    <div class="no-print">
        <div class="header">
            <div style="background: white; padding: 8px; border-radius: 50%; display: inline-block; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
                <svg width="50" height="50" viewBox="0 0 100 100">
                    <circle cx="50" cy="50" r="48" fill="#1e3799" />
                    <text x="50" y="42" text-anchor="middle" font-family="Arial" font-weight="bold" font-size="20" fill="white">Super</text>
                    <text x="50" y="68" text-anchor="middle" font-family="Arial" font-weight="bold" font-size="26" fill="white">Clean</text>
                </svg>
            </div>
            <h3 style="margin:10px 0; color:var(--primary);">GESTION PRESSING</h3>
            <p style="font-size:11px; margin:2px 0;">📍 Nkounga Immeuble de Karche</p>
            <p style="font-size:12px; font-weight:bold; color:var(--secondary);">📞 675 864 103 / 692 315 267</p>
            <div style="font-size: 10px; color: var(--success); font-weight: bold;">🚚 Service Express Disponible</div>
        </div>

        <div class="card">
            <h3>Nouvelle Commande</h3>
            <input type="text" id="client" placeholder="Nom du client">
            <input type="tel" id="telephone" placeholder="Numéro WhatsApp" oninput="verifierFidelite(this.value)">
            
            <div id="info-fidelite" style="background: rgba(56, 173, 169, 0.1); padding: 10px; border-radius: 10px; margin: 10px 0; display: none; border: 1px solid var(--success);">
                <span>⭐ Points : <strong id="pts-client">0</strong></span>
                <button id="btn-gift" onclick="utiliserPoints()" style="width:auto; padding:5px; font-size:10px; background:var(--warning); color:black; display:none;">Cadeau -500F</button>
            </div>

            <select id="article"></select>
            <input type="number" id="quantite" value="1" min="1">
            <button onclick="ajouterAuPanier()" style="background:var(--primary)">+ AJOUTER AU PANIER</button>

            <div id="panier-temporaire" style="display:none; margin-top:15px; border:1px dashed var(--warning); padding:10px; border-radius:10px;">
                <ul id="liste-panier" style="font-size:13px;"></ul>
                <hr>
                <select id="mode-paiement" onchange="document.getElementById('zone-av').style.display=(this.value=='Avance'?'block':'none')">
                    <option value="Comptant">✅ Payé Comptant</option>
                    <option value="Avance">💰 Avance versée</option>
                    <option value="Retrait">🕒 Paye au retrait</option>
                </select>
                <input type="number" id="montant-avance" id="zone-av" style="display:none;" placeholder="Montant versé">
                
                <div style="text-align:right; font-weight:bold; font-size:20px; color:var(--primary); margin:10px 0;">TOTAL : <span id="total-final">0</span> F</div>
                <button onclick="validerCommande()" style="background:var(--success)">✅ VALIDER ET IMPRIMER</button>
            </div>
        </div>

        <div id="listeCommandes"></div>

        <div class="card" style="border-left: 6px solid #9b59b6;">
            <h3>🧼 Stock Lessive</h3>
            <div style="display: flex; gap: 10px; align-items: center;">
                <div style="flex:1; text-align:center;">Stock: <strong id="stock-val">0</strong> L</div>
                <div style="flex:1;"><input type="number" id="add-stock" placeholder="+ Litres"><button onclick="majStock()" style="padding:5px;">Ajouter</button></div>
            </div>
        </div>
    </div>

    <div id="ticket-print"></div>

    <script>
        const PIN_SECRET = "2026";
        let panier = [];
        let remiseFid = 0;
        let stock = parseFloat(localStorage.getItem('sc_stock')) || 10;
        let fid = JSON.parse(localStorage.getItem('sc_fid')) || {};
        
        // GRILLE TARIFAIRE COMPLETE
        const catalogue = [
            {n:"T-Shirt", p:500}, {n:"Chemise", p:500}, {n:"Polo", p:500}, {n:"Pull Over", p:700}, {n:"Blouson", p:1000}, {n:"Démembré", p:300},
            {n:"Veste 2 pièces", p:1500}, {n:"Veste 3 pièces", p:2000}, {n:"Pantalon simple", p:500}, {n:"Pantalon jeans", p:500},
            {n:"Jupe simple", p:500}, {n:"Jupe plissée", p:1000}, {n:"Costume simple", p:2000}, {n:"Costume de luxe", p:2500},
            {n:"Boubou femme", p:1500}, {n:"Boubou homme 3 pcs", p:2500}, {n:"Robe simple", p:500}, {n:"Robe de soirée", p:3000},
            {n:"Kaba court", p:1000}, {n:"Kaba long", p:1500}, {n:"Toge (Magistrat/Avocat)", p:3500}, {n:"1 Drap", p:750},
            {n:"Couvre lit / Couette", p:2000}, {n:"Rideau lourd", p:1500}, {n:"Petite Serviette", p:500}, {n:"Peignoir", p:2000}, {n:"Tennis", p:500}
        ];

        function verifierPIN() { if(document.getElementById('pin-input').value === PIN_SECRET) document.getElementById('auth-screen').style.display = 'none'; }

        function chargerCatalogue() {
            document.getElementById('article').innerHTML = catalogue.map(i => `<option value="${i.p}">${i.n} (${i.p}F)</option>`).join('');
        }

        function ajouterAuPanier() {
            const s = document.getElementById('article');
            const n = s.options[s.selectedIndex].text.split(' (')[0];
            const p = parseInt(s.value);
            const q = parseInt(document.getElementById('quantite').value);
            panier.push({n, p, q, t: p*q});
            MAJPanier();
        }

        function MAJPanier() {
            const z = document.getElementById('panier-temporaire');
            if(panier.length > 0) {
                z.style.display = 'block';
                document.getElementById('liste-panier').innerHTML = panier.map(i => `<li>${i.n} x${i.q}</li>`).join('');
                let total = panier.reduce((s, i) => s + i.t, 0) - remiseFid;
                document.getElementById('total-final').innerText = total < 0 ? 0 : total;
            }
        }

        function verifierFidelite(tel) {
            const pts = fid[tel] || 0;
            const info = document.getElementById('info-fidelite');
            if(pts > 0) {
                info.style.display = 'block';
                document.getElementById('pts-client').innerText = pts;
                document.getElementById('btn-gift').style.display = pts >= 10 ? 'inline-block' : 'none';
            } else { info.style.display = 'none'; }
        }

        function utiliserPoints() {
            const tel = document.getElementById('telephone').value;
            if(fid[tel] >= 10) { fid[tel] -= 10; remiseFid = 500; MAJPanier(); verifierFidelite(tel); alert("Remise de 500F appliquée !"); }
        }

        function validerCommande() {
            const c = document.getElementById('client').value;
            const t = document.getElementById('telephone').value;
            const tf = parseInt(document.getElementById('total-final').innerText);
            const mode = document.getElementById('mode-paiement').value;
            
            if(!c || !t) return alert("Remplissez le nom et le téléphone !");

            let av = 0, rest = tf, stat = "Non payé", cls = "bg-nonpaye";
            if(mode === "Comptant") { av = tf; rest = 0; stat = "Payé"; cls = "bg-paye"; }
            else if(mode === "Avance") { av = parseInt(document.getElementById('montant-avance').value) || 0; rest = tf - av; stat = rest <= 0 ? "Payé" : "Reste "+rest+"F"; cls = rest <= 0 ? "bg-paye" : "bg-reste"; }

            const cmd = { id: Date.now(), client: c, tel: t, arts: panier, tot: tf, av, rest, stat, cls, date: new Date().toLocaleDateString('fr-FR') };
            let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
            cmds.push(cmd);
            localStorage.setItem('sc_cmds', JSON.stringify(cmds));
            
            fid[t] = (fid[t] || 0) + Math.floor(tf/1000);
            localStorage.setItem('sc_fid', JSON.stringify(fid));
            stock -= 0.1;
            localStorage.setItem('sc_stock', stock);

            imprimerTicket(cmd.id);
            location.reload();
        }

        function afficherCommandes() {
            let cmds = JSON.parse(localStorage.getItem('sc_cmds')) || [];
            document.getElementById('listeCommandes').innerHTML = '<h3>Commandes Actives</h3>' + cmds.map(c => `
                <div class="commande-item">
                    <div><strong>${c.client}</strong><br><span class="badge ${c.cls}">${c.stat}</span></div>
                    <button onclick="imprimerTicket(${c.id})" style="width:auto; padding:5px;">Ticket</button>
                </div>
            `).join('');
        }

        function imprimerTicket(id) {
            const c = JSON.parse(localStorage.getItem('sc_cmds')).find(x => x.id === id);
            document.getElementById('ticket-print').innerHTML = `
                <div style="text-align:center; padding:10px;">
                    <h2>SUPER CLEAN</h2><p>Nkounga, Immeuble de Karche</p><hr>
                    <p style="text-align:left;">Client: ${c.client}<br>Date: ${c.date}</p><hr>
                    ${c.arts.map(a => `<div>${a.n} x${a.q} : ${a.t}F</div>`).join('')}<hr>
                    <h3 style="text-align:right;">TOTAL: ${c.tot} F</h3>
                    <p style="text-align:left;">Payé: ${c.av} F<br><strong>RESTE: ${c.rest} F</strong></p>
                    <p>Merci !</p>
                </div>`;
            window.print();
        }

        function majStock() { stock += parseFloat(document.getElementById('add-stock').value) || 0; localStorage.setItem('sc_stock', stock); location.reload(); }

        window.onload = () => { chargerCatalogue(); afficherCommandes(); document.getElementById('stock-val').innerText = stock.toFixed(1); };
    </script>
</body>
</html>
