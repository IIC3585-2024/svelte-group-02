<script>
  import { createEventDispatcher } from 'svelte';
  import { formatTime } from '$lib/utils';
  export let subscription, lapsed, time, project, name, duration;
  const dispatch = createEventDispatcher();

  function handlePauseTask(e) {
    dispatch('pauseTask', {project, name, duration});
  }

  function handleStartTask(e) {
    e.preventDefault();
    if (!project || !name) return;
    dispatch('startTask', {project, name, duration});
  }

  function handleEndTask(e) {
    dispatch('endTask', {project, name, duration});
    project = '';
    name = '';
    duration = 0;
  }
</script>

<style>
  .controls{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
  }

  input {
    border-radius: 5px;
  }

  p {
    font-family: "Lucida Console", Monaco, monospace;
    font-size: 15px;
    color: #000000;
    font-weight: 700;
  }
</style>


<input type="text" class="form-control" id="projectName" placeholder="Proyecto" bind:value={project}>
<input type="text" class="form-control" id="projectDescription" placeholder="Tarea" bind:value={name}>
<div class="controls">
    {#if subscription }
        <button class="btn btn-primary" on:click={handleEndTask}>&#x23F9</button>
        <button class="btn btn-primary" on:click={handlePauseTask}>&#x23F8</button>
        <p>{formatTime(time)}</p>
    {:else if lapsed}
        <button class="btn btn-primary" on:click={handleEndTask}>&#x23F9</button>
        <button class="btn btn-primary" on:click={handleStartTask}>&#x23F5</button>
        <p>{formatTime(time)}</p>
    {:else}
        <button class="btn btn-primary" on:click={handleStartTask}>&#x23F5</button>
    {/if}
</div>

