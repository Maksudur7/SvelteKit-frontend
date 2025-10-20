<script>
	import { LogIn, Users } from "lucide-svelte";
	import "../../app.css";
	import { goto } from "$app/navigation";
	function home() {
		goto("/");
	}
</script>

<nav class="flex items-center justify-between px-6 py-3 bg-white shadow-sm">
	<div onclick={() => home()} class="flex cursor-pointer items-center space-x-2">
		<div class="p-2 bg-blue-50 rounded-xl">
			<Users class="w-5 h-5 text-blue-600" />
		</div>
		<h1 class="font-semibold text-lg text-gray-800">Person Manager</h1>
	</div>

	<!-- Right: Sign In Button -->
	<a
		href="/login"
		class="flex items-center space-x-1 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition"
	>
		<LogIn class="w-4 h-4" />
		<span>Sign In</span>
	</a>
</nav>
