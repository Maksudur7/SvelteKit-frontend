<script>
	import Navbar from "$lib/components/Navbar.svelte";
</script>

<div class="min-h-screen bg-gray-50">
	<Navbar />
	<main class="pt-6">
		<slot />
	</main>
</div>
