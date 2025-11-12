# Recipe Hub Frontend (SvelteKit)

Modern SvelteKit app with Ocean Professional theme to discover, search, and manage recipes.

## Quick start

1. Install dependencies
   npm install

2. Run the dev server
   npm run dev

The app runs at http://localhost:3000 (or the port configured in VITE_PORT/vite.config.ts).

## Environment variables

Create a .env in the project root (copy from .env.example):

- VITE_API_BASE: Preferred API base URL, e.g. https://api.example.com
- VITE_BACKEND_URL: Fallback API base if VITE_API_BASE is not set
- VITE_FRONTEND_URL: Optional frontend URL (used for redirects/metadata)
- VITE_WS_URL: Optional WebSocket URL
- VITE_FEATURE_FLAGS: Comma-separated flags. Include 'mockData' to use local mock dataset.

If neither VITE_API_BASE nor VITE_BACKEND_URL are set, the API client defaults to '/api'.

## Feature flags

- mockData: Use in-memory dataset and bypass network calls.

## Routes

- /           Home (Discover)
- /search     Search recipes
- /recipes/:id   Recipe details (lazy loaded)
- /create     Create new recipe
- /edit/:id   Edit existing recipe

## Accessibility

Semantic HTML, focus-visible styles, alt text for images, sufficient contrast, and keyboard-friendly components.

## Notes

- Images are loaded lazily with sizes/decoding attributes.
- Skeleton loaders show while fetching data.
- Friendly error and empty states included.
