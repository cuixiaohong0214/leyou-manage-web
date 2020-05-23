<!--
 * @Description: 
 * @Version: 2.0
 * @Autor: 崔晓鸿
 * @Date: 2020-04-07 15:56:27
 * @LastEditors: 崔晓鸿
 * @LastEditTime: 2020-04-09 11:26:33
 -->
<template>
  <div>
    <v-layout class="px-3 pb-2">
      <v-flex xs2>
        <v-btn small color="info">新增品牌</v-btn>
      </v-flex>
      <v-spacer/>
      <v-flex xs4>
        <v-text-field label="搜索" hide-details append-icon="search" v-model="key"></v-text-field>
      </v-flex>
    </v-layout>  
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
    <template slot="items" slot-scope="props">
      <td class="text-xs-center">{{props.item.id}}</td>
      <td class="text-xs-center">{{props.item.name}}</td>
      <td class="text-xs-center"><img :src="props.item.image" alt=""></td>
      <td class="text-xs-center">{{props.item.letter}}</td>
      <td class="text-xs-center">
        <v-btn flat icon color="info">
          <v-icon>edit</v-icon>
        </v-btn>
        <v-btn flat icon color="error">
          <v-icon>delete</v-icon>
        </v-btn>
      </td>
    </template>
    </v-data-table>
  </div>
</template>
<script>
export default {
  name:"MyBrand",
  data(){
    return{
      headers: [
          {text: '品牌id',align: 'center',sortable: true,value: 'id'},
          {text: '品牌名称',align: 'center',sortable: false,value: 'name'},
          {text: '品牌LOGO',align: 'center',sortable: false,value: 'image'},
          {text: '品牌首字母',align: 'center',sortable: false,value: 'letter'},
          {text: '操作',align: 'center',sortable: false,value: ''}
      ],
      brands:[],
      pagination:{},
      totalBrands:0,
      loading:false,
      key:""  //查询条件
    }
  },
  created(){

    this.brands=[
      {id:2032,name:"OPPO",image:"1.jpg",letter:"O"},
      {id:2033,name:"小米",image:"2.jpg",letter:"X"},
      {id:2034,name:"OPPO",image:"1.jpg",letter:"O"},
      {id:2035,name:"小米",image:"2.jpg",letter:"X"},
      {id:2036,name:"OPPO",image:"1.jpg",letter:"O"},
      {id:2037,name:"小米",image:"2.jpg",letter:"X"},
      {id:2038,name:"华为",image:"3.jpg",letter:"H"}
    ];
    this.totalBrands=15

    this.loadBrands();
  },
  watch:{
    key(){
      this.pagination.page=1;
      this.loadBrands();
    },
    pagination:{
      deep:true,
      handler(){
        this.loadBrands();
      }
    }
  },
  methods:{
    loadBrands(){
      this.loading=true
      this.$http.get("/item/brand/page",{
        params:{
          page:this.pagination.page,
          rows:this.pagination.rowsPerpage,
          sortBy:this.pagination.descending,
          key:this.key,
        }
      }).then(resp=>{
        //console.log(resp)
        this.brands=resp.data.items;
        this.totalBrands=resp.data.total;
        this.loading=false
      })
    }
  }
  
}
</script>
<style scoped>

</style>