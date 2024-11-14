# vue2-turnstile

A [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/) library for Vue 2.

## Installation

```bash
npm install git+https://github.com/vmartins/vue2-turnstile.git --save
```

## Usage

```vue
<script>
import VueTurnstile from 'vue2-turnstile'

export default {
  components: { VueTurnstile },

  data() {
    return {
      token: '',
    }
  },
}
</script>

<template>
  <div>
    <vue-turnstile
        v-model="token"
        site-key="1x0123456789ABCDEFGHIJKL"
    />
    <div>Token: {{ token }}</div>
  </div>
</template>
```

## Customization options

| Prop             | Type                                          | Description                                                                                                                             | Required | Default    |
| ---------------- | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------- | ---------- |
| v-model          | `String`                                      | Binding that contains the token returned by the Turnstile widget                                                                        | Yes      | N/A        |
| site-key         | `String`                                      | Your Turnstile sitekey                                                                                                                  | Yes      | N/A        | 
| action           | `String`                                      | A customer value that can be used to differentiate widgets under the same sitekey in analytics and which is returned upon validation.   | No       | `''`       |
| theme            | `'light' \| 'dark' \| 'auto'`                 | Widget theme - auto respects the user's browser preference                                                                              | No       | `'auto'`   |
| language         | `String`                                      | Language to display - ISO 639-1 two-letter language code                                                                                | No       | `auto`     |
| tabindex         | `Number`                                      | The tabindex of Turnstile’s iframe for accessibility purposes                                                                           | No       | `0`        |
| size             | `'normal' \| 'flexible' \| 'compact'`         | Widget size                                                                                                                             | No       | `'normal'` |
| retry            | `'auto' \| 'never'`                           | Controls whether the widget should automatically retry to obtain a token if it did not succeed                                          | No       | `'auto'`   |
| retry-interval   | `Number`                                      | When ```retry=auto```, this controls the time between retry attempts in milliseconds. Value must be a positive integer less than 900000 | No       | `8000`     |
| refresh-expired  | `'auto' \| 'manual' \| 'never'`               | Automatically refreshes the token when it expires.                                                                                      | No       | `'auto'`   |
| refresh-timeout  | `'auto' \| 'manual' \| 'never'`               | Controls whether the widget should automatically refresh upon entering an interactive challenge and observing a timeout.                | No       | `'auto'`   |
| appearance       | `'always' \| 'execute' \| 'interaction-only'` | Appearance controls when the widget is visible                                                                                          | No       | `'always'` |
| feedback-enabled | `Boolean`                                     | Allows Cloudflare to gather visitor feedback upon widget failure                                                                        | No       | `true`     |


## Events

| Method                   | Params  | Description                                                                           |
| ------------------------ | ------- | ------------------------------------------------------------------------------------- |
| `@error`                 | `code`  | Callback invoked when there is an error (e.g. network error or the challenge failed). |
| `@render`                | -       | Callback invoked when widget rendered.                                                |
| `@success`               | `token` | Callback invoked upon success of the challenge. The callback is passed a token.       |
| `@expired`               | -       | Callback invoked when the token expires and does not reset the widget.                |
| `@unsupported`           | -       | callback invoked when a given client/browser is not supported                         |
| `@before-interactive`    | -       | Callback invoked before the challenge enters interactive mode.                        |
| `@after-interactive`     | -       | callback invoked when challenge has left interactive mode.                            |


## License

The MIT License (MIT). Please see LICENSE file for more information.
