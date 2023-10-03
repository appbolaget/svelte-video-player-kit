<script>
  import { browser } from "$app/environment";

  import { onMount, onDestroy } from "svelte";
  import { screenfull } from "./libs/screenfull.js";

  export let element;
  export let isFullscreen;
  export let isFullscreenEnabled;

  const userAgent = navigator.userAgent || navigator.vendor || window.opera;

  if (/iPhone/.test(userAgent) && !window.MSStream) {
    isFullscreenEnabled = false;
  } else {
    isFullscreenEnabled = screenfull && screenfull?.isEnabled;
  }

  if (isFullscreenEnabled) screenfull.on("change", onChange);

  function onChange(e) {
    if (element == e.target) isFullscreen = screenfull && screenfull?.isFullscreen;
  }

  $: {
    if (isFullscreenEnabled)
      isFullscreen ? screenfull.request(element) : screenfull.exit();
  }

  onMount(() => {
    if (browser && window.screenfull) {
    }
  });

  onDestroy(() => {
    screenfull.off("change", onChange);
  });
</script>
