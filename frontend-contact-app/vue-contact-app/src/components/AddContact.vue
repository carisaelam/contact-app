<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-6">
        <form @submit.prevent="saveContact">
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
          <button type="submit" class="btn btn-dark btn-sm mt-4">Submit</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';

const contact = ref({
  name: '',
  email: '',
  phone: '',
  designation: '',
});

const toast = useToast();
const router = useRouter();

const saveContact = async () => {
  console.log('Contact values are: ', contact.value);

  if (
    !contact.value.name ||
    !contact.value.email ||
    !contact.value.phone ||
    !contact.value.designation
  ) {
    toast.error('All fields are required');
  }

  try {
    const url = 'http://localhost:3000/api/contacts';
    const response = await axios.post(url, contact.value);
    console.log(response);
    if (response.status === 201) {
      toast.success('Contact added!');
      contact.value.name = '';
      contact.value.email = '';
      contact.value.phone = '';
      contact.value.designation = '';
      router.push({ name: 'ContactList' });
    }
  } catch (error) {
    console.log(error);
  }
};
</script>
