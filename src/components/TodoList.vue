<template>
  <section>
    <transition-group name="list" tag="ul">
      <li v-for="(todoItem, index) in propsdata" :key="todoItem" class="shadow">
        <i class="checkBtn fas fa-check" aria-hidden="true" @click="updateState(todoItem)"></i>
        <span :class="{textCompleted:doneItems[index]}"> {{ todoItem }}{{doneItems[index]}} </span>
        <span class="detailBtn" type="button" @click="showDetailModal(todoItem,index)">
          <i class="fas fa-ellipsis-v"></i>
        </span>
        <modal v-if="DetailModal" @close="DetailModal = false">
          <h3 slot="header">Detail</h3>
          <span slot="footer" @click="DetailModal = false">내용 블라블라
            <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
          </span>
        </modal>

        <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)">
          <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
      </li>
    </transition-group>
  </section>
</template>

<script>
import Modal from './common/DetailModal.vue'



export default {
  data(){
    return{
      DetailModal: false,
      doneItems: []
      
    }
  }
  ,

  props: ['propsdata'],
  methods: {
    removeTodo(todoItem, index) {
      this.$emit('removeTodo', todoItem, index);
    },
    updateState(todoItem){
      var items=JSON.parse(localStorage.getItem(todoItem))
      items.done=!items.done
      localStorage.setItem(todoItem,JSON.stringify(items))
			for (var i = 0; i < localStorage.length; i++) {
        var item=JSON.parse(localStorage.getItem(localStorage.key(i)))
        this.doneItems.splice(i,1,item.done)
        console.log(this.doneItems)
        
      }
    },
    //   if (this.doneItems[index] == true) {
    //     document.getElementById("todo").className = "textCompleted"}
    //     else {document.getElementById("todo").className = "textNotComplted"}

    // },
    // updateState(todoItem, index){
    //   // var items = JSON.parse(localStorage.getItem(todoItem))
    //   // console.log(this.doneItems)
    //   // items.done=!items.done
    //   // localStorage.setItem(todoItem, JSON.stringify(items)) //로컬스토리지에 업데이트된 done 저장
    //   // this.doneItems.splice(index,1,items.done)
    //   // console.log(this.doneItems)


    // },
    
    showDetailModal(todoItem, index){
      this.$emit('showDetailModal',todoItem,index)
      this.DetailModal=!this.DetailModal;
    }
  },
    created() {
		if (localStorage.length > 0) {
			for (var i = 0; i < localStorage.length; i++) {
        var items=JSON.parse(localStorage.getItem(localStorage.key(i)))
        this.doneItems.push(items.done)
        console.log(this.doneItems)
      }
    }
  },
  components: {
    Modal: Modal
  }

}
</script>

<style scoped>
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
  }
  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }

  .detailBtn {
    margin-left: auto;
    color: #de4343;
  }
  .removeBtn {
    margin-left: 10px;
    color: #de4343;
  }
  .textCompleted {
    text-decoration: line-through;
    color: #b3adad;
  }

  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }
</style>
