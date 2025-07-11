<template>
  <div class="theme-generator">
    <dock
      @trigger-visible="handleTriggerVisible"
      @refresh-content="handleRefreshContent"
      @change-theme="handleChangeTheme"
      @click-setting="handleClickSetting"
      :drawerVisible="visible"
      :showSetting="showSetting"
    />
    <panel-drawer
      :drawerVisible="visible"
      :theme="theme"
      :refresh="refresh"
      @panel-drawer-visible="handleDrawerVisible"
      :propsTop="propsTop"
    />
  </div>
</template>

<script>
import {
  applyThemeFromLocal,
  DEFAULT_THEME,
  generateNewTheme,
  getOptionFromLocal,
  syncThemeToIframe,
} from './common/Themes';

import Dock from './dock/index.vue';
import PanelDrawer from './panel-drawer/index.vue';

const activeTabMap = {
  color: 0,
  font: 1,
  radius: 2,
  shadow: 3,
  size: 4,
};

export default {
  name: 'ThemeGenerator',
  components: {
    PanelDrawer,
    Dock,
  },
  props: {
    propsTop: String,
    showSetting: {
      type: [Boolean, String],
    },
    device: {
      type: String,
      default: 'web',
    },
  },
  provide() {
    return {
      device: this.device,
    };
  },
  data() {
    return {
      activeTabMap,
      refresh: false,
      visible: 0,
      activeTabIdx: activeTabMap.color,
      theme: DEFAULT_THEME,
    };
  },
  mounted() {
    const localTheme = getOptionFromLocal('color') ?? DEFAULT_THEME.value;
    generateNewTheme(localTheme, undefined, this.device);
    syncThemeToIframe(this.device);
    applyThemeFromLocal(this.device);
  },
  methods: {
    handleChangeTheme(theme) {
      this.theme = theme;
    },
    handleRefreshContent() {
      this.refresh = !this.refresh;
    },
    handleTriggerVisible() {
      this.visible = true;
    },
    handleDrawerVisible(v) {
      this.visible = v;
      this.$emit('panel-drawer-visible', v);
    },
    handleClickSetting() {
      this.$emit('click-setting');
      this.visible = false;
    },
  },
};
</script>

<style lang="less" scoped>
@import './styles/reset.min.css';
@import './styles/tdesign.min.css';

@media screen and (max-width: 960px) {
  .theme-generator {
    display: none;
  }
}
</style>
<style>
.t-popup .t-select-option {
  font-size: 14px;
}
.t-popconfirm {
  z-index: 10000;
}
.t-popup .t-input-number {
  font-size: 14px;
}
.t-popup .t-input {
  border-radius: 3px !important;
}
.t-popup .t-icon {
  font-size: 14px !important;
}

.t-popconfirm .t-icon {
  font-size: 20px !important;
}
.t-popup .t-select__empty {
  font-size: 14px;
}
.t-button.t-size-l {
  font-size: 16px;
}
.t-radio-button__label {
  font-size: 14px;
}
.t-slider__button {
  width: 16px;
  height: 16px;
}
.t-button--variant-base.t-button--theme-primary:hover,
.t-button--variant-base.t-button--theme-primary:focus-visible {
  border-color: var(--brand-main-hover);
  background-color: var(--brand-main-hover);
}
</style>
