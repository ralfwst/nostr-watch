<template>
  <RelaysNav 
    v-bind:resultsProp="results" />

  <div id="wrapper" class="mx-auto max-w-7xl">  
    <form class="space-y-8 divide-y divide-gray-200">
      <div class="space-y-8 divide-y divide-gray-200 sm:space-y-5">
        <div class="space-y-6 sm:space-y-5">
          <div class="mt-8 content-left">
            <h1 class="align-left text-4xl capitalize font-semibold text-gray-900 dark:text-white/90">
              User Preferences
            </h1>
          </div>
            
            <div class="sm:grid sm:grid-cols-3 sm:items-start sm:gap-4 sm:border-t sm:border-gray-200 dark:sm:border-slate-800 sm:pt-5">
              <label for="about" class="block text-sm font-medium text-gray-700 dark:text-gray-300 sm:mt-px sm:pt-2">Client-side processing</label>
              <div class="mt-1 sm:col-span-2 sm:mt-0">
                <Switch
                  v-model="store.prefs.clientSideProcessing"
                  :class="store.prefs.clientSideProcessing ? 'bg-indigo-600' : 'bg-gray-200 dark:bg-black'"
                  class="relative inline-flex h-5 w-10 shrink-0 cursor-pointer rounded-full border-2 border-transparent transition-colors duration-200 ease-in-out focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
                >
                  <span class="sr-only">Use client-side processing</span>
                  <span
                    aria-hidden="true"
                    :class="store.prefs.clientSideProcessing ? 'translate-x-5' : 'translate-x-0'"
                    class="pointer-events-none inline-block h-4 w-4 transform rounded-full bg-white shadow-lg ring-0 transition duration-200 ease-in-out"
                  />
                </Switch>
                <p class="mt-2 text-sm text-gray-500">If enabled, checks will run client-side instead of pulling data from nostr.watch history node. The history node is faster, but less accurate, client-side processing is slower, but more accurate.</p>
              </div>
            </div>

            <div 
              class="sm:grid sm:grid-cols-3 sm:items-start sm:gap-4 sm:border-t sm:border-gray-200 dark:sm:border-slate-800 sm:pt-5"
              v-if="store.prefs.clientSideProcessing">
              <label for="about" class="block text-sm font-medium text-gray-700 dark:text-gray-300 sm:mt-px sm:pt-2">
                Auto refresh
              </label>
              <div class="mt-1 sm:col-span-2 sm:mt-0">
                <Switch
                  v-model="store.prefs.refresh"
                  :class="store.prefs.refresh ? 'bg-indigo-600' : 'bg-gray-200 dark:bg-black'"
                  class="relative inline-flex h-5 w-10 shrink-0 cursor-pointer rounded-full border-2 border-transparent transition-colors duration-200 ease-in-out focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
                >
                  <span class="sr-only">Automatically refresh?</span>
                  <span
                    aria-hidden="true"
                    :class="store.prefs.refresh ? 'translate-x-5' : 'translate-x-0'"
                    class="pointer-events-none inline-block h-4 w-4 transform rounded-full bg-white shadow-lg ring-0 transition duration-200 ease-in-out"
                  />
                </Switch>
                <p class="mt-2 text-sm text-gray-500">Automatically refresh</p>
              </div>
            </div>

            <div 
              class="sm:grid sm:grid-cols-3 sm:items-start sm:gap-4 sm:border-t sm:border-gray-200 dark:sm:border-slate-800 sm:pt-5"
              v-if="store.prefs.refresh && store.prefs.clientSideProcessing">
              <label for="about" class="block text-sm font-medium text-gray-700 dark:text-gray-300 sm:mt-px sm:pt-2">
                Refresh Interval
              </label>
              <div class="mt-1 sm:col-span-2 sm:mt-0">
                <div class="mt-4 space-y-4">
                  <div class="flex items-center">
                    <input v-model="store.prefs.duration" :value="30*60*1000" id="push-everything" name="push-notifications" type="radio" class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                    <label for="push-everything" class="ml-3 block text-sm font-medium text-gray-700  dark:text-gray-500">30 minutes</label>
                  </div>
                  <div class="flex items-center">
                    <input v-model="store.prefs.duration" :value="1*60*60*1000" id="push-email" name="push-notifications" type="radio" class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                    <label for="push-email" class="ml-3 block text-sm font-medium text-gray-700  dark:text-gray-500">1 hour</label>
                  </div>
                  <div class="flex items-center">
                    <input v-model="store.prefs.duration" :value="2*60*60*1000" id="push-nothing" name="push-notifications" type="radio" class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                    <label for="push-nothing" class="ml-3 block text-sm font-medium text-gray-700 dark:text-gray-500">2 hours</label>
                  </div>
                  <div class="flex items-center">
                    <input v-model="store.prefs.duration" :value="6*60*60*1000" id="push-nothing" name="push-notifications" type="radio" class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                    <label for="push-nothing" class="ml-3 block text-sm font-medium text-gray-700 dark:text-gray-500">6 hours</label>
                  </div>
                  <div class="flex items-center">
                    <input v-model="store.prefs.duration" :value="12*60*60*1000" id="push-nothing" name="push-notifications" type="radio" class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                    <label for="push-nothing" class="ml-3 block text-sm font-medium text-gray-700 dark:text-gray-500">12 hours</label>
                  </div>
                  <div class="flex items-center">
                    <input v-model="store.prefs.duration" :value="24*60*60*1000" id="push-nothing" name="push-notifications" type="radio" class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                    <label for="push-nothing" class="ml-3 block text-sm font-medium text-gray-700 dark:text-gray-500">24 hours</label>
                  </div>
                </div>
                <p class="mt-2 text-sm text-gray-500">How often should nostr.watch recheck relays?</p>
              </div>
            </div>

            <div class="sm:grid sm:grid-cols-3 sm:items-start sm:gap-4 sm:border-t sm:border-gray-200 dark:sm:border-slate-800 sm:pt-5">
              <label for="about" class="block text-sm font-medium text-gray-700 dark:text-gray-300 sm:mt-px sm:pt-2">
                Pin Favorites
              </label>
              <div class="mt-1 sm:col-span-2 sm:mt-0 align-left">
                <Switch
                  v-model="store.prefs.pinFavorites"
                  :class="store.prefs.pinFavorites ? 'bg-indigo-600' : 'bg-gray-200 dark:bg-black'"
                  class="relative inline-flex h-5 w-10 shrink-0 cursor-pointer rounded-full border-2 border-transparent transition-colors duration-200 ease-in-out focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
                >
                  <span class="sr-only">Pin Favorites</span>
                  <span
                    aria-hidden="true"
                    :class="store.prefs.pinFavorites ? 'translate-x-5' : 'translate-x-0'"
                    class="pointer-events-none inline-block h-4 w-4 transform rounded-full bg-white shadow-lg ring-0 transition duration-200 ease-in-out"
                  />
                </Switch>
                <p class="mt-2 text-sm text-gray-500">Pin favorites to top</p>
              </div>
            </div>

            <div class="sm:grid sm:grid-cols-3 sm:items-start sm:gap-4 sm:border-t sm:border-gray-200 dark:sm:border-slate-800 sm:pt-5">
              <label for="about" class="block text-sm font-medium text-gray-700 dark:text-gray-300 sm:mt-px sm:pt-2">
                Fast Latency  (default: 20)
              </label>
              <div class="mt-1 sm:col-span-2 sm:mt-0 align-left">
                {{ store.prefs.latencyFast }}
                <input 
                  v-model="store.prefs.latencyFast"
                  class="rounded-lg overflow-hidden appearance-none bg-gray-400 h-3 w-128" 
                  type="range" min="1" max="500" step="1" />
                <p class="mt-2 text-sm text-gray-500">What should nostr.watch consider as a fast latency?</p>
              </div>
            </div>

            <div class="sm:grid sm:grid-cols-3 sm:items-start sm:gap-4 sm:border-t sm:border-gray-200 dark:sm:border-slate-800 sm:pt-5">
              <label for="about" class="block text-sm font-medium text-gray-700 dark:text-gray-300 sm:mt-px sm:pt-2">
                Slow Latency (default 1000)
              </label>
              <div class="mt-1 sm:col-span-2 sm:mt-0">
                {{ store.prefs.latencySlow }}
                <input 
                  v-model="store.prefs.latencySlow"
                  class="rounded-lg overflow-hidden appearance-none bg-gray-400 h-3 w-128" 
                  type="range" min="500" max="5000" step="1"/>
                <p class="mt-2 text-sm text-gray-500">What should nostr.watch consider as a slow latency?</p>
              </div>
            </div>
          </div>
        </div>
    </form>
  </div>
