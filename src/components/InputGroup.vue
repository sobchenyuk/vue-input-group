<template>

    <div
            class="input-group__wrapper"
            :class="{ [isInputGroupWrapperClass]: isInputGroupWrapperClass.length}"
            aria-labelledby="block group input fields"
    >

        <label
                class="input-group__label"
                :class="{ [isLabelGroupClass]: isLabelGroupClass.length}"
                v-if="isLabel"
        >{{ labelGroupText }}</label>

        <div
                class="input-group"
                :class="{
                    [isInputGroupClass]: isInputGroupClass.length,
                    'input-group_type_is-many': isManyInput
                }"
                aria-labelledby="group input"
        >
            <div
                    class="input-group__col"
                    v-for="(item, index) in countInput" :key="item.toString()"
            >

                <input
                        type="text"
                        :maxlength="maxlength"
                        :placeholder="placeholder"
                        v-model="text[index]"
                        @click="onClick(index)"
                        @input="onInput($event)"
                        @keyup="keymonitor"
                        @blur="onBlur"
                        class="input-group__control"
                        :class="{ [isInputGroupControlClass]: isInputGroupControlClass.length}"
                        :aria-labelledby="`password entry-${index.toString()}`"
                >
            </div>


        </div>
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
            placeholderSymbol: {
                type: String,
                default: 'X',
            },
            labelGroupText: {
                type: String,
                default: ''
            },
            allClass: {
                type: Object,
                default() {
                    return {
                        inputGroupWrapper : '',
                        labelGroup : '',
                        inputGroup : '',
                        inputGroupControl : ''
                    }
                }
            }
        },
        data() {
            return {
                number: [],
                activeNum: '',
                elem: [],
                count: '',
                text: [],
                select: true,

                widthInputControlGroup: '',
                widthInputControl: ''
            }
        },

        methods: {

            init() {
                window.addEventListener('mouseout', this.onMouseout);

                this.elem = this.$el.querySelectorAll('.input-group__control');

                this.number = Array.apply(null, {length: this.countInput}).map(Number.call, Number);
            },

            // delete child support focus if cursor leaves browser screen
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

            // I get all the keystrokes at the input field
            keymonitor(e) {
                const key = e.key;
                const target = e.target;
                const countNegative = this.activeNum - 1;
                if (key === 'Backspace') {
                    if (this.number.indexOf(countNegative) !== -1) {
                        this.select = false;
                        // if the cursor is at the beginning
                        if (target.selectionStart === 0) {
                            this.onLeft();
                            // rearrange cursor
                            const elem = this.elem[this.activeNum]; // current item
                            elem.setSelectionRange( elem.value.length, elem.value.length );
                        }
                    }
                }
            },

            // set focus on the selected item
            onClick(val) {
                this.activeNum = val;
                if (this.activeNumTrue) {
                    this.elem[this.activeNum].value.length > 0 && this.elem[this.activeNum].select();
                }
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
            },

            getClassName(name) {
                return typeof this.allClass[name] !== 'undefined'
                    ?  this.allClass[name].trim()
                    : ''
            },

        },

        computed: {

            activeNumTrue() {
                return typeof this.activeNum !== 'string'
            },

            isLabel() {
                return this.labelGroupText.trim()
            },

            isManyInput() {
                return this.countInput > 1
            },

            placeholder() {
                const symbol = this.placeholderSymbol;
                let i = 0;
                let result = '';
                while (i < this.maxlength) {
                    result += symbol;
                    i++
                }
                return result;
            },

            isInputGroupWrapperClass() {
                return this.getClassName('inputGroupWrapper')
            },
            isLabelGroupClass() {
                return this.getClassName('labelGroup')
            },
            isInputGroupClass() {
                return this.getClassName('inputGroup')
            },
            isInputGroupControlClass() {
                return this.getClassName('inputGroupControl')
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
    *, ::after, ::before {
        box-sizing: border-box;
    }

    .input-group {
        display: flex;
        flex-wrap: wrap;
        margin-right: -15px;
        margin-left: -15px;
    }

    .input-group__col {
        flex-basis: 0;
        flex-grow: 1;
        max-width: 100%;
        position: relative;
        width: 100%;
        min-height: 1px;
        padding-right: 15px;
        padding-left: 15px;
    }

    .input-group__wrapper {
        width: 100%;
    }

    .input-group__label {
        font-size: 40px;
        padding: .5em;
        display: inline-block;
        text-align: center;
    }

    .input-group__control {
        text-align: center;
        font-size: 60px;
        display: block;
        width: 100%;
    }

    @media (max-width: 768px) {
        .input-group__control {
            font-size: 40px;
        }
    }
</style>
