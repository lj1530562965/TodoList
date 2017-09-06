<template>
    <div id="app" class="container">
      <h1>Todos</h1>
      <div style="padding-bottom: 10px;width: 365px;margin: 0 auto;">
          <span class="glyphicon glyphicon-chevron-down" style="color: #e6e6e6;float:left;line-height: 35px;" @click="showAll"></span>
          <input type="text" class="form-control" placeholder="What needs to be done?" style="width: 325px;margin: 0 auto;" v-model="newItem" @keyup.enter="addNews" >
      </div>
      <div class="tabbable" id="tabs-74955">
          <table class="table tab-content" style="width: 355px;margin: 0 auto;" v-if="isShowAll">
                  <tbody class="tab-pane active" id="panel-1">
                      <tr v-for="(item,index) in items" v-bind:class="{'cur':item.state==true}">
                          <td class="col-md-1"><input type="checkbox" v-model="item.state" name="checkbox" @change="checkfn"></td>
                          <td><span v-bind:class="{ 'line-through': item.state }">{{item.label}}</span></td>
                          <td><span class="glyphicon glyphicon-remove" style="color:#cc9a9a;" @click="deleteTodo(index)"></span></td>
                      </tr>
                  </tbody>
                  <tbody class="tab-pane" id="panel-2">
                      <tr v-for="(item,index) in items" v-if="item.state==false" v-bind:class="{'cur':item.state==true}">
                          <td class="col-md-1"><input type="checkbox" v-model="item.state" name="checkbox" @change="checkfn"></td>
                          <td><span v-bind:class="{ 'line-through': item.state }">{{item.label}}</span></td>
                          <td><span class="glyphicon glyphicon-remove" style="color:#cc9a9a;" @click="deleteTodo(index)"></span></td>
                      </tr>
                  </tbody>
                  <tbody class="tab-pane" id="panel-3">
                      <tr v-for="(item,index) in items" v-if="item.state==true" v-bind:class="{'cur':item.state==true}">
                          <td class="col-md-1"><input type="checkbox" v-model="item.state" name="checkbox" @change="checkfn"></td>
                          <td><span v-bind:class="{ 'line-through': item.state }">{{item.label}}</span></td>
                          <td><span class="glyphicon glyphicon-remove" style="color:#cc9a9a;" @click="deleteTodo(index)"></span></td>
                      </tr>
                  </tbody>
          </table>
          <footer>
          <ul style="padding-left: 32%;">
              <li style="line-height: 30px;">
                  <span v-html="lengths"></span>item
              </li>
              <li>
                  <input class="btn btn-default" type="button" value="All" href="#panel-1" data-toggle="tab">
              </li>
              <li>
                  <input class="btn btn-default" type="button" value="Active" @click="falseflagfn" href="#panel-2" data-toggle="tab">
              </li>
              <li>
                  <input class="btn btn-default" type="button" value="Completed" @click="trueflagfn" href="#panel-3" data-toggle="tab">
              </li>
              <li>
                  <input class="btn btn-default" type="button" value="clear completed" v-show="clearcom" @click="cleartrueflagfn">
              </li>
          </ul>
          </footer>
      </div>
    </div>
</template>
<script>
import Store from './store'
export default {
  name: 'app',
  data () {
        return {
            items:Store.fetch(),
            newItem:'',
            isShowAll:true,
            falseflag:[],
            trueflag:[]
        }
  },
  mounted: function () {
        this.falseflagfn()
        this.trueflagfn()
  },
  computed: {
        clearcom: function () {
            if(this.trueflag.length > 0){
                return true
            }else{
                return false
            }
        },
        lengths: function () {
            if(this.falseflag){
                return this.falseflag.length
            }else{
                return 0
            }
        }
  },
  watch: {
      items:{
          handler:function(items){
              Store.save(items)
          },
          deep:true
      }
  },
  methods: {
        addNews () {
            this.items.push({
                "label":this.newItem,
                "state":false
            });
            this.newItem='';
            this.isShowDeleteTag = false
            this.falseflagfn()
        },
        deleteTodo (index) {
            this.items.splice(index, 1)
            this.falseflagfn()
            this.trueflagfn()
        },
        showAll () {
            this.isShowAll = !this.isShowAll
        },
        checkfn () {
            this.falseflagfn()
            this.trueflagfn()
        },
        falseflagfn () {
            this.falseflag = []
            for(var i=0;i<this.items.length;i++){
                if(this.items[i].state==false){
                    this.falseflag.push(this.items[i])
                }
            }
            console.log(this.falseflag)
        },
        trueflagfn () {
            this.trueflag = []
            for(var i=0;i<this.items.length;i++){
                if(this.items[i].state==true){
                    this.trueflag.push(this.items[i])
                }
            }
            console.log(this.trueflag)
        },
        cleartrueflagfn () {
            this.trueflag = []
            this.items = this.falseflag
        }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.line-through {
    text-decoration: line-through;
    color: #d9d9d9;
}
.btn{
    cursor:pointer;
}
ul li{
    list-style:none;
    float: left;
    margin: 0 5px;
}
.cur{
    background-color: #efefef;
}
</style>
