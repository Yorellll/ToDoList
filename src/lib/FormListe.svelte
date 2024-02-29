<!--//TODO Faire le form qui créé la liste la et qui créé le lien vers celle ci pour rediriger l'utiliteur à la création-->
<script lang="ts">

    import {navigate} from "svelte-routing";

    let title = "";
    let liste = [true];

    const redirectAndCreate = () => {

        localStorage.setItem(title, JSON.stringify(liste))

        navigate(`/${title}`);
    }
</script>

<input id="Title" type="text" bind:value={title}/>
<button on:click={redirectAndCreate} >Créer</button>