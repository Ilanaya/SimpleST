<template>
    <button
        class="btn button-main"
        type="button"
        @click="calcHL(matrix, probabilities)"
    >
        calcHL
    </button>
    <div>{{ result }}</div>
</template>
<script lang="ts">
import { defineComponent, PropType } from "vue";

import {
    getLambda,
    findMinInRows,
    sumMathProbabilities,
    prettifyOutput,
} from "../criteriasUtils";
export default defineComponent({
    data() {
        return {
            lambda: getLambda(),
            result: "",
        };
    },
    props: {
        matrix: {
            type: Object as PropType<number[][]>,
            required: true,
        },
        probabilities: {
            type: Object as PropType<number[]>,
            required: true,
        },
    },

    methods: {
        calcHL(matrix: number[][], probabilities: number[], lambda = 0.5) {
            const lambdaMathProbabilities = sumMathProbabilities(
                matrix,
                probabilities
            ).map((value) => lambda * value);
            const lambdaMinInRows = findMinInRows(matrix).map(
                (value) => (1 - Number(lambda.toFixed(1))) * value
            );
            const options = lambdaMathProbabilities.map(
                (value, i) => value + lambdaMinInRows[i]
            );
            const decision = Math.max(...options);
            this.result = prettifyOutput(options, decision);
        },
    },
});
</script>
<style scoped src="./criteriasStyle.css"></style>
