<script>
	import '../app.css';
	import Navbar from '$lib/components/Navbar.svelte'; // Importa el componente de Navbar
	let { children } = $props();
</script>

<Navbar />
  {@render children()}
