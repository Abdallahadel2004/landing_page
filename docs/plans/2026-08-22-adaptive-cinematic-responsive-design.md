# Adaptive Cinematic Responsive Design

## Goal

Make the Mashhoor landing page reliable and polished across desktop, tablet, and phone screens while preserving the cinematic desktop experience and reducing mobile animation cost.

## Current Context

The page is a single `index.html` document with inline CSS and scroll choreography, plus `crispy-chicken-3d.js` for the Three.js chicken scene. The hero uses a sticky 650vh scroll stage, a canvas animation, a clipped orbit reveal, and several absolute-positioned overlays. The repository already contains a branded chicken bucket image suitable as a static fallback.

## Design

### Responsive modes

- Desktop and large tablets keep the 3D chicken and orbit reveal.
- Phones use a static branded chicken image in the hero, with a shorter scroll stage and a readable stacked menu presentation.
- `prefers-reduced-motion: reduce` disables the Three.js scene, scroll-driven interpolation, orbit rotation, hover transforms, and looping decorative motion at every viewport size. Content remains visible in its final readable state.
- The animation mode is selected once on load and recalculated on resize/orientation changes so device rotation does not leave a stale canvas state.

### Hero and scroll behavior

- Add an in-document static hero layer using the existing generated bucket asset, positioned with `object-fit: contain` and constrained by viewport dimensions.
- Keep the canvas available only when the current mode supports it. Avoid creating a WebGL renderer on phone/reduced-motion modes to save memory and battery.
- Use a shorter phone scroll stage and replace the large orbital geometry with a horizontal, touch-friendly menu rail/grid. Desktop timing remains cinematic but avoids unnecessary per-frame DOM writes after the scene has completed.
- Use `requestAnimationFrame` only while the page is visible and the animated mode is active. Pause the render loop when the document is hidden.

### Layout and controls

- Replace viewport-relative overflow traps and invalid asset URLs with local paths that work when the page is opened directly or served.
- Ensure the header, logo, CTA, overlay copy, newsletter form, and footer use stable gutters, min-width constraints, and wrapping behavior at 320px through large desktop widths.
- Keep touch targets at least 44px, remove hover-only affordances on touch devices, and preserve visible focus states.
- Keep the existing brand palette, typography, and content. Do not add a new framework or duplicate the full page markup for mobile.

### Accessibility and resilience

- Preserve semantic headings, labels, and button names.
- Respect reduced-motion preferences in CSS and JavaScript.
- If Three.js or WebGL initialization fails, fall back to the static hero without blocking the rest of the page.
- Keep newsletter feedback usable without relying on animation completing.

## Files

- Modify `index.html`: responsive CSS, static fallback layer, mode selection, scroll/orbit behavior, asset paths, and form/layout resilience.
- Modify `crispy-chicken-3d.js`: expose a lightweight destroy/pause path and guard renderer setup/resize for unsupported devices.
- Use existing `media/Gemini_Generated_Image_w35ze8w35ze8w35z.png` as the static hero fallback.

## Verification

- Run a local static server and load the page at desktop, tablet, and 320px/375px/430px phone widths.
- Verify no horizontal scrolling, clipped text, overlapping controls, or blank hero canvas.
- Verify phone mode does not create a WebGL renderer, reduced-motion mode remains static, and desktop mode still transitions from chicken to menu.
- Verify orientation/resize changes, touch scrolling, CTA links, newsletter submission feedback, and footer links.
- Run a syntax check for inline JavaScript and inspect the final git diff for unrelated changes.
