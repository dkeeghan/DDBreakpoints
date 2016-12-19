![Deloitte Digital](docs/deloittedigital-logo-white.png)

# DDBreakpoints for LESS
Breakpoints LESS Mixin and JavaScript library, used to accelerate and simplify media query development during the development process of responsive pages.

The LESS and JS also allow for the ability to create static (non-responsive) stylesheets as well by setting a variable allowing backwards support for non responsive browsers (like IE8) easily.

*This is a port of the official SCSS version found at [DDBreakpoints by DeloitteDigitalAPAC]https://github.com/DeloitteDigitalAPAC/DDBreakpoints*

## Getting Started

### LESS

Import the LESS into your own project

```less
@import 'dd-breakpoints.less';
```

#### Usage

At the most basic level, everything comes from a single mixin:

```less
.bp(@min, {
    // your styles here
}}
```

or

```less
.bp(@min, @max=0, {
    // your styles here
}}
```

The recommended usage for the mixin is to go mobile first:

```less
.module {
    // base styles

    .bp(m, {
		// medium styles
	});

    .bp(l, {
        // large styles
    });

    .bp(xl, {
        // extra large styles
        // not included in the static sheet
    });
}
```

But if you have to, you can go large first too:

```less
.module {
    // desktop styles

    .bp(0, l, {
        // large and below styles
    });

    .bp(0, m, {
        // medium and below styles
        // not included in the static sheet
    });

    .bp(0, s, {
        // small and below styles
        // not included in the static sheet
    });
}
```

You can even use pixel based widths mixed with breakpoint names.

```less
.module {
    // base styles

    .bp(300, m, {
        // between 300px (in ems) and medium breakpoint
        // not included in the static sheet
    });

    .bp(m, 2000, {
        // between medium breakpoint and 2000px (in ems)
    });

    .bp(200, 250, {
        // be as specific as you need
        // not included in the static sheet
    });
}
```

And you can also check against heights too

```less
.module {
    // base styles

    bph(0, 500, {
        // between 0 and 500px high
        // height breakpoints are never included in the static sheet
    });

    .bph(500, {
        // above 500px high
    });
}
```

#### Options

You can customise a number of options in the LESS. When doing this, if you're also using the JS library, make sure you update the values to match there as well.

##### Flags

Set these flags early in the document, they can be included after you include the breakpoint less file, however should be set before any usage of the mixin.

```less
@IS_RESPONSIVE: true; // [boolean] tells the mixin to either export media queries or not
@FONT_BASE: 16; // [number] base font size (in px) of your site
```

##### Breakpoints

The default breakpoints can be updated simply by editing the following variables. These should be set *before* the less mixin is included into the page.

These default values have been chosen because they are the most common screen resolutions that we normally support.

```less
@bp-min-xxs: 359;
@bp-min-xs: 480;
@bp-min-s: 640;
@bp-min-m: 768; // iPad portrait
@bp-min-l: 1024; // iPad landscape
@bp-min-xl: 1244; // 1280px screen resolution minus scrollbars
@bp-min-xxl: 1410; // 1440px screen resolution minus scrollbars

@bp-max-xxs: @bp-min-xs - 1;
@bp-max-xs: @bp-min-s - 1;
@bp-max-s: @bp-min-m - 1;
@bp-max-m: @bp-min-l - 1;
@bp-max-l: @bp-min-xl - 1;
@bp-max-xl: @bp-min-xxl - 1;
```

The static stylesheet size is specified with the following two variables:

```less
@bp-min-static: unit(0, em);
@bp-max-static: unit(@bp-max-l / @FONT_BASE, em);
```

You don't need to set a maximum of the highest breakpoint.

### JavaScript

There are two main functions for the JS library.

#### `.get()`

Returns the media query as a string. Perfect for use with [enquire.js](http://wicky.nillia.ms/enquire.js/).

You can pass through the same values as the SCSS, however you can also pass through a single string of comma separated values which can be passed through dynamically like from `data-` attributes.

```javascript
DD.bp.get(min /* string || number */, max = 0 /* string || number */, property = 'width' /* string */);

// examples
DD.bp.get('s');
DD.bp.get('s', 'l');
DD.bp.get(0, 500);

// string notation
DD.bp.get('s,l');
```

There is also a shortcut function for height based media queries

```javascript
DD.bp.getHeight(min /* string || number */, max = 0 /* string || number */);
```

#### `.is()`

Returns a boolean indicating if the current viewport matches the requested media query. This uses `window.matchMedia().matches` so use a [polyfill](https://github.com/paulirish/matchMedia.js/) if you need one.

```javascript
DD.bp.is(min /* string || number */, max = 0 /* string || number */, property = 'width' /* string */);

// examples
DD.bp.is('s');
DD.bp.is('s', 'l');
DD.bp.is(0, 500);

// string notation
DD.bp.is('s,l');
```

There is also a shortcut function for height based media queries

```javascript
DD.bp.isHeight(min /* string || number */, max = 0 /* string || number */);
```

#### `.options()`

You can customise the JavaScript library by using the `options()` method.

```javascript
DD.bp.options(opts /* object */);
```

There are three customisable options:

* `baseFontSize` [number=16] Base font size of your site (browser default is normally 16px) this helps convert pixel widths to relative units for the media queries (using em units)
* `isResponsive` [boolean=true] Set to false if the site shouldn't get a responsive stylesheet (e.g. IE8 and below)
* `breakpoints` [Array] Use a custom list of name/pixel width breakpoints instead of the default in an array of `{ name: 'NAME', px: 000 }`

```javascript
DD.bp.options({
    baseFontSize: 16
    isResponsive: true,
    breakpoints: [
        { name: 'small', px: 400 },
        { name: 'medium', px: 800 },
        { name: 'large', px: 1200 }
    ]
});
```

Make sure to ensure that the values used here match the values used in the SCSS.

## Change log

This repo is kept in line with the project [DDBreakpoints by DeloitteDigitalAPAC]https://github.com/DeloitteDigitalAPAC/DDBreakpoints. See that project for details.

## Background

I normally use SCSS for all the projects that I work on, and created [DDBreakpoints]https://github.com/DeloitteDigitalAPAC/DDBreakpoints to make responsive websites much easier to manage. When I was working on a project for a client who used LESS I really missed the simplicity and ease of DDBreakpoints, so I ported it across to LESS so that I could use it there.

My LESS skills aren't the best, and this was my first time using it, so I'm sure there are some inefficiencies in here that could be fixed, however it works perfectly for what I need it to do (which is clone the SCSS version).

The JS can coexist between the two projects without an issue.

## LICENSE (BSD-3-Clause)
[View License](LICENSE)
