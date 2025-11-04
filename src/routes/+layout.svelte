<script>
	import '../app.css';

	let { children } = $props();
</script>

<div class="min-h-screen bg-gradient-to-l from-blue-200 to-blue-100">
	<header class="p-4 bg-blue-100 text-black text-center flex justify-between items-center">
    <h1 class="text-3xl font-bold-mono text-left my-4">Washing Express</h1>
    <nav class="flex justify-end space-x-5">
        <a href="/" class="text-black-600">Home</a>
        <a href="/washingMachines" class="text-black-600">Washing Machines</a>
        <a href="/rooms" class="text-black-600">Rooms</a>
        <a href="/dryers" class="text-black-600">Dryers</a>
        <!-- <a href="/manual" class="text-black-600">Manual</a> -->
    </nav>
</header>
<div class="flex justify-center mt-6">
	<h1 class="bg-white text-black px-6 py-3 border-2 border-blue-500 rounded-lg shadow-md inline-block">
		Welcome to your aid for your home washing machines
	</h1>
</div>

	{@render children()}

	<!-- make the footer movable based on content height -->
	<footer class="footer bg-blue-100 text-black text-center p-7 z-50 mt-4 fixed bottom-0 w-full">
		<p>Made by Osuntuyi John Ayorinde</p>
	</footer>
</div>
