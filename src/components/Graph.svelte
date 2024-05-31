<script>


  import Bar from 'svelte-frappe-charts';

  export let data, options;

  
</script>

<Bar class="chart" data={data} options={options} type="bar" />

