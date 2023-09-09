<template>
  <main>
    <div class="form-group">
      <InputText v-model="name" placeholder="Name" />
      <InputText v-model="surname" placeholder="Surname" />
      <InputText
        v-model="age"
        placeholder="Age"
        inputmode="numeric"
        type="text"
        :class="{ 'invalid-input': !isAgeValid }"
        @input="validateAge"
      />
      <Button @click="addData" :disabled="!isAgeValid">Add Data</Button>
    </div>

    <DataTable :value="data">
      <Column field="name" header="Name"></Column>
      <Column field="surname" header="Surname"></Column>
      <Column field="age" header="Age"></Column>
      <Column header="Actions">
        <template #body="{ data }">
          <button @click="removeData(data)" class="remove-button">Remove</button>
        </template>
      </Column>
    </DataTable>
  </main>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      surname: "",
      age: "",
      data: [],
      isAgeValid: true, // varsayılan olarak doğru kabul edilir
    };
  },
  methods: {
    validateAge() {
      // Yaş değerini sayıya dönüştürmeye çalışın
      const parsedAge = parseInt(this.age, 10);

      // Geçerli bir sayı değilse veya negatifse
      if (isNaN(parsedAge) || parsedAge < 0) {
        this.isAgeValid = false;
      } else {
        this.isAgeValid = true;
      }
    },
    addData() {
      this.data.push({
        name: this.name,
        surname: this.surname,
        age: this.age,
      });

      this.name = "";
      this.surname = "";
      this.age = "";
    },
    removeData(item) {
      const index = this.data.indexOf(item);
      if (index !== -1) {
        this.data.splice(index, 1);
      }
    },
  },
};
</script>

<style scoped>
.form-group {
  display: flex;
  flex-direction: column;
}
.invalid-input {
  border-color: red;
}
.remove-button {
  background-color: #f0f0f0;
  border: none;
  cursor: pointer;
}
</style>
