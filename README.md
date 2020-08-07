# nuxt-facebook-pixel-module



> A NuxtJS module thats injects Facebook Pixel code

## Table of Contents

* [Requirements](#requirements)
* [Install](#install)
* [Getting Started](#getting-started)
* [License](#license)

## Requirements

* npm or yarn
* NuxtJS
* NodeJS

## Install


1. Add `@forked-prs/nuxt-facebook-pixel-module` dependency to your project

```bash
yarn add --dev @forked-prs/nuxt-facebook-pixel-module # or npm install --save-dev @forked-prs/nuxt-facebook-pixel-module
```

## Getting Started

2. Add `@forked-prs/nuxt-facebook-pixel-module` to the `buildModules` section of `nuxt.config.js`

```js
export default {
  buildModules: [
    ['@forked-prs/nuxt-facebook-pixel-module', {
      track: 'PageView',
      pixelId: 'FACEBOOK_PIXEL_ID',
      disabled: false,
      debug: false,
    }]
  ]
}
```

:warning: If you are using Nuxt **< v2.9** you have to install the module as a `dependency` (No `--dev` or `--save-dev` flags) and use `modules` section in `nuxt.config.js` instead of `buildModules`.


### Using top level options

```js
export default {
  buildModules: [
    '@forked-prs/nuxt-facebook-pixel-module'
  ],
  facebook: {
    track: 'PageView',
    pixelId: 'FACEBOOK_PIXEL_ID',
  }
}
```

### Runtime Config

You can use [runtime config](https://nuxtjs.org/guide/runtime-config) if need to use dynamic environment variables in production. Otherwise, the options will be hardcoded during the build and won't be read from `nuxt.config` anymore.

```js
export default {
  buildModules: [
    '@forked-prs/nuxt-facebook-pixel-module'
  ],

  facebook: {
    pixelId: '123456789012345', // Used as fallback if no runtime config is provided
  },

  publicRuntimeConfig: {
    facebook: {
      /* module options */
      pixelId: process.env.FB_PIXEL_ID || 'some-staging-key',
      disabled: false,
      debug: !!process.env.FB_PIXEL_DEBUG,
    }
  },
}
```

## Disabling the pixel (for GDPR)

If you'd like to install the pixel disabled, and enable it later after the user has consented to its use, you can do so by setting `disabled: true` in the pixel configuration:

```js
{
  buildModules: [
    '@forked-prs/nuxt-facebook-pixel-module',
  ],
  facebook: {
    ...
    disabled: true
  },
}
```

By Default it is disabled in development mode
you can set 
`debug: true`

to enable while in dev or if using conjuction with runtimeconfig you can set `FB_PIXEL_DEBUG=true` in your .env


Now, in your component, you can call the following in order to start the pixel and track the current page.

```js
this.$fb.enable()
```

## Module options

List of possible options in the module:

| Option   | Default  | Required | Description                                                                               |
|----------|----------|----------|-------------------------------------------------------------------------------------------|
| pixelId  | null     | true     | The unique pixel identifier provided by Facebook.                                         |
| track    | PageView | false    | Default tracking event.                                                                   |
| version  | 2.0      | false    | Tracking version.                                                                         |
| disabled | false    | false    | Disable the Pixel by default when initialized. Can be enabled later through `$fb.enable()`.

## Facebook pixel instance

The tracking pixel instance is available on all vue component instances as $fb. It has the following methods:

| Method            | Purpose                                                                                                  | Equivalent to                  |
|-------------------|----------------------------------------------------------------------------------------------------------|--------------------------------|
| enable()          | If you had previously set `disabled: true` in config, enables the pixel and tracks the current page view | $fb.init(), $fb.track()        |
| init()            | Initialises the pixel                                                                                    | fbq('init', <options.pixelId>) |
| track(event, parameters)           | Sends a track event with optional `parameters`. It's `PageView` by default if the `event` is not defined.                                                                                      | fbq('track', <options.track>, parameters)  |
| query(key, value, parameters) | Call the underlying fbq instance with anything else. The `parameters` attribute is optional.                                                      | fbq(key, value, parameters)                |

## License

[MIT License](./LICENSE)
