**Executive Summary**

This guideline defines the **UI/UX specifications** for the "Model Portfolio Landing Page Template" as displayed on Aura. The specifications enable an AI coding agent to accurately implement a premium, modern web front-end with meticulous attention to design, accessibility, and user experience. Every rule, layout, and interactive element is covered in depth to ensure consistency, scalability, and optimal engagement.

***

# Model Portfolio Landing Page — UI/UX Specifications

## 1. Information Architecture & Layout

- **Header Navigation:**
  - Logo/Brand (left)
  - Navigation links: Portfolio, Campaigns, About, Contact
  - Social icons: X (Twitter), Instagram, LinkedIn, Email
  - Sticky/fixed header recommended for persistent access.
- **Hero Section:**
  - Professional portrait image background
  - Name (AURORA) prominent, large font
  - Subtitle: "International fashion model & brand ambassador."
  - Short blurb: "Capturing elegance and beauty..."
  - CTA buttons under text: View Portfolio, Book Session
- **Featured Campaigns Section:**
  - Section header: "Featured"
  - Grid/list layout for recent work cards
  - Each card: Campaign image, title, summary, tags (Editorial, Runway, etc.), "View details" link
- **Industry Recognition:**
  - Section header: "Industry Recognition"
  - Testimonial slider or stacked cards—quote, photo/avatar, name, role/company
- **Featured In:**
  - Magazine logos (VOGUE, ELLE, Harper’s Bazaar, etc.) in a horizontal list.
- **Contact / Collaboration Section:**
  - Title: "Let's Work Together"
  - Contact details: email, phone, location, response time, availability, languages spoken
  - Contact form: First Name, Last Name, Email, Company/Agency, Project Type (dropdown), Message textarea, Submit Button
- **Services Section:**
  - List of service links: Editorial Fashion, Runway Shows, Commercial Campaigns, Beauty & Cosmetics, Brand Ambassador
- **Experience Section:**
  - List of experience links: Portfolio, Campaigns, Testimonials, Press, Contact
- **Footer:**
  - Copyright, media kit, terms, privacy
  - Aura branding ("Made in Aura"), optional links

***

## 2. Visual & UX Design Principles

- **Color Palette:** Clean, modern, high-fashion—white, off-whites, black, gold accents, muted backgrounds. Use subtle gradients for depth.
- **Typography:** Use luxury/editorial fonts for names/headings (e.g., Serif or Display). Simple, readable sans-serif for body.
  - Hierarchy: Large name, mid-size headers, standard body text
- **Spacing & Layout:** Generous padding/margins, especially around cards and major sections.
- **Imagery:** High-res portrait photos, campaign visuals. Images should adapt responsively—use cover fit or aspect ratio containers.
- **Buttons:** Rounded corners, gold highlight or black, hover/focus effects with slight shadow uplift. Clear CTA labels.
- **Cards:** Shadow, border radius, subtle hover animation to elevate content upon interaction.
- **Forms:** Clean input fields with clear labels, error validation, and focused border states. Dropdown with meaningful options.
- **Accessibility:** All elements must pass WCAG AA contrast. Keyboard navigation and screen reader labels for all interactive controls.
- **Responsiveness:** Mobile-first. Adaptive layout for tablet and desktop. Collapse nav into hamburger menu on mobile.
- **Microinteractions:** Button hover, link underline on hover, animated transitions (fade, slide) for testimonials/carousels.
- **Animation:** Subtle fade-in for section transitions or on-scroll reveals. Avoid excessive animation.

***

## 3. Component Specifications

### 3.1. Header
```markdown
- Navigation link structure (ul > li > a)
- Social icons (SVG or webfont, 24px) with accessible aria-label
- Logo left, nav center, icons right (flex layout)
- Responsive: collapse to hamburger with flyout menu below 800px
```

### 3.2. Hero
```markdown
- Full viewport height (min-height: 90vh)
- Centered text block (headline, subtitle, description)
- Action buttons in row, spaced
- Background image: cover, dark overlay for contrast
- Animation: fade-in headline/text on load
```

### 3.3. Featured Campaigns Grid
```markdown
- Section heading
- Card per campaign (image, title, tags, description, link)
- 3-column grid desktop, 1-column mobile
- Card hover: scale up, shadow
```

### 3.4. Testimonials
```markdown
- Quote, avatar (optional), name, role
- Card style with quote marks icons
- Carousel or stacked mobile view
- Swipe/arrow for carousel navigation
```

### 3.5. Contact Form
```markdown
- Input: First Name, Last Name, Email, Company/Agency
- Dropdown: Project Type (default option + 6 categories)
- Textarea: Message
- Button: Send Message (disabled until required fields completed)
- Error messages below input on invalid, focus highlight
- Accessibility labels for each input
```

### 3.6. Footer
```markdown
- 2-column or stacked for mobile
- Left: copyright, year, site title
- Right: links (media kit, terms, privacy), Aura logo
- Small text, adequate contrast
```

***

## 4. Accessibility & Quality

- All images with alt tags.
- ARIA labels for nav, social icons, form fields.
- Contrast ratios checked and validated.
- Focus outlines for keyboard, skip to content link.
- Error handling: real-time validation, error feedback for form fields.
- Test with screen readers and mobile devices.

***

## 5. Validation & Testing

- Test on Chrome, Safari, Edge, and Firefox (latest versions).
- Confirm responsiveness on iPhone SE, iPhone Pro, iPad, Android.
- Test with keyboard-only navigation.
- Validate with Lighthouse: performance, accessibility, SEO.
- Unit tests for custom components, especially forms and carousels.

***

## 6. Documentation & Handoff

- Provide:
  - Component specs (JSX or HTML structure, CSS modules/scoping)
  - Annotated wireframes (optional, Figma/sketch links)
  - CSS variables for palette and sizing
  - Accessibility notes and testing results
  - README: install dependencies, how to deploy, design/component rationale

***

## 7. Future Improvements

- Consider dark mode toggle, image lazy loading, more animation options
- Optional: add blog, media kit download flow, multi-language switcher
- Regularly review analytics for engagement insights
- Plan for reusable design tokens and component library expansion

***

> By following this specification, AI coding agent can systematically build a high-fidelity, accessible, and elegant model portfolio landing page matching the professional standards seen on Aura.

[1](https://www.aura.build/share/aurora)

https://www.perplexity.ai/search/ai-coding-agent-system-prompt-4jMPruHaQ3eqMQfr3dZ.QA#0
