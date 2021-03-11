<template>
  <div class="hello">
    <a-button type="primary" @click="showModal">
      {{chartData.length>1? "Añadir al gráfico" : "Crear gráfico"}}
      
    </a-button>
    <a-button  @click="cleanChart">
      Limpiar
    </a-button>
    <a-modal v-model="visible" okText= "Terminar" cancelText= "Cancelar" title="Basic Modal" @ok="handleOk">
      <a-form
        :form="form"
        :label-col="{ span: 5 }"
        :wrapper-col="{ span: 12 }"
        @submit="handleSubmit"
      >
        <a-form-item label="Campaña">
          <a-input
            v-decorator="[
              'bell',
              {
                rules: [{ required: true, message: 'Please input your note!' }],
              },
            ]"
          />
        </a-form-item>
        <a-form-item label="Conversión">
          <a-input
            v-decorator="[
              'conversion',
              {
                rules: [
                  { required: true, message: 'Please input your gender!' },
                  { validator: checkConversion }
                ],
              },
            ]"
          />
          
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 12, offset: 5 }">
          <a-button type="primary" html-type="submit">
            Añadir
          </a-button>
        </a-form-item>
      </a-form>
    </a-modal>
    <div >
      <GChart 
      class="chart"
      type="PieChart"
      :data="chartData"
      :options="chartOptions"
    />
    </div>
    
    <!-- <div :v-if="chartData.length=0">
      No hay datos
    </div> -->
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      // Array will be automatically processed with visualization.arrayToDataTable function
      chartData: [
        ["Task", "Hours per Day"], 
      ],
      chartOptions: {
        chart: {
          title: "Conversiones por campaña",
          
        },
        pieHole: 0.4,
      },
      visible: false,
      formLayout: "horizontal",
      form: this.$form.createForm(this, { name: "coordinated" }),
    };
  },
  methods: {
    showModal() {
      this.visible = true;
    },
    cleanChart() {
      this.chartData = [["Task", "Hours per Day"]]
    },
    handleOk(e) {
      console.log(e);
      this.visible = false;
    },
    handleSubmit(e) {
      
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log("Received values of form: ", values);
          this.chartData.push([values.bell, Number(values.conversion)])
        }
      });
      e.preventDefault();
    },
    
    checkConversion(rule, value, callback) {
      console.log(value)
      if (value > 0) {
        callback();
        return;
      }
      callback('El valor tiene que ser un número suprior a 0!');
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.chart {
  min-height: 400px;
}
</style>
