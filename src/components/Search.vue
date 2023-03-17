<script lang="ts" setup>
import client from "architect-sdk/lib/core/Client";
import { useRouter } from "vue-router";
import { architectSDK } from "../architectSDKConfig";
import ContactList from "./ContactList.vue";
import Search from "./Search.vue"



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
</template>


<style scoped>
.SearchStroke {
    position: relative;
    padding-right: 100px;
    margin-right: 66%;
}
</style>