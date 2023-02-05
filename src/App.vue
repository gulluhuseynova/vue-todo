<template>
    <v-app>
        <v-main>
            <v-container>
                <v-row justify="center">
                    <v-col cols="12" sm="6" md="4">
                        <div id="title" style="text-align: center; font-size: 40px; margin-bottom: 20px">Todo App</div>
                        <v-row justify="center">
                            <v-col cols="auto">
                                <v-card color="#c6b1d37e" style="width: 250px">
                                    <p style="font-family: Kaushan Script, cursive; text-align: center; font-size: 25px">Timer</p>
                                    <v-row no-gutters>
                                        <v-col style="text-align: center">
                                            <v-btn color="rgba(146, 123, 340, 0.207)" elevation="4" x-small class="stopwatch" @click="start"
                                                >Start</v-btn
                                            >
                                            <v-btn color="rgba(146, 123, 340, 0.207)" elevation="4" x-small class="stopwatch" @click="stop"
                                                >Stop</v-btn
                                            >
                                            <v-btn color="rgba(146, 123, 340, 0.207)" elevation="4" x-small class="stopwatch" @click="reset"
                                                >Reset</v-btn
                                            >
                                            <p id="count">{{ formattedElapsedTime }}</p>
                                        </v-col>
                                    </v-row>
                                </v-card>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col>
                                <label style="font-weight: bold; font-family: Kaushan Script, cursive">New Todo</label>
                                <v-text-field
                                    hide-details
                                    v-model="todo"
                                    solo
                                    dense
                                    background-color="rgba(146, 123, 140, 0.507)"
                                    @keyup.enter="addTodo"
                                >
                                    <!-- @keyup.enter="addTodo" add btn yerine enter -->
                                </v-text-field>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col>
                                <v-btn @click="addTodo" block color="rgba(146, 123, 150, 0.507)"> Add </v-btn>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col>
                                <div id="title2">Todo List</div>
                            </v-col>
                        </v-row>
                        <div id="list">
                            <v-row class="list-item" v-for="(todoObject, i) in todos" :key="i">
                                <v-col id="text">
                                    <v-checkbox v-model="todoObject.isChecked" :label="todoObject.text" color="pink"></v-checkbox>
                                </v-col>
                                <v-col cols="auto" style="align-self: center">
                                    <v-btn @click="removeTodo(i)" elevation="2" x-small dark color="rgba(146, 123, 150, 0.507)">Remove</v-btn>
                                </v-col>
                            </v-row>
                        </div>
                    </v-col>
                </v-row>
            </v-container>
        </v-main>
    </v-app>
</template>

<script>
export default {
    name: "App",

    components: {},

    data: () => ({
        todos: [],
        todo: "",

        elapsedTime: 0,
        timer: undefined,
    }),

    computed: {
        formattedElapsedTime() {
            const date = new Date(null);
            date.setSeconds(this.elapsedTime / 1000);
            const utc = date.toUTCString();
            return utc.substr(utc.indexOf(":") - 2, 8);
        },
    },
    methods: {
        addTodo() {
            if (this.todo === "") {
                alert("You must write something!");
            } else if (this.todo.length > 100) {
                alert("ERROR");
            } else {
                var todoObj = {
                    text: this.todo,
                    isChecked: false,
                };
                this.todos.push(todoObj);
                this.todo = "";
            }
        },
        removeTodo(index) {
            this.todos.splice(index, 1);
        },
        //stopwatch
        start() {
            this.timer = setInterval(() => {
                this.elapsedTime += 1000;
            }, 1000);
        },
        stop() {
            clearInterval(this.timer);
        },
        reset() {
            this.elapsedTime = 0;
            clearInterval(this.timer);
        },
    },
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Kaushan+Script&family=Licorice&display=swap");
.v-application {
    background-color: rgba(181, 153, 173, 0.507) !important ;
}
#title {
    font-family: "Kaushan Script", cursive;
}
#title2 {
    font-family: "Kaushan Script", cursive;
    display: flex;
    justify-content: left;
    font-size: 30px;
    border-bottom: 2px solid;
}
#list {
    max-height: 300px;
    overflow: auto;

    .list-item {
        border: 2px solid hsla(0, 0%, 0%, 0.35);
        border-radius: 6px;
        margin: 10px 0;
        background-color: rgba(146, 123, 140, 0.507);
    }
}

// scroll
::-webkit-scrollbar-track {
    --webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    background-color: #c6b1d37e;
}

::-webkit-scrollbar {
    width: 6px;
    background-color: #f5f5f5;
}

::-webkit-scrollbar-thumb {
    background-color: #c292b7;
}
#text {
    font-family: "Kaushan Script", cursive;
    text-transform: uppercase;

    ::v-deep .v-label {
        overflow-wrap: anywhere !important;
    }
}

//
.stopwatch {
    margin: 5px;
    background-color: rgba(255, 192, 203, 0.363);
    border: 1px solid grey;
}
#count {
    text-align: center;
}
</style>
