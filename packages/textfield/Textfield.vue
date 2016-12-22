
<template>
<div ref="root" class="mdc-textfield" :class="allClasses">
  <textarea v-if="multiline"
            :id="id"
            ref="textarea"
            class="mdc-textfield__input"
	    @focus="onInputFocus"
            @blur="onInputBlur"
            @input="fireEvent"
            :value="value"
	    :placeholder="placeholder"
            :rows="rows"
            :cols="cols">
  </textarea>
  <input v-else
         class="mdc-textfield__input"
         ref="input"
	 :id="id"
         type="text"
         :value="value"
         @focus="onInputFocus"
         @blur="onInputBlur"
         @input="fireEvent"
         :placeholder="placeholder"
         :aria-label="placeholder">

  <label v-if="label" :for="id" class="mdc-textfield__label" :class="labelClasses">{{label}}</label>
</div>
</template>

<script lang="babel">
import { MDCTextfieldFoundation } from '@material/textfield';

export default {
  props: {
    id: String,
    label: {
      type: String,
      required: false
    },
    placeholder: {
      type: String,
      required: false
    },
    value: {
      type: String,
      required: true
    },
    multiline: {
      type: Boolean,
      required: false
    },
    fullwidth: {
      type: Boolean,
      required: false
    },
    rows: {
      type: Number,
      required: false
    },
    cols: {
      type: Number,
      required: false
    },
    disabled: {
      type: Boolean,
      required: false
    }
  },
  data () {
    return {
      classes: {},
      labelClasses: {},
      helptextClasses: {},
      inputFocusHandlers: [],
      inputBlurHandlers: [],
      foundation: null
    };
  },
  mounted () {
    let vm = this;

    // Avoid issue w/ prefilled text and FOUC
    if (this.value) {
      vm.$set(vm.labelClasses, 'mdc-textfield__label--float-above', true);
      vm.$set(vm.classes, 'mdc-textfield--upgraded', true);
    }

    this.foundation = new MDCTextfieldFoundation({
      addClass (className) {
        vm.$set(vm.classes, className, true);
      },
      removeClass (className) {
        vm.$delete(vm.classes, className);
      },
      addClassToLabel (className) {
        vm.$set(vm.labelClasses, className, true);
      },
      removeClassFromLabel (className) {
        vm.$delete(vm.labelClasses, className);
      },
      addClassToHelptext (className) {
        vm.$set(vm.helptextClasses, className, true);
      },
      removeClassFromHelptext (className) {
        vm.$delete(vm.helptextClasses, className);
      },
      helptextHasClass (className) {
        return Boolean(vm.helptextClasses[className]);
      },
      setHelptextAttr (name, value) {
        // TODO: Implement help text!
      },
      removeHelptextAttr (name) {
        // TODO: Implement help text!
      },
      registerInputFocusHandler (handler) {
        vm.inputFocusHandlers.push(handler);
      },
      deregisterInputFocusHandler (handler) {
        let index = vm.inputFocusHandlers.indexOf(handler);
        if (index >= 0) {
          vm.inputFocusHandlers.splice(index, 1)
        }
      },
      registerInputBlurHandler (handler) {
        vm.inputBlurHandlers.push(handler);
      },
      deregisterInputBlurHandler (handler) {
        let index = vm.inputBlurHandlers.indexOf(handler);
        if (index >= 0) {
          vm.inputBlurHandlers.splice(index, 1)
        }
      },
      getNativeInput () {
        return vm.$refs.input || vm.$refs.textarea;
      }
    });
    this.foundation.init();
  },
  beforeUnmount () {
    this.foundation.destroy();
  },
  methods: {
    onInputFocus (event) {
      this.inputFocusHandlers.forEach((h) => h(event));
    },
    onInputBlur (event) {
      this.inputBlurHandlers.forEach((h) => h(event));
    },
    fireEvent (event) {
      this.$emit('input', event.target.value);
    }
  },
  computed: {
    allClasses () {

      return Object.assign({},
        {
          'mdc-textfield--multiline': this.multiline,
          'mdc-textfield--fullwidth': this.fullwidth,
	},
        this.classes
      );
    }
  }
}

</script>

<style lang="scss">
@import '@material/textfield/mdc-textfield';
</style>
