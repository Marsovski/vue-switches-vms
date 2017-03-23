<template>
    <label :class="classObject">
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
		callBack: Function,
		selected: false,

		disabled: false,

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
			this.$emit('callBack');
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
     * VMS
     */
    $color-vms-default: #01b6bf;

    $theme-vms-colors: (
        default : $color-vms-default
    );

    .vue-switcher {
        position: relative;
        display: inline-block;

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
                top: 0px;
                height: 24px;
                width: 50px;

                &:after {
                    height: 14px;
                    width: 14px;
                    margin-left: -20px;
                    top: 5px;
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

        &-theme--vms {
            @each $colorName, $color in $theme-vms-colors {
                &.vue-switcher-color--#{$colorName} {

                    div {
                        @if $colorName == 'default' {
                            background-color: $color;
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
                                background-color: #a8abbc;
                            } @else {
                                background-color: lighten($color, 30%);
                            }

                            &:after {
                                background-color: #fff;
                            }
                        }
                    }
                }
            }
        }
    }

</style>
