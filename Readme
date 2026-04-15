<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Planner Médico 💊</title>

    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/822/822143.png">
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Merriweather:ital,wght@1,700&display=swap" rel="stylesheet">

    <style>
        :root {
            --teal: #3a9ea5;
            --teal-light: #ecf4f5;
            --gray-800: #1e293b;
            --gray-200: #e2e8f0;
            --white: #ffffff;
        }

        /* OCULTAR CABECERA DE GITHUB */
        header, #banner, .gh-header { display: none !important; }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8;
            padding: 0;
            margin: 0;
        }

        .page {
            max-width: 600px;
            margin: 0 auto;
            background: var(--white);
            min-height: 100vh;
        }

        /* TOOLBAR */
        .toolbar {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 5px;
            padding: 10px;
            background: #f8fafc;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .toolbar button {
            padding: 8px 2px;
            border-radius: 8px;
            border: 1px solid var(--gray-200);
            font-size: 11px;
            font-weight: 600;
            background: white;
        }

        /* HEADER */
        .page-header {
            background: linear-gradient(135deg, var(--teal), #5ba9b0);
            color: white;
            padding: 30px 20px;
        }

        h1 { font-size: 22px; font-weight: 800; }
        .input-row { display: flex; gap: 10px; margin-top: 15px; }
        .input-group { flex: 1; }
        .input-group label { font-size: 9px; text-transform: uppercase; font-weight: 800; display: block; margin-bottom: 3px; }
        .input-group input {
            width: 100%;
            background: rgba(255,255,255,0.2);
            border: 1px solid rgba(255,255,255,0.3);
            border-radius: 6px;
            padding: 6px;
            color: white;
            font-size: 13px;
        }

        /* TABLA DE MEDICACIÓN */
        .table-container { overflow-x: auto; padding: 15px 5px; }
        table { width: 100%; border-collapse: collapse; min-width: 500px; }
        th { font-size: 10px; text-transform: uppercase; padding: 8px; background: var(--teal-light); color: var(--teal); border: 1px solid #d1e5e7; }
        td { border: 1px solid var(--gray-200); padding: 5px; }
        
        .med-input { width: 100%; border: none; font-size: 12px; font-weight: 600; outline: none; }
        .dose-input { width: 100%; border: none; font-size: 10px; color: #64748b; outline: none; }

        /* CIRCULOS DE CHECK */
        .check-cell {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            border: 2px solid var(--gray-200);
            margin: auto;
            cursor: pointer;
            transition: 0.2s;
        }
        .check-cell.taken { background-color: #dcfce7; border-color: #22c55e; position: relative; }
        .check-cell.taken::after { content: '✓'; color: #166534; position: absolute; left: 6px; top: 2px; font-weight: bold; }
        
        .check-cell.skipped { background-color: #fee2e2; border-color: #ef4444; position: relative; }
        .check-cell.skipped::after { content: '✕'; color: #991b1b; position: absolute; left: 7px; top: 2px; font-weight: bold; }

        /* SECCIONES INFERIORES */
        .section { padding: 15px 20px; border-top: 8px solid #f0f4f8; }
        .section-title { font-size: 11px; font-weight: 800; color: var(--teal); text-transform: uppercase; margin-bottom: 8px; }
        textarea { width: 100%; border: 1px solid var(--gray-200); border-radius: 8px; padding: 10px; font-size: 13px; font-family: inherit; }

        /* FOOTER FIRMA */
        .page-footer { padding: 30px 20px; background: #f8fafc; text-align: center; }
        .footer-sig { border-top: 1px dashed var(--gray-200); pt: 20px; display: flex; flex-direction: column; align-items: center; gap: 10px; }
        .footer-sig label { font-size: 10px; font-weight: 800; color: var(--teal); text-transform: uppercase; }
        .footer-sig input { 
            font-family: 'Merriweather', serif; font-style: italic; font-size: 18px; 
            border: none; border-bottom: 2px solid var(--teal); background: transparent; 
            width: 80%; text-align: center; outline: none; padding: 5px;
        }

        @media print { .toolbar { display: none; } }
    </style>
</head>
<body>

<div class="page">
    <div class="toolbar">
        <button onclick="window.print()">🖨️ Imprimir</button>
        <button onclick="saveData()" style="background: #e0f2fe;">💾 Guardar</button>
        <button onclick="loadData()">📂 Cargar</button>
        <button onclick="resetData()" style="color: red;">↺ Borrar</button>
    </div>

    <div class="page-header">
        <h1>Planner de Medicación</h1>
        <div class="input-row">
            <div class="input-group">
                <label>Semana del</label>
                <input type="text" id="weekDate" placeholder="7 - 13 Abr">
            </div>
            <div class="input-group">
                <label>Responsable</label>
                <input type="text" id="caregiver" placeholder="Nombre">
            </div>
        </div>
    </div>

    <div class="table-container">
        <table>
            <thead>
                <tr>
                    <th>Medicación / Dosis</th>
                    <th>L</th><th>M</th><th>M</th><th>J</th><th>V</th><th>S</th><th>D</th>
                </tr>
            </thead>
            <tbody id="med-body">
                </tbody>
        </table>
    </div>

    <div class="section">
        <div class="section-title">👤 Información del Paciente</div>
        <textarea id="patientInfo" rows="2"></textarea>
    </div>

    <div class="section" style="background: #fff5f5;">
        <div class="section-title" style="color: #c53030;">⚠️ Dosis Olvidadas / Motivo</div>
        <textarea id="missedDoses" rows="2"></textarea>
    </div>

    <div class="page-footer">
        <div class="footer-sig">
            <label>Firma del Cuidador / Paciente</label>
            <input type="text" id="signature" placeholder="Firme aquí...">
        </div>
    </div>
</div>

<script>
    // Generar las filas de la tabla
    const body = document.getElementById('med-body');
    for (let i = 0; i < 6; i++) {
        let tr = document.createElement('tr');
        tr.innerHTML = `
            <td>
                <input type="text" class="med-input" placeholder="Medicina ${i+1}">
                <input type="text" class="dose-input" placeholder="Dosis/Hora">
            </td>
            ${Array(7).fill().map(() => `<td><div class="check-cell" onclick="toggleCheck(this)"></div></td>`).join('')}
        `;
        body.appendChild(tr);
    }

    function toggleCheck(el) {
        if (!el.classList.contains('taken') && !el.classList.contains('skipped')) {
            el.classList.add('taken');
        } else if (el.classList.contains('taken')) {
            el.classList.replace('taken', 'skipped');
        } else {
            el.classList.remove('skipped');
        }
    }

    function saveData() {
        const rows = [];
        document.querySelectorAll('#med-body tr').forEach(tr => {
            const checks = Array.from(tr.querySelectorAll('.check-cell')).map(c => 
                c.classList.contains('taken') ? 'T' : c.classList.contains('skipped') ? 'S' : 'E'
            );
            rows.push({
                name: tr.querySelector('.med-input').value,
                dose: tr.querySelector('.dose-input').value,
                checks: checks
            });
        });

        const data = {
            weekDate: document.getElementById('weekDate').value,
            caregiver: document.getElementById('caregiver').value,
            patientInfo: document.getElementById('patientInfo').value,
            missedDoses: document.getElementById('missedDoses').value,
            signature: document.getElementById('signature').value,
            meds: rows
        };
        localStorage.setItem('medDataFull', JSON.stringify(data));
        alert('Guardado en el móvil ✅');
    }

    function loadData() {
        const saved = localStorage.getItem('medDataFull');
        if (!saved) return;
        const data = JSON.parse(saved);
        
        document.getElementById('weekDate').value = data.weekDate || '';
        document.getElementById('caregiver').value = data.caregiver || '';
        document.getElementById('patientInfo').value = data.patientInfo || '';
        document.getElementById('missedDoses').value = data.missedDoses || '';
        document.getElementById('signature').value = data.signature || '';

        const trs = document.querySelectorAll('#med-body tr');
        data.meds.forEach((m, i) => {
            if (trs[i]) {
                trs[i].querySelector('.med-input').value = m.name;
                trs[i].querySelector('.dose-input').value = m.dose;
                const cells = trs[i].querySelectorAll('.check-cell');
                m.checks.forEach((status, j) => {
                    if (status === 'T') cells[j].classList.add('taken');
                    if (status === 'S') cells[j].classList.add('skipped');
                });
            }
        });
    }

    function resetData() {
        if(confirm('¿Borrar todo?')) {
            localStorage.removeItem('medDataFull');
            location.reload();
        }
    }

    window.onload = loadData;
</script>

</body>
</html>
