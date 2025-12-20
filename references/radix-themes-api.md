# Radix Themes API Reference

Quick reference for `@radix-ui/themes` components and props.

## Theme Setup

### Basic Setup

```tsx
// app/layout.tsx or _app.tsx
import '@radix-ui/themes/styles.css';
import { Theme } from '@radix-ui/themes';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Theme>
          {children}
        </Theme>
      </body>
    </html>
  );
}
```

### Theme Configuration

```tsx
<Theme
  accentColor="mint"        // Primary color
  grayColor="gray"          // Neutral color
  panelBackground="solid"   // "solid" | "translucent"
  scaling="100%"            // "90%" | "95%" | "100%" | "105%" | "110%"
  radius="medium"           // "none" | "small" | "medium" | "large" | "full"
  appearance="light"        // "light" | "dark" | "inherit"
>
```

### Nested Themes

Override theme settings for a subtree:

```tsx
<Theme accentColor="blue">
  <Card>Main content</Card>

  <Theme accentColor="orange" radius="full">
    <Card>Different accent</Card>
  </Theme>
</Theme>
```

### ThemePanel (Development)

```tsx
import { Theme, ThemePanel } from '@radix-ui/themes';

<Theme>
  <MyApp />
  <ThemePanel />  {/* Live theme editor */}
</Theme>
```

**IMPORTANT: Theme vs CSS**
- Use `<Theme>` component for configuration - NOT CSS overrides
- Component props (`color`, `variant`, `size`) - NOT className
- Theme CSS variables are for reading, not overriding

---

## Layout Components

### Flex

```tsx
import { Flex } from '@radix-ui/themes';

<Flex
  direction="row" | "column" | "row-reverse" | "column-reverse"
  align="start" | "center" | "end" | "baseline" | "stretch"
  justify="start" | "center" | "end" | "between"
  wrap="nowrap" | "wrap" | "wrap-reverse"
  gap="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  gapX="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  gapY="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
>
```

### Grid

```tsx
import { Grid } from '@radix-ui/themes';

<Grid
  columns="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" | string
  rows="repeat(2, 64px)" | string
  flow="row" | "column" | "dense" | "row-dense" | "column-dense"
  align="start" | "center" | "end" | "baseline" | "stretch"
  justify="start" | "center" | "end" | "between"
  gap="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  gapX="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  gapY="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
>
```

### Box

```tsx
import { Box } from '@radix-ui/themes';

<Box
  as="div" | "span"
  // All shared layout props
>
```

### Container

```tsx
import { Container } from '@radix-ui/themes';

<Container
  size="1" | "2" | "3" | "4"  // max-width: 448px, 688px, 880px, 1136px
  align="left" | "center" | "right"
>
```

### Section

```tsx
import { Section } from '@radix-ui/themes';

<Section
  size="1" | "2" | "3" | "4"  // Vertical padding scale
>
```

## Shared Layout Props

Available on Box, Flex, Grid, Container, Section:

```tsx
// Padding (values: "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9")
p, px, py, pt, pr, pb, pl

// Margin (values: "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9")
m, mx, my, mt, mr, mb, ml

// Dimensions
width="100%" | "auto" | string
minWidth="0" | string
maxWidth="100%" | string
height="100%" | "auto" | string
minHeight="0" | string
maxHeight="100%" | string

// Position
position="static" | "relative" | "absolute" | "fixed" | "sticky"
inset="0" | "auto" | string
top="0" | string
right="0" | string
bottom="0" | string
left="0" | string

// Overflow
overflow="visible" | "hidden" | "clip" | "scroll" | "auto"
overflowX="..."
overflowY="..."

// Flex child props
flexBasis="auto" | "0" | string
flexShrink="0" | "1"
flexGrow="0" | "1"

// Grid child props
gridArea="header" | string
gridColumn="1 / 3" | string
gridColumnStart="1" | string
gridColumnEnd="3" | string
gridRow="1 / 3" | string
gridRowStart="1" | string
gridRowEnd="3" | string
```

---

## Typography Components

### Heading

```tsx
import { Heading } from '@radix-ui/themes';

<Heading
  as="h1" | "h2" | "h3" | "h4" | "h5" | "h6"
  size="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  weight="regular" | "medium" | "bold"
  align="left" | "center" | "right"
  trim="normal" | "start" | "end" | "both"
  truncate={true}
  wrap="wrap" | "nowrap" | "pretty" | "balance"
  color="gray" | "gold" | "bronze" | ... // accent colors
  highContrast={true}
>
```

