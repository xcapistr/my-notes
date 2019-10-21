[< back](../README.md)
# VUE
![](../img/vue.png)

- **direktivy**: `v-...`

- **binding**: `{{ }}` `v-bind:src=""`

- **2-way binding**: `v-model`

- **for**: `v-for="nieco in volaco" :key="nieco.id"`,
`v-for="(nieco, index) in volaco"`

- **if**: `v-if=""` - rozhoduje ci sa element prida do DOMu

    `v-else`

- **show**: `v-show` - skryje sa ako display: none

- **event handling**: `v-on:click=""`

    alebo
    `@click=""`
    `@mouseover=""`
    `@submit=""`
    `@keyup.enter=""`

- **script.js:**
    ```jsx    
    var app = new Vue({
        el: "#app",
        data: {},
        computed: {}
        methods: {}
    });
    ```

- **style binding**
    `<... :style="{backgroundColor: volaco}">`
- **class binding**
    `< :class="nazovTriedy">`
    `< :class="[nazovTriedy, dalsia]">`
    `< :class="[podmienka ? nazovTriedy : '']">`
    `< :class="{nazovTriedy: booleanHodnota}">`

    use case -> `:class="{ faded: dude.cool < 10, gold: dude.cool > 50 }"`

    !poznamka: ked dam pred prop dvojbodku - `:prop=""` - mozem pisat do " " JavaScript 

- **computed property** 
    - lepsie ako metody, vzdy ked dojde k zmene zavislosti, prepocitaju sa
    ```js
    computed:{
         title() {
              return this.volaco + ' ' + this.cosi
         }
    }
    ```
    - v class komponente implementujem getter metody `get someProp() { return value }`

- Povolit **vyvojove nastroje v chrome**

    `Vue.config.devtools = true;`

- **Komponent**
    ```jsx
    Vue.component('komponent', {
        props:{
              message: {
                 type: String, required: true, default: "Hi"
             },
            ...
         },
        template: `<div>...</div>`,
        data() {return {}}
    })
    ```

    `<komponent my-prop="" :my-prop2="">`

    - nazov mozem zapisat PascalCaseom alebo kebab-caseom
    - props mozme uvadzat s : na zaciatku alebo aj bez ???
    - v komponente ich zapisujem ako obycajne pole - props: [...] - alebo ako objekt s atributmi ktore su tiez objekty definujuce vlastnosti - props: { atribut: {}}
    - data je funkcia ktora vracia data object

    - komponenty v suboroch

        - nazvy: PascalCase.vue
    - lepsi zapis (html, js, css v 1 subore)
        ```jsx
        <template></template>
        <script lang="ts">
            export default {};     
        </script>
        <style lang="sass|css|scss"></style>
        ```
- **Slot**
    - aby som mohol zvonka pridavat child elementy do komponentu
    - v sablone komponentu ... `<slot></slot>`
    - potom
        ```html    
        <komponent>
             <child-komponent></child-component>
        </komonent>
        ```

    - ak vlozim nejake elementy do slotu vnutri komponentu budu sa zobrazovat ako defaultny case v pripade ze zvonka nic nevlozim


- **Events**

        - emitovanie: `$emit`
            ```js
            nejakyEvent() {
              this.$emit('nejaky-event')     
            }
            ``` 

        - mozem poslat aj parameter
            `this.$emit('nejaky-event', parameter)`
        - stane sa parametrom kazdej metody ktora sa zavola s eventom

    - **event handler**
        `<... @nejaky-event="daco" ...>`
        - takto mozme v cez komponent zavolat napr. metodu jeho rodica, staci priradit eventu metodu

        - s parametrom:
        `<... @nejaky-event="nejakaFunkciaAleboPrikaz($event)" ...>`

    - **Predavanie dat**
        - rodic -> potomok: pass props
        
        - potomok -> rodic: $emit events

    - **Modifikatory**
        - `.prevent` - modifikator ktory zabrani defaultnemu chovaniu eventu, napr. refresh stranky pri evente submit
        `<... @submit.prevent="metoda">`
        - `.once` - event moze nastat len raz


