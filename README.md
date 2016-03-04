# Vue In Viewport Mixin


Vue mixin to determine when a DOM element is visible in the client window

Example usage:
* Just require the mixin from your component.
* Use the optional offset props with the `:` [dynamic syntax](http://vuejs.org/guide/components.html#Literal_vs-_Dynamic) so the value is parsed as a JS number.  Numbers between 0 and 1 are treated like percentages of the page height.
	```
	large-copy(:in-viewport-offset-top="-100" :in-viewport-offset-bottom="0.5")
	```

## Directive version

There is also a directive you can use.  For example:

```coffee
Vue.directive 'in-viewport', require 'vue-in-viewport-mixin/directive'
```
```jade
section(v-in-viewport
	:in-viewport-active='false'
	:in-viewport-offset-top='-100'
	:in-viewport-offset-bottom='0.5'
	:in-viewport-once='false')
```

## Delaying init

You may want to delay the in-viewport checks from happening until some time after the vm is has been added to the dom.  For instance, if the vm is added to the dom early but is invisible (to allow for media loading), you want to wait to register in viewport behavior until it is displayed.

Set `:in-viewport-active="false"` do disable the auto-activation of the mixin AND the directive.  Then, when you are ready to initialize in-viewport, set `inViewportActive` on the parent *vm*.  Both mixin and directive watch this to toggle their behavior.

Here is an example Vue mixin that accomplishes this:

```coffee
module.exports =

	# Give this var to all components that don't already have it as a prop
	data: -> return inViewportActive: null unless @inViewportActive?

	# Set active to false initially and then later active
	created: ->
		@inViewportActive = false
		setTimeout (=> @inViewportActive = true), 1000
```
