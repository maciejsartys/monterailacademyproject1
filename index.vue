<template>
  <main>
    <h1>Buddies debt list</h1>
    <div class='selectPerson'>
      <h2>Select person: </h2>
      <select v-model="selectedPerson">
        <option disabled value="">Select person</option>
        <option v-for="person in persons" :key="person.id" v-bind:value="person.id">{{ person.name }}</option>
      </select>
    </div>
    <template v-if="this.selectedPerson !== null">
        <div>
          <label for="description">Description: </label>
          <input type="text" id="description" placeholder="Type here.." v-model="newOperationDescription">
          <label for="amount">Amount: </label>
          <input type="number" id="amount" step="0.01" v-model.number="newOperationAmount">
          <button @click="addNewOperation">Add</button>
        </div>       
        <ul>
          <li class='topRow'>Current balance:<span class='amount'>{{ selectedPersonBalance }}</span></li>
          <li v-for="operation in selectedPersonOperations" :key="operation.id" >
            {{ operation.description }} <span class='amount'>{{ operation.amount}}</span> 
          </li>
        </ul>
      </template>
  </main>
</template>

<style>
html {
  height: 100%;
}
body {
  font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
  font-size: 14px;
  line-height: 1.42857143;
  color: #333;
  margin: 20px;
  color: #435757;
  background: linear-gradient(-20deg, #d0b782 20%, #a0cecf 80%);
  margin-bottom: 0px;
  min-height: 100%;
}

main {
  max-width: 600px;
  margin: 0 auto;
  border-top: 5px solid #435757;
  background-color: rgba(255, 255, 255, .2);
  box-shadow: 0 0 20px rgba(0, 0, 0, .1);
  user-select: none;
  min-height: 500px;
}
h1 {
  margin: 0;
  padding: 20px;
  background-color: rgba(255, 255, 255, .4);
  font-size: 1.8em;
  text-align: center;
}
h2 {
  display: inline-block;
  margin: 0;
  padding: 10px 0;
  font-size: 1.2em;
}
li {
  list-style: none;
  font-size: 2em;
  border-top: 1px dashed #fff;
}
li.topRow{
  font-size: 2.4em;
  background-color: rgba(255, 255, 255, .2);
}
label {
  display: inline-block;
  max-width: 100%;
  margin-bottom: 5px;
  font-size: 1.2em;
  box-sizing: border-box;
}
input {
  padding: 12px 20px;
  width: 75px;
  height: 18px;
  padding: 6px 12px;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.amount {
  display: block;
  float: right;
  font-weight: normal;
  margin-right: 10px;
}
button {
  margin: 0px 10px;
  padding: 6px 16px;
  font-size: 1.2em;
  line-height: 1.3333333;
  border-radius: 10px;
  color: #000;
  background-color: rgba(255, 255, 255, .2);
  border: 1px solid;
  border-color: #aaa;
  display: inline-block;
}
main {
  padding: 0 35px;
}
</style>

<script>
export default {
  data() {
    return {
      operations: [
        { id: 1, person: 1, amount: 2.5, description: 'Donuts' },
        { id: 2, person: 2, amount: 3.5, description: 'Coca-Cola' },
        { id: 3, person: 3, amount: 12.5, description: 'Pizza' },
        { id: 4, person: 1, amount: -1.5, description: 'Return' },
        { id: 5, person: 2, amount: 2.5, description: 'Donuts' },
        { id: 6, person: 3, amount: 2.5, description: 'Donuts' },
        { id: 7, person: 3, amount: -10.0, description: 'Return' },
        { id: 8, person: 4, amount: 8, description: 'Beer' },
        { id: 9, person: 1, amount: 7.5, description: 'Breakfast' },
      ],
      persons: [
        { id: 1, name: "Marta"},
        { id: 2, name: "Emilia"},
        { id: 3, name: "Karol"},
        { id: 4, name: "Tomek"},
      ],
      selectedPerson: null,
      newOperationAmount: 0.0,
      newOperationDescription: "",
    }
  },
  computed: {
    selectedPersonName() {
      if (this.selectedPerson !== null) {
        return this.persons.filter((person) =>  person.id === this.selectedPerson)[0].name
      } else {
        return null
      }
    },
    selectedPersonOperations() {
      return this.operations.filter((operation) => operation.person === this.selectedPerson)
    },
    selectedPersonBalance() {
      return this.selectedPersonOperations.reduce((prev, curr) => prev + curr.amount, 0)
    }
  },
  methods: {
    addNewOperation() {
      let lastId = this.operations.slice(-1)[0].id
      this.operations.push({
        id: lastId + 1,
        person: this.selectedPerson,
        amount: this.newOperationAmount,
        description: this.newOperationDescription,
      })
    }
  }
}
</script>
