<template>
    <label :class="classObject">
        <span class="vue-switcher__label">
            <span v-if="label" v-text="label"></span>
            <span v-if="!label && enabled" v-text="textEnabled"></span>
            <span v-if="!label && !enabled" v-text="textDisabled"></span>
        </span>

        <input type="checkbox" :disabled="disabled" v-model="enabled">

        <div></div>
    </label>
</template>

<script>

export default {
	name: 'switches',
	props: {
		typeBold: {
			default: true
		},
		selected: false,
		disabled: false,

		label: '',

		textEnabled: {
			default: ''
		},

		textDisabled: {
			default: ''
		},

		color: {
			default: 'default'
		},

		theme: {
			default: 'vms'
		}
	},

	data() {
		return {
			enabled: !!this.selected
		};
	},

	mounted() {
		this.$emit('input', this.enabled = !!this.selected);
	},

	watch: {
		enabled(val) {
			this.$emit('input', val);
		},

		selected(val) {
			this.enabled = !!val;
		}
	},

	computed: {
		classObject() {
			const { color, enabled, theme, typeBold, disabled } = this;

			return {
				'vue-switcher': true,
				[`vue-switcher--unchecked`]: !enabled,
				[`vue-switcher--disabled`]: disabled,
				[`vue-switcher--bold`]: typeBold,
				[`vue-switcher--bold--unchecked`]: typeBold && !enabled,
				[`vue-switcher-theme--` + theme]: theme,
				[`vue-switcher-color--` + color]: color
			};
		}
	}
};

</script>


