
<template>
<div ref="root" class="mdc-radio" :class="classes">
  <input ref="native"
         type="radio"
	 class="mdc-radio__native-control"
         @change="fireEvent"
         v-model="checked"
	 :name="name"
	 :value="val"
         :id="id"
         :aria-labelledby="labelId" />
  <div class="mdc-radio__background">
    <div class="mdc-radio__outer-circle"></div>
    <div class="mdc-radio__inner-circle"></div>
  </div>
</div>
</template>

<script lang="babel">
import { MDCRadioFoundation } from '@material/radio';

export default {
  props: {
    id: String,
    disabled: {
      required: false
    },
    name: String,
    labelId: String,
    val: {
      type: [String, Number],
      required: true
    },
    value: {
      type: [String, Number],
      required: true
    }
  },
  data () {
    return {
      classes: {},
      foundation: null
    };
  },
  mounted () {
    // TODO: Ripple!
    let vm = this;
    this.foundation = new MDCRadioFoundation({
      addClass (className) {
        vm.$set(vm.classes, className, true);
      },
      removeClass (className) {
        vm.$delete(vm.classes, className);
      },
      getNativeControl () {
        return vm.$refs.native;
      },
    });
    this.foundation.init();
    this.foundation.setDisabled(this.disabled);
  },
  beforeUnmount () {
    this.foundation.destroy();
  },
  methods: {
    fireEvent (event) {
      this.$emit('input', this.checkedProxy);
    }
  },
  watch: {
    disabled(val) {
      if (this.foundation.isDisabled() === val) {
        return;
      }

      this.foundation.setDisabled(val);
    }
  },
  computed: {
    checked: {
      get() { return this.value; },
      set (val) { this.checkedProxy = val; }
    },
    labelId () {
      return this.id + '-label';
    }
  }
}

</script>

<style lang="scss">
@import '@material/radio/mdc-radio';
</style>
