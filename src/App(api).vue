<template>
  <div id="app">
    <van-contact-card
    type='edit' name='gehao' tel='13844416882' add-text='在这里添加'
    @click='card'
    ></van-contact-card>
    <van-contact-list
    v-model="id" :list='list' addText='新建' default-tag-text='默认'
    @add='add' @edit='edit' @select='select'
    ></van-contact-list>
    <van-contact-edit
    :contact-info=info :is-edit=isEdit :is-saving=saving :is-deleting=deleting :tel-validator=validator
    @save=save @delete=del
    ></van-contact-edit>
  </div>
</template>

<script>
import {ContactCard, ContactList, ContactEdit } from 'vant'
export default {
  name: 'App',
  data() {
    return {
      id:1,
      list:[{id:2,name:'gaoda',tel:'15698756412'}],
      info:{id:3,name:'hahh',tel:'15632478962'},
      isEdit:true,
      saving:false,
      deleting:false,
      validator(){
        return true
      }
    }
  },
  components:{
    'van-contact-card':ContactCard,
    'van-contact-list':ContactList,
    'van-contact-edit':ContactEdit
  },methods: {
    card(){
      console.log('---card---');
    },
    add(){
      console.log('---add---');
    },
    edit(item,index){
      console.log('---edit---',item,index);
    },
    select(item,index){
      console.log('---select---',item,index);
    },
    save(content){
      console.log(content);
    },
    del(content){
      console.log(content);
    }
  },
}
</script>

<style scoped>

</style>
