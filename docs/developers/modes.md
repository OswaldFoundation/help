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

### Read Aloud

Agastya's Read Aloud mode finds the most important content on your webpage and reads it. You can add a `data-agastya-read-aloud` attribute to a custom element to read that instead, just make sure that is the dominant content of your webpage:

```html
<main id="content" data-agastya-read-aloud>
	...
</main>
```

You can also use the [Read Aloud Text API](#read-aloud-text).

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
agastya.api("customize", { lineHeight: 2 }); // Change line height to 2
```

There can also be multiple key-value pairs:

```js
// Change line height to 2
// Also change font size to 120%
agastya.api("customize", { lineHeight: 2, fontSize: 120 });
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

### Font family

A special case applies when selecting a reading font, since custom web fonts need to be loaded from a CDN. In this case, the value looks like this:

```
openDyslexic__FILE__https://static.oswaldlabs.com/open-dyslexic.css
```

Here, the name of the font is `openDyslexic` and the stylesheet URL is `https://static.oswaldlabs.com/open-dyslexic.css`. Note that this is not the URL of the web font itself, but a CSS stylesheet which loads the font using `@font-face`.

So, setting Noto Sans (loaded via Google Fonts) as the webpage font can be done like this:

```js
// Change font to Noto Sans
agastya.api("customization", { fontFamily: "Noto Sans__FILE__https://fonts.googleapis.com/css?family=Noto+Sans:400,400i,700,700i" });
```

Default system fonts have been proxied to the following:

| Font family | Slug |
| ----------- | ---- |
| System font stack | `systemDefault` |
| Arial, sans-serif | `arialOrSansSerif` |
| Times New Roman, serif | `timesNewRomanOrSerif` |
| Courier, monospaced | `courierOrMonospaced` |

To set the system font stack, for example, you can do this:

```js
// Use system font stack
agastya.api("customization", { fontFamily: "systemDefault" });
```

## Ignoring elements

Sometimes, you don't want Agastya CSS to overwrite a specific element. For this, you can use the `data-agastya-ignore` attribute:

```html
<span data-agastya-ignore>Ignore me!</span>
```

Changing the background color, for example, will not do anything for the `span` element.

To ignore an element and all its children, you can use `data-agastya-ignore-deep` instead:

```html
<div data-agastya-ignore-deep>
	<span>Hello</span>
	<span>World</span>
</div>
```

In the above example, both `span`s and the `div` itself will be ignores.

## URL params

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

## Agastya Services

There are also some Agastya Services, powered by Oswald Labs Platform, which are made available to our users.

### Read Aloud Text

Agastya's exclusive Read Aloud mode automatically finds the important content on your webpage and reads it aloud. However, there may be times when you want to read custom text. For this case, you can use Real Aloud Text.

The API generates the URL to an MP3 file which can be embedded or used with the HTML5 `audio` tag:

```js
const url = agastya.api("read-aloud-link", { text: "Hello, world! How are you?" });
console.log(url); // https://developer.oswaldlabs.com/read-aloud...
```

Even though the generated URL is simple enough to hard-code, it is **highly recommended** to use the API to generate this URL, since this URL will change in the future and may include additional parameters, and the API will always remain up-to-date.