### Text

```tsx
import { Text } from '@radix-ui/themes';

<Text
  as="p" | "span" | "div" | "label"
  size="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  weight="regular" | "medium" | "bold"
  align="left" | "center" | "right"
  trim="normal" | "start" | "end" | "both"
  truncate={true}
  wrap="wrap" | "nowrap" | "pretty" | "balance"
  color="gray" | ... // accent colors
  highContrast={true}
>
```

### Link

```tsx
import { Link } from '@radix-ui/themes';

<Link
  href="#"
  size="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  weight="regular" | "medium" | "bold"
  underline="auto" | "always" | "hover" | "none"
  color="gray" | ... // accent colors
  highContrast={true}
  truncate={true}
>
```

### Strong, Em

```tsx
import { Strong, Em } from '@radix-ui/themes';

<Text>This is <Strong>important</Strong> and <Em>emphasized</Em>.</Text>
```

---

## Interactive Components

### Button

```tsx
import { Button } from '@radix-ui/themes';

<Button
  size="1" | "2" | "3" | "4"
  variant="classic" | "solid" | "soft" | "surface" | "outline" | "ghost"
  color="gray" | "gold" | "bronze" | ... // accent colors
  highContrast={true}
  radius="none" | "small" | "medium" | "large" | "full"
  loading={true}  // Shows spinner, disables button
  disabled={true}
>
```

### IconButton

```tsx
import { IconButton } from '@radix-ui/themes';

<IconButton
  size="1" | "2" | "3" | "4"
  variant="classic" | "solid" | "soft" | "surface" | "outline" | "ghost"
  loading={true}  // Shows spinner
>
  <MagnifyingGlassIcon />
</IconButton>
```

### TextField

```tsx
import { TextField } from '@radix-ui/themes';

<TextField.Root
  size="1" | "2" | "3"
  variant="classic" | "surface" | "soft"
  radius="none" | "small" | "medium" | "large" | "full"
  placeholder="..."
  defaultValue="..."
  value={value}
  onChange={handler}
>
  <TextField.Slot side="left">
    <Icon />
  </TextField.Slot>
</TextField.Root>
```

### TextArea

```tsx
import { TextArea } from '@radix-ui/themes';

<TextArea
  size="1" | "2" | "3"
  variant="classic" | "surface" | "soft"
  placeholder="..."
  resize="none" | "vertical" | "horizontal" | "both"
/>
```

### Checkbox

```tsx
import { Checkbox } from '@radix-ui/themes';

<Checkbox
  size="1" | "2" | "3"
  variant="classic" | "surface" | "soft"
  color="..."
  highContrast={true}
  defaultChecked={true}
  checked={checked}
  onCheckedChange={handler}
/>
```

### Switch

```tsx
import { Switch } from '@radix-ui/themes';

<Switch
  size="1" | "2" | "3"
  variant="classic" | "surface" | "soft"
  color="..."
  highContrast={true}
  radius="none" | "small" | "medium" | "large" | "full"
  defaultChecked={true}
  checked={checked}
  onCheckedChange={handler}
/>
```

### Select

```tsx
import { Select } from '@radix-ui/themes';

<Select.Root defaultValue="apple">
  <Select.Trigger placeholder="Pick a fruit" />
  <Select.Content>
    <Select.Group>
      <Select.Label>Fruits</Select.Label>
      <Select.Item value="apple">Apple</Select.Item>
      <Select.Item value="orange">Orange</Select.Item>
    </Select.Group>
    <Select.Separator />
    <Select.Item value="grape">Grape</Select.Item>
  </Select.Content>
</Select.Root>
```

---

## Loading States

### Skeleton

**RULE: Always use Skeleton for content that takes time to load.**

```tsx
import { Skeleton } from '@radix-ui/themes';

// Wrap content - inherits dimensions
<Skeleton loading={isLoading}>
  <Text>Content appears when loaded</Text>
</Skeleton>

// Fixed dimensions
<Skeleton width="48px" height="48px" />

// With text (wrap text node directly for accurate sizing)
<Text>
  <Skeleton>Lorem ipsum dolor sit amet.</Skeleton>
</Text>
```

