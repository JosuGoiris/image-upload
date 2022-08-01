<template>
    <div class="container mt-4">
        <div class="text-center">
            <h1>Subir Imagenes con Vue 3</h1>
        </div>

        <div class="text-center">
            <p v-if="success.value == true" class="text-success">Imagen subida!</p>
        </div>

        <div class="text-center mt-5">
            <form @submit="submitForm" enctype="multipart/form-data">
                <input type="file" @change="onChange">
                <div class="mt-3">
                    <button type="submit" class="btn btn-primary">Subir</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { ref } from 'vue';

    export default {
        setup() {
            const file = ref('');
            const success = false;

            const onChange = (e) => {
                file.value = e.target.files[0];
            }

            const submitForm = (e) => {
                e.preventDefault();
                const config = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                }
                let data = new FormData();
                data.append('file', file.value);
                axios.post('/api/upload', data, config)
                    .then(function (res) {
                        success.value = true;
                    })
                    .catch(function (err) {
                        console.log(err)
                        output = err;
                    });
            }

            return {
                onChange,
                submitForm,
                success,
            }
        }
    }
</script>
