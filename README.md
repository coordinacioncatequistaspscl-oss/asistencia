<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sistema Parroquial - Santa Catalina Labouré 😼</title>
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
      --success-bg: #d1fae5;
      --success-text: #065f46;
      --danger-bg: #fee2e2;
      --danger-text: #991b1b;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', system-ui, sans-serif; }
    body { background-color: var(--bg-color); color: var(--text-main); display: flex; justify-content: center; align-items: flex-start; min-height: 100vh; padding: 20px; }
    
    .container { width: 100%; max-width: 540px; background: var(--card-bg); border-radius: 16px; box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05); padding: 24px; border: 1px solid var(--border); }
    .header { text-align: center; margin-bottom: 20px; }
    .header h2 { font-size: 1.4rem; font-weight: 700; }
    .header p { font-size: 0.85rem; color: var(--text-muted); }

    /* Navegación por Pestañas (Tabs) */
    .nav-tabs { display: flex; border-bottom: 2px solid var(--border); margin-bottom: 20px; gap: 8px; }
    .tab-btn { flex: 1; padding: 10px; background: none; border: none; border-bottom: 3px solid transparent; font-weight: 600; font-size: 0.85rem; color: var(--text-muted); cursor: pointer; transition: all 0.2s ease; }
    .tab-btn.active { color: var(--primary); border-bottom-color: var(--primary); }

    .tab-content { display: none; }
    .tab-content.active { display: block; }

    /* UI Comun */
    .time-badge { display: flex; justify-content: space-between; align-items: center; background: #f9fafb; border: 1px solid var(--border); padding: 10px 14px; border-radius: 10px; font-size: 0.85rem; margin-bottom: 18px; }
    .counter-badge { background: var(--primary); color: white; padding: 3px 10px; border-radius: 20px; font-weight: 700; font-size: 0.8rem; }
    
    /* Formularios e Inputs */
    .form-group { margin-bottom: 14px; }
    .form-group label { display: block; font-size: 0.85rem; font-weight: 600; margin-bottom: 6px; }
    .form-input, .form-select { width: 100%; padding: 10px 12px; border: 1px solid var(--border); border-radius: 8px; font-size: 0.9rem; outline: none; background: #fff; }
    .form-input:focus, .form-select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1); }
    
    .btn-action { width: 100%; padding: 12px; background-color: var(--primary); color: white; border: none; border-radius: 10px; font-size: 0.95rem; font-weight: 600; cursor: pointer; margin-top: 10px; transition: background 0.2s; }
    .btn-action:hover { background-color: var(--primary-hover); }
    .btn-action:disabled { background-color: #a5b4fc; cursor: not-allowed; }

    .btn-secondary { background-color: #4b5563; }
    .btn-secondary:hover { background-color: #374151; }

    /* Apartado 1: Asistencia */
    .nino-row { display: flex; justify-content: space-between; align-items: center; padding: 10px 12px; background: #f9fafb; border: 1px solid var(--border); border-radius: 10px; margin-bottom: 8px; }
    .nino-row span { font-size: 0.9rem; font-weight: 500; }
    .switch-asistencia { display: flex; gap: 6px; }
    .btn-estado { padding: 6px 12px; border-radius: 6px; border: 1px solid var(--border); background: white; cursor: pointer; font-weight: 600; font-size: 0.8rem; }
    .btn-estado.presente { background: var(--success-bg); color: var(--success-text); border-color: #a7f3d0; }
    .btn-estado.ausente { background: var(--danger-bg); color: var(--danger-text); border-color: #fecaca; }

    /* Apartado 2: Catequistas */
    .stats-card { background: #eef2ff; border: 1px solid #c7d2fe; padding: 14px; border-radius: 12px; text-align: center; margin-bottom: 18px; }
    .stats-card strong { font-size: 1.6rem; color: var(--primary); display: block; }
    .stats-card span { font-size: 0.85rem; color: #4338ca; font-weight: 500; }
    
    .catequista-item { display: flex; justify-content: space-between; align-items: center; padding: 10px 12px; background: white; border: 1px solid var(--border); border-radius: 8px; margin-bottom: 8px; }
    .catequista-info h4 { font-size: 0.9rem; font-weight: 600; }
    .catequista-info p { font-size: 0.78rem; color: var(--text-muted); margin-top: 2px; }
    .catequista-info code { font-size: 0.75rem; background: #f3f4f6; padding: 2px 6px; border-radius: 4px; color: #4b5563; margin-right: 4px; }
    
    .actions-group { display: flex; gap: 6px; }
    .btn-icon { background: none; border: none; cursor: pointer; font-size: 1rem; padding: 4px 8px; border-radius: 4px; }
    .btn-edit:hover { background: #eef2ff; }
    .btn-delete:hover { background: #fee2e2; }

    /* Sub-pestañas de Catequistas */
    .sub-nav { display: flex; gap: 10px; margin-bottom: 16px; }
    .sub-btn { flex: 1; padding: 8px; background: #f3f4f6; border: 1px solid var(--border); border-radius: 8px; font-weight: 600; font-size: 0.8rem; cursor: pointer; color: var(--text-muted); }
    .sub-btn.active { background: var(--primary); color: white; border-color: var(--primary); }

    /* Mensajes */
    .status-msg { margin-top: 16px; padding: 12px; border-radius: 10px; font-size: 0.85rem; text-align: center; font-weight: 500; display: none; white-space: pre-line; }
    .status-msg.success { display: block; background-color: #ecfdf5; color: var(--success-text); border: 1px solid #a7f3d0; }
    .status-msg.loading { display: block; background-color: #eff6ff; color: #1e40af; border: 1px solid #bfdbfe; }
    .status-msg.error { display: block; background-color: #fef2f2; color: var(--danger-text); border: 1px solid #fecaca; }
  </style>
</head>
<body>

  <div class="container">
    <div class="header">
      <h2>Parroquia Santa Catalina Labouré 😼</h2>
      <p>Sistema de Gestión Parroquial</p>
    </div>

    <!-- Navegación por pestañas principales -->
    <div class="nav-tabs">
      <button class="tab-btn active" onclick="cambiarTab('tabAsistencia')">📋 Asistencia</button>
      <button class="tab-btn" onclick="cambiarTab('tabCatequistas')">👤 Catequistas</button>
    </div>

    <!-- APARTADO 1: PLANILLA DE ASISTENCIA -->
    <div id="tabAsistencia" class="tab-content active">
      <div class="time-badge">
        <span>Fecha Clase: <strong id="lblFecha">--/--/----</strong></span>
        <span class="counter-badge" id="badgeTotalEstudiantes">0 alumnos</span>
      </div>

      <form id="formPlanilla">
        <div id="listaEstudiantes">
          <p style="text-align: center; color: var(--text-muted); font-size: 0.9rem;">Cargando lista de estudiantes...</p>
        </div>
        <button type="submit" id="btnGuardar" class="btn-action" disabled>Guardar Asistencia del Día 😼</button>
      </form>
    </div>

    <!-- APARTADO 2: CATEQUISTAS -->
    <div id="tabCatequistas" class="tab-content">
      <!-- Estadísticas -->
      <div class="stats-card">
        <strong id="numCatequistas">0</strong>
        <span>Catequistas Registrados en la Parroquia</span>
      </div>

      <!-- Selector de Sub-acción (Registrar / Modificar) -->
      <div class="sub-nav">
        <button id="subBtnReg" class="sub-btn active" onclick="cambiarSubModo('registrar')">➕ Nuevo Registro</button>
        <button id="subBtnMod" class="sub-btn" onclick="cambiarSubModo('modificar')">✏️ Modificar Datos</button>
      </div>

      <!-- FORMULARIO 1: NUEVO CATEQUISTA -->
      <form id="formCatequistaRegistro">
        <div class="form-group">
          <label for="catNombre">Nombre Completo</label>
          <input type="text" id="catNombre" class="form-input" placeholder="Ej: Ana María Gómez" required>
        </div>
        <div class="form-group">
          <label for="catEquipo">Equipo de Catequesis</label>
          <select id="catEquipo" class="form-select" required>
            <option value="" disabled selected>Seleccione el equipo...</option>
            <option value="Infancia / Bautismo">Infancia / Bautismo</option>
            <option value="Primera Comunión">Primera Comunión</option>
            <option value="Confirmación">Confirmación</option>
            <option value="Juvenil / Post-Confirmación">Juvenil / Post-Confirmación</option>
            <option value="Coordinación General">Coordinación General</option>
          </select>
        </div>
        <div class="form-group">
          <label for="catCodigo">Código Único <span style="font-weight:400; color:var(--text-muted);">(Opcional, se autogenera)</span></label>
          <input type="text" id="catCodigo" class="form-input" placeholder="Ej: CAT-001">
        </div>
        <div class="form-group">
          <label for="catPass">Contraseña de Seguridad <span style="font-weight:400; color:var(--text-muted);">(Para futuras modificaciones)</span></label>
          <input type="password" id="catPass" class="form-input" placeholder="Crea una contraseña segura" required>
        </div>
        <button type="submit" class="btn-action">Guardar Registro en Google Sheets 🚀</button>
      </form>

      <!-- FORMULARIO 2: MODIFICAR CATEQUISTA (REQUIERE CONTRASEÑA) -->
      <form id="formCatequistaModificar" style="display: none;">
        <div class="form-group">
          <label for="modSelect">Seleccione su Usuario</label>
          <select id="modSelect" class="form-input" onchange="cargarDatosModificacion()" required>
            <option value="" disabled selected>Cargando catequistas...</option>
          </select>
        </div>
        <div class="form-group">
          <label for="modNombre">Nombre Completo</label>
          <input type="text" id="modNombre" class="form-input" required>
        </div>
        <div class="form-group">
          <label for="modEquipo">Equipo de Catequesis</label>
          <select id="modEquipo" class="form-select" required>
            <option value="Infancia / Bautismo">Infancia / Bautismo</option>
            <option value="Primera Comunión">Primera Comunión</option>
            <option value="Confirmación">Confirmación</option>
            <option value="Juvenil / Post-Confirmación">Juvenil / Post-Confirmación</option>
            <option value="Coordinación General">Coordinación General</option>
          </select>
        </div>
        <div class="form-group">
          <label for="modPassActual">Contraseña Actual <span style="color: red;">*</span></label>
          <input type="password" id="modPassActual" class="form-input" placeholder="Ingrese su contraseña para autorizar cambios" required>
        </div>
        <button type="submit" class="btn-action btn-secondary">Actualizar Mis Datos 🔒</button>
      </form>

      <hr style="border: 0; border-top: 1px solid var(--border); margin: 20px 0;">

      <!-- Buscador y Listado Informativo -->
      <div class="form-group">
        <input type="text" id="searchCatequista" class="form-input" placeholder="🔍 Buscar catequista por nombre o equipo..." oninput="filtrarCatequistas()">
      </div>

      <div id="listaCatequistas">
        <p style="text-align: center; color: var(--text-muted); font-size: 0.9rem;">Cargando listado...</p>
      </div>
    </div>

    <div id="mensaje" class="status-msg"></div>
  </div>

  <script>
    const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbxtTjNByorw8uznetHg8rJfHZFx_DGVWEtSrcjovlYoF-Vy8hgvYBEt_mdq_jv59YNi/exec";
    
    let estudiantesData = [];
    let catequistasData = [];

    // Fecha actual para asistencia
    const hoy = new Date();
    const fechaFormateada = `${String(hoy.getDate()).padStart(2, '0')}/${String(hoy.getMonth() + 1).padStart(2, '0')}/${hoy.getFullYear()}`;
    document.getElementById('lblFecha').textContent = fechaFormateada;

    // Control de Pestañas principales
    function cambiarTab(tabId) {
      document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
      document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
      
      document.getElementById(tabId).classList.add('active');
      event.target.classList.add('active');
      ocultarMensaje();
    }

    // Control de Sub-modos en Catequistas (Registrar vs Modificar)
    function cambiarSubModo(modo) {
      ocultarMensaje();
      if (modo === 'registrar') {
        document.getElementById('formCatequistaRegistro').style.display = 'block';
        document.getElementById('formCatequistaModificar').style.display = 'none';
        document.getElementById('subBtnReg').classList.add('active');
        document.getElementById('subBtnMod').classList.remove('active');
      } else {
        document.getElementById('formCatequistaRegistro').style.display = 'none';
        document.getElementById('formCatequistaModificar').style.display = 'block';
        document.getElementById('subBtnMod').classList.add('active');
        document.getElementById('subBtnReg').classList.remove('active');
        poblarSelectModificacion();
      }
    }

    // --- APARTADO 1: ASISTENCIA ---
    async function cargarEstudiantes() {
      try {
        const response = await fetch(SCRIPT_URL);
        const res = await response.json();
        if (res.status === "success" && res.estudiantes && res.estudiantes.length > 0) {
          estudiantesData = res.estudiantes;
          renderizarListaEstudiantes();
          document.getElementById('btnGuardar').disabled = false;
        } else {
          document.getElementById('listaEstudiantes').innerHTML = `<p style="text-align:center; color:red;">No se encontraron estudiantes registrados.</p>`;
        }
      } catch (e) {
        document.getElementById('listaEstudiantes').innerHTML = `<p style="text-align:center; color:red;">Error al conectar con la base de datos.</p>`;
      }
    }

    function renderizarListaEstudiantes() {
      const container = document.getElementById('listaEstudiantes');
      container.innerHTML = "";
      document.getElementById('badgeTotalEstudiantes').textContent = `${estudiantesData.length} alumnos`;

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
      parent.querySelectorAll('.btn-estado').forEach(b => b.classList.remove('presente', 'ausente'));
      if (btn.dataset.estado === 'P') {
        btn.classList.add('presente');
      } else {
        btn.classList.add('ausente');
      }
    }

    document.getElementById('formPlanilla').addEventListener('submit', async (e) => {
      e.preventDefault();
      mostrarMensaje("loading", "Guardando columna del día en Google Sheets...");

      const asistencia = [];
      document.querySelectorAll('.nino-row').forEach(row => {
        const btnActivo = row.querySelector('.btn-estado.presente, .btn-estado.ausente');
        if (btnActivo) {
          asistencia.push({ id: btnActivo.dataset.id, estado: btnActivo.dataset.estado });
        }
      });

      try {
        await fetch(SCRIPT_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'text/plain;charset=utf-8' },
          body: JSON.stringify({ action: 'asistencia', fecha: fechaFormateada, asistencia: asistencia })
        });
        mostrarMensaje("success", `¡PLANILLA ACTUALIZADA! 😼\nFecha: ${fechaFormateada}\nRegistros guardados correctamente.`);
      } catch (err) {
        mostrarMensaje("error", "Error al guardar la asistencia.");
      }
    });

    // --- APARTADO 2: CATEQUISTAS ---
    async function cargarCatequistas() {
      try {
        const response = await fetch(`${SCRIPT_URL}?type=catequistas`);
        const res = await response.json();
        if (res.status === "success" && res.catequistas) {
          catequistasData = res.catequistas;
        } else {
          catequistasData = JSON.parse(localStorage.getItem('catequistas_local') || '[]');
        }
      } catch (e) {
        catequistasData = JSON.parse(localStorage.getItem('catequistas_local') || '[]');
      }
      actualizarUICatequistas(catequistasData);
    }

    function actualizarUICatequistas(lista) {
      document.getElementById('numCatequistas').textContent = catequistasData.length;
      const container = document.getElementById('listaCatequistas');
      container.innerHTML = "";

      if (lista.length === 0) {
        container.innerHTML = `<p style="text-align:center; color:var(--text-muted); font-size:0.85rem;">No hay catequistas registrados todavía.</p>`;
        return;
      }

      lista.forEach(cat => {
        const div = document.createElement('div');
        div.className = 'catequista-item';
        div.innerHTML = `
          <div class="catequista-info">
            <h4>${cat.nombre}</h4>
            <p><strong>Equipo:</strong> ${cat.equipo || 'Sin asignar'}</p>
            <code style="margin-top:4px; display:inline-block;">ID: ${cat.codigo}</code>
          </div>
          <div class="actions-group">
            <button class="btn-icon btn-delete" title="Eliminar" onclick="eliminarCatequista('${cat.codigo}')">🗑️</button>
          </div>
        `;
        container.appendChild(div);
      });
    }

    // Registrar Nuevo Catequista
    document.getElementById('formCatequistaRegistro').addEventListener('submit', async (e) => {
      e.preventDefault();
      const nombre = document.getElementById('catNombre').value.trim();
      const equipo = document.getElementById('catEquipo').value;
      let codigo = document.getElementById('catCodigo').value.trim();
      const password = document.getElementById('catPass').value;

      if (!codigo) {
        codigo = `CAT-${Math.floor(1000 + Math.random() * 9000)}`;
      }

      if (catequistasData.some(c => c.codigo.toLowerCase() === codigo.toLowerCase())) {
        mostrarMensaje("error", "El código ingresado ya existe. Por favor use otro.");
        return;
      }

      const nuevoCatequista = { nombre, equipo, codigo, password };
      catequistasData.push(nuevoCatequista);
      localStorage.setItem('catequistas_local', JSON.stringify(catequistasData));

      try {
        fetch(SCRIPT_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'text/plain;charset=utf-8' },
          body: JSON.stringify({ action: 'nuevo_catequista', catequista: nuevoCatequista })
        });
      } catch (err) {}

      document.getElementById('formCatequistaRegistro').reset();
      actualizarUICatequistas(catequistasData);
      mostrarMensaje("success", `¡Registro exitoso en Santa Catalina Labouré! 😼\nCódigo asignado: ${codigo}`);
    });

    // Llenar select para modificar
    function poblarSelectModificacion() {
      const select = document.getElementById('modSelect');
      select.innerHTML = '<option value="" disabled selected>Seleccione su usuario...</option>';
      catequistasData.forEach(c => {
        const opt = document.createElement('option');
        opt.value = c.codigo;
        opt.textContent = `${c.nombre} (${c.equipo || 'Sin equipo'})`;
        select.appendChild(opt);
      });
    }

    function cargarDatosModificacion() {
      const codigo = document.getElementById('modSelect').value;
      const cat = catequistasData.find(c => c.codigo === codigo);
      if (cat) {
        document.getElementById('modNombre').value = cat.nombre;
        document.getElementById('modEquipo').value = cat.equipo || '';
        document.getElementById('modPassActual').value = '';
      }
    }

    // Modificar datos con validación de contraseña
    document.getElementById('formCatequistaModificar').addEventListener('submit', async (e) => {
      e.preventDefault();
      const codigo = document.getElementById('modSelect').value;
      const passIngresada = document.getElementById('modPassActual').value;
      
      const catIndex = catequistasData.findIndex(c => c.codigo === codigo);
      if (catIndex === -1) {
        mostrarMensaje("error", "Seleccione un catequista válido.");
        return;
      }

      // Validar contraseña
      if (catequistasData[catIndex].password && catequistasData[catIndex].password !== passIngresada) {
        mostrarMensaje("error", "Contraseña incorrecta. No se pueden aplicar los cambios.");
        return;
      }

      // Aplicar cambios
      catequistasData[catIndex].nombre = document.getElementById('modNombre').value.trim();
      catequistasData[catIndex].equipo = document.getElementById('modEquipo').value;

      localStorage.setItem('catequistas_local', JSON.stringify(catequistasData));

      try {
        fetch(SCRIPT_URL, {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'text/plain;charset=utf-8' },
          body: JSON.stringify({ action: 'modificar_catequista', catequista: catequistasData[catIndex] })
        });
      } catch (err) {}

      actualizarUICatequistas(catequistasData);
      mostrarMensaje("success", "¡Datos modificados y actualizados correctamente! 😼");
      cambiarSubModo('registrar');
    });

    function filtrarCatequistas() {
      const q = document.getElementById('searchCatequista').value.toLowerCase();
      const filtrados = catequistasData.filter(c => 
        c.nombre.toLowerCase().includes(q) || (c.equipo && c.equipo.toLowerCase().includes(q))
      );
      actualizarUICatequistas(filtrados);
    }

    function eliminarCatequista(codigo) {
      if (confirm(`¿Seguro que deseas eliminar al catequista con código ${codigo}?`)) {
        catequistasData = catequistasData.filter(c => c.codigo !== codigo);
        localStorage.setItem('catequistas_local', JSON.stringify(catequistasData));
        actualizarUICatequistas(catequistasData);
        mostrarMensaje("success", "Catequista eliminado correctamente.");
      }
    }

    function mostrarMensaje(tipo, texto) {
      const msg = document.getElementById('mensaje');
      msg.className = `status-msg ${tipo}`;
      msg.innerText = texto;
    }

    function ocultarMensaje() {
      const msg = document.getElementById('mensaje');
      msg.className = "status-msg";
    }

    // Carga inicial
    cargarEstudiantes();
    cargarCatequistas();
  </script>
</body>
</html>
