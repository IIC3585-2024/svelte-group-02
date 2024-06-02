<script>
import { createEventDispatcher } from 'svelte';
import { importExampleTasks } from '$lib/utils'

const dispatch = createEventDispatcher();

async function importTasks() {
    const exampleTasks = await importExampleTasks();
	alert('Tareas de ejemplo importadas');
    dispatch('importTasks', {
        exampleTasks: exampleTasks
    });
}
</script>
  
<style>
  button {
	background-color:#00957a;
	border-radius:10px;
	border:2px solid #ffffff;
	display:inline-block;
	cursor:pointer;
	color:#ffffff;
	font-size:15px;
	font-weight:bold;
	padding:15px 14px;
	text-decoration:none;
  }

  button:hover {
	background-color:#117964;
  }

  button:active {
	position:relative;
	top:1px;
  }
</style>
  
<button on:click={importTasks}>
	Importar Tareas de Ejemplo
</button>