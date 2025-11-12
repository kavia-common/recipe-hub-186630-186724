<script lang="ts">
    import '../app.css';
    import HeaderNav from '../lib/components/HeaderNav.svelte';
    import SidebarCategories from '../lib/components/SidebarCategories.svelte';
    import Footer from '../lib/components/Footer.svelte';
    import { categoriesStore } from '../lib/stores/categories';
    import { onMount } from 'svelte';
    import { api } from '../lib/services/api';

    let { children } = $props();

    onMount(async () => {
        // Load categories (mock or API based on feature flags)
        try {
            const cats = await api.listCategories();
            categoriesStore.set(cats);
        } catch (e) {
            // leave as default mock if fails
            console.warn('Failed to load categories', e);
        }
    });
</script>

<div class="page">
    <header class="header">
        <HeaderNav />
    </header>
    <div class="content">
        <aside class="sidebar" aria-label="Recipe categories and filters">
            <SidebarCategories />
        </aside>
        <main id="main" tabindex="-1" class="main">
            {@render children()}
        </main>
    </div>
    <Footer />
</div>

<style>
    .page {
        min-height: 100vh;
        background: var(--color-background);
        display: grid;
        grid-template-rows: auto 1fr auto;
    }

    .header {
        background: var(--gradient-header);
        backdrop-filter: saturate(1.2);
        border-bottom: 1px solid rgba(17,24,39,0.08);
        position: sticky;
        top: 0;
        z-index: 10;
    }

    .content {
        display: grid;
        grid-template-columns: 280px 1fr;
        gap: 1rem;
        max-width: var(--container-max);
        width: 100%;
        margin: 0 auto;
        padding: 1rem;
    }

    .sidebar {
        position: sticky;
        top: 74px;
        height: fit-content;
        align-self: start;
    }

    .main {
        min-height: 60vh;
    }

    @media (max-width: 900px) {
        .content {
            grid-template-columns: 1fr;
        }
        .sidebar {
            order: 2;
        }
        .main {
            order: 1;
        }
    }
</style>
