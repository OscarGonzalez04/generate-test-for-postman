<template>
  <div class="container-testing-postman">

    <h1>{{title}}</h1>

    <div class="container-testing-input shadow p-3 mt-4 mt-4 bg-white">
        <b-form-textarea
            id="textarea"
            v-model="jsonText"
            placeholder="Introduce el JSON..."
            rows="3"
            max-rows="6">
        </b-form-textarea>
        <b-button 
            @click="convertJsonToTest()"
            class="mt-3"
            block variant="success">
            Generar pruebas
        </b-button>
    </div>
    

    <div class="results-testing-postman shadow p-3 bg-white">

      <p class="text-left">
        pm.test("El código de estado es 200", ()=>{<br>
        &nbsp; &nbsp; &nbsp;pm.response.to.have.status(200);<br>
        });<br>
        <br>
        const response = pm.response.json(); <br>
      </p>
     

        <p 
            :key="'field'+index"
            class="text-left"
            v-for="(item, index) in schemeValidation">
            pm.test("El campo {{item.key}} existe y es un {{item.typeKey}}", ()=>{ <br>
            &nbsp; &nbsp; &nbsp;pm.expect(response).to.have.property('{{item.key}}'); <br>

            <span v-if="item.typeKey === 'object' || item.typeKey === 'array' ">
                &nbsp; &nbsp; &nbsp;pm.expect(response.{{item.key}}).to.be.an('{{item.typeKey}}'); <br>
            </span>
            <span v-else>
                &nbsp; &nbsp; &nbsp;pm.expect(response.{{item.key}}).to.be.a('{{item.typeKey}}'); <br>
            </span>
            
            
            <span v-if="item.typeKey === 'object' && item.childKeys.length > 0">
                &nbsp; &nbsp; &nbsp;pm.expect(response.{{item.key}}).to.have.all.keys({{item.childKeys}}); <br>
            </span>

             <span v-if="item.key === 'date' || item.key === 'dateGmt'
                || item.key === 'modified' || item.key === 'modifiedGmt' ">
                &nbsp; &nbsp; &nbsp;pm.expect(response.{{item.key}}).to.match(/[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|3[0-1])T(2[0-3]|[01][0-9]):[0-5][0-9]:[0-5][0-9]/g); <br>
            </span>
            }); <br>
        </p>
    </div>


  </div>
</template>

<script>
export default {
  data () {
    return {
      title: 'Pagina para generar testing utilizados en Postman',
      jsonText: '',
      schemeValidation: [
        /*{
            key: "user"
            typeKey: "object",
            childKeys: "keys, keys"
        }*/
      ]
    }
  },
  methods: {
    convertJsonToTest(){
        //console.log("Convertiremos JSON a pruebas", JSON.parse(this.jsonText) );

        let requestData = JSON.parse(this.jsonText);
        let fieldsPostman = [];

        for(let key in requestData){

            let childsKeys = [];
            let typeData = typeof requestData[key];

            if(typeData === "object"){
                let itemsObject = requestData[key]; 
                console.log("itemsObject: ", itemsObject);

                for(let keySecond in itemsObject){
                  childsKeys.push(keySecond);  
                }
            }

            let typeOfItem = typeof requestData[key];

            if(typeOfItem === "object" && Array.isArray(requestData[key]) ){
              typeOfItem = "array";
            }

            fieldsPostman.push({
                key: key,
                typeKey: typeOfItem,
                childKeys: childsKeys
            });
        }

        this.schemeValidation = fieldsPostman;
        this.jsonText = "";
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container-testing-input, 
.results-testing-postman{
    max-width: 75%;
    margin: auto;
}
</style>
