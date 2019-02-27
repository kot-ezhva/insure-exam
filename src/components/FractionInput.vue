<template>
    <div class="fraction__input" :class="{'has-error': hasError}">
        <input type="text" v-model="model" @input="onInput()">
    </div>
</template>

<script>
    export default {
        name: 'FractionInput',
        props: {
            value: {
                type: [Number, String],
                required: true,
            }
        },
        computed: {
            hasError() {
                if (this.model.toString().match(/^\d+$/)) {
                    if (parseInt(this.model) < 1 || parseInt(this.model) > 99) {
                        return true;
                    }

                    return false;
                }

                return true;
            },
        },
        data() {
            return {
                model: this.value,
            };
        },
        methods: {
            onInput() {
                this.$emit('input', this.model);
            },
        },
    }
</script>
