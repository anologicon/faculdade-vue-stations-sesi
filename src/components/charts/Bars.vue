<script>
import { Bar } from 'vue-chartjs'

export default {
  name: 'Bars',
  props: ['modelData'],
  data() {
    return {
      dataModel: this.modelData,
    }
  },
  extends: Bar,
  mounted () {
    // Overwriting base render method with actual data.
    this.renderChart(this.dataModel);
    console.log(this.dataModel);
    
  },
}
</script>
