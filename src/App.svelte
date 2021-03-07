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

  const availableCompanyShifts = [
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

  let error;

  const updateUserShifts = ({ detail }) => {
    error = null;
    // create an array of yes/no values to see if selected shift falls into each user shifts start and end times
    // check array if any yes's then dont allow shit to be added
    let shiftAllowedArray = usershifts.map(({ start, end }) => {
      if (detail.start >= start && detail.start <= end) {
        console.log('true');
        return true;
      } else {
        console.log('false');
        return false;
      }
    });

    if (shiftAllowedArray?.length) {
      if (shiftAllowedArray.filter(item => item).length) {
        error =
          'Unable to add shift, this shift coinsides with another one of your shifts';
      } else {
        usershifts = [...usershifts, detail];
      }
    } else {
      usershifts = [...usershifts, detail];
    }
  };

  const removeShift = ({ start, end }) => {
    usershifts = usershifts.filter(
      shift => shift.start !== start && shift.end !== end
    );
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
    <p>{error}</p>
  {/if}

  <EnterShift on:addshift={updateUserShifts} shifts={availableCompanyShifts} />
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
</style>
