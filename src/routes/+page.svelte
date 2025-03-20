<script>
    import { onDestroy } from 'svelte';

    let showVideo = true;
    let showDelayedButton = false;
    let iframeEl;
    let delayTimer;

    // Función que inicia el video y oculta el iframe cuando el video termina
    function goToQuestionnaire() {
      showVideo = false;
      showDelayedButton = false;
      clearTimeout(delayTimer);
      delayTimer = setTimeout(() => {
        showDelayedButton = true;
      }, 120000);
    }

    // Función para reiniciar el proceso
    function resetApp() {
      showVideo = true;
      showDelayedButton = false;
      clearTimeout(delayTimer);
    }

    // Limpiar cualquier temporizador al destruir el componente
    onDestroy(() => {
      clearTimeout(delayTimer);
    });
</script>

<style>
    @font-face {
      font-family: 'HurmeGeometricSans1';
      src: url('/fonts/HurmeGeometricSans1-Regular.otf') format('opentype');
      font-weight: normal;
      font-style: normal;
    }

    body, html {
      margin: 0;
      padding: 0;
      height: 100%;
      overflow: hidden;
    }

    /* Iframe que aparece después del video */
    iframe {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      border: none;
      z-index: 0;
      display: block;
      opacity: 0; /* Inicialmente oculto */
      transition: opacity 0.5s ease;
    }

    iframe.visible {
      opacity: 1; /* Se hace visible cuando cambia el estado */
    }

    /* Contenedor del video */
    .video-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      z-index: 5;
      transition: opacity 0.5s ease;
      opacity: 1;
      pointer-events: auto;
    }

    .video-overlay.hidden {
      opacity: 0;
      pointer-events: none;
    }

    /* Video dentro del contenedor, recorte desde abajo */
    .video-overlay video {
      width: 100%;
      height: 100%;
      object-fit: cover; /* Asegura que el video cubra el contenedor */
      object-position: top; /* Recorta desde la parte inferior */
    }

    .video-overlay button {
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      z-index: 6;
      background: none;
      border: none;
      cursor: pointer;
    }

    /* Botón "Volver" con delay */
   /* Botón "Volver" tamaño intermedio */
.overlay-btn {
  position: fixed;
  top: 93%; /* También lo subí un poco */
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
  background: #ffffff;
  color: #010f1f;
  border: none;
  padding: 8px 35px; /* Punto medio entre 10x60 y 6x20 */
  border-radius: 10px; /* Intermedio */
  cursor: pointer;
  font-size: 20px; /* Tamaño medio entre 24px y 16px */
  font-weight: bold;
  font-family: 'HurmeGeometricSans1', sans-serif;
  box-shadow: 0 5px 9px rgba(0,0,0,0.25);
  transition: all 0.5s ease;
  opacity: 0;
  pointer-events: none;
}

.overlay-btn.active {
  opacity: 1;
  pointer-events: auto;
}

.overlay-btn:hover {
  background: #f0f0f0;
  transform: translate(-50%, -50%) scale(1.05);
}

/* Botón "Participa" */
.participa-btn {
  position: fixed;
  top: 85%; /* Ajusta la posición según sea necesario */
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
  background: #ffffff; /* Revertido al color original */
  color: #010f1f;
  border: none;
  padding: 12px 45px; /* Un poco más grande */
  border-radius: 10px;
  cursor: pointer;
  font-size: 24px; /* Un poco más grande */
  font-weight: bold;
  font-family: 'HurmeGeometricSans1', sans-serif;
  box-shadow: 0 5px 9px rgba(0,0,0,0.25);
  transition: all 0.5s ease;
}

.participa-btn.hidden {
  display: none;
}

.participa-btn:hover {
  background: #f0f0f0; /* Revertido al color original */
  transform: translate(-50%, -50%) scale(1.1); /* Más grande al pasar el ratón */
}

</style>

<!-- Iframe cargado siempre desde el inicio y oculto hasta que el video desaparezca -->
<iframe bind:this={iframeEl} src="https://www.pg.experiencias-oralb.com/zfbq25pk?L=Full+Page" title="Cuestionario de Oral B" class={showVideo ? '' : 'visible'}></iframe>

<!-- Video inicial con transición -->
<div class="video-overlay {showVideo ? '' : 'hidden'}">
    <video src="./Video.mp4" autoplay muted loop playsinline></video> <!-- Changed to relative path -->
    <button aria-label="Ir a la experiencia Oral B" on:click={goToQuestionnaire}></button>
</div>

<!-- Botón "Volver" con delay -->
{#if !showVideo}
  <button 
    class="overlay-btn {showDelayedButton ? 'active' : ''}" 
    on:click={resetApp}>
    Finalizar
  </button>
{/if}

<!-- Botón "Participa" -->
<button 
  class="participa-btn {showVideo ? '' : 'hidden'}" 
  on:click={goToQuestionnaire}>
  ¡Participa!
</button>
