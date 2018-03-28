<template>
    <div :class="['spinner',{'spinner-global':global}]" :style="spinnerOverlayStyle" v-show="active">
        <div class="spinner-wrapper">
            <div class="spinner-circle" :style="spinnerStyle"></div>
            <div class="spinner-text" :style="textStyle" v-if="label">{{ label }}</div>
        </div>
    </div>
</template>

<script>

export default {

    props: {
        'global': {
            type: Boolean,
            default: false
        },
        'size': {
            // pixel width/height
            type: Number,
            default: 32
        },
        'line': {
            type: Number,
            default: 3
        },
        'shadow': {
            type: String,
            default: '#EEEEEE'
        },
        'color': {
            type: String,
            default: '#337ab7',
        },
        'speed': {
            type: Number,
            default: 0.8
        },
        'spacing': {
            type: Number,
            default: 4
        },
        'label': {
            type: String,
            default: ''
        },
        'label-size': {
            type: Number,
            default: 13
        },
        'label-color': {
            type: String,
            default: '#555555'
        },
        'background-color': {
            type: String,
            default: 'rgba(255,255,255,0.9)'
        },
        'loading': {
            type: Boolean,
            default: false
        },
        'delay': {
            type: Number,
            default: 500
        }

    },

    data () {
        return {
            active: false
        }
    },
    watch: {
        loading: function(val) {
            if (val) {
                this.start();
            }
            else {
                this.finish();
            }
        }
    },
    computed: {
        spinnerOverlayStyle() {
            return {
                'background-color': this.backgroundColor,
                'padding': this.size + 'px auto',
            }
        },
        spinnerStyle() {
            return {
                'animation-duration': this.speed + 's',
                'animation-iteration-count': 'infinite',
                'animation-name': 'spin',
                'animation-timing-function': 'linear',
                'border-radius': '100%',
                'border-width': this.line + 'px',
                'border-style': 'solid',
                'border-color': this.shadow,
                'border-top-color': this.color,
                'height': this.size + 'px',
                'width': this.size + 'px'
            }
        },
        textStyle() {
            return {
                'color': this.labelColor,
                'font-size': this.labelSize + 'px',
                'margin-top': Math.min(Math.max(Math.ceil(this.labelSize/8), 3), 12)
            }
        }
    },
    methods: {
        start () {
            clearTimeout(this._spinnerAnimation);
            this.documentBody.style.overflowY = 'hidden';
            this.active = true;
        },
        finish () {
            this._spinnerAnimation = setTimeout(() => {
                this.active = false;
                this.documentBody.style.overflowY = this.documentBodyOverflow;
            }, this.delay);
        }
    },
    mounted () {
        this.documentBody = document.querySelector('body')
        this.documentBodyOverflow = this.documentBody.style.overflowY || ''
    },
    created () {
        this.$on('spinner-start', this.start);
        this.$on('spinner-finish', this.finish);
    },
    beforeDestroy () {
        this.$off('spinner-start', this.start);
        this.$off('spinner-finish', this.finish);
        clearTimeout(this._spinnerAnimation);
        this.documentBody.style.overflowY = this.documentBodyOverflow;
    }
}
</script>

<style>
    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
    .spinner {
        position: absolute;
        top: 0; bottom: 0; left: 0; right: 0;
        z-index: 100000;
        width: 100%;
        text-align: center;
        transition: all 0.3s linear;
    }
    .spinner.spinner-global {
        position: fixed;
    }
    .spinner .spinner-wrapper {
        position: absolute;
        top: 50%; left: 50%;
        transform: translate(-50%, -50%);
    }
    .spinner .spinner-circle {
        position: relative;
        display: inline-block;
        z-index: 10;
        --transition: all 0.3s linear;
    }
    .spinner .spinner-text {
        position: relative;
        text-align: center;
        z-index: 10;
        width: 100%;
    }
</style>