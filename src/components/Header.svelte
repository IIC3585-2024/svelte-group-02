<script>
//   import { items } from "../stores";

//   $: itemCount = $items.length;
//   $: completedItemCount = $items.filter((item) => item.complete).length;
  import {tasks} from '../tasks';
  $: tasksCount = $tasks.length;
 
</script>

<style>
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 20px;
    color: #eeeeee;
    font-weight: bold;
    font-size: 1.5em;
    }
</style>

<div class="header">
  <div>Bienvenido a TimeTracker {tasksCount}</div>
  <!-- <span> {completedItemCount}/{itemCount} </span> -->
</div>
