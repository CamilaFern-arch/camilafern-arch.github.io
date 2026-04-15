
<html lang="es">
    <style></style>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <title>Mi Control de Medicación</title>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Mi Planner 💊">
    
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/822/822143.png">
    <link rel="icon" type="image/png" href="https://cdn-icons-png.flaticon.com/512/822/822143.png">

    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@300;400;600;700;800&family=DM+Mono:wght@400;500&family=Merriweather:ital,wght@0,700;1,400&display=swap" rel="stylesheet">
    
    <style>
        /* Mantenemos tus variables de color originales */
        :root {
            --teal: #3a9ea5; --teal-light:#e6f5f6; --teal-mid: #b2dfe2; --mint: #f0faf9;
            --blue: #4a78c4; --blue-light:#eef2fb; --lavender: #7c6fcd; --lav-light: #f0eefb;
            --rose: #d95f76; --rose-light:#fdeef1; --amber: #e0913a; --amb-light: #fef4e8;
            --green: #3aaa6e; --grn-light: #eafaf2; --gray-50: #f8fafc; --gray-100: #f1f5f9;
            --gray-200: #e2e8f0; --gray-400: #94a3b8; --gray-600: #475569; --gray-800: #1e293b;
            --white: #ffffff; --border: #dde8ea; --shadow: 0 2px 16px rgba(58,158,165,0.10);
        }

        * { margin:0; padding:10; box-sizing:border-box; }
        body { font-family: 'Nunito', sans-serif; background: #ecf4f5; color: var(--gray-800); min-height: 100vh; padding: 24px 16px 48px; }

        /* Estilos de Botones y Diseño (Se mantienen igual para no romper la estética) */
        .toolbar { display: flex; justify-content: center; gap: 10px; margin-bottom: 28px; flex-wrap: wrap; }
        .btn { font-family: 'Nunito', sans-serif; font-weight: 700; font-size: 12px; letter-spacing: 0.5px; padding: 9px 20px; border-radius: 50px; border: 2px solid var(--teal); background: var(--teal); color: white; cursor: pointer; transition: all 0.18s; display: flex; align-items: center; gap: 6px; }
        .btn:hover { background: #2e8a91; transform: translateY(-1px); box-shadow: 0 4px 14px rgba(58,158,165,0.25); }
        .btn.ghost { background: white; color: var(--teal); }
        .btn.danger { background: white; color: var(--rose); border-color: var(--rose); }

        .page { background: white; max-width: 900px; margin: 0 auto 32px; border-radius: 16px; box-shadow: var(--shadow); overflow: hidden; border: 1.5px solid var(--border); }
        .page-header { background: linear-gradient(135deg, var(--teal) 0%, #2e8a91 60%, var(--blue) 100%); padding: 28px 36px 22px; position: relative; overflow: hidden; }
        .page-title { font-family: 'Merriweather', serif; font-size: 28px; font-weight: 700; color: white; line-height: 1.1; }
        .page-tagline { font-size: 12px; color: rgba(255,255,255,0.80); margin-top: 4px; }

        .hfield { display: flex; flex-direction: column; gap: 3px; }
        .hfield label { font-size: 9px; font-weight: 700; letter-spacing: 1.5px; text-transform: uppercase; color: rgba(255,255,255,0.70); }
        .hfield input { font-family: 'Nunito', sans-serif; font-size: 13px; font-weight: 600; color: white; background: rgba(255,255,255,0.15); border: 1.5px solid rgba(255,255,255,0.30); border-radius: 8px; padding: 5px 10px; outline: none; width: 160px; }

        .patient-strip { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; background: var(--gray-50); border: 1.5px solid var(--gray-200); border-radius: 12px; padding: 14px 18px; margin-bottom: 24px; }
        .sec-label { font-size: 10px; font-weight: 800; letter-spacing: 2px; text-transform: uppercase; color: var(--teal); margin-bottom: 12px; display: flex; align-items: center; gap: 8px; }
        .sec-label::after { content: ''; flex: 1; height: 1.5px; background: var(--teal-mid); }

        /* Med Cards */
        .med-card { border: 1.5px solid var(--border); border-radius: 12px; overflow: hidden; margin-bottom: 14px; }
        .med-card-header { display: grid; grid-template-columns: 28px 1fr 1fr 1fr 1fr 40px; gap: 10px; align-items: center; padding: 10px 14px; background: var(--teal-light); border-bottom: 1.5px solid var(--teal-mid); }
        .med-num { width: 24px; height: 24px; border-radius: 50%; background: var(--teal); color: white; font-size: 11px; font-weight: 800; display: flex; align-items: center; justify-content: center; }
        .med-input { font-family: 'Nunito', sans-serif; font-size: 13px; font-weight: 700; border: none; background: transparent; border-bottom: 1.5px solid var(--teal-mid); outline: none; width: 100%; }
        
        /* Time Pills */
        .time-pills-row { display: flex; gap: 6px; padding: 10px 14px; flex-wrap: wrap; }
        .time-pill { display: flex; align-items: center; gap: 5px; background: var(--blue-light); border: 1.5px solid #c5d5f0; border-radius: 50px; padding: 4px 10px; cursor: pointer; font-size: 11px; font-weight: 700; }
        .time-pill.active { color: white; }
        .time-pill.am.active { background: var(--amber); border-color: var(--amber); }
        .time-pill.noon.active { background: var(--green); border-color: var(--green); }
        .time-pill.pm.active { background: var(--lavender); border-color: var(--lavender); }
        .time-pill.night.active { background: #4a5abf; border-color: #4a5abf; }

        /* Tracker Table */
        .tracker-wrap { overflow-x: auto; border-radius: 12px; border: 1.5px solid var(--border); margin-bottom: 24px; }
        .tracker-table { width: 100%; border-collapse: collapse; min-width: 620px; }
        .tracker-table thead tr th { padding: 10px 8px; font-size: 9px; font-weight: 800; color: white; background: var(--teal); text-align: center; }
        .check-box { width: 22px; height: 22px; border-radius: 6px; border: 2px solid var(--gray-200); cursor: pointer; display: inline-flex; align-items: center; justify-content: center; transition: all 0.15s; margin: auto; }
        .check-box.done { background: var(--green); border-color: var(--green); color: white; }
        .check-box.skip { background: var(--rose-light); border-color: var(--rose); color: var(--rose); }

        .notes-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 24px; }
        .note-card { border: 1.5px solid var(--border); border-radius: 12px; overflow: hidden; }
        .note-card-header { padding: 8px 14px; font-size: 10px; font-weight: 800; text-transform: uppercase; }
        textarea { width: 100%; height: 80px; border: none; padding: 10px 14px; font-size: 12px; outline: none; resize: none; }

        @media (max-width: 640px) { .patient-strip, .notes-grid { grid-template-columns: 1fr; } .med-card-header { grid-template-columns: 28px 1fr 1fr; } }
    </style>
</head>

<body>

    <div class="toolbar no-print">
        <button class="btn" onclick="window.print()">🖨️ Imprimir</button>
        <button class="btn ghost" onclick="saveAll()">💾 Guardar</button>
        <button class="btn ghost" onclick="loadAll()">📂 Cargar</button>
        <button class="btn danger" onclick="clearAll()">↺ Reiniciar</button>
    </div>

    <div class="page">
        <div class="page-header">
            <div class="header-top">
                <div>
                    <span style="font-size: 36px;">💊</span>
                    <div class="page-title">Planner de Medicación</div>
                    <div class="page-tagline">Control semanal · Seguro · Organizado · Fácil</div>
                </div>
                <div class="header-fields">
                    <div class="hfield">
                        <label>Semana del</label>
                        <input type="text" id="weekOf" placeholder="7 – 13 Abr, 2026">
                    </div>
                    <div class="hfield">
                        <label>Cuidador / Responsable</label>
                        <input type="text" id="careProvider" placeholder="Nombre">
                    </div>
                </div>
            </div>
        </div>

        <div class="page-body">
            <div class="sec-label">👤 Información del Paciente</div>
            <div class="patient-strip">
                <div class="pfield"><label>Nombre Completo</label><input type="text" placeholder="Ej. Juan Pérez" id="patName"></div>
                <div class="pfield"><label>F. Nacimiento</label><input type="text" placeholder="DD / MM / AAAA" id="patDOB"></div>
                <div class="pfield"><label>Médico Tratante</label><input type="text" placeholder="Dr. Nombre" id="patDoc"></div>
                <div class="pfield"><label>Alergias / Notas</label><input type="text" placeholder="Ninguna..." id="patAllergies"></div>
            </div>

            <div class="sec-label">💊 Medicamentos</div>
            <div id="medCards"></div>
            <button class="btn ghost" style="width:100%; justify-content:center; border-style:dashed; margin-bottom:24px;" onclick="addMed()">＋ Añadir Medicamento</button>

            <div class="sec-label">📅 Seguimiento Semanal de Dosis</div>
            <div class="tracker-wrap">
                <table class="tracker-table">
                    <thead>
                        <tr>
                            <th style="text-align:left; padding-left:14px;">Medicamento / Dosis / Hora</th>
                            <th>LUN</th><th>MAR</th><th>MIÉ</th><th>JUE</th><th>VIE</th><th>SÁB</th><th>DOM</th>
                        </tr>
                    </thead>
                    <tbody id="trackerBody"></tbody>
                </table>
            </div>

            <div class="sec-label">📝 Notas y Observaciones</div>
            <div class="notes-grid">
                <div class="note-card">
                    <div class="note-card-header" style="background:var(--teal-light); color:var(--teal);">🩺 Efectos Secundarios</div>
                    <textarea placeholder="Anote cualquier reacción, mareos, náuseas..."></textarea>
                </div>
                <div class="note-card">
                    <div class="note-card-header" style="background:var(--rose-light); color:var(--rose);">⚠️ Dosis Olvidadas</div>
                    <textarea placeholder="Registro de dosis no tomadas y el motivo..."></textarea>
                </div>
                <div class="note-card">
                    <div class="note-card-header" style="background:var(--amber-light); color:var(--amber);">📞 Instrucciones Médicas</div>
                    <textarea placeholder="Notas de la última consulta o llamada..."></textarea>
                </div>
                <div class="note-card">
                    <div class="note-card-header" style="background:var(--lav-light); color:var(--lavender);">🔄 Cambios en Tratamiento</div>
                    <textarea placeholder="Nuevas recetas o medicinas suspendidas..."></textarea>
                </div>
            </div>

            <div class="sec-label">🔃 Recordatorio de Reposición</div>
            <div style="display:grid; grid-template-columns:repeat(4,1fr); gap:10px;" id="refillGrid"></div>
        </div>

        <div class="page-footer" style="padding: 20px 36px; background: var(--gray-50); display:flex; justify-content: space-between; align-items:center;">
            <div style="display:flex; gap:15px; font-size:10px; font-weight:700;">
                <span>LEYENDA:</span>
                <span><span style="color:var(--green)">✓</span> Tomada</span>
                <span><span style="color:var(--rose)">✗</span> Saltada</span>
                <span><span>⬜</span> Pendiente</span>
            </div>
            <div style="text-align:right;">
                <label style="font-size:9px; font-weight:800; color:var(--teal);">FIRMA DEL CUIDADOR</label><br>
                <input type="text" placeholder="Firme aquí..." style="border:none; border-bottom:1px solid #ccc; background:transparent; font-style:italic;">
            </div>
        </div>
    </div>

    <script>
        // Lógica de programación (Se mantiene en inglés para compatibilidad, pero el contenido es español)
        let meds = [{ name:'', dose:'', route:'', prescriber:'', notes:'', times:[] }];
        const TIMES = [
            { id:'am', icon:'🌅', label:'Mañana', cls:'am' },
            { id:'noon', icon:'☀️', label:'Mediodía', cls:'noon' },
            { id:'pm', icon:'🌇', label:'Tarde', cls:'pm' },
            { id:'night', icon:'🌙', label:'Noche', cls:'night'},
        ];

        function renderMeds() {
            const container = document.getElementById('medCards');
            container.innerHTML = '';
            meds.forEach((med, i) => {
                const div = document.createElement('div');
                div.className = 'med-card';
                div.innerHTML = `
                    <div class="med-card-header">
                        <div class="med-num">${i+1}</div>
                        <div><input class="med-input" placeholder="Nombre" value="${med.name}" oninput="meds[${i}].name=this.value; syncTracker()"></div>
                        <div><input class="med-input" placeholder="Dosis" value="${med.dose}" oninput="meds[${i}].dose=this.value; syncTracker()"></div>
                        <div><input class="med-input" placeholder="Vía (Oral/IV)" value="${med.route||''}" oninput="meds[${i}].route=this.value"></div>
                        <div><input class="med-input" placeholder="Médico" value="${med.prescriber||''}" oninput="meds[${i}].prescriber=this.value"></div>
                        <button onclick="removeMed(${i})" style="border:none; background:none; cursor:pointer; color:var(--rose);">✕</button>
                    </div>
                    <div class="time-pills-row">
                        <span style="font-size:9px; font-weight:800; margin-right:10px; color:var(--gray-400)">HORARIO:</span>
                        ${TIMES.map(t => `<div class="time-pill ${t.cls} ${med.times.includes(t.id) ? 'active' : ''}" onclick="toggleTime(${i},'${t.id}',this)">${t.icon} ${t.label}</div>`).join('')}
                    </div>
                `;
                container.appendChild(div);
            });
            syncTracker();
            renderRefills();
        }

        function toggleTime(i, t, el) {
            const idx = meds[i].times.indexOf(t);
            if(idx === -1) { meds[i].times.push(t); el.classList.add('active'); }
            else { meds[i].times.splice(idx,1); el.classList.remove('active'); }
            syncTracker();
        }

        function addMed() { meds.push({ name:'', dose:'', route:'', prescriber:'', notes:'', times:[] }); renderMeds(); }
        function removeMed(i) { if(meds.length > 1) { meds.splice(i,1); renderMeds(); } }

        let checkStates = {};
        function syncTracker() {
            const tbody = document.getElementById('trackerBody');
            tbody.innerHTML = '';
            meds.forEach((med, mi) => {
                if(!checkStates[mi]) checkStates[mi] = [0,0,0,0,0,0,0];
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td style="padding-left:14px;">
                        <div style="font-weight:800; font-size:12px;">${med.name || 'Medicamento'}</div>
                        <div style="font-size:10px; color:var(--teal)">${med.dose || '-'}</div>
                    </td>
                    ${[0,1,2,3,4,5,6].map(di => `<td><div class="check-box ${checkStates[mi][di]===1?'done':checkStates[mi][di]===2?'skip':''}" onclick="cycleCheck(${mi},${di},this)">${checkStates[mi][di]===1?'✓':checkStates[mi][di]===2?'✗':''}</div></td>`).join('')}
                `;
                tbody.appendChild(tr);
            });
        }

        function cycleCheck(mi, di, el) {
            checkStates[mi][di] = (checkStates[mi][di] + 1) % 3;
            const s = checkStates[mi][di];
            el.className = 'check-box ' + (s===1?'done':s===2?'skip':'');
            el.textContent = s===1?'✓':s===2?'✗':'';
        }

        function renderRefills() {
            const grid = document.getElementById('refillGrid');
            grid.innerHTML = '';
            meds.forEach((med, i) => {
                const card = document.createElement('div');
                card.style.cssText = 'border:1.5px solid var(--border); border-radius:10px; padding:10px; background:var(--gray-50);';
                card.innerHTML = `<div style="font-size:9px; font-weight:800; color:var(--teal);">${med.name || 'Med '+ (i+1)}</div><input type="text" placeholder="Próx. Compra" style="font-size:11px; border:none; width:100%;" value="${med.refillDate||''}" oninput="meds[${i}].refillDate=this.value">`;
                grid.appendChild(card);
            });
        }

        function saveAll() { localStorage.setItem('med_planner', JSON.stringify({meds, checkStates})); alert('¡Guardado en este dispositivo! 💾'); }
        function loadAll() { const data = JSON.parse(localStorage.getItem('med_planner')); if(data){ meds=data.meds; checkStates=data.checkStates; renderMeds(); } }
        function clearAll() { if(confirm('¿Borrar todo?')) { localStorage.clear(); location.reload(); } }

        window.onload = renderMeds;
    </script>
</body>
</html>
