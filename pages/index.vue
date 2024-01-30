<template>
  <div class="m-10">
    <div class="text-center">
      <UButton
        color="gray"
        @click="showAdd = true"
        class="w-28 text-center bg-zinc-300"
        >Add Details</UButton
      >
    </div>
    <table class="w-3/5 m-5 border-separate">
      <tbody>
        <tr>
          <th>Name</th>
          <th class="w-1/4">Age</th>
          <th class="w-1/3">Action</th>
        </tr>
        <tr v-for="(item, indx) of data">
          <td>{{ item.name }}</td>
          <td>{{ item.age }}</td>
          <td>
            <UButton color="gray" @click="editor(indx)" class="mx-5 bg-zinc-300"
              >Edit</UButton
            >
            <UButton
              color="gray"
              @click="(showDelete = true), (idx = indx)"
              class="bg-zinc-300"
              >Delete</UButton
            >
          </td>
        </tr>
      </tbody>
    </table>

    <Form
      title="Add Details"
      btnName="Save"
      @data="insert"
      @close="showAdd = false"
      v-model="showAdd"
    />

    <Form
      title="Edit Details"
      btnName="Update"
      :inData="editData"
      @data="edit"
      @close="showEdit = false"
      v-model="showEdit"
      :key="idx"
    />

    <Form
      title="Delete Details"
      @delete="remove"
      @close="showDelete = false"
      v-model="showDelete"
      :key="idx"
    />
  </div>
</template>

<script setup>
const showAdd = ref(false);
const showEdit = ref(false);
const showDelete = ref(false);
const idx = ref(0);
const data = ref();
//   [
//   {
//     name: "Raju",
//     age: 23,
//     gender: "Male",
//   },
// ]
const data1 = ref();
onMounted(() => {
  data.value = JSON.parse(localStorage.getItem("myData")) || [];
});

// console.log(data1);
const editData = ref();
const insert = (item) => {
  data.value.push(item);
  showAdd.value = false;
  localStorage.setItem("myData", JSON.stringify(data.value));
};
const editor = (indx) => {
  showEdit.value = true;
  idx.value = indx;
  editData.value = data.value[idx.value];
};
const edit = (item) => {
  data.value.splice(idx.value, 1, item);
  showEdit.value = false;
  localStorage.setItem("myData", JSON.stringify(data.value));
};
const remove = () => {
  data.value.splice(idx.value, 1);
  showDelete.value = false;
  localStorage.setItem("myData", JSON.stringify(data.value));
};
</script>
<style lang="postcss">
th,
td {
  @apply border border-gray-500 rounded-2xl text-center h-12;
}
th {
  @apply bg-zinc-200;
}
</style>
