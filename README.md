<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Planilla de Asistencia - Santa Catalina Labouré 😼</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --primary: #4f46e5;
      --primary-hover: #4338ca;
      --bg-color: #f3f4f6;
      --card-bg: #ffffff;
      --text-main: #1f2937;
      --border: #e5e7eb;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', system-ui, sans-serif; }
    body { background-color: var(--bg-color); color: var(--text-main); display: flex; justify-content: center; align-items: center; min-height: 100vh; padding: 20px; }
    .container { width: 100%; max-width: 480px; background: var(--card-bg); border-radius: 16px; box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05); padding: 28px 20px; border: 1px solid var(--border); }
    .header { text-align: center; margin-bottom: 20px; }
    .header h2 { font-size: 1.3rem; font-weight: 700; }
    .header p { font-size: 0.85rem; color: #6b7280; }
    .time-badge { display: flex; justify-content: space-between; background: #f9fafb; border: 1px solid var(--border); padding: 8px 14px; border-radius: 10px; font-size: 0.85rem; margin-bottom: 18px; }
    .nino-row { display: flex; justify-content: space-between; align-items: center; padding: 10px 12px; background: #f9fafb; border: 1px solid var(--border); border-radius: 10px; margin-bottom: 8px; }
    .nino-row span { font-size: 0.9rem; font-weight: 500; }
    .switch-asistencia { display: flex; gap: 6px; }
    .btn-estado { padding: 6px 12px; border-radius: 6px; border: 1px solid var(--border); background: white; cursor: pointer; font-weight: 600; font-size: 0.8rem; }
    .btn-estado.presente { background: #d1fae5; color: #065f46; border-color: #a7f3d0; }
    .btn-estado.ausente { background: #fee2e2; color: #991b1b; border-color: #fecaca; }
    button[type="submit"] { width: 100%; padding: 12px; background-color: var(--primary); color: white; border: none; border-radius: 10px; font-size: 0.95rem; font-weight: 600; cursor: pointer; margin-top: 16px; }
    button[type="submit"]:hover { background-color: var(--primary-hover); }
    .status-msg { margin-top: 16px; padding: 12px; border-radius: 10px; font-size: 0.85rem; text-align: center; font-weight: 500; display: none; white-space: pre-line; }
    .status-msg.success { display: block; background-color: #ecfdf5; color: #065f46; border: 1px solid #a7f3d0; }
    .status-msg.loading { display: block; background-color: #eff6ff; color: #1e40af; border: 1px solid #bfdbfe; }
    .status-msg.error { display: block; background-color: #fef2f2; color: #991b1b; border: 1px solid #fecaca; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h2>Planilla de Asistencia 😼</h2>
      <p>Parroquia Santa Catalina Labouré</p>
    </div>

    <div class="time-badge">
      <span>Fecha Clase: <strong id="lblFecha">--/--/----</strong></span>
    </div>

    <form id="formPlanilla">
      <div id="listaEstudiantes">
        <p style="text-align: center; color: #6b7280; font-size: 0.9rem;">Cargando lista de estudiantes desde Google Sheets...</p>
      </div>
      <button type="submit" id="btnGuardar" disabled>Guardar Asistencia del Día 😼</button>
    </form>

    <div id="mensaje" class="status-msg"></div>
  </div>

  <script>
    const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbxtTjNByorw8uznetHg8rJfHZFx_DGVWEtSrcjovlYoF-Vy8hgvYBEt_mdq_jv59YNi/exec";
    let estudiantesData = [];

    const hoy = new Date();
    const fechaFormateada = `${String(hoy.getDate()).padStart(2, '0')}/${String(hoy.getMonth() + 1).padStart(2, '0')}/${hoy.getFullYear()}`;
    document.getElementById('lblFecha').textContent = fechaFormateada;

    async function cargarEstudiantes() {
      try {
        const response = await fetch(SCRIPT_URL);
        const res = await response.json();
        if (res.status === "success" && res.estudiantes.length > 0) {
          estudiantesData = res.estudiantes;
          renderizarLista();
          document.getElementById('btnGuardar').disabled = false;
        } else {
          document.getElementById('listaEstudiantes').innerHTML = `<p style="text-align:center; color:red;">No se encontraron estudiantes en la hoja.</p>`;
        }
      } catch (e) {
        document.getElementById('listaEstudiantes').innerHTML = `<p style="text-align:center; color:red;">Error al cargar la lista.</p>`;
      }
    }

    function renderizarLista() {
      const container = document.getElementById('listaEstudiantes');
      container.innerHTML = "";
      estudiantesData.forEach(est => {
        const div = document.createElement('div');
        div.className = 'nino-row';
        div.innerHTML = `
          <span>${est.nombre}</span>
          <div class="switch-asistencia">
            <button type="button" class="btn-estado presente" data-id="${est.id}" data-estado="P" onclick="toggleEstado(this)">P</button>
            <button type="button" class="btn-estado" data-id="${est.id}" data-estado="A" onclick="toggleEstado(this)">A</button>
          </div>
        `;
        container.appendChild(div);
      });
    }

    function toggleEstado(btn) {
      const parent = btn.parentElement;
      parent.querySelectorAll('.btn-estado').forEach(b => {
        b.classList.remove('presente', 'ausente');
      });
      if (btn.dataset.estado === 'P') {
        btn.classList.add('presente');
      } else {
        btn.classList.add('ausente');
      }
    }

    document.getElementById('formPlanilla').addEventListener('submit', async (e) => {
      e.preventDefault();
      const mensaje = document.getElementById('mensaje');
      mensaje.className = "status-msg loading";
      mensaje.innerText = "Guardando columna del día en Google Sheets...";

      const asistencia = [];
      document.querySelectorAll('.nino-row').forEach(row => {
        const btnActivo = row.querySelector('.btn-estado.presente, .btn-estado.ausente');
        if (btnActivo) {
          asistencia.push({
            id: btnActivo.dataset.id,
            estado: btnActivo.dataset.estado
          });
        }
      });

      try {
        await fetch(SCRIPT_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'text/plain;charset=utf-8' },
          body: JSON.stringify({ fecha: fechaFormateada, asistencia: asistencia })
        });

        mensaje.className = "status-msg success";
        mensaje.innerText = `¡PLANILLA ACTUALIZADA! 😼\n\nFecha: ${fechaFormateada}\nRegistros guardados correctamente.`;
      } catch (err) {
        mensaje.className = "status-msg error";
        mensaje.innerText = "Error al guardar en el servidor.";
      }
    });

    cargarEstudiantes();
  </script>
</body>
</html>
