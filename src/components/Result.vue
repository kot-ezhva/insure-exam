<template>
    <section class="result">
        <div class="result__integer" v-if="integer">
            {{ integer }}
        </div>
        <div class="result__wrap" v-if="showFraction">
            <div class="result__value">{{ simplifiedTop }}</div>
            <hr>
            <div class="result__value">{{ simplifiedBottom }}</div>
        </div>
    </section>
</template>

<script>
    export default {
        name: 'Result',
        props: {
            top: {
                type: [String, Number],
                required: true,
            },
            bottom: {
                type: [String, Number],
                required: true,
            },
        },
        computed: {
            integer() {
                return Math.floor(this.top / this.bottom);
            },
            showFraction() {
                return (this.top % this.bottom);
            },
            topWithoutInteger() {
                return this.top - this.bottom * this.integer;
            },
            commonDivider() {
                const getDivider = (a, b) => {
                    if (b) {
                        return getDivider(b, a % b);
                    } else {
                        return Math.abs(a);
                    }
                };

                return getDivider(this.topWithoutInteger, this.bottom);
            },
            simplifiedTop() {
                return this.topWithoutInteger / this.commonDivider;
            },
            simplifiedBottom() {
                return this.bottom / this.commonDivider;
            },
        }
    }
</script>