<script>
  import { local_file_store, platform_config_store, media_store_filtered } from "../../stores/store";
  export let medium;

  let src;
  let used_filepath;
  let media_lat, media_long, geolocation;
  if ($platform_config_store["Source of media files"].includes("local")) {
    try {
      used_filepath = $local_file_store[medium.UAR].name;
      src = URL.createObjectURL($local_file_store[medium.UAR]);
    } catch {
      src = null;
    }
  } else {
    used_filepath =
      medium[$platform_config_store["Title of column used for url"]];
    src = used_filepath;
  }
  media_lat = $media_store_filtered[medium.UAR].lat
  media_long = $media_store_filtered[medium.UAR].long
  if (media_lat & media_long){
    geolocation = "Coordinates: " + media_lat + ", " + media_long
  }
  else{
    geolocation = "This item has no geolocation available."
  }
</script>
<div style="font-size:.75em; padding-left:1em;">
  { src }<br/>
  { geolocation }
</div>
{#if src !== null}
  {#if used_filepath.toLowerCase().includes("mp4") || used_filepath
      .toLowerCase()
      .includes("mov") || used_filepath
      .toLowerCase()
      .includes("webm") || used_filepath.toLowerCase().includes("m4v")}
    <div class="medium_video" id={medium.id}>
      <video controls muted {src} type="video/mp4" />
    </div>
  {:else if used_filepath.toLowerCase().includes("png") || used_filepath
      .toLowerCase().includes("jpeg") || used_filepath
      .toLowerCase().includes("jpg") || used_filepath
      .toLowerCase().includes("webp") || used_filepath.toLowerCase().includes("heic")}
    <div class="medium_image" id={medium.id}>
      <!-- svelte-ignore a11y-missing-attribute -->
      <img {src} />
    </div>
  {/if}
{/if}

<style>
  .medium_video {
    width: 100%;
    height: 40vh;
    margin: 0 auto;
    overflow: hidden; /* Add this */
  }

  .medium_image {
    height: 40vh;
    display: flex;
    flex-flow: column;
    flex-direction: row;
    justify-content: center;
  }

  video {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
</style>
