<template>
    <div class="container mt-2">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card card-default">
                    <div class="card-header">Articals</div>
                    <div class="card-body">
                        
                        
                            <div v-for="item in articals" :key="item.id" class="card mb-4">
                                <div class="card-header bg-primary text-white" v-text="item.title"></div>
                                <div class="card-body">
                                    {{ item.body }}
                                </div>
                            </div>

                            <ul class="pagination">
                                <li @click="fetchData(pagination.prev_page_url)" :class="[pagination.prev_page_url ? '' : 'disabled', 'page-item']"><a class="page-link" href="#">Previous</a></li>
                                <li @click="fetchData(pagination.first_page_url)" :class="[pagination.first_page_url ? '' : 'disabled', 'page-item']"><a class="page-link" href="#">First</a></li>
                                
                                 <li v-for="page in pagesNumbers" :key="page" :class="[page == pagination.current_page ? 'active': '', 'page-item' ]">
                                    <a href="#" class="page-link" @click.prevent="fetchData(pagination.path + '?page=' +page)">{{ page }}</a>
                                </li>
                                
                                <li @click="fetchData(pagination.last_page_url)" :class="[pagination.last_page_url ? '' : 'disabled', 'page-item']"><a class="page-link" href="#">Last</a></li>
                                <li @click="fetchData(pagination.next_page_url)" :class="[pagination.next_page_url ? '' : 'disabled', 'page-item']"><a class="page-link" href="#">Next</a></li>
                            </ul>


                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                articals: [],
                pagination: {},
                pagesNumbers: []
            }
        },

        mounted() {
            this.fetchData();
        },

        methods: {
            fetchData(url) {
                url = url || '/api/articales'
                axios.get(url).then(res => res).then(res => {
                    this.articals = res.data.data
                    this.makePagination(res.data)
                })
            },

            makePagination(data) {
                let pagination = {
                    first_page_url: data.first_page_url,
                    next_page_url: data.next_page_url,
                    last_page_url: data.last_page_url,
                    current_page: data.current_page,
                    prev_page_url: data.prev_page_url,
                    last_page: data.last_page,
                    per_page: data.per_page,
                    total: data.total,
                    from: data.from,
                    to: data.to,
                    path: data.path
                }
                this.pagination = pagination
                this.setPageNumbers(pagination)
            },

            setPageNumbers(pagination) { 
                if (!pagination.to) { 
                    return [];  
                }
                let from = pagination.current_page - 3;
                if (from < 1) {
                    from = 1;
                }
                let to = from + (3 * 2);
                if (to >= pagination.last_page) {
                    to = pagination.last_page;
                }
                let pagesArray = [];
                for (let page = from; page <= to; page++) {
                    pagesArray.push(page);
                }

                this.pagesNumbers = pagesArray;
                
            }
        }
    }
</script>
