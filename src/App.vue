<template>
    <div id="app" :style="appStyle">

        <InputGroup
                :maxlength="maxlength"
                :countInput="countInput"
                :placeholderSymbol="placeholderSymbol"
                :labelGroupText="labelGroupText"
                :allClass="{
                    inputGroupWrapper : 'inputGroupWrapper',
                    labelGroup : 'labelGroup',
                    inputGroup : 'inputGroup',
                    inputGroupControl : 'inputGroupControl'
                }"
                @v-input="vInput"
        />

        <p class="result">Script result: <span class="result__input">{{ inputGroupText | join }}</span></p>

    </div>
</template>

<script>
    import logo from './assets/logo.png'
    import InputGroup from "@/components/InputGroup";

    export default {
        name: 'app',

        components: {InputGroup},

        data() {
            return {
                countInput: 4,
                maxlength: 2,
                inputGroupText: [],
                placeholderSymbol: 'x',
                labelGroupText: 'Label: Vue Input Group',

                appStyle: {
                    background: `url(${logo})`,
                    backgroundRepeat: 'no-repeat',
                    backgroundPosition: 'center',
                    backgroundSize: 'contain'
                }
            }
        },

        filters: {
            join(value) {
                if (!value) return ''
                return value.join('-')
            }
        },

        methods: {

            vInput(result) {
                this.inputGroupText = result;
            }

        }
    }
</script>

<style>
    html, body {
        height: 100%;
    }

    body {
        display: flex;
        align-items: center;
    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        max-width: 1200px;
        margin-left: auto;
        margin-right: auto;
    }

    #app:before {
        content: '';
        display: block;
        position: absolute;
        left: 0;
        top: 0;
        height: 100%;
        width: 100%;
        background: rgba(255, 255, 255, 0.7411764705882353);
    }

    .result {
        margin-top: 1em;
        font-size: 50px;
        font-weight: 900;
        display: inline-block;
        position: relative;
        z-index: 1;
    }

    .inputGroupWrapper {
        position: relative;
        z-index: 1;
    }

    .labelGroup {
        text-shadow: 0 3px 2px #00802d;
        color: #35495e;
    }

    @media (max-width: 768px) {
        .result {
            font-size: 40px;
        }

        .result__input {
            display: block;
        }
    }
</style>
