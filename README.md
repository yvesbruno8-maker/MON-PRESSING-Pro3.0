<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN PRESSING - GESTION PRO V3</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
    
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --accent: #00d2d3;
        }
        
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); }
        
        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 6px solid var(--primary); }
        .card { background: white; padding: 15px; border-radius: 20px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); margin-bottom: 15px; }
        
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; margin-bottom: 10px; }
        .stat-card { background: white; padding: 10px; border-radius: 15px; text-align: center; border-bottom: 4px solid var(--primary); }
        .stat-card b { font-size: 11px; color: var(--primary); display:block; }
        .stat-card span { font-size: 8px; font-weight: bold; color: gray; text-transform: uppercase; }

        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 12px; border: 1px solid #ddd; font-size: 14px; box-sizing: border-box; outline:none; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; }
        button:active { transform: scale(0.98); }

        .commande-card { background: white; padding: 15px; margin-bottom: 15px; border-radius: 18px; position: relative; box-shadow: 0 4px 6px rgba(0,0,0,0.05); transition: 0.3s; }
        .badge-id { position: absolute; top: 12px; right: 12px; font-size: 11px; color: white; padding: 3px 10px; border-radius: 6px; font-family: monospace; }
        
        .switch { position: relative; display: inline-block; width: 40px; height: 20px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #ccc; transition: .4s; border-radius: 20px; }
        input:checked + .slider { background-color: var(--success); }

        .hidden { display: none !important; }
        
        @media print {
            body * { visibility: hidden; }
            #ticket-print, #ticket-print * { visibility: visible; }
            #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; font-family: 'Courier New', monospace; }
        }
        #ticket-print { display: none; padding: 10px; background: white; color: black; }
    </style>
</head>
<body>

<div id="auth-screen" style="height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background:var(--primary); color:white; padding:20px; text-align:center;">
    <h1>SUPER CLEAN</h1>
    <p>Gestion Pressing & Blanchisserie</p>
    <div style="width: 100%; max-width: 320px; margin-top:25px; background:rgba(255,255,255,0.1); padding:20px; border-radius:20px;">
        <input type="password" id="u-pass" placeholder="Code d'accès (1234)">
        <button onclick="login()" style="background:var(--success); height:55px; margin-top:15px;">CONNEXION</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="header">
        <h3 style="margin:0; color:var(--primary);">Super CLEAN Pressing</h3>
        <p id="user-role" style="font-size:10px; color: #777;"></p>
        <div style="display:flex; gap:5px; margin-top:10px;">
            <button onclick="genererRapportPDF()" style="background:var(--dark); font-size:10px;">📄 BILAN PDF</button>
            <button onclick="exporterDonnees()" style="background:var(--secondary); font-size:10px;">💾 BACKUP JSON</button>
        </div>
    </div>

    <div id="admin-stats" class="card">
        <div class="stats-grid">
            <div class="stat-card" style="border-color:var(--success)"><span>CA GLOBAL</span><b id="sj">0 F</b></div>
            <div class="stat-card"><span>COMMANDES</span><b id="sm">0</b></div>
            <div class="stat-card" style="border-color:var(--danger)"><span>A PERCEVOIR</span><b id="sn">0 F</b></div>
        </div>
    </div>

    <div class="card">
        <h4 style="margin:0 0 10px 0;">📝 Nouvelle Réception</h4>
        <input type="text" id="client" placeholder="Nom du Client">
        <input type="tel" id="tel" placeholder="Téléphone WhatsApp (ex: 690...)">
        
        <div style="display: flex; gap: 5px; margin-top:10px;">
            <select id="select-article" style="flex:2;"></select>
            <input type="number" id="qte" value="1" min="1" style="flex:0.5;">
            <button onclick="ajouterAuPanier()" style="flex:0.5; background:var(--accent);">+</button>
        </div>

        <div id="liste-panier" style="background:#f9f9f9; padding:10px; border-radius:10px; margin:10px 0; font-size:12px; min-height:40px;">
            <p style="text-align:center; color:#999;">Panier vide</p>
        </div>

        <div style="border-top:1px solid #eee; padding-top:10px;">
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:5px;">
                <select id="mode-paiement">
                    <option value="Espèces">💵 Espèces</option>
                    <option value="Orange Money">🍊 Orange Money</option>
                    <option value="Mobile Money">🟡 MoMo</option>
                </select>
                <select id="type-paiement" onchange="toggleAvance()">
                    <option value="Payé">✅ Payé (Total)</option>
                    <option value="Avance">💰 Avance</option>
                    <option value="À la livraison">⏳ À la livraison</option>
                </select>
            </div>
            <input type="number" id="montant-avance" placeholder="Montant versé" style="display:none; border:2px solid var(--warning);">
        </div>

        <div style="display:flex; justify-content:space-between; align-items:center; margin-top:10px;">
            <span>TOTAL : <strong id="total-panier" style="color:var(--danger); font-size:18px;">0 F</strong></span>
            <button onclick="creerCommande()" style="width:auto; background:var(--success); padding:10px 20px;">ENREGISTRER</button>
        </div>
    </div>

    <div class="card" style="background:var(--secondary);">
        <input type="text" id="recherche" onkeyup="afficherCommandes()" placeholder="🔍 Chercher un client ou ticket...">
        <div style="display:flex; align-items:center; justify-content:space-between; color:white; font-size:11px; margin-top:5px;">
            <span>Masquer les tickets livrés</span>
            <label class="switch">
                <input type="checkbox" id="masquer-livre" onchange="afficherCommandes()" checked>
                <span class="slider"></span>
            </label>
        </div>
    </div>

    <div id="liste-commandes"></div>
</div>

<div id="ticket-print">
    <div style="text-align:center; border-bottom:1px dashed #000; padding-bottom:5px;">
        <h2 style="margin:5px 0;">SUPER CLEAN</h2>
        <p style="font-size:11px;">Pressing PRO | Tél: +237 6XX XXX XXX</p>
    </div>
    <div style="margin:10px 0; font-size:12px;">
        <p><strong>TICKET :</strong> <span id="t-id"></span></p>
        <p><strong>DATE :</strong> <span id="t-date"></span></p>
        <p><strong>CLIENT :</strong> <span id="t-client"></span></p>
    </div>
    <div id="t-details" style="border-top:1px solid #000; padding:5px 0; font-size:11px;"></div>
    <div style="border-top:1px solid #000; border-bottom:1px solid #000; padding:5px 0; font-size:12px;">
        <p style="display:flex; justify-content:space-between; margin:2px 0;"><span>TOTAL :</span> <span id="t-montant"></span></p>
        <p style="display:flex; justify-content:space-between; margin:2px 0;"><span>AVANCE :</span> <span id="t-avance"></span></p>
        <p style="display:flex; justify-content:space-between; margin:2px 0;"><strong>RESTE :</strong> <strong id="t-reste"></strong></p>
    </div>
    <p style="font-size:9px; text-align:center; margin-top:10px;">Merci de votre confiance !</p>
</div>

<script>
    // 1. BASE DE DONNÉES (De ton image)
    const TARIFS = {
        "HAUTS": {"T-Shirt":500, "Chemise":500, "Polo":500, "Pull Over":700, "Blouson":1000, "Veste 2p":1500, "Veste 3p":2000},
        "BAS": {"Pantalon":500, "Jeans":500, "Short":500, "Jupe":500},
        "ENSEMBLES": {"Costume":2000, "Boubou":1500, "Jogging":1500, "Pyjama":1000},
        "LINGE": {"Drap":750, "Couette":2000, "Serviette":1000, "Rideau":1500},
        "AUTRES": {"Robe":1000, "Robe soirée":3000, "Cravate":500, "Tennis":500}
    };

    let commandes = JSON.parse(localStorage.getItem('pressing_data_v3')) || [];
    let panierActuel = [];

    // 2. FONCTIONS DE DÉMARRAGE
    function login() {
        if(document.getElementById('u-pass').value === "1234") {
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('app').classList.remove('hidden');
            initialiserTarifs();
            afficherCommandes();
            calculerStats();
        } else { alert("Code incorrect !"); }
    }

    function initialiserTarifs() {
        const select = document.getElementById('select-article');
        for (let cat in TARIFS) {
            let group = document.createElement('optgroup'); group.label = cat;
            for (let art in TARIFS[cat]) {
                let opt = document.createElement('option');
                opt.value = TARIFS[cat][art]; opt.innerText = `${art} (${TARIFS[cat][art]} F)`;
                group.appendChild(opt);
            }
            select.appendChild(group);
        }
    }

    function toggleAvance() {
        const val = document.getElementById('type-paiement').value;
        document.getElementById('montant-avance').style.display = (val === 'Avance') ? 'block' : 'none';
    }

    // 3. GESTION DU PANIER
    function ajouterAuPanier() {
        const sel = document.getElementById('select-article');
        const qte = parseInt(document.getElementById('qte').value);
        panierActuel.push({
            designation: sel.options[sel.selectedIndex].text.split(' (')[0],
            prix: parseFloat(sel.value),
            qte: qte,
            st: parseFloat(sel.value) * qte
        });
        majPanier();
    }

    function majPanier() {
        const div = document.getElementById('liste-panier');
        let total = 0;
        div.innerHTML = panierActuel.map((it, i) => {
            total += it.st;
            return `<div style="display:flex; justify-content:space-between; border-bottom:1px solid #eee; padding:5px 0;">
                <span>${it.qte}x ${it.designation}</span>
                <b>${it.st} F <span onclick="panierActuel.splice(${i},1);majPanier()" style="color:red; cursor:pointer; margin-left:10px;">✕</span></b>
            </div>`;
        }).join('') || '<p style="text-align:center; color:#999;">Panier vide</p>';
        document.getElementById('total-panier').innerText = total + " F";
    }

    // 4. LOGIQUE COMMANDE
    function creerCommande() {
        const client = document.getElementById('client').value;
        const total = panierActuel.reduce((s, i) => s + i.st, 0);
        if(!client || total === 0) return alert("Veuillez remplir le client et le panier !");

        const typeP = document.getElementById('type-paiement').value;
        const avance = typeP === 'Avance' ? parseFloat(document.getElementById('montant-avance').value) || 0 : (typeP === 'Payé' ? total : 0);
        const reste = total - avance;

        const cmd = {
            id: "SC-" + Math.floor(Math.random()*9000+1000),
            client: client,
            tel: document.getElementById('tel').value,
            articles: [...panierActuel],
            montant: total,
            avance: avance,
            reste: reste,
            mode: document.getElementById('mode-paiement').value,
            type: typeP,
            date: new Date().toISOString(),
            status: 'En attente'
        };

        commandes.unshift(cmd);
        localStorage.setItem('pressing_data_v3', JSON.stringify(commandes));
        
        // Reset
        panierActuel = []; majPanier(); 
        document.getElementById('client').value = ""; document.getElementById('tel').value = "";
        afficherCommandes(); calculerStats();
        alert("Enregistré avec succès !");
    }

    function changerStatut(id, st) {
        const idx = commandes.findIndex(c => c.id === id);
        commandes[idx].status = st;
        localStorage.setItem('pressing_data_v3', JSON.stringify(commandes));
        afficherCommandes();
    }

    function afficherCommandes() {
        const container = document.getElementById('liste-commandes');
        const terme = document.getElementById('recherche').value.toLowerCase();
        const masquer = document.getElementById('masquer-livre').checked;
        
        container.innerHTML = "";
        commandes.filter(c => {
            const match = c.client.toLowerCase().includes(terme) || c.id.toLowerCase().includes(terme);
            return masquer ? (match && c.status !== 'Livré') : match;
        }).forEach(c => {
            const col = c.status === 'Prêt' ? '#2ecc71' : (c.status === 'Livré' ? '#2f3542' : '#4a69bd');
            container.innerHTML += `
                <div class="commande-card" style="border-left: 8px solid ${col}">
                    <span class="badge-id" style="background:${col}">${c.id}</span>
                    <strong>${c.client.toUpperCase()}</strong><br>
                    <small>📞 ${c.tel} | ${c.mode}</small>
                    <div style="font-size:11px; color:#555; margin:5px 0; border-bottom:1px solid #eee;">
                        ${c.articles.map(a => `${a.qte}x ${a.designation}`).join(', ')}
                    </div>
                    <div style="display:flex; justify-content:space-between; font-size:12px;">
                        <b style="color:var(--success)">Encaissé: ${c.avance} F</b>
                        <b style="color:var(--danger)">Reste: ${c.reste} F</b>
                    </div>
                    <div style="display:grid; grid-template-columns: 1fr 1fr; gap:5px; margin-top:10px;">
                        <select onchange="changerStatut('${c.id}', this.value)" style="padding:5px; height:30px; margin:0;">
                            <option value="En attente" ${c.status==='En attente'?'selected':''}>Attente</option>
                            <option value="Prêt" ${c.status==='Prêt'?'selected':''}>Prêt</option>
                            <option value="Livré" ${c.status==='Livré'?'selected':''}>Livré</option>
                        </select>
                        <button onclick="imprimerTicket('${c.id}')" style="background:var(--dark); padding:5px;">🖨️ Ticket</button>
                    </div>
                    <div style="display:flex; gap:5px; margin-top:5px;">
                        <button onclick="envoyerWA('${c.id}','pret')" style="background:#25D366; font-size:10px;">WA Prêt</button>
                        <button onclick="envoyerWA('${c.id}','retard')" style="background:var(--warning); font-size:10px;">WA Retard</button>
                    </div>
                </div>`;
        });
    }

    // 5. OUTILS
    function envoyerWA(id, type) {
        const c = commandes.find(x => x.id === id);
        let msg = type === 'pret' ? `Bonjour ${c.client}, votre commande ${c.id} est prête chez Super Clean !` : `Bonjour ${c.client}, un léger retard est accusé sur votre commande ${c.id}.`;
        window.open(`https://wa.me/237${c.tel.replace(/\s/g,'')}?text=${encodeURIComponent(msg)}`, '_blank');
    }

    function calculerStats() {
        const totalCA = commandes.reduce((s,c) => s+c.montant, 0);
        const totalReste = commandes.reduce((s,c) => s+c.reste, 0);
        document.getElementById('sj').innerText = totalCA + " F";
        document.getElementById('sm').innerText = commandes.length;
        document.getElementById('sn').innerText = totalReste + " F";
    }

    function imprimerTicket(id) {
        const c = commandes.find(x => x.id === id);
        document.getElementById('t-id').innerText = c.id;
        document.getElementById('t-date').innerText = new Date(c.date).toLocaleString();
        document.getElementById('t-client').innerText = c.client;
        document.getElementById('t-montant').innerText = c.montant + " F";
        document.getElementById('t-avance').innerText = c.avance + " F";
        document.getElementById('t-reste').innerText = c.reste + " F";
        document.getElementById('t-details').innerHTML = c.articles.map(a => `<div style="display:flex; justify-content:space-between;"><span>${a.qte}x ${a.designation}</span><span>${a.st} F</span></div>`).join('');
        window.print();
    }

    function genererRapportPDF() {
        const { jsPDF } = window.jspdf; const doc = new jsPDF();
        doc.text("BILAN SUPER CLEAN PRESSING", 14, 20);
        const rows = commandes.map(c => [new Date(c.date).toLocaleDateString(), c.client, c.montant, c.avance, c.reste, c.mode]);
        doc.autoTable({ head: [['Date', 'Client', 'Total', 'Encaissé', 'Reste', 'Mode']], body: rows, startY: 30 });
        doc.save("Bilan_Pressing.pdf");
    }

    function exporterDonnees() {
        const blob = new Blob([JSON.stringify(commandes)], {type: 'application/json'});
        const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = "backup_pressing.json"; a.click();
    }
</script>
</body>
</html>
