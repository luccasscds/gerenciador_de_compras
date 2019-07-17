<template>
  <div id="Body">
    <div class="header">
      <h1>Gerenciador de Compras</h1>

      <ul>
        <li><a href="../index.html">Home</a></li>
        <div style="float:right">
          <input id="search" type="text" v-model="search" placeholder="pesquise aqui...">
        </div>
        
      </ul>
      
    </div>
     <div id="icons">
        <img id="icon1" src="../assets/calc.png"><font color="#34A8DB">R$ {{valorTotal}}</font>
      </div>
    <div class="caixa">
      <h2>Adicione suas compras:</h2>
      <form v-on:submit.prevent="addProduto">
        <label for="new-produto"></label>
        <span>
          <table>
            <tr>
              <th class="tabtext"><span>O produto comprado:</span></th>
              <th class="tabinp"><input class="form-control" v-model="newProduto" id="new-produto" placeholder="Ex.: camisa" maxlength="20" required></th>
            </tr>
            <tr>
              <th class="tabtext"><span>A loja visitada:</span></th>
              <th class="tabinp"><input class="form-control" v-model="newLoja" id="new-produto" placeholder="Ex.: Riachuelo" maxlength="20" required></th>
            </tr>
            <tr>
              <th class="tabtext"><span>O preço da compra:</span></th>
              <th class="tabinp"><input 
                v-model="newPreco" 
                class="form-control"
                id="new-produto" 
                type="number" step="0.01"
                placeholder="Ex.: R$ 29.99" 
                min="0.0" max="100000.0" required>
              </th>
            </tr>
            <tr>
              <th class="tabtext"><span>Quantidade:</span></th>
              <th class="tabinp">
                <input 
                v-model="newQuant" 
                class="form-control"
                id="new-produto" 
                type="number" 
                placeholder="Quantidade de itens" 
                 required>
              </th>
            </tr>
          </table>
        </span>
        <div class="btn">
          <button class="btn btn-dark" @click="calcularGastos">Adicionar</button><br>
        </div>
      </form>
      
    </div>
   
    <div>
      <table class="lista" align="center">
        <tr>
          <th  class="titlist" colspan="6">LISTA DE COMPRAS REALIZADAS</th>
        </tr>
        <tr class="contlist">
          <th>Produto</th>
          <th>Loja</th>
          <th>Preço Uni.</th>
          <th>Quantidade</th>
          <th>Preço Total</th>
          <th></th>
        </tr>
        <tr class="contlist" v-for="produto in filtersearch" :key="produto.id">
          <th>{{produto.title}}</th>
          <th>{{produto.loja}}</th>
          <th>{{produto.preco}}</th>
          <th>{{produto.quant}}</th>
          <th>{{produto.total}}</th>
          <th id="btn"><button class="btn btn-outline-danger" @click="removeProduto(produto,produto.total)">x</button></th>
        </tr>
  
      </table>
    </div>

  </div>
</template>

<script>
  export default {
    name: 'Body',
    data(){
      return {
        search: '',
        idCompra: 1,
        newProduto: '',
        newLoja: '',
        newPreco: null,
        newQuant: '',
        valorTotal: 0, 
        compras: []
      }
    },
    computed: {
      filtersearch: function () {
        return this.compras.filter((produto) => {
          return produto.title.toLowerCase().match(this.search)
        })
      }
    },
    methods: {
      addProduto: function () {
      
        if (this.newQuant == ""){
          this.newQuant = 1
        }
        
        this.compras.push({
          id: this.idCompra++,
          title: this.newProduto,
          loja: this.newLoja,
          preco: this.newPreco,
          quant: this.newQuant,
          total: this.newPreco * this.newQuant
        })
        this.newProduto = ''
        this.newLoja = ''
        this.newPreco = null
        this.newQuant = null
      },
      removeProduto: function(produto,total) {var del = this.compras.indexOf(produto)
          const Swalok = Swal.mixin({
                  customClass: {
                    confirmButton: 'btn-primary',
                    cancelButton: 'btn btn-danger'
                  },
                  buttonsStyling: true,
                })
          Swalok.fire({
            title: 'Apagar produto?',
            text: "",
            type: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Sim, apagar produto!',
            cancelButtonText: 'Cancelar'
          }).then((result) => {
            if(result.value){
              
              this.compras.splice(this.compras.indexOf(produto), 1),
               this.valorTotal -= total
               this.valorTotal = parseFloat(this.valorTotal.toFixed(2))
      }else if(result.dismiss === Swal.DismissReason.cancel){
            }
          })
        },
      calcularGastos(){
      
        if (this.newQuant == ""){
          this.newQuant = 1
        }
      
        if(this.newProduto != '' && this.newLoja != ''){
          this.valorTotal = parseFloat((this.newPreco * this.newQuant + this.valorTotal).toFixed(2))
        }
      }
    } 
  }
</script>

<style></style>
