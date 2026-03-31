<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPER CLEAN PRESSING - GESTION PRO</title>
    <style>
        :root { 
            --primary: #1e3799; --secondary: #4a69bd; --success: #2ecc71; 
            --danger: #e74c3c; --warning: #f39c12; --dark: #2f3542; 
            --light: #f1f2f6; --accent: #00d2d3;
        }
        
        body { font-family: 'Segoe UI', system-ui, sans-serif; margin: 0; padding: 10px; background: var(--light); color: var(--dark); line-height: 1.4; }
        
        /* Animations & Alertes */
        @keyframes blink { 50% { opacity: 0.4; } }
        .retard-urgent { border: 3px solid var(--danger) !important; animation: blink 1.2s infinite; }
        
        /* Composants Interface */
        .header { text-align: center; background: white; padding: 15px; border-radius: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); margin-bottom: 15px; border-top: 6px solid var(--primary); }
        .card { background: white; padding: 15px; border-radius: 20px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); margin-bottom: 15px; border: 1px solid #eee; }
        
        .stats-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; margin-bottom: 10px; }
        .stat-card { background: white; padding: 10px; border-radius: 15px; text-align: center; border-bottom: 4px solid var(--primary); }
        .stat-card b { font-size: 11px; color: var(--primary); display:block; }
        .stat-card span { font-size: 8px; font-weight: bold; color: gray; text-transform: uppercase; }

        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border-radius: 12px; border: 1px solid #ddd; font-size: 14px; box-sizing: border-box; outline:none; }
        button { background: var(--primary); color: white; border: none; font-weight: bold; cursor: pointer; transition: 0.2s; }
        button:active { transform: scale(0.96); }

        .commande-card { background: white; padding: 15px; margin-bottom: 15px; border-radius: 18px; position: relative; border-left: 8px solid var(--primary); box-shadow: 0 4px 6px rgba(0,0,0,0.02); }
        .badge-id { position: absolute; top: 12px; right: 12px; font-size: 11px; background: var(--dark); color: white; padding: 3px 10px; border-radius: 6px; font-family: monospace; }
        
        .hidden { display: none !important; }
        
        /* Design Impression Ticket */
        @media print {
            body * { visibility: hidden; }
            #ticket-print, #ticket-print * { visibility: visible; }
            #ticket-print { display: block !important; position: absolute; left: 0; top: 0; width: 80mm; padding: 10px; font-family: 'Courier New', Courier, monospace; color: black; background: white; }
        }
        #ticket-print { display: none; }
    </style>
</head>
<body>

<div id="auth-screen" style="height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; background:var(--primary); color:white; padding:20px; text-align:center;">
    <h1 style="margin-bottom:5px;">SUPER CLEAN</h1>
    <p style="opacity:0.8;">Pressing & Blanchisserie PRO</p>
    <div style="width: 100%; max-width: 320px; margin-top:25px; background:rgba(255,255,255,0.1); padding:20px; border-radius:20px;">
        <input type="tel" id="u-phone" placeholder="Numéro de téléphone">
        <input type="password" id="u-pass" placeholder="Mot de passe">
        <button onclick="login()" style="background:var(--success); height:55px; margin-top:15px; font-size:16px;">ACCÉDER AU SYSTÈME</button>
    </div>
</div>

<div class="hidden" id="app">
    <div class="header">
        <h3 style="margin:0; color:var(--primary); text-transform:uppercase;">Super CLEAN Pressing</h3>
        <p id="user-role" style="font-size:10px; font-weight:bold; color: #777; margin:5px 0;"></p>
    </div>

    <div id="admin-stats" class="hidden">
        <div class="stats-grid">
            <div class="stat-card" style="border-color:var(--success)"><span>CA JOUR</span><b id="sj">0 F</b></div>
            <div class="stat-card"><span>CA MOIS</span><b id="sm">0 F</b></div>
            <div class="stat-card" style="border-color:var(--danger)"><span>NET MOIS</span><b id="sn">0 F</b></div>
        </div>
        <div class="card" style="padding:10px; font-size:9px; display:flex; justify-content:space-between; background:#fdfdfd; border:1px solid #eee;">
            <span>💵 Cash: <b id="c-cash">0</b></span>
            <span>🍊 OM: <b id="c-om">0</b></span>
            <span>🟡 MoMo: <b id="c-momo">0</b></span>
            <span>💳 Visa: <b id="c-visa">0</b></span>
        </div>
    </div>

    <div class="card" id="reception-form">
        <h4 style="margin:0 0 10px 0; display:flex; align-items:center; gap:8px;">📝 Nouvelle Réception</h4>
        <input type="text" id="client" placeholder="Nom complet du Client">
        <input type="tel" id="tel" placeholder="WhatsApp (9
        
