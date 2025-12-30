# Claude Code skill: Enforce Radix primitives

A Claude Code skill that enforces [Radix Themes](https://www.radix-ui.com/themes) components over raw HTML and Tailwind CSS in React/Next.js projects.

## What It Does

This skill automatically triggers when writing React/Next.js UI code and enforces consistent, theme-driven component usage:

| Prevents | Enforces |
|----------|----------|
| Inline Tailwind classes | Theme props (`gap="3"`, `size="2"`) |
| `className` and `cn()` | Component variants |
| Raw HTML (`<div>`, `<button>`, `<p>`) | Radix components (`<Flex>`, `<Button>`, `<Text>`) |
| Simple 3rd party UI libs | Radix equivalents |

## Installation

### Option 1: Clone directly

```bash
git clone https://github.com/ag0x00/enforce-radix-primitives ~/.claude/skills/enforce-radix-primitives
```

### Option 2: Download the .skill file

Download `enforce-radix-primitives.skill` from releases and install via Claude Code.

## Example

```tsx
// BAD - Tailwind classes, raw HTML
<div className="flex flex-col gap-4 p-6">
  <h1 className="text-2xl font-bold">Settings</h1>
  <button className="bg-blue-600 text-white px-4 py-2">Save</button>
</div>

// GOOD - Radix Themes components
import { Flex, Heading, Button } from '@radix-ui/themes';

<Flex direction="column" gap="4" p="6">
  <Heading size="6">Settings</Heading>
  <Button>Save</Button>
</Flex>
```

## Component Mapping

### Layout

| Instead of | Use |
|------------|-----|
| `<div className="flex">` | `<Flex>` |
| `<div>` | `<Box>` |
| `<div className="grid">` | `<Grid>` |
| `<main>` | `<Container>` |
| `<section>` | `<Section>` |

### Typography

| Instead of | Use |
|------------|-----|
| `<h1>`-`<h6>` | `<Heading>` |
| `<p>`, `<span>` | `<Text>` |
| `<a>` | `<Link>` |

### Interactive

| Instead of | Use |
|------------|-----|
| `<button>` | `<Button>` |
| `<input>` | `<TextField.Root>` |
| `<textarea>` | `<TextArea>` |
| `<select>` | `<Select.Root>` |
| `<input type="checkbox">` | `<Checkbox>` |

### Theme Props

| Tailwind | Radix Prop |
|----------|------------|
| `gap-4` | `gap="4"` |
| `p-4`, `px-4` | `p="4"`, `px="4"` |
| `text-sm` | `size="2"` |
| `font-bold` | `weight="bold"` |
| `text-gray-500` | `color="gray"` |

## Third-Party Components

The skill handles 3rd party components intelligently:

### Simple Components → Replace

If a direct Radix equivalent exists, replace it:

| 3rd Party | Radix Replacement |
|-----------|-------------------|
| `react-spinners` | `<Spinner />` |
| `react-loading-skeleton` | `<Skeleton>` |
| Simple tooltip libs | `<Tooltip>` |
| Simple modal libs | `<Dialog.Root>` |
| Simple dropdown libs | `<DropdownMenu.Root>` |
| `react-toggle` | `<Switch />` |
| `react-avatar` | `<Avatar>` |

### Complex Components → Flag

For complex components without direct equivalents:

```tsx
// FLAG: ThirdPartyDatePicker - no Radix equivalent
// OPTION: Build custom using Popover + Calendar primitives
<Box mb="4">
  <ThirdPartyDatePicker />
</Box>

// FLAG: TipTap - complex internals, keep dependency
<RichTextEditor />
```

## Requirements

Install and configure Radix Themes in your project:

```bash
npm install @radix-ui/themes
```

```tsx
// app/layout.tsx
import '@radix-ui/themes/styles.css';
import { Theme } from '@radix-ui/themes';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Theme>{children}</Theme>
      </body>
    </html>
  );
}
```

## Files

```
enforce-radix-primitives/
├── SKILL.md                         # Main skill instructions
├── references/
│   └── radix-themes-api.md          # Offline API reference
└── LICENSE.txt
```

## License

MIT
