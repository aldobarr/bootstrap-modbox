# bootstrap-modbox
Native JavaScript wrapper for simple Bootstrap 5 modals.  Provides support for alert, confirm, and prompt modals, as well as advanced custom dialogs.

&nbsp;

## Table of Contents

- [Description](#description)
- [Requirements](#requirements)
- [Getting Started](#getting-started)
- [Documentation](#documentation)
- [Notes](#notes)
- [Copyright and License](#copyright-and-license)


## Description

I know, not *another* JavaScript library for Bootstrap modals, right?

However, I found that existing libraries seem to be no longer updated/supported, are for older versions of Bootstrap, and/or still require jQuery.  So, I decided to write my own for Bootstrap 5, using all native JavaScript and native Bootstrap markup and CSS - there is no custom styling.

I should mention that both the standard and module versions of the library are using [newer JavaScript features](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#browser_compatibility) (as of this writing) which are only supported in more recent versions of the major browsers; like class structures, including private fields and methods.

That said, I have also included a compatibility version of the library transpiled by Babel, if needed.  It targets the versions of all major browsers which first included class support (for specifics, check out the [Grunt configuration file](https://github.com/erobertson42/bootstrap-modbox/blob/main/Gruntfile.js)).  I *did not* go back as far as ES5, because Bootstrap 5 no longer supports it either.


## Requirements

Just [Bootstrap](https://getbootstrap.com/) version 5.0.1 or greater.


## Getting Started

The modbox library can be added to your project in several ways:
- [NPM](https://www.npmjs.com/package/bootstrap-modbox):
	```
	npm i bootstrap-modbox
	```
	Although since it's client-side, if you're not using a bundler (eg. Webpack), you'll also have to copy the necessary `.js` file(s) from `node_modules/bootstrap-modbox/dist` into your project folder structure and include it in your page(s) like the examples below.

- [Unpkg CDN](https://unpkg.com/browse/bootstrap-modbox/):
	```html
	<script src="https://unpkg.com/bootstrap-modbox"></script>
	```
	```javascript
	import modbox from 'https://unpkg.com/bootstrap-modbox@1.0.0/dist/bootstrap-modbox.esm.min.js';
	```

- Download from [GitHub](https://github.com/erobertson42/bootstrap-modbox/releases):
	```html
	<script src="dist/bootstrap-modbox.min.js"></script>
	```
	```javascript
	import modbox from './dist/bootstrap-modbox.esm.min.js';
	```


## Documentation

Full documentation and examples are available at https://erobertson42.github.io/bootstrap-modbox/.

All of the class modal methods return a Promise, whereas calling the  constuctor does not (`new modbox()`).  For `.confirm()` and `.prompt()`, the Promise is resolved when clicking the `okButton`, and rejected when the modal is closed in any other way (`closeButton`, 'X', ESC key, clicking the backdrop, etc).  For `.alert()` and its variants, which only have the single Close button, the Promise is always resolved.

Simple alert box:
```javascript
modbox.alert('Pay attention to the thing.');
```

Alert box with success styling and alternate title (similar methods also exist for `.info()`, `.danger()`, and `.error()`):
```javascript
modbox.success({
	title: 'Done!',
	body: 'The thing has completed successfully.'
});
```

Confirm box with custom button label:
```javascript
modbox.confirm({
	body: 'Confirm the thing?',
	okButton: {
		label: 'Confirm'
	}
})
.then(() => console.log('Confirm button clicked'))
.catch(() => console.log('Confirm button not clicked'));
```


## Notes

- The input value returned by `modbox.prompt()` is not sanitized in any way.  If the intent is to store or process this data, it is your responsibility to make sure it is safe.
- Safari does not yet support [private class methods](https://caniuse.com/mdn-javascript_classes_private_class_methods) (latest version as of writing: 15).  As a work-around, I added a Grunt task to convert all private methods into private field functions:

	```javascript
	/* source */
	#myPrivateMethod(foo, bar) {
		// do the thing
	}

	/* dist */
	#myPrivateMethod = (foo, bar) => {
		// do the thing
	}
	```
	This step is meant to be temporary, and I plan to remove it sometime after Safari adds support.


## Copyright and License

&copy; 2021 Eric Robertson and released under the [MIT License](https://github.com/erobertson42/bootstrap-modbox/blob/main/LICENSE).
