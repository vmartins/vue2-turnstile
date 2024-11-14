<template>
    <div ref="turnstile"></div>
</template>

<script>
export default {
    props: {
        value: {
            type: String,
            required: true,
        },
        siteKey: {
            type: String,
            required: true,
        },
        action: {
            type: String,
            required: false,
            default: '',
        },
        theme: {
            type: String,
            required: false,
            default: 'auto',
        },
        language: {
            type: String,
            required: false,
            default: 'auto',
        },
        tabindex: {
            type: Number,
            required: false,
            default: 0,
        },
        size: {
            type: String,
            required: false,
            default: 'normal',
        },
        retry: {
            type: String,
            required: false,
            default: 'auto'
        },
        retryInterval: {
            type: Number,
            required: false,
            default: 8000
        },
        refreshExpired: {
            type: String,
            required: false,
            default: 'auto'
        },
        refreshTimeout: {
            type: String,
            required: false,
            default: 'auto'
        },
        appearance: {
            type: String,
            required: false,
            default: 'always',
        },
        feedbackEnabled: {
            type: Boolean,
            required: false,
            default: true
        }
    },

    data() {
        return {
            widgetId: null,
        }
    },

    computed: {
        turnstileOptions() {
            return {
                'sitekey': this.siteKey,
                'action': this.action,
                'theme': this.theme,
                'language': this.language,
                'tabindex': this.tabindex,
                'size': this.size,
                'retry': this.retry,
                'retry-interval': this.retryInterval,
                'refresh-expired': this.refreshExpired,
                'refresh-timeout': this.refreshTimeout,
                'appearance': this.appearance,
                'feedback-enabled': this.feedbackEnabled,
                'callback': this.successCallback,
                'error-callback': this.errorCallback,
                'expired-callback': this.expiredCallback,
                'unsupported-callback': this.unsupportedCallback,
                'before-interactive-callback': this.beforeInteractiveCallback,
                'after-interactive-callback': this.afterInteractivecallback,
            }
        },
    },

    mounted() {
        window['_turnstileCb'] = () => {
            this.render()
        }

        const script = document.createElement('script')
        script.src = "https://challenges.cloudflare.com/turnstile/v0/api.js?onload=_turnstileCb"
        script.async = true
        script.addEventListener('error', () => {
            console.log('Failed to load Turnstile.')
        })

        document.head.appendChild(script)
    },

    beforeUnmount() {
        this.remove()
    },

    methods: {
        render() {
            this.widgetId = window.turnstile.render(this.$refs.turnstile, this.turnstileOptions)
            this.$emit('render', this.widgetId)
        },

        remove() {
            if (this.widgetId) {
                window.turnstile.remove(this.widgetId)
                this.widgetId = null
            }
        },

        successCallback(token) {
            this.$emit('success', token)
            this.$emit('input', token)
        },

        errorCallback(code) {
            this.$emit('error', code)
        },

        expiredCallback() {
            this.$emit('expired')
            this.$emit('input', '')
        },

        unsupportedCallback() {
            this.$emit('unsupported')
        },

        beforeInteractiveCallback() {
            this.$emit('before-interactive')
        },

        afterInteractivecallback() {
            this.$emit('after-interactive')
        },
    }
}
</script>