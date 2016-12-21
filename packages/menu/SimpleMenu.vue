
<template>
<div tabindex="-1" :class="classes">
  <ul ref="itemsContainer" class="mdc-simple-menu__items mdc-list" role="menu" aria-hidden="true">
    <slot></slot>
  </ul>
</div>
</template>

<script lang="babel">
import { MDCSimpleMenuFoundation } from '@material/menu';
import {getTransformPropertyName} from '@material/menu/util';

export default {
  data () {
    return {
      previousFocus: null,
      classes: { 'mdc-simple-menu': true },
      foundation: null
    };
  },
  mounted () {
    let vm = this;
    this.foundation = new MDCSimpleMenuFoundation({
      addClass (className) {
        vm.$set(vm.classes, className, true);
      },
      removeClass (className) {
        vm.$delete(vm.classes, className);
      },
      hasClass (className) {
        return Boolean(vm.classes[className]);
      },
      hasNecessaryDom () {
        return Boolean(vm.$el);
      },
      getInnerDimensions () {
        return {width: vm.$refs.itemsContainer.offsetWidth, height: vm.$refs.itemsContainer.offsetHeight};
      },
      hasAnchor () {
        return vm.$el.parentElement &&
	   vm.$el.parentElement.classList.contains('mdc-menu-anchor');
      },
      getAnchorDimensions () {
        return vm.$el.parentElement.getBoundingClientRect();
      },
      getWindowDimensions () {
        return {width: window.innerWidth, height: window.innerHeight};
      },
      setScale (x, y) {
        vm.$el.style[getTransformPropertyName(window)] = `scale(${x}, ${y})`;
      },
      setInnerScale (x, y) {
        vm.$refs.itemsContainer.style[getTransformPropertyName(window)] = `scale(${x}, ${y})`;
      },
      getNumberOfItems () {
        return vm.domItems.length;
      },
      registerInteractionHandler (type, handler) {
        vm.$el.addEventListener(type, handler);
      },
      deregisterInteractionHandler (type, handler) {
        vm.$el.removeEventListener(type, handler);
      },
      registerDocumentClickHandler (handler) {
        document.addEventListener('click', handler);
      },
      deregisterDocumentClickHandler (handler) {
        document.removeEventListener('click', handler);
      },
      getYParamsForItemAtIndex (index) {
        const {offsetTop: top, offsetHeight: height} = vm.domItems[index];
        return {top, height};
      },
      setTransitionDelayForItemAtIndex (index, value) {
        vm.domItems[index].style.setProperty('transition-delay', value);
      },
      getIndexForEventTarget (target) {
        return vm.domItems.indexOf(target);
      },
      notifySelected ({index}) {
	vm.$emit('selected', { item: vm.domItems[index], index });
      },
      notifyCancel () {
        vm.$emit('cancel');
      },
      saveFocus () {
        vm.previousFocus = document.activeElement;
      },
      restoreFocus () {
        if (vm.previousFocus) {
          vm.previousFocus.focus();
        }
      },
      isFocused () {
        return vm.previousFocus === vm.$el;
      },
      focus () {
        vm.$el.focus();
      },
      getFocusedItemIndex () {
        return vm.domItems.indexOf(document.activeElement);
      },
      focusItemAtIndex (index) {
        vm.domItems[index].focus();
      },
      isRtl () {
        return window.getComputedStyle(vm.$el).getPropertyValue('direction') === 'rtl';
      },
      setTransformOrigin (origin) {
        vm.$el.style[`${getTransformPropertyName(window)}-origin`] = origin;
      },
      setPosition (position) {
        vm.$el.style.left = 'left' in position ? position.left : null;
        vm.$el.style.right = 'right' in position ? position.right : null;
        vm.$el.style.top = 'top' in position ? position.top : null;
        vm.$el.style.bottom = 'bottom' in position ? position.bottom : null;
      }

    });
    this.foundation.init();
  },
  beforeUnmount () {
    this.foundation.destroy();
  },
  methods: {
    open () {
      this.foundation.open();
    }
  },
  computed: {
    domItems () {
      return [].slice.call(this.$refs.itemsContainer.querySelectorAll('.mdc-list-item[role]'));
    }
  }
}

</script>

<style lang="scss">
@import '@material/menu/mdc-menu';
</style>
