<script>
  // $: projectNameInput = "";
  // $: projectDescriptionInput = "";
  import {v4 as uuidv4} from 'uuid';
  import {tasks} from '../tasks';
  import Task from './Task.svelte';
  import {onMount} from 'svelte';
  import TaskApi from '../TasksApi';
  import NewTask from './NewTask.svelte';

    function handleNewTask(e){
      $tasks = [ {id: uuidv4(), project: e.detail.project, name: e.detail.name, duration: e.detail.duration}, ...$tasks];
      TaskApi.saveTasks($tasks);

    }

    function handleDeleteTask(e){
      $tasks = $tasks.filter(task => task.id !== e.detail.id);
    }

    function handleEditTask(e){
      console.log("editando", e.detail);

    }
    
    onMount(async () => {
        $tasks = await TaskApi.getTasks();

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
      <NewTask on:newTask={handleNewTask}/>
      <!-- <input type="text" class="form-control" id="projectName" bind:value={projectNameInput}>
      <input type="text" class="form-control" id="projectDescription" bind:value={projectDescriptionInput}>
      <button class="btn btn-primary">Comenzar</button> -->
    </div>
    <div class="container-tasks">
      <table>
        <tr>
          <th>Proyecto</th>
          <th>Tarea</th>
          <th>Duración</th>
          <th>--------</th>
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