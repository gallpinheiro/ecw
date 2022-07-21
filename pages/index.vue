<style>

  body {
    background: #F9FBE7;
    overflow-y: auto;
  }

  .canvas-container {
    text-align: center;
  }

  canvas {
    display: inline;
  }

  .list-group {
    margin-bottom: 25px
  }

  .card-header {
    background-color: #00695C;
  }

  .form-control, .list-group-item {
    border: 1px solid #00695C;
  }

  .col-form-label{
    color: white;
  }

</style>

<template>
  
  <div class="container">
  <h1 style="color: #00695C; margin-top: 30px">ESW Calculator</h1>
  <div class="row" style="margin-top: 30px;">
    <div class="col d-flex justify-content-center">
      <div class="card" style="width: 24rem; border: 1px solid #00695C; border-radius: 1.5rem">
            
        <div class="canvas-container"><canvas id="anions-canvas" width="250" height="250"></canvas></div>
        <template v-if="(anionId > 0) & (anionId <= 7444)">{{draw(asmiles[anionId - 1], 'anions-canvas')}}</template>
        <template v-else>{{clean('anions-canvas')}}</template>

        <div class="card-header">
          <div class="form-group row">
            <label for="anion" class="col-sm-3 col-form-label">Anion ID</label>
            <div class="col-sm-9">
              <input type="number" class="form-control" id="anion" placeholder="Insert ID here" min="0" max="7444" v-model="anionId">
            </div>
          </div>

        </div>

        <ul class="list-group list-group-flush">
          <li class="list-group-item">
            <template v-if="(anionId > 0) & (anionId <= 7444)"> 
              <strong> SMILES: </strong> {{asmiles[anionId - 1]}}
            </template>
            <template v-else>Choose an anion to print its SMILES here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(anionId > 0) & (anionId <= 7444)"> 
              <strong> Cores: </strong> {{acores[anionId - 1]}}
            </template>
            <template v-else>Choose an anion to print its core here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(anionId > 0) & (anionId <= 7444)"> 
              <strong> Backbones: </strong> {{abackbones[anionId - 1]}}
            </template>
            <template v-else>Choose an anion to print its backbones here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(anionId > 0) & (anionId <= 7444)"> 
              <strong> HOMO: </strong> {{ahomo[anionId - 1]}} eV
            </template>
            <template v-else>Choose an anion to print its HOMO here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(anionId > 0) & (anionId <= 7444)"> 
              <strong> LUMO: </strong> {{alumo[anionId - 1]}} eV
            </template>
            <template v-else>Choose an anion to print its LUMO here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(anionId > 0) & (anionId <= 7444)"> 
              <strong> Total energy: </strong> {{aetot[anionId - 1]}} eV
            </template>
            <template v-else>Choose an anion to print its total energy here.</template>
          </li>
        </ul>
      </div>
    </div>

    <div class="col d-flex justify-content-center">

      <div class="card" style="width: 24rem; border: 1px solid #00695C; border-radius: 1.5rem">
            
        <div class="canvas-container"><canvas id="cations-canvas" width="250" height="250"></canvas></div>
        <template v-if="(cationId > 0) & (cationId <= 9172)">{{draw(csmiles[cationId - 1], 'cations-canvas')}}</template>
        <template v-else>{{clean('cations-canvas')}}</template>

        <div class="card-header">
          <div class="form-group row">
            <label for="cation" class="col-sm-3 col-form-label">Cation ID</label>
            <div class="col-sm-9">
              <input type="number" class="form-control" id="cation" placeholder="Insert ID here" min="0" max="9172" v-model="cationId">
            </div>
          </div>

        </div>

        <ul class="list-group list-group-flush">
          <li class="list-group-item">
            <template v-if="(cationId > 0) & (cationId <= 9172)"> 
              <strong> SMILES: </strong> {{csmiles[cationId - 1]}}
            </template>
            <template v-else>Choose a cation to print its SMILES here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(cationId > 0) & (cationId <= 9172)"> 
              <strong> Cores: </strong> {{ccores[cationId - 1]}}
            </template>
            <template v-else>Choose a cation to print its core here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(cationId > 0) & (cationId <= 9172)"> 
              <strong> Backbones: </strong> {{cbackbones[cationId - 1]}}
            </template>
            <template v-else>Choose a cation to print its backbones here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(cationId > 0) & (cationId <= 9172)"> 
              <strong> HOMO: </strong> {{chomo[cationId - 1]}} eV
            </template>
            <template v-else>Choose a cation to print its HOMO here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(cationId > 0) & (cationId <= 9172)"> 
              <strong> LUMO: </strong> {{clumo[cationId - 1]}} eV
            </template>
            <template v-else>Choose a cation to print its LUMO here.</template>
          </li>
          <li class="list-group-item">
            <template v-if="(cationId > 0) & (cationId <= 9172)"> 
              <strong> Total energy: </strong> {{cetot[cationId - 1]}} eV
            </template>
            <template v-else>Choose a cation to print its total energy here.</template>
          </li>
        </ul>

      </div>

    </div>
  </div>
  
  <div class="row d-flex justify-content-center" style="margin-top: 30px;">    
    <template v-if="anionId && cationId">{{ecw()}}</template>
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
      this.ahomo       = _.map(response.data, (el) => {return el['homo'];});
      this.alumo       = _.map(response.data, (el) => {return el['lumo'];});
      this.aetot       = _.map(response.data, (el) => {return el['etot'];});
      this.acores      = _.map(response.data, (el) => {return el['cores'];});
      this.abackbones  = _.map(response.data, (el) => {return el['backbones'];});
      this.asmiles     = _.map(response.data, (el) => {return el['smiles'];});
    });

    this.$axios.get('/cations.json').then((response) => {
      this.chomo       = _.map(response.data, (el) => {return el['homo'];});
      this.clumo       = _.map(response.data, (el) => {return el['lumo'];});
      this.cetot       = _.map(response.data, (el) => {return el['etot'];});
      this.ccores      = _.map(response.data, (el) => {return el['cores'];});
      this.cbackbones  = _.map(response.data, (el) => {return el['backbones'];});
      this.csmiles     = _.map(response.data, (el) => {return el['smiles'];});
    });

  },

  data() {
    return {
      anionId: undefined,
      cationId: undefined,
      ahomo: undefined,
      alumo: undefined,
      aetot: undefined,
      acores: undefined,
      abackbones: undefined,
      asmiles: undefined,
      chomo: undefined,
      clumo: undefined,
      cetot: undefined,
      ccores: undefined,
      cbackbones: undefined,
      csmiles: undefined,
    }
  },

  methods: {
    ecw(){
      let esw_value = (Math.min(this.alumo[this.anionId - 1], this.clumo[this.cationId - 1]) + 3.0915315909677425) - (Math.max(this.ahomo[this.anionId - 1], this.chomo[this.cationId - 1]) - 3.4620853512903227)
      if (esw_value >= 0){return 'ESW: ' + Math.round(esw_value * 10000) / 10000 + ' V';}
      return 'ESW is negative. 😢';
    },
    draw(smi, view) {

      let smilesDrawer = new SmilesDrawer.Drawer({debug: false, atomVisualization: 'default', width: 250, height: 250})
      SmilesDrawer.parse(smi, function (tree) {
          smilesDrawer.draw(tree, view, 'light', false);
      });

    },
    clean(view) {
      let smilesDrawer = new SmilesDrawer.Drawer({debug: false, atomVisualization: 'default', width: 250, height: 250, scale: 0, bondThickness: 0, bondLength: 0, shortBondLength: 0, bondSpacing: 0, fontSizeLarge: 0, fontSizeSmall: 0})
      SmilesDrawer.parse('C', function (tree) {
          smilesDrawer.draw(tree, view, 'light', false);
      }); 
    },
  },

}
</script>