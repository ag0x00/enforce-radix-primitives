---
name: enforce-radix-primitives
description: Use when writing ANY React/Next.js UI code, creating components, layouts, or modifying pages - prevents inline Tailwind, raw HTML elements (div, button, input, a), and className usage; enforces Radix Themes components and theme props; replaces simple 3rd party components with Radix equivalents, flags complex ones for review
---

# Enforce Radix Primitives

## Overview

**Stop perpetuating ad-hoc styling. Use Radix Themes.**

When you see inline Tailwind classes or raw HTML elements, this is technical debt. The fix is not "add more Tailwind" - it's using Radix Themes components governed by a single theme.

## The Rule

**Pages contain ZERO styling code. Only component composition with theme props.**

```tsx
// BAD - Tailwind classes, raw HTML
export default function SettingsPage() {
  return (
    <div className="min-h-screen bg-gray-50 py-8">
      <div className="flex flex-col gap-4">
        <h1 className="text-2xl font-bold">Settings</h1>
        <input className="border rounded-md px-3 py-2" />
        <button className="bg-blue-600 text-white px-4 py-2">Save</button>
      </div>
    </div>
  );
}

// GOOD - Radix Themes components with theme props
import { Box, Flex, Heading, Button } from '@radix-ui/themes';

export default function SettingsPage() {
  return (
    <Box p="6">
      <Flex direction="column" gap="4">
        <Heading size="6">Settings</Heading>
        <TextField.Root placeholder="Username" />
        <Button>Save</Button>
      </Flex>
    </Box>
  );
}
```

## Code Smells - STOP When You See These

| Smell | Example | Replace With |
|-------|---------|--------------|
| `<div>` | `<div className="flex">` | `<Flex>`, `<Box>`, `<Grid>` |
| `<div>` for spacing | `<div className="space-y-4">` | `<Flex direction="column" gap="4">` |
| `<p>` | `<p className="text-sm">` | `<Text size="2">` |
| `<h1>`-`<h6>` | `<h1 className="text-2xl">` | `<Heading size="6">` |
| `<button>` | `<button className="...">` | `<Button variant="...">` |
| `<a>` | `<a href="..." className="...">` | `<Link>` |
| `<input>` | `<input className="...">` | `<TextField.Root>` |
| `className` | Any `className` prop | Theme props: `gap`, `p`, `size` |
| `cn()` | `cn("flex", condition && "bg-blue")` | Component variants |

## Radix Themes Component Mapping

### Layout Primitives

| Instead of | Use | Example |
|------------|-----|---------|
| `<div className="flex gap-4">` | `<Flex>` | `<Flex gap="4" align="center">` |
| `<div className="flex flex-col">` | `<Flex direction="column">` | `<Flex direction="column" gap="3">` |
| `<div className="grid grid-cols-3">` | `<Grid>` | `<Grid columns="3" gap="3">` |
| `<div>` (generic container) | `<Box>` | `<Box p="4" mb="2">` |
| `<main>`, page wrapper | `<Container>` | `<Container size="2">` |
| `<section>` | `<Section>` | `<Section size="2">` |

### Typography

| Instead of | Use | Example |
|------------|-----|---------|
| `<h1>`-`<h6>` | `<Heading>` | `<Heading size="6" weight="bold">` |
| `<p>`, `<span>` | `<Text>` | `<Text size="2" color="gray">` |
| `<a>` | `<Link>` | `<Link href="#" size="2">` |
| `<strong>` | `<Strong>` | `<Strong>important</Strong>` |
| `<em>` | `<Em>` | `<Em>emphasized</Em>` |

### Interactive Components

| Instead of | Use | Example |
|------------|-----|---------|
| `<button>` | `<Button>` | `<Button variant="soft" size="2">` |
| `<input type="text">` | `<TextField.Root>` | `<TextField.Root placeholder="...">` |
| `<textarea>` | `<TextArea>` | `<TextArea size="2">` |
| `<select>` | `<Select.Root>` | Radix Select compound component |
| `<input type="checkbox">` | `<Checkbox>` | `<Checkbox defaultChecked />` |
| `<input type="radio">` | `<RadioGroup>` | Radix RadioGroup compound |

### Theme Props (Replace Tailwind Classes)

| Tailwind | Radix Theme Prop | Values |
|----------|------------------|--------|
| `gap-*` | `gap` | `"1"` - `"9"` |
| `p-*`, `px-*`, `py-*` | `p`, `px`, `py`, `pt`, `pr`, `pb`, `pl` | `"1"` - `"9"` |
| `m-*`, `mx-*`, `my-*` | `m`, `mx`, `my`, `mt`, `mr`, `mb`, `ml` | `"1"` - `"9"` |
| `w-*`, `h-*` | `width`, `height` | CSS values |
| `justify-*` | `justify` | `"start"`, `"center"`, `"end"`, `"between"` |
| `items-*` | `align` | `"start"`, `"center"`, `"end"`, `"baseline"` |
| `flex-wrap` | `wrap` | `"wrap"`, `"nowrap"` |
| `text-sm/lg/xl` | `size` | `"1"` - `"9"` |
| `font-bold` | `weight` | `"regular"`, `"medium"`, `"bold"` |
| `text-gray-500` | `color` | `"gray"`, `"blue"`, `"red"`, etc. |

