<template>
  <transition>
    <div
      v-if="modelValue"
      class="fixed fixed-stretch backdrop-filter backdrop-blur-sm bg-lightgrey/50 z-100 overflow-y-auto lg:py-24 lg:px-8"
      @click="$emit('update:modelValue', false)"
    >
      <div
        class="max-w-full <lg:min-h-screen bg-white p-8 sm:rounded-lg w-full lg:w-5xl z-120 mx-auto"
        @click.stop
      >
        <div class="flex justify-between">
          <a
            class="inline-flex items-center opacity-40 mr-auto cursor-pointer"
            @click="$emit('update:modelValue', false)"
          >
            <i class="text-2xl mdi mdi-arrow-left-thin mr-2" />
            Keep Browsing
          </a>
          <a
            class="cursor-pointer"
            @click="$emit('update:modelValue', false)"
          >
            <i class="text-xl mdi mdi-close" />
          </a>
        </div>

        <div class="flex flex-col lg:flex-row mt-9">
          <div class="w-full lg:w-120 flex-shrink-0">
            <img
              class="w-full mb-3"
              :src="token.image"
              width="600"
              height="600"
              :alt="token.name"
            >
            <div class="flex justify-between text-xs">
              <div class="uppercase">
                <span class="mb-1">
                  {{ wallet.getWalletTokenInfo(token.name).tokenName }}
                </span><br>
                {{ wallet.getWalletTokenInfo(token.name).tokenNumber }}
              </div>
              <div
                v-if="token.redeemed"
                class="text-black/30"
              >
                REDEEMED
              </div>
              <div
                v-else-if="alreadyInCart(token.tokenID)"
                class="text-black/30"
              >
                PENDING REDEMPTION
              </div>
              <router-link
                v-else
                :to="{
                  name: 'Redeem',
                  params: { id: getCapsuleTrait(token) }
                }"
              >
                REDEEM NOW ↗
              </router-link>
            </div>
          </div>
          <div class="flex flex-col w-full lg:ml-8 <lg:mt-8">
            <h2 class="text-xl sm:text-3xl mb-4">
              {{ token.name }}
            </h2>
            <div class="grid grid-cols-2 sm:mx-4 mb-8">
              <div
                v-for="(item, index) in token.attributes"
                :key="`attr_${index}`"
              >
                <h5 class="sm:text-lg font-medium uppercase">
                  {{ item.trait_type }}
                </h5>
                <span class="text-xs sm:text-sm font-medium capitalize">
                  {{ item.value }}
                </span>
              </div>
            </div>
            <router-link
              v-if="token.redeemed != true && !alreadyInCart(token.tokenID)"
              class="text-2xl font-medium ml-auto mt-auto"
              :to="{
                name: 'Redeem',
                params: { id: getCapsuleTrait(token) }
              }"
            >
              REDEEM NOW ↗
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script setup lang="ts">
import wallet, { Token } from '@/use/wallet'
import { alreadyInCart, getCapsuleTrait } from '@/utils/capsule'

defineEmits<{
  (event: 'redeem', value: Token[]): void
  (event: 'update:modelValue', value: boolean): void
}>()
defineProps<{
  token: Token
  modelValue: boolean
}>()
</script>
