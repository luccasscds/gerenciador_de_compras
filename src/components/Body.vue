<template>
  <div id="Body">
    <div class="header">
      <h1>Gerenciador de Compras</h1>

      <ul>
        <li><a href="../App.vue">Home</a></li>
        <!--<div style="float:right">
          <input id="search" type="text" placeholder="pesquise aqui...">
        </div>-->
        
      </ul>
      
    </div>
     <div id="icons">
        <img id="icon1" src="../assets/calc.png"><font color="#34A8DB">R$ {{valorTotal}}</font>
        <!--<img id="icon2" src="../../public/imagens/shop.png"><font color="#34A8DB"></font>-->
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
                <select class="form-control" v-model="newQuant" id="new-produto"><br>
                <option disabled value="">Escolha um item</option>
                <option>1</option>
                <option>2</option>
                <option>3</option>
                <option>4</option>
                <option>5</option>
                <option>6</option>
                <option>7</option>
                <option>8</option>
                <option>9</option>
                <option>10</option>
              </select>
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
        <tr class="contlist">
          <th colspan="6">LISTA DE COMPRAS REALIZADAS</th>
        </tr>
        <tr class="contlist">
          <th>Produto</th>
          <th>Loja</th>
          <th>Preço Uni.</th>
          <th>Quantidade</th>
          <th>Preço Total</th>
          <th></th>
        </tr>
        <tr class="contlist" v-for="produto in compras" :key="produto.id">
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
        idCompra: 1,
        newProduto: '',
        newLoja: '',
        newPreco: null,
        newQuant: 1,
        valorTotal: 0,
        compras: []
      }
    },
    methods: {
      addProduto: function () {
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
        this.newQuant = 1
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
        this.valorTotal = parseFloat((this.newPreco * this.newQuant + this.valorTotal).toFixed(2))
      }
    } 
  }
</script>

<style></style>