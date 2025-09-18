# Responsive & Accessibility Design Notes

This document captures the layout, breakpoints, and accessibility intent for the Oppo Jupiter Ultra Swap Widget scaffold.

## Objectives
- Provide a mobile-first adaptive container for embedding Jupiter widget
- Keep ancillary panels (referral monitor, logs) collapsible / stackable on narrow screens
- Maintain readable tap targets and sufficient contrast

## Breakpoints
| Range | Layout Behavior |
|-------|-----------------|
| 0 – 679px | Single column; widget first; referral panel stacks below |
| 680 – 1099px | Same as mobile but increases padding, larger font scale |
| 1100px+ | Two-column grid: widget (flex-grow) + side panel fixed width (~340px) |

## Sizing Rules
- Grid gap: 1.75rem desktop, 1rem mobile
- Panel radius: 14px (token consistent)
- Min widget height: 560px to avoid jump during initial load

## Typography
- System font stack for fast render & familiar feel
- Clamp-based heading for fluid scaling

## Color & Contrast
- Background: #0d1117
- Panel: #161b22
- Accent gradient: #ffbf3c → #ff8f1f
- Status pills use semantic background + accessible foreground contrast (WCAG AA targeted, verify with tooling)

## Interaction
- Buttons: gradient background with subtle hover ring (box-shadow) and active translation
- Scroll containers: `overflow:auto` with momentum on mobile

## Accessibility (A11y)
| Aspect | Approach |
|--------|---------|
| Color contrast | High contrast palette, verify tokens via tooling (e.g. axe, Lighthouse) |
| Focus states | ToDo: add custom focus ring distinct from hover |
| Reduced motion | ToDo: prefer `prefers-reduced-motion` adjustments for transitions |
| Semantic regions | `header`, `main`, `footer`, list markup for status entries |
| Live updates | Potential future: ARIA live region for fee status list |

## Performance
- Single static HTML, no framework overhead
- Defer Jupiter script load
- Minimal inline CSS; could be extracted if bundle grows

## Future Enhancements
- Collapsible referral panel on mobile (accordion)
- Theme switch (dark/light) with CSS variables
- Print-safe mode (strip interactive controls)
- Skeleton shimmer while widget loads

---
Refine these guidelines as real user testing feedback arrives.