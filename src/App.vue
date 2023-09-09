<template>
  <main>
    <div class="form-group">
      <InputText v-model="name" placeholder="Name" />
      <InputText v-model="surname" placeholder="Surname" />
      <InputText v-model="age" placeholder="Age" />
      <Button @click="addData">Add Data</Button>
    </div>

    <DataTable :value="data">
      <Column field="name" header="Name"></Column>
      <Column field="surname" header="Surname"></Column>
      <Column field="age" header="Age"></Column>
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
    };
  },
  methods: {
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
  },
};
</script>
<style scoped>
.form-group {
    display: flex;
    flex-direction: column;
}
</style>
