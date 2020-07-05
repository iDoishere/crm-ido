
<template>
  <div class="main">
    <div class="main-image">{{title}}</div>

    <div>
      <AddUser v-on:addUser="addUser" />
    </div>

    <table style="overflow-x:auto;">
      <thead>
        <tr>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Email</th>
          <th>Gender</th>
          <th>IP</th>
          <th>actions</th>
        </tr>
      </thead>
      <tbody>
        <tr class="tr" v-for="user in displayedUsers" :key="user.id">
          <td>
            <IdoInput
              :edit="user.editName"
              :text="user.name"
              @toEdit="AllowOnlyOneInputToEdit(user,'editName')"
              @save="saveRow(user, 'name', 'editName', $event,'.input-props')"
            />
          </td>

          <td>
            <IdoInput
              :edit="user.editLastName"
              :text="user.lastName"
              @toEdit="AllowOnlyOneInputToEdit(user, 'editLastName')"
              @save="saveRow(user, 'lastName', 'editLastName', $event,'.input-props')"
            />
          </td>

          <td>
            <IdoInput
              :edit="user.editEmail"
              :text="user.email"
              @toEdit="AllowOnlyOneInputToEdit(user, 'editEmail')"
              @save="saveRow(user, 'email', 'editEmail', $event , '.input-props')"
            />
          </td>

          <td>
            <IdoInput
              :edit="user.editGender"
              :text="user.gender"
              @toEdit="AllowOnlyOneInputToEdit(user, 'editGender')"
              @save="saveRow(user, 'gender', 'editGender', $event,'.input-props')"
            />
          </td>

          <td>
            <IdoInput
              :edit="user.editIp"
              :text="user.ip"
              @toEdit="AllowOnlyOneInputToEdit(user, 'editIp')"
              @save="saveRow(user, 'ip', 'editIp', $event, '.input-props')"
            />
          </td>
          <td>
            <button
              @click="removeElement(user.id)"
              v-on:click="addUser"
              type="button"
              class="btn btn-secondary"
            >Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <div>
      <nav aria-label="Page navigation example">
        <ul class="pagination">
          <li class="page-item">
            <a
              href="javascript: void(0)"
              v-if="page != 1"
              @click="page--"
              class="page-link"
            >Previous</a>
          </li>

          <li
            active
            v-for="(pageNumber,key) in pages.slice(page-1, page+5)"
            :key="key"
            class="page-item"
          >
            <a @click="page = pageNumber" class="page-link" href="javascript: void(0)">
              {{pageNumber}}
              <span class="sr-only">(current)</span>
            </a>
          </li>
          <li class="page-item">
            <a
              href="javascript: void(0)"
              v-if="page < pages.length"
              @click="page++"
              class="page-link"
            >Next</a>
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
import json from "../Mock_Data/MOCK_DATA.json";
import IdoInput from "./Input.vue";
import AddUser from "./AddUser";
export default {
  name: "Users",
  components: {
    IdoInput,
    AddUser
  },
  created() {
    this.getUsers();
  },
  data() {
    return {
      title: "CRM-USERS",
      data_store: [],
      perPage: 10,
      pages: [],
      page: 1,
      usersEachPage: 0,
      regexOnlyLetter: /^[a-z]+$/i,
      ipRegex: /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/,
      generalIfInput: true,
      userAnimationAdd: false
    };
  },

  methods: {
    //function that check if user have opened a input ' if yes he wont allowed to open anymore until he close the current input.
    AllowOnlyOneInputToEdit(user, prop) {
      if (this.generalIfInput) {
        user[prop] = true;
        this.generalIfInput = false;
      }
    },
    //validtion email
    isEmailEditCorrect: function(value) {
      let bol = false;
      const reg = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      let ifEdit = reg.test(value);
      ifEdit ? (bol = true) : (bol = false);
      return bol;
    },
    // user press enter to save
    saveRow(user, prop, editProp, value, alon) {
      if (prop == "email") {
        if (this.isEmailEditCorrect(value)) {
          user[prop] = value;
          user[editProp] = false;
        } else {
          document.querySelector(alon).value = user.email;
        }
      }
      if (prop == "name") {
        if (this.regexOnlyLetter.test(value)) {
          user[prop] = value;
          user[editProp] = false;
        } else {
          document.querySelector(alon).value = user.name;
        }
      }
      if (prop == "lastName") {
        if (this.regexOnlyLetter.test(value)) {
          user[prop] = value;
          user[editProp] = false;
        } else {
          document.querySelector(alon).value = user.lastName;
        }
      }
      if (prop == "ip") {
        if (this.ipRegex.test(value)) {
          user[prop] = value;
          user[editProp] = false;
        } else {
          document.querySelector(alon).value = user.ip;
        }
      }
      if (prop == "gender") {
        if (value == "Male" || value == "Female") {
          user[prop] = value;
          user[editProp] = false;
        } else {
          document.querySelector(alon).value = user.gender;
        }
      }
      this.generalIfInput = true;
    },
    //set amount of pages acooring to the size of users
    setPages() {
         this.pages = [];
      this.usersEachPage = Math.ceil(this.data_store.length / this.perPage);
      // check how i can improve this every time i delete the loop is runing
      for (let index = 1; index <= this.usersEachPage; index++) {
        this.pages.push(index);
      }
    },
    //clean inputs after user added to the array.
    resetAdd() {
      this.newTodo = {
        name: "",
        lastName: "",
        email: "",
        gender: "",
        ip: ""
      };
    },
    // display x users each page
    paginate(data_store) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return data_store.slice(from, to);
    },
    //get users and create a dynamic object
    getUsers() {
      var mappedUsers = json.map(function(user) {
        return {
          id: user.id,
          name: user.first_name,
          lastName: user.last_name,
          email: user.email,
          gender: user.gender,
          ip: user.ip_address,
          editName: false,
          editLastName: false,
          editEmail: false,
          editGender: false,
          editIp: false
        };
      });
      this.data_store = [...mappedUsers];
    },
    //remove user
    removeElement: function(id) {
      this.data_store = this.data_store.filter(user => user.id !== id);
      this.setPages();
    },
    // add user only after validation
    addUser(data) {
      if (this.checkifUserExits(data.user)) {
        alert("User Exits in database ,plz try again");
        return;
      }
      if (!this.empty(data.user)) return;
      if (!this.checkOnlyLetters(data.user)) return;
      if (!data.ifEmail) return;
      if (!data.ifIp) return;

      const lastId = this.data_store.length;
      const updatedUser = {
        ...data.user,
        id: lastId + 1,
        editName: false,
        editLastName: false,
        editEmail: false,
        editGender: false,
        editIp: false
      };

      this.data_store.unshift(updatedUser);
      this.userAnimationAdd = true;
      this.resetAdd();
    },
    checkifUserExits(user) {
      return this.data_store.some(el => el.email === user.email);
    },
    empty(newUser) {
      if (
        newUser.name == "" ||
        newUser.lastName == "" ||
        newUser.gender == ""
      ) {
        return false;
      }
      return true;
    },
    checkOnlyLetters(newUser) {
      if (
        this.regexOnlyLetter.test(newUser.name) &&
        this.regexOnlyLetter.test(newUser.lastName)
      ) {
        return true;
      } else {
        return false;
      }
    },
    animationAddUser() {
      if (this.userAnimationAdd) {
        let elementTr = document.getElementsByClassName("tr")[0];
        elementTr.classList.add("animation-add");
        setTimeout(() => {
          elementTr.classList.remove("animation-add");
          this.userAnimationAdd = false;
        }, 3501);
      }
    }
  },
  updated() {
    this.animationAddUser();
  },
  computed: {
    displayedUsers() {
      return this.paginate(this.data_store);
    }
  },
  watch: {
    data_store() {
      this.setPages();
    }
  }
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  justify-content: space-around;
}
.main-image {
  background-image: url("https://www.mycustomer.com/sites/default/files/istockphoto_thinkstock_CRM2%20%282%29_3_0.jpg");
  height: 30vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

table {
  border-collapse: collapse;
  border-spacing: 0;
  width: 100%;
  border: 1px solid #ddd;
  justify-content: center;
  margin-top: 1%;
  position: relative;
  animation-name: getTable;
  animation-duration: 2.2s;
  animation-timing-function: linear;
}
td {
  padding: 3px;
}
th {
  padding: 3px;
  border-bottom: 1px dotted black;
}

@keyframes getTable {
  from {
    left: -1300px;
  }
  to {
    left: 0;
  }
}
.animation-add {
  background: green;
  animation-name: row-added;
  animation-duration: 3.5s;
  animation-timing-function: linear;
}
@keyframes row-added {
  0% {
    opacity: 0;
    background: transparent;
  }
  50% {
    background: greenyellow;
  }
  100% {
    opacity: 1;
    background: none;
    background: transparent;
  }
}
.paginattion-btn {
  padding: 6px;
  margin: 2px;
  border-radius: 3px;
  font-size: 1rem;
  cursor: pointer;
}
.add-user {
  display: flex;
  justify-content: center;
}
.pagination {
  display: flex;

  justify-content: center;
}
/* .has-error {
  border-bottom: 2px solid red;
}
.has-success {
  border-bottom: 2px solid green;
} */
</style>
