---
name: frontend-design
description: Create distinctive, production-grade frontend interfaces with high design quality. Use this skill when the user asks to build web components, pages, or applications. Generates creative, polished code that avoids generic AI aesthetics.
license: Complete terms in LICENSE.txt
---

This skill guides creation of distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Implement real working code with exceptional attention to aesthetic details and creative choices.

The user provides frontend requirements: a component, page, application, or interface to build. They may include context about the purpose, audience, or technical constraints.

## Design Thinking

Before coding, understand the context and commit to a BOLD aesthetic direction:
- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Pick an extreme: brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian, etc. There are so many flavors to choose from. Use these for inspiration but design one that is true to the aesthetic direction.
- **Constraints**: Technical requirements (framework, performance, accessibility).
- **Differentiation**: What makes this UNFORGETTABLE? What's the one thing someone will remember?

**CRITICAL**: Choose a clear conceptual direction and execute it with precision. Bold maximalism and refined minimalism both work - the key is intentionality, not intensity.

Then implement working code (HTML/CSS/JS, React, Vue, etc.) that is:
- Production-grade and functional
- Visually striking and memorable
- Cohesive with a clear aesthetic point-of-view
- Meticulously refined in every detail

## Frontend Aesthetics Guidelines

### Visual Design

- **Typography**: Choose fonts that are beautiful, unique, and interesting. Avoid generic fonts like Arial and Inter; opt instead for distinctive choices that elevate the frontend's aesthetics. Pair a distinctive display font with a refined body font. Use fluid typography (clamp()) for responsive scaling. Establish a type scale (1.2-1.5 ratio) for hierarchy.

- **Color & Theme**: Commit to a cohesive aesthetic. Use CSS variables for consistency. Dominant colors with sharp accents outperform timid, evenly-distributed palettes. Ensure WCAG AA contrast ratios (4.5:1 for text, 3:1 for large text). Build a color system with primary, secondary, accent, and semantic colors (success, warning, error).

- **Motion**: Use animations for effects and micro-interactions. Prioritize CSS-only solutions for HTML. Use Motion library for React when available. Focus on high-impact moments: one well-orchestrated page load with staggered reveals (animation-delay) creates more delight than scattered micro-interactions. Use scroll-triggering and hover states that surprise. Respect `prefers-reduced-motion` for accessibility.

- **Spatial Composition**: Unexpected layouts. Asymmetry. Overlap. Diagonal flow. Grid-breaking elements. Generous negative space OR controlled density. Use CSS Grid and Flexbox for modern layouts. Establish a spacing scale (4px, 8px, 16px, 24px, 32px, 48px, 64px) for consistency.

- **Backgrounds & Visual Details**: Create atmosphere and depth rather than defaulting to solid colors. Add contextual effects and textures that match the overall aesthetic. Apply creative forms like gradient meshes, noise textures, geometric patterns, layered transparencies, dramatic shadows, decorative borders, custom cursors, and grain overlays.

### Technical Excellence

- **Responsive Design**: Mobile-first approach. Design for 320px minimum width. Use fluid layouts, flexible images, and media queries. Test on real devices. Breakpoints: 640px (sm), 768px (md), 1024px (lg), 1280px (xl), 1536px (2xl).

- **Performance**: Optimize images (WebP, AVIF), lazy load below-the-fold content, minimize bundle size, achieve Core Web Vitals targets (LCP <2.5s, FID <100ms, CLS <0.1). Use CSS containment and will-change sparingly.

- **Accessibility**: Semantic HTML, ARIA labels where needed, keyboard navigation, focus indicators, skip links, alt text for images, proper heading hierarchy, sufficient color contrast, screen reader testing.

- **Component Architecture**: Build reusable, composable components. Use props for configuration, slots for content. Keep components focused (single responsibility). Avoid prop drilling with context/composition.

- **CSS Architecture**: Use BEM, CSS Modules, or CSS-in-JS consistently. Organize by component, not by type. Use CSS custom properties for theming. Avoid deep nesting (≤3 levels). Keep specificity low.

## Anti-Patterns to Avoid

NEVER use generic AI-generated aesthetics:
- Overused font families (Inter, Roboto, Arial, system fonts, Space Grotesk)
- Cliched color schemes (purple gradients on white backgrounds)
- Predictable layouts and component patterns
- Cookie-cutter design that lacks context-specific character
- Centered hero sections with generic CTAs
- Identical card grids without visual hierarchy
- Overuse of blur effects and glassmorphism

