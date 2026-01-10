<script setup>
import { computed, ref } from 'vue'
import belLat from '@skip405/bel-lat'

const source = ref('')
const style = ref('lacinka')

const styles = [
  { value: 'lacinka', label: 'Łacinka (default)' },
  { value: 'geo-2000', label: 'Geographical names (2000)' },
  { value: 'geo-2023', label: 'Geographical names (2023)' },
]

const examples = [
  'Купала',
  'Лацінка',
  'Шчучыншчына',
  'Віцебск',
  'Навагрудак',
]

const output = computed(() => {
  if (!source.value.trim()) {
    return ''
  }

  return belLat(source.value, { style: style.value })
})

const setExample = (value) => {
  source.value = value
}

const clearAll = () => {
  source.value = ''
}
</script>

<template>
  <div class="min-h-screen bg-slate-950 text-slate-100">
    <div class="mx-auto flex w-full max-w-5xl flex-col gap-10 px-6 py-12">
      <header class="flex flex-col gap-4">
        <p class="text-sm font-semibold uppercase tracking-[0.3em] text-sky-300">
          Belarusian Name Transliterator
        </p>
        <div class="flex flex-col gap-3">
          <h1 class="text-4xl font-semibold leading-tight text-white sm:text-5xl">
            Belarusian Cyrillic → Latin script in one step
          </h1>
          <p class="max-w-3xl text-base text-slate-300 sm:text-lg">
            Paste Belarusian names in Cyrillic, choose the desired transliteration standard, and the
            service instantly outputs the Belarusian Latin spelling using @skip405/bel-lat.
          </p>
        </div>
      </header>

      <section class="grid gap-6 lg:grid-cols-[1.1fr_0.9fr]">
        <div class="flex flex-col gap-4 rounded-3xl border border-slate-800 bg-slate-900/60 p-6">
          <div class="flex flex-wrap items-center justify-between gap-4">
            <h2 class="text-lg font-semibold text-white">Source (Cyrillic)</h2>
            <div class="flex items-center gap-2">
              <span class="text-xs uppercase tracking-wide text-slate-400">
                {{ source.length }} chars
              </span>
              <button
                class="rounded-full border border-slate-700 px-3 py-1 text-xs font-semibold text-slate-200 transition hover:border-slate-500 hover:text-white"
                type="button"
                @click="clearAll"
              >
                Clear
              </button>
            </div>
          </div>
          <textarea
            v-model="source"
            class="min-h-[180px] w-full rounded-2xl border border-slate-800 bg-slate-950/80 px-4 py-3 text-base text-slate-100 shadow-inner outline-none ring-sky-500/40 transition focus:ring-2"
            placeholder="Напрыклад: Яўгенія або Шчучыншчына"
          ></textarea>
          <div class="flex flex-wrap gap-2">
            <span class="text-xs font-semibold uppercase tracking-wide text-slate-400">Try</span>
            <button
              v-for="example in examples"
              :key="example"
              class="rounded-full border border-slate-800 px-3 py-1 text-sm text-slate-200 transition hover:border-slate-600 hover:text-white"
              type="button"
              @click="setExample(example)"
            >
              {{ example }}
            </button>
          </div>
        </div>

        <div class="flex flex-col gap-4 rounded-3xl border border-slate-800 bg-slate-900/60 p-6">
          <div class="flex flex-col gap-3">
            <h2 class="text-lg font-semibold text-white">Output (Latin)</h2>
            <label class="text-xs font-semibold uppercase tracking-wide text-slate-400" for="style">
              Transliteration standard
            </label>
            <select
              id="style"
              v-model="style"
              class="w-full rounded-2xl border border-slate-800 bg-slate-950/80 px-4 py-2 text-sm text-slate-100 outline-none ring-sky-500/40 transition focus:ring-2"
            >
              <option v-for="option in styles" :key="option.value" :value="option.value">
                {{ option.label }}
              </option>
            </select>
          </div>
          <div class="flex flex-1 flex-col gap-3">
            <label class="text-xs font-semibold uppercase tracking-wide text-slate-400">Result</label>
            <div
              class="flex min-h-[140px] items-center rounded-2xl border border-dashed border-slate-700 bg-slate-950/60 px-4 py-3 text-lg text-slate-100"
            >
              <span v-if="output" class="break-words">{{ output }}</span>
              <span v-else class="text-slate-500">
                The Latin transliteration will appear here.
              </span>
            </div>
            <p class="text-sm text-slate-400">
              Output updates instantly as you type. Use the geographical options for official place
              name transliterations.
            </p>
          </div>
        </div>
      </section>

      <section class="grid gap-4 rounded-3xl border border-slate-800 bg-slate-900/40 p-6 md:grid-cols-3">
        <div class="flex flex-col gap-2">
          <h3 class="text-sm font-semibold uppercase tracking-wide text-slate-400">Fast</h3>
          <p class="text-sm text-slate-200">
            Client-side transliteration runs instantly in the browser.
          </p>
        </div>
        <div class="flex flex-col gap-2">
          <h3 class="text-sm font-semibold uppercase tracking-wide text-slate-400">Standards</h3>
          <p class="text-sm text-slate-200">
            Switch between Łacinka or the 2000/2023 geographical name rules.
          </p>
        </div>
        <div class="flex flex-col gap-2">
          <h3 class="text-sm font-semibold uppercase tracking-wide text-slate-400">Flexible</h3>
          <p class="text-sm text-slate-200">
            Handles full names, single words, or phrases with punctuation.
          </p>
        </div>
      </section>
    </div>
  </div>
</template>
