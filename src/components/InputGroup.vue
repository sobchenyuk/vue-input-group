<template>
    <div class="input-group">

        <input
            v-for="(item, index) in countInput" :key="item.toString()"
            type="text"
            :maxlength="maxlength"
            @click="onClick(index)"
            @input="onInput($event)"
            @keyup="keymonitor"
            @blur="onBlur"
            class="input"
        >

    </div>
</template>

<script>
    export default {
        name: "InputGroup",
        props: {
            countInput: {
                type: Number,
                default: 2,
            },
            maxlength: {
                type: Number,
                default: 2,
            },
        },
        data() {
            return {
                number: [],
                activeNum: '',
                elem: [],
                count: '',
                text: '',
            }
        },

        methods: {

            init() {
                window.addEventListener('mouseout', e => {
                    if (this.activeNumTrue) {
                        if (!e.toElement) {
                            this.elem[this.activeNum].blur();
                            this.activeNum = '';
                        }
                    }
                });

                this.elem = this.$el.querySelectorAll('.input');

                this.number = Array.apply(null, {length: this.countInput}).map(Number.call, Number);

            },

            onBlur() {
                this.activeNum = '';
            },

            keymonitor(e) {
                const key = e.key;
                const target = e.target;
                const val = target.value;
                const countNegative = this.activeNum - 1;
                if (key === 'Backspace') {
                    if (this.number.indexOf(countNegative) !== -1) {
                        if (target.selectionStart === 0) {
                            val.length === 0 && this.onLeft();
                        }
                    }
                }
            },

            onClick(val) {
                this.activeNum = val;
                if (this.activeNumTrue) {
                    this.elem[this.activeNum].select();
                }
            },

            onInput($event) {
                const target = $event.target;
                target.value.length === this.maxlength && this.onRight();
            },

            onLeft() {
                if (this.activeNumTrue) {
                    this.count = this.activeNum -1;
                    this.counter();
                }
            },

            onRight() {
                if (this.activeNumTrue) {
                    this.count = this.activeNum +1;
                    this.counter();
                }
            },

            counter() {
                const elem = this.elem;
                const count = this.count;
                if (this.number.indexOf(count) !== -1) {
                    elem[this.activeNum].blur();
                    elem[count].focus();
                    elem[count].value.length && elem[count].select();
                    this.activeNum = count;
                }
            }

        },

        computed: {
            activeNumTrue() {
                return typeof this.activeNum !== 'string'
            }
        },

        mounted() {
            this.init();
        }
    }
</script>

<style scoped>
    .input-group {
        display: flex;
    }

    .input {
        text-align: center;
        font-size: 60px;
        width: 25%;
    }

    .input + .input {
        margin-left: 1em;
    }
</style>