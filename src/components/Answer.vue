<template>
  <div
    class="answer text-center bg-teal-900 text-yellow-300 text-2xl py-20 px-8 rounded"
  >
    <p>
      <span class="text-yellow-400">
        <a class="text-yellow-400" target="wtt-tag">{{ answers[0] }}</a>
        <a
          class="bg-blue-500 text-white p-2 rounded text-xl font-bold block mt-5"
          v-if="answers[1]"
          :href="answers[1]"
        >
          다음 단계
        </a>
      </span>
    </p>
  </div>
</template>

<script>
export default {
  name: "Answer",
  props: {
    answers: Array,
  },

  methods: {
    getLinkForTag(tag) {
      let url = `https://developer.mozilla.org/en-US/docs/Web/HTML/Element`;

      const tagName = tag.match(/[A-za-z0-9]*/);
      url += `/${tagName}`;

      const hasTypeAttribute = tag.includes("type");

      if (hasTypeAttribute) {
        const typeAttributeValue = tag.match(/type="([A-Za-z0-9 _]*)"/)[1];

        url += `/${typeAttributeValue}`;
      }

      return url;
    },
  },
};
</script>

<style>
.answer {
  width: 50vw;
  min-width: 300px;
}
</style>
