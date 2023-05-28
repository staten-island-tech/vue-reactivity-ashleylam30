<template>
    <div class="container">
        <div v-for="(animal, index) in createList" :key="index" class="child">
            <h1>{{ animal[0] }}</h1>
            <img :src="animal[27].url" :key="animal[0]">
            <p>Scientific Name: {{ animal[2].value }}</p>
            <p>Status: {{ animal[4] }}</p>
            <a :href="animal[2].url">Learn More</a>
        </div>
    </div>
</template>
<script>
export default {
    name: "List",
    props: {
        list: {
            required: true,
        },
        ammount: {
            type: Number,
            required: true,
        }
    },
    computed: {
        createList() {
            if (this.list === undefined) {
                return [];
            } else if (this.list.length <= this.ammount) {
                return this.list
            } else {
                return this.list.slice(0, this.ammount)
            }
        }
    },
    data() {
        return {
            apiCall: [],
            animal: "",
            showAnimals: 200,
            data: []
        }
    }, mounted() {
    }
}
</script>
<style scoped>
.container {
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-wrap: wrap;
}

.child {
    margin: 1.5rem;
    text-align: center;
    background-color: rgb(255, 129, 129);
    width: 35rem;
    height: 30rem;
    border-radius: 30px;
}

img {
    height: 20rem;
    max-width: 35rem;
}

h1 {
    color: black;
    font-size: 2rem;
}

p {
    color: black;
    font-size: 1rem;
}
</style>