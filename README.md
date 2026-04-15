<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Mi Control de Medicación 💊</title>

    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Mi Planner 💊">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/822/822143.png">
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Merriweather:ital,wght@1,700&display=swap" rel="stylesheet">

    <style>
        :root {
            --teal: #3a9ea5;
            --teal-mid: #7dbdb3;
            --teal-light: #ecf4f5;
            --gray-800: #1e293b;
            --gray-500: #64748b;
            --gray-200: #e2e8f0;
            --gray-50: #f8fafc;
            --white: #ffffff;
            --red: #ef4444;
        }

        /* ── LIMPIEZA PARA QUITAR NOMBRE DE USUARIO ── */
        header, #banner, .gh-header { display: none !important; }

        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8;
            color: var(--gray-800);
            line-height: 1.4;
            padding: 10px;
        }

        .page {
            max-width: 500px;
            margin: 0 auto;
            background: var(--white);
            border-radius: 16px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
            overflow: hidden;
        }

        /* ── BOTONES DE CONTROL ── */
        .toolbar {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            padding: 15px;
            background: var(--gray-50);
        }

        button {
            padding: 10px;
            border-radius: 12px;
            border: 1px solid var(--gray-200);
            background: white;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }

        button:active { transform: scale(0.98); }

        /* ── ENCABEZADO MÉDICO ── */
        .page-header {
            background: linear-gradient(135deg, var(--teal), #5ba9b0);
            color: white;
            padding: 25px 20px;
            position: relative;
        }

        .pill-icon { font-size: 32px; margin-bottom: 10px; display: block; }
        h1 { font-size: 24px; font-weight: 800; line-height: 1.1; }
        .sub-text { font-size: 12px; opacity: 0.9; margin-top: 5px; }

        .input-group { margin-top: 15px; }
        .input-group label { display: block; font-size: 10px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 4px; }
        .input-group input {
            width: 100%;
            background: rgba(255,255,255,0.2);
            border: 1px solid rgba(255,255,255,0.3);
            border-radius: 8px;
            padding: 8px;
            color: white;
            outline: none;
        }

        /* ── SECCIONES DE NOTAS ── */
        .section { padding: 15px 20px; border-bottom: 1px solid var(--gray-50); }
        .section-title {
            font-size: 11px;
            font-weight: 800;
            color: var(--teal);
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        textarea {
            width: 100%;
            border: 1px solid var(--gray-200);
            border-radius: 10px;
            padding: 10px;
            font-size: 14px;
            resize: none;
            color: var(--gray-500);
            font-family: inherit;
        }

        /* ── FOOTER Y FIRMA (MEJORADO) ── */
        .page-footer {
            background: var(--gray-50);
            padding: 25px 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .footer-legend {
            display: flex;
            justify-content: center;
            gap: 15px;
            font-size: 12px;
            font-weight: 600;
        }

        .legend-item { display: flex; align-items: center; gap: 5px; }
        .legend-box { width: 14px; height: 14px; border-radius: 3px; }
        .done { background: #dcfce7; border: 1px solid #22c55e; }
        .skip { background: #fee2e2; border: 1px solid #ef4444; }
        .empty { background: #f1f5f9; border: 1px solid var(--gray-200); }

        .footer-sig {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            width: 100%;
            padding-top: 15px;
            border-top: 1px dashed var(--gray-200);
        }

        .footer-sig label {
            font-size: 10px;
            font-weight: 800;
            color: var(--teal);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .footer-sig input {
            font-family: 'Merriweather', serif;
            font-style: italic;
            font-size: 18px;
            border: none;
            border-bottom: 2px solid var(--teal-mid);
            background: transparent;
            width: 80%;
            text-align: center;
            padding: 5px;
            outline: none;
        }

        @media print { .toolbar { display: none; } body { padding: 0; } .page { box-shadow: none; border-radius: 0; } }
    </style>
</head>
<body>

    <div class="page">
        <div class="toolbar">
            <button onclick="window.print()">🖨️ Imprimir</button>
            <button onclick="saveData()">💾 Guardar</button>
            <button onclick="loadData()">📂 Cargar</button>
            <button onclick="resetData()" style="color: var(--red);">↺ Reiniciar</button>
        </div>

        <div class="page-header">
            <span class="pill-icon">💊</span>
            <h1>Planner de Medicación</h1>
            <p class="sub-text">Control semanal · Seguro · Organizado</p>
            
            <div class="input-group">
                <label>Semana del</label>
                <input type="text" id="weekDate" placeholder="Ej. 7 - 13 Abr, 2026">
            </div>
            <div class="input-group">
                <label>Cuidador / Responsable</label>
                <input type="text" id="caregiver" placeholder="Nombre">
            </div>
        </div>

        <div class="section">
            <div class="section-title">👤 Información del Paciente</div>
            <textarea id="patientInfo" rows="3" placeholder="Nombre, fecha de nacimiento, alergias importantes..."></textarea>
        </div>

        <div class="section">
            <div class="section-title">⚠️ Dosis Olvidadas</div>
            <textarea id="missedDoses" rows="2" placeholder="Registro de dosis no tomadas y el motivo..."></textarea>
        </div>

        <div class="section">
            <div class="section-title">📞 Instrucciones Médicas</div>
            <textarea id="medicalNotes" rows="2" placeholder="Notas de la última consulta o llamada..."></textarea>
        </div>

        <div class="page-footer">
            <div class="footer-legend">
                <div class="legend-item"><div class="legend-box done"></div> Tomada</div>
                <div class="legend-item"><div class="legend-box skip"></div> Saltada</div>
                <div class="legend-item"><div class="legend-box empty"></div> Pendiente</div>
            </div>
            
            <div class="footer-sig">
                <label>Firma del Cuidador o Paciente</label>
                <input type="text" id="signature" placeholder="Firme aquí...">
            </div>
        </div>
    </div>

    <script>
        // Función para guardar datos en el teléfono
        function saveData() {
            const data = {
                weekDate: document.getElementById('weekDate').value,
                caregiver: document.getElementById('caregiver').value,
                patientInfo: document.getElementById('patientInfo').value,
                missedDoses: document.getElementById('missedDoses').value,
                medicalNotes: document.getElementById('medicalNotes').value,
                signature: document.getElementById('signature').value
            };
            localStorage.setItem('medicationData', JSON.stringify(data));
            alert('¡Datos guardados con éxito! ✅');
        }

        // Función para cargar datos guardados
        function loadData() {
            const saved = localStorage.getItem('medicationData');
            if (saved) {
                const data = JSON.parse(saved);
                document.getElementById('weekDate').value = data.weekDate || '';
                document.getElementById('caregiver').value = data.caregiver || '';
                document.getElementById('patientInfo').value = data.patientInfo || '';
                document.getElementById('missedDoses').value = data.missedDoses || '';
                document.getElementById('medicalNotes').value = data.medicalNotes || '';
                document.getElementById('signature').value = data.signature || '';
                alert('Datos cargados 📂');
            } else {
                alert('No hay datos guardados.');
            }
        }

        // Función para borrar todo
        function resetData() {
            if(confirm('¿Seguro que quieres borrar todo el contenido?')) {
                document.querySelectorAll('input, textarea').forEach(el => el.value = '');
                localStorage.removeItem('medicationData');
            }
        }

        // Cargar automáticamente al abrir
        window.onload = loadData;
    </script>
</body>
</html>
