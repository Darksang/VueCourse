<script setup>
    import { ref, watch, computed } from "vue"
    
    import { useRoute } from "vue-router"

    import Question from "../components/Question.vue"
    import QuizHeader from "../components/QuizHeader.vue"
    import Result from "../components/Result.vue"

    import quizes from "../data/quizes.json"

    const route = useRoute()

    const quizId = parseInt(route.params.id)
    const quiz = quizes.find(q => q.id === quizId)

    const currentQuestionIndex = ref(0)

    const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
    const barPercentage = computed(() => `${currentQuestionIndex.value / quiz.questions.length * 100}%`)

    const numberOfCorrectAnswers = ref(0)

    const showResults = ref(false)

    const onOptionSelected = (isCorrect) => {
        if (isCorrect) {
            numberOfCorrectAnswers.value++
        }

        if (currentQuestionIndex.value === quiz.questions.length - 1) {
            showResults.value = true
        }

        currentQuestionIndex.value++
    }
</script>

<template>
    <div>
        <QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage"/>
        <div>
            <Question v-if="!showResults"
                :question="quiz.questions[currentQuestionIndex]" @selectOption="onOptionSelected" />
            <Result v-else :quizQuestionLength="quiz.questions.length" :correctAnswers="numberOfCorrectAnswers"/>
        </div>
    </div>
</template>