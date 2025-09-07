# WikiFlow - TikTok-Style Wikipedia Reader

A serene and addictive iPhone app for wandering through Wikipedia knowledge with intuitive one-thumb navigation. Built with React, TypeScript, and Tailwind CSS.

## 🎯 What You're Building

WikiFlow is a mobile-first web application that transforms Wikipedia browsing into a TikTok-like experience:
- **Vertical swiping** to discover new topics from different categories
- **Horizontal swiping** for related/adjacent articles  
- **Compact Twitter integration** showing real-time discussions about topics
- **iPhone-native design** with proper safe areas, haptic feedback, and iOS gestures
- **Physics-based animations** with spring transitions and momentum
- **Long-press quick actions** for saving, sharing, and managing articles

## 🏗️ Architecture Overview

```
WikiFlow/
├── App.tsx                     # Main app with iPhone shell and state management
├── components/
│   ├── SwipeContainer.tsx      # TikTok-style gesture handling & article transitions
│   ├── ArticleCard.tsx         # Wikipedia article display with Twitter integration  
│   ├── TweetEmbed.tsx          # Compact Twitter/X post components
│   ├── QuickActions.tsx        # Long-press bottom sheet modal
│   ├── SearchOverlay.tsx       # Pull-down search interface
│   └── ui/                     # Shadcn/ui components library
├── data/
│   ├── articles.ts             # Wikipedia article data & topic diversity logic
│   └── tweets.ts               # Twitter/X posts with verified users & timestamps
├── hooks/
│   └── useSwipeGestures.ts     # Physics-based gesture detection system
└── styles/
    └── globals.css             # Tailwind V4 + iOS-specific styling
```

## 📋 Prerequisites

- **Node.js** 18+ and npm
- **Code editor** (VS Code recommended)
- **Basic knowledge** of React, TypeScript, and Tailwind CSS
- **iPhone** or iPhone simulator for testing (optional but recommended)

## 🚀 Step-by-Step Setup

### 1. Initialize React Project

```bash
npx create-react-app wikiflow --template typescript
cd wikiflow
```

### 2. Install Dependencies

```bash
# Core dependencies
npm install motion/react lucide-react sonner@2.0.3

# Development dependencies  
npm install -D tailwindcss@next @tailwindcss/typography
```

### 3. Configure Tailwind CSS

Create `tailwind.config.js`:
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### 4. Project Structure Setup

```bash
# Create directory structure
mkdir -p src/components/ui src/components/figma src/data src/hooks src/styles src/guidelines

# Remove default files
rm src/App.css src/App.test.tsx src/logo.svg src/reportWebVitals.ts src/setupTests.ts
```

## 🎨 Design System Specifications

### Color Palette
```css
/* Light Mode */
--background: #ffffff
--foreground: #2a2a2a  
--primary: #030213
--secondary: #f3f3f5
--muted: #ececf0
--accent: #e9ebef
--border: rgba(0, 0, 0, 0.1)

/* Dark Mode */
--background: #2a2a2a
--foreground: #ffffff
--primary: #ffffff
--secondary: #404040
--muted: #404040
--accent: #404040
--border: #404040
```

### Typography System
```css
/* iOS San Francisco Font Stack */
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;

/* Font Sizes (iOS-optimized) */
h1: 2rem (32px) - Article titles
h2: 1.5rem (24px) - Section headers  
h3: 1.25rem (20px) - Subsections
p: 1rem (16px) - Body text
small: 0.875rem (14px) - Metadata
```

### Spacing Scale
```css
/* Consistent 4px grid system */
gap-1: 4px
gap-2: 8px  
gap-3: 12px
gap-4: 16px
gap-6: 24px
gap-8: 32px
```

### Border Radius
```css
--radius: 10px (iOS standard)
rounded-xl: 12px (cards)
rounded-2xl: 16px (modals)
rounded-full: 50% (avatars, pills)
```

## 📱 Core Files Implementation

