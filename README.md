yId('reception-form').classList.add('hidden');
        } else {
            document.getElementById('expense-panel').classList.remove('hidden');
        }

        let h = ""; 
        for(let cat in GRILLE) {
            h += `<optgroup label="${cat}">`;
            GRILLE[cat].forEach(i => h += `<option value="${i.p}">${i.n} (${i.p}F)</option>`);
            h += `</optgroup>`;
        }
        document.getElementById('article-list').innerHTML = h;
        document.getElementById('date_liv').valueAsDate = new Date(Date.now() + 86400000);
        update();
    }

    function ajouterPanier() {
        const s = document.getElementById('article-list');
        panier.push({ n: s.options[s.selectedIndex].text.split(' (')[0], p: parseInt(s.value), note: document.getElementById('article-note').value });
        document.getElementById('panier-zone').style.display = 'block';
        document.getElementById('panier-items').innerHTML = panier.map(i => `• ${i.n} [${i.note}]`).join('<br>');
        document.getElementById('total-calc').innerText = panier.reduce((s, i) => s + i.p, 0);
    }

    function valider() {
        const c = document.getElementById('client').value, t = document.getElementById('tel').value, dl = document.getElementById('date_liv').value;
        const av = parseInt(document.getElementById('avance-client').value) || 0;
        if(!c || !t || !dl || panier.length === 0) return alert("Veuillez remplir tous les champs !");
        
        const totalCmd = panier.reduce((s, i) => s + i.p, 0);
        const cmd = { 
            uid: "SC-"+Math.floor(1000 + Math.random() * 9000), c, t, 
            items: [...panier], tot: totalCmd, avance: av, reste: totalCmd - av,
            ts: Date.now(), liv: dl, date_rec: new Date().toLocaleDateString('fr-FR') 
        };
        
        let db = JSON.parse(localStorage.getItem('sc_db')) || [];
        db.push(cmd); localStorage.setItem('sc_db', JSON.stringify(db));
        imprimerTicket(cmd);
        
        panier = [];
        document.getElementById('client').value = "";
        document.getElementById('avance-client').value = "";
        document.getElementById('panier-zone').style.display = 'none';
        update();
    }

    function update() {
        const db = JSON.parse(localStorage.getItem('sc_db')) || [];
        const arc = JSON.parse(localStorage.getItem('sc_archives')) || [];
        const exp = JSON.parse(localStorage.getItem('sc_expenses')) || [];
        const todayStr = new Date().toLocaleDateString('fr-FR');
        const isoToday = new Date().toISOString().split('T')[0];
        
        // Finances
        if(user.r !== "LAVEUR") {
            let recJ = 0; let depJ = 0;
            [...db, ...arc].forEach(x => { if(x.date_rec === todayStr) recJ += x.avance; });
            exp.forEach(e => { if(e.date === todayStr) depJ += e.montant; });
            document.getElementById('sj').innerHTML = `<span style="color:var(--success)">+${recJ}</span> | <span style="color:var(--danger)">-${depJ}</span><br><b>${recJ-depJ} F</b>`;
            
            // Toolbar Admin
            if(user.r === "ADMIN") {
                document.getElementById('toolbar-admin').innerHTML = `
                    <div style="display:flex; gap:5px; justify-content:center; margin-top:10px;">
                        <button onclick="voirHistorique()" style="background:var(--dark); font-size:9px; width:auto; padding:5px 10px;">📂 LITIGES</button>
                        <button onclick="voirJournalDepenses()" style="background:var(--danger); font-size:9px; width:auto; padding:5px 10px;">💸 DÉPENSES</button>
                        <button onclick="sauvegarderDonnees()" style="background:var(--secondary); font-size:9px; width:auto; padding:5px 10px;">💾 BACKUP</button>
                    </div>`;
            }
        }

        // Stock
        let st = {}; let am = 0;
        db.forEach(c => c.items.forEach(i => { st[i.n] = (st[i.n] || 0) + 1; if(i.note==='Amidonné') am++; }));
        let sH = am > 0 ? `<div class="stock-item starch-item">🌾 AMIDON: ${am}</div>` : "";
        for(let a in st) sH += `<div class="stock-item">${a}: ${st[a]}</div>`;
        document.getElementById('stock-display').innerHTML = sH || "Atelier vide";

        // Liste
        document.getElementById('main-list').innerHTML = `<h4>📦 Production en cours</h4>` + db.reverse().map(x => {
            let uC = "", bC = "bg-gray", lbl = "Prévu: " + new Date(x.liv).toLocaleDateString('fr-FR');
            if (x.liv < isoToday) { uC = "retard-urgent"; bC = "bg-red"; lbl = "⚠️ RETARD"; }
            else if (x.liv === isoToday) { uC = "today-urgent"; bC = "bg-orange"; lbl = "🔥 AUJOURD'HUI"; }

            return `
            <div class="commande-card ${uC}">
                <span class="badge-id">${x.uid}</span>
                <strong>${x.c}</strong><br>
                <div class="status-badge ${bC}">${lbl}</div>
                <div style="font-size:11px; margin: 8px 0;">${x.items.map(i => `• ${i.n} [${i.note}]`).join('<br>')}</div>
                <div style="font-size:12px; color:var(--danger); font-weight:bold; margin-bottom:10px;">RESTE À PAYER: ${x.reste} F</div>
                <div style="display:flex; gap:5px;">
                   <button onclick="finish('${x.uid}')" style="background:var(--success); font-size:10px; padding:8px 12px;">LIVRÉ ✅</button>
                </div>
            </div>`;
        }).join('');
    }

    function finish(id) {
        if(confirm("Confirmer la livraison et archiver pour historique ?")) {
            let db = JSON.parse(localStorage.getItem('sc_db')) || [];
            let archives = JSON.parse(localStorage.getItem('sc_archives')) || [];
            const idx = db.findIndex(x => x.uid === id);
            if(idx > -1) {
                let cmd = db[idx];
                cmd.date_livraison_reelle = new Date().toLocaleString('fr-FR');
                archives.push(cmd);
                db.splice(idx, 1);
                localStorage.setItem('sc_db', JSON.stringify(db));
                localStorage.setItem('sc_archives', JSON.stringify(archives));
                update();
            }
        }
    }

    function ajouterDepense() {
        const l = document.getElementById('exp-label').value, a = parseInt(document.getElementById('exp-amount').value);
        if(!l || !a) return alert("Remplissez la dépense !");
        let exps = JSON.parse(localStorage.getItem('sc_expenses')) || [];
        exps.push({ label:l, montant:a, date:new Date().toLocaleDateString('fr-FR'), ts:Date.now() });
        localStorage.setItem('sc_expenses', JSON.stringify(exps));
        document.getElementById('exp-label').value = ""; document.getElementById('exp-amount').value = "";
        update();
    }

    function voirHistorique() {
        const arc = JSON.parse(localStorage.getItem('sc_archives')) || [];
        let html = `<div id="m-h" style="position:fixed; top:0; left:0; width:100%; height:100%; background:white; z-index:10000; padding:15px; overflow-y:auto;">
            <button onclick="document.getElementById('m-h').remove()" style="background:var(--danger)">FERMER</button>
            <input type="text" id="s-h" onkeyup="filterH()" placeholder="🔍 Chercher client ou ID..." style="margin:10px 0; border:2px solid var(--primary)">
            <div id="h-cont">` + arc.reverse().map(x => `<div class="h-item" style="border-bottom:1px solid #ddd; padding:10px;"><b>${x.c}</b> (${x.uid})<br><small>Recu: ${x.date_rec} | Sorti: ${x.date_livraison_reelle}<br>Articles: ${x.items.map(i=>i.n).join(',')}</small></div>`).join('') + `</div></div>`;
        document.body.insertAdjacentHTML('beforeend', html);
    }
    function filterH() { 
        let v = document.getElementById('s-h').value.toLowerCase();
        document.querySelectorAll('.h-item').forEach(i => i.style.display = i.innerText.toLowerCase().includes(v) ? 'block' : 'none');
    }

    function voirJournalDepenses() {
        const ex = JSON.parse(localStorage.getItem('sc_expenses')) || [];
        let html = `<div id="m-e" style="position:fixed; top:0; left:0; width:100%; height:100%; background:white; z-index:10001; padding:15px; overflow-y:auto;">
            <button onclick="document.getElementById('m-e').remove()" style="background:var(--dark)">FERMER JOURNAL</button>
            <h3 style="color:var(--danger)">DÉPENSES EFFECTUÉES</h3>` + 
            ex.reverse().map(e => `<div style="padding:10px; border-bottom:1px solid #eee;">${e.date} : <b>${e.label}</b> - <span style="color:red">${e.montant} F</span></div>`).join('') + `</div>`;
        document.body.insertAdjacentHTML('beforeend', html);
    }

    function sauvegarderDonnees() {
        const data = { prod: JSON.parse(localStorage.getItem('sc_db')), arc: JSON.parse(localStorage.getItem('sc_archives')), exp: JSON.parse(localStorage.getItem('sc_expenses')) };
        const blob = new Blob([JSON.stringify(data, null, 2)], {type : 'application/json'});
        const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = 'backup_super_clean.json'; a.click();
    }

    function imprimerTicket(cmd) {
        const t = document.getElementById('ticket-print');
        t.innerHTML = `<div style="text-align:center; font-family:monospace; padding:10px;">
            <h3>SUPER CLEAN PRESSING</h3>
            <p>ID: ${cmd.uid} | Client: ${cmd.c}</p><hr>
            <div style="text-align:left">${cmd.items.map(i=>`• ${i.n} [${i.note}]`).join('<br>')}</div><hr>
            <div style="text-align:right">TOTAL: ${cmd.tot} F<br><b>AVANCE: ${cmd.avance} F</b><br>RESTE: ${cmd.reste} F</div><hr>
            <p>Date retrait: ${new Date(cmd.liv).toLocaleDateString('fr-FR')}</p>
        </div>`;
        window.print();
    }

    function logout() { sessionStorage.clear(); location.reload(); }
    window.onload = () => { if(sessionStorage.getItem('sc_u')) { user = JSON.parse(sessionStorage.getItem('sc_u')); init(); } };
</script>
</body>
</html>
