# Fix Svelte 5 Runes Mode Error

- [x] Replace `$: typeStyles = getTypeStyles(type);` with `$derived` syntax
- [x] Test that the component still works correctly
- [x] Final design system compliance check - verify design system classes prioritized over Tailwind, NO shadows, NO custom CSS, NO arbitrary values