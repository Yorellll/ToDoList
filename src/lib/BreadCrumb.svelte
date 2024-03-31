<script lang="ts">

    import type {typeListe} from "./FormList.svelte";

    export let previousList: typeListe;
    export let currentList: typeListe;
</script>
<p class="breadcrumb-content">
    <a aria-label={`Lien vers ${previousList.title}`} href="/{previousList.urlTitle}">{previousList.title}</a>
    /
    <a aria-label={`Lien vers ${currentList.title}`} href="/{currentList.urlTitle}">{currentList.title}</a>
</p>
