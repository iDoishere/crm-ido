
<template >
  <div >
    <input class="input-props" v-if="edit"
     :value="text"
     @keyup.enter="save($event.target.value)" 
     />
    <div  v-if="!edit"
      @click="toEdit()">
      <label class="label" title="Press to edit">{{text}}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "Input",
  props: {
    text: {
      type: String,
      default: ""
    },
    edit: {
      type: Boolean,
      default: false
    }
  },
  
  data() {
    return {};
  },
  methods: {
    //save the change in input,sent by key up enter
    save(value) {
      this.$emit("save", value);
    },
    //open input when press on name
    toEdit(edit) {
      this.$emit("toEdit",!edit);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.label{
  cursor: pointer;
   font-style: italic;
   font-size: 1rem;
  font-weight: bold;
}
</style>
