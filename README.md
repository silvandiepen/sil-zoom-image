# Zoom Image

Click and move, thats what you can do with these images. Sometimes you don't want to create full big things to show zooms.
This simple (Vue) component just makes it possible to all you need, see a normal view and a zoomed view.
No other options, it's not necessary. Keep it simple!

### Install

Install the package
`npm install @sil/zoom-image`

Import the package

`import ZoomImage from '~/@sil/zoom-image`

Define the component:

```js
export default {
	components: {
		ZoomImage
	}
}
```

Use the component:

```html
<template>
	<div>
		<zoom-image src="https://sample.test/image.jpg" cursor="['https://sample.test/cursor.jpg','https://sample.test/cursor-active.jpg']" size="100% auto" />
	</div>
</template>

```


		
### Props

#### src

`src` needs an a absolute link to the image.

default: `null`

#### size

`size` is the size of the initial view. This is used on background-size:. 

default: `contain`

#### showOverlay

`showOverlay` displays an overlay over the image when you hover.

default: `false`

#### showIcon

`showOverlay` displays an zoom icon over the image when you hover.

default: `false`

#### cursor

`cursor` allows giving an alternate cursor when hovering over the component. The prop needs an array, where the values should
be absolute url's to images. When 2 are given, the first one is used on the default state, where the second one will be used when zoomed in.

If you give one link, this is used for both.

default: `[]`