### 1. Global Styles (`src/styles/globals.css`)

**EXACT COPY**: Use the provided `globals.css` file exactly as shown. Key features:
- **iOS font system** with San Francisco font stack
- **Safe area support** for iPhone notch/Dynamic Island
- **Custom CSS variables** for consistent theming
- **Tailwind V4 integration** with custom color tokens
- **Typography hierarchy** with proper line heights
- **Scrollbar hiding** utilities for clean mobile experience

### 2. Main App Component (`src/App.tsx`)

**EXACT COPY**: Use the provided `App.tsx` file exactly. Critical design elements:

#### iPhone Shell Design
```tsx
{/* iPhone Status Bar - EXACT positioning */}
<div className="absolute top-0 left-0 right-0 h-11 bg-black z-50" 
     style={{ paddingTop: 'env(safe-area-inset-top)' }}>
  <div className="flex items-center justify-between px-6 pt-2">
    <div className="text-white text-sm font-medium">9:41</div>
    {/* Battery/signal indicators */}
  </div>
</div>

{/* Rounded app container - EXACT styling */}
<div className="absolute inset-0 bg-background rounded-t-3xl overflow-hidden"
     style={{ 
       paddingTop: 'max(env(safe-area-inset-top), 44px)',
       paddingBottom: 'env(safe-area-inset-bottom)',
       marginTop: '44px'
     }}>
```

#### State Management Pattern
```tsx
// Core state hooks - DO NOT MODIFY
const [showQuickActions, setShowQuickActions] = useState(false);
const [showSearch, setShowSearch] = useState(false);
const [selectedArticle, setSelectedArticle] = useState<Article | null>(null);
const [savedArticles, setSavedArticles] = useState<Article[]>([]);
```

### 3. Swipe Container (`src/components/SwipeContainer.tsx`)

**KEY DESIGN SPECIFICATIONS:**

#### TikTok-Style Animation Values
```tsx
// Transition timing - EXACT values for TikTok feel
transition={{ 
  type: "spring", 
  damping: 35, 
  stiffness: 500,
  duration: 0.25 // Faster like TikTok
}}
```

#### Gesture Transform Mathematics
```tsx
// Scale and opacity calculations - EXACT formulas
transform: `translateY(-${translateAmount}px) scale(${1 - progress * 0.02})`
opacity: 1 - progress * 0.1
```

#### Category Indicator Styling
```tsx
{/* EXACT positioning and styling */}
<div className="absolute top-6 right-4 z-20">
  <div className="bg-black/60 text-white px-3 py-1.5 rounded-full text-xs font-medium backdrop-blur-sm">
    {currentArticle.category}
  </div>
</div>
```

### 4. Article Card (`src/components/ArticleCard.tsx`)

**CRITICAL DESIGN ELEMENTS:**

#### Progress Bar Styling
```tsx
{/* Sticky header with EXACT backdrop blur */}
<div className="sticky top-0 z-10 bg-background/95 backdrop-blur-xl border-b border-border/30">
  <Progress value={(currentSection + 1) / article.sections.length * 100} 
           className="h-0.5 rounded-none bg-gray-200 dark:bg-gray-800" />
```

#### Section Navigation Pills
```tsx
{/* iOS-style pills - EXACT styling */}
<button className={`px-4 py-2 rounded-full text-sm whitespace-nowrap transition-all duration-200 ${
  currentSection === index
    ? 'bg-blue-500 text-white shadow-lg shadow-blue-500/25'
    : 'bg-gray-100 dark:bg-gray-800 text-gray-600 dark:text-gray-300'
}`}
style={{ minHeight: '44px' }} // iOS touch target
```

#### Typography Line Length
```tsx
{/* EXACT character limits for readability */}
<h1 className="mb-4 text-3xl font-bold leading-tight max-w-[75ch] text-gray-900 dark:text-white">
<p className="mb-8 text-lg text-gray-600 dark:text-gray-300 max-w-[65ch] leading-relaxed">
```