<style lang="scss">
    /**
     * Default
     */
    $color-default-default: #aaa;
    $color-default-green: #53b96e;
    $color-default-blue: #539bb9;
    $color-default-red: #b95353;
    $color-default-orange: #b97953;
    $color-default-yellow: #bab353;

    $theme-default-colors: (
        default : $color-default-default,
        blue    : $color-default-blue,
        red     : $color-default-red,
        yellow  : $color-default-yellow,
        orange  : $color-default-orange,
        green   : $color-default-green
    );

    /**
     * Bulma
     */
    $color-bulma-default: #f5f5f5;
    $color-bulma-primary: #00d1b2;
    $color-bulma-blue: #3273dc;
    $color-bulma-red: #ff3860;
    $color-bulma-yellow: #ffdd57;
    $color-bulma-green: #22c65b;

    $theme-bulma-colors: (
        default : $color-bulma-default,
        primary : $color-bulma-primary,
        blue    : $color-bulma-blue,
        red     : $color-bulma-red,
        yellow  : $color-bulma-yellow,
        green   : $color-bulma-green
    );

    /**
     * Bootstrap
     */
    $color-bootstrap-default: #fff;
    $color-bootstrap-primary: #337ab7;
    $color-bootstrap-success: #5cb85c;
    $color-bootstrap-info: #5bc0de;
    $color-bootstrap-warning: #f0ad4e;
    $color-bootstrap-danger: #c9302c;

    $theme-bootstrap-colors: (
        default : $color-bootstrap-default,
        primary : $color-bootstrap-primary,
        success : $color-bootstrap-success,
        info    : $color-bootstrap-info,
        warning : $color-bootstrap-warning,
        danger  : $color-bootstrap-danger
    );

    /**
     * VMS
     */
    $color-vms-default: #01b6bf;

    $theme-vms-colors: (
        default : $color-vms-default
    );

    .vue-switcher {
        position: relative;
        display: inline-block;

        &__label {
            display: block;
            font-size: 10px;
            margin-bottom: 5px;
        }

        input {
            opacity: 0;
            width: 100%;
            height: 100%;
            position: absolute;
            z-index: 1;
            cursor: pointer;
        }

        div {
            height: 10px;
            width: 40px;
            position: relative;
            border-radius: 30px;
            display: -webkit-flex;
            display: -ms-flex;
            display: flex;
            align-items: center;
            justify-content: flex-start;
            cursor: pointer;

            &:after {
                content: '';
                height: 18px;
                width: 18px;
                border-radius: 100px;
                display: block;
                transition: all ease .3s;
                position: absolute;
                left: 100%;
                margin-left: -17px;
                cursor: pointer;
                top: -4px;
            }
        }

        &--unchecked {
            div {
                justify-content: flex-end;

                &:after {
					left: 15px;
                }
            }
        }

        &--disabled {
            div {
                opacity: .3;
            }

            input {
                cursor: not-allowed;
            }
        }

        &--bold {
            div {
                top: -8px;
                height: 26px;
                width: 51px;

                &:after {
                    margin-left: -22px;
                    top: 4px;
                }
            }
            &--unchecked {
                div {
                    &:after {
                        left: 26px;
                    }
                }
            }
        }

        &-theme--default {
            @each $colorName, $color in $theme-default-colors {
                &.vue-switcher-color--#{$colorName} {

                    div {
                        @if $colorName == 'default' {
                            background-color: lighten($color, 0%);
                        } @else {
                            background-color: lighten($color, 10%);
                        }

                        &:after {
                            @if $colorName == 'default' {
                                background-color: darken($color, 5%);
                            } @else {
                                background-color: $color
                            }
                        }
                    }

                    &.vue-switcher--unchecked {
                        div {

                            @if $colorName == 'default' {
                                background-color: $color;
                            } @else {
                                background-color: lighten($color, 30%);
                            }

                            &:after {
                                background-color: lighten($color, 10%);
                            }
                        }
                    }
                }
            }
        }

        &-theme--bulma {
            @each $colorName, $color in $theme-bulma-colors {
                &.vue-switcher-color--#{$colorName} {

                    div {
                        @if $colorName == 'default' {
                            background-color: darken($color, 10%);
                        } @else {
                            background-color: lighten($color, 10%);
                        }

                        &:after {
                            background-color: $color;
                        }
                    }

                    &.vue-switcher--unchecked {
                        div {

                            @if $colorName == 'default' or $colorName == 'yellow' {
                                background-color: darken($color, 5%);
                            } @else {
                                background-color: lighten($color, 30%);
                            }

                            &:after {

                                @if $colorName == 'default' {
                                    background-color: $color;
                                } @else {
                                    background-color: lighten($color, 10%);
                                }
                            }
                        }
                    }
                }
            }
        }

        &-theme--bootstrap {
            @each $colorName, $color in $theme-bootstrap-colors {
                &.vue-switcher-color--#{$colorName} {
                    div {
                        @if $colorName == 'default' {
                            background-color: darken($color, 10%);
                        } @else {
                            background-color: lighten($color, 10%);
                        }

                        &:after {
                            @if $colorName == 'default' {
                                background-color: darken($color, 6%);
                            } @else {
                                background-color: $color;
                            }
                        }
                    }

                    &.vue-switcher--unchecked {
                        div {

                            @if $colorName == 'default' {
                                background-color: darken($color, 4%);
                            } @else {
                                background-color: lighten($color, 30%);
                            }

                            &:after {

                                @if $colorName == 'default' {
                                    background-color: darken($color, 6%);
                                } @else {
                                    background-color: lighten($color, 10%);
                                }
                            }
                        }
                    }
                }
            }
        }


        &-theme--vms {
            @each $colorName, $color in $theme-vms-colors {
                &.vue-switcher-color--#{$colorName} {

                    div {
                        @if $colorName == 'default' {
                            background-color: lighten($color, 0%);
                        } @else {
                            background-color: lighten($color, 10%);
                        }

                        &:after {
                            @if $colorName == 'default' {
                                background-color: #fff;
                            } @else {
                                background-color: $color
                            }
                        }
                    }

                    &.vue-switcher--unchecked {
                        div {

                            @if $colorName == 'default' {
                                background-color: darken(#ccc, 20%);
                            } @else {
                                background-color: lighten($color, 30%);
                            }

                            &:after {
                                background-color: darken(#fff, 20%);
                            }
                        }
                    }
                }
            }
        }


    }

</style>