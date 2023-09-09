<script setup>
import { ref } from "vue";
import contacts from "./contacts.json";

let sortedContactList = ref(contacts.slice(0, 5));

function addRandomContact() {
  const randomContactIndex = Math.floor(Math.random() * contacts.length);
  const randomContact = contacts[randomContactIndex];

  const isContactAlreadyAdded = sortedContactList.value.some(
    (contact) => contact.id === randomContact.id
  );

  if (
    !isContactAlreadyAdded &&
    sortedContactList.value.length < contacts.length
  ) {
    sortedContactList.value.push(randomContact);
  } else if (sortedContactList.value.length === contacts.length) {
    alert("No more contacts available to add.");
  } else {
    const nextRandomContact = contacts.find(
      (contact) =>
        !sortedContactList.value.some(
          (addedContact) => addedContact.id === contact.id
        )
    );
    sortedContactList.value.push(nextRandomContact);
  }
  
}
let sortNameStatus = ref(true);
function sortAlphabetically() {
  if (sortNameStatus) {
    sortedContactList.value.sort((a, b) => a.name.localeCompare(b.name));
    sortNameStatus = false;
  } else {
    sortedContactList.value.sort((a, b) => b.name.localeCompare(a.name));
    sortNameStatus = true;
  }
}

let sortPopularityStatus = ref(true);
function sortByPopularity() {
  if (sortPopularityStatus) {
    sortedContactList.value.sort((a, b) => a.popularity - b.popularity);
    sortPopularityStatus = false;
  } else {
    sortedContactList.value.sort((a, b) => b.popularity - a.popularity);
    sortPopularityStatus = true;
  }
}

function deleteContact(index) {
  sortedContactList.value.splice(index, 1);
}
</script>
<template>
  <div class="main-container">
    <h1>Iron contacts</h1>
    <button @click="addRandomContact">Add Random Contact</button>
    <button @click="sortAlphabetically">Sort by Name</button>
    <button @click="sortByPopularity">Sort by Popularity</button>
    <table>
      <tbody>
        <tr>
          <th>Picture</th>
          <td>Name</td>
          <td>Popularity</td>
          <td>Won Oscar</td>
          <td>Won Emmy</td>
          <td>Actions</td>
        </tr>
        <tr v-for="(contact, index) in sortedContactList" :key="index">
          <td><img :src="contact.pictureUrl" alt="" /></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity }}</td>
          <td v-if="contact.wonOscar === true">🏆</td>
          <td v-else></td>
          <td v-if="contact.wonEmmy === true">&#11088</td>
          <td v-else></td>
          <td><button @click="deleteContact(index)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<style>
.main-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
}

button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  cursor: pointer;
  font-size: 16px;
  border-radius: 4px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th {
  background-color: #f0f0f0;
  font-weight: bold;
  padding: 10px;
}

td {
  padding: 10px;
  border: 1px solid #ddd;
}

img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  object-fit: cover;
}

button.delete-button {
  background-color: #d9534f;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 4px;
  font-size: 14px;
  transition: background-color 0.3s ease;
}

button.delete-button:hover {
  background-color: #c9302c;
}

td.award {
  font-size: 20px;
  color: #ffc107;
}

</style>