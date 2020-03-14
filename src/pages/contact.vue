<template>
  <div>
    <!-- 联系人卡片 -->
    <van-contact-card
      :type="cardType"
      :name="currentContact.name"
      :tel="currentContact.tel"
      @click="showList = true"
    />

    <!-- 联系人列表 -->
    <van-popup v-model="showList" position="bottom">
      <van-contact-list
        v-model="chosenContactId"
        :list="list"
        @add="onAdd"
        @edit="onEdit"
        @select="onSelect"
      />
    </van-popup>

    <!-- 联系人编辑 -->
    <van-popup v-model="showEdit" position="bottom">
      <van-contact-edit
        :contact-info="editingContact"
        :is-edit="isEdit"
        @save="onSave"
        @delete="onDelete"
      />
    </van-popup>
  </div>
</template>

<script type="text/ecmascript-6">
import { ContactCard, ContactList, ContactEdit, Popup } from "vant";
const OK = 200
export default {
  name: "contact",
  data() {
    return {
      chosenContactId: null,
      editingContact: {},
      showList: false,
      showEdit: false,
      isEdit: false,
      list: []
    };
  },
  computed: {
    cardType() {
      return this.chosenContactId !== null ? "edit" : "add";
    },

    currentContact() {
      const id = this.chosenContactId;
      return id !== null ? this.list.filter(item => item.id === id)[0] : {};
    }
  },

  methods: {
    // 添加联系人
    onAdd() {
      this.editingContact = {};
      this.isEdit = false;
      this.showEdit = true;
    },

    // 编辑联系人
    onEdit(item) {
      this.isEdit = true;
      this.showEdit = true;
      this.editingContact = item;
    },

    // 选中联系人
    onSelect() {
      this.showList = false;
    },

    // 保存联系人
    async onSave(info) {
      this.showEdit = false;
      this.showList = false;
      let result
      if (this.isEdit) {
        // this.list = this.list.map(item => (item.id === info.id ? info : item));
        //   result = await axios({
        //   url:"http://localhost:9000/api/contact/edit",
        //   method:'put',
        //   data:{
        //     name:info.name,
        //     tel:info.tel,
        //     id:info.id
        //   }
        // })
        result = await this.$http.contact.editContact({name:info.name,tel:info.tel,id:info.id})
      } else {
        // this.list.push(info);
        //   result = await axios({
        //   url:'http://localhost:9000/api/contact/new/json',
        //   method:'post',
        //   data:{
        //     name:info.name,
        //     tel:info.tel
        //   }
        // })
        // let formData = new FormData()
        // formData.append('name',info.name)
        // formData.append('tel',info.tel)
        // result = await axios({
        //   url:"http://localhost:9000/api/contact/new/form",
        //   method:'post',
        //   data:formData
        // })
        // result = await this.$http.contact.createContactByJson({name:info.name,tel:info.tel})
        result = await this.$http.contact.createContactByForm({name:info.name,tel:info.tel})
      }
      await this.updataList()
      this.chosenContactId = result.data.id;
    },

    // 删除联系人
    async onDelete(info) {
      this.showEdit = false;
      // this.list = this.list.filter(item => item.id !== info.id);
      // let result = await axios({
      //   url:"http://localhost:9000/api/contact",
      //   method:'delete',
      //   params:{
      //     id:info.id
      //   }
      // })
      let result = await this.$http.contact.delContact({id:info.id})
      if (this.chosenContactId === info.id) {
        this.chosenContactId = null;
      }
      if(result.code === OK) await this.updataList()
    },
    async updataList(){
      //  let result = await axios({
      //   url:"http://localhost:9000/api/contactList",
      //   method:'get'
      // })
      let result = await this.$http.contact.getContactList()
      if(result.code === OK){
        this.list = result.data
      }
    }
  },
  components: {
    "van-contact-card": ContactCard,
    "van-contact-list": ContactList,
    "van-contact-edit": ContactEdit,
    "van-popup": Popup
  },
  mounted() {
   this.updataList()
  },
};
</script>

<style scoped lang='stylus'></style>
