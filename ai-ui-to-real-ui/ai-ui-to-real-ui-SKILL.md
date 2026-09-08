---
name: ai-ui-to-real-ui
description: Use this skill whenever the user wants to take an AI-generated UI (from v0, Lovable, Bolt, a rough Claude/Cursor mockup, a screenshot, or a pasted component) and integrate it into an existing, working application without breaking functionality or drifting from the app's existing design system. Trigger this any time the user says things like "make this look real," "integrate this mockup," "match my existing UI," "this looks AI-generated," "convert this design into my codebase," or asks to add/restyle a screen, page, or component while preserving current colors, fonts, icons, theme, spacing, or behavior. Also trigger for general "de-AI-ify this UI" or "make my app not look AI-made" requests, and for UX/UI audits of existing screens before or after implementation.
---

# AI UI → Real UI

Convert an AI-generated UI (mockup, generated component, screenshot, or rough prototype) into a real, production-safe part of the existing codebase. This is an **integration pass, not a redesign**: same functionality, same design system, zero visual drift, no invented colors/icons/fonts/theme.

Treat this as work a senior product designer + senior engineer would do together, in this order. Do not skip straight to writing code.

## Step 1 — Audit the existing codebase first

Before touching the mockup, inspect and report back on what already exists:

- Design tokens / theme file / `tailwind.config` / CSS custom properties
- Font stack already in use (`globals.css`, `_app`, `layout`, font imports) — never add a second font family
- Icon library already imported (e.g. `lucide-react`, `heroicons`, `phosphor`) — never mix in a second icon set
- Reusable components already available: Button, Input, Modal, Card, Table, Badge, Dropdown, etc.
- Existing spacing scale, border-radius scale, shadow scale
- Existing patterns for loading / empty / error / success states
- Existing copy voice/tone (formal vs casual, product terminology already used elsewhere)

If any of this is unclear, grep the codebase for the theme/config files before proceeding rather than guessing.

## Step 2 — Intent check (do this before writing UI code)

Answer explicitly, even in a short internal note:

1. Who uses this screen day-to-day, and what's the worst mistake they could make on it?
2. What is the ONE primary action on this screen? Everything else is secondary or tertiary.
3. Which of these six states does this screen need to handle: loading, empty, partial data, error, success, offline? Spec each one before implementing.

## Step 3 — Strip the "AI-generated" look

Actively remove telltale AI UI patterns that don't match the existing app:

- Generic purple/blue gradients, glassmorphism, or default shadcn styling not already used elsewhere in the app
- A second icon library or icon style inconsistent with what's already imported
- Fonts not in the existing stack
- One-off arbitrary Tailwind values (e.g. `bg-[#7c3aed]`, `text-[15px]`) instead of existing design tokens
- Generic placeholder copy ("Welcome back!", "Get Started", "Lorem ipsum") — replace with the app's actual voice
- Decorative filler (blobs, mesh gradients, stock emoji icons, unnecessary illustrations) not used elsewhere in the app
- Over-dense or under-considered layouts, e.g. a 12-column table dumping every field instead of the handful of columns users actually scan

## Step 4 — Rebuild using what already exists

- Reuse existing components instead of writing new styled versions from scratch
- Map every color / spacing / radius / shadow value used in the new UI to existing tokens — never invent new ones
- If the mockup conflicts with the existing design system in any way (color, font, icon, spacing, tone), **the existing system always wins**
- Preserve all existing logic: API calls, state management, routing, form submission/validation, prop and type interfaces — none of it should break or change shape
- Preserve and improve accessibility: labels, focus states, contrast, keyboard navigation, ARIA attributes

## Step 5 — Hierarchy & affordance pass

- Rank actions on the screen: one primary action (solid/filled), secondary (outline/ghost), tertiary (text/icon-only)
- Destructive actions (delete, remove, revoke) belong in an overflow menu with explicit confirmation (typed confirmation for high-stakes actions) — never placed at equal visual weight next to non-destructive actions like Edit
- Anything clickable must look clickable; anything disabled must communicate why (tooltip, helper text, or inline message)
- Feedback (toast, inline validation, spinner) should appear with effectively no perceived delay
- Status/state indicators should never rely on color alone (e.g. pair a status pill with an icon + label, not just green/red text) for colorblind accessibility

## Step 6 — Keep the diff minimal

- Change only what's needed to achieve the visual and functional goal
- Do not touch unrelated files or refactor things that weren't asked for
- Do not rename or reshape existing props, types, or public interfaces

## Step 7 — Report back

After implementation, always summarize:

- Every file changed and why
- Any place a judgment call was made to reconcile the mockup with the existing design system
- Any state (loading/empty/error/offline) that was added but wasn't present in the original AI-generated mockup
- Anything flagged as off-system that was intentionally left out of the mockup (so the user can confirm the call)

## Hard constraint

If the AI-generated mockup's colors, icons, fonts, spacing, or theme conflict with what's already in the codebase, the existing codebase always wins. Never introduce a new design language into the app, even one that looks nicer in isolation — consistency with the real product beats a prettier one-off screen.

## Using this as a standalone audit

If the user just wants an audit (no implementation yet), run Steps 1–3 and Step 5, then return a severity-ranked list of issues (critical / moderate / minor) with concrete fixes, rather than vague commentary like "the spacing feels off."
