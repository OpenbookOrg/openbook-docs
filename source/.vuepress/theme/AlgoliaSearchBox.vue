<template lang="pug">
  form#search-form.algolia-search-wrapper.search-box
    input#algolia-search-input.search-query
</template>

<script>
import docsearch from 'docsearch.js';

export default {
  props: ['options'], /* eslint-disable-line vue/require-prop-types */

  watch: {
    $lang (newValue) {
      this.update(this.options, newValue);
    },

    options (newValue) {
      this.update(newValue, this.$lang);
    }
  },

  mounted () {
    this.initialize(this.options, this.$lang);
  },

  methods: {
    initialize (userOptions, lang) {
      const { algoliaOptions = {} } = userOptions;
      docsearch(Object.assign({}, userOptions, {
        inputSelector: '#algolia-search-input',
        algoliaOptions: Object.assign({
          'facetFilters': [`lang:${lang}`].concat(algoliaOptions.facetFilters || [])
        }, algoliaOptions)
      }));
    },

    update (options, lang) {
      this.$el.innerHTML = '<input id="algolia-search-input" class="search-query">';
      this.initialize(options, lang);
    }
  }
};
</script>

<style lang="scss">
@import './styles/colors';
@import './styles/variables';

@import '~docsearch.js/dist/cdn/docsearch.min.css';

.algolia-search-wrapper {
  & > span {
    vertical-align: middle;
  }

  .algolia-autocomplete {
    line-height: normal;

    .ds-dropdown-menu {
      background-color: #fff;
      border: 1px solid #999;
      border-radius: 4px;
      font-size: 16px;
      margin: 6px 0 0;
      padding: 4px;
      text-align: left;

      &::before {
        border-color: #999;
      }

      [class*="ds-dataset-"] {
        border: none;
        padding: 0;
      }

      .ds-suggestions {
        margin-top: 0;
      }

      .ds-suggestion {
        border-bottom: 1px solid $border-color;
      }
    }

    .algolia-docsearch-suggestion--highlight {
      color: #2c815b;
    }

    .algolia-docsearch-suggestion {
      border-color: $border-color;
      padding: 0;

      .algolia-docsearch-suggestion--category-header {
        padding: 5px 10px;
        margin-top: 0;
        background: $accent;
        color: #fff;
        font-weight: 600;

        .algolia-docsearch-suggestion--highlight {
          background: rgba(255, 255, 255, 0.6);
        }
      }

      .algolia-docsearch-suggestion--wrapper {
        padding: 0;
      }

      .algolia-docsearch-suggestion--title {
        font-weight: 600;
        margin-bottom: 0;
        color: $text-color;
      }

      .algolia-docsearch-suggestion--subcategory-column {
        vertical-align: top;
        padding: 5px 7px 5px 5px;
        border-color: $border-color;
        background: #f1f3f5;

        &::after {
          display: none;
        }
      }

      .algolia-docsearch-suggestion--subcategory-column-text {
        color: #555;
      }
    }

    .algolia-docsearch-footer {
      border-color: $border-color;
    }

    .ds-cursor .algolia-docsearch-suggestion--content {
      background-color: #e7edf3 !important;
      color: $text-color;
    }
  }
}

@media ( min-width: $mq-mobile ) {
  .algolia-search-wrapper {
    .algolia-autocomplete {
      .algolia-docsearch-suggestion {
        .algolia-docsearch-suggestion--subcategory-column {
          float: none;
          width: 150px;
          min-width: 150px;
          display: table-cell;
        }

        .algolia-docsearch-suggestion--content {
          float: none;
          display: table-cell;
          width: 100%;
          vertical-align: top;
        }

        .ds-dropdown-menu {
          min-width: 515px !important;
        }
      }
    }
  }
}

@media ( max-width: $mq-mobile ) {
  .algolia-search-wrapper {
    .ds-dropdown-menu {
      min-width: calc(100vw - 4rem) !important;
      max-width: calc(100vw - 4rem) !important;
    }

    .algolia-docsearch-suggestion--wrapper {
      padding: 5px 7px 5px 5px !important;
    }

    .algolia-docsearch-suggestion--subcategory-column {
      padding: 0 !important;
      background: white !important;
    }

    .algolia-docsearch-suggestion--subcategory-column-text::after {
      content: ' > ';
      font-size: 10px;
      line-height: 14.4px;
      display: inline-block;
      width: 5px;
      margin: -3px 3px 0;
      vertical-align: middle;
    }
  }
}
</style>
