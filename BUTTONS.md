# Button Styles Guide

This document describes the button styles available in the dao.like.co website.

## Button Classes

All buttons use the base `.btn` class plus a variant class for styling.

### Base Button Class (`.btn`)

The `.btn` class provides:
- Consistent padding: `0.75rem 1.5rem`
- Font weight: 600 (semibold)
- Border radius: `0.5rem`
- Smooth hover animation (translateY with shadow)
- Cursor pointer
- No border
- Inline-block display

### Button Variants

#### Primary Button (`.btn-primary`)
- **Background**: Primary color (#28646e)
- **Text**: White
- **Hover**: Darker primary (#1a4a52)
- **Usage**: Main call-to-action buttons

```html
<a href="#" class="btn btn-primary">Read Declaration</a>
```

#### Secondary Button (`.btn-secondary`)
- **Background**: Secondary color (#50e3c2)
- **Text**: Dark gray (#111827)
- **Hover**: Darker secondary (#2fc9a8)
- **Usage**: Secondary actions, alternative CTAs

```html
<a href="#" class="btn btn-secondary">Explore 3ook.com</a>
```

#### White Button (`.btn-white`)
- **Background**: White
- **Text**: Primary color (#28646e)
- **Hover**: Light gray (#f3f4f6)
- **Usage**: Buttons on dark backgrounds (like hero sections)

```html
<a href="#" class="btn btn-white">Visit 3ook.com</a>
```

#### Accent Button (`.btn-accent`)
- **Background**: Accent color (#f7931a)
- **Text**: White
- **Hover**: Darker accent (#d97706)
- **Usage**: Special emphasis, promotional content

```html
<a href="#" class="btn btn-accent">Special Action</a>
```

#### Outline Button (`.btn-outline`)
- **Background**: Transparent
- **Border**: 2px solid (current color)
- **Hover**: Semi-transparent white background
- **Usage**: Minimal style, ghost buttons

```html
<a href="#" class="btn btn-outline">Learn More</a>
```

## Hover Effects

All buttons have:
1. **Lift effect**: Moves up 2px on hover
2. **Shadow**: Adds elevation shadow
3. **Color change**: Background darkens/lightens
4. **Smooth transition**: 0.2s ease-in-out

## Dark Mode Support

The button styles automatically adapt to dark mode:
- `.btn-white` uses slightly different grays in dark mode
- `.btn-secondary` maintains dark text even in dark mode for contrast

## Current Usage in Site

- **Hero Section**: `.btn-secondary` and `.btn-white`
- **What is LikeCoin**: `.btn-primary`
- **3ook.com Section**: `.btn-secondary`
- **Single Pages**: `.btn-secondary`

## Accessibility

All buttons include:
- Proper cursor pointer
- Clear hover states
- Good contrast ratios
- Touch-friendly padding (min 44x44px hit area)
