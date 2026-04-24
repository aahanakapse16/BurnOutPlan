# Design System Document: High-End Editorial Burnout Recovery

## 1. Overview & Creative North Star
**The Creative North Star: "The Digital Sanctuary"**

This design system is not a utility tool; it is a restorative environment. To move beyond the "standard SaaS" aesthetic, we are employing a **High-End Editorial** approach. We treat the interface like a premium wellness journal—one that breathes, values negative space, and prioritizes serenity over density.

To break the "template" look, we avoid rigid grids in favor of **Intentional Asymmetry**. Overlapping elements, generous margins, and a sophisticated typographic scale ensure the user feels they are entering a curated experience, not a database. This system is designed to reduce cognitive load by using soft tonal shifts rather than harsh structural lines.

---

## 2. Colors & Surface Logic
The palette is rooted in nature and earth tones, designed to lower cortisol levels.

### Color Tokens
*   **Primary (Sage):** `#376847` (Base) | `#6B9E78` (Container/Action)
*   **Secondary (Sand):** `#E8DFD0` (Container) | `#645E52` (On-Secondary)
*   **Tertiary (Teal/Success):** `#066A5F` (Base) | `#4FA093` (Container)
*   **Background (Canvas):** `#F9F7F4`
*   **Surface Tiers:**
    *   `surface-container-lowest`: `#FFFFFF` (Highest prominence cards)
    *   `surface-container-low`: `#F5F3F0` (Secondary sections)
    *   `surface-container-high`: `#EAE8E5` (Interactive elements/input backgrounds)

### The "No-Line" Rule
**Explicit Instruction:** Prohibit 1px solid borders for sectioning or containment. Boundaries must be defined solely through background color shifts. For example, a `surface-container-low` section should sit directly on the `background` canvas. The lack of "hard" lines reduces visual noise and mimics the soft edges of the natural world.

### Surface Hierarchy & Nesting
Treat the UI as a series of physical layers—like stacked sheets of fine, heavy-weight paper. 
*   **Layer 1 (The Desk):** `background` (#F9F7F4).
*   **Layer 2 (The Journal):** `surface-container-low` for large content areas.
*   **Layer 3 (The Insight):** `surface-container-lowest` (Pure White) for the most critical actionable cards.

### Glass & Gradient Logic
To add "soul," avoid flat colors for hero moments. Use a subtle linear gradient from `primary` to `primary_container` (at a 45-degree angle) for high-impact CTAs. For floating navigation or modal overlays, use **Glassmorphism**: a semi-transparent `surface` color with a `backdrop-blur` of 12px-20px to maintain a sense of lightness.

---

## 3. Typography: Editorial Authority
We pair a soft, approachable Display face with a highly legible, sophisticated Body face.

### The Scale
*   **Display (Plus Jakarta Sans):** Used for "Check-in" moments and daily quotes. Large, bold, and welcoming.
    *   `display-lg` (3.5rem): Reserved for hero headlines.
*   **Headline (Plus Jakarta Sans):** For section titles.
    *   `headline-md` (1.75rem): Used to introduce new recovery modules.
*   **Title (Manrope):** For card titles and navigation headers.
    *   `title-lg` (1.375rem): Semi-bold to provide structure without aggression.
*   **Body (Manrope):** For all reading content.
    *   `body-lg` (1rem): Optimized for 1.6 line-height to ensure a restful reading experience.
*   **Label (Manrope):** Small-caps or reduced-size text for metadata.
    *   `label-md` (0.75rem): Used for timestamps or "Time to complete" tags.

---

## 4. Elevation & Depth
In this system, depth is felt, not seen. We move away from "Material" drop shadows toward **Tonal Layering**.

*   **The Layering Principle:** Use the Surface Scale. Place a `surface-container-lowest` (#FFFFFF) card on a `surface-container-low` (#F5F3F0) background. This creates a natural "lift" that is easier on the eyes than a shadow.
*   **Ambient Shadows:** Use only for elements that truly "float" (e.g., a Bottom Sheet or FAB). Shadows must be:
    *   `color`: `on-surface` at 6% opacity.
    *   `blur`: 24px - 40px.
    *   `spread`: -4px.
*   **The "Ghost Border" Fallback:** If a border is required for accessibility (e.g., in a high-glare environment), use the `outline-variant` token at **15% opacity**. Never use 100% opaque borders.

---

## 5. Components

### Buttons
*   **Primary:** `primary` background with `on-primary` text. No shadow. 16px corner radius.
*   **Secondary:** `secondary-container` background. This provides a "soft" tactile feel.
*   **Tertiary:** No background. Text-only with a subtle underline or icon.

### Cards & Recovery Modules
*   **Constraint:** Forbid divider lines within cards. 
*   **Separation:** Use vertical white space (32px or 48px) to separate content sections.
*   **Visual Interest:** Use `primary-fixed-dim` for small decorative accents or progress indicators within cards to add a "signature" sage touch.

### Input Fields
*   **Style:** `surface-container-high` background. No border. 
*   **Focus State:** A soft 2px outer glow using `primary_container` at 30% opacity. Avoid harsh color changes.

### Sanctuary-Specific Components
*   **The Breath-Work Pulse:** A large, circular component using a radial gradient of `primary-container` to `surface`, used for timed breathing exercises.
*   **Tonal Progress Rings:** Use `tertiary` (Teal) for "Recovery Progress" to distinguish it from "Task Completion" (Sage).

---

## 6. Do's and Don'ts

### Do
*   **Do** use asymmetrical margins. If a card is centered, give it extra breathing room on the top to create an "Editorial" feel.
*   **Do** use `on-surface-variant` for secondary text to keep the contrast soft and readable.
*   **Do** prioritize "White Space" as a functional element of the recovery process.

### Don't
*   **Don't** use 1px dividers. If you feel the need for a line, use a background color change instead.
*   **Don't** use pure black (#000000). Always use `on-surface` (#1B1C1A) to keep the aesthetic warm.
*   **Don't** use tight corner radii. Stay strictly within the `lg` (16px) or `xl` (24px) range to maintain the "spa-like" softness.
*   **Don't** crowd the screen. If a module feels "busy," split it into two separate views.