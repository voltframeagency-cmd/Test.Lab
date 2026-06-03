# Swup v4.9.0 — Creative Developer Reference Guide

> Compiled from the official `swup/swup` repository type definitions, source code, and official documentation.
> Swup is a versatile, lightweight, and extensible page transition library for server-rendered websites. It animates content replacement performantly and enables smooth single-page-app-like navigation.

---

## Table of Contents

- [Installation](#installation)
- [Core Configuration — Options](#core-configuration--options)
- [Core Swup Instance API](#core-swup-instance-api)
- [The Visit Object Lifecycle](#the-visit-object-lifecycle)
- [Hooks & Lifecycle Events](#hooks--lifecycle-events)
- [Cache Manager](#cache-manager)
- [Classes Manager](#classes-manager)
- [Helpers & Utilities](#helpers--utilities)
- [Official Plugin Ecosystem](#official-plugin-ecosystem)
  - [Scroll Plugin](#scroll-plugin)
  - [Preload Plugin](#preload-plugin)
  - [Head Plugin](#head-plugin)
  - [Scripts Plugin](#scripts-plugin)
  - [Fragment Plugin](#fragment-plugin)
  - [Body Class Plugin](#body-class-plugin)
  - [JS Plugin](#js-plugin)
  - [Forms Plugin](#forms-plugin)
  - [Route Name Plugin](#route-name-plugin)
  - [Progress Bar Plugin](#progress-bar-plugin)
  - [Accessibility (A11y) Plugin](#accessibility-a11y-plugin)
- [Advanced Usage & Best Practices](#advanced-usage--best-practices)

---

## Installation

### npm / yarn / pnpm

Install the core library:

```bash
npm install swup
yarn add swup
pnpm add swup
```

### ES Module Import

```js
import Swup from 'swup';

const swup = new Swup();
```

### CDN (UMD / Global)

Include via jsdelivr or unpkg:

```html
<!-- Core Library -->
<script src="https://cdn.jsdelivr.net/npm/swup@4.9.0/dist/Swup.umd.js"></script>

<!-- Initialize -->
<script>
  const swup = new Swup();
</script>
```

### Package Exports

| Path | Format / Target |
|---|---|
| `swup` (default) | ESM: `dist/Swup.modern.js`, CJS: `dist/Swup.cjs` |
| `swup` (unpkg/umd) | UMD: `dist/Swup.umd.js` |
| `swup/types` | TypeScript definitions: `dist/types/index.d.ts` |

---

## Core Configuration — Options

Pass an options object during initialization to customize Swup's default behavior:

```js
const swup = new Swup({
  containers: ['#content'],
  animateHistoryBrowsing: true,
  cache: true
});
```

### Options Table

| Option | Type | Default | Description |
|---|---|---|---|
| `containers` | `string[]` | `['#swup']` | CSS selectors of containers to be replaced on page visits. |
| `animationSelector` | `string \| false` | `'[class*="transition-"]'` | CSS selector used to detect animation duration. Swup waits for transitions on elements matching this selector. |
| `animationScope` | `'html' \| 'containers'` | `'html'` | Defines where Swup appends transition state class names. |
| `cache` | `boolean` | `true` | Enables/disables local in-memory page caching. |
| `linkSelector` | `string` | `'a[href]'` | Selector for identifying links that trigger Swup page navigation. |
| `linkToSelf` | `'scroll' \| 'navigate'` | `'scroll'` | How Swup handles clicking a link to the current page. `'scroll'` scrolls to top/anchor, `'navigate'` reloads the page. |
| `native` | `boolean` | `false` | Enables native browser page transitions using the `View Transitions API` (if supported). |
| `requestHeaders` | `Record<string, string>` | `{'X-Requested-With': 'swup', 'Accept': '...'}` | Custom HTTP headers sent with the fetch requests. |
| `timeout` | `number` | `0` | Request timeout in milliseconds. `0` disables timeout. |
| `hooks` | `Partial<HookInitOptions>` | `{}` | Inline lifecycle hook handlers to register on startup. |
| `plugins` | `Plugin[]` | `[]` | Plugins to instantiate and mount on startup. |
| `ignoreVisit` | `(url, { el, event }) => boolean` | *(Checks `[data-no-swup]`)* | Callback to exclude specific links/interactions from Swup navigation. |
| `resolveUrl` | `(url) => string` | `(url) => url` | Callback to rewrite URLs before fetching them. |
| `skipPopStateHandling`| `(event) => boolean` | *(Checks state source)* | Callback to ignore certain browser `popstate` navigation events. |

---

## Core Swup Instance API

### Properties

- `swup.version`: `string` — The installed library version (e.g. `'4.9.0'`).
- `swup.options`: `Options` — The merged options object.
- `swup.defaults`: `Options` — The default configuration settings.
- `swup.plugins`: `Plugin[]` — Mounted plugin instances.
- `swup.visit`: `Visit` — Current active visit context details.
- `swup.cache`: `Cache` — Cache manager instance.
- `swup.hooks`: `Hooks` — Hook registry manager.
- `swup.classes`: `Classes` — Transition CSS class manager.
- `swup.location`: `Location` — Wrapper around the current URL location path.
- `swup.currentPageUrl`: `string` — *[Deprecated]* Alias for `swup.location.url`.

### Instance Methods

#### `swup.navigate(url, options?, init?)` -> `void`
Triggers Swup to navigate to a new local URL path.
- `url`: `string` (e.g. `"/about"`)
- `options`: `NavigationOptions & FetchOptions` (e.g. `{ animate: false, history: 'replace' }`)
- `init`: `Omit<VisitInitOptions, 'to'>` (e.g. `{ el: anchorElement, event: clickEvent }`)

```js
swup.navigate('/portfolio', {
  animate: true,
  history: 'push',
  cache: { read: true, write: true }
});
```

#### `swup.use(plugin)` -> `Plugin[]`
Installs and mounts a plugin instance.

```js
import SwupScrollPlugin from '@swup/scroll-plugin';
swup.use(new SwupScrollPlugin());
```

#### `swup.unuse(pluginOrName)` -> `Plugin[]`
Unmounts and uninstalls a plugin by instance reference or class name string.

```js
swup.unuse('SwupScrollPlugin');
```

#### `swup.findPlugin(pluginOrName)` -> `Plugin | undefined`
Locates an active plugin instance.

```js
const scrollPlugin = swup.findPlugin('SwupScrollPlugin');
```

#### `swup.delegateEvent(selector, type, callback, options?)` -> `DelegateEventUnsubscribe`
Registers a delegated event listener.
- Returns an object containing a `destroy()` function to remove the listener.

```js
const sub = swup.delegateEvent('button.nav', 'click', (event, target) => {
  console.log('Button clicked:', target);
});
sub.destroy(); // clean up
```

#### `swup.fetchPage(url, options?)` -> `Promise<PageData>`
Fetches raw page HTML from the server.
- Automatically handles redirects, timeouts, and error hooks.

```js
const page = await swup.fetchPage('/blog');
console.log(page.html);
```

#### `swup.destroy()` -> `Promise<void>`
Disables the Swup instance, removing all event listeners, emptying the cache, and unmounting plugins.

```js
await swup.destroy();
```

#### `swup.shouldIgnoreVisit(href, options?)` -> `boolean`
Checks if a given path or link element should be ignored by Swup logic.

#### `swup.getAnchorElement(hash)` -> `Element | null`
Returns the DOM element matching a specific hash anchor.

---

## The Visit Object Lifecycle

Each page transition in Swup creates a transient `Visit` instance. This object tracks the state, custom configuration overrides, metadata, and options for the current transition.

```js
swup.hooks.before('visit:start', (visit) => {
  // Override settings per visit
  visit.animation.animate = false;
  visit.scroll.reset = false;
  visit.meta.customData = 'hello';
});
```

### Visit Class Property Structure

| Component | Property | Type | Description |
|---|---|---|---|
| **Identity** | `id` | `number` | Unique random identifier for the transition. |
| | `state` | `VisitState` | Current enum progress of the transition. |
| | `meta` | `Record<string, any>` | Custom user-defined metadata object. |
| **Containers** | `containers` | `string[]` | Target containers to replace during this visit. |
| **From (Source)** | `from.url` | `string` | Originating page URL path. |
| | `from.hash` | `string` | Originating page URL anchor hash. |
| **To (Target)** | `to.url` | `string` | Target page URL path. |
| | `to.hash` | `string` | Target page URL anchor hash. |
| | `to.html` | `string` | Raw HTML content received from the server. |
| | `to.document` | `Document` | Parsed document of the target page (available once loaded). |
| **Animation** | `animation.animate` | `boolean` | Whether this transition cycle is animated. |
| | `animation.wait` | `boolean` | If true, waits for page content to load before animating out. |
| | `animation.name` | `string` | Custom animation identifier (adds `to-[name]` class). |
| | `animation.native` | `boolean` | If true, uses the browser's View Transitions API. |
| | `animation.scope` | `string[] \| 'html' \| 'containers'` | Elements where transition CSS classes are appended. |
| | `animation.selector` | `string \| false` | Selector used to detect CSS transitions. |
| **Trigger** | `trigger.el` | `Element` | The DOM element that triggered the visit. |
| | `trigger.event` | `Event` | The event (e.g. click, popstate) that triggered the visit. |
| **Cache** | `cache.read` | `boolean` | If true, Swup will attempt to read from cache. |
| | `cache.write` | `boolean` | If true, Swup will cache the fetched response. |
| **History** | `history.action` | `'push' \| 'replace'` | History record updates. |
| | `history.popstate` | `boolean` | True if triggered by browser back/forward buttons. |
| | `history.direction` | `'forwards' \| 'backwards' \| undefined` | Traveling direction in popstate navigation. |
| **Scroll** | `scroll.reset` | `boolean` | If true, resets scroll positions to top or target. |
| | `scroll.target` | `string \| false` | Custom selector/anchor to scroll to on render. |

### Visit States (VisitState)

Transitions proceed through the following states in sequence:
- `1` (CREATED)
- `2` (QUEUED)
- `3` (STARTED)
- `4` (LEAVING)
- `5` (LOADED)
- `6` (ENTERING)
- `7` (COMPLETED)
- `8` (ABORTED)
- `9` (FAILED)
- `10` (IGNORED)

---

## Hooks & Lifecycle Events

Swup uses an advanced hooks registry to let developers run custom logic at precise moments of the navigation cycle.

```js
// Register hook listener
swup.hooks.on('content:replace', (visit, args) => {
  console.log('Replaced content of:', visit.to.url);
});
```

### Hook Register Methods

- `swup.hooks.on(hookName, handler, options?)` — Listens for a hook.
- `swup.hooks.before(hookName, handler, options?)` — Shortcut to run before the default handler.
- `swup.hooks.replace(hookName, defaultHandlerReplacer, options?)` — Replaces the default internal handler.
- `swup.hooks.once(hookName, handler, options?)` — Executes once and unregisters.
- `swup.hooks.off(hookName, handler?)` — Unregisters a specific handler, or clears all if omitted.

### Options Parameters

```ts
type HookOptions = {
  once?: boolean;      // Run once and remove
  before?: boolean;    // Run before default handler
  priority?: number;   // Execution order (lower = runs first, default: 0)
  replace?: boolean;   // Replace internal default behavior
}
```

### Hook Event Definitions

| Hook Name | Arguments Passed (`args`) | Default Action / Details |
|---|---|---|
| `visit:start` | `undefined` | Fired when transition cycle initiates. |
| `visit:transition` | `undefined` | Manages animation sequences and content rendering. |
| `visit:abort` | `undefined` | Fired when a running visit is cancelled. |
| `visit:end` | `undefined` | Fired when the entire transition completes. |
| `page:load` | `{ page?: PageData, cache?: boolean, options: FetchOptions }` | Loads page HTML (resolves via cache or fetch). |
| `page:view` | `{ url: string, title: string }` | Dispatched when a page is visible to user. |
| `fetch:request` | `{ url: string, options: FetchOptions }` | Performs the network request. Returns `Promise<Response>`. |
| `fetch:error` | `{ url: string, status: number, response: Response }` | Dispatched if a page request returns HTTP 500. |
| `fetch:timeout` | `{ url: string }` | Dispatched when a request times out. |
| `animation:out:start` | `undefined` | Fired as leaving animation begins. |
| `animation:out:await` | `{ skip: boolean }` | Awaits completion of CSS transition/animation. |
| `animation:out:end` | `undefined` | Fired when leaving animation completes. |
| `animation:in:start` | `undefined` | Fired as entering animation begins. |
| `animation:in:await` | `{ skip: boolean }` | Awaits completion of entering CSS transition. |
| `animation:in:end` | `undefined` | Fired when entering animation completes. |
| `animation:skip` | `undefined` | Fired when animations are skipped (e.g. `animate: false`). |
| `content:replace` | `{ page: PageData }` | Replaces containers, updates document title & focus. |
| `content:scroll` | `undefined` | Handles scrolling to top or specific target anchor. |
| `scroll:top` | `{ options: ScrollIntoViewOptions }` | Scrolls window viewport to the top of the page. |
| `scroll:anchor` | `{ hash: string, options: ScrollIntoViewOptions }` | Scrolls viewport to target anchor element. |
| `cache:set` | `{ page: PageData }` | Fired when a page record is stored in cache. |
| `cache:clear` | `undefined` | Fired when the cache is emptied. |
| `link:click` | `{ el: HTMLAnchorElement, event: Event }` | Dispatched when user clicks a Swup-enabled link. |
| `link:self` | `undefined` | Fired when user clicks a link to the current page. |
| `link:anchor` | `{ hash: string }` | Fired on clicking link targeting an anchor. |
| `link:newtab` | `{ href: string }` | Fired on cmd/ctrl clicks (normal browser new-tab). |
| `history:popstate` | `{ event: PopStateEvent }` | Fired on popstate back/forward navigation. |
| `enable` | `undefined` | Fired when Swup initializes/enables. |
| `disable` | `undefined` | Fired when Swup is destroyed. |

### Hook Modifiers (Dot Syntax)

You can attach modifiers directly to hook names when registering inline via options:

```js
const swup = new Swup({
  hooks: {
    'visit:start.once': (visit) => console.log('First visit!'),
    'content:replace.before': (visit) => console.log('About to replace...'),
    'page:load.replace': (visit, args, defaultHandler) => { /* custom fetch */ }
  }
});
```

### Custom DOM Events

Swup also dispatches native bubbles-enabled events on the `document` object, prefixed with `swup:`. These are useful for vanilla scripts outside of Swup module scope:

- `swup:any` — Dispatched for every single hook event.
- `swup:[hookName]` — e.g. `swup:page:view`, `swup:visit:start`

```js
document.addEventListener('swup:page:view', (event) => {
  const { url, title } = event.detail.args;
  console.log(`User is looking at ${title} (${url})`);
});
```

---

## Cache Manager

Managed by the `swup.cache` instance.

### Properties

- `swup.cache.size`: `number` — Total number of pages cached in-memory.
- `swup.cache.all`: `Map<string, CacheData>` — Shallow copy of the entire cache dictionary.

### Methods

- `swup.cache.has(url)` -> `boolean` — Checks if a page cache exists.
- `swup.cache.get(url)` -> `CacheData | undefined` — Retrieves a page record.
- `swup.cache.set(url, pageData)` -> `void` — Stores page data in cache.
- `swup.cache.update(url, payload)` -> `void` — Modifies/extends cached page data attributes.
- `swup.cache.delete(url)` -> `void` — Removes a specific URL from the cache.
- `swup.cache.clear()` -> `void` — Empties the cache.
- `swup.cache.prune(predicate)` -> `void` — Prunes entries matching a custom filter.

```js
// Prune cache: remove blog pages only
swup.cache.prune((url, page) => url.startsWith('/blog/'));
```

---

## Classes Manager

Managed by the `swup.classes` instance. Controls classes appended to the target elements during page transitions.

### Swup Transition Classes

| Class Name | Applied During | Element Scope |
|---|---|---|
| `html.swup-enabled` | Permanently while Swup is active. | `<html>` |
| `html.swup-native` | Permanently if `native: true` is active. | `<html>` |
| `is-changing` | Throughout the entire transition lifecycle. | Scope (`<html>` or container) |
| `is-leaving` | While animating out / fetching new page. | Scope (`<html>` or container) |
| `is-rendering` | While content is being swapped in the DOM. | Scope (`<html>` or container) |
| `is-animating` | While animating in the new page. | Scope (`<html>` or container) |
| `is-popstate` | If transition was triggered by popstate navigation. | Scope (`<html>` or container) |
| `to-[name]` | Custom transition animations based on trigger. | Scope (`<html>` or container) |

### Methods

- `swup.classes.add(...classNames)` -> `void`
- `swup.classes.remove(...classNames)` -> `void`
- `swup.classes.clear()` -> `void` — Clears all swup-prefixed transition classes.

---

## Helpers & Utilities

Import utilities directly from the swup bundle:

```js
import { classify, Location, matchPath, delegateEvent } from 'swup';
```

### `classify(text, fallback?)` -> `string`
Converts arbitrary text (like URL paths) into CSS-safe class name slugs.

```js
classify('/about-us/team_member'); // 'about-us-team-member'
```

### `Location`
A wrapper around the native browser `URL` class.
- `Location.fromElement(anchorElement)` -> `Location`
- `Location.fromUrl(urlString)` -> `Location`
- `.url`: `string` — Returns the combined pathname and query parameter suffix (e.g. `"/blog?page=2"`).

### `matchPath(pattern, options?)` -> `MatchFunction`
Wrapper around `path-to-regexp` to create path matches for custom transition logic.

```js
const isProjectPage = matchPath('/portfolio/:id');
const match = isProjectPage(window.location.pathname);
if (match) console.log('Project ID:', match.params.id);
```

### `delegateEvent`
delegates target matching for event handling (using `delegate-it`).

---

## Official Plugin Ecosystem

### Scroll Plugin

#### Installation & Initialization
```bash
npm install @swup/scroll-plugin
```
```js
import SwupScrollPlugin from '@swup/scroll-plugin';
const swup = new Swup({
  plugins: [new SwupScrollPlugin({ /* options */ })]
});
```

#### Options Table

| Option | Type | Default | Description |
|---|---|---|---|
| `doScrollingRightAway` | `boolean` | `false` | Scroll down to the anchor right away, before entering animation finishes. |
| `animateScroll` | `boolean \| object` | `true` | Enables smooth scroll animations. Can pass options for scroll speed/easing. |
| `scrollFriction` | `number` | `0.3` | Animation scrolling speed friction coef. |
| `scrollAcceleration` | `number` | `0.08` | Animation acceleration coef. |
| `scrollContainers` | `string` | `'[data-swup-scroll-container]'` | Selectors for elements containing custom scroll behaviors. |
| `shouldResetScrollPosition` | `() => boolean` | `() => true` | Custom filter check to block scroll resetting. |
| `scrollFunction` | `(x, y, options) => void` | *(window.scrollTo)* | Replace scroll function with custom implementation (e.g. GSAP). |

---

### Preload Plugin

#### Installation & Initialization
```bash
npm install @swup/preload-plugin
```
```js
import SwupPreloadPlugin from '@swup/preload-plugin';
const swup = new Swup({
  plugins: [new SwupPreloadPlugin({ /* options */ })]
});
```

#### Options Table

| Option | Type | Default | Description |
|---|---|---|---|
| `throttle` | `number` | `5` | Maximum number of concurrent preload requests. |
| `preloadHoveredLinks` | `boolean` | `true` | Preloads page when user hovers cursor over a link. |
| `preloadVisibleLinks` | `boolean` | `false` | Preloads page when links scroll into viewport (uses `IntersectionObserver`). |
| `preloadInitialPage` | `boolean` | `true` | Preloads pages referenced in initial HTML links. |

#### Attributes Setup
Configure preloading directly in HTML:
- `<a href="/about" data-swup-preload>Link</a>` — Explicit preload trigger.
- `<div data-swup-preload-all><a href="..."></a></div>` — Preload all links in container.

---

### Head Plugin

Updates metadata in `<head>` (e.g., stylesheets, meta tags, titles).

#### Installation & Initialization
```bash
npm install @swup/head-plugin
```
```js
import SwupHeadPlugin from '@swup/head-plugin';
const swup = new Swup({
  plugins: [new SwupHeadPlugin({ /* options */ })]
});
```

#### Options Table

| Option | Type | Default | Description |
|---|---|---|---|
| `persistTags` | `string \| (tag) => boolean` | `'link[rel=stylesheet]'` | Tags that will remain in head, not overwritten. |
| `persistAssets` | `boolean` | `false` | Set true to keep orphaned styles and scripts instead of destroying. |
| `awaitAssets` | `boolean` | `false` | Waits for newly injected stylesheets to load before displaying page. |

---

### Scripts Plugin

Re-evaluates `<script>` tags in body/head when a new page load completes.

#### Installation & Initialization
```bash
npm install @swup/scripts-plugin
```
```js
import SwupScriptsPlugin from '@swup/scripts-plugin';
const swup = new Swup({
  plugins: [new SwupScriptsPlugin({ /* options */ })]
});
```

#### Options Table

| Option | Type | Default | Description |
|---|---|---|---|
| `head` | `boolean` | `true` | Re-run script tags found inside `<head>`. |
| `body` | `boolean` | `true` | Re-run script tags found inside `<body>`. |
| `optin` | `boolean` | `false` | Only re-run scripts with `[data-swup-reload-script]` attribute. |

#### Attributes Setup
- `<script data-swup-ignore-script>...</script>` — Do not re-run this script.

---

### Fragment Plugin

Dynamically updates sub-sections (fragments) of a page depending on matching rules.

#### Installation & Initialization
```bash
npm install @swup/fragment-plugin
```
```js
import SwupFragmentPlugin from '@swup/fragment-plugin';
const swup = new Swup({
  plugins: [new SwupFragmentPlugin({ rules: [...] })]
});
```

#### Rules Configuration
```js
new SwupFragmentPlugin({
  rules: [
    {
      from: '/blog/:id',
      to: '/blog',
      containers: ['#comments-count', '#ratings']
    }
  ]
});
```

---

### Body Class Plugin

Updates `<body>` element class attributes with each page transition.

#### Installation & Initialization
```bash
npm install @swup/body-class-plugin
```
```js
import SwupBodyClassPlugin from '@swup/body-class-plugin';
const swup = new Swup({
  plugins: [new SwupBodyClassPlugin()]
});
```

---

### JS Plugin

Allows page transitions using JavaScript (e.g. GSAP, Anime.js) instead of CSS classes.

#### Installation & Initialization
```bash
npm install @swup/js-plugin
```
```js
import SwupJsPlugin from '@swup/js-plugin';
const swup = new Swup({
  plugins: [new SwupJsPlugin(animationsArray)]
});
```

#### Configuration Example
```js
const animationsArray = [
  {
    from: '(.*)',
    to: '(.*)',
    out: (next) => gsap.to('#swup', { opacity: 0, onComplete: next }),
    in: (next) => gsap.to('#swup', { opacity: 1, onComplete: next })
  }
];
```

---

### Forms Plugin

Enables AJAX-based form submissions through Swup transitions.

#### Installation & Initialization
```bash
npm install @swup/forms-plugin
```
```js
import SwupFormsPlugin from '@swup/forms-plugin';
const swup = new Swup({
  plugins: [new SwupFormsPlugin({ /* options */ })]
});
```

---

### Route Name Plugin

Adds custom class helpers corresponding to specific routes for targeted animation setups.

#### Installation & Initialization
```bash
npm install @swup/route-name-plugin
```
```js
import SwupRouteNamePlugin from '@swup/route-name-plugin';
const swup = new Swup({
  plugins: [new SwupRouteNamePlugin({
    routes: [
      { name: 'home', path: '/' },
      { name: 'project', path: '/portfolio/:id' }
    ]
  })]
});
// Appends 'to-home', 'to-project' class rules to scope elements
```

---

### Progress Bar Plugin

Shows a custom progress indicator at the top of the page during slow requests.

#### Installation & Initialization
```bash
npm install @swup/progress-bar-plugin
```
```js
import SwupProgressBarPlugin from '@swup/progress-bar-plugin';
const swup = new Swup({
  plugins: [new SwupProgressBarPlugin({ /* options */ })]
});
```

---

### Accessibility (A11y) Plugin

Enhances accessibility for screen readers on AJAX content loads.

#### Installation & Initialization
```bash
npm install @swup/a11y-plugin
```
```js
import SwupA11yPlugin from '@swup/a11y-plugin';
const swup = new Swup({
  plugins: [new SwupA11yPlugin()]
});
```

---

## Advanced Usage & Best Practices

### CSS Page Transitions

When using Swup default animation options, configure your stylesheet transitions to watch for the classes managed by the Classes manager:

```css
/* Define base transition properties */
#swup {
  transition: opacity 0.4s ease, transform 0.4s ease;
  opacity: 1;
  transform: translateY(0);
}

/* State: page leaving */
html.is-changing #swup {
  opacity: 0;
  transform: translateY(20px);
}

/* State: page entering */
html.is-rendering #swup {
  opacity: 0;
  transform: translateY(-20px);
}
```

### GSAP Integration (with JS Plugin)

```js
import Swup from 'swup';
import SwupJsPlugin from '@swup/js-plugin';
import gsap from 'gsap';

const swup = new Swup({
  plugins: [
    new SwupJsPlugin([
      {
        from: '(.*)',
        to: '(.*)',
        out: (next) => {
          gsap.to('.content-area', {
            opacity: 0,
            y: 30,
            duration: 0.5,
            onComplete: next
          });
        },
        in: (next) => {
          gsap.fromTo('.content-area',
            { opacity: 0, y: -30 },
            { opacity: 1, y: 0, duration: 0.5, onComplete: next }
          );
        }
      }
    ])
  ]
});
```

### Custom Page Transition Actions

Override options context dynamically per hook:

```js
// Skip animation if navigating back to home
swup.hooks.before('visit:start', (visit) => {
  if (visit.to.url === '/') {
    visit.animation.animate = false;
  }
});
```

### Custom Plugin Boilerplate

Create custom plugins by conforming to the `Plugin` object specification:

```js
const myCustomPlugin = {
  name: 'MyCustomPlugin',
  isSwupPlugin: true,
  
  mount() {
    this.swup.hooks.on('page:view', this.onPageView);
  },
  
  unmount() {
    this.swup.hooks.off('page:view', this.onPageView);
  },
  
  onPageView(visit) {
    console.log('Plugin noticed view change to:', visit.to.url);
  }
};

swup.use(myCustomPlugin);
```
