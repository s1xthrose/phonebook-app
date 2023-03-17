<script lang="ts" setup>
import { useRouter } from "vue-router";
import { architectSDK } from "../architectSDKConfig";
import ContactList from "./ContactList.vue";
const router = useRouter();

const logout = async () => {
    try {
        await architectSDK.logout();
        router.push("/");
    } catch (e) {
        console.log(e);
    }
};


</script>


<template>
    <div class="header">
        
        <button @click="logout" class="btn btn-purple">Выйти</button>
    </div>
</template>
<style scoped>
.SearchStroke {
position: relative;
padding-right: 100px;
margin-right: 66%;
} 
.header {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding: 20px 10px;
}

.btn-purple {
    margin-right: 1%;
    background: #CD5C5C;
}
</style>