### Spinner

```tsx
import { Spinner } from '@radix-ui/themes';

// Standalone
<Spinner />
<Spinner size="1" | "2" | "3" />

// Conditional (preserves child dimensions)
<Spinner loading={isLoading}>
  <Switch defaultChecked />
</Spinner>
```

### Button Loading State

```tsx
// Button with built-in loading spinner
<Button loading>Saving...</Button>

// Preserves button size, shows spinner, auto-disables
<Button loading={isSaving} disabled={isSaving}>
  Save
</Button>
```

### Progress

```tsx
import { Progress } from '@radix-ui/themes';

<Progress
  value={75}
  size="1" | "2" | "3"
  variant="classic" | "surface" | "soft"
  color="blue" | "green" | ...
  radius="none" | "small" | "full"
  highContrast={true}
/>
```

---

## Container Components

### Card

```tsx
import { Card } from '@radix-ui/themes';

<Card size="1" | "2" | "3" | "4" | "5" variant="surface" | "classic" | "ghost">
  <Flex gap="3" direction="column">
    <Text>Card content</Text>
  </Flex>
</Card>

// Card as link
<Card asChild>
  <a href="/page">Clickable card</a>
</Card>
```

### Avatar

```tsx
import { Avatar } from '@radix-ui/themes';

<Avatar
  src="/avatar.jpg"
  fallback="AB"           // Initials if image fails
  size="1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
  radius="none" | "small" | "medium" | "large" | "full"
  variant="solid" | "soft"
  color="..."
/>
```

### Badge

```tsx
import { Badge } from '@radix-ui/themes';

<Badge
  size="1" | "2" | "3"
  variant="solid" | "soft" | "surface" | "outline"
  color="gray" | "gold" | "red" | ...
  radius="none" | "small" | "medium" | "large" | "full"
  highContrast={true}
>
  Pro
</Badge>
```

---

## Animation & Microinteractions

**RULE: All interactive elements should have motion feedback.**

### Data Attributes for Animation

Radix primitives expose `data-state` for CSS animations:

```css
/* Dialog/Modal animations */
.DialogOverlay[data-state="open"] {
  animation: fadeIn 300ms ease-out;
}
.DialogOverlay[data-state="closed"] {
  animation: fadeOut 300ms ease-in;
}

.DialogContent[data-state="open"] {
  animation: slideIn 300ms ease-out;
}
.DialogContent[data-state="closed"] {
  animation: slideOut 300ms ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
@keyframes fadeOut {
  from { opacity: 1; }
  to { opacity: 0; }
}
@keyframes slideIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes slideOut {
  from { opacity: 1; transform: translateY(0); }
  to { opacity: 0; transform: translateY(10px); }
}
```

### Collision-Aware Animations

Use `data-side` for popovers, dropdowns, tooltips:

```css
.PopoverContent {
  animation-duration: 200ms;
  animation-timing-function: cubic-bezier(0.16, 1, 0.3, 1);
}

.PopoverContent[data-side="top"] {
  animation-name: slideUp;
}
.PopoverContent[data-side="bottom"] {
  animation-name: slideDown;
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes slideDown {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}
```

### Origin-Aware Animations

Use CSS custom properties for transform origin:

```css
.DropdownMenuContent {
  transform-origin: var(--radix-dropdown-menu-content-transform-origin);
  animation: scaleIn 200ms ease-out;
}

.PopoverContent {
  transform-origin: var(--radix-popover-content-transform-origin);
}

.TooltipContent {
  transform-origin: var(--radix-tooltip-content-transform-origin);
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}
```

### Recommended Timing

| Interaction | Duration | Easing |
|-------------|----------|--------|
| Hover states | 150ms | ease-out |
| Tooltips | 200ms | cubic-bezier(0.16, 1, 0.3, 1) |
| Modals/Dialogs | 300ms | ease-out (in), ease-in (out) |
| Page transitions | 300-500ms | ease-in-out |
| Skeleton pulse | 1.5s | ease-in-out, infinite |

---

## Colors

### Using Colors via Theme

**Prefer theme props over CSS variables:**

```tsx
// GOOD - theme props
<Text color="gray">Muted text</Text>
<Button color="red">Delete</Button>
<Badge color="green">Active</Badge>

// AVOID - CSS variables directly
<Box style={{ color: 'var(--gray-11)' }}>...</Box>
```

