# lite-n-toasty
Lite N Toasty is a tiny but powerful JavaScript library for toast notifications. It's responsive, dependency-free and tiny (~3KB).

Download and include the files in on your main app page or anywhere you want to see your notifications.


## Features

- 📱 Responsive
- ✨ Optional ripple-like fancy revealing effect
- 😈 Simple but highly extensible API. Create your own toast types and customize them.
- 🎃 Support to render custom HTML content within the toasts
- 🐣 Tiny footprint (<3K gzipped)
- 👴🏽 Works on IE11



## Usage

This section explains the base case using the minified bundle. 

Add the css and js files to your main document:


### Basic

```javascript
// Create an instance of LiteNToasty
var toasty = new LiteNToasty();

// Display an error notification
toasty.error('You must fill out the form before moving forward');

// Display a success notification
toasty.success('Your changes have been successfully saved!');
```

## API

You can set some options when creating a LiteNToasty instance.

### `new LiteNToasty(options: CustomOptions)`

Param | Type | Default | Details
------------ | ------------- | ------------- | -------------
duration | `number` | 2000 | Number of miliseconds before hiding the notification. Use `0` for infinite duration.
ripple | `boolean` | true | Whether to show the notification with a ripple effect
position | [`Position`](#position) | `{x:'right',y:'bottom'}` | Viewport location where notifications are rendered
dismissible | `boolean` | false | Whether to allow users to dismiss the notification with a button
types | [`NotificationOptions[]`](#notificationoptions) | Success and error toasts | Array with individual configurations for each type of toast

### `dismiss(notification: notification)`

Dismiss a specific notification.

```javascript
const toasty = new LiteNToasty();
const notification = toasty.success('Address updated');
toasty.dismiss(notification);
```

### `dismissAll()`

Dismiss all the active notifications.

```javascript
const toasty = new LiteNToasty();
toasty.success('Address updated');
toasty.error('Please fill out the form');
toasty.dismissAll();
```

## Events

Every individual notification emits events. You can register listeners using the `on` method.

### `'click'`

Triggers when the notification is clicked

```javascript
const toasty = new LiteNToasty();
const notification = toasty.success('Address updated. Click here to continue');
notification.on('click', ({target, event}) => {
  // target: the notification being clicked
  // event: the mouseevent
  window.location.href = '/';
});
```

### `'dismiss'`

Triggers when the notification is **manually** (not programatically) dismissed.

```javascript
const toasty = new LiteNToasty();
toasty
  .error({
    message: 'There has been an error. Dismiss to retry.',
    dismissible: true
  })
  .on('dismiss', ({target, event}) => foobar.retry());
```

## Interfaces

### Position

Viewport location where notifications are rendered.

Param | Type | Details
------------ | ------------- | -------------
x | `left \| center \| right` | x-position
y | `top \| center \| bottom` | y-position

### NotificationOptions

Configuration interface for each individual toast.

Param | Type  | Details
------------ | ------------- | -------------
type | `string` | Notification type for which this configuration will be applied
className | `string` | Custom class name to be set in the toast wrapper element
duration | `number` | 2000 | Number of miliseconds before hiding the notification
icon | [`Icon \| false`](#icon) | An object with the properties of the icon to be rendered. 'false' hides the icon.
background | `string` | Background color of the toast
message | `string` | Message to be rendered inside of the toast. Becomes the default message when used in the global config.
ripple | `boolean` | Whether or not to render the ripple at revealing
dismissible | `boolean` | Whether to allow users to dismiss the notification with a button

### Icon

Icon configuration

Param | Type | Details
------------ | ------------- | -------------
className | `string` | Custom class name to be set in the icon element
tagName | `string` | HTML5 tag used to render the icon
text | `string` | Inner text rendered within the icon (useful when using [ligature icons](https://css-tricks.com/ligature-icons/))
color | `string` | Icon color. It must be a valid CSS color value. Defaults to background color.

## Examples

### Global configuration

The following example configures Notyf with the following settings:
- 1s duration
- Render notifications in the top-right corner
- New custom notification called 'warning' with a [ligature material icon](https://google.github.io/material-design-icons/)
- Error notification with custom duration, color and dismiss button

```javascript
const toasty = new LiteNToasty({
  duration: 1000,
  position: {
    x: 'right',
    y: 'top',
  },
  types: [
    {
      type: 'warning',
      background: 'orange',
      icon: {
        className: 'material-icons',
        tagName: 'i',
        text: 'warning'
      }
    },
    {
      type: 'error',
      background: 'indianred',
      duration: 2000,
      dismissible: true
    }
  ]
});
```

### Custom toast type

Register a new toast type and use it by referencing its type name:

```javascript
const toasty = new LiteNToasty({
  types: [
    {
      type: 'info',
      background: 'blue',
      icon: false
    }
  ]
});

toasty.open({
  type: 'info',
  message: 'Send us <b>an email</b> to get support'
});
```

**Warning:** LiteNToasty doesn't sanitize the content when rendering your message. To avoid [injection attacks], you should either sanitize your HTML messages or make sure you don't render user generated content on the notifications.

### Default types with custom configurations

The default types are 'success' and 'error'. You can use them simply by passing a message as its argument, or you can pass a settings object in case you want to modify its behaviour.

```javascript
const toasty = new LiteNToasty();

toasty.error({
  message: 'Accept the terms before moving forward',
  duration: 9000,
  icon: false
})
```


## Inspired By

LiteNToasty is built on and inspired by Notyf. https://github.com/caroso1222/notyf
