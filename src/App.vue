<script setup lang="ts">
import QrcodeVue from 'qrcode.vue'
import { ref } from 'vue'

const value = ref('')
const color = ref('#000000')
const background = ref('transparent')
const qrcode = ref<HTMLElement | null>(null)

const download = () => {
  const svgFile = new Blob([qrcode.value!.outerHTML], { type: 'image/svg+xml' })
  const url = URL.createObjectURL(svgFile)
  const link = document.createElement('a')
  link.href = url
  link.download = 'qrcode.svg'
  link.click()
  URL.revokeObjectURL(url)
}
</script>

<template>
  <div class="container mx-auto max-w-5xl py-20 flex flex-col items-center px-4">
    <h1 class="text-3xl font-bold mb-4">Gerador de QrCode</h1>
    <div class="max-w-96 mb-4 w-full">
      <label for="name" class="block text-sm font-medium leading-6 text-gray-900">Conteúdo</label>
      <input
        name="name"
        type="text"
        v-model="value"
        class="w-full border block min-w-0 grow py-1.5 pr-3 pl-1 text-base text-gray-900 placeholder:text-gray-400 focus:outline-none sm:text-sm/6"
        placeholder="Digite o conteúdo"
      />
    </div>

    <div class="max-w-96 mb-4 w-full">
      <label for="color" class="block text-sm font-medium leading-6 text-gray-900">Cor</label>
      <input
        name="color"
        type="color"
        v-model="color"
        class="w-full border block min-w-0 grow py-1.5 pr-3 pl-1 text-base text-gray-900 placeholder:text-gray-400 focus:outline-none sm:text-sm/6"
        placeholder="Selecione a cor"
      />
    </div>

    <div class="max-w-96 mb-4 w-full">
      <label for="background" class="block text-sm font-medium leading-6 text-gray-900"
        >Cor de fundo</label
      >
      <input
        name="background"
        type="color"
        v-model="background"
        class="w-full border block min-w-0 grow py-1.5 pr-3 pl-1 text-base text-gray-900 placeholder:text-gray-400 focus:outline-none sm:text-sm/6"
        placeholder="Selecione a cor"
      />
      <button
        class="text-sm mt-1 bg-orange-600 text-white px-1 py-0.5"
        @click="background = 'transparent'"
      >
        Fundo transparente
      </button>
    </div>

    <div ref="qrcode" class="mt-10 max-w-[300px] md:max-w-[500px] mx-auto overflow-hidden">
      <qrcode-vue
        :foreground="color"
        :background="background"
        id="qr-code"
        :value="value"
        :size="800"
        level="H"
        render-as="svg"
      />
    </div>

    <div class="text-center">
      <button class="mt-10 bg-orange-600 text-white px-4 py-2" @click="download">Baixar</button>
    </div>
  </div>
</template>

<style>
svg {
  max-width: 300px !important;
  height: auto;
}

@media screen and (min-width: 768px) {
  svg {
    max-width: 500px !important;
  }
}
</style>
