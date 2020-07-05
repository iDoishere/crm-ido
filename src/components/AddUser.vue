 <template>
  <div class="main-inputs">
    <div class="add-inputs">
      <div :class="['input-group', isNameValid()]">
        <input class="input" v-model="newTodo.name" placeholder="Name" />
      </div>

      <div :class="['input-group', isLastNameValid()]">
        <input class="input" v-model="newTodo.lastName" placeholder="Last Name" />
      </div>

      <div :class="['input-group', isGenderValid()]">
        <input class="input" placeholder="Gender" v-model="newTodo.gender" list="list-gender" />

        <datalist id="list-gender">
          <option v-for="g in genders" v-bind:key="g">{{g}}</option>
        </datalist>
      </div>

      <div :class="['input-group', isEmailValid()]">
        <input class="input input-email" v-model="newTodo.email" placeholder="Email" />
      </div>

      <div :class="['input-group', isIpValid()]">
        <input class="input" v-model="newTodo.ip" placeholder="Ip" />
      </div>
    </div>

    <div class="add-btn">
      <button v-on:click="addUser" type="button" class="btn btn-secondary">ADD</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddUser",
  data() {
    return {
      genders: ["Male", "Female"],
      newTodo: {
        name: "",
        lastName: "",
        email: "",
        gender: "",
        ip: ""
      },
      ifEmailCorrect: false,
      ifIpCorrect: false,
      regexOnlyLetter: /^[a-z]+$/i,
      ipRegex: /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/
    };
  },
  methods: {
    //open
    isNameValid: function() {

      let final = "";
      this.newTodo.name == "" ? (final = "has-error") : (final = "has-success");
      this.regexOnlyLetter.test(this.newTodo.name) == false
        ? (final = "has-error")
        : (final = "has-success");
      return final;
    },

    isLastNameValid: function() {
      let final = "";
      this.newTodo.lastName == ""
        ? (final = "has-error")
        : (final = "has-success");
      this.regexOnlyLetter.test(this.newTodo.lastName) == false
        ? (final = "has-error")
        : (final = "has-success");
      return final;
    },
    isGenderValid: function() {
      let final = "";
      this.newTodo.gender == ""
        ? (final = "has-error")
        : (final = "has-success");
      return final;
    },
    isEmailValid: function() {
      const reg = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      let final =
        this.newTodo.email == ""
          ? "has-error"
          : reg.test(this.newTodo.email)
          ? "has-success"
          : "has-error";

      final == "has-success"
        ? (this.ifEmailCorrect = true)
        : (this.ifEmailCorrect = false);

      return final;
    },
    isIpValid: function() {
      let final =
        this.newTodo.ip == ""
          ? "has-error"
          : this.ipRegex.test(this.newTodo.ip)
          ? "has-success"
          : "has-error";
      final == "has-success"
        ? (this.ifIpCorrect = true)
        : (this.ifIpCorrect = false);
      return final;
    },
    addUser() {
      let user = { ...this.newTodo };
      let ifEmail = this.ifEmailCorrect;
      let ifIp = this.ifIpCorrect;
      this.$emit("addUser", { user, ifEmail, ifIp });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-inputs {
  display: flex;
  flex-direction: column;
  margin-top: 2%;
  
}
.add-inputs {
  display: flex;
  justify-content: space-evenly;
  width: 100%;
}
.input {
  width: 90%;
  outline: none;
}
.input:hover {
  width: 110%;
  transition: all .3s linear;
}
.b-form-input {
  width: 15%;
}
.input-group {
  width: 15%;
}
.has-error {
  border-bottom: 2px solid red;
  width: 15%;
}
.has-success {
  border-bottom: 2px solid green;
  width: 13%;
}
.add-btn {
  margin-top: 1%;
}
</style>
