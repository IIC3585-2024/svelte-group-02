<script>
import { createEventDispatcher } from 'svelte';
import { importExampleTasks } from '$lib/utils'

const dispatch = createEventDispatcher();

async function importTasks() {
    const exampleTasks = await importExampleTasks();
    dispatch('importTasks', {
        exampleTasks: exampleTasks
    });
}
</script>
  
<style>

</style>
  
<button on:click={importTasks}>
	Importar Tareas de Ejemplo
</button>