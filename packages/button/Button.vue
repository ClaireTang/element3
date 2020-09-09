<template>
  <button
    class="el-button"
    @click="handleClick"
    :disabled="buttonDisabled || loading"
    :autofocus="autofocus"
    :type="nativeType"
    :class="[
      type ? 'el-button--' + type : '',
      buttonSize ? 'el-button--' + buttonSize : '',
      {
        'is-disabled': buttonDisabled,
        'is-loading': loading,
        'is-plain': plain,
        'is-round': round,
        'is-circle': circle
      }
    ]"
  >
    <i class="el-icon-loading" v-if="loading"></i>
    <i :class="icon" v-if="icon && !loading"></i>
    <span v-if="$slots.default">
      <slot></slot>
    </span>
  </button>
</template>
<script>
import { computed, getCurrentInstance, inject, unref, toRefs } from 'vue'
export default {
  name: 'ElButton',

  // inject: {
  //   elForm: {
  //     default: ''
  //   },
  //   elFormItem: {
  //     default: ''
  //   }
  // },

  props: {
    type: {
      type: String,
      default: 'default'
    },
    size: {
      type: String,
      default: ''
    },
    icon: {
      type: String,
      default: ''
    },
    nativeType: {
      type: String,
      default: 'button'
    },
    loading: Boolean,
    disabled: Boolean,
    plain: Boolean,
    autofocus: Boolean,
    round: Boolean,
    circle: Boolean
  },

  setup(props, ctx) {
    const { size, disabled } = toRefs(props)

    const elForm = inject('elForm', {})
    const elFormItem = inject('elFormItem', {})

    const _elFormItemSize = computed(() => unref(elFormItem.elFormItemSize))

    const buttonSize = computed(
      () =>
        size.value ||
        _elFormItemSize.value ||
        (getCurrentInstance().proxy.$ELEMENT || {}).size
    )

    const buttonDisabled = computed(
      () => disabled.value || unref(elForm.disabled)
    )

    const handleClick = (evt) => {
      ctx.emit('click', evt)
    }

    return {
      buttonSize,
      buttonDisabled,
      handleClick
    }
  },

  emits: ['click']

  // computed: {
  //   _elFormItemSize() {
  //     return (this.elFormItem || {}).elFormItemSize;
  //   },
  //   buttonSize() {
  //     return this.size || this._elFormItemSize || (this.$ELEMENT || {}).size;
  //   },
  //   buttonDisabled() {
  //     return this.disabled || (this.elForm || {}).disabled;
  //   }
  // },

  // methods: {
  //   handleClick(evt) {
  //     this.$emit('click', evt);
  //   }
  // }
}
</script>

