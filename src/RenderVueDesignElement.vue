<script>
import {ButtonSubmit, DatatableHeader, DatatableSearch} from 'bedrock-core-vue';

export default {
  components: {
    ButtonSubmit,
    DatatableSearch,
    DatatableHeader,
  },

  props: {
    rawVue: {
      type: String,
      required: true,
    },

    info: {
      type: String,
      required: false,
    },
  },

  created() {
    this.$options.template = this.rawVue;
  },

  data() {
    return {
      showCode: false,

      showInfo: false,
    };
  },
};
</script>
