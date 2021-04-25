<template>
  <v-card class="datatable">
    <datatable-header :title="$t('bedrock-development.routes.index.title')">
      <template v-slot:search>
        <datatable-search :search-input.sync="searchInput"></datatable-search>
      </template>

      <template v-slot:filter_01>
        <datatable-filter :selected.sync="methodSelected" :options="filterMethods">
        </datatable-filter>
      </template>

      <template v-slot:filter_02>
        <v-checkbox v-model="filterWithoutPermissions" :label="$t('bedrock-development.routes.without_permissions')">
        </v-checkbox>
      </template>
    </datatable-header>

    <v-data-table
      :headers="headers"
      :items="datatableValues"
      :search="searchData"
      :custom-filter="filter"
      :items-per-page="10"
      item-key="index"
      single-select
      class="elevation-1"
      show-expand
    >
      <template v-slot:[`item.methods`]="{ item }">
        <v-chip
          v-if="getHttpMethodsText(item.methods, 'GET')"
          :color="getHttpMethodsColor(item.methods, 'GET')"
          x-small
          outlined
          >{{ getHttpMethodsText(item.methods, 'GET') }}
        </v-chip>

        <v-chip
          v-if="getHttpMethodsText(item.methods, 'DELETE')"
          :color="getHttpMethodsColor(item.methods, 'DELETE')"
          x-small
          outlined
          >{{ getHttpMethodsText(item.methods, 'DELETE') }}
        </v-chip>

        <v-chip
          v-if="getHttpMethodsText(item.methods, 'POST')"
          :color="getHttpMethodsColor(item.methods, 'POST')"
          x-small
          outlined
          >{{ getHttpMethodsText(item.methods, 'POST') }}
        </v-chip>
        <v-chip
          v-if="getHttpMethodsText(item.methods, 'PATCH')"
          :color="getHttpMethodsColor(item.methods, 'PATCH')"
          x-small
          outlined
          >{{ getHttpMethodsText(item.methods, 'PATCH') }}
        </v-chip>
        <v-chip
          v-if="getHttpMethodsText(item.methods, 'PUT')"
          :color="getHttpMethodsColor(item.methods, 'PUT')"
          x-small
          outlined
          >{{ getHttpMethodsText(item.methods, 'PUT') }}
        </v-chip>

        <!--            <v-chip-->
        <!--              v-if="getHttpMethodsText(item.methods, 'HEAD')"-->
        <!--              :color="getHttpMethodsColor(item.methods, 'HEAD')"-->
        <!--              x-small-->
        <!--              outlined-->
        <!--              >{{ getHttpMethodsText(item.methods, 'HEAD') }}-->
        <!--            </v-chip>-->
      </template>

      <template v-slot:[`item.middleware`]="{ item }">
        <span v-html="item.middleware" style="white-space: nowrap"></span>
      </template>

      <template v-slot:expanded-item="{ headers, item }">
        <td :colspan="headers.length">
          Middleware: {{ item.middleware }} <br />
          Methods: {{ item.methods }}<br />
          Controller: {{ item.controller }}<br />
        </td>
        <td :colspan="headers.length"></td>
      </template>

      <!--            <template v-slot:item.meta.text_status="{ item }">-->
      <!--                <td style="display: none"></td>-->
      <!--            </template>-->
      <!--            <template v-slot:item.meta.review_status="{ item }">-->
      <!--                <td style="display: none"></td>-->
      <!--            </template>-->
      <!--            <template v-slot:item.dummy="{ item }">-->
      <!--                <td style="background-color: red;
          padding-left: 0px !important; padding-right: 0px !important; "></td>-->
      <!--            </template>-->

      <!--          <template v-slot:item.icons="{ item }">-->
      <!--            <div class="d-flex">-->
      <!--              <div-->
      <!--                :style="`color:${$t(-->
      <!--    `multilingual-admin.textstatus.status.${item.meta.text_status}.color`,-->
      <!--                )}`"-->
      <!--              >-->
      <!--                <span-->
      <!--                  :class="-->
      <!--$t(`multilingual-admin.textstatus.status.${item.meta.text_status}.fa_icon`)-->
      <!--                  "-->
      <!--                ></span>-->
      <!--                &nbsp;-->
      <!--              </div>-->
      <!--              <div-->
      <!--                :style="`color:${$t(-->
      <!-- `multilingual-admin.reviewstatus.status.${item.meta.review_status}.color`,-->
      <!--                )}`"-->
      <!--              >-->
      <!--                <span-->
      <!--                  :class="-->
      <!-- $t(`multilingual-admin.reviewstatus.status.${item.meta.review_status}.fa_icon`)-->
      <!--                  "-->
      <!--                ></span>-->
      <!--              </div>-->
      <!--            </div>-->
      <!--          </template>-->
    </v-data-table>
  </v-card>
</template>

<script>
import {Datatable, DatatableFilter, DatatableHeader, DatatableSearch} from 'bedrock-core-vue';

export default {
  extends: Datatable,

  components: {
    DatatableHeader,
    DatatableFilter,
    DatatableSearch,
  },

  props: {
    title: {
      type: String,
    },
    datatableValues: {
      type: Array,
    },
    filterMethods: {
      type: Array,
    },
  },

  data() {
    return {
      headers: null,
      methodSelected: null,
      filterWithoutPermissions: false,
    };
  },

  created() {
    this.setHeaders();
  },

  methods: {
    hasFilter() {
      if (this.methodSelected || this.filterWithoutPermissions) {
        return true;
      }
      return false;
    },

    applyFilters(item) {
      if (this.filterWithoutPermissions) {
        if (item.permission !== 0) {
          return false;
        }
        return true;
      }

      if (item.methods.search(this.methodSelected) === -1) {
        return false;
      }

      return true;
    },

    getHttpMethodsText(methods, method) {
      return methods.includes(method) ? method : '';
    },

    getHttpMethodsColor(methods, method) {
      if (methods.includes(method)) {
        if (method === 'GET') {
          return 'blue';
        }

        if (method === 'POST') {
          return 'green';
        }
        if (method === 'PATCH') {
          return 'green';
        }
        if (method === 'PUT') {
          return 'green';
        }

        if (method === 'DELETE') {
          return 'red';
        }
        if (method === 'HEAD') {
          return 'grey';
        }
      }

      return 'grey';
    },

    setHeaders() {
      this.headers = [
        {
          text: this.$t('bedrock-development.routes.field.url.label'),
          value: 'uri',
        },
        {
          text: this.$t('bedrock-development.routes.field.methods.label'),
          value: 'methods',
        },
        {
          text: this.$t('bedrock-development.routes.field.middleware.label'),
          value: 'middleware',
          width: 100,
        },
        {
          text: this.$t('bedrock-development.routes.field.name.label'),
          value: 'name',
        },
        {
          text: this.$t('bedrock-development.routes.field.controller.label'),
          value: 'controller',
        },
        {
          text: this.$t('bedrock-development.routes.field.prefix.label'),
          value: 'prefix',
        },
        {
          text: this.$t('bedrock-development.routes.field.has_permissions.label'),
          value: 'permission',
        },
        {
          text: 'd',
          value: 'dummy',
          class: 'hidden',
          width: '1px',
          align: ' d-none',
        } /* add dummy column to be able to additional default filtering on status */,
      ];
    },
  },
};
</script>

<style scoped>
/*!* Hard hide the column of the DUMMY data *!*/
/*::v-deep .v-data-table__wrapper thead tr th:nth-of-type(7) {*/
/*    display:none;*/
/*}*/
/*::v-deep .v-data-table__wrapper tr td:nth-of-type(7) {*/
/*    display:none;*/
/*}*/

::v-deep .v-data-table__wrapper thead tr th.hidden {
  display: none;
}

::v-deep .v-card__title.danger {
  background-color: red;
}

::v-deep tr.v-data-table__selected {
  background: #7d92f5 !important;
}

::v-deep tr.v-data-table__expanded__content {
  background: #c2c0c2 !important;
  border: none !important;
  box-shadow: none !important;
}
</style>
