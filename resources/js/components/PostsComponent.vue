<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card mb-3 mt-3" v-for="item in list" :key="item.id">
                    <a class="card-header" v-bind:href="item.slug" v-text="item.title"></a>

                    <div class="card-body">
                        <p>Item: {{ item.id }}</p>
                        <p class="card-text" v-text="item.body"></p>
                    </div>
                </div>

                <infinite-loading @infinite="infiniteHandler"></infinite-loading>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                list: [],
                page: 0
            };
        },
        methods:{
            infiniteHandler($state) {
                this.page++;
                console.log(this.page);
                let url = 'http://vuejslaravel.test/api/posts?page=' + this.page;

                axios.get(url)
                .then(response => {
                    let posts = response.data.data;

                    
                    if (posts.length) {
                        this.list = this.list.concat(posts);
                        // [].push.apply(this.list, posts);
                        console.log(this.list);
                        $state.loaded();
                    }else{
                        $state.complete();
                    }
                });
            }
        }
    }
</script>
