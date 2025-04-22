<script lang="ts" setup>
import Prism from "prismjs";
import "prismjs/themes/prism-tomorrow.min.css";
import "prismjs/components/prism-markup";
import "prismjs/components/prism-javascript";

const { code, name = "Component" } = defineProps<{
  code: string;
  name?: string;
}>();

const copySuccess = ref<boolean>(false);
const codeElement = ref<HTMLElement | null>(null);

const highlightCode = () => {
  if (codeElement.value) {
    Prism.highlightElement(codeElement.value);
  }
};

const handleCopyCode = async () => {
  if (code) {
    await navigator.clipboard.writeText(code);
    copySuccess.value = true;
  }
};

onMounted(() => {
  highlightCode();
});

watch(
  () => code,
  () => {
    nextTick(() => {
      highlightCode();
    });
  },
  { immediate: true }
);
</script>

<template>
  <div class="flex mx-auto">
    <div class="rounded-xl flex flex-col w-full">
      <div class="w-full justify-between items-center flex">
        <p class="text-gray-400 text-sm">{{ name + ".vue" }}</p>
        <UPopover>
          <UButton
            :icon="
              copySuccess
                ? 'i-ph-check-square-offset-duotone'
                : 'i-ph-clipboard'
            "
            variant="ghost"
            @click="handleCopyCode"
          />
          <template #content>
            <div class="flex flex-row justify-between p-2">
              <p class="text-(--ui-primary) text-xs">
                Code Copied to Clipboard!
              </p>
            </div>
          </template>
        </UPopover>
      </div>
      <div class="relative">
        <pre
          class="bg-slate-600/20 rounded-xl p-2 max-h-[500px] overflow-auto"
        ><code ref="codeElement" class="language-markup whitespace-pre">{{ code }}</code></pre>
      </div>
    </div>
  </div>
</template>
