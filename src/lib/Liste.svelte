<script lang="ts">

    let id = 0;
    let title = String();
    let sousListes: Array<Object> = [];
    let isEnCours = true;

    const addListe = () => {
        sousListes.push();
    }

    const deleteListe = (id: number) => {
        sousListes = sousListes.filter( t => t.id !== id )
    }

</script>

<input id="Title" bind:value={title} />