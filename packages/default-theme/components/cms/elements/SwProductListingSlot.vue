<template>
  <div class="sw-product-list">
    <SfLoader :loading="loading" class="sw-product-list__loader"/>
      <div class="sw-product-list__wrapper" v-if="products.length">
        <div class="sw-product-list__list" :class="{'sw-product-list__list--blur': loading}">
          <SwProductCard
            class="sw-product-list__product-card"
            v-for="product in products"
            :key="product.id"
            :product="product"
          />
          <div class="sw-product-list__place-holder"/>
        </div>
        <SfPagination
          class="sw-product-list__pagination desktop-only"
          :current="pagination.currentPage"
          :total="Math.ceil(pagination.total / pagination.perPage)"
          :visible="5"
          @click="changedPage"
        />
      </div>
      <SfHeading
        v-else
        title="No products found"
        subtitle="let us look for them"
      />
  </div>
</template>

<script>
import SwProductCard from '../../SwProductCard'
import { SfPagination, SfHeading, SfLoader } from '@storefront-ui/vue'
import { useProductListing } from '@shopware-pwa/composables'
export default {
  components: {
    SwProductCard,
    SfPagination,
    SfHeading,
    SfLoader
  },
  props: {
    content: {
      type: Object,
      default: () => ({})
    }
  },
  setup({ content }) {
    const propProducts = content.data.listing || []
    const {
      products,
      changePagination,
      pagination,
      loading
    } = useProductListing(propProducts)

    const changedPage = async (pageNumber) => {
      await changePagination(pageNumber)
    }

    return {
      products,
      changedPage,
      pagination,
      loading
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~@storefront-ui/vue/styles.scss';

// additional screen variables
$desktop-big: 1200px;
$desktop: 1024px;
$desktop-small: 900px;
$tablet: 768px;
$tablet-small: 600px;
$phone: 480px;
// max photo width
$mx-photo-wth-5: 20%;
$mx-photo-wth-4: 25%;
$mx-photo-wth-3: 33%;
$mx-photo-wth-2: 50%;
$mx-photo-wth-1: 75%;
// product cards - limit in column
$col-prod-5: 1 0 $mx-photo-wth-5;
$col-prod-4: 1 0 $mx-photo-wth-4;
$col-prod-3: 1 0 $mx-photo-wth-3;
$col-prod-2: 1 0 $mx-photo-wth-2;
$col-prod-1: 1 0 $mx-photo-wth-1;



@mixin for-desktop-big {
  @media screen and (min-width: $desktop-big) {
    @content;
  }
}

@mixin for-desktop {
  @media screen and (min-width: $desktop) {
    @content;
  }
}

@mixin for-desktop-small {
  @media screen and (min-width: $desktop-small) {
    @content
  }
}

@mixin for-tablet {
  @media screen and (min-width: $tablet) {
    @content
  }
}

@mixin for-tablet-small {
  @media screen and (min-width: $tablet-small) {
    @content
  }
}

@mixin for-phone {
  @media screen and (min-width: $phone) {
    @content
  }
}

.sw-product-list {
  display: flex;
  justify-content: center;
  position: relative;

  &__loader {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    --loader-overlay-background: transparent;
    width: 38px;
    height: 38px;
    z-index: 2;
  }

  ::v-deep &__wrapper {
    display: flex;
    flex-direction: column;
    width: 100%;
  }

  &__list {
    display: flex;
    width: 100%;
    flex-flow: row wrap;
    transition: filter .1s ease-in;
    &--blur {
      filter: blur(10px);
    }
  }

  ::v-deep &__product-card {
    flex: 1 0 50%;
    padding: var(--spacer);

    @include for-phone {
      flex: $col-prod-3;
      padding: var(--spacer);
    }

    @include for-tablet-small {
      flex: $col-prod-3;
      padding: var(--spacer);
    }

    @include for-tablet {
      flex: $col-prod-4;
      padding: var(--spacer);
    }

    @include for-desktop-small {
      flex: $col-prod-4;
      padding: var(--spacer);
    }


    @include for-desktop {
      flex: $col-prod-4;
      padding: var(--spacer);
    }

    @include for-desktop-big {
      flex: $col-prod-5;
      padding: var(--spacer-big);
    }
  }
 
  &__pagination {
    @include for-desktop-small {
      display: flex;
      justify-content: center;
      margin-top: var(--spacer-big);
    }
  }
}

.section {
  @media (max-width: $desktop-min) {
    padding-left: var(--spacer-big);
    padding-right: var(--spacer-big);
  }
}

::v-deep .sf-product-card {
  max-width: $mx-photo-wth-2 !important;

  @include for-phone {
    max-width: $mx-photo-wth-3 !important;
  }

  @include for-tablet-small {
    max-width: $mx-photo-wth-3 !important;
  }

  @include for-tablet {
    max-width: $mx-photo-wth-4 !important;
  }

  @include for-desktop-small {
    max-width: $mx-photo-wth-4 !important;
  }

  @include for-desktop {
    max-width: $mx-photo-wth-4 !important;
  }

  @include for-desktop-big {
    max-width: $mx-photo-wth-5 !important;
  }
}
</style>
