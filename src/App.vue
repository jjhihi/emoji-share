<template>
    <v-app>
        <v-main>
            <EmojiList/>
        </v-main>
    </v-app>
</template>

<script>
import EmojiList from './components/EmojiList';

export default {
    name: 'App',

    components: {
        EmojiList,
    },

    data: () => ({
        //
    }),
};
</script>
