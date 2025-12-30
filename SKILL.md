---
name: enforce-radix-themes
description: Use when writing ANY React/Next.js UI code, creating components, layouts, or modifying pages - prevents inline Tailwind, raw HTML elements (div, button, input, a), and className usage; enforces Radix Themes components and theme props; replaces simple 3rd party components with Radix equivalents, flags complex ones for review
---

# Enforce Radix Themes

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
import { Box, Flex, Heading, Button, TextField } from '@radix-ui/themes';

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
| `className` for styling | `className="flex gap-2"` | Theme props: `gap`, `p`, `size` |

## Allowed className Usage

**className is allowed for:**
- `data-*` attributes for testing: `className="group/message"`
- Animation libraries (Framer Motion): See "Framer Motion" section
- Third-party component integration that requires it

**className is NOT allowed for:**
- Layout (use Flex, Box, Grid)
- Spacing (use gap, p, m props)
- Typography (use size, weight, color props)
- Colors (use color prop or theme tokens)

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

## Responsive Design

**Use object syntax for breakpoints:**

```tsx
// Responsive direction
<Flex direction={{ initial: "column", sm: "row" }}>

// Responsive columns
<Grid columns={{ initial: "1", md: "2", lg: "3" }}>

// Responsive text size
<Text size={{ initial: "2", md: "3" }}>

// Responsive spacing
<Box p={{ initial: "2", md: "4", lg: "6" }}>
```

**Breakpoints:**
- `initial` - Mobile first (0px+)
- `xs` - 520px+
- `sm` - 768px+
- `md` - 1024px+
- `lg` - 1280px+
- `xl` - 1640px+

## Framer Motion Compatibility

**Framer Motion requires DOM elements. Two approaches:**

```tsx
// Option 1: style prop (preferred)
// Use style for layout, keep motion for animation
<motion.div
  style={{ width: '100%' }}
  animate={{ opacity: 1 }}
  data-role={message.role}
>
  <Flex gap="3" align="center">
    {/* Radix components inside */}
  </Flex>
</motion.div>

// Option 2: asChild prop
// Motion element becomes the Box
<Box asChild p="4">
  <motion.div animate={{ opacity: 1 }}>
    <Text>Animated content</Text>
  </motion.div>
</Box>
```

**Rule:** Motion wrappers are the ONE exception where raw elements are allowed, but they should contain NO styling className - only animation props and data attributes.

## Migration Period

**During migration from Tailwind/shadcn to Radix Themes:**

1. Both systems may coexist temporarily
2. New code MUST use Radix Themes
3. When touching existing code, migrate it
4. Don't mix: a component is either fully Tailwind OR fully Radix Themes

```tsx
// BAD - mixed systems
<Flex className="bg-blue-500" gap="4">

// GOOD - pure Radix Themes
<Flex gap="4" style={{ backgroundColor: 'var(--accent-9)' }}>

// ALSO GOOD - if you need custom color, use CSS variable
<Box style={{ backgroundColor: 'var(--cyan-9)' }}>
```

## Third-Party Components

**Replace simple 3rd party components with Radix equivalents. Flag complex ones.**

### Simple Components - Replace

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

### Complex Components - Flag

For complex 3rd party components without direct equivalents, flag and wrap:

```tsx
// FLAG: Complex component - no Radix equivalent
// Wrap in Box for consistent spacing, keep internal styling
<Box mb="4">
  <ThirdPartyDatePicker />
</Box>
```

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
# Should return zero results in migrated components
grep -r "className=" path/to/component.tsx | grep -v "group/" | grep -v "data-"
grep -rE "<div[^>]*>" path/to/component.tsx
grep -rE "<button[^>]*>" path/to/component.tsx
grep -rE "<p>" path/to/component.tsx
```

## Loading States

**RULE: Always use Skeleton for content that takes time to load.**

```tsx
import { Skeleton, Button } from '@radix-ui/themes';

// Wrap content - inherits dimensions
<Skeleton loading={isLoading}>
  <Text>Content appears when loaded</Text>
</Skeleton>

// Button with loading state (auto-disables, shows spinner)
<Button loading={isSaving}>Save</Button>
```

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

// Responsive
<Flex direction={{ initial: "column", md: "row" }}>
<Grid columns={{ initial: "1", md: "2", lg: "3" }}>
<Text size={{ initial: "2", md: "3" }}>
```
