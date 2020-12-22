<style>
    *{
        margin: 0;
        padding: 0;
        overflow: hidden;
    }
    .main{
        display: flex;
    }
    .sidebar{
        width: 20%;
        height: 100vh;
        background-color: #292F4C;
    }
    .header{
        width: 100%;
        height: 7vh;
        background-color: white;
    }
    .content{
        width: 80%;
    }

    .task{
        width: 50%;
        background-color: #E6E9EF;
        border: 1px solid #E6E9EF;
        padding: 10px;
    }

    .container-task{
        margin-top: 1px;
        display: flex;
    }

    .image-person{
        width: 25px;
        height: 25px;
    }

    .image-person img{
        border-radius: 50%;
        width: 25px;
        height: 25px;
        object-fit: contain;
        object-position: center;
    }

    .profile-image{
        width: 100%;
        height: 300px;
        background-color: red;
    }

    .profile-image img{
        width: 100%;
        height: 300px;
        object-fit: cover;
        object-position: center;
    }

    .sidebar-2{
        width: 40vw;
        height: 100vh;
        background-color: white;
        position: absolute;
        top: 0;
        right: 0;
        -webkit-box-shadow: -3px 0px 5px 0px rgba(0,0,0,0.75);
        -moz-box-shadow: -3px 0px 5px 0px rgba(0,0,0,0.75);
        box-shadow: -3px 0px 5px 0px rgba(0,0,0,0.75);
        transform: translateX(40vw);
        transition: all 0.5s;
    }

    .sidebar-2.active{
        transform: translateX(0);
    }

    .comment{
        margin-top: 20px;
        border: 1px solid #E6E9EF;
        padding: 10px;
        border-radius: 10px;
    }
</style>

<template>
    <section class="main">
        <div class="sidebar">
            <div class="profile-image">
                <img src="https://i.pinimg.com/originals/43/61/10/436110680b5f57146eaf3548455bb89e.jpg" alt="">
            </div>
        </div>
        <div class="content">
            <div class="header"></div>
            
            <div class="projects p-2">
                <h2 class="m-1">Programacion</h2>

                <p style="width: 50%;" class="mt-2">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Et a assumenda nam, odio distinctio recusandae at possimus expedita tempore eaque pariatur cum labore eveniet ex quo unde beatae magni earum!</p>
                
                <p>Group 1</p>

                <div v-for="(task, index) in tasks" :key="index" class="container-task">
                    <div class="task" @click="selectTask(task)">
                        {{ task.title }}
                    </div>
                    <div style="width: 200px; display: flex; justify-content: center; align-items: center;">
                        <div style="cursor: pointer;" @click="removePerson(task, indexP)" class="image-person" v-for="(person, indexP) in task.persons" :key="indexP">
                            <img :src="person.photo" alt="">
                        </div>
                        <div style="cursor: pointer;" class="image-person" @click="tasIndex = index" data-toggle="modal" data-target="#exampleModalCenter">
                            <img src="https://st4.depositphotos.com/1156795/20814/v/600/depositphotos_208142514-stock-illustration-profile-placeholder-image-gray-silhouette.jpg" alt="">
                        </div>
                    </div>
                    <div style="width: 200px; display: flex; justify-content: center; align-items: center;">
                        <span>Componente 1</span>
                    </div>
                </div>
            </div>
            <div class="sidebar-2" :class="[active ? 'active' : '']">
                <span class="mt-2 ml-2" @click="active = false" style="cursor: pointer;">X</span>

                <div class="mt-5 ml-2" v-if="editTask != null">
                    <h2>{{ editTask.title }}</h2>

                    <div class="mt-5" style="width: 100%; height: 200px;">
                        <Editor v-model="data" :image-provider="imageProvider" />
                    </div>
                    <div>
                        <button @click="saveComment" class="btn btn-info">save</button>
                    </div>
                    <div>
                        <div class="comment" v-for="(item, index) in editTask.comments" :key="index" v-html="item">
                            
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLongTitle">Asignar responsable</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <ul class="list-group">
                        <li class="list-group-item" style="display: flex" v-for="(person, index) in persons" :key="index" @click="getPerson(person)" data-dismiss="modal">
                            <div class="image-person">
                                <img :src="person.photo" alt="">
                            </div>
                            <p class="ml-4">{{ person.name }}</p>
                        </li>
                    </ul>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>
                </div>
                </div>
            </div>
        </div>
        
    </section>
</template>

<script>
// Import the editor
import Editor from '@baoshishu/vue-editor'
export default {
    components: {
        Editor,
    },

    data(){
        return{
            data: '',
            imageProvider: {
                name: 'qiniu', // provider name
                token:
                '-qWchT63mkZEJch0ygm3bN9h3peInHqCcSAEMtvV:9YAz4dCiB3EAdYuoDVO0YvObtqY=:eyJzY29wZSI6InRlc3QiLCJkZWFkbGluZSI6MTkwMjAyODY1NX0=', // upload token
                domain: 'cdn-testing.zanquan.net', // upload domain
                
            },

            active: false,
            tasIndex: 0,

            editTask: null,

            tasks: [
                {
                    title: 'Tarea numero 1',
                    persons: [],
                    comments: [

                    ]
                },
                {
                    title: 'Tarea numero 2',
                    persons: [],
                    comments: [

                    ]
                },
                {
                    title: 'Tarea numero 3',
                    persons: [],
                    comments: [

                    ]
                }
            ],
            persons: [
                {
                    name: 'Paola Gomez',
                    photo: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQAGpCfyaBWKtITCtTzXR8hpdQAvJpmg7-PCQ&usqp=CAU'
                },
                {
                    name: 'Miguel de la Madrid',
                    photo: 'https://0xpatrik.com/content/images/2018/04/patrik_hudak.jpeg'
                },
                {
                    name: 'Juan Carlos Perez',
                    photo: 'https://mindbodygreen-res.cloudinary.com/images/w_767,q_auto:eco,f_auto,fl_lossy/usr/QKNTXGf/james-dinicolantonio-pharmd.jpg'
                }
            ]
        }
    },

    mounted() {
        
    },

    methods: {
        getPerson(args){
            let found = this.tasks.find((element, index) => index == this.tasIndex)

            found.persons.push(args)
            
        },

        selectTask(args){
           this.editTask = args
           this.active = true
        },

        removePerson(args, index){
            args.persons.splice(index, 1)
        },

        saveComment(){
            this.editTask.comments.push(this.data)

            this.data = ''
        }
    },
}
</script>


