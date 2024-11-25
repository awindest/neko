<script lang="ts">
	import type { Snippet } from 'svelte';
	let { children }: { children: Snippet } = $props();
	import './styles.css';
</script>

{@render children?.()}
