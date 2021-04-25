<template>
  <div class="d-flex flex-column">
    <div class="d-flex justify-content-between">
      <h1>{{ element.title }}</h1>
      <div>
        <span class="fal fa-info-circle" @click="showInfo = !showInfo"></span>
        <span class="fal fa-code" @click="showCode = !showCode"></span>
      </div>
    </div>

    <div class="d-flex flex-column mt-3">
      <span v-if="showInfo" v-html="element.info"></span>
      <span v-if="element.raw_type === 'HTML'" v-html="element.raw"></span>
      <render-vue-design-element v-if="element.raw_type === 'VUE'" :raw-vue="element.raw">
      </render-vue-design-element>
      <pre v-if="showCode" style="background-color: #f0efed">{{ element.raw.trim() }}</pre>
    </div>
  </div>
</template>

<script>
import RenderVueDesignElement from './RenderVueDesignElement.vue';

export default {
  components: {
    RenderVueDesignElement,
  },

  props: {
    element: {
      type: Object,
      required: true,
    },
  },

  created() {
    this.$options.template = this.rawHtml;
  },

  data() {
    return {
      showCode: false,

      showInfo: false,

      state: {
        confirmTitle: this.confirmTitle,
        method: this.method,
        confirmResultMessage: '',
        successMessage: '',
      },
    };
  },

  watch: {
    method() {
      this.state.method = this.method;
      this.state.confirmTitle = this.confirmTitle;
    },
  },

  mounted() {
  },

  methods: {
    verified(response) {
      this.confirmResultMessage = '';

      if (response.message) {
        this.state.confirmResultMessage = response.message;
      }

      if (response.data && response.data.next) {
        this.state.method = response.data.next.method;
        this.state.confirmTitle = response.data.next.title;
      }

      if (response.data && response.data.finished) {
        this.state.successMessage = response.data.finished.successMessage;

        if (response.data.finished.successRedirect) {
          this.popupDialog = true;
          window.location.replace(response.data.finished.successRedirect);
        }
      }

      this.$emit('verified');
    },
  },
};
</script>
