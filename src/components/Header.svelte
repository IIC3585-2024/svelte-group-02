<script>
  import { tasks } from '$lib/tasks';
  $: tasksCount = $tasks.length;
</script>

<style>
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-direction: column;
    color: #eeeeee;
    font-weight: bold;
    font-size: 1.5em;
  }

  h3 {
    font-size: 0.9em;
    opacity: 0.8;
  }
</style>

<div class="header">
  <div>
    <h2>Bienvenido a TimeTracker </h2>
    <h3> Cantidad de Tareas: {tasksCount} </h3>
  </div>
</div>
