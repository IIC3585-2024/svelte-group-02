<script>
  import { tasks } from '$lib/tasks';
  import { formatTime } from '$lib/utils';

  $: totalTime = formatTime($tasks.reduce((duration, task) => duration + task.duration, 0));
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
    text-align: center;
  }
</style>

<div class="header">
  <div>
    <h2>Bienvenido a TimeTracker</h2>
    <h3>Tiempo Total: {totalTime}</h3>
  </div>
</div>
