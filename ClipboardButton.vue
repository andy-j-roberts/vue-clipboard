<template>
    <button class="btn btn-default" type="button" :class="{'btn-success':this.ui.copied}" :data-clipboard-text="text">
        <i class="fa fa-clipboard"></i>
    </button>
</template>
<script>
    var Clipboard = require('clipboard');
    export default {
        props: {
            text: {
                type: String,
                required: true
            }
        },
        data() {
            return {
                ui: {
                    copied: false
                }
            }
        },
        created() {
            bus.$on('copiedToClipboard', (component) => {
                if (component != this) {
                    this.ui.copied = false;
                }
            })
        },
        mounted() {
            var clipboard = new Clipboard(this.$el);
            clipboard.on('success', (e) => {
                this.ui.copied = true;
                bus.$emit('copiedToClipboard', this);
            });
            clipboard.on('error', (e) => {
                this.ui.copied = false;
            });
        }
    }
</script>
