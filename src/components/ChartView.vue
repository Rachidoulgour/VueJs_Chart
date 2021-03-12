<template>
  <div class="chart-view">
    <div class="chart-section">
      <div class="chart-header">
        <div class="title">
          <h3>Conversiones por Campaña</h3>
        </div>
        <div class="chart-btn">
          <a-button type="primary" @click="showModal">
            {{ chartData.length > 1 ? "Añadir campaña" : "Crear gráfico" }}
          </a-button>
          <a-button @click="cleanChart">
            Limpiar
          </a-button>
        </div>
      </div>
      <a-modal
        v-model="visible"
        okText="Terminar"
        cancelText="Cancelar"
        title="Añadir Campaña"
        @ok="handleOk"
      >
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
                  rules: [
                    {
                      required: true,
                      message: 'Por favor, introduzca la campañia!',
                    },
                  ],
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
                    {
                      required: true,
                      message: 'Por favor, introduzca la conversión!',
                    },
                    { validator: checkConversion },
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
      <div>
        <GChart
          class="chart"
          type="PieChart"
          :data="chartData"
          :options="chartOptions"
        />
      </div>

      
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      // Array will be automatically processed with visualization.arrayToDataTable function
      chartData: [["Task", "Hours per Day"]],
      chartOptions: {
        // chart: {
        //   title: "Conversiones por campaña",
        // },
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
      this.chartData = [["Task", "Hours per Day"]];
    },
    handleOk(e) {
      console.log(e);
      this.visible = false;
    },
    handleSubmit(e) {
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log("Received values of form: ", values);
          this.chartData.push([values.bell, Number(values.conversion)]);
          values.bell = " ";
          console.log(values);
        }
      });
      e.preventDefault();
    },

    checkConversion(rule, value, callback) {
      if (value > 0) {
        callback();
        return;
      }
      callback("El valor tiene que ser un número suprior a 0!");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.chart-view {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: solid 1px rgb(233, 234, 235);
  border-radius: 5px;
  box-shadow: 0 5px 5px rgb(157, 163, 162);
  width: 90%;
  height: 400px;
  margin: 0;
  padding: 10px;
  background-color: rgb(233, 234, 235);
}
.chart-section {
  height: 100%;
  border: solid 1px rgb(233, 234, 235);
  border-radius: 5px;
  width: 100%;
  background-color: white;
  
}
.chart-header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
h3 {
  margin: 5px 10px;
  font-weight: bold;
  color: rgba(0, 0, 0, 0.65);
}
.chart-btn {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  margin: 5px;
}
.ant-btn {
  margin-left: 5px;
}
.chart {
  min-height: 260px;
  width: 90%;
}
</style>
