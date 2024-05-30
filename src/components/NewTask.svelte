<script>
    import { createEventDispatcher } from 'svelte';

    let project = '';
    let name = '';
    let duration = '00:10:00';

    const dispatch = createEventDispatcher();
    
    function handleNewTask(e){
        
        dispatch('newTask', {project, name, duration});
    }
</script>

<input type="text" class="form-control" id="projectName" bind:value={project}>
<input type="text" class="form-control" id="projectDescription" bind:value={name}>
<button class="btn btn-primary" on:click={handleNewTask}>Comenzar</button>