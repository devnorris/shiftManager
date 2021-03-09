<script>
  import { createEventDispatcher } from 'svelte';

  export let shifts;

  let shiftSelected = null;
  const dispatch = createEventDispatcher();

  const addShift = () => {
    dispatch('addshift', shiftSelected);
    shiftSelected = null;
  };
  console.log(shiftSelected);
</script>

<div>
  <select bind:value={shiftSelected}>
    <option disabled selected={!shiftSelected}>Choose a shift</option>
    {#each shifts as shift}
      <option value={shift}>
        {shift.start} - {shift.end}
      </option>
    {/each}
  </select>
  <button on:click={addShift} disabled={!shiftSelected}>Add</button>
</div>
