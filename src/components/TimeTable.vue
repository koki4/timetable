<template>
    <div>
        <v-app>
            <h2>時間割</h2>
            <v-container>
                <v-row>
                    <v-col>
                        <v-row>
                            <v-col cols="1">
                                <v-card class="time_title">
                                    <v-card-title class="justify-center">\</v-card-title>
                                </v-card>
                            </v-col>
                            <v-col v-for="day in days" :key="day" cols="2">
                                <v-card color="cyan">
                                    <v-card-title class="justify-center">{{day}}</v-card-title>
                                </v-card>
                            </v-col>
                        </v-row>
                        <v-row v-for="y in 5" :key="y">
                            <v-col v-for="x in 6" :key="x" :cols="is_zero_col(x)">
                                <v-card v-if="is_zero(x)" >
                                    <v-responsive :aspect-ratio="1/2.2">
                                        <v-card-title class="pl-2">
                                            <span class="timetitle"><span class="number">{{y}}</span>限</span>
                                        </v-card-title>
                                    </v-responsive>
                                </v-card>
                                <v-card v-else @click="push(x,y)">
                                    <v-responsive :aspect-ratio="1/1">
                                        <v-card-text class="text-center">{{ time_table[days[x-2]][y-1] }}</v-card-text>
                                    </v-responsive>
                                </v-card>
                                <v-dialog
                                    v-model="dialog"
                                    width="auto"
                                    :retain-focus="false"
                                >
                                    <v-card>
                                        <v-card-title>
                                            {{days[index_x-2]}}曜日{{index_y}}限の授業を設定してください。
                                        </v-card-title>
                                        <v-card-text>
                                            <v-text-field
                                                v-model="time_table[days[index_x-2]][index_y-1]"
                                                :counter="10"
                                                label="科目名"
                                                required
                                            ></v-text-field>
                                        </v-card-text>
                                        <v-card-actions>
                                            <v-btn color="primary" block @click="add();dialog=false;">編集</v-btn>
                                        </v-card-actions>
                                    </v-card>
                                </v-dialog>
                            </v-col>
                        </v-row>
                    </v-col>
                </v-row>
            </v-container>
        </v-app>
    </div>
</template>

<script>
import { initializeApp } from "firebase/app";
const firebaseConfig = {
    apiKey: "AIzaSyCUEmlL5Jqx_Unyd4y18EJkcdFTC7M2xVE",
    authDomain: "dragon-0000.firebaseapp.com",
    projectId: "dragon-0000",
    storageBucket: "dragon-0000.appspot.com",
    messagingSenderId: "825929187153",
    appId: "1:825929187153:web:506ae95b5e4fccc8e0dd70"
};
import { getFirestore, doc, onSnapshot, setDoc} from 'firebase/firestore/';
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

export default {
    name: "",
    data() {
        return {
            days: ['月','火','水','木','金'],
            is_push: false,
            dialog: false,
            index_x: 2,
            index_y: 1,
            time_table: {
                '月':["","","","",""],
                '火':["","数学","","",""],
                '水':["","","","",""],
                '木':["","","","",""],
                '金':["","","","",""],
            },
            people:[],
        };
    },
    computed:{
        
    },
    created() {
        onSnapshot(doc(db, "timetable", "data"), (doc) => {
            this.time_table = doc.data()
        })
    },
    methods:{
        push(x, y) {
            this.index_x = x;
            this.index_y = y;
            this.dialog = !this.dialog;
        },
        async add() {
            await setDoc(doc(db, "timetable", "data"), this.time_table)
        },
        is_zero(x){
            return x == 1;
        },
        is_zero_col(x){
            if(x==1){
                return 1;
            } else {
                return 2;
            }
        }
    }
}
</script>

<style>

.timetitle {
    writing-mode: vertical-rl;
    text-combine-upright: all;
}
.numer{
    writing-mode: vertical-rl;
    text-combine-upright: all;
}
</style>