</template>

<script>
//vue
import { defineComponent } from 'vue'
//pinia
import { setupStore } from '@/store'
//shared methods
import RelaysLib from '@/shared/relays-lib.js'
import SharedComputed from '@/shared/computed.js'

import { Switch } from '@headlessui/vue'

const localMethods = {}

export default defineComponent({
  name: 'HomePage',

  components: {
    Switch
  },

  setup(){
    // const {resultsProp: results} = toRefs(props)
    return { 
      store : setupStore(),
      // results: results
    }
  },

  props: {
  },

  data() {
    return {
    }
  },

  updated(){},

  watch: {
    results: function(){
      //console.log('results changed.')
    }
  },

  beforeMount(){
  },

  async mounted() {

  },

  computed: Object.assign(SharedComputed, {
    
  }),

  methods: Object.assign(RelaysLib, localMethods), 

})
</script>

<style scoped>
@media screen and (-webkit-min-device-pixel-ratio: 0) {
     
     input[type="range"]::-webkit-slider-thumb {
         width: 15px;
         -webkit-appearance: none;
     appearance: none;
         height: 15px;
         cursor: ew-resize;
         background: #FFF;
         box-shadow: -405px 0 0 400px #605E5C;
         border-radius: 50%;
         
     }
 }
 </style>