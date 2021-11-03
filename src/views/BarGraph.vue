<script>
import { Bar } from "vue-chartjs";

export default {
  extends: Bar,
  name: "BarGraph",
  props: {
    graphdata: {
      type: Object,
      default: null,
    },
    options: {
      type: Object,
      default: null,
    },
  },
  mounted () {
      this.renderChart(this.graphdata, this.options);
  }
};
</script>