<template>
	<div class="home">
		<RecordDiary />
	</div>
</template>

<script>
// @ is an alias to /src
import RecordDiary from "@/components/RecordDiary.vue";

export default {
	name: "RecordView",
	components: {
		RecordDiary,
	},
};
</script>
