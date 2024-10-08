<!--
SPDX-FileCopyrightText: 2023 Marlon W (Mawoka)

SPDX-License-Identifier: MPL-2.0
-->

<script lang="ts">
	import type { Question } from '$lib/quiz_types';
	import { QuizQuestionType } from '$lib/quiz_types';

	export let data;
	export let question: Question;

	let quiz_answers = [];
	let quiz_colors = [];
	let answer_correct: boolean[] = [];

	for (const i of question.answers) {
		quiz_answers.push(i.answer);
		quiz_colors.push(i.color);
		answer_correct.push(i.right);
	}

	let sorted_data = {};
	for (const i of quiz_answers) {
		sorted_data[i] = 0;
	}

	// Function to clean and normalize text answers
	const cleanTextAnswer = (answer: string) => {
		return answer.trim().replace(/\s+/g, ' ');
	};

	// Count the answers
	for (const i of data) {
		let answer_found = false;
		for (const j of question.answers) {
			let submitted_answer = cleanTextAnswer(i.answer);
			let correct_answer = cleanTextAnswer(j.answer);
			if (!j.case_sensitive) {
				submitted_answer = submitted_answer.toLowerCase();
				correct_answer = correct_answer.toLowerCase();
			}
			if (submitted_answer === correct_answer) {
				sorted_data[j.answer] += 1;
				answer_found = true;
				break;
			}
		}
		if (!answer_found) {
			sorted_data[i.answer] += 1;
		}
	}

	const isCorrect = (answer) => {
      return question.answers.some(a => cleanTextAnswer(a.answer) === cleanTextAnswer(answer) && a.right);
    };
  
    const isIncorrect = (answer) => {
      return question.answers.some(a => cleanTextAnswer(a.answer) === cleanTextAnswer(answer) && !a.right);
    };
</script>

<div class="flex justify-center w-full">
	<div
		class="m-auto w-fit gap-4 flex flex-col bg-[#E9F3FF] bg-opacity-50 py-10 px-16 rounded-xl"
		style="grid-template-columns: repeat({quiz_answers.length}, minmax(0, 1fr));"
	>
		<div class="flex gap-12">
			{#each quiz_answers as answer}
				<span class="text-center self-end mx-auto text-lg text-[#00529B] dark:text-white"
					>{#if sorted_data[answer] > 0}{sorted_data[answer]}{/if}</span
				>
			{/each}
		</div>
		<div class="flex gap-12">
			{#each quiz_answers as answer, i}
				<div
					class="w-20 self-end flex justify-center border text-[#00529B] dark:text-white border-white  rounded"
					
					class:opacity-70={!answer_correct[i] &&
						question.type !== QuizQuestionType.VOTING}
					style="height: {(sorted_data[answer] * 20) /
						data.length}rem; background-color: {quiz_colors[i]
						? quiz_colors[i]
						: '#001293'}"
				/>
			{/each}
		</div>
		<div class="flex gap-12">
			{#each quiz_answers as answer, i}
				<div class="w-20">
					<p
						class=" text-base  text-str text-[#00529B] font-bold dark:text-white"
						class:line-through={!answer_correct[i] &&
							question.type !== QuizQuestionType.VOTING && question.type !== QuizQuestionType.TEXT}
					>
						{@html answer}
					</p>
				</div>
			{/each}
		</div>
	</div>
</div>

<div class="flex justify-center w-full px-4">
	<div class="m-auto gap-4 flex flex-col items-center">
		<!-- <h2 class="text-2xl font-bold text-center text-[#00529B] mb-4 dark:text-[#fff] mt-10">Answers</h2> -->
		<!-- <p class="text-lg md:text-xl lg:text-2xl text-center mb-6 dark:text-[#fff] text-[#00529B]">{question.question}</p> -->
		<!-- <div class="flex justify-center gap-4 w-full max-w-5xl">
			{#each quiz_answers as answer, i}
				<div class="w-full flex justify-center p-4">
					<div class="flex items-center justify-between p-4 border  rounded gap-3 border-[#0AEDFE] bg-opacity-80 shadow-lg bg-[#92CAF4] dark:bg-gray-800">
						<span class="text-lg text-white dark:text-white">{answer}</span>
						{#if question.type === QuizQuestionType.VOTING || question.type === QuizQuestionType.TEXT}
							<span class="text-xl font-bold text-green-500">
								{sorted_data[answer]}
							</span>
						{:else}
							<span class="text-xl font-bold" class:text-green-500={isCorrect(answer)} class:text-red-500={isIncorrect(answer)}>
								{isCorrect(answer) ? '✓' : isIncorrect(answer) ? '✗' : ''}
							</span>
						{/if}
					</div>
				</div>
			{/each}
		</div> -->
	</div>
</div>