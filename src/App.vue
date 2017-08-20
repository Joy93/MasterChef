<template>
  <div id = 'app'>
    <nav class="navbar navbar-default" style = "margin-top: -50px; height: 80px;">
      <div class="container-fluid">
        <div class="navbar-header">
          <a class="navbar-brand" href="#">
            <img alt="Brand" src="./assets/logo20.png" style = "height: 300%; margin-top: -5px">
          </a>
          <a href="#" style = "margin-left: 60px; font-size: 4em; color: Green">Master Chef</a>
        </div>

        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
          <ul class="nav navbar-nav navbar-right">
          <li><a href="#" style = "margin-top: 30px; font-size: 1.5em">Recipe List</a></li>
          <li class="dropdown" style = "margin-top: 30px; font-size: 1.5em">
            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Account <span class="caret"></span></a>
            <ul class="dropdown-menu">
              <li><a href="#">Sign in</a></li>
              <li><a href="#">Sign Up</a></li>
              <li role="separator" class="divider"></li>
              <li><a href="#">Log Out</a></li>
            </ul>
          </li>
        </ul>
        </div>
      </div>
      </nav>
      <div class = "container panel panel-success fixed-panel">
          <div class = "panel-heading" style = "height: 40px;">
            <h3 style = "font-size: 2em; margin-top: -5px;"> New Grocery</h3>
          </div>

          <div class = "panel-body">
            <form id = "form" class = "form-inline" v-on:submit.prevent="addGrocery">
              <div class = "row-fluid">
              <div class = "col-md-9">
              <div class = "form group fixedForm">
                <label for = "groceryCategory" style = "margin-right: 55px;">Category</label>
                <input type="text" id = "groceryCategory" class = "form-control" v-model = "newGrocery.category">
              </div></br>
              <div class = "form group fixedForm">
                <label for = "groceryName" style = "margin-right: 63px;">Grocery</label>
                <input type="text" id = "groceryName" class = "form-control" v-model = "newGrocery.name">
              </div></br>
              <div class = "form group fixedForm">
                <label for = "groceryQuantity" style = "margin-right: 58px;">Quantity</label>
                <input type="text" id = "groceryQuantity" class = "form-control" v-model = "newGrocery.quantity">
              </div></br>
              <div class = "form group fixedForm">
                <label for = "groceryBestby" style = "margin-right: 30px;">Best-By Date</label>
                <input type="text" id = "groceryBestby" class = "form-control" v-model = "newGrocery.bestbydate">
              </div>
            </div>
            <div class = "col-md-1" style = "margin-top: 40px;">
              <input type="submit" class="btn btn-success" value = "Save" style = "min-width: 80px;">
              <button v-on: click="clearValue" class="btn btn-warning" style = "min-width: 80px; margin-top: 10px">Cancel</button>
            </div>
            </div>
            </form>
          </div>
        </div>


      <div class = "container panel panel-warning fixed-panel">
        <div class = "panel-heading" style = "height: 40px;">
          <h3 style = "font-size: 2em; margin-top: -5px;"> Grocery List</h3>
        </div>
        <div class = "panel-body" style = "max-height: 300px; overflow-y: scroll;">
          <table class = "table table-striped table-hover">
            <thead>
              <th>

						  </th>
              <th style = "text-align: center;">
                Category
              </th>
              <th style = "text-align: center;">
                Grocery
              </th>
              <th style = "text-align: center;">
                Quantities
              </th>
              <th style = "text-align: center;">
                Best-By Date
              </th>
            </thead>
            <tbody>
              <tr v-for="grocery in groceries">
                <td style="width: 4.1%; width:50px;">
										<div class="checkbox radio-margin">
												<label>
													<input type="checkbox" value="">
													<span class="cr"><i class="cr-icon glyphicon glyphicon-ok"></i></span>
												</label>
										</div>
								</td>
                <td id="gC">
                   {{grocery.category}}
                </td>
                <td id="gN">
                  {{grocery.name}}
                </td>
                <td id="gQ">
                  {{grocery.quantity}}
                </td>
                <td id="gB" v-on:click="toggleEdit(this, grocery)">
                  {{grocery.bestbydate}}
                </td>
                <td>
                  <span class="glyphicon glyphicon-trash" v-on:click="removeGrocery(grocery)"></span>
                </td>
                <td>
                  <span class="glyphicon glyphicon-edit" v-on:click="editGrocery(grocery)"></span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  </div>
</template>

<script>
  import Firebase from 'firebase'

  let config = {
    apiKey: "AIzaSyAwDStEBMLtsXguos0pM1RsdqE91bt0hbo",
    authDomain: "inventory-1a541.firebaseapp.com",
    databaseURL: "https://inventory-1a541.firebaseio.com",
    projectId: "inventory-1a541",
    storageBucket: "inventory-1a541.appspot.com",
    messagingSenderId: "30035892881"
  }

  let app = Firebase.initializeApp(config);
  let db = app.database();

  let groceryRef = db.ref('groceries');


  export default {
    name: 'app',
    firebase: {
      groceries: groceryRef
    },
    data() {
      return {
        newGrocery: {
          category: '',
          name: '',
          bestbydate: '',
          quantity: ''
        }
      }
    },
    methods: {
      addGrocery: function(){
        groceryRef.push(this.newGrocery);
        this.newGrocery.category = '';
        this.newGrocery.name = '';
        this.newGrocery.bestbydate = '';
        this.newGrocery.quantity = '';
      },
      clearValue: function(){
        this.newGrocery.category = '';
        this.newGrocery.name = '';
        this.newGrocery.bestbydate = '';
        this.newGrocery.quantity = '';
      },
      removeGrocery: function(grocery){
        groceryRef.child(grocery['.key']).remove();
      },

    }
  }



</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

/*Radio and Checkbox START*/
.checkbox label:after,
.radio label:after {
    content: '';
    display: table;
    clear: both;
}

.checkbox .cr,
.radio .cr {
    position: relative;
    display: inline-block;
    border: 1px solid #a9a9a9;
    border-radius: .6em;
    width: 1.3em;
    height: 1.3em;
    float: left;
    margin-right: .6em;
}

.radio .cr {
    border-radius: 50%;
}

.checkbox .cr .cr-icon,
.radio .cr .cr-icon {
    position: absolute;
    color: green;
    font-size: .8em;
    line-height: 0;
    top: 50%;
    left: 10%;
}
.checkbox label input[type="checkbox"],
.radio label input[type="radio"] {
    display: none;
}

.checkbox label input[type="checkbox"] + .cr > .cr-icon,
.radio label input[type="radio"] + .cr > .cr-icon {
    transform: scale(3) rotateZ(-20deg);
    opacity: 0;
    transition: all .3s ease-in;
}

.checkbox label input[type="checkbox"]:checked + .cr > .cr-icon,
.radio label input[type="radio"]:checked + .cr > .cr-icon {
    transform: scale(1) rotateZ(0deg);
    opacity: 1;
}

.checkbox label input[type="checkbox"]:disabled + .cr,
.radio label input[type="radio"]:disabled + .cr {
    opacity: .5;
}
/*Radio and Checkbox END*/

.fixed-panel {
  min-height: 10px;
  max-height: 300px;
  width: 40%;
  margin-left: 50px;
}
.fixedForm {
  text-align: left;
}


</style>