### 5. Twitter Integration (`src/components/TweetEmbed.tsx`)

**EXACT COMPACT SIZING:**

#### Compact Mode Dimensions
```tsx
{/* Avatar size - EXACTLY 24px */}
<img className="w-6 h-6 rounded-full object-cover flex-shrink-0" />

{/* Image height - EXACTLY 96px */}
<img className="w-full h-24 object-cover" />

{/* Icon sizes - EXACTLY 12px */}
<MessageCircle className="w-3 h-3" />
```

#### Verification Badge SVG
```tsx
{/* EXACT verified badge path */}
<svg viewBox="0 0 24 24" className="w-3 h-3 text-blue-500" fill="currentColor">
  <path d="M22.25 12c0-1.43-.88-2.67-2.19-3.34.46-1.39.2-2.9-.81-3.91s-2.52-1.27-3.91-.81c-.66-1.31-1.91-2.19-3.34-2.19s-2.67.88-3.33 2.19c-1.4-.46-2.91-.2-3.92.81s-1.26 2.52-.8 3.91c-1.31.67-2.2 1.91-2.2 3.34s.89 2.67 2.2 3.34c-.46 1.39-.21 2.9.8 3.91s2.52 1.27 3.91.81c.67 1.31 1.91 2.19 3.34 2.19s2.68-.88 3.34-2.19c1.39.46 2.9.2 3.91-.81s1.27-2.52.81-3.91c1.31-.67 2.19-1.91 2.19-3.34zm-11.71 4.2L6.8 12.46l1.41-1.42 2.26 2.26 4.8-5.23 1.47 1.36-6.2 6.77z"/>
</svg>
```

### 6. Data Structure (`src/data/articles.ts` & `src/data/tweets.ts`)

**TOPIC DIVERSITY ALGORITHM:**
```tsx
// EXACT logic for TikTok-style topic jumping
export const getSerendipitousArticle = (currentArticle: Article | null, excludeIds: string[] = []): Article => {
  const availableArticles = Object.values(sampleArticles).filter(
    article => !excludeIds.includes(article.id)
  );

  if (currentArticle) {
    const differentCategoryArticles = availableArticles.filter(
      article => article.category !== currentArticle.category
    );
    
    if (differentCategoryArticles.length > 0) {
      return differentCategoryArticles[Math.floor(Math.random() * differentCategoryArticles.length)];
    }
  }

  return availableArticles[Math.floor(Math.random() * availableArticles.length)];
};
```

**TIMESTAMP CALCULATION:**
```tsx
// EXACT formula for last week timestamps
timestamp: new Date(Date.now() - 1 * 24 * 60 * 60 * 1000).toISOString(), // 1 day ago
timestamp: new Date(Date.now() - 3 * 24 * 60 * 60 * 1000).toISOString(), // 3 days ago
```

### 7. Gesture System (`src/hooks/useSwipeGestures.ts`)

**PHYSICS CALCULATIONS:**
```tsx
// EXACT sensitivity and threshold values
const SWIPE_THRESHOLD = 50;      // Minimum distance for swipe
const VELOCITY_THRESHOLD = 0.3;   // Minimum velocity for swipe
const MAX_SWIPE_TIME = 300;      // Maximum time for swipe gesture
```

## 🔧 Component Integration

### State Flow Diagram
```
App.tsx (main state)
    ↓
SwipeContainer (gesture handling)
    ↓
ArticleCard (content display)
    ↓
TweetEmbed (social integration)
```

### Event Handling Chain
```
Touch Start → useSwipeGestures → SwipeContainer → App → QuickActions/Search
```

## 📐 Exact Measurements

### iPhone Safe Areas
```css
/* Status bar height */
top: 44px (with safe-area-inset-top)

/* Home indicator */
bottom: 34px (with safe-area-inset-bottom)

/* Content margins */
padding: 16px (sides)
```

