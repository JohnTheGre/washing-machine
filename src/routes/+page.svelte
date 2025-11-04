<script>
  import { onMount } from 'svelte';

  let washingMachines = [];
  let dryers = [];
  let error = null;
  let loading = true;
  let showCreateMachineForm = false;
  let newLocation = "";
  let newMachineNumber = "";
  let showCreateDryerForm = false;
  let newDryerNumber = "";
  let newDryerLocation = "";

    onMount(() => {
        async function fetchData() {
            try {
                const response = await fetch('http://localhost:3000/api/washing-machines');
                const data = await response.json();
                washingMachines = data;
        } catch (err) {
            error = 'Error fetching data: ' + err.message;
        } finally {
            loading = false;
        }

        try {
            const response = await fetch('http://localhost:3000/api/dryers');
            const data = await response.json();
            dryers = data;
        } catch (err) {
            error = 'Error fetching data: ' + err.message;
        } finally {
            loading = false;
        }
    }

    fetchData();

    // refresh every 30 seconds
    const interval = setInterval(fetchData, 30000);

    return () => clearInterval(interval);
  });   

  async function createMachine() {
    try {
      const response = await fetch('http://localhost:3000/api/washing-machines', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          machineNumber: 'WM' + newMachineNumber,
          location: newLocation,
          status: 'available',
          remaining: 0
        })
      });
      if (!response.ok) {
        throw new Error('Failed to create machine');
      }
      const newMachine = await response.json();
      washingMachines = [...washingMachines, newMachine];
    } catch (err) {
      error = 'Error creating machine: ' + err.message;
    }
  }

  async function createDryer() {
    try {
      const response = await fetch('http://localhost:3000/api/dryers', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          dryerNumber: 'DR' + newDryerNumber,
          location: newDryerLocation,
          status: 'available',
          timer: 0
        })
      });

      if (!response.ok) throw new Error('Failed to create dryer');

      const result = await response.json();
      dryers = [...dryers, result.data]; 

    } catch (err) {
      error = 'Error creating dryer: ' + err.message;
    }
  }

</script>

<div id="washingExpress" class="flex flex-col md:flex-row gap-8 justify-center p-4">
  <main class="p-4">
    {#if loading}
      <p>Loading washing machines...</p>
    {:else if error}
      <p class="text-red-500">{error}</p>
    {:else}
      <h2 class=" flex text-xl font-bold mb-4">
        <img src="plus.png" alt="plus sign" class="ml-2" on:click={() => showCreateMachineForm = !showCreateMachineForm} style="cursor: pointer;">
        Washing Machines
      </h2>
      <ul class="flex flex-wrap gap-4 justify-start">
        {#each washingMachines as machine}
          <li class="border-2 border-blue-500 rounded-lg p-4 bg-gradient-to-r from-sky-100 to-blue-100 hover:from-sky-200 hover:to-blue-200 transition-colors duration-300 min-w-[200px] max-w-[250px]   ">
            <a href={`/washingMachines/${machine.id}`}>
              <p>Number: {machine.machineNumber}</p>
              <p>Location: {machine.location}</p>
              <p class="mb-2">Status: {machine.status}</p>
              <p class="mb-2">Time Left: {machine.remaining} min</p>
            </a>
          </li>
        {/each}
      </ul>
    {/if}
  </main>

  <main2>
    {#if loading}
      <p>Loading dryers...</p>
    {:else if error}
      <p class="text-red-500">{error}</p>
    {:else}
      <h2 class=" flex text-xl font-bold mb-4">
        <img src="plus.png" alt="plus sign" class="ml-2" on:click={() => showCreateDryerForm = !showCreateDryerForm} style="cursor: pointer;">
        Dryers 
      </h2>
      <ul class="flex flex-wrap gap-4 justify-start">
        {#each dryers as dryer}
          <li class="border-2 border-yellow-500 rounded-lg p-4 bg-gradient-to-r from-yellow-100 to-yellow-200 hover:from-yellow-200 hover:to-yellow-300 transition-colors duration-300 min-w-[200px] max-w-[250px]">
            <a href={`/dryers/${dryer.id}`}>
            <p>Number: {dryer.dryerNumber}</p>
            <p>Status: {dryer.status}</p>
            <p>Location: {dryer.location}</p>
            <p>Timer Left: {dryer.remaining} min</p>
          </a>
          </li>
        {/each}
      </ul>
    {/if}
  </main2>
</div>

{#if showCreateMachineForm}
  <div class="flex justify-center w-full mt-6">
    <div class="p-6 bg-white border border-gray-300 rounded-2xl shadow-md flex flex-col gap-3 w-[320px]">
      <h3 class="text-lg font-semibold text-center">Create Washing Machine</h3>
      <input type="text" bind:value={newMachineNumber} placeholder="Machine Number" class="border border-gray-300 rounded px-2 py-1"/>
      <input type="text" bind:value={newLocation} placeholder="Location" class="border border-gray-300 rounded px-2 py-1"/>
      <div class="flex gap-2 justify-center pt-2">
        <button class="bg-blue-500 text-white px-4 py-1 rounded-lg" on:click={() => { createMachine(); showCreateMachineForm = false; }}> Create </button>
        <button class="bg-gray-500 text-white px-4 py-1 rounded-lg" on:click={() => showCreateMachineForm = false}> Cancel </button>
      </div>
    </div>
  </div>
{/if}

{#if showCreateDryerForm}
  <div class="flex justify-center w-full mt-6">
    <div class="p-6 bg-white border border-gray-300 rounded-2xl shadow-md flex flex-col gap-3 w-[320px]">
      <h3 class="text-lg font-semibold text-center">Create Dryer</h3>
      <input type="text" bind:value={newDryerNumber} placeholder="Dryer Number" class="border border-gray-300 rounded px-2 py-1"/>
      <input type="text" bind:value={newDryerLocation} placeholder="Location" class="border border-gray-300 rounded px-2 py-1"/>
      <div class="flex gap-2 justify-center pt-2">
        <button class="bg-yellow-500 text-white px-4 py-1 rounded-lg" on:click={() => { createDryer(); showCreateDryerForm = false; }}> Create </button>
        <button class="bg-gray-500 text-white px-4 py-1 rounded-lg" on:click={() => showCreateDryerForm = false}> Cancel </button>
      </div>
    </div>
  </div>
{/if}
