<script lang="ts">
    import RecipeForm from '../../../lib/components/RecipeForm.svelte';
    import { onMount } from 'svelte';
    import { api } from '../../../lib/services/api';
    import type { Recipe } from '../../../lib/types';

    let { data } = $props<{ data: { params: { id: string } } }>();
    let recipe = $state<Recipe | null>(null);
    let loading = $state(true);
    let error = $state<string | null>(null);

    onMount(async () => {
        try {
            recipe = await api.getRecipe(data.params.id);
        } catch {
            error = 'Failed to load recipe for editing.';
        } finally {
            loading = false;
        }
    });
</script>

<svelte:head>
    <title>Edit Recipe</title>
</svelte:head>

<section class="container">
    {#if loading}
        <div class="card skeleton" style="height: 420px;"></div>
    {:else if error}
        <div role="alert" class="card" style="padding:1rem;">
            <p>{error}</p>
        </div>
    {:else if recipe}
        <div class="card" style="padding:1rem;">
            <h2 style="margin:0 0 1rem 0;">Edit recipe</h2>
            <RecipeForm mode="edit" initial={recipe} />
        </div>
    {/if}
</section>
