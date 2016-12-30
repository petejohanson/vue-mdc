
<template>
<div ref="root" class="mdc-select" role="listbox" :class="classes">
  <span class="mdc-select__selected-text">{{value}}</span>
  <mdc-simple-menu ref="menu" class="mdc-select__menu">
    <ul class="mdc-list mdc-simple-menu__items">
      <li v-for="o in options" :key="o.id" class="mdc-list-item" role="option" :id="o.id" :aria-selected="value === o.label" tabindex="0">{{o.label}}</li>
    </ul>
  </mdc-simple-menu>
</div>
</template>

<script lang="babel">
import { MdcSimpleMenu } from '@v-material/menu';
import { MDCSelectFoundation } from '@material/select';

export default {
  props: {
    id: String,
    options: {
      required: true
    },
    value: {
      type: String,
      required: true
    }
  },
  components: { MdcSimpleMenu },
  data () {
    return {
      classes: {},
      foundation: null
    };
  },
  mounted () {
    let vm = this;
    this.foundation = new MDCSelectFoundation({
      addClass (className) {
        vm.$set(vm.classes, className, true);
      },
      removeClass (className) {
        vm.$delete(vm.classes, className);
      },
      setAttr (attr, value) {
        vm.$refs.root.setAttribute(attr, value);
      },
      rmAttr (attr, value) {
        vm.$refs.root.removeAttribute(attr, value);
      },
      computeBoundingRect () {
        return vm.$refs.root.getBoundingClientRect ();
      },
      registerInteractionHandler (type, handler) {
        vm.$refs.root.addEventListener(type, handler);
      },
      deregisterInteractionHandler (type, handler) {
        vm.$refs.root.removeEventListener(type, handler);
      },
      focus () {
        vm.$refs.root.focus();
      },
      makeTabbable () {
        vm.$refs.root.tabIndex = 0;
      },
      makeUntabbable () {
        vm.$refs.root.tabIndex = -1;
      },
      getComputedStyleValue (prop) {
        return window.getComputedStyle(vm.$refs.root).getPropertyValue(prop);
      },
      setStyle (propertyName, value) {
        vm.$refs.root.style.setProperty(propertyName, value);
      },
      create2dRenderingContext () {
        return document.createElement('canvas').getContext('2d');
      },
      setMenuElStyle (propertyName, value) {
        vm.$refs.menu.$el.style.setProperty(propertyName, value);
      },
      setMenuElAttr (attr, value) {
        vm.$refs.menu.$el.setAttribute(attr, value);
      },
      rmMenuElAttr (attr) {
        vm.$refs.menu.$el.removeAttribute(attr);
      },
      getMenuElOffsetHeight () {
        return vm.$refs.menu.$el.offsetHeight;
      },
      openMenu (focusIndex) {
        return vm.$refs.menu.open({ focusIndex });
      },
      isMenuOpen () {
        return vm.$refs.menu.isOpen;
      },
      setSelectedTextContent (selectedTextContent) {
	vm.$emit('input', selectedTextContent);
      },
      getNumberOfOptions () {
        return vm.options.length;
      },
      getTextForOptionAtIndex (index) {
        return vm.options[index].label;
      },
      setAttrForOptionAtIndex (index, attr, value) {
        return vm.$refs.menu.domItems[index].setAttribute(attr, value);
      },
      rmAttrForOptionAtIndex (index, attr, value) {
        return vm.$refs.menu.domItems[index].removeAttribute(attr);
      },
      getOffsetTopForOptionAtIndex (index) {
        return vm.$refs.menu.domItems[index].offsetTop;
      },
      registerMenuInteractionHandler (type, handler) {
        vm.$refs.menu.$el.addEventListener(type, handler);
      },
      deregisterMenuInteractionHandler (type, handler) {
        vm.$refs.menu.$el.removeEventListener(type, handler);
      },
      notifyChange () {
	// We do this right in setSelectedTextContent
	// to properly support v-model.
      },
      getWindowInnerHeight () {
        return window.innerHeight;
      }
    });
    this.foundation.init();
  },
  beforeUnmount () {
    this.foundation.destroy();
  },
  computed: {
    labelId () {
      return this.id + '-label';
    }
  }
}

</script>

<style lang="scss">
@import '@material/select/mdc-select';
</style>
