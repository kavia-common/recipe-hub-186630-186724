<script lang="ts">
    import type { PageData } from './$types';
    import { onMount } from 'svelte';
    import { api } from '../../../lib/services/api';
    import type { Recipe } from '../../../lib/types';
    let data: PageData;
    let recipe = $state<Recipe | null>(null);
    let loading = $state(true);
    let error = $state<string | null>(null);

    let RecipeDetailComponent = $state<typeof import('../../../lib/components/RecipeDetail.svelte').default | null>(null);

    onMount(async () => {
        try {
            const module = await import('../../../lib/components/RecipeDetail.svelte');
            RecipeDetailComponent = module.default;
        } catch {
            // ignore lazy load error; page will stay with card skeleton/error elsewhere
        }
    });

    $effect(() => {
        const id = data?.params?.id;
        if (!id) return;
        (async () => {
            loading = true;
            error = null;
            try {
                recipe = await api.getRecipe(id);
            } catch {
                error = 'Unable to load recipe. Please try again.';
            } finally {
                loading = false;
            }
        })();
    });
</script>

<svelte:head>
    <title>{recipe ? recipe.title : 'Recipe Details'}</title>
</svelte:head>

<section class="container">
    {#if loading}
        <div class="card skeleton" style="height: 420px;"></div>
    {:else if error}
        <div role="alert" class="card" style="padding:1rem;">
            <p>{error}</p>
            <button class="btn btn-outline" onclick={() => location.reload()}>Retry</button>
        </div>
    {:else if recipe && RecipeDetailComponent}
        <RecipeDetailComponent {recipe} />
    {/if}
</section>