### Component Dimensions
```css
/* Tweet card compact mode */
height: ~120px (without image)
height: ~216px (with image)

/* Article card header */
height: ~140px (sticky header)

/* Quick actions modal */
height: ~280px (bottom sheet)
```

### Animation Timing
```css
/* TikTok-style transitions */
duration: 250ms (swipe transitions)
duration: 200ms (UI state changes)
duration: 150ms (micro-interactions)
```

## 🧪 Testing Checklist

### Gesture Testing
- [ ] Swipe up changes topic category
- [ ] Swipe down returns to previous article
- [ ] Swipe left/right finds related articles
- [ ] Long press triggers quick actions
- [ ] Pull down opens search

### Visual Testing  
- [ ] Proper iPhone safe area handling
- [ ] Smooth physics-based animations
- [ ] Twitter cards appear compact
- [ ] Category labels show correctly
- [ ] Progress bar updates on scroll

### Responsive Testing
- [ ] Works on iPhone 12 Pro (390x844)
- [ ] Works on iPhone 15 Pro Max (430x932) 
- [ ] Handles landscape orientation
- [ ] Touch targets minimum 44px

## 🚀 Deployment

### Build for Production
```bash
npm run build
```

### Deployment Platforms
- **Vercel**: `npx vercel --prod`
- **Netlify**: `npm run build && netlify deploy --prod --dir=build`
- **GitHub Pages**: Use `gh-pages` package

### PWA Optimization
Add to `public/manifest.json`:
```json
{
  "name": "WikiFlow",
  "short_name": "WikiFlow", 
  "display": "standalone",
  "orientation": "portrait",
  "theme_color": "#030213",
  "background_color": "#ffffff"
}
```

## 🎯 Learning Objectives

By completing this project, you will learn:

1. **iOS Design Patterns**: Safe areas, navigation, micro-interactions
2. **Physics-Based Animations**: Spring transitions, momentum, easing
3. **Gesture Recognition**: Touch events, swipe detection, momentum calculation
4. **State Management**: React hooks, event flow, component communication
5. **Mobile-First Design**: Responsive layouts, touch targets, performance
6. **API Integration**: Twitter embeds, external data sources
7. **TypeScript Patterns**: Interface design, type safety, component props

## 🔍 Common Issues & Solutions

### Gestures Not Working
- Check `touch-action: manipulation` in CSS
- Verify event listeners are properly attached
- Test on actual mobile device, not just desktop

### Animations Stuttering
- Use `transform` instead of changing `top/left` properties
- Enable hardware acceleration with `will-change: transform`
- Reduce animation complexity during gestures

### Safe Areas Not Applied
- Ensure `env(safe-area-inset-*)` is in inline styles
- Test on iPhone with notch/Dynamic Island
- Add `viewport-fit=cover` to meta viewport tag

### Twitter Cards Too Large
- Verify `compact={true}` prop is passed
- Check Tailwind classes match exactly
- Ensure line-clamp utilities are working

## 📚 Additional Resources

- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [Framer Motion Documentation](https://www.framer.com/motion/)
- [Tailwind CSS V4 Guide](https://tailwindcss.com/docs/v4-beta)
- [React Hook Patterns](https://react-hooks-library.vercel.app/)

## 🎨 Design Inspiration

WikiFlow draws inspiration from:
- **TikTok**: Vertical scrolling, content discovery
- **Apple News**: Typography, reading experience  
- **Twitter**: Real-time social context
- **iOS**: Native patterns, gestures, animations

## 📝 Final Notes

This project represents a production-quality mobile web application with:
- **Performance optimized** for 60fps animations
- **Accessibility compliant** with proper ARIA labels
- **SEO friendly** with semantic HTML structure
- **Type safe** with comprehensive TypeScript coverage

Take your time with each component and pay attention to the exact measurements and styling. The magic is in the details! 🪄

---

**Made with ❤️ for Wikipedia knowledge exploration**