### Available Colors

Accent colors: `gray`, `gold`, `bronze`, `brown`, `yellow`, `amber`, `orange`, `tomato`, `red`, `ruby`, `crimson`, `pink`, `plum`, `purple`, `violet`, `iris`, `indigo`, `blue`, `cyan`, `teal`, `jade`, `green`, `grass`, `lime`, `mint`, `sky`

### Color Scale (1-12)

| Step | Use Case |
|------|----------|
| 1-2 | App background |
| 3-4 | Subtle background, hover states |
| 5 | Active/selected background |
| 6 | Subtle borders, separators |
| 7 | UI element border, focus ring |
| 8 | Hovered border |
| 9 | Solid background (primary) |
| 10 | Hovered solid background |
| 11 | Low-contrast text |
| 12 | High-contrast text |

### Semantic Color Aliases

```css
:root {
  --accent-1: var(--blue-1);
  --accent-9: var(--blue-9);  /* Primary action */

  --success-9: var(--green-9);
  --warning-9: var(--yellow-9);
  --danger-9: var(--red-9);
}
```

---

## Responsive Props

All props support responsive values using object syntax:

```tsx
<Flex
  direction={{ initial: "column", sm: "row" }}
  gap={{ initial: "2", md: "4", lg: "6" }}
>

<Grid
  columns={{ initial: "1", sm: "2", md: "3", lg: "4" }}
>

<Text
  size={{ initial: "2", md: "3" }}
>
```

### Breakpoints

| Key | Width |
|-----|-------|
| `initial` | 0px+ |
| `xs` | 520px+ |
| `sm` | 768px+ |
| `md` | 1024px+ |
| `lg` | 1280px+ |
| `xl` | 1640px+ |

---

## Theme Scale Reference

### Spacing/Gap/Padding Scale

| Value | Pixels |
|-------|--------|
| "1" | 4px |
| "2" | 8px |
| "3" | 12px |
| "4" | 16px |
| "5" | 24px |
| "6" | 32px |
| "7" | 40px |
| "8" | 48px |
| "9" | 64px |

### Font Size Scale

| Value | Size |
|-------|------|
| "1" | 12px |
| "2" | 14px |
| "3" | 16px |
| "4" | 18px |
| "5" | 20px |
| "6" | 24px |
| "7" | 28px |
| "8" | 35px |
| "9" | 60px |

### Container Sizes

| Size | Max Width |
|------|-----------|
| "1" | 448px |
| "2" | 688px |
| "3" | 880px |
| "4" | 1136px |

---

## Common Patterns

### Page Layout

```tsx
<Container size="3">
  <Section size="2">
    <Heading size="8" mb="4">Page Title</Heading>
    <Text color="gray" size="3">Description</Text>
  </Section>

  <Section size="2">
    <Grid columns={{ initial: "1", md: "2" }} gap="4">
      <Card>...</Card>
      <Card>...</Card>
    </Grid>
  </Section>
</Container>
```

### Form Layout

```tsx
<Flex direction="column" gap="4">
  <Box>
    <Text as="label" size="2" weight="medium" mb="1">
      Email
    </Text>
    <TextField.Root placeholder="you@example.com" />
  </Box>

  <Box>
    <Text as="label" size="2" weight="medium" mb="1">
      Message
    </Text>
    <TextArea placeholder="Your message..." />
  </Box>

  <Flex gap="3" justify="end">
    <Button variant="soft">Cancel</Button>
    <Button>Submit</Button>
  </Flex>
</Flex>
```

### Loading State Pattern

```tsx
function UserCard({ user, isLoading }) {
  return (
    <Card>
      <Flex gap="3" align="center">
        <Skeleton loading={isLoading}>
          <Avatar src={user?.avatar} fallback="?" size="4" />
        </Skeleton>

        <Flex direction="column" gap="1">
          <Skeleton loading={isLoading}>
            <Text weight="medium">{user?.name || 'Loading...'}</Text>
          </Skeleton>
          <Skeleton loading={isLoading}>
            <Text size="2" color="gray">{user?.email || 'email@example.com'}</Text>
          </Skeleton>
        </Flex>
      </Flex>
    </Card>
  );
}
```
