<script lang="ts">
    import RecipeForm from '../../lib/components/RecipeForm.svelte';
</script>

<svelte:head>
    <title>Create Recipe</title>
</svelte:head>

<section class="container">
    <div class="card" style="padding:1rem;">
        <h2 style="margin:0 0 1rem 0;">Create a new recipe</h2>
        <RecipeForm mode="create" />
    </div>
</section>
