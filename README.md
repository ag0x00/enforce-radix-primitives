# enforce-radix-primitives

A Claude Code skill that enforces Radix Themes components over raw HTML and Tailwind CSS.

## What it does

This skill triggers whenever you write React/Next.js UI code and:

- **Prevents** inline Tailwind classes, `className`, and `cn()` usage
- **Prevents** raw HTML elements (`<div>`, `<button>`, `<input>`, `<a>`, `<p>`)
- **Enforces** Radix Themes components (`<Flex>`, `<Box>`, `<Button>`, `<Text>`, etc.)
- **Enforces** theme props (`gap="3"`, `size="2"`) instead of utility classes
- **Requires** Skeleton components for loading states
- **Requires** animations for microinteractions

## Installation

```bash
# Clone to your Claude Code skills directory
git clone https://github.com/YOUR_USERNAME/enforce-radix-primitives ~/.claude/skills/enforce-radix-primitives
```

## Example

```tsx
// BAD - This triggers the skill to intervene
<div className="flex flex-col gap-4 p-6">
  <h1 className="text-2xl font-bold">Settings</h1>
  <button className="bg-blue-600 text-white px-4 py-2">Save</button>
</div>

// GOOD - What the skill enforces
import { Flex, Heading, Button } from '@radix-ui/themes';

<Flex direction="column" gap="4" p="6">
  <Heading size="6">Settings</Heading>
  <Button>Save</Button>
</Flex>
```

## Component Mapping

| Instead of | Use |
|------------|-----|
| `<div className="flex">` | `<Flex>` |
| `<div>` | `<Box>` |
| `<div className="grid">` | `<Grid>` |
| `<p>`, `<span>` | `<Text>` |
| `<h1>`-`<h6>` | `<Heading>` |
| `<button>` | `<Button>` |
| `<a>` | `<Link>` |
| `<input>` | `<TextField.Root>` |
| `className="gap-4"` | `gap="4"` |
| `className="p-4"` | `p="4"` |

## Requirements

Your project should have `@radix-ui/themes` installed:

```bash
npm install @radix-ui/themes
```

And configured in your root layout:

```tsx
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

## License

MIT
