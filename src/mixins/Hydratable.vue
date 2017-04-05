<script>
  // Original concept stolen from Jack McDade.
  export default {
    props: {
      endpoint: {
        type:    [String, Boolean],
        default: false,
      },

      hydrateKey: String,
    },


    data() {
      return {
        loading: true,
        items:   [],
      };
    },


    methods: {
      hydrate() {
        (this.endpoint) ? this.hydrateFromEndpoint() : this.hydrateFromPreload();
      },

      hydrateFromEndpoint() {
        this.$http.get(this.endpoint).then(function (response) {
          this.items   = response.data;
          this.loading = false;
        });
      },

      hydrateFromPreload() {
        // Use data from a specific key if provided.
        let items = this.hydrateKey
          ? window.hexHydrant[this.hydrateKey]
          : window.hexHydrant;

        // Parse preloaded data if JSON.
        this.items = typeof items === 'string' ? JSON.parse(items) : items;

        this.loading = false;
      }
    },


    created() {
      this.hydrate();
    }
  }
</script>
