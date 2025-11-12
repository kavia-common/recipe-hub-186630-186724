<script lang="ts">
    import RecipeGrid from '../lib/components/RecipeGrid.svelte';
    import SearchBar from '../lib/components/SearchBar.svelte';
    import { recipesStore, loadInitialRecipes } from '../lib/stores/recipes';
    import { onMount } from 'svelte';

    let loading = true;
    let error: string | null = null;

    onMount(async () => {
        try {
            await loadInitialRecipes();
        } catch {
            error = 'Failed to load recipes. Please try again.';
        } finally {
            loading = false;
        }
    });
</script>

<svelte:head>
    <title>Discover Recipes</title>
    <meta name="description" content="Discover tasty recipes to cook today" />
</svelte:head>

<section class="hero">
    <div class="container hero-inner">
        <div>
            <h1>Discover delicious recipes</h1>
            <p class="text-muted">Browse curated dishes, save favorites, and create your own.</p>
        </div>
        <SearchBar />
    </div>
</section>

<section class="container section">
    {#if error}
        <div role="alert" class="card alert">
            <p>{error}</p>
            <button class="btn btn-outline" on:click={() => location.reload()}>Retry</button>
        </div>
    {:else if loading}
        <div class="grid">
            {#each [...Array(8).keys()] as i (i)}
                <div class="card skeleton" style="height: 240px;"></div>
            {/each}
        </div>
    {:else}
        <RecipeGrid {recipesStore} />
    {/if}
</section>

<style>
    .hero {
        background: var(--gradient-header);
        border-bottom: 1px solid rgba(17,24,39,0.08);
    }
    .hero-inner {
        padding: 2rem 1rem;
        display: grid;
        gap: 1rem;
        grid-template-columns: 1fr 420px;
        align-items: center;
    }
    .section {
        padding: 1rem 0 2rem;
    }
    .grid {
        display: grid;
        grid-template-columns: repeat(4, minmax(0, 1fr));
        gap: 1rem;
    }
    .alert {
        padding: 1rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 1rem;
        border: 1px solid rgba(17,24,39,0.08);
    }
    @media (max-width: 1100px) {
        .grid { grid-template-columns: repeat(3, 1fr); }
        .hero-inner { grid-template-columns: 1fr; }
    }
    @media (max-width: 700px) {
        .grid { grid-template-columns: repeat(2, 1fr); }
    }
    @media (max-width: 450px) {
        .grid { grid-template-columns: 1fr; }
    }
</style>
