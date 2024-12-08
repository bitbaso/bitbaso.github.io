<script>
	import '../app.css';
	import Navbar from '$lib/components/Navbar.svelte'; // Importa el componente de Navbar
	import Footer from '$lib/components/Footer.svelte'; // Importa el componente de Footer
	let { children } = $props();
</script>

<Navbar />
  {@render children()}
<Footer />