- **CLI**

    - full system, rozne features out of the box
    - konfiguracia webpacku, Hot module replacement (HMR)

    `npm i -g @vue/cli`

    - **vytvorenie projektu**
        `vue create nazov-projektu`
        - enter
        - medzernikom vyberam - babel, router, vuex, linter

        - projekt sa da vytvorit aj cez vue ui (prikaz vue ui)

        - babel - vzdy, >=ES6
        - ts - ak potrebujem typy, classes, interfaces

        - pwa - progresive web app - stranky s offline features (instaluju sa, push notifikacie, cashovanie)

        - router - zoznam routes
        - vuex - ako redux pre vue - store pre data (single source of thruth)
        - css preprocesory - sass/less/stylus
        - unit testy - mocha, chai, jest
        - e2e - cypress, nightwatch
        - eslint - upozornuje ma na chyby (doporucene zapisy, proavidla), podla toho ako je nakonfigurovany 
        - prettier - code formater


- **extensions**: 
    - **vetur** - zvyraznena syntax
    - snippets: 
        - **scaffold** - html sablona pre vue subor
        - div>ul>li  - doplni html
        - vdata, vprops, von, vif, vfor

- **Ref** 
    - nastavim ako property elementu, napr `<... ref="odkazNaEl">`, `this.$refs.odkazNaEl`

- **Import Bootstrap**
    - `npm i bootstrap-vue bootstrap`
    - v *main.ts*
    `import BootstrapVue from "bootstrap-vue";`
    `Vue.use(BootstrapVue);`

### Animacie
`<transition name="animacia"></transition>`

- **animate.js** vlastne animacie
- potom mam v css nove triedy:
`.animacia-enter-active`,`.animacia-enter`, `.animacia-enter-to`, `.animacia-leave-active`, ...

![](../img/transition.png)

- alebo mozem v transition elemente pouzit JS hooks a volat na nich metody, napr. `v-on:before-enter`, `v-on:enter`, `v-on:after-enter`, `v-on:enter-cancelled`, `v-on:before-leave`, ...

### Prebliknutie stranky pri nacitani Vue
- **v-cloak** atribut pridam do elementu
- mozem ho nastylovat v css:
    `[v-cloak] { display: none }`

### Code splitting 
- v routri mozem povedat webpacku aby componenty rozdeloval na chunks, tie su lazy loaded
- jeden chunk by mal mat do 70kB, hlavne ten v ktorom je prve nacitanie stranky

### Filtre
- podobne ako data, computed a metody mame sekciu filters:{}
- definicia:
    ```js
        nazovFiltra: function(paramater) {
                 return parameter.atr1 + ' ' + parameter.atr2  
         }
    ```
- v sablone volam s vyuzitim pipe `{{ volaco | nazovFiltra }}`
- `volaco` ide do paramatra funkcie
- filtre su teda specialita pre sablony (computed props su naviac cashovane)
- filter moze mat aj dalsie parametre

`nazovFiltra(paramater, dlzka) {...}`

`{{ volaco | nazovFiltra(dlzka) }}`

### mounted()
metoda `mounted()` je lifecycle hook pre vytvorenie vue komponentu, nepatri do methods, stoji samostatne

### Event bus
- udalost emitnem z root komponentu

    `this.$root.$emit('new-songs',parametre...)`

- zachytim tiez na root elemente, v sekcii elementu mounted (je to funkcia podobne ako `data()`)
    ```js
    mounted() {
         this.$root.$on('new-songs', data => {
             console.log('aha', data)
         })
    }
    ```

### SASS
- premenne *_variables.scss*
- v scss `@import 'variables';`

- definicia: `$text-color: red;`
- volanie: `color: $text-color`

- ak chcem variables importovat vsade kde je scss kod, pridam do projektu konfigurak *vue.config.js*
```js
module.exports = {
     runtimeCompiler: true,
     css: {
         loaderOptions: {
             sass: {
                 data: `
                      @import '@/assets/scss/_variables.scss';
                     `
             }
         }
    }
}
```
Vytvorenie pluginu
napr. src/helpers.js
const MyHelper = {
     install(Vue) {
          Vue.prototype.$log = function(data) {
               console.log(data)
          }
     }
}

export default MyHelper
potom v main.js:
import MyHelper from './helpers'
Vue.use(MyHelper)

potom vsade:
<element @nejaky-event="$log( $event )"

VUEX
store obsahuje objekty:
state, mutations, actions,plugins, getters 

data v store: 
$store.state.nejakaPemenna

volanie mutacii
$store.commit('VOLAKAMUTACIA')

volanie akcii
$store.dispatch('funkcia')
v akcii vlastne volam mutaciu, potrebujem na to akcii predat context v parametri (akcia(context){}) a cez kontext pristupim k commit (context.commit('MUTACIA'))

asynchronny kod vzdy do functions (mutacie su len sync)