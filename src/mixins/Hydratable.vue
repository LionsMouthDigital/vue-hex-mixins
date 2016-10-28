<script>
  // Stolen from Jack McDade.
  export default {
    props: {
      endpoint: {
        type:    [String, Boolean],
        default: false,
      },

      preload: {
        type:    [String, Boolean],
        default: false,
      },
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
        // Parse preloaded data into object if it's JSON.
        window.hydratablePreload = typeof window.hydratablePreload === 'string'
          ? JSON.parse(window.hydratablePreload)
          : window.hydratablePreload;

        // Use preloaded data from a specific key if a string was provided in the `preload` prop,
        // otherwise just use the whole array.
        this.items = (typeof this.preload === 'string')
          ? window.hydratablePreload
          : window.hydratablePreload[this.preload];

        this.loading = false;
      }
    },


    created() {
      this.hydrate();
    }
  }
</script>
