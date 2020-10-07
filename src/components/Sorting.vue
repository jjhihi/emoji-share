<template>
    <v-container>
        <v-row class="text-center">
            <v-btn-toggle mandatory group v-model="sort" @change="sortType">
                <v-btn small value="rank">랭킹순</v-btn>
                <v-divider inset vertical></v-divider>
                <v-btn small value="low">낮은 평점순</v-btn>
                <v-divider inset vertical></v-divider>
                <v-btn small value="high">높은 평점순</v-btn>
                <v-divider inset vertical></v-divider>
                <v-btn small value="download">다운로드순</v-btn>
            </v-btn-toggle>
            <v-spacer/>
            <v-menu offset-y open-on-hover>
                <template v-slot:activator="{ on, attrs }">
                    <v-btn text small v-bind="attrs" v-on="on">
                        {{count}}개씩 보기
                        <v-icon right> mdi-chevron-down </v-icon>
                    </v-btn>
                </template>
                <v-list dense>
                    <v-list-item v-for="(item, index) in count_items" :key="index" link @click="setItem(index, item.value)">
                        <v-list-item-content>
                            <v-list-item-title v-text="item.text"></v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
            </v-menu>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: 'Sorting',

    data: () => ({
        count: 12,
        sort: "rank",
        count_items: [
            { text: '12개씩 보기', value: 12 },
            { text: '24개씩 보기', value: 24 },
            { text: '36개씩 보기', value: 36 },
            { text: '48개씩 보기', value: 48 },
        ],
    }),
    methods: {
        sortType(v) {
            this.sort = v;
        },
        setItem(e, count) {
            this.count = count
        }
    }
}
</script>
