<template>
    <div>
        <section>
            <div class="container">
                <h1>Contatta l'Host</h1>

                <div v-if="success"><h3 style="color:green">L'Email è stata inviata correttamente!</h3></div>

                <form>
                    <div class="mb-3">
                        <label for="email" class="form-label">Indirizzo Email</label>
                        <input v-model="email" type="email" class="form-control" id="email">
                    </div>

                    <div v-if="errors.email">
                        <p v-for="(error, index) in errors.email" :key="index">{{ error }}</p>
                    </div>

                    <div class="mb-3">
                        <label for="name" class="form-label">Nome</label>
                        <input v-model="name" type="text" class="form-control" id="name">
                    </div>

                    <div v-if="errors.name">
                        <p v-for="(error, index) in errors.name" :key="index">{{ error }}</p>
                    </div>

                    <div class="mb-3">
                        <label for="object" class="form-label">Oggetto dell'Email</label>
                        <input v-model="object" type="text" class="form-control" id="object">
                    </div>

                    <div v-if="errors.object">
                        <p v-for="(error, index) in errors.object" :key="index">{{ error }}</p>
                    </div>

                    <div class="mb-3">
                        <label for="message" class="form-label">Testo dell'Email</label>
                        <textarea v-model="message" class="form-control" id="message" cols="30" rows="10"></textarea>
                    </div>

                    <div v-if="errors.message">
                        <p v-for="(error, index) in errors.message" :key="index">{{ error }}</p>
                    </div>

                    <button type="submit" @click.prevent="sendMessage()" class="btn btn-primary">Invia</button>
                </form>
            </div>
        </section>
    </div>
</template>


<script>
export default {
    name: 'ContactHost',
    data: function() {
        return {
            location_id: this.$route.params.id,
            email: '',
            name: '',
            object: '',
            message: '',
            success: false,
            errors: {}
        };
    },
    methods: {
        getUser() {
        axios.get('/api/leads/take')
        .then((response) => {
            if(response) {
                console.log(response)
                // this.user = response.data.results;
                // this.name = this.user.name;
                // this.email = this.user.email;
            }
            else {
                this.$router.push({ name: 'not-found' });
            }
        });
        },
        sendMessage: function() {
            axios.post('/api/leads/store', {
                location_id: this.location_id,
                email: this.email,
                name: this.name,
                object: this.object,
                message: this.message
            })
            .then((response) => {
                if(response.data.success) {
                    this.location_id = '';
                    this.name = '';
                    this.email = '';
                    this.object = '';
                    this.message = '';
                    this.success = true;
                    this.error = {};
                } else {
                    this.success = false;
                    this.errors = response.data.errors
                }
            });
        }
    },
    created: function(){
        this.getUser();
    }
}
</script>

<style lang="scss" scoped>

</style>
