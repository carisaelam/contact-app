<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-6">
        <form @submit.prevent="updateContact">
          <div class="form-group">
            <input
              type="text"
              v-model="contact.name"
              class="form-control"
              placeholder="Enter name"
            />
          </div>

          <div class="form-group mt-4">
            <input
              type="email"
              v-model="contact.email"
              class="form-control"
              placeholder="Enter email"
            />
          </div>

          <div class="form-group mt-4">
            <input
              type="text"
              v-model="contact.phone"
              class="form-control"
              placeholder="Enter phone number"
            />
          </div>

          <div class="form-group mt-4">
            <input
              type="text"
              v-model="contact.designation"
              class="form-control"
              placeholder="Enter designation"
            />
          </div>
          <button type="submit" class="btn btn-primary btn-sm mt-4">
            Update
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useRouter, useRoute } from 'vue-router';
import { useToast } from 'vue-toastification';

const route = useRoute();
const router = useRouter();

const toast = useToast();

const contact = ref({
  name: '',
  email: '',
  phone: '',
  designation: '',
});

const getContactById = async () => {
  try {
    const url = `http://localhost:3000/api/contacts/${route.params.id}`;
    const response = await axios.get(url);
    contact.value = response.data;
  } catch (error) {
    console.log(error);
  }
};

const updateContact = async () => {
  if (
    !contact.value.name ||
    !contact.value.email ||
    !contact.value.phone ||
    !contact.value.designation
  ) {
    toast.error('All fields are required');
  }

  try {
    const url = `http://localhost:3000/api/contacts/${route.params.id}`;
    const response = await axios.put(url, contact.value);
    console.log(response.data);
    if (response.status === 200) {
      toast.success('Contact updated!');
      router.push({ name: 'ContactList' });
    }
  } catch (error) {
    console.log(error);
  }
};

onMounted(() => {
  getContactById();
});
</script>