## Third-Party Components

**Replace simple 3rd party components with Radix equivalents. Flag complex ones.**

### Simple Components → Replace

If a 3rd party component has a direct Radix equivalent, replace it:

| 3rd Party | Radix Replacement |
|-----------|-------------------|
| `react-spinners` | `<Spinner />` |
| `react-loading-skeleton` | `<Skeleton>` |
| Simple tooltip libs | `<Tooltip>` |
| Simple modal libs | `<Dialog.Root>` |
| Simple dropdown libs | `<DropdownMenu.Root>` |
| `react-toggle` | `<Switch />` |
| Simple tabs libs | `<Tabs.Root>` |
| `react-avatar` | `<Avatar>` |
| Simple badge libs | `<Badge>` |
| `react-callout` | `<Callout.Root>` |

```tsx
// BAD - unnecessary dependency
import Skeleton from 'react-loading-skeleton';
<Skeleton height={20} />

// GOOD - use Radix
import { Skeleton, Text } from '@radix-ui/themes';
<Skeleton><Text>Loading...</Text></Skeleton>
```

### Complex Components → Flag or Propose

For complex 3rd party components without direct equivalents:

1. **Flag and leave as-is** if replacement is non-trivial
2. **Propose building** an equivalent from Radix primitives if feasible

```tsx
// Complex component - flag as deviation
// FLAG: ThirdPartyDatePicker - no Radix equivalent, would require building from primitives
// OPTION: Build custom DatePicker using Popover + Calendar grid from primitives
<Box mb="4">
  <ThirdPartyDatePicker />
</Box>

// Complex rich-text editor - leave as-is
// FLAG: TipTap/Slate - complex internals, keep dependency
<Box p="4">
  <RichTextEditor />
</Box>
```

### When to Propose Building

Propose building from primitives when:
- Component is medium complexity (date picker, autocomplete, color picker)
- Only basic features are needed
- Removing the dependency reduces bundle size significantly

Do NOT propose building when:
- Component is highly complex (rich text editors, data grids, charts)
- Advanced features are required (drag-and-drop, virtualization)
- Maintenance burden outweighs dependency cost

## Red Flags - You're About to Violate This

STOP if you think:
- "Just add className, it's faster" - Use theme props
- "Match existing code" - Refactor, don't copy debt
- "It's just one div" - Use `<Box>` or `<Flex>`
- "User said quickly" - Radix components ARE quick
- "className='flex gap-2' is fine" - No. Use `<Flex gap="2">`
- "I need custom spacing" - Use theme scale: `gap="4"` not `gap-[17px]`

## Verification

After writing UI code, verify:

```bash
# Should return zero results in page/route files
grep -r "className=" app/ pages/ --include="*.tsx"
grep -r "<div" app/ pages/ --include="*.tsx"
grep -r "<button" app/ pages/ --include="*.tsx"
grep -r "<p>" app/ pages/ --include="*.tsx"
```

## Loading States

**RULE: Always use Skeleton for content that takes time to load.**

```tsx
import { Skeleton, Spinner, Button } from '@radix-ui/themes';

// Wrap content - inherits dimensions
<Skeleton loading={isLoading}>
  <Text>Content appears when loaded</Text>
</Skeleton>

// Button with loading state (auto-disables, shows spinner)
<Button loading={isSaving}>Save</Button>
```

## Animation & Microinteractions

**RULE: All interactive elements should have motion feedback.**

Use `data-state` and `data-side` attributes for CSS animations:

```css
.DialogContent[data-state="open"] {
  animation: slideIn 300ms ease-out;
}
.PopoverContent[data-side="bottom"] {
  animation: slideDown 200ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

## Documentation

**For detailed API reference, use Context7 to fetch latest docs:**

```
Context7 library IDs:
- /websites/radix-ui_themes     (Radix Themes components)
- /websites/radix-ui-primitives (Radix Primitives)
- /websites/radix-ui_colors     (Color system)
```

Query topics: `layout`, `typography`, `Button`, `animation`, `Skeleton`, `Theme setup`, etc.

See `references/radix-themes-api.md` for offline quick reference.

## Quick Reference

```tsx
// Layout
<Flex gap="3" direction="column" align="center" justify="between">
<Grid columns="3" gap="4">
<Box p="4" mb="2">
<Container size="2">
<Section size="1">

// Typography
<Heading size="6" weight="bold" mb="2">
<Text size="2" color="gray">
<Link href="#" underline="hover">

// Interactive
<Button variant="soft" size="2">
<TextField.Root placeholder="..." size="2">
<TextArea placeholder="..." />
<Checkbox defaultChecked />
<Switch size="2" />

// Loading
<Skeleton loading={isLoading}><Text>...</Text></Skeleton>
<Button loading>Saving...</Button>

// Responsive (object syntax)
<Flex direction={{ initial: "column", md: "row" }}>
<Grid columns={{ initial: "1", md: "2", lg: "3" }}>
```
