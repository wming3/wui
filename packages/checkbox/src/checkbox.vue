<template>
    <label class="wui-checkbox" :class="shape == 'circle' ? 'wui-checkbox-circle' : ''">
        <template v-if="group">
            <input type="checkbox" v-model="model" :value="val" @change="changeHandler" :disabled="disabled" />
        </template>
        <template v-else>
            <input type="checkbox" v-model="checked" :disabled="disabled" />
        </template>

        <span class="wui-checkbox-icon" :style="iconStyles()"><i :style="checkIconStyles()"></i></span>

        <template v-if="!$slots.default">
            <span class="wui-checkbox-text">{{val}}</span>
        </template>
        <template v-else>
            <span class="wui-checkbox-text"><slot></slot></span>
        </template>
    </label>
</template>

<script type="text/babel">

    export default {
        name: 'w-checkbox',
        data() {
            return {
                model: [],
                group: false,
                checked: this.value
            }
        },
        props: {
            value: {
                type: Boolean,
                default: false
            },
            val: {
                type: [Boolean, String, Number]
            },
            disabled: {
                type: Boolean,
                default: false
            },
            color: {
                default: '#4CD864'
            },
            size: {
                validator(val) {
                    return /^([1-9]\d*)$/.test(val);
                },
                default: 20
            },
            shape: {
                validator(value) {
                    return ['square', 'circle'].indexOf(value) > -1;
                },
                default: 'square'
            }
        },
        methods: {
            changeHandler() {
                if (this.disabled) return;
                setTimeout(() => {
                    this.$parent.change(this.model);
                }, 0);
            },
            iconStyles() {
                const size = (this.group ? this.$parent.size : this.size) + 'px';
                const color = this.group ? this.$parent.color : this.color;

                return {
                    width: size,
                    height: size,
                    color: color
                };
            },
            checkIconStyles() {
                const size = this.group ? this.$parent.size : this.size;

                return {
                    width: Math.round(size / 3.2) + 'px',
                    height: Math.round(size / 1.7) + 'px'
                }
            }
        },
        watch: {
            checked(val) {
                this.$emit('input', val);
            },
            value(val) {
                this.checked = val;
            }
        },
        created() {
            if (this.$parent.color) {
                this.group = true;
            }
        }
    }
</script>

<style lang="less" scoped>
    @import './checkbox.less';
</style>