## Design Principles

1. **Intentionality Over Intensity**: Bold maximalism and refined minimalism both work - the key is commitment to a clear vision.

2. **Context-Specific Design**: Every interface should feel designed for its specific purpose, audience, and content. No two designs should be identical.

3. **Visual Hierarchy**: Guide the eye through size, color, contrast, spacing, and motion. The most important element should be unmistakable.

4. **Consistency Within Variation**: Use a design system (spacing, colors, typography) but allow creative expression within that framework.

5. **Progressive Enhancement**: Start with semantic HTML and core functionality, then layer on visual enhancements.

6. **Performance as Design**: Fast interfaces feel better. Optimize aggressively without sacrificing aesthetics.

## Implementation Checklist

### Before Coding
- [ ] Defined clear aesthetic direction (tone, mood, differentiation)
- [ ] Identified target audience and use cases
- [ ] Established technical constraints (framework, browser support, performance budget)
- [ ] Chosen distinctive typography (display + body fonts)
- [ ] Created color palette with contrast-checked values
- [ ] Sketched layout concept (spatial composition, hierarchy)

### During Implementation
- [ ] Semantic HTML structure with proper heading hierarchy
- [ ] Responsive design tested at multiple breakpoints
- [ ] Accessibility: keyboard navigation, ARIA labels, alt text, focus indicators
- [ ] Performance: optimized images, lazy loading, minimal bundle size
- [ ] CSS architecture: organized, maintainable, low specificity
- [ ] Motion: meaningful animations with reduced-motion support
- [ ] Design system: consistent spacing, colors, typography scales

### After Implementation
- [ ] Cross-browser testing (Chrome, Firefox, Safari, Edge)
- [ ] Mobile device testing (iOS, Android)
- [ ] Accessibility audit (screen reader, keyboard-only navigation)
- [ ] Performance audit (Lighthouse, Core Web Vitals)
- [ ] Visual QA: spacing, alignment, typography, colors match design intent
- [ ] Interactive states: hover, focus, active, disabled, loading, error
- [ ] Edge cases: long content, empty states, error states, loading states

## Intensity Levels

### Lite — Polished Basics
- Clean, functional design with good typography and spacing
- Responsive layout with mobile-first approach
- Accessibility fundamentals (semantic HTML, contrast, keyboard nav)
- Subtle animations and transitions
- Performance optimized (images, lazy loading)

### Full — Distinctive Design
- Bold aesthetic direction with unique visual identity
- Custom typography pairing and fluid type scales
- Cohesive color system with CSS variables
- Thoughtful motion design with staggered reveals
- Advanced layouts (Grid, asymmetry, overlap)
- Comprehensive accessibility (ARIA, screen reader testing)
- Performance budget enforced (Core Web Vitals targets)

### Ultra — Unforgettable Experience
- Extreme commitment to aesthetic vision (maximalist OR minimalist)
- Unexpected, memorable design choices throughout
- Advanced motion choreography and scroll-triggered effects
- Custom cursors, textures, and atmospheric details
- Pixel-perfect implementation with meticulous attention to detail
- Full accessibility compliance (WCAG AAA where possible)
- Aggressive performance optimization (sub-second LCP)
- Design system with comprehensive component library

## Boundaries

- **Accessibility is non-negotiable**: Creative expression must not compromise usability for people with disabilities.
- **Performance matters**: Beautiful designs that load slowly fail users. Optimize aggressively.
- **Context over trends**: Design for the specific use case, not for design awards or portfolio pieces.
- **Simplicity when appropriate**: Not every interface needs elaborate animations or complex layouts. Match complexity to purpose.
- **Browser support**: Know your target browsers and test accordingly. Progressive enhancement allows creativity while maintaining compatibility.
- **Maintainability**: Code should be readable and maintainable. Clever CSS tricks should not sacrifice clarity.

**IMPORTANT**: Match implementation complexity to the aesthetic vision. Maximalist designs need elaborate code with extensive animations and effects. Minimalist or refined designs need restraint, precision, and careful attention to spacing, typography, and subtle details. Elegance comes from executing the vision well.

Remember: Claude is capable of extraordinary creative work. Don't hold back, show what can truly be created when thinking outside the box and committing fully to a distinctive vision.