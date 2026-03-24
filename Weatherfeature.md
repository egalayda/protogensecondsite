# Tech Spec: Weather Widget — Homepage Footer

## Overview

Add a 5-day weather forecast for **Chicago, IL** (Fahrenheit) to the homepage footer, right-aligned alongside the existing site links and copyright notice. The widget is powered by **weatherwidget.io**, a free embeddable weather service.

## Requirements

| # | Requirement | Status |
|---|-------------|--------|
| 1 | Display a 5-day weather forecast | ✅ Implemented |
| 2 | Location set to Chicago, IL | ✅ Implemented |
| 3 | Temperature units in Fahrenheit | ✅ Implemented |
| 4 | Widget right-aligned in the footer | ✅ Implemented |
| 5 | Dark theme to match footer styling | ✅ Implemented |
| 6 | Responsive — stacks on mobile | ✅ Implemented |

## Technical Details

### Provider

- **Service:** [weatherwidget.io](https://weatherwidget.io)
- **Cost:** Free tier (no API key required)
- **Integration method:** Embed script + anchor tag

### Widget Configuration

| Parameter | Value | Purpose |
|-----------|-------|---------|
| `href` | `https://forecast7.com/en/41d88n87d63/chicago/?unit=us` | Chicago coords + Fahrenheit (`unit=us`) |
| `data-label_1` | `CHICAGO` | Primary label displayed on widget |
| `data-label_2` | `Weather` | Secondary label |
| `data-days` | `5` | Number of forecast days shown |
| `data-theme` | `dark` | Dark theme to match the `#1e293b` footer background |

### Embed Code

```html
<a class="weatherwidget-io"
   href="https://forecast7.com/en/41d88n87d63/chicago/?unit=us"
   data-label_1="CHICAGO"
   data-label_2="Weather"
   data-days="5"
   data-theme="dark">CHICAGO Weather</a>

<script>
!function(d,s,id){
  var js,fjs=d.getElementsByTagName(s)[0];
  if(!d.getElementById(id)){
    js=d.createElement(s);
    js.id=id;
    js.src='https://weatherwidget.io/js/widget.min.js';
    fjs.parentNode.insertBefore(js,fjs);
  }
}(document,'script','weatherwidget-io-js');
</script>
```

### Layout Changes

**Footer structure** changed from centered single-column to a flex row:

```
┌────────────────────────────────────────────────────┐
│  footer-grid (display: flex; justify: space-between)│
│  ┌──────────────────┐   ┌────────────────────────┐ │
│  │  footer-left      │   │  footer-weather (300px)│ │
│  │  • Nav links      │   │  weatherwidget.io      │ │
│  │  • Copyright      │   │  5-day / Chicago / °F  │ │
│  └──────────────────┘   └────────────────────────┘ │
└────────────────────────────────────────────────────┘
```

**Responsive behavior** (`max-width: 640px`): The grid stacks vertically and centers both sections.

### Files Modified

| File | Change |
|------|--------|
| `index.html` | Footer restructured into `.footer-grid` with `.footer-left` and `.footer-weather` divs; widget embed added |
| `styles.css` | New `.footer-grid`, `.footer-left`, `.footer-weather` classes; responsive breakpoint added |

### Dependencies

| Dependency | Type | Loaded From |
|------------|------|-------------|
| `widget.min.js` | External script | `https://weatherwidget.io/js/widget.min.js` |

- No npm packages required
- No API keys required
- Script loads asynchronously and only once (guarded by `id` check)

### Risks & Considerations

- **Third-party dependency:** If weatherwidget.io goes offline, the widget degrades to a plain text link ("CHICAGO Weather") pointing to forecast7.com — no layout breakage.
- **Performance:** The script is small (~5 KB) and loads asynchronously. No impact on page load or Largest Contentful Paint.
- **Privacy:** weatherwidget.io may set cookies for analytics. Review their privacy policy if GDPR compliance is required.
