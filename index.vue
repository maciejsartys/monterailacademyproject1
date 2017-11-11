<template>
  <div>
    <div>
      <span>Select person: </span>
      <select v-model="selectedPerson">
        <option disabled value="">Select person</option>
        <option v-for="person in persons" :key="person.id" v-bind:value="person.id">{{ person.name }}</option>
      </select>
    </div>
    <template v-if="this.selectedPerson !== null">
        <h1>
          Active person: {{ selectedPersonName }}
        </h1>
        <h2>Current balance: {{ selectedPersonBalance }}</h2>
        <div>
          <label for="amount">Amount: </label>
          <input type="number" id="amount" step="0.01" v-model.number="newOperationAmount">
          <label for="description">Description: </label>
          <input type="text" id="description" placeholder="Type here.." v-model="newOperationDescription">
          <button @click="addNewOperation">Add</button>
        </div>       
        <ul>
          <li v-for="operation in selectedPersonOperations" :key="operation.id" >
            id: {{ operation.id }}, amount: {{ operation.amount}}, description: {{ operation.description }}
          </li>
        </ul>
      </template>
  </div>
</template>

<style>
h1 {
    font-family: "Helvetica";
    color: #19ad19
}
li {
  list-style: none;
  font-size: 30px;
}
input[type="checkbox"] {
  width: 20px;
  height: 20px;
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
      let lastId = this.operations.pop().id
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
