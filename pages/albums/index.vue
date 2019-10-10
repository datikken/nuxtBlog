<template>
    <section>
        <h1>Albums</h1>
        <ul>
            <li v-for="album of displayedPosts" :key="album.id">
                <a href="" @click.prevent="openAlbum(album)">Album {{album.id}}</a>
            </li>
        </ul>
          <div class="clearfix btn-group col-md-2 offset-md-5">
            <button type="button" class="btn btn-sm btn-outline-secondary" v-if="page != 1" @click="page--"> prev </button>
            <button type="button" class="btn btn-sm btn-outline-secondary" v-for="pageNumber in pages.slice(page-1, page+5)" @click="page = pageNumber" :key="pageNumber"> {{pageNumber}} </button>
            <button type="button" @click="page++" v-if="page < pages.length" class="btn btn-sm btn-outline-secondary"> next </button>
          </div>
    </section>
</template>

<script>
import axios from 'axios';

export default {
    data: () => ({
        posts: [],
        baseUrl: 'https://jsonplaceholder.typicode.com/photos?_start=0&_limit=25',
        page: 1,
        perPage: 9,
        pages: [],
    }),
    methods: {
        getPosts () {
            axios.get(this.baseUrl)
            .then(response => {
                this.posts = response.data;
            })
            .catch(response => {
                console.log(response);
            });
        },
        setPages () {
            let numberOfPages = Math.ceil(this.posts.length / this.perPage);
            for (let index = 1; index <= numberOfPages; index++) {
                this.pages.push(index);
            }
        },
        paginate (posts) {
            let page = this.page;
            let perPage = this.perPage;
            let from = (page * perPage) - perPage;
            let to = (page * perPage);
            return  posts.slice(from, to);
        },
        openAlbum(album) {
            this.$router.push('/albums/' + album.id);
        }
    },
    computed: {
        displayedPosts () {
            return this.paginate(this.posts);
        }
    },
    watch: {
        posts () {
            this.setPages();
        }
    },
    created () {
        this.getPosts();
    }
}
</script>