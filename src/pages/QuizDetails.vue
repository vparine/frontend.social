<template>
  <div class="host">
    <b-container>
      <b-row class="description">
        <b-col>
          <h1>{{ quiz.title }}</h1>
        </b-col>
      </b-row>
      <b-row>
        <b-col md="12">
          <QuestionStrip
            v-for="(question, index) in quiz.questions"
            :key="index"
            :question="question"
            :quiz-id="quiz._id"
          />
        </b-col>
        <b-col
          md="12"
          class="button"
        >
          <button @click="runQuiz">
            Run
          </button>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import quizService from '@/services/quiz.service';
import eventBus from '@/utilities/eventBus';
import QuestionStrip from '@/components/Quiz/QuizQuestion';
import { ToastType, messages } from '@/constants/constants';
export default {
	name: 'QuizDetails',
	components: {
		QuestionStrip,
	},
	props: {},
	data() {
		return {
			quiz: {},
			quizData: '',
		};
	},
	computed: {
		signedInUser() {
			return this.$store.state.signedInUser;
		},
		items() {
			return [
				{
					text: 'Quiz',
					active: true,
				},
			];
		},
	},
	mounted() {
		this.loadQuiz(this.$route.params.id);
	},
	created() {},
	methods: {
		loadQuiz(quizId) {
			quizService.getQuizById(quizId).then((res) => {
				this.quiz = res;
			});
		},
		runQuiz() {
			quizService.runQuiz(this.$route.params.id).then((res) => {
				this.$router.push(`/quiz/${this.$route.params.id}/run/${res.uniqueId}`);
			});
		},
	},
};
</script>

<style scoped lang="scss">
.button {
	left: 50%;
	margin-bottom: 10px;
}
</style>
