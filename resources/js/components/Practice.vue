<template>
    <div>
        <div>
            <h2>{{ ProjectName }}</h2>
        </div>

        <div class="form-group">
            <label for="name">Enter Name</label>
            <input type="text" v-model="item.name" name="name" id="name" class="form-control" placeholder="Enter Name">
        </div>
        <div class="form-group">
            <label for="phone">Enter Phone</label>
            <input type="text" v-model="item.phone" name="phone" id="phone" class="form-control"
                   placeholder="Enter Phone">
        </div>
        <button class="btn btn-success btn-block" @click="save">
            {{ isEditing ? "Update" : "Save" }}
        </button>

        <div class="mt-5" v-if="list.length > 0">
            <h2 class="text-center"> Telephone Record </h2>
            <ul class="list-group">
                <li class="list-group-item"
                    v-for="item in list" :key="item.id"
                >
                    {{ item.name }}
                    <span class="float-right">
                   <button class="btn btn-warning btn-sm" @click="editItem(item)">Edit</button>
                   <button class="btn btn-danger btn-sm" @click="deleteTel( item.id )">Delete</button>
               </span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>

export default {
    name: "Practice",
    data() {
        return {
            ProjectName: "Laravel Vue Js Crud Practice Project",
            list: [],
            item: {
                name: "",
                phone: "",
            },
            temp_id: null,
            isEditing: false,
        }
    },
    mounted() {
        this.fetchAll();
    },
    methods: {
        fetchAll() {
            axios.get('api/tel')
                .then(res => this.list = res.data)
        },
        save() {
            let method = axios.post
            let url = "/api/tel"
            if (this.temp_id) {
                method = axios.put
                url = `/api/tel/${this.temp_id}`
            }

            try {
                var reslt = method(url, this.item)
                    .then(res => {
                        this.fetchAll()
                        this.item = {
                            name: "",
                            phone: ""
                        }
                        this.temp_id = null
                        this.isEditing = false
                    })
            } catch (e) {
                console.log(e)
            }
        },
        deleteTel(id) {
            try {
                axios.delete(`/api/tel/${id}`).then(res => this.fetchAll())

            } catch (e) {
                console.log(e);
            }
        },


        editItem(tel) {
            this.item = {
                name: tel.name,
                phone: tel.phone
            }
            this.temp_id = tel.id;
            this.isEditing = true;

        }
    }
}

</script>

<style scoped>

</style>
