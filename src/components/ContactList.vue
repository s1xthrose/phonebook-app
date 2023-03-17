<script lang="ts" setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import { architectSDK, Contact } from "../architectSDKConfig";
import Header from "./Header.vue";
import index from '../router/index'

const contacts = ref<Contact[]>([]);
const router = useRouter();

const getContacts = async () => {
    try {
        contacts.value = await architectSDK.contact.getAll();
    } catch (e) {
        console.log(e);
    }
};

const deleteContact = async (id: string) => {
    try {
        await architectSDK.contact.delete(id);
        contacts.value = contacts.value.filter((contact) => contact.id !== id);
    } catch (e) {
        console.log(e);
    }
};

const editContact = (id: string) => router.push("/contact/" + id);

onMounted(getContacts);



const searchContacts = async (value: string) => {
    contacts.value = contacts.value.filter(contact => {
        return contact.firstName.toLowerCase().includes(value.toLowerCase()) ||
            contact.lastName.toLowerCase().includes(value.toLowerCase()) ||
            contact.phone.toLowerCase().includes(value.toLowerCase()) ||
            contact.email.toLowerCase().includes(value.toLowerCase());
    });
};

const addToFavorites = async (id: string) => {
    try {
        const contact = contacts.value.find(contact => contact.id === id);
        if (contact) {
            const updatedContact = {
                ...contact,
                isFavorite: true
            };
            
        }
    } catch (e) {
        console.log(e);
    }
};

</script>

<template>
    <link href="https://use.fontawesome.com/releases/v5.7.2/css/all.css" rel="stylesheet" />
    <Header />
    <div class="wrapper">
        <div class="list-header">
            <h3>Список контактов</h3>

            <input type="text" class="ConSearch" placeholder="Поиск" @input="searchContacts($event.target.value)" />

            <router-link to="/contact">
                <button class="btn btn-purple">Добавить</button>
            </router-link>
        </div>
        <ul>
            <li v-for="contact of contacts" :key="contact.id" @click="editContact(contact.id)">
                <div class="info">
                    <img v-if="contact.pictureUrl" :src="contact.pictureUrl" width="48" height="48" />
                    <div class="personal-info">
                        <div class="purple-text">
                            {{ contact.firstName }} {{ contact.lastName }}
                        </div>
                        <div class="gray-text">{{ contact.phone }}</div>
                    </div>
                </div>
                <div class="gray-text">{{ contact.email }}</div>
                <div class="info-butns">
                    <i class="fas fa-heart favorite" @click.stop="addToFavorites(contact.id)"></i>
                    <i class="fas fa-times delete" @click.stop="deleteContact(contact.id)"></i>
                </div>
            </li>
        </ul>
    </div>
</template>

<style scoped>
.wrapper {
    padding: 20px;
    box-shadow: 0 0 0 2px rgb(216, 216, 216);
    margin: auto;
}

.list-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-inline-start: 40px;
}

ul {
    list-style-type: none;
}

li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 85px;
    border-top: 1px solid rgb(216, 216, 216);
    padding: 10px 0;
    cursor: pointer;
}

.info {
    display: inline-flex;
    gap: 10px;
}

.personal-info>div {
    margin-bottom: 10px;
}

.purple-text {
    color: rgb(79, 70, 229);
    font-weight: 500;
}

.gray-text {
    color: rgb(107, 114, 128);
}

.delete {
    color: red;
    margin-left: 5%;
    margin-top: 40%;
}

.ConSearch {
    font-family: Georgia, "Times New Roman", Times, serif;
    background: rgba(255, 255, 255, .1);
    border: none;
    border-radius: 4px;
    font-size: 15px;
    margin: 0;
    outline: 0;
    padding: 10px;
    width: 40%;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    background-color: #e8eeef;
    color: #8a97a0;
    -webkit-box-shadow: 0 1px 0 rgba(0, 0, 0, 0.03) inset;
    box-shadow: 0 1px 0 rgba(0, 0, 0, 0.03) inset;

}

.favorite {
    color: green;
}

.info-butns
{
    position: relative;
    justify-content: space-between;
    
}

</style>