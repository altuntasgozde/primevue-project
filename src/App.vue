<template>
  <div>
    <div class="filter-form">
      <div class="form-group">
        <InputText v-model="nameFilter" placeholder="Filter by Name" />
      </div>
      <div class="form-group">
        <InputText v-model="surnameFilter" placeholder="Filter by Surname" />
      </div>
      <div class="form-group">
        <InputText v-model="ageFilter" placeholder="Filter by Age" />
      </div>
      <div class="form-group">
        <InputText v-model="schoolNumberFilter" placeholder="Filter by School Number" />
      </div>
      <div class="form-group">
        <InputText v-model="locationFilter" placeholder="Filter by Location" />
      </div>
    </div>

    <div class="form-group">
      <InputText v-model="name" placeholder="Name" />
    </div>
    <div class="form-group">
      <InputText v-model="surname" placeholder="Surname" />
    </div>
    <div class="form-group">
      <InputText
        v-model="age"
        placeholder="Age"
        inputmode="numeric"
        type="text"
        :class="{ 'invalid-input': !isAgeValid }"
        @input="validateAge"
      />
    </div>
    <div class="form-group">
      <InputText v-model="schoolNumber" placeholder="School Number" />
    </div>
    <div class="form-group">
      <InputText v-model="location" placeholder="Location (City, State)" />
    </div>
    <Button @click="addOrUpdateData" :disabled="!isAgeValid">
      {{ editingIndex === -1 ? "Add Data" : "Update Data" }}
    </Button>

    <DataTable
      :value="filteredData"
      :paginator="true"
      :rows="10"
      :rowsPerPageOptions="[5, 10, 20]"
      :sortMode="'multiple'"
    >
      <Column field="name" header="Name" :sortable="true" filterMatchMode="contains">
        <template #filter>
          <InputText v-model="nameFilter" placeholder="Filter by Name" />
        </template>
      </Column>
      <Column field="surname" header="Surname" :sortable="true" filterMatchMode="contains">
        <template #filter>
          <InputText v-model="surnameFilter" placeholder="Filter by Surname" />
        </template>
      </Column>
      <Column field="age" header="Age" :sortable="true" filterMatchMode="equals">
        <template #filter>
          <InputText v-model="ageFilter" placeholder="Filter by Age" />
        </template>
      </Column>
      <Column field="schoolNumber" header="School Number" :sortable="true" filterMatchMode="contains">
        <template #filter>
          <InputText v-model="schoolNumberFilter" placeholder="Filter by School Number" />
        </template>
      </Column>
      <Column field="location" header="Location" :sortable="true" filterMatchMode="contains">
        <template #filter>
          <InputText v-model="locationFilter" placeholder="Filter by Location" />
        </template>
      </Column>
      <Column header="Actions">
        <template #body="{ data }">
          <button @click="editData(data)" class="edit-button">Edit</button>
          <button @click="removeData(data)" class="remove-button">Remove</button>
        </template>
      </Column>
    </DataTable>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const nameFilter = ref('');
    const surnameFilter = ref('');
    const ageFilter = ref('');
    const schoolNumberFilter = ref('');
    const locationFilter = ref('');
    const name = ref('');
    const surname = ref('');
    const age = ref('');
    const schoolNumber = ref('');
    const location = ref('');
    const data = ref([
      {
        name: "John",
        surname: "Doe",
        age: 30,
        schoolNumber: "12345",
        location: "İstanbul",
      },
    ]);
    const isAgeValid = ref(true);
    const editingIndex = ref(-1);

    const validateAge = () => {
      const parsedAge = parseInt(age.value, 10);

      if (isNaN(parsedAge) || parsedAge < 0) {
        isAgeValid.value = false;
      } else {
        isAgeValid.value = true;
      }
    };

    const addOrUpdateData = () => {
      if (editingIndex.value === -1) {
        data.value.push({
          name: name.value,
          surname: surname.value,
          age: age.value,
          schoolNumber: schoolNumber.value,
          location: location.value,
        });
      } else {
        data.value[editingIndex.value] = {
          name: name.value,
          surname: surname.value,
          age: age.value,
          schoolNumber: schoolNumber.value,
          location: location.value,
        };
        editingIndex.value = -1; 
      }

    name.value = '';
      surname.value = '';
      age.value = '';
      schoolNumber.value = '';
      location.value = '';
    };

    const editData = (item) => {
      name.value = item.name;
      surname.value = item.surname;
      age.value = item.age;
      schoolNumber.value = item.schoolNumber;
      location.value = item.location;
      editingIndex.value = data.value.indexOf(item);
    };

    const removeData = (item) => {
      const index = data.value.indexOf(item);
      if (index !== -1) {
        data.value.splice(index, 1);
      }
    };

    const filteredData = computed(() => {
      return data.value.filter((item) => {
        const nameMatch = item.name.toLowerCase().includes(nameFilter.value.toLowerCase());
        const surnameMatch = item.surname.toLowerCase().includes(surnameFilter.value.toLowerCase());
        const ageMatch = ageFilter.value === '' || item.age === parseInt(ageFilter.value, 10);
        const schoolNumberMatch = item.schoolNumber.toLowerCase().includes(schoolNumberFilter.value.toLowerCase());
        const locationMatch = item.location.toLowerCase().includes(locationFilter.value.toLowerCase());

        return nameMatch && surnameMatch && ageMatch && schoolNumberMatch && locationMatch;
      });
    });

    return {
      nameFilter,
      surnameFilter,
      ageFilter,
      schoolNumberFilter,
      locationFilter,
      name,
      surname,
      age,
      schoolNumber,
      location,
      data,
      isAgeValid,
      editingIndex,
      validateAge,
      addOrUpdateData,
      editData,
      removeData,
      filteredData,
    };
  },
};
</script>

<style scoped>
.filter-form {
  display: flex;
  margin-bottom: 20px;
}

.form-group {
  margin-right: 10px;
}

.invalid-input {
  border-color: red;
}

.edit-button,
.remove-button {
  background-color: #f0f0f0;
  border: none;
  cursor: pointer;
}
</style>
