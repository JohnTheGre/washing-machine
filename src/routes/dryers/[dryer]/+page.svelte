<script>
    export let params;
    let dryer = null;
    let error = null;
    let newMinutes = 0;

    const loadDryerData = async () => {
        try {
            const response = await fetch(`http://localhost:3000/api/dryers/${params.dryer}`);
            if (!response.ok) {
                throw new Error('Dryer not found');
            }
            dryer = await response.json();
        } catch (err) {
            error = err.message;
        }
    };
    loadDryerData();

    async function setTimer() {
        const response = await fetch(`http://localhost:3000/api/dryers/${params.dryer}/timer`, {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ timer: parseInt(newMinutes) })
        });

        if (response.ok) {
            loadDryerData(); // refresh data
        } else {
            alert("Error setting timer");
        }
    }

    async function deleteDryer() {
        const response = await fetch(`http://localhost:3000/api/dryers/${params.dryer}`, {
            method: "DELETE"
        });

        if (response.ok) {
            // Redirect to dryers list after deletion
            window.location.href = '/dryers';
        } else {
            alert("Error deleting dryer");
        }
    }
</script>

{#if error}
    <p class="text-red-500">Error: {error}</p>
{:else if !dryer}
    <p>Loading dryer data...</p>
{:else}
    <div class="p-4 bg-gradient-to-r from-yellow-100 to-yellow-200 hover:from-yellow-200 hover:to-yellow-300 transition-colors duration-300 rounded shadow">
        <h2 class="text-2xl font-bold mb-4">Dryer: {dryer.dryerNumber}</h2>
        <p class="mb-2">Location: {dryer.location}</p>
        <p class="mb-2">Status: {dryer.status}</p>
        <p class="text-xl font-semibold mt-4 mb-2">Timer:</p>
        {#if dryer.remaining > 0}
         <p>{dryer.remaining} minutes remaining</p>
        {:else}
            <p>No active timer</p>
            <input type="number" bind:value={newMinutes} min="1" class="border p-2 mr-2 cusor" placeholder="Minutes" />
            <button on:click={setTimer} class="bg-blue-500 text-white p-2 rounded cursor-pointer">
                Set Timer
            </button>
            <button on:click={deleteDryer} class="bg-red-500 text-white p-2 rounded cursor-pointer">
                Delete Dryer
            </button>
        {/if}
    </div>
{/if}