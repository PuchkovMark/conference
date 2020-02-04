<template>
    <v-card>
        <v-tabs
                v-model="tabs"
                fixed-tabs
        >
            <v-tabs-slider></v-tabs-slider>
            <v-tab
                    href="#mobile-tabs-5-1"
                    class="primary--text"
            >
                Item 1
            </v-tab>

            <v-tab
                    href="#mobile-tabs-5-2"
                    class="primary--text"
            >
                Item 2
            </v-tab>
        </v-tabs>

        <v-tabs-items v-model="tabs" style="height: auto;">
            <v-tab-item
                    :value="'mobile-tabs-5-' + 1"
            >
                <v-card flat>
                    <v-card-text class="labelText">Определим следующие три состояния: S<sub>0</sub> — цель не повреждена; S<sub>1</sub> — цель
                        повреждена; S<sub>2</sub> — цель разрушена. Зададим вектор начальных вероятностей:
                    </v-card-text>
                    <v-card-actions>
                        <v-card-text class="labelText">Цель не повреждена</v-card-text>
                        <v-text-field
                                class="labelInput"
                                readonly
                                type="number"
                                v-model.trim="s0"
                        ></v-text-field>
                        <v-card-text class="labelText">Цель повреждена</v-card-text>
                        <v-text-field
                                class="labelInput"
                                readonly
                                type="number"
                                v-model.trim="s1"
                        ></v-text-field>
                        <v-card-text class="labelText">Цель разрушена</v-card-text>
                        <v-text-field
                                class="labelInput"
                                readonly
                                type="number"
                                v-model.trim="s2"
                        ></v-text-field>
                    </v-card-actions>

                    <v-card-text>Зададим матрицу перехода состояний</v-card-text>
                    <v-simple-table>
                        <template v-slot:default>
                            <thead>
                            <tr>
                                <th class="text-left">Откуда/Куда</th>
                                <th class="text-left">В S<sub>0</sub></th>
                                <th class="text-left">В S<sub>1</sub></th>
                                <th class="text-left">В S<sub>2</sub></th>
                                <th class="text-left">Сумма вероятностей</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr>
                                <td>Из S<sub>0</sub></td>
                                <td>0.45</td>
                                <td>0.40</td>
                                <td>0.15</td>
                                <td>0.45 + 0.40 + 0.15 = 1</td>
                            </tr>
                            <tr>
                                <td>Из S<sub>1</sub></td>
                                <td>0</td>
                                <td>0.45</td>
                                <td>0.55</td>
                                <td>0 + 0.45 + 0.55 = 1</td>
                            </tr>
                            <tr>
                                <td>Из S<sub>2</sub></td>
                                <td>0</td>
                                <td>0</td>
                                <td>1</td>
                                <td>0 + 0 + 1 = 1</td>
                            </tr>
                            </tbody>
                        </template>
                    </v-simple-table>
                    <v-card-text>Матрица задает вероятность перехода из каждого состояния в каждое. Заметим, что
                        вероятности заданы так, что сумма вероятностей перехода из некоторого состояния в остальные
                        всегда равна единице (куда-то система должна перейти обязательно).
                    </v-card-text>
                    <v-card-text>Используя модель и метод статистического моделирования, попытаемся решить следующую
                        задачу: <b>определить среднее количество снарядов, необходимое для полного разрушения цели.</b>
                    </v-card-text>
                    <v-card-text>Проимитируем, используя функцию рандома (от 0 до 1), процесс стрельбы. Пусть начальное
                        состояние будет S<sub>0</sub>.
                    </v-card-text>
                    <div v-for="(step, index) in steps" :key="index" v-if="steps !== []">
                        <v-card-text v-html="step"></v-card-text>
                    </div>
                    <v-card-actions>
                        <v-btn @click="globalSimulation">Запустить симуляцию</v-btn>
                    </v-card-actions>
                    <v-card-actions>
                        <v-card-text>Среднее колличество снарядов для разрушения цели:</v-card-text>
                        <v-card-text class="rezult" readonly v-text="result"></v-card-text>
                    </v-card-actions>
                </v-card>
            </v-tab-item>
            <v-tab-item
                    :value="'mobile-tabs-5-' + 2"
            >
                <v-card flat>
                    <v-text-field style="text-align: center">Страница находится в разработке</v-text-field>
                </v-card>
            </v-tab-item>
        </v-tabs-items>
    </v-card>
</template>

<script>

    export default {
        name: 'home',
        data() {
            return {
                tabs: null,
                s0: 0.80,
                s1: 0.20,
                s2: 0,
                result: '',
                steps: []
            }
        },
        created() {},
        methods: {
            globalSimulation() {
                let steps = this.steps

                // случайное число от 0 до 1
                function randomInteger() {
                    return Math.random().toFixed(2);
                }
                let b = 8 // то что ввел пользователь (колличество пораженных целей)
                let s0 = this.s0

                let k = 0
                let c = 0

                while (k <= b) {
                    if (k === b) {
                        steps.push(`итог: ${c} / ${k} <br /> <hr style="border: 5px solid silver;" />`)
                        let mid = c / k
                        return this.result = Math.round(mid)
                    }
                    let n = 0

                    let rand = randomInteger()
                    //rand < this.s0 ? s = 0 : s = 1
                    if (rand < s0) {
                        var s = 0
                        steps.push(`${rand}: цель не повреждена`)
                    } else {
                        var s = 1
                        steps.push(`${rand}: цель повреждена!`)
                    }

                    while (s <= 2) {
                        let rand = randomInteger()
                        //console.log(`Вероятности попадания: ${rand}`)
                        if (s === 0) {
                            // TODO: 0.45 must be dinamic
                            if (rand <= 0.45) {
                                s = 0
                                steps.push(`${rand}: цель находится в состоянии S<sub>0</sub> и остается в состоянии S<sub>0</sub>, так как 0 < ${rand} < 0.45`)

                            } else {
                                // TODO: dinamic number
                                if ((rand > 0.45) && (rand <= 0.85)) {
                                    s = 1
                                    steps.push(`${rand}: цель находится в состоянии S<sub>0</sub> и переходит в состояние s<sub>1</sub>, так как 0.45 < ${rand} < 0.85`)
                                } else {
                                    s = 2
                                    steps.push(`${rand}: цель находилась в S<sub>0</sub> и переходит в состоянии S<sub>2</sub>, так как 0.45 <  0.85 < ${rand}`)
                                }
                            }
                        } else {
                            if (s === 1) {
                                if (rand <= 0.45) {
                                    s = 1
                                    steps.push(`${rand}: цель находится в состоянии S<sub>1</sub>  остается в состоянии S<sub>1</sub>, так как 0 < ${rand} < 0.45`)
                                } else {
                                    s = 2
                                    steps.push(`${rand}: цель находится в состоянии S<sub>1</sub> и переходит в состояние S<sub>2</sub>, так как 0.45 < ${rand} < 0.45 + 0.55`)
                                }
                            } else {
                                if (k < b) {
                                    c += n
                                    k++
                                    steps.push(`выстрелов сделано: ${n}`)
                                    steps.push(`сумма всех выстрелов: ${c} <br /> <hr />`)
                                    break
                                } else {
                                    console.log('что то пошло не так')
                                }
                            }
                        }
                        n++
                    }
                }
            }
        }
    }
</script>

<style scoped>
    .rezult {
        margin-right: 0;
    }
    /*.labelInput {
        min-width: 20px;
    }*/

    @media screen and (max-width: 865px) {
        .rezult {
            margin-right: 0;
        }
    }
</style>
