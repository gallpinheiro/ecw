<style>

  body {
    background: #FFF3E0;
    overflow-y: auto;
  }

  .canvas-container {
    text-align: center;
  }

  canvas {
    display: inline;
  }

</style>

<template>
  <div class="container">

  <div class="row" style="margin-top: 30px;">
    <div class="col d-flex justify-content-center">
      <div class="card" style="width: 24rem;">
            
        <div class="canvas-container"><canvas id="anions-canvas" width="250" height="250"></canvas></div>
        <template v-if="anionId">{{draw(asmiles[anionId], 'anions-canvas')}}</template>

        <div class="card-header">
          <div class="form-group row">
            <label for="anion" class="col-sm-3 col-form-label">Anion ID</label>
            <div class="col-sm-9">
              <input type="number" class="form-control" id="anion" placeholder="Anion ID" min=0 max=1000 v-model="anionId">
            </div>
          </div>

        </div>

        <ul class="list-group list-group-flush">
          <li class="list-group-item">
            <template v-if="anionId"> 
              <strong> SMILES: </strong> {{asmiles[anionId]}}
            </template>
            <template v-else>Choose an anion to print its SMILES here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="anionId"> 
              <strong> HOMO: </strong> {{ahomo[anionId]}}
            </template>
            <template v-else>Choose an anion to print its HOMO value here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="anionId"> 
              <strong> LUMO: </strong> {{alumo[anionId]}}
            </template>
            <template v-else>Choose an anion to print its LUMO value here.</template>
          </li>
        </ul>
      </div>
    </div>

    <div class="col d-flex justify-content-center">

      <div class="card" style="width: 24rem;">
            
        <div class="canvas-container"><canvas id="cations-canvas" width="250" height="250"></canvas></div>
        <template v-if="cationId">{{draw(csmiles[cationId], 'cations-canvas')}}</template>

        <div class="card-header">
          <div class="form-group row">
            <label for="cation" class="col-sm-3 col-form-label">Cation ID</label>
            <div class="col-sm-9">
              <input type="number" class="form-control" id="cation" placeholder="Cation ID" min=0 max=1000 v-model="cationId">
            </div>
          </div>

        </div>

        <ul class="list-group list-group-flush">
          <li class="list-group-item">
            <template v-if="cationId"> 
              <strong> SMILES: </strong> {{csmiles[cationId]}}
            </template>
            <template v-else>Choose an anion to print its SMILES here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="cationId"> 
              <strong> HOMO: </strong> {{chomo[cationId]}}
            </template>
            <template v-else>Choose a cation to print its HOMO value here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="cationId"> 
              <strong> LUMO: </strong> {{clumo[cationId]}}
            </template>
            <template v-else>Choose a cation to print its LUMO value here.</template>
          </li>
        </ul>

      </div>

    </div>
  </div>
  
  <div class="row d-flex justify-content-center" style="margin-top: 30px;">    
    <template v-if="anionId && cationId"> {{ecw()}} </template>
    <template v-else>Oh, no. 😢 Please, provide the anion and cation indices. </template>
  </div>

  </div>
</template>

<script>
import _ from 'lodash';
import BootstrapVue from 'bootstrap-vue/dist/bootstrap-vue.esm';
import 'bootstrap-vue/dist/bootstrap-vue.css';
import 'bootstrap/dist/css/bootstrap.css';


export default {
  name: 'IndexPage',
  mounted() {

    this.$axios.get('/anions.json').then((response) => {
      this.ahomo  = _.map(response.data, (el) => {return el['homo'];});
      this.alumo  = _.map(response.data, (el) => {return el['lumo'];});
      this.asmiles  = _.map(response.data, (el) => {return el['smiles'];});
    });

    this.$axios.get('/cations.json').then((response) => {
      this.chomo  = _.map(response.data, (el) => {return el['homo'];});
      this.clumo  = _.map(response.data, (el) => {return el['lumo'];});
      this.csmiles  = _.map(response.data, (el) => {return el['smiles'];});
    });

  },

  data() {
    return {
      anionId: undefined,
      cationId: undefined,
      ahomo: undefined,
      alumo: undefined,
      asmiles: undefined,
      chomo: undefined,
      clumo: undefined,
      csmiles: undefined,
    }
  },

  methods: {
    ecw(){
      let ecw_value = (Math.min(this.alumo[this.anionId], this.clumo[this.cationId]) + 3.0915315909677425) - (Math.max(this.ahomo[this.anionId], this.chomo[this.cationId]) - 3.4620853512903227)
      if (ecw_value >= 0){return 'ECW: ' + ecw_value;}
      return 'ECW is negative. 😢';
    },
    draw(smi, view) {

      let smilesDrawer = new SmilesDrawer.Drawer({debug: false, atomVisualization: 'default', width: 250, height: 250})
      SmilesDrawer.parse(smi, function (tree) {
          smilesDrawer.draw(tree, view, 'light', false);
      });

    },
  },

}
</script>