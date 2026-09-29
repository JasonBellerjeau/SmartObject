<script>
  let laceTension = 6;
  let connected = true;
  let pressure = { heel: 72, midfoot: 38, forefoot: 55 };
</script>

<div>
  <h3>Self-tightening laces</h3>
  <p>Tension: {laceTension}/10</p>

  <h3>Mobile companion</h3>
  <p>{connected ? 'Connected' : 'Disconnected'}</p>

  <h3>Smart soles</h3>
  {#each Object.entries(pressure) as [zone, value]}
    <p>{zone}: {value}%</p>
  {/each}
</div>