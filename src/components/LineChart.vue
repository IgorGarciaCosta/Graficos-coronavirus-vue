<script>
import { Line } from "vue-chartjs";
export default {
  extends: Line,
  props: {
    label: {
      type: String,
    },
    chartData: {
      type: Array,
    },
    options: {
      type: Object,
    },
  },

  mounted() {
    //usa o reverse pq a API manda os dados em ordem decresc. e quero em ordem crescente
    const dates = this.chartData.map((d) => d.date).reverse();
    const totals = this.chartData.map((d) => d.total).reverse();

    this.renderChart(
      {
        labels: dates,
        datasets: [
          {
            label: this.label,
            data: totals,
          },
        ],
      },
      this.options
    );
  },
};
</script>