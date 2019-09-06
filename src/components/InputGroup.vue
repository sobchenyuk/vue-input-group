<template>
    <div class="input-group">

        <input
            v-for="(item, index) in countInput" :key="item.toString()"
            type="text"
            :maxlength="maxlength"
            :placeholder="placeholder"
            v-model="text[index]"
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
                text: [],
                select: true
            }
        },

        methods: {

            init() {
                window.addEventListener('mouseout', this.onMouseout);

                this.elem = this.$el.querySelectorAll('.input');

                this.number = Array.apply(null, {length: this.countInput}).map(Number.call, Number);

            },

            onMouseout(e) {
                if (this.activeNumTrue) {
                    if (!e.toElement) {
                        this.elem[this.activeNum].blur();
                        this.activeNum = '';
                    }
                }
            },

            onBlur() {
                this.activeNum = '';
            },

            keymonitor(e) {
                const key = e.key;
                const target = e.target;
                const countNegative = this.activeNum - 1;
                if (key === 'Backspace') {
                    if (this.number.indexOf(countNegative) !== -1) {
                        this.select = false;
                        target.selectionStart === 0 && this.onLeft();
                    }
                }
            },

            onClick(val) {
                this.activeNum = val;
                this.activeNumTrue && this.elem[this.activeNum].select();
            },

            onInput($event) {
                const target = $event.target;
                this.select = true;
                target.value.length === this.maxlength && this.onRight();

                const array = Array.from(this.text.filter((e) => e))

                this.$emit('v-input', array)
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
                    if(this.select) elem[count].value.length && elem[count].select();
                    this.activeNum = count;
                }
            }

        },

        computed: {
            activeNumTrue() {
                return typeof this.activeNum !== 'string'
            },
            placeholder() {
                let i = 0;
                let result = '';
                while (i < this.maxlength) {
                    result += 'X';
                    i++
                }
                return result;
            }
        },

        mounted() {
            this.init();
        },

        destroyed() {
            window.removeEventListener('mouseout', this.onMouseout)
        }

    }
</script>

<style>
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

    @media (max-width: 768px) {
        .input {
            font-size: 40px;
        }

        .input + .input {
            margin-left: 0.5em;
        }
    }
</style>