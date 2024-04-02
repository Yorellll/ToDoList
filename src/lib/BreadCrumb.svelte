<script lang="ts">
  import type { typeListe } from "./FormList.svelte";

  export let previousList: typeListe;
  export let currentList: typeListe;
</script>

<div class="breadcrumb-content">
  <a aria-label={`Lien vers ${previousList.title}`} href="/{previousList.urlTitle}">{previousList.title}</a>
  /
  <p aria-label={`Lien vers ${currentList.title}`}>{currentList.title}</p>
</div>
