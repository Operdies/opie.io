<script>
  export let game;
  let baseDir = `games/${game}`;

  import { onMount } from "svelte";

  // This project is using Sapper, and this sort of DOM manipulation is only available in the browser
  // We therefore need to run it in onMount so it runs in the client browser
  onMount(() => {
    var buildUrl = `${baseDir}/Build`;
    var loaderUrl = `${buildUrl}/${game}.loader.js`;
    var config = {
      dataUrl: `${buildUrl}/${game}.data`,
      frameworkUrl: `${buildUrl}/${game}.framework.js`,
      codeUrl: `${buildUrl}/${game}.wasm`,
      streamingAssetsUrl: "StreamingAssets",
      companyName: "Dong Inc.",
      productName: game,
      productVersion: "0.1",
    };

    var container = document.querySelector("#unity-container");
    var canvas = document.querySelector("#unity-canvas");
    var loadingBar = document.querySelector("#unity-loading-bar");
    var progressBarFull = document.querySelector("#unity-progress-bar-full");
    var fullscreenButton = document.querySelector("#unity-fullscreen-button");
    var mobileWarning = document.querySelector("#unity-mobile-warning");

    // By default Unity keeps WebGL canvas render target size matched with
    // the DOM size of the canvas element (scaled by window.devicePixelRatio)
    // Set this to false if you want to decouple this synchronization from
    // happening inside the engine, and you would instead like to size up
    // the canvas DOM size and WebGL render target sizes yourself.
    // config.matchWebGLToCanvasSize = false;

    if (/iPhone|iPad|iPod|Android/i.test(navigator.userAgent)) {
      container.className = "unity-mobile";
      // Avoid draining fillrate performance on mobile devices,
      // and default/override low DPI mode on mobile browsers.
      config.devicePixelRatio = 1;
      mobileWarning.style.display = "block";
      setTimeout(() => {
        mobileWarning.style.display = "none";
      }, 5000);
    }
    loadingBar.style.display = "block";

    var script = document.createElement("script");
    script.src = loaderUrl;
    script.onload = () => {
      createUnityInstance(canvas, config, (progress) => {
        progressBarFull.style.width = 100 * progress + "%";
      })
        .then((unityInstance) => {
          loadingBar.style.display = "none";
          fullscreenButton.onclick = () => {
            unityInstance.SetFullscreen(1);
          };
        })
        .catch((message) => {
          alert(message);
        });
    };
    document.body.appendChild(script);
  });
</script>

<link rel="stylesheet" href="{baseDir}/TemplateData/style.css" />

<div id="unity-container" class="unity-desktop">
  <canvas id="unity-canvas" />
  <div id="unity-loading-bar">
    <div id="unity-logo" />
    <div id="unity-progress-bar-empty">
      <div id="unity-progress-bar-full" />
    </div>
  </div>
  <div id="unity-mobile-warning">
    WebGL builds are not supported on mobile devices.
  </div>
  <div id="unity-footer">
    <div id="unity-webgl-logo" />
    <div id="unity-fullscreen-button" />
    <div id="unity-build-title">{game}</div>
  </div>
</div>

<style>
  /* Override styles defined by Unity
    ** We want to control the layout in this component
     */
  #unity-container {
    position: relative;
    display: inline-block;    
    width: 1280px;
    height: 720px;
  }
  #unity-canvas {
    width: 1280px;
    height: 720px;
  }
  #unity-container.unity-desktop {
    left: unset;
    top: unset;
    transform: unset;
  }
</style>
