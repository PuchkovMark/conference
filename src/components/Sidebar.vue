<template>
    <div>
        <v-app-bar app :color="'indigo'">
            <v-app-bar-nav-icon style="color: white;"></v-app-bar-nav-icon>

            <v-toolbar-title style="color: white;">Главная</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn icon color="white">
                <v-icon>mdi-bell</v-icon>
            </v-btn>

            <v-btn icon>
                <v-icon>mdi-magnify</v-icon>
            </v-btn>

            <v-menu left bottom>
                <template v-slot:activator="{ on }">
                    <v-btn icon v-on="on">
                        <v-icon>mdi-dots-vertical</v-icon>
                    </v-btn>
                </template>

                <v-list>
                    <v-list-item
                            v-for="n in 5"
                            :key="n"
                            @click="() => {}"
                    >
                        <v-list-item-title>Option {{ n }}</v-list-item-title>
                    </v-list-item>
                </v-list>
            </v-menu>
        </v-app-bar>

    </div>
</template>

<script>

    export default {
    }
</script>

<style scoped>

</style>