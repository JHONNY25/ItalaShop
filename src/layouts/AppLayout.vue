 <template>
    <v-container fluid class="m-0 p-0" style="height:100vh;">
        <v-row>
            <v-col cols="10">
                <v-app-bar fixed flat dark style="margin: 0 auto;">
                    <v-app-bar-nav-icon></v-app-bar-nav-icon>

                    <v-btn depressed href="/" class="text-xl-h5 text-decoration-none white--text">ITALA</v-btn>

                    <v-spacer></v-spacer>

                    <template>
                        <v-text-field class="mb-0" color="secondary lighten-5" outlined single-line label="Search ..." prepend-inner-icon="mdi-magnify" style="max-width: 400px; height: 52px !important;"></v-text-field>
                    </template>

                    <v-spacer></v-spacer>

                    <v-menu offset-y left transition="scale-transition">
                        <template v-slot:activator="{ attrs, on }">
                            <v-btn
                            class="white--text ma-8"
                            v-bind="attrs"
                            v-on="on"
                            icon
                            >
                            <v-icon>mdi-account</v-icon>
                            </v-btn>
                        </template>

                        <v-list>
                            <v-list-item link href="/login" v-if="$store.state.token === null">
                                <v-list-item-title v-text="'Iniciar sesión'"></v-list-item-title>
                            </v-list-item>
                            <v-list-item link v-if="$store.state.token !== null">
                                <v-list-item-title v-text="'Nuevo producto'"></v-list-item-title>
                            </v-list-item>
                            <v-list-item link v-if="$store.state.token !== null">
                                <v-list-item-title v-text="'Configuración'"></v-list-item-title>
                            </v-list-item>
                            <v-list-item link @click="logout" v-if="$store.state.token !== null">
                                <v-list-item-title v-text="'Cerrar sesión'"></v-list-item-title>
                            </v-list-item>
                        </v-list>
                    </v-menu>

                    <v-btn icon href="/shopping-cart">
                        <v-icon>mdi-cart</v-icon>
                    </v-btn>

                </v-app-bar>
            </v-col>
        </v-row>

        <slot name="carousel"></slot>

        <v-container>
            <v-row>
                <v-col cols="10" style="max-width: 1200px; margin: 0 auto;">
                    <slot name="content"></slot>
                </v-col>
            </v-row>
        </v-container>

        <v-row>
            <v-footer width="100%" class="flex justify-center" dark>
                <v-col cols="7">
                    <div class="white--text text-center">
                        <v-card-text>
                            <v-btn
                            v-for="icon in icons"
                            :key="icon"
                            class="mx-4 white--text"
                            icon
                            >
                            <v-icon size="24px">
                                {{ icon }}
                            </v-icon>
                            </v-btn>
                        </v-card-text>

                        <v-card-text class="white--text pt-0 text-xl-h6 font-weight-ligh">
                            ITALA
                        </v-card-text>

                        <v-divider></v-divider>

                        <v-card-text class="white--text">
                            {{ new Date().getFullYear() }} — <strong>ITALA</strong>
                        </v-card-text>
                    </div>
                </v-col>
            </v-footer>
        </v-row>
    </v-container>
</template>

<script>
    import gql from 'graphql-tag'
    import {onLogout} from '../vue-apollo.js'

    export default{
        data(){
            return{
                icons: [
                    'mdi-facebook',
                    'mdi-twitter',
                    'mdi-linkedin',
                    'mdi-instagram',
                ]
            }
        },
        methods:{
            async logout(){
                await this.$apollo.mutate({
                    mutation: gql`mutation{
                        logout{
                            status
                        }
                    }`
                })

                onLogout(this.$apollo.provider.defaultClient)
                this.$router.push('/login')
            }
        }
    }
</script>
<style>
    .v-text-field.v-text-field--solo .v-input__control {
        min-height: 32px;
    }

    .v-toolbar__content {
        margin: 0 auto;
        max-width: 1200px;
    }
</style>