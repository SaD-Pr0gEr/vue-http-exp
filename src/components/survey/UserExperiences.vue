<template>
    <section>
        <base-card>
            <h2>Submitted Experiences</h2>
            <div>
                <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
            </div>
            <p v-if="isLoading">Loading...</p>
            <p v-else-if="!isLoading && error">{{ error }}</p>
            <p v-else-if="!isLoading && (!results || results.length === 0)">No data found</p>
            <ul v-else>
                <survey-result
                    v-for="result in results"
                    :key="result.id"
                    :name="result.name"
                    :rating="result.rating"
                ></survey-result>
            </ul>
        </base-card>
    </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
    components: {
        SurveyResult,
    },
    data() {
        return {
            results: [],
            isLoading: false,
            error: null
        }
    },
    methods: {
        loadExperiences() {
            this.isLoading = true;
            fetch("https://vue-http-10ae1-default-rtdb.europe-west1.firebasedatabase.app/data.json")
                .then((response) => {if (response.ok) {
                    return response.json();
                }})
                .then((data) => {
                    this.isLoading = false
                    this.error = null
                    const results = []
                    for (const res in data) {
                        results.push({
                            id: res,
                            name: data[res].name,
                            rating: data[res].rating
                        })
                    }
                    this.results = results;
                }).catch((error) => {
                    this.isLoading = false
                    console.log(error)
                    this.error = "Can't get data... Please try again later";
                })
        }
    },
    mounted() {
        this.loadExperiences();
    }
};
</script>

<style scoped>
ul {
    list-style: none;
    margin: 0;
    padding: 0;
}
</style>
