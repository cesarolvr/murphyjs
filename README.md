# MurphyJS

<div align="center">
  
  <img src="logo.png" alt="MurphyJS Logo" width="600"/>
  <br/><br/>
  <img src="https://img.shields.io/npm/v/murphyjs.svg" alt="npm version" />
  <!-- <img src="https://img.shields.io/bundlephobia/minzip/murphyjs" alt="bundle size" /> -->
  <img src="https://img.shields.io/npm/dm/murphyjs" alt="npm downloads" />
  <img src="https://img.shields.io/github/license/cesarolvr/murphyjs" alt="license" />
  <br />
  <br />

  A lightweight JavaScript library for creating smooth animations with a simple API.
</div>


## Features

- 🚀 Lightweight and fast (only 3.7KB)
- 🎨 Simple and intuitive API
- 🌈 Beautiful animations
- 📱 Mobile-friendly
- 🎯 No dependencies
- 🎮 Total control of IntersectionObserver parameters
- 🎁 Some animations implemented by default
- 🏝 Plug and play solution to landing pages and simple projects
- ❎ Native fallback to not supported browsers
- 🛎️ Built-in event system for animation lifecycle (in, out, finish, cancel, reset, cleanup)
- 🔄 Mirror animations for smooth scroll transitions
- 📏 Viewport position control with predefined aliases
- 📱 Mobile optimization with device detection

## Installation

Using npm:
```bash
npm install murphyjs
```

Using yarn:
```bash
yarn add murphyjs
```

