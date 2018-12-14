<template>
    <div class="home">
        <div class="input-container">
            <div class="input-group">
                <!-- <input type="text" v-on:keydown.enter="getWord" v-model="word" /> -->
                <input type="text" v-model="location" />
                <button @click="getCams" type="submit">
                    Get Cameras
                </button>
            </div>
            <div class="word-meaning">
                {{ wordMeaning || error }}
                <div class="word-links" v-if="wordData">
                    <router-link v-bind:to="{ name: 'Word', params: { id: word, word: word, data:wordData } }">Read More</router-link>
                    <router-link v-bind:to="{ name: 'WordList'}">Word List</router-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import WordService from '@/services/WordService'
    import axios from 'axios'
    export default {
        name: 'Home',
        data () {
            return {
                word: '',
                wordData: '',
                error: '',
                location: ''
            }
        },
        computed: {
            wordMeaning () {
                if (this.wordData) {
                    return this.wordData.senses[0].definition
                }
                return ''
            }
        },
        methods: {
            async getWord () {
                if (this.word === '') {
                    this.error = 'Please enter a word.'
                    this.wordData = ''
                    return false
                }
                const response = await WordService.getWord({ word: this.word })

                let responses = response.data.results

                if (responses.length === 0) {
                    this.error = 'Your word could not be found and was not added.'
                    this.wordData = ''
                    return false
                }

                this.wordData = responses[0]
            },
            getCams () {
                if (this.location === '') {
                    this.error = 'Please enter a location.'
                    return false
                }
                console.log('is it even hitting this?')

            //    axios.get({
            //         baseURL: "https://webcamstravel.p.rapidapi.com/webcams/list/country=United%20States?lang=en&show=webcams%3Aimage%2Clocation",
            //         headers: {
            //             'X-Mashape-Key': 'JviyEmZi47msh0CazXqu01lns7yip1Ak2uPjsnJUCMan059fuE',
            //             'X-Mashape-Host': 'webcamstravel.p.rapidapi.com'
            //         }
            //     }).then(function(response){
            //         console.log(response)
            //     })
                axios({
                    url:'localhost:8000/test',
                    method:'GET',
                    headers:{
                        'Content-Type':'application/x-www-form-urlencoded'
                    }

                }).then(function(response){
                    console.log(response)
                }).catch(function(err){
                        console.error(err)
                })
                // axios
                //     .get('localhost:8000/test',
                //         {
                //             headers : {
                //                 'Content-Type':'application/x-www-form-urlencoded',
                //                 'Access-Control-Allow-Origin':'*',
                //                 'Access-Control-Allow-Methods': '*',
                //                 'Access-Control-Allow-Headers': '*'

                //             }
                //         })
                //     .then(function(response) {
                //         console.log(response)
                //     }).catch(function(err){
                //         console.error(err)
                //     })

                // axios({
                //     method:'get',
                //     url:'https://webcamstravel.p.rapidapi.com/webcams/list/country=United%20States?lang=en&show=webcams%3Aimage%2Clocation',
                // }) .then(function(response) {
                //     return console.log(response)
                //     response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
                // });
            }
        }
    }
</script>

<style lang="scss" scoped>
    .home{
        display: flex;
        align-items:center;
        justify-content: center;
        height: calc(100vh);

        .input-container{
            max-width: 500px;
            width:100%;

            .input-group{
                display:flex;
                justify-content: space-between;

                input {
                    box-sizing: border-box;
                    width: calc(100% - 150px);
                    padding: .5rem;
                    border: 2px solid royalblue;
                    font-size: 1rem;
                    border-radius: 0;
                    -webkit-appearance: none;
                }

                button{
                    width:150px;
                    height:50px;
                    -webkit-appearance: none;
                    background: royalblue;
                    color: white;
                    text-transform: uppercase;
                    border: none;
                    font-weight:bold;
                    font-size: 12pt;
                }
            }
        }

        .word-meaning {
            margin: 20px 0;
            font-size: 18px;
            font-weight: bold;

            .word-links{
                margin: 30px 0 20px 0;
                font-size: 14px;
                font-weight: regular;

                a {
                    border: 1px solid royalblue;
                    text-decoration: none;
                    padding: 5px 12px 3px 12px;
                    margin: 0 10px;
                    text-transform: uppercase;

                    &:focus {
                        color: royalblue;
                    }
                }
            }
        }
    }
</style>