<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// --- CONFIGURACIÓN ---
// Fecha del cumpleaños: 25 de Julio de 2026
const FECHA_CUMPLE = new Date('2026-07-25T19:00:00').getTime()
const CODIGO_SECRETO = 'JULIO25'

// --- ESTADO REACTIVO ---
const dias = ref('00')
const horas = ref('00')
const minutos = ref('00')
const segundos = ref('00')

const codigoIngresado = ref('')
const mostrarUbicacion = ref(false)
const errorCodigo = ref(false)

let timerId = null

// Función para asegurar que siempre haya 2 dígitos (ej. "05" en lugar de "5")
const formatearNumero = (num) => (num < 10 ? `0${num}` : num)

// Lógica de la cuenta regresiva
const calcularTiempo = () => {
  const ahora = new Date().getTime()
  const diferencia = FECHA_CUMPLE - ahora

  if (diferencia <= 0) {
    clearInterval(timerId)
    dias.value = horas.value = minutos.value = segundos.value = '00'
    return
  }

  dias.value = formatearNumero(Math.floor(diferencia / (1000 * 60 * 60 * 24)))
  horas.value = formatearNumero(Math.floor((diferencia % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)))
  minutos.value = formatearNumero(Math.floor((diferencia % (1000 * 60 * 60)) / (1000 * 60)))
  segundos.value = formatearNumero(Math.floor((diferencia % (1000 * 60)) / 1000))
}

const validarCodigo = () => {
  if (codigoIngresado.value.trim().toUpperCase() === CODIGO_SECRETO) {
    mostrarUbicacion.value = true
    errorCodigo.value = false
    codigoIngresado.value = '' // Limpiar input
  } else {
    mostrarUbicacion.value = false
    errorCodigo.value = true
  }
}

onMounted(() => {
  calcularTiempo()
  timerId = setInterval(calcularTiempo, 1000)
})

onUnmounted(() => {
  if (timerId) clearInterval(timerId)
})
</script>

<template>
  <div class="zigzag-background">
    <div class="app-container">
      
      <!-- TARJETA DE INVITACIÓN PRINCIPAL -->
      <main class="invitation-card">
        <div class="card-content">
          <h2>TE INVITO A:</h2>
          <h1>¡Celebrar el Cumpleaños de<br>Rodrigo El Rorek!</h1>
          <p class="date">Sábado 25 de Julio de 2026.</p>
          <p class="date">7:00 PM</p>
          <p>Prepárate para ponerte hasta su puta madre y jugar billar</p>
          <h2 class="highlight">¡NO FALTES!</h2>
        </div>
        
        <!-- [MODIFICADO] SECCIÓN DE LA IMAGEN RESPONSIVE -->
        <!-- He encapsulado la imagen en su propio contenedor para controlarla mejor -->
        <div class="clown-image-container">
          <!-- Asegúrate de que el archivo image_3.png esté en la carpeta /public de tu proyecto -->
          <img 
            src="/Payasito.png" 
            alt="Payaso real con pastel y globos" 
            class="clown-real-img"
          />
        </div>
      </main>

      <!-- CUENTA REGRESIVA (Sigue igual) -->
      <section class="timer-section">
        <p class="timer-title">¡FALTAN!</p>
        <div class="countdown">
          <div class="time-box">
            <span class="digits">{{ dias }}</span>
            <span class="label">Días</span>
          </div>
          <span class="colon">:</span>
          <div class="time-box">
            <span class="digits">{{ horas }}</span>
            <span class="label">Horas</span>
          </div>
          <span class="colon">:</span>
          <div class="time-box">
            <span class="digits">{{ minutos }}</span>
            <span class="label">Minutos</span>
          </div>
          <span class="colon">:</span>
          <div class="time-box">
            <span class="digits">{{ segundos }}</span>
            <span class="label">Segundos</span>
          </div>
        </div>
      </section>

      <section class="map-section">
        <div class="map-card">
          <h3>📍 Ubicación</h3>
          <p>Billar Plaza Kristal, Aguascalientes</p>
          <iframe 
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d511.52260345462!2d-102.28077709681021!3d21.878238172580655!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x8429ee0b51555555%3A0x171574150a034db4!2sCinemas%20Kristal!5e0!3m2!1ses-419!2smx!4v1784067234895!5m2!1ses-419!2smx" 
            width="100%" 
            height="300" 
            style="border:0; border-radius: 12px;" 
            allowfullscreen="" 
            loading="lazy">
          </iframe>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
