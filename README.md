<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Asistencia Parroquia Santa Catalina Labouré 😼</title>
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
      --text-muted: #6b7280;
      --border: #e5e7eb;
      --focus-ring: rgba(79, 70, 229, 0.2);
    }
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', system-ui, sans-serif; }
    body { background-color: var(--bg-color); color: var(--text-main); display: flex; justify-content: center; align-items: center; min-height: 100vh; padding: 20px; }
    .container { width: 100%; max-width: 480px; background: var(--card-bg); border-radius: 16px; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05); padding: 28px 20px; border: 1px solid rgba(229, 231, 235, 0.8); }
    .tabs { display: flex; gap: 8px; margin-bottom: 20px; background: #eef2ff; padding: 4px; border-radius: 12px; }
    .tab-btn { flex: 1; padding: 10px; border: none; border-radius: 8px; font-size: 0.85rem; font-weight: 600; cursor: pointer; background: transparent; color: #4b5563; transition: all 0.2s; }
    .tab-btn.active { background: white; color: var(--primary); box-shadow: 0 2px 4px rgba(0,0,0,0.05); }
    .header { text-align: center; margin-bottom: 20px; }
    .header h2 { font-size: 1.3rem; font-weight: 700; }
    .header p { font-size: 0.85rem; color: var(--text-muted); }
    .time-badge { display: flex; justify-content: space-between; background: #f9fafb; border: 1px solid var(--border); padding: 8px 14px; border-radius: 10px; font-size: 0.85rem; margin-bottom: 18px; }
    .time-badge span strong { color: var(--primary); }
    .form-group { margin-bottom: 16px; }
    label { display: block; font-size: 0.85rem; font-weight: 600; margin-bottom: 6px; color: #374151; }
    input, select { width: 100%; padding: 10px 12px; border: 1px solid var(--border); border-radius: 10px; font-size: 0.9rem; outline: none; }
    input:focus, select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px var(--focus-ring); }
    .hidden { display: none; }
    .nino-row { display: flex; gap: 8px; align-items: center; margin-bottom: 8px; background: #f9fafb; padding: 8px; border-radius: 8px; border: 1px solid #f3f4f6; }
    .nino-row input[type="text"] { flex: 1; }
    .nino-row label { margin-bottom: 0; font-size: 0.8rem; display: flex; align-items: center; gap: 4px; cursor: pointer; }
    .btn-add { background: #e0e7ff; color: var(--primary); border: none; padding: 8px; border-radius: 8px; font-weight: 600; width: 100%; cursor: pointer; font-size: 0.85rem; margin-bottom: 16px; }
    button[type="submit"] { width: 100%; padding: 12px; background-color: var(--primary); color: white; border: none; border-radius: 10px; font-size: 0.95rem; font-weight: 600; cursor: pointer; margin-top: 8px; }
    button[type="submit"]:hover { background-color: var(--primary-hover); }
    .status-msg { margin-top: 16px; padding: 12px; border-radius: 10px; font-size: 0.85rem; text-align: center; font-weight: 500; display: none; white-space: pre-line; }
    .status-msg.success { display: block; background-color: #ecfdf5; color: #065f46; border: 1px solid #a7f3d0; }
    .status-msg.loading { display: block; background-color: #eff6ff; color: #1e40af; border: 1px solid #bfdbfe; }
    .status-msg.error { display: block; background-color: #fef2f2; color: #991b1b; border: 1px solid #fecaca; }
  </style>
</head>
<body>
  <div class="container">
    <div class="tabs">
      <button class="tab-btn active" onclick="switchTab('general')">Llegada General</button>
      <button class="tab-btn" onclick="switchTab('catequista')">Modo Catequista 📋</button>
    </div>
    <div class="header">
      <h2 id="tituloSeccion">Control de Asistencia 😼</h2>
      <p>Parroquia Santa Catalina Labouré</p>
    </div>
    <div class="time-badge">
      <span>Día: <strong id="lblFecha">--/--/----</strong></span>
      <span>Hora: <strong id="lblHora">--:--:--</strong></span>
    </div>
    <form id="formGeneral">
      <div class="form-group">
        <label for="nombre">Nombre(s)</label>
        <input type="text" id="nombre" placeholder="ej. nombre" required>
      </div>
      <div class="form-group">
        <label for="apellido">Apellido(s)</label>
        <input type="text" id="apellido" placeholder="ej. APELLIDO" required>
      </div>
      <div class="form-group">
        <label for="opcionSacramento">Sacramento / Motivo</label>
        <select id="opcionSacramento" required>
          <option value="" disabled selected>selecciona una opción...</option>
          <option value="Primera comunión">Primera comunión</option>
          <option value="Confirmación">Confirmación</option>
          <option value="Otro">Otro</option>
        </select>
      </div>
      <div class="form-group hidden" id="campoOtroGrupo">
        <label for="otroTexto">¿Qué estás haciendo?</label>
        <input type="text" id="otroTexto" placeholder="ej. catequista, acompañante, etc.">
      </div>
      <button type="submit">Registrar Llegada 😼</button>
    </form>
    <form id="formCatequista" class="hidden">
      <div class="form-group">
        <label for="nombreCatequista">Tu Nombre (Catequista)</label>
        <input type="text" id="nombreCatequista" placeholder="ej. nombre del catequista" required>
      </div>
      <div class="form-group">
        <label for="nombreGrupo">Nombre de tu Grupo</label>
        <input type="text" id="nombreGrupo" placeholder="ej. Grupo San José / Confirmación A" required>
      </div>
      <label>Lista de Niños / Asistentes</label>
      <div id="listaNinos">
        <div class="nino-row">
          <input type="text" placeholder="Nombre completo del niño" class="input-nino" required>
          <label><input type="checkbox" class="check-asistencia" checked> Presente</label>
        </div>
      </div>
      <button type="button" class="btn-add" onclick="agregarFilaNino()">+ Agregar otro niño</button>
      <button type="submit">Guardar Asistencia de Grupo 😼</button>
    </form>
    <div id="mensaje" class="status-msg"></div>
  </div>
  <script>
    const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbzzszA6NNGfCUV8FoF4jM3jcy8xgAFCTctoWblOguIcUvWMVYHEugKXYehC2QrQBEpD/exec";
    function actualizarReloj() {
      const ahora = new Date();
      document.getElementById('lblFecha').textContent = ahora.toLocaleDateString();
      document.getElementById('lblHora').textContent = ahora.toLocaleTimeString();
    }
    actualizarReloj();
    setInterval(actualizarReloj, 1000);
    function switchTab(tab) {
      document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
      document.getElementById('mensaje').className = "status-msg";
      if(tab === 'general') {
        document.querySelectorAll('.tab-btn')[0].classList.add('active');
        document.getElementById('formGeneral').classList.remove('hidden');
        document.getElementById('formCatequista').classList.add('hidden');
        document.getElementById('tituloSeccion').innerText = "Control de Asistencia 😼";
      } else {
        document.querySelectorAll('.tab-btn')[1].classList.add('active');
        document.getElementById('formGeneral').classList.add('hidden');
        document.getElementById('formCatequista').classList.remove('hidden');
        document.getElementById('tituloSeccion').innerText = "Registro de Grupo 📋";
      }
    }
    const selectSacramento = document.getElementById('opcionSacramento');
    const campoOtroGrupo = document.getElementById('campoOtroGrupo');
    const otroTextoInput = document.getElementById('otroTexto');
    selectSacramento.addEventListener('change', () => {
      if (selectSacramento.value === 'Otro') {
        campoOtroGrupo.classList.remove('hidden');
        otroTextoInput.required = true;
      } else {
        campoOtroGrupo.classList.add('hidden');
        otroTextoInput.required = false;
        otroTextoInput.value = '';
      }
    });
    function agregarFilaNino() {
      const container = document.getElementById('listaNinos');
      const div = document.createElement('div');
      div.className = 'nino-row';
      div.innerHTML = `
        <input type="text" placeholder="Nombre completo del niño" class="input-nino" required>
        <label><input type="checkbox" class="check-asistencia" checked> Presente</label>
      `;
      container.appendChild(div);
    }
    document.getElementById('formGeneral').addEventListener('submit', async (e) => {
      e.preventDefault();
      enviarDatos({
        tipo: 'individual',
        nombreCompleto: `${document.getElementById('nombre').value.trim()} ${document.getElementById('apellido').value.trim()}`,
        motivo: selectSacramento.value === 'Otro' ? otroTextoInput.value.trim() : selectSacramento.value
      }, () => {
        document.getElementById('formGeneral').reset();
        campoOtroGrupo.classList.add('hidden');
      });
    });
    document.getElementById('formCatequista').addEventListener('submit', async (e) => {
      e.preventDefault();
      const ninosInputs = document.querySelectorAll('.nino-row');
      const lista = [];
      ninosInputs.forEach(row => {
        const nombre = row.querySelector('.input-nino').value.trim();
        const asistio = row.querySelector('.check-asistencia').checked;
        if(nombre) lista.push({ nombre, asistio });
      });
      enviarDatos({
        tipo: 'grupo',
        catequista: document.getElementById('nombreCatequista').value.trim(),
        nombreGrupo: document.getElementById('nombreGrupo').value.trim(),
        ninos: lista
      }, () => {
        document.getElementById('formCatequista').reset();
        document.getElementById('listaNinos').innerHTML = `
          <div class="nino-row">
            <input type="text" placeholder="Nombre completo del niño" class="input-nino" required>
            <label><input type="checkbox" class="check-asistencia" checked> Presente</label>
          </div>
        `;
      });
    });
    async function enviarDatos(payload, callbackExito) {
      const mensaje = document.getElementById('mensaje');
      mensaje.className = "status-msg loading";
      mensaje.innerText = "Guardando en Google Sheets...";
      try {
        await fetch(SCRIPT_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'text/plain;charset=utf-8' },
          body: JSON.stringify(payload)
        });
        mensaje.className = "status-msg success";
        if(payload.tipo === 'individual') {
          mensaje.innerText = `¡CONFIRMACIÓN EXITOSA! 😼\n\nRegistrado: ${payload.nombreCompleto}\nMotivo: ${payload.motivo}`;
        } else {
          mensaje.innerText = `¡CONFIRMACIÓN EXITOSA! 😼\n\nGrupo: ${payload.nombreGrupo}\nCatequista: ${payload.catequista}\nNiños registrados: ${payload.ninos.length}`;
        }
        callbackExito();
      } catch (err) {
        mensaje.className = "status-msg error";
        mensaje.innerText = "Error al conectar con el servidor.";
      } finally {
        btn.disabled = false;
      }
    }
  </script>
</body>
</html>
