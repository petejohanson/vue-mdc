
<template>
<div>
  <div class="demo-toolbar mdc-theme--primary-bg mdc-theme--text-primary-on-primary mdc-typography--title mdc-elevation--z4">
    <button class="demo-menu material-icons" @click="$refs.drawer.open()">menu</button>
    <span class="demo-toolbar--label">VueJS Material Components Demo</span>
  </div>

  <mdc-temporary-drawer ref="drawer" style="z-index: 20;">
    <div slot="header" class="mdc-temporary-drawer__header-content mdc-theme--primary-bg mdc-them--text-primary-on-primary">
      Header here
    </div>

    <nav class="mdc-list-group">
      <div id="icon-with-text-demo" class="mdc-list">
        <a class="mdc-list-item mdc-temporary-drawer--selected" href="#">
          <i class="material-icons mdc-list-item__start-detail" aria-hidden="true">inbox</i>Inbox
        </a>
        <a class="mdc-list-item" href="#">
          <i class="material-icons mdc-list-item__start-detail" aria-hidden="true">star</i>Star
        </a>
        <a class="mdc-list-item" href="#">
          <i class="material-icons mdc-list-item__start-detail" aria-hidden="true">send</i>Sent Mail
        </a>
        <a class="mdc-list-item" href="#">
          <i class="material-icons mdc-list-item__start-detail" aria-hidden="true">drafts</i>Drafts
        </a>
      </div>

      <hr class="mdc-list-divider">

      <div class="mdc-list">
        <a class="mdc-list-item" href="#">
          <i class="material-icons mdc-list-item__start-detail" aria-hidden="true">email</i>All Mail
        </a>
        <a class="mdc-list-item" href="#">
          <i class="material-icons mdc-list-item__start-detail" aria-hidden="true">delete</i>Trash
        </a>
        <a class="mdc-list-item" href="#">
          <i class="material-icons mdc-list-item__start-detail" aria-hidden="true">report</i>Spam
        </a>
      </div>
    </nav>
  </mdc-temporary-drawer>

  <main>
    <div class="demo-surface" v-mdc-ripple><p>{{label}}</p></div>
    <div>
      <mdc-form-field :align-end='alignEnd'>
        <mdc-checkbox v-model="checked" :label="label" id="my-check" label-id="my-check-label"></mdc-checkbox>
        <mdc-checkbox-label id="my-check-label" for="my-check" :label="label"></mdc-checkbox-label>
      </mdc-form-field>

      <mdc-form-field :align-end='alignEnd'>
        <mdc-radio v-model="radioChecked" :disabled="checked" id="my-radio1" name="radios" val="One" label-id="my-radio1-label"></mdc-radio>
        <mdc-radio-label id="my-radio1-label" for="my-radio1" label="One"></mdc-radio-label>
      </mdc-form-field>

      <mdc-form-field :align-end='alignEnd'>
        <mdc-radio v-model="radioChecked" :disabled="checked" id="my-radio2" name="radios" val="Two" label-id="my-radio2-label"></mdc-radio>
        <mdc-radio-label id="my-radio2-label" for="my-radio2" label="Two"></mdc-radio-label>
      </mdc-form-field>
      <span class="mdc-theme--dark" style="display: inline-block; padding-right: 12px;">
        <mdc-form-field :align-end='alignEnd'>
          <mdc-radio v-model="radioChecked" :disabled="checked" id="my-radio3" name="radios" val="Three" label-id="my-radio3-label"></mdc-radio>
          <mdc-radio-label id="my-radio3-label" for="my-radio3" label="Three"></mdc-radio-label>
        </mdc-form-field>

        <mdc-form-field :align-end='alignEnd'>
          <mdc-radio v-model="radioChecked" :disabled="checked" id="my-radio4" name="radios" val="Four" label-id="my-radio4-label"></mdc-radio>
          <mdc-radio-label id="my-radio4-label" for="my-radio4" label="Four"></mdc-radio-label>
        </mdc-form-field>
      </span>
    </div>
    <div>
      <mdc-textfield id="my-textfield" label="Test Textfield" v-model="text"></mdc-textfield>
    </div>
    <div>
      <mdc-textfield id="my-multiline" label="Test Multine" v-model="multilineText" :multiline="true" :rows="6" :cols="40"></mdc-textfield>
    </div>
    <div>
      <mdc-form-field>
        <mdc-checkbox v-model="alignEnd" :label="label" id="my-check" label-id="my-check-label"></mdc-checkbox>
        <mdc-checkbox-label id="my-check-label" for="my-check" label="Align End?"></mdc-checkbox-label>
      </mdc-form-field>
      <input v-model="label"></input>
    </div>
    <div>
     <p>Change count: {{changeCount}}</p>
    </div>
    <button type="button" @click="showSnackbar">Show Snackbar</button>
    <mdc-snackbar event='mailSent'></mdc-snackbar>

    <mdc-icon-toggle v-model="favorited"
                 :toggle-on="{'label': favoritedLabel, 'content': 'favorite'}"
                 :toggle-off="{'label': 'Add to favorites', 'content': 'favorite_border'}">
    </mdc-icon-toggle>
    <div>
     <div>
       <label for="favorited-label">Favorited Label</label>
       <input id="favorited-label" v-model="favoritedLabel"></input>
     </div>
     <p>Favorited?: {{favorited}}</p>
    </div>
    <div class="mdc-menu-anchor">
      <button @click="$refs.menu.open()">Open Menu</button>
      <mdc-simple-menu ref="menu" :items="['Menu One', 'Menu Two']" @selected="onMenuItemSelected">
        <mdc-menu-list-item>Item One</mdc-menu-list-item>
        <mdc-menu-list-item>Item Two</mdc-menu-list-item>
	<div class="mdc-list-divider" role="separator"></div>
        <mdc-menu-list-item>Item Three</mdc-menu-list-item>
        <mdc-menu-list-item>Item Four</mdc-menu-list-item>
      </mdc-simple-menu>
    </div>
  </main>
