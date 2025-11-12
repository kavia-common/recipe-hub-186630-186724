<script lang="ts">
    import SearchBar from '../../lib/components/SearchBar.svelte';
    import RecipeGrid from '../../lib/components/RecipeGrid.svelte';
    import { searchRecipesStore, searchRecipes } from '../../lib/stores/recipes';
    let query = '';

    async function onSearch(e: CustomEvent<string>) {
        query = e.detail;
        await searchRecipes(query);
    }
</script>

<svelte:head>
    <title>Search Recipes</title>
</svelte:head>

<section class="container">
    <SearchBar on:search={onSearch} />
    {#if $searchRecipesStore.loading}
        <div class="grid">
            {#each [...Array(8).keys()] as i (i)}
                <div class="card skeleton" style="height: 240px;"></div>
            {/each}
        </div>
    {:else if $searchRecipesStore.error}
        <div role="alert" class="card" style="padding:1rem;">
            <p>{$searchRecipesStore.error}</p>
        </div>
    {:else}
        <RecipeGrid recipesStore={searchRecipesStore} />
    {/if}
</section>

<style>
    .grid {
        display: grid;
        grid-template-columns: repeat(4, minmax(0, 1fr));
        gap: 1rem;
        margin-top: 1rem;
    }
    @media (max-width: 1100px) { .grid { grid-template-columns: repeat(3, 1fr); } }
    @media (max-width: 700px) { .grid { grid-template-columns: repeat(2, 1fr); } }
    @media (max-width: 450px) { .grid { grid-template-columns: 1fr; } }
</style>
