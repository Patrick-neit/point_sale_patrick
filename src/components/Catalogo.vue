<template>
  <div class="grid">
    <div class="col-6">
      <div class="card p-fluid">
        <h5>Formulario de Registro</h5>
        <div class="formgrid grid">
          <div class="field col-6">
            <label for="lugar">Lugar</label>
            <Dropdown
              v-model="optionsPlace"
              :options="optionsPlaces"
              optionLabel="name"
              placeholder="Seleccione un Lugar"/>
          </div>
          <div class="field col-6">
            <label for="pago">Pago</label>
            <Dropdown
              v-model="optionsPayment"
              :options="optionsPayments"
              optionLabel="name"
              placeholder="Seleccione un Tipo de Pago"
            />
          </div>
          <div class="field col-6">
            <label for="nit_ci">Nit/Ci</label>
            <InputText id="nit_ci" type="text"  placeholder="Nit/Ci"  />
          </div>
          <div class="field col-6">
            <label for="cliente">Cliente</label>
            <InputText id="cliente" type="text" placeholder="Cliente" />
          </div>
          <div class="field col-6">
            <label for="empresa">Empresa</label>
            <InputText id="empresa" type="text" placeholder="Empresa" />
          </div>
          <div class="field col-6">
            <label for="celular">Celular</label>
            <InputText id="celular" type="text" placeholder="Celular" />
          </div>
        </div>
        <div class="card-footer">
          <div class="field col-3">
            <Button label="Registrar Venta" class="mr-2 mb-2" />
          </div>
        </div>
      </div>
    </div>
    <div class="col-6">
      <div class="card">
        <h5>Platos Ofertados</h5>
        <DataView
          :value="plates"
          :layout="layout"
          :paginator="true"
          :rows="9"
          :sortOrder="sortOrder"
          :sortField="sortField"
        >
          <template #header="">
            <div class="grid grid-nogutter">
              <div class="col-6 text-left">
                <Dropdown
                  v-model="sortKey"
                  :options="sortOptions"
                  optionLabel="nombre"
                  placeholder="Seleccione una Categoria"
                  @change="onSortChange($event)"
                />
              </div>
              <div class="col-6 text-right">
                <DataViewLayoutOptions v-model="layout" />
              </div>
            </div>
          </template>
          <template #list="slotProps">
            <div class="col-12">
              <div
                class="
                  flex flex-column
                  md:flex-row
                  align-items-center
                  p-3
                  w-full
                "
              >
                <img
                  :src="ruta + '' + slotProps.data.imagen"
                  :alt="slotProps.data.nombre"
                  class="my-4 md:my-0 w-9 md:w-10rem shadow-2 mr-5"
                />
                <div class="flex-1 text-center md:text-left">
                  <div class="font-bold text-2xl">
                    {{ slotProps.data.Plato }}
                  </div>
                  <div class="flex align-items-center">
                    <i class="pi pi-tag mr-2"></i>
                  </div>
                </div>
                <div
                  class="
                    flex flex-row
                    md:flex-column
                    justify-content-between
                    w-full
                    md:w-auto
                    align-items-center
                    md:align-items-end
                    mt-5
                    md:mt-0
                  "
                >
                  <span
                    class="
                      text-2xl
                      font-semibold
                      mb-2
                      align-self-center
                      md:align-self-end
                    "
                    >Bs. {{ slotProps.data.Precio }}</span
                  >
                  <Button
                    icon="pi pi-shopping-cart"
                    label="Add to Cart"
                  ></Button>
                </div>
              </div>
            </div>
          </template>
          <template #grid="slotProps">
            <div class="col-12 md:col-4">
              <div class="card m-3 border-1 surface-border">
                <div class="text-center">
                  <img
                    :src="ruta + '' + slotProps.data.imagen"
                    :alt="slotProps.data.nombre"
                    class="w-9 shadow-2 my-3 mx-0"
                  />
                  <div class="text-1xl font-bold">
                    {{ slotProps.data.Plato }}
                  </div>
                </div>
                <div class="flex align-items-center justify-content-between">
                  <span class="text-1xl font-semibold product-badge"
                    >Bs. {{ slotProps.data.Precio }}</span
                  >
                  <Button
                    icon="pi pi-shopping-cart "
                    style="font-size: 0.001rem"
                    v-on:click="addPlate(slotProps.data)"
                  ></Button>
                </div>
              </div>
            </div>
          </template>
        </DataView>
      </div>
    </div>
    <div class="col-12">
      <div class="card">
        <h5>Detalle de Venta</h5>
        <DataTable
          :value="carrito"
          :scrollable="false"
          scrollHeight="500px"
          :loading="loading2"
          scrollDirection="both"
          class="mt-2"
        >
          <Column
            field="plato"
            header="Plato"
            :style="{ width: '150px' }"
            frozen
          ></Column>
          <Column
            field="cantidad"
            header="Cantidad"
            :style="{ width: '150px' }"
          >
          
            <template #body="slotProps">
              <input
                type="number"
                :value="slotProps.data.cantidad"
                style="width: 100%"
                v-on:keyup="keyupNitCi(slotProps.data.cantidad,slotProps.data.Precio)"
              />
            </template>
          </Column>
          <Column field="costo" header="Costo" :style="{ width: '150px' }">
          </Column>
          <Column
            field="subtotal"
            header="Sub Total"
            :style="{ width: '150px' }"
          ></Column>
          <template #groupfooter="slotProps">
            <td style="text-align: right" class="text-bold pr-6">
              Total Customers: {{ totalVents(slotProps.data.cantidad) }}
            </td>
          </template>
        </DataTable>

        <div class="card">
          <div class="grid">
            <div class="col-10">
              <p>Total :</p>
            </div>
            <div class="col-2">
              <p class="text-right">0 bs.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProductService from "../service/ProductService";
