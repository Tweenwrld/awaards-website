# Tailwind CSS v4 Migration Fixes

This document details the fixes applied to resolve utility class errors when migrating from Tailwind CSS v3 to v4.

## Overview

The project was experiencing errors with custom utility classes that were defined in the `@layer utilities` section of the CSS file. Tailwind CSS v4 handles custom utilities differently than v3, causing these classes to be unrecognized.

## Errors Encountered

### 1. `font-general` Utility Class Error
```
[plugin:@tailwindcss/vite:generate:serve] Cannot apply unknown utility class `font-general`
```

### 2. `border-hsla` Utility Class Error
```
[plugin:@tailwindcss/vite:generate:serve] Cannot apply unknown utility class `border-hsla`
```

## Root Cause

In **Tailwind CSS v4**, custom utility classes defined in `@layer utilities` are not processed the same way as in v3. The framework expects utilities to be generated from configuration or use standard Tailwind syntax.

## Fixes Applied

### Fix 1: Font Utility Classes

**Problem:** Custom font utilities were defined in CSS but not recognized by Tailwind v4.

**Original Code in `src/index.css`:**
```css
@layer utilities {
  .font-general {
    font-family: "general", sans-serif;
  }
  
  .font-zentry {
    font-family: "zentry", sans-serif;
  }
  
  .font-circular-web {
    font-family: "circular-web", sans-serif;
  }
  
  .font-robert-medium {
    font-family: "robert-medium", sans-serif;
  }
  
  .font-robert-regular {
    font-family: "robert-regular", sans-serif;
  }
}
```

**Solution:**
1. **Removed** all custom font utility definitions from the CSS file
2. **Replaced** `font-general` usage with inline styles in React components
3. **Updated** CSS classes to use direct `font-family` declarations

**Files Modified:**

**`src/index.css`:**
- Removed custom font utility definitions (lines 40-58)
- Updated `.nav-hover-btn` class:
```css
.nav-hover-btn {
  @apply relative ms-10 text-xs uppercase text-blue-50 /* ... other styles ... */;
  font-family: "general", sans-serif;
}
```

**`src/components/Button.jsx`:**
```jsx
// Before
<span className='relative incline-flex overflow-hidden font-general text-xs uppercase'>

// After
<span className='relative incline-flex overflow-hidden text-xs uppercase' style={{fontFamily: '"general", sans-serif'}}>
```

**`src/components/About.jsx`:**
```jsx
// Before
<h2 className='font-general text-sm uppercase md:text-[10px]'>Welcome to Zentry</h2>

// After
<h2 className='text-sm uppercase md:text-[10px]' style={{fontFamily: '"general", sans-serif'}}>Welcome to Zentry</h2>
```

### Fix 2: Border Utility Class

**Problem:** Custom `border-hsla` utility was not recognized by Tailwind v4.

**Original Code in `src/index.css`:**
```css
@layer utilities {
  .border-hsla {
    @apply border border-white/20;
  }
}

.bento-tilt_1 {
  @apply relative border-hsla col-span-2 overflow-hidden rounded-md transition-transform duration-300 ease-out;
}
```

**Solution:**
1. **Removed** the custom `.border-hsla` utility definition
2. **Replaced** its usage with standard Tailwind classes

**Files Modified:**

**`src/index.css`:**
```css
// Before
.border-hsla {
  @apply border border-white/20;
}

.bento-tilt_1 {
  @apply relative border-hsla col-span-2 overflow-hidden rounded-md transition-transform duration-300 ease-out;
}

// After
.bento-tilt_1 {
  @apply relative border border-white/20 col-span-2 overflow-hidden rounded-md transition-transform duration-300 ease-out;
}
```

## Configuration Preserved

The font family configuration in `tailwind.config.js` was kept intact as it should work with Tailwind v4:

```javascript
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {
      fontFamily: {
        zentry: ["zentry", "sans-serif"],
        general: ["general", "sans-serif"],
        "circular-web": ["circular-web", "sans-serif"],
        "robert-medium": ["robert-medium", "sans-serif"],
        "robert-regular": ["robert-regular", "sans-serif"],
      },
      // ... other configurations
    },
  },
  plugins: [],
};
```

## Best Practices for Tailwind v4

1. **Avoid Custom Utilities in CSS:** Use Tailwind's configuration-based approach instead
2. **Use Standard Tailwind Classes:** Prefer built-in utilities over custom ones
3. **Inline Styles as Fallback:** For complex font families, inline styles can be more reliable
4. **Configuration-Based Customization:** Define custom values in `tailwind.config.js` rather than CSS

## Verification

After applying these fixes:
- ✅ No more `font-general` utility class errors
- ✅ No more `border-hsla` utility class errors
- ✅ Application loads successfully
- ✅ Font families render correctly
- ✅ Border styles display as expected

## Files Modified Summary

1. **`src/index.css`** - Removed custom utility definitions, updated existing classes
2. **`src/components/Button.jsx`** - Replaced `font-general` with inline style
3. **`src/components/About.jsx`** - Replaced `font-general` with inline style

## Dependencies

- **Tailwind CSS:** v4.1.11
- **@tailwindcss/vite:** v4.1.11
- **Vite:** v7.0.4
- **React:** v19.1.0
