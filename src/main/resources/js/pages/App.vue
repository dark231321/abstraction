<template>
    <v-app>
        <v-app-bar app>
            <v-toolbar-title>Abstraction</v-toolbar-title>
            <v-spacer/>
            <span v-if="profile">{{profile.name}}</span>
            <v-btn v-if="profile" icon href="/logout">
                <v-icon>exit_to_app</v-icon>
            </v-btn>
            <v-btn v-if="!profile" href="/login">Login</v-btn>
        </v-app-bar>
        <v-main>
            <v-container v-if="profile">
                <messages-list/>
            </v-container>
        </v-main>
    </v-app>
</template>>

<script>
    import {mapMutations, mapState} from 'vuex'
    import MessagesList from "components/messages/MessagesList.vue";
    import {addHandler} from "util/ws";

    export default {
        components: {
            MessagesList
        },
        computed: mapState(['profile']),
        methods: mapMutations(['addMessageMutation', 'updateMessageMutation', 'removeMessageMutation']),
        created() {
            addHandler(data => {
                if (data.objectType === 'MESSAGE') {
                    switch (data.eventType) {
                        case 'CREATE':
                            this.addMessageMutation(data.body)
                            break
                        case 'UPDATE':
                            this.updateMessageMutation(data.body)
                            break
                        case 'REMOVE':
                            this.removeMessageMutation(data.body)
                            break
                        default:
                            console.error(`Looks like the event type if unknown "${data.eventType}"`)
                    }
                } else {
                    console.error(`Looks like the object type if unknown "${data.objectType}"`)
                }
            })
        }
    }
</script>>

<style>

</style>