import axios from "axios";
//:src="ruta+''+ plate.imagen"
export default {
    data() {
      return {
        optionsPlace: null,
        optionsPlaces: [
          { name: "En Restaurante" },
          { name: "Para Llevar" },
          { name: "Venta Externa" },
          { name: "Delivery" },
        ],
        optionsPayment: null,
        optionsPayments: [
          { name: "Efectivo" },
          { name: "Tarjeta Credito/Debito" },
          { name: "Comida Personal" },
        ],
        dataviewValue: null,
        layout: "grid",
        sortKey: null,
        sortOrder: null,
        sortField: null,
        sortOptions: [],
        plates: "",
        ruta: "http://192.168.0.150/eerpwebv2/public/",
        carrito: [],
        subtotal: 0,
        subtotales: [],
      };
    },
    productService: null,
    created() {
      this.productService = new ProductService();
    },

    mounted() {
      this.productService
        .getProducts()
        .then((data) => (this.dataviewValue = data));
      this.getCategorias();
    },
    updated() {
      console.log(this.subtotal);
    },
    methods: {
      eventsSubtotal(){
        
      },
      keyupNitCi(cantidad,precio){

        console.log(cantidad);
        console.log(precio);
      },
      calculateSubtotal(cantidad, precio) {
        console.log(cantidad, precio);
      },
      onSortChange(event) {
        let id = event.value.id;
        this.getPlates(id);
      },
      getCategorias() {
        //axios.get("http://192.168.0.150/eerpwebv2/public/api/getPlates?"+ 'categoria_plato_id=3')
        let result = axios
          .get("http://192.168.0.150/eerpwebv2/public/api/getCategories")
          .then((res) => {
            this.sortOptions = res.data.categorias;
          })
          .catch((err) => {
            console.log(err);
          });
      },

      getPlates(id) {
        //axios.get("http://192.168.0.150/eerpwebv2/public/api/getPlates?"+ 'categoria_plato_id=3')
        let result = axios
          .get(
            "http://192.168.0.150/eerpwebv2/public/api/getPlates?" +
              "categoria_plato_id=" +
              id +
              "&sucursal_id=" +
              3
          )
          .then((res) => {
            if (res.data.plate.length > 0) {
              this.plates = res.data.plate;
            } else {
              this.plates = null;
            }
          })
          .catch((err) => {
            console.log(err);
          });
      },
      addPlate(data) {
        console.log(data);
        this.carrito.push({
          plato: data.Plato,
          cantidad: 0,
          costo: data.Precio,
          subtotal: 0,
        });
      },
      
      totalVents(price) {
        return price + 1;
      },

      digitar(){
        console.log("entro al digitar");
      }
    },
};
</script>

<style scoped lang="scss">
@import "../assets/demo/badges.scss";
</style>
