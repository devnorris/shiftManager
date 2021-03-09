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
          (shiftSelectedStart > start && shiftSelectedStart < end) ||
          (shiftSelectedEnd > start && shiftSelectedEnd < end) ||
          (shiftSelectedStart < start && shiftSelectedEnd > end)
      ).length
        ? (error = 'This shift coincides with one of your logged shifts')
        : (usershifts = [...usershifts, detail]);
    }
  };

  const removeShift = removedShift => {
    error = null;
    usershifts = usershifts.filter(userShift => removedShift !== userShift);
  };
</script>

<div class="shiftLog">
  <h2>User Shift Log</h2>
  <div class="userShifts">
    {#if !usershifts.length}
      <p style="text-align: center">No shifts logged</p>
    {/if}
    {#each usershifts as shift}
      <div class="individualShift">
        <p>{shift.start} - {shift.end}</p>
        <button
          class="removeButton"
          on:click|preventDefault={removeShift(shift)}
          >x
        </button>
      </div>
    {/each}
  </div>

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
    max-width: 500px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 20px;
    margin: 0 auto;
  }
  .userShifts {
    border: 1px solid black;
    padding: 5px;
    width: 100%;
    max-width: 175px;
  }

  .individualShift {
    margin: 0 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid black;
  }
  .individualShift:last-of-type {
    border-bottom: 0;
  }
  .removeButton {
    display: flex;
    align-items: center;
    color: red;
    background-color: transparent;
    border: 2px solid red;
    border-radius: 50%;
    height: 25px;
    width: 25px;
    padding: 0 0 2px 7px;
    margin: 0;
    cursor: pointer;
  }
  .error {
    color: red;
    border: 1px solid red;
    padding: 5px;
    min-width: 100px;
    width: auto;
    margin-bottom: 0;
  }
</style>