For detailed documentation and examples, visit our [documentation site](https://www.murphyjs.org/).

## Quick Start

```javascript
import { Murphy } from 'murphyjs';

// Create a new instance
const murphy = new Murphy();

// Animate elements
murphy.animate('.box', {
  opacity: [0, 1],
  y: [20, 0],
  duration: 1000
});
```

## Usage

### 1. Tag your HTML

In your markup, decorate your element with attribute `data-murphy`:

```html
<div data-murphy="left-to-right">Any content here</div>
```

The default effect of murphy is `bottom-to-top`, but you can also use:
- `top-to-bottom`
- `left-to-right`
- `right-to-left`

### 2. Reset your CSS

In your CSS, reset all the tagged elements:

```css
*[data-murphy] {
  opacity: 0;
}
```

### 3. Start murphy

In JavaScript, import and run `play` when your page is completely loaded:

```javascript
import murphy from "murphyjs";
murphy.play();
```

Or if you're using the script tag:

```html
<script src="./murphy/index.js"></script>
<script>
  murphy.play();
</script>
```

## Configuration

You can configure the animation of each decorated element individually using these attributes:

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| data-murphy | String | 'bottom-to-top' | Animation direction |
| data-murphy-appearance-distance | Int | 50px | Distance from viewport edge to trigger animation |
| data-murphy-element-distance | Int | 30px | Distance the element moves during animation |
| data-murphy-ease | String | 'ease' | Animation easing function (can be a cubic-bezier) |
| data-murphy-animation-delay | Int | 300ms | Delay before animation starts |
| data-murphy-element-threshold | Float | 1.0 | How much of the element needs to be visible to trigger (0-1) |
| data-murphy-animation-duration | Int | 300ms | Duration of the animation |
| data-murphy-root-margin | String | '0px 0px -50px 0px' | Custom root margin for the Intersection Observer. Use this to control when animations trigger based on viewport position. You can use predefined aliases: 'top', 'middle', 'bottom', 'quarter', 'three-quarters' |
| data-murphy-group | String | undefined | Group identifier for controlling animations for specific groups of elements |
| data-murphy-mirror | Boolean | false | Whether to play the animation in reverse when the element leaves the viewport |
| data-murphy-disable-mobile | Boolean | false | Whether to disable animations on mobile devices (screen width <= 768px or mobile user agent) |

## Advanced Features

### Mirror Animations

Enable mirror animations to create smooth transitions when elements leave the viewport:

```html
<div data-murphy="bottom-to-top" data-murphy-mirror="true">
  This element will animate in when scrolling down and animate out when scrolling up
</div>
```

### Viewport Position Control

Control when animations trigger based on the element's position in the viewport using the `data-murphy-root-margin` attribute. For convenience, we provide several aliases:

```html
<!-- Animate when element reaches middle of viewport -->
<div data-murphy="bottom-to-top" data-murphy-root-margin="middle">
  This will animate when it reaches the middle of the viewport
</div>

<!-- Animate when element reaches bottom of viewport -->
<div data-murphy="bottom-to-top" data-murphy-root-margin="bottom">
  This will animate when it reaches the bottom of the viewport
</div>

<!-- Animate when element is 25% from bottom of viewport -->
<div data-murphy="bottom-to-top" data-murphy-root-margin="quarter">
  This will animate when it's 25% from the bottom of the viewport
</div>

<!-- Animate when element is 75% from bottom of viewport -->
<div data-murphy="bottom-to-top" data-murphy-root-margin="three-quarters">
  This will animate when it's 75% from the bottom of the viewport
</div>
```

You can also use raw CSS margin values if you need more precise control:

```html
<div data-murphy="bottom-to-top" data-murphy-root-margin="0px 0px -50% 0px">
  This will animate when it reaches the middle of the viewport
</div>
```

The root margin follows the CSS margin syntax: `top right bottom left`. Negative values create an inset margin, which means the animation will trigger when the element reaches that point in the viewport.

### Available Viewport Position Aliases

| Alias | Description | Raw Value |
|-------|-------------|-----------|
| `top` | Triggers at top of viewport | `'0px 0px 0px 0px'` |
| `middle` | Triggers at middle of viewport | `'0px 0px -50% 0px'` |
| `bottom` | Triggers at bottom of viewport | `'0px 0px 0px 0px'` |
| `quarter` | Triggers at 25% from bottom | `'0px 0px -25% 0px'` |
| `three-quarters` | Triggers at 75% from bottom | `'0px 0px -75% 0px'` |

## Group-based Animations

You can group elements using the `data-murphy-group` attribute. This allows you to control animations for specific groups of elements. For example, you can play or reset animations for only a subset of elements by specifying a group name:

```html
<div data-murphy="bottom-to-top" data-murphy-group="group1">Group 1</div>
<div data-murphy="top-to-bottom" data-murphy-group="group1">Group 1</div>
<div data-murphy="left-to-right" data-murphy-group="group2">Group 2</div>
<div data-murphy="right-to-left" data-murphy-group="group2">Group 2</div>
```

You can then control animations for a specific group using the API:

```js
// Play animations for group1 only
murphy.play('group1');

// Reset animations for group2 only
murphy.reset('group2');
```

## API

### Global Methods

| Method | Description |
|--------|-------------|
| `play(group?: string)` | Start monitoring elements in DOM tagged with `data-murphy` attribute. Optionally specify a group to animate only elements in that group. |
| `cancel()` | Cancel all animations and reset elements to their final state. |
| `reset(group?: string)` | Reset all animations to their initial state. Optionally specify a group to reset only elements in that group. |
| `cleanup()` | Disconnect all Intersection Observers and clean up resources. |

### Murphy Class

The `Murphy` class provides a programmatic way to create animations:

```javascript
import { Murphy } from 'murphyjs';

// Create a new instance
const murphy = new Murphy();

// Animate elements
murphy.animate('.box', {
  opacity: [0, 1],
  y: [20, 0],
  duration: 1000
});
```

#### `animate(selector, options)`

Animates elements matching the selector with the specified options.

##### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `selector` | String | CSS selector for target elements |
| `options` | Object | Animation configuration |

##### Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `opacity` | Array | [0, 1] | Start and end opacity values |
| `x` | Array | [0, 0] | Start and end x translation in pixels |
| `y` | Array | [0, 0] | Start and end y translation in pixels |
| `duration` | Number | 1000 | Animation duration in milliseconds |
| `delay` | Number | 0 | Delay before animation starts in milliseconds |
| `ease` | String | 'ease' | Easing function name |

### Events

MurphyJS provides a set of events that you can listen to for better control and integration:

| Event | Description |
|-------|-------------|
| `murphy:in` | Fired when an element enters the viewport |
| `murphy:out` | Fired when an element leaves the viewport |
| `murphy:finish` | Fired when an animation completes |
| `murphy:cancel` | Fired when an animation is cancelled |
| `murphy:reset` | Fired when an element is reset |
| `murphy:cleanup` | Fired when observers are cleaned up |

#### Event Example

```javascript
document.addEventListener('murphy:in', (event) => {
  const { element } = event.detail;
  console.log('Element entered viewport:', element);
});

document.addEventListener('murphy:finish', (event) => {
  const { element } = event.detail;
  console.log('Animation finished:', element);
});
```

### Available Animations

MurphyJS comes with several built-in animations that you can use with the `data-murphy` attribute:

#### Basic Animations
- `bottom-to-top`
- `top-to-bottom`
- `left-to-right`
- `right-to-left`

#### Flip Animations
- `flip-left`
- `flip-right`
- `flip-up`
- `flip-down`

#### Zoom Animations
- `zoom-in`
- `zoom-out`

#### Fade Animations
- `fade`
- `fade-up`
- `fade-down`
- `fade-left`
- `fade-right`

#### Rotate Animations
- `rotate-left`
- `rotate-right`

#### Scale Animations
- `scale-up`
- `scale-down`

#### Slide Animations
- `slide-up`
- `slide-down`
- `slide-left`
- `slide-right`

#### Bounce Animations
- `bounce-in`
- `bounce-out`