<svelte:head>
    <!-- Importation de la feuille de style CSS de Pico -->
    <link rel="stylesheet" href="https://unpkg.com/@picocss/pico@latest/css/pico.min.css">
</svelte:head>

<script>
    let toDoList = []; // tableau de todo

    let textInput = "";

    function addToDo() {
        // Ajoute une nouvelle tâche à toDoList avec le contenu de textInput
        toDoList = [...toDoList, {content: textInput, editing: false, checked: false}];
        textInput = ""; // Réinitialise le champ de saisie de texte
    }

    function setEditing(i, isEditing) {
        // Met à jour la propriété "editing" de la tâche correspondante dans toDoList
        toDoList[i].editing = isEditing; // true or false
    }

    function deleteTodo(i) {
        // Supprime la tâche correspondante du tableau toDoList
        toDoList.splice(i, 1);
        toDoList = toDoList; // Rafraîchit toDoList pour déclencher le rendu dans Svelte
    }
</script>

<div style="margin: 0 auto; padding: 20px; width: 700px;">
    <h2 style="text-align: center;">Liste des tâches</h2>
    <p>Entrez vos tâches ici</p>
    <div style="display: flex;">
        <input type="text" bind:value={textInput}>
        <button style="width: 120px;height: 55px; margin-left:10px" on:click={addToDo}>Ajouter</button>
    </div>
</div>

{#each toDoList as toDo, i}
    <div style="display: flex; align-items: baseline; width: 700px; margin: 0 auto;">
        {#if toDo.editing}
            <!-- Affiche un champ de texte pour éditer la tâche -->
            <input type="text" bind:value={toDo.content}>
        {:else}
            <!-- Affiche une case à cocher et le contenu de la tâche -->
            <input type="checkbox" bind:checked={toDo.checked}>
            <h4 style="flex-grow: 1">{toDo.content}</h4>
        {/if}
        <div style="display: flex">
            {#if toDo.editing}
                <!-- Bouton pour sauvegarder les modifications -->
                <button on:click={() => setEditing(i, false)}>Sauvegarder</button>
            {:else}
                <!-- Bouton pour activer le mode d'édition -->
                <button on:click={() => setEditing(i, true)}>Editer</button>
            {/if}
            <!-- Bouton pour supprimer la tâche -->
            <button on:click={() => deleteTodo(i)}>Supprimer</button>
        </div>
    </div>
{/each}
