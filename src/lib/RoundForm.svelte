<script>
    export let round = 1;
    let estimated = 0;
    let actual = 0;
    let improvement = '';
  </script>
  
  <h3>Runde {round}</h3>
  <label>
    Estimert produksjon:
    <input type="number" bind:value={estimated} />
  </label>
  <label>
    Faktisk produksjon:
    <input type="number" bind:value={actual} />
  </label>
  <label>
    Forbedringsforslag:
    <textarea bind:value={improvement}></textarea>
  </label>
  