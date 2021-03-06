<template>
  <div class="layout">
    <TopNavigation />
    <SwPluginTopNavigation />
    <SfBreadcrumbs
      v-show="getBreadcrumbs.length > 0"
      :breadcrumbs="getBreadcrumbs"
      class="sw-breadcrumbs"
      @click="redirectTo"
    />
    <nuxt />
    <SwCart />
    <SwBottomNavigation class="layout__bottom-navigation" />
    <SwFooter />
  </div>
</template>

<script>
import { SfBreadcrumbs } from '@storefront-ui/vue'
import TopNavigation from '@shopware-pwa/default-theme/components/TopNavigation'
import SwBottomNavigation from '@shopware-pwa/default-theme/components/SwBottomNavigation'
import SwCart from '@shopware-pwa/default-theme/components/SwCart'
import SwFooter from '@shopware-pwa/default-theme/components/cms/elements/SwFooter'
import SwPluginTopNavigation from 'sw-plugins/SwPluginTopNavigation'

export default {
  components: {
    SfBreadcrumbs,
    TopNavigation,
    SwCart,
    SwFooter,
    SwBottomNavigation,
    SwPluginTopNavigation
  },
  computed: {
    componentBreadcrumbs() {
      // TODO probably move to vuex now as it's not rendered on server side
      return (
        this.$route.matched.map(r => {
          return r.components.default.options.data().breadcrumbs
        })[0] || {}
      )
    },
    getBreadcrumbs() {
      return Object.keys(this.componentBreadcrumbs)
        .map(key => this.componentBreadcrumbs[key])
        .map(breadcrumb => ({
          text: breadcrumb.name,
          route: {
            link: breadcrumb.path
          }
        }))
    }
  },
  methods: {
    redirectTo(route) {
      return this.$router.push(route.link)
    }
  }
}
</script>

<style lang="scss">
@import '~@storefront-ui/vue/styles';

html {
  overflow-x: hidden;
  height: 100vh;
}

a {
  text-decoration: none;
  color: var(--c-link);
  &:hover {
    color: var(--c-link-hover);
  }
}

/*Header styles*/
h1 {
  font-family: var(--body-font-family-secondary);
  font-size: var(--h1-font-size);
  font-weight: var(--h1-font-weight);
  line-height: 1.6;
  margin: 0;
}

h2 {
  font-family: var(--body-font-family-secondary);
  font-size: var(--h2-font-size);
  font-weight: var(--h2-font-weight);
  line-height: 1.6;
  margin: 0;
}

h3 {
  font-family: var(--body-font-family-secondary);
  font-size: var(--h3-font-size);
  font-weight: var(--h3-font-weight);
  line-height: 1.6;
  margin: 0;
}

h4 {
  font-family: var(--body-font-family-secondary);
  font-size: var(--h4-font-size);
  font-weight: var(--h4-font-weight);
  line-height: 1.6;
  margin: 0;
}

h5 {
  font-family: var(--body-font-family-secondary);
  font-size: var(--h5-font-size);
  font-weight: var(--h5-font-weight);
  line-height: 1.6;
  margin: 0;
}

body {
  overflow-x: hidden;
  padding: 0;
  margin: 0;
  min-height: 100vh;
  font-family: var(--body-font-family-primary);
  font-weight: var(--body-font-weight-primary);
  font-size: var(--font-size-regular);
  line-height: 1.6;
}

#__nuxt {
  height: 100vh;
}

#__layout {
  height: 100%;
}

.layout {
  box-sizing: border-box;
  @include for-desktop {
    max-width: 1320px;
    margin: auto;
  }
  height: 100%;
  display: flex;
  flex-direction: column;

  &__bottom-navigation {
    @include for-desktop() {
      display: none;
    }
  }
}

.sw-breadcrumbs {
  padding: 0 var(--spacer-extra-big) var(--spacer-big) var(--spacer-extra-big);
}

/* Delete firefox outline */
:focus {
  outline: none;
}
::-moz-focus-inner {
  border: 0;
}
</style>
