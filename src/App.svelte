<script>
  // --- Comments ---
  // Wont be using Typescript since I have yet to teach myself the basics
  // Styling doesn't look great but would move onto that for the next step

  import EnterShift from './EnterShift.svelte';

  let usershifts = [
    {
      start: '0600',
      end: '1000'
    },
    {
      start: '1600',
      end: '2000'
    }
  ];

  let availableCompanyShifts = [
    {
      start: '0000',
      end: '2359'
    },
    {
      start: '0600',
      end: '1800'
    },
    {
      start: '0000',
      end: '1200'
    },
    {
      start: '0600',
      end: '1200'
    },
    {
      start: '1800',
      end: '2359'
    },
    {
      start: '0000',
      end: '0600'
    },
    {
      start: '1200',
      end: '2359'
    },
    {
      start: '1200',
      end: '1800'
    }
  ];

  $: filteredCompanyShiftOptions = availableCompanyShifts.filter(
    shift => !usershifts.includes(shift)
  );

  let error;

  const updateUserShifts = ({ detail }) => {
    error = null;
    const shiftSelectedStart = detail.start;
    const shiftSelectedEnd = detail.end;

    if (!usershifts.length) {
      usershifts = [...usershifts, detail];
    } else {
      // Filter user shifts based on a few rules below to see if shift selected
      // interfers with shifts alreayd logged
      usershifts.filter(
        ({ start, end }) =>
          shiftSelectedStart === start ||
          shiftSelectedEnd === end ||
          (shiftSelectedStart > start && shiftSelectedEnd < end)
      ).length
        ? (error =
            'Unable to add shift, this shift coinsides with another one of your shifts')
        : (usershifts = [...usershifts, detail]);
    }
  };

  const removeShift = removedShift => {
    error = null;
    usershifts = usershifts.filter(userShift => removedShift !== userShift);
  };
</script>

<div>
  <h2>User Shifts</h2>
  {#each usershifts as shift}
    <div class="shiftLog">
      <p>{shift.start} - {shift.end}</p>
      <button class="removeButton" on:click|preventDefault={removeShift(shift)}>
        <p class="removeX">x</p>
      </button>
    </div>
  {/each}

  {#if error}
    <p class="error">{error}</p>
  {/if}

  <EnterShift
    on:addshift={updateUserShifts}
    shifts={filteredCompanyShiftOptions}
  />
</div>

<style>
  .shiftLog {
    display: flex;
    justify-content: space-between;
    border: 1px solid blue;
    padding: 5px;
    margin: 10px 0;
    max-width: 200px;
    width: 100%;
  }
  .removeButton {
    background-color: transparent;
    border: none;
  }
  .removeX {
    color: red;
  }
  .error {
    color: red;
    border: 1px solid red;
    padding: 5px;
    min-width: 100px;
    width: auto;
  }
</style>
