<script>
  import {createEventDispatcher} from 'svelte';
  import {formatTime} from '$lib/utils';
  export let id, project, name, duration;
  const dispatch = createEventDispatcher();

  function triggerEdit(){
    dispatch('edit', {id, project, name, duration});
  }

  function triggerDelete(){
    dispatch('delete', {id});
  }

</script>

<style>
      td {
      border: 1px  dashed gray;
      padding: 10px;
  }

  table tr:last-child td {
    border-bottom: 0;
  }
  table tr td:first-child {
    border-left: 0;
  }
  table tr td:last-child {
    border-right: 0;
  }
</style>

<tr class="task" >
    <td>{project}</td>
    <td>{name}</td>
    <td>{formatTime(duration)}</td>
    <td>
        <button class="btn btn-primary" on:click={triggerEdit}>&#9658</button>
        <button class="btn btn-primary" on:click={triggerDelete}>X</button>
    </td>
</tr>