</div>
</template>

<script lang="babel">
import { MdcRipple } from '@v-material/ripple';
import { MdcSimpleMenu, MdcMenuListItem } from '@v-material/menu';
import { MdcSnackbar } from '@v-material/snackbar';
import { MdcIconToggle } from '@v-material/icon-toggle';
import { MdcTextfield } from '@v-material/textfield';
import { MdcRadio, MdcRadioLabel } from '@v-material/radio';
import { MdcCheckbox, MdcCheckboxLabel } from '@v-material/checkbox';
import { MdcFormField } from '@v-material/form-field';
import { MdcTemporaryDrawer } from '@v-material/drawer';

export default {
  data () {
    return {
      text: 'Test text field',
      multilineText: 'Test text field',
      label: 'Disable Radios',
      radioChecked: 'One',
      checked: true,
      alignEnd: false,
      changeCount: 0,
      favorited: true,
      favoritedLabel: 'Remove from favorites'
    };
  },
  components: {
    MdcFormField,
    MdcSimpleMenu,
    MdcMenuListItem,
    MdcTextfield,
    MdcCheckbox,
    MdcCheckboxLabel,
    MdcRadio,
    MdcRadioLabel,
    MdcIconToggle,
    MdcSnackbar,
    MdcTemporaryDrawer
  },
  directives: { MdcRipple },
  watch: {
    checked () {
      this.changeCount++;
    }
  },
  methods: {
    onMenuItemSelected ({index, item}) {
      console.log('Item selected: ', item);
    },
    showSnackbar () {
      this.$root.$emit('mailSent', {
        message: 'Mail Sent',
        actionText: 'Undo',
        actionHandler: () => console.log('Undo it')
      });
    }
  }
};
</script>

<style lang="scss">
$mdc-theme-primary: #f44336;
$mdc-theme-accent: #f06292;

@import '@material/typography/mdc-typography';
@import '@material/ripple/mdc-ripple';
@import '@material/elevation/mdc-elevation';
@import '@material/list/mdc-list';
@import '@material/theme/mdc-theme';


.demo-toolbar {
  width: 100%;
  height: 56px;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 0 16px;
  box-sizing: border-box;
}

.demo-toolbar--label {
  margin-left: 16px;
}

@media (min-width: 600px) {
  .demo-toolbar {
    height: 64px;
  }
}

.demo-menu {
  background: none;
  border: none;
  width: 24px;
  height: 24px;
  padding: 0;
  margin: 0;
  color: #FFF;
  box-sizing: border-box;
}

.demo-surface {
  @include mdc-elevation(2);
  width: 150px;
  height: 150px;
}

.mdc-theme--dark {
  background-color: #303030;
}

main {
  padding: 12px;
}
</style>
