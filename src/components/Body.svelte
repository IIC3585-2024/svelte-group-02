<script>
  import {v4 as uuidv4} from 'uuid';
  import {tasks} from '../tasks';
  import Task from './Task.svelte';
  import {onMount} from 'svelte';
  import TaskApi from '../TasksApi';
  import NewTask from './NewTask.svelte';
  import {time} from '../time.js';

  import { onDestroy } from 'svelte';

    let lapse = 0;
	// previous is set to record the time accumulated before the pause button is pressed
    let previous = 0;

    // unsubscribe is set to refer to the function used to unsubscribe from the store
    let unsubscribe;


    function handleStartTask(e){
      unsubscribe = time.subscribe(value => {
		    lapse = value + previous;
	    });
      console.log("comenzando", e.detail);
    }
        function terminate() {
        // check if unsubscribe is truthy (this to cover the situation in which the stop button is pressed after the pause button)
        if (unsubscribe) {
            unsubscribe();
            unsubscribe = null;
        }
    }


    function handleEndTask(e){
      console.log("terminando", lapse);
      $tasks = [ {id: uuidv4(), project: e.detail.project, name: e.detail.name, duration: lapse}, ...$tasks];
      lapse = 0;
      previous = 0;

      laps = [];
      terminate();
      TaskApi.saveTasks($tasks);
    }



    function handlePauseTask(e){
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
      console.log("editando", e.detail);

    }



    onMount(async () => {
        $tasks = await TaskApi.getTasks();

    });

    onDestroy(() => {
        terminate();
    });

</script>


<!-- svelte-ignore non-top-level-reactive-declaration -->
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
      />
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