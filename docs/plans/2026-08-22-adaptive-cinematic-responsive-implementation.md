# Adaptive Cinematic Responsive Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make the Mashhoor landing page reliable and polished across desktop, tablet, and phone screens while preserving cinematic desktop motion and using a static phone/reduced-motion fallback.

**Architecture:** Keep the existing single-page HTML architecture. Add a responsive mode controller that decides whether to initialize Three.js, introduce a static hero layer for unsupported or reduced-motion contexts, simplify mobile menu presentation, and make scroll/render loops pause when unnecessary. Keep content and brand assets unchanged.

**Tech Stack:** HTML, CSS, vanilla JavaScript, Three.js r128, existing local media assets.

---

### Task 1: Add resilient hero fallback and responsive mode tokens

**Files:**
- Modify: `index.html` in the root style block and hero markup

**Steps:**
1. Add CSS variables and base styles for animation mode, safe viewport sizing, touch targets, and the static hero layer.
2. Add a static hero image layer using `media/Gemini_Generated_Image_w35ze8w35ze8w35z.png` behind foreground copy.
3. Add explicit mobile/reduced-motion styles that hide the canvas, show the fallback, shorten the stage, and keep hero copy readable.
4. Replace the missing percent-encoded video source with a guarded local/empty state so a failed video never produces console noise or layout impact.

### Task 2: Make the scroll choreography responsive and motion-aware

**Files:**
- Modify: `index.html` inline script and responsive CSS

**Steps:**
1. Add a `matchMedia`-based mode selector for phone width, coarse pointer, reduced motion, and WebGL availability.
2. Initialize Three.js only in supported animated mode; use a no-op fallback in other modes.
3. Clamp scroll progress against zero-height/short-stage cases and use mode-specific stage heights and reveal timing.
4. In static modes, set final readable overlay states without per-frame DOM writes.
5. Pause the animation loop while the document is hidden and re-evaluate on resize/orientation changes.

### Task 3: Simplify mobile menu presentation and controls

**Files:**
- Modify: `index.html` inline script and responsive CSS

**Steps:**
1. Replace the large orbital transform values on phones with a bounded horizontal menu rail/grid using the existing menu photos.
2. Ensure menu images, CTA buttons, header pill, input, and footer links remain within 320px-wide layouts and meet touch target sizing.
3. Remove hover-dependent transforms on touch devices and add `prefers-reduced-motion` transition overrides.
4. Fix malformed/garbled visible punctuation and copy where safe while keeping content meaning intact.

### Task 4: Harden Three.js lifecycle and resize behavior

**Files:**
- Modify: `crispy-chicken-3d.js`

**Steps:**
1. Guard canvas/context/renderer creation and return a safe API when WebGL is unavailable.
2. Add an explicit pause/resume/destroy-capable API for the page controller.
3. Make resize calculations use the canvas bounding rect and cap pixel ratio for mobile performance.
4. Stop render work after destruction and avoid duplicate resize listeners.

### Task 5: Verify behavior across viewports and motion settings

**Files:**
- No source additions unless a small test harness is required

**Steps:**
1. Run a local static server and load the page at 320px, 375px, 430px, 768px, 1024px, and desktop widths.
2. Confirm no horizontal overflow, clipped text, blank hero, or overlapping controls.
3. Test phone mode, desktop mode, reduced-motion mode, orientation changes, touch scrolling, CTA links, newsletter success feedback, and footer links.
4. Run JavaScript syntax checks and inspect the final diff for unrelated changes.
