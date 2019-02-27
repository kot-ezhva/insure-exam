<template>
    <main id="app">
        <h1 class="title">Calculator</h1>

        <div class="wrap">

            <section
                class="fraction"
                v-for="(model, key) in fractionModels"
                :key="generateKey(key)"
            >
                <div class="fraction__wrap">
                    <button class="fraction__remove" @click="remove(key)" v-if="allowRemove"></button>

                    <FractionInput v-model="model.top"/>
                    <hr>
                    <FractionInput v-model="model.bottom"/>
                </div>

                <div class="fraction__symbol">
                    {{ key === (fractionModels.length - 1) ? '=' : '+' }}
                </div>
            </section>

            <Result
                :top="commonTop"
                :bottom="commonBottom"
            />

            <section class="actions">
                <Button @click="append()" :disabled="!canAppend">+ Add fraction</Button>
            </section>
        </div>
    </main>
</template>

<script>
    import FractionInput from '@/components/FractionInput.vue';
    import Button from '@/components/Button.vue';
    import Result from '@/components/Result.vue';

    export default {
        name: 'app',
        components: { FractionInput, Button, Result },
        data() {
            return {
                fractionModels: [
                    {
                        top: 1,
                        bottom: 1,
                    },
                    {
                        top: 1,
                        bottom: 1,
                    },
                ],
            };
        },
        computed: {
            canAppend() {
                return this.fractionModels.length < 5;
            },
            allowRemove() {
                return this.fractionModels.length > 2;
            },
            commonBottom() {
                return this.fractionModels.reduce((accumulator, item) => accumulator * item.bottom, 1);
            },
            commonTop() {
                let result = [];
                this.fractionModels.forEach(item => {
                    result.push(this.commonBottom / item.bottom * item.top);
                });

                return result.reduce((accumulator, item) => accumulator + item);
            },
        },
        methods: {
            append() {
                this.fractionModels.push({
                    top: 1,
                    bottom: 1,
                });
            },
            generateKey(key) {
                return `key_${Math.floor(Math.random() * 100)}_${key}`;
            },
            remove(key) {
                this.$delete(this.fractionModels, key);
            }
        },
    };
</script>
