<script>
	import PopulateButton from './PopulateButton.svelte';
  import { v4 as uuidv4 } from 'uuid';
  import { tasks } from '$lib/tasks';
  import Task from './Task.svelte';
  import { onMount } from 'svelte';
  import TaskApi from '$lib/TasksApi';
  import NewTask from './NewTask.svelte';
  import { time } from '$lib/time.js';
  import { onDestroy } from 'svelte';

  let lapse = 0;
  let name = '';
  let project = '';
	// previous is set to record the time accumulated before the pause button is pressed
  let previous = 0;
  let duration = 0;

  // unsubscribe is set to refer to the function used to unsubscribe from the store
  let unsubscribe;
  let changingTask = false;
  let timerOn = false;

  function handleImportTasks(event) {
    const importedTasks = event.detail.exampleTasks;
    importedTasks.forEach(task => {
      $tasks = $tasks.filter(t => t.name != task.name || t.project != task.project);
    });
    $tasks = [ ...$tasks, ...importedTasks]
    TaskApi.saveTasks(importedTasks);
  }

  function handleStartTask(e) {
    if (timerOn) {
      alert("Ya hay una tarea en curso");
      return;
    }

    if (!changingTask){
      if ($tasks.filter(task => task.name == e.detail.name && task.project == e.detail.project).length > 0){
        alert("Ya existe una tarea con ese nombre en ese proyecto");
        return;
      };
    }

    timerOn = true;
    unsubscribe = time.subscribe(value => {
	    lapse = value + previous ;
	  });
    console.log("comenzando", e.detail);
  }

  function terminate() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    timerOn = false;
  }

  function handleEndTask(e){
    console.log("terminando", lapse);
    $tasks = $tasks.filter(task => task.name != e.detail.name || task.project != e.detail.project);
    $tasks = [ {id: uuidv4(), project: e.detail.project, name: e.detail.name, duration: lapse}, ...$tasks];
    TaskApi.saveTasks($tasks);
    lapse = 0;
    previous = 0;
    laps = [];
    terminate();
    name = '';
    project = '';
    changingTask = false;
    duration = 0;
  }

  function handlePauseTask(e) {
    previous = lapse;
    terminate();
    console.log("pausando", e.detail);
  }
  
  $:subscription = !!unsubscribe;
  $:lapsed = !!lapse;
  let laps = [];
  $: timer = lapse;

  function handleDeleteTask(e){
    $tasks = $tasks.filter(task => task.id !== e.detail.id);
    TaskApi.saveTasks($tasks);
  }

  function handleEditTask(e){
    name = e.detail.name;
    project = e.detail.project;
    previous = e.detail.duration;
    changingTask = true;
    handleStartTask(e);
    console.log("editando", e.detail);
  }

  onMount(async () => {
    $tasks = await TaskApi.getTasks();
  });

  onDestroy(() => {
    terminate();
  });
</script>

<style>
  .form-group {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    gap: 10px;
  }

  .body-time {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: 10px;
    background: #009579;
  }

  .populate-div {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .container-tasks {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0;
    background: rgba(255, 255, 255, 0.8);
    border-radius: 5px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    text-align: center;
  }

  th{
    border: 1px  dashed gray;
    padding: 10px;
  }

  table tr:first-child th {
    border-top: 0;
  }

  table tr th:first-child {
    border-left: 0;
  }

  table tr th:last-child {
    border-right: 0;
  }
</style>

<div class="body-time">
    <div class="form-group">
      <NewTask 
        on:endTask={handleEndTask}
        on:pauseTask={handlePauseTask}
        on:startTask={handleStartTask}
        subscription={subscription}
        lapsed={lapsed}
        time={timer}
        name={name}
        duration={duration}
        project={project}
      />
    </div>
    <div class="populate-div">
      <PopulateButton on:importTasks={handleImportTasks} />
    </div>
    <div class="container-tasks">
      <table>
        <tr>
          <th>Proyecto</th>
          <th>Tarea</th>
          <th>Duración</th>
          <th>------</th>
        </tr>
        {#each $tasks as task (task)}
          <Task
            id={task.id} 
            project={task.project}
            name={task.name}
            duration={task.duration}
            on:delete={handleDeleteTask}
            on:edit={handleEditTask}  
          />
        {/each}
      </table>
    </div>
</div>