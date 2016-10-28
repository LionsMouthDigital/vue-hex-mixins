<script>
  export default {
    data() {
      return {
        index: null,
      };
    },

    methods: {
      /**
       * Set the index of a child component.
       *
       * @return {void}
       */
      setIndex() {
        this.$parent.$children.forEach((item, i) => {
          if (item._uid === this._uid) {
            this.index = i + 1
          }
        })
      },
    },

    created() {
      this.setIndex()
    },
  }
</script>
