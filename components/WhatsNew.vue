<template>
  <div class="WhatsNew">
    <h3 class="WhatsNew-heading">
      <v-icon size="24" class="WhatsNew-heading-icon">
        mdi-information
      </v-icon>
      {{ $t('最新のお知らせ') }}
      <span class="alert"
        >※
        <a href="https://twitter.com/ngs_ken_iryou" target="_blank"
          >長崎県医療政策課のTwitter情報を表示しています。</a
        ></span
      >
    </h3>
    <ul class="WhatsNew-list">
      <li v-for="(item, i) in items" :key="i" class="WhatsNew-list-item">
        <a
          class="WhatsNew-list-item-anchor"
          :href="item.url"
          target="_blank"
          rel="noopener noreferrer"
        >
          <time
            class="WhatsNew-list-item-anchor-time px-2"
            :datetime="formattedDate(item.date)"
          >
            {{ formattedDateForDisplay(item.date) }}
          </time>
          <span class="WhatsNew-list-item-anchor-link">
            {{ item.text }}
            <v-icon
              v-if="!isInternalLink(item.url)"
              class="WhatsNew-item-ExternalLinkIcon"
              size="12"
            >
              mdi-open-in-new
            </v-icon>
          </span>
        </a>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import {
  convertDateByCountryPreferTimeFormat,
  convertDateToISO8601Format
} from '@/utils/formatDate'

export default Vue.extend({
  props: {
    items: {
      type: Array,
      required: true
    }
  },
  methods: {
    isInternalLink(path: string): boolean {
      return !/^https?:\/\//.test(path)
    },
    formattedDate(dateString: string) {
      return convertDateToISO8601Format(dateString)
    },
    formattedDateForDisplay(dateString: string) {
      return convertDateByCountryPreferTimeFormat(dateString, this.$i18n.locale)
    }
  }
})
</script>

<style lang="scss">
.alert {
  padding: 8px;
  color: #f00;
  font-size: 12px;
}
.alert a:link,
.alert a:visited,
.alert a:hover,
.alert a:active {
  color: #f00;
}

.WhatsNew {
  @include card-container();

  padding: 10px;
  margin-bottom: 20px;
}

.WhatsNew-heading {
  display: flex;
  align-items: center;

  @include card-h2();

  margin-bottom: 12px;
  color: $gray-2;
  margin-left: 12px;

  &-icon {
    margin: 3px;
  }
}

.WhatsNew .WhatsNew-list {
  padding-left: 0;
  list-style-type: none;

  &-item {
    &-anchor {
      text-decoration: none;
      margin: 5px;
      font-size: 14px;

      @include lessThan($medium) {
        display: flex;
        flex-wrap: wrap;
      }

      &-time {
        flex: 0 0 90px;

        @include lessThan($medium) {
          flex: 0 0 100%;
        }

        color: $gray-1;
      }

      &-link {
        flex: 0 1 auto;

        @include text-link();

        @include lessThan($medium) {
          padding-left: 8px;
        }
      }

      &-ExternalLinkIcon {
        margin-left: 2px;
        color: $gray-3 !important;
      }
    }
  }
}
</style>