/* Importamos una fuente divertida estilo cómic desde Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&display=swap');

/* FONDO ZIGZAG AMARILLO Y BLANCO (Sigue igual) */
.zigzag-background {
  min-height: 100vh;
  width: 100%;
  font-family: 'Comic Neue', cursive, sans-serif;
  background-color: #fce181;
  background-image: 
    linear-gradient(135deg, rgba(255,255,255,0.7) 25%, transparent 25%), 
    linear-gradient(225deg, rgba(255,255,255,0.7) 25%, transparent 25%), 
    linear-gradient(45deg, rgba(255,255,255,0.7) 25%, transparent 25%), 
    linear-gradient(315deg, rgba(255,255,255,0.7) 25%, #fce181 25%);
  background-position:  20px 0, 20px 0, 0 0, 0 0;
  background-size: 40px 40px;
  background-repeat: repeat;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 40px 20px;
  box-sizing: border-box;
}

.app-container {
  max-width: 600px;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

/* TARJETA PRINCIPAL (MODIFICADA) */
.invitation-card {
  background-color: #faf8f5;
  border: 5px solid #4a90e2;
  border-radius: 20px 30px 15px 25px; 
  padding: 40px 20px;
  text-align: center;
  position: relative; /* Importante para posicionar al payaso */
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  overflow: visible; /* Permitir que el payaso sobresalga si es necesario */
}

/* [AÑADIDO] Contenedor del texto para separarlo del payaso */
.card-content {
  position: relative;
  z-index: 2; /* Texto por encima */
  padding-bottom: 20px; /* Espacio para el payaso en mobile */
}

.invitation-card h1 {
  font-size: 2.2rem;
  color: #333;
  margin: 15px 0;
  line-height: 1.2;
}

.invitation-card h2 {
  font-size: 1.5rem;
  color: #333;
  margin: 0;
}

.invitation-card .date {
  font-size: 1.3rem;
  font-weight: bold;
  margin: 15px 0;
}

.invitation-card .highlight {
  font-size: 2rem;
  margin-top: 20px;
}

/* [AÑADIDO] ESTILOS DE LA IMAGEN REAL (DESKTOP FIRST) */
.clown-image-container {
  position: absolute; /* Posicionamiento absoluto respecto a .invitation-card */
  bottom: -40px; /* Sobresale un poco por abajo */
  right: -30px; /* Sobresale un poco por la derecha */
  width: 200px; /* Tamaño fijo en desktop */
  z-index: 1; /* Por debajo del texto pero por encima de la tarjeta */
  pointer-events: none; /* Que los clics pasen a través de la imagen */
}

.clown-real-img {
  width: 100%;
  height: auto;
  display: block;
}

/* CUENTA REGRESIVA, CÓDIGO Y MAPA (Siguen igual en desktop) */
.timer-section { text-align: center; }
.timer-title { font-weight: bold; background-color: rgba(255, 255, 255, 0.8); display: inline-block; padding: 5px 15px; border-radius: 20px; margin-bottom: 15px; }
.countdown { display: flex; justify-content: center; align-items: center; gap: 5px; }
.time-box { display: flex; flex-direction: column; align-items: center; }
.digits { background-color: #222; color: white; font-size: 2.5rem; font-weight: bold; padding: 10px 15px; border-radius: 8px; letter-spacing: 2px; box-shadow: inset 0 3px 5px rgba(0,0,0,0.5); }
.colon { font-size: 2.5rem; font-weight: bold; color: #222; margin-bottom: 25px; }
.label { font-size: 1rem; font-weight: bold; margin-top: 5px; background-color: rgba(255, 255, 255, 0.7); padding: 2px 8px; border-radius: 10px; }
.secret-section { background-color: rgba(255, 255, 255, 0.9); padding: 20px; border-radius: 15px; text-align: center; border: 3px solid #fce181; }
.secret-label { font-weight: bold; font-size: 1.2rem; margin-bottom: 15px; }
.form-group { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }
.input-code { font-family: inherit; font-size: 1.2rem; padding: 10px 15px; border: 3px solid #222; border-radius: 8px; width: 200px; text-align: center; background-color: #eef5ff; outline: none; }
.btn-reveal { font-family: inherit; font-size: 1.1rem; font-weight: bold; background-color: #f2c94c; border: 3px solid #222; padding: 10px 20px; border-radius: 8px; cursor: pointer; transition: transform 0.1s; }
.btn-reveal:active { transform: scale(0.95); }
.error { color: #d32f2f; font-weight: bold; margin-top: 10px; }
.map-card { background-color: white; padding: 20px; border-radius: 15px; border: 4px solid #4a90e2; text-align: center; box-shadow: 0 10px 20px rgba(0,0,0,0.2); animation: popIn 0.5s ease-out forwards; }
.map-card h3 { margin-top: 0; }
@keyframes popIn { 0% { transform: scale(0.8); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }

/* ========================================= */
/* [MODIFICADO] MEDIA QUERIES PARA MOBILE (< 600px) */
/* Esta sección es vital para la responsividad */
/* ========================================= */
@media (max-width: 600px) {
  /* Ajustar padding del contenedor principal */
  .zigzag-background {
    padding: 20px 10px;
  }

  /* Reducir tamaños de fuente en la tarjeta */
  .invitation-card {
    padding: 30px 15px;
  }
  
  .invitation-card h1 { font-size: 1.6rem; }
  .invitation-card h2 { font-size: 1.1rem; }
  .invitation-card .date { font-size: 1rem; }
  .invitation-card .highlight { font-size: 1.4rem; }

  /* [CLAVE] Ajustar la imagen del payaso para Mobile */
  .clown-image-container {
    position: relative; /* Cambiamos a relativo para que fluya con el texto */
    width: 120px; /* Payaso más pequeño en móvil */
    margin: 0 auto; /* Centrado */
    bottom: 0; /* Ya no sobresale */
    right: 0;
    margin-top: -30px; /* Lo subimos un poco para que pise el texto final */
    padding-top: 10px;
    clear: both; /* Asegurar que se ponga después del texto */
  }

  /* Ajustar la cuenta regresiva para móvil */
  .countdown {
    gap: 3px;
  }
  
  .digits {
    font-size: 1.8rem;
    padding: 8px 10px;
  }
  
  .colon {
    font-size: 1.8rem;
    margin-bottom: 20px;
  }
  
  .label {
    font-size: 0.8rem;
  }

  /* Ajustar el formulario secreto */
  .form-group {
    flex-direction: column; /* Input y botón uno encima de otro */
    gap: 5px;
  }
  
  .input-code {
    width: 100%; /* Ocupa todo el ancho */
    box-sizing: border-box;
  }
  
  .btn-reveal {
    width: 100%;
  }
}
</style>