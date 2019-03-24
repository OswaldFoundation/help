# Apps and modes

Using the Agastya API, you can enable and disable apps and modes:

```js
agastya.api(ACTION, slug);
```

## Toggling modes

To toggle modes, you can use `enableMode`, `disableMode`, or `toggleMode`. For example, you can toggle modes like this:

```js
agastya.api("enableMode", "dyslexia"); // Enable dyslexia-friendly mode
agastya.api("disableMode", "large-font"); // Disable large font
agastya.api("toggleMode", "read-aloud"); // Toggle read aloud
```

Modes that don't require additional configuration can be toggled:

| Mode | Slug |
| ---- | ---- |
| Dyslexia-friendly mode | `dyslexia` |
| Blue light filter | `blue-light-filter` |
| Night mode | `night` |
| Large font | `large-font` |
| Legible fonts | `legible-fonts` |
| Keyboard navigation | `keyboard-navigation` |
| Highlight links | `highlight-links` |
| Big Cursor | `big-cursor` |
| Desaturate | `desaturate` |
| Reading mode | `reading-mode` |
| Read aloud | `read-aloud` |

## Configurable modes

Some apps and modes can't simply be enabled or disabled, they need additional options.

### Translation

For translation, you need to provide the language code:

```js
agastya.api("translate", "fr"); // Translate to French
```

To change the language back to the default, send the language code:

```js
agastya.api("translate", "en"); // Go back to English
```

If you don't know the page's language, you can use the [Global details](tracking.html#global-details) API:

```js
const details = agastya.getDetails();
const language = details.language;
agastya.api("translate", language); // Go back to default language
```

### Contrast

Similarly, you need to provide the choice when changing the contrast settings:

```js
agastya.api("contrast", "highContrast"); // Change to high contrast
```

And supply `none` to go back to the default:

```js
agastya.api("contrast", "none"); // Revert contrast settings
```

| Contrast option | Slug |
| --------------- | ---- |
| None | `none` |
| High contrast | `highContrast` |
| Grayscale | `grayscale` |
| Invert | `invert` |
| Invert grayscale | `invertGrayscale` |
| Yellow on black | `yellowBlack` |
| Cyan on black | `cyanBlack` |
| Green on black | `greenBlack` |

## Customizations

Finally, there are also customizations available for individual properties:

```js
agastya.api("customization", { lineHeight: 2 }); // Change line height to 2
```

There can also be multiple key-value pairs:

```js
// Change line height to 2
// Also change font size to 120%
agastya.api("customization", { lineHeight: 2, fontSize: 120 });
```

To remove a previously set customization, use `reset`:

```js
agastya.api("reset", "lineHeight"); // Reset line height customization
```

| Customization | Slug |
| ------------- | ---- |
| Font family | `fontFamily` |
| Font size | `fontSize` |
| Letter spacing | `letterSpacing` |
| Line height | `lineHeight` |
| Word spacing | `wordSpacing` |
| Background color | `backgroundColor` |
| Text color | `color` |
| Border color | `borderColor` |
| CSS filter | `filter` |

## URL Params

Default modes can also be enabled in the URL params of a webpage using `agastyaInit`. For example, the following starts dyslexia-friendly mode on load:

```
https://example.com?agastyaInit=dyslexia
```

Multiple modes can be comma separated:

```
https://example.com?agastyaInit=dyslexia,large-font
```

Modes with customizations may also be added, key-value pairs are comma separated:

```
https://example.com?agastyaInit=dyslexia,fontSize:110,lineHeight:1.4
```