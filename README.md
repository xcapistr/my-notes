# REACT TUTORIAL
odkazy:
 [https://jsbin.com](https://jsbin.com/) 
 [https://codepen.io](https://codepen.io/) 
[React – A JavaScript library for building user interfaces](https://reactjs.org/)


### REACT
JS kniznica pre UI

### Babel 
-kompiluje kod napisany v reacte(specialitky ako napr. JSX) a es6(>) do bezneho JS pre prehliadac
-zakladny komponent moze byt napisany ako funkcia (s velkym pismenom), ktora vrati JSX

### Render funkcia
-vyzaduje import reactDOM
-reactDOM.render(<Komponent/>, document.getElementById('id_rodica'));
-pre dotiahnutie rodica mozem pouzit aj `document.querySelector('#id_rodica')`

### Single-page vs multi-page
-single page apka - jedno html, prerendrovava sa na klientskej strane (cela apka je pod reaktivnym root komponentom - render staci zavolat raz)
-multi page apka - viac html, obsah sa rendruje na strane servera (moze mat reaktivne casti ktore si mozme predstavit ako widgety)

### ES6>*
-starsie verzie - var - premenne
-es6 -let (premenna), const (konstanta)

### Arrow funkcie
-nepiseme klucove slovo function
-unkcia je def. ako const (alebo let ak ju chcem reassignovat)
-nepouziva sa this keyword
-ak je vo funkcii len return, mozem vynechat {} aj klucove slovo return

### Export/import
-ak urobim v subore jeden default export, staci importovat import hocijakyNazovfrom 'relativna cesta k suboru', ak exportujem viac veci, musim import specifikovat import { konkretny export } from 'realtivna cesta k suboru'
-dalsie moznosti:
`import {nieco as Nieco} from '...'`
`import / as bundled from '...'`

### Dedicnost*
-extends, nezabudnut na super(); v konstruktore

### Atributy a metody
**atributy** tried sa pisu bez klucoveho slova - len myProperty = 45
**metody** tried sa pisu ako arrow funkcie
-ak takto napiseme matody a atributy, nepotrebujeme konstruktor ani metodu super();
-! lepsie practice je **nepouzit arrow funkciu na metody** (kamil)

### doplnanie poli a objektov
`const novePole = [...starePole, 1, 2]`
`const novyObjekt = { ...staryObjekt, novaProp: 2}`

### destructuring (destructive assignment)
`[a, b, ...rest] = [10, 20, 30, 40, 50];`

### Primitivne typy vs objekty
**primitivne typy** sa priradenim kopiruju (prenesie sa value)
-priradenim **objektu** do premennej naopak ziskame odkaz do pamati, nie kopiu
-priradenie kopie prevedieme nasledovne = { ...kopirovanyObjekt };

### Zoznamy, listy, pole
nieco ako for each -> pomocou metody map nad polom
pole.map((prvok, index) => { return <...> })

zide sa funkcia splice(odkial, kolkoPrvkov) na **odstranenie prvku z pola**
**vytvorenie kopie pola** pomocou funkcie slice: kopiaPola = pole.slice();
-alebo **spread operator**: kopiaPola = [...pole];
!index nie je dobry key (problematika z prekreslovanim domu -> stare sa porovnava s novym -> iny komponent moze mat rovnaky index ako nejaky co uz existoval...)

**vyhladanie prvku**
pole.findIndex(prvok => prvok === daco);

**kopia objektu**
kopiaObjektu = {...objekt}
kopiaObjektu = Object.assign({}, objekt)

**rozdelenie stringu** do pola stringov
split('deliaciZnak')
split('') -> rozbitie po znakoch


**prechadzanie pola**
-pomocou map
`pole.map((prvok, index)=>{ ... })`
dalsie dolezite funkcie nad polom:
`find()` `findIndex()` `filter()` `reduce()` `concat()` `slice()` `splice()`

**Prechadzanie asociativneho pola**
for (let key in data) {
  console.log({nazov: key, hodnota: data[key]});
}

### workflow
* dependency mngmnt tool: npm/yarn
* bundler: webpack
* compiler: babel
* dev server (local)

### preparation
-potrebujem npm a create-ract-app
 `sudo npm install create-react-app -g`

### jsx restrictions
/ na konci, class->className, vzdy 1 component, onclick -> onClick

### Functional (stateless) vs Class(stageful) components
**functional component** - najjednoduchsi komponent - len js funkcia -> best practice je pouzivat functional component tak casto ako sa len da (objekt props je v parametri, nemaju state)
**class-based component** - druhy sposob, komponent je objekt triedy, dedi z Component (k objektu props pristupujem cez this.props)
	* metody sa nezapisuju arrow funkciami
	* cez props sa mozem odkazovat aj k metodam-> metodu definovanu v rodicovi chceme volat z potomka, zabalime ju do props, a pomocou .bind nabindujeme this a ostatne parametre napr.	`<potomok click={this.funkcia.bind(this, 	dalsieAtributyFunkcie)}`
	* pouzitie funkcie bez bind -> pomocou arrow funkcie
	`<potomok click={() => this.funkcia}>`
	
-potomokovia komponentu v **props.children** -> vsetko co je vnutri
-komponenty by v render funkcii nemali vraciat privela JSX, niektore casti JSX je vhodne preddefinovat a potom len zavolat v ramci return()
-plati ze treba pouzivat co najviac stateless ale kazda feature(autentifikacia, blog, ...) ma pravo mat root komponent ktory je statefull
-statefull pouzivame len ked vieme ze bude potreba manageovat stav alebo mat pristup k lifecycle hooks

### lifecycle events
len u stateful komponentov
 [https://reactjs.org/docs/events.html#supported-events](https://reactjs.org/docs/events.html#supported-events) 

poradie:
**constructor(props)** - default es6 feature, volanie super(props), nastavovanie stavu(inicializacia), ziadne sideEffects(requesty...)
**componentWillMount** - toto je uz react kniznice, update stavu, opat ziadne side effects
**render()** - priprava a strukturovanie JSX
pri render funkcii sa rendruju aj child komponenty
**componentDidMount** - sem patria side effects, nepatri sem update stavu (nic co znova spusti render)

**componentWillUnmount**- ked ma byt komponent odstraneny z DOMu

Update udalosti(volane rodicom) - poradie:
**componentWillRecieveProps(nextProps)** - synchronizacia stavu a props, nepouzivat na side effects
**shouldComponentUpdate(nextProps, nexState)** - rozhodovanie ci pokracovat, takto mozme setrit vykon a zabranovat nechceny updatom, nepouzivat na side effects
**componentWillUpdate(nextProps, nextState)** -synchronizacia stavu a props, nepouzivat na side effects
**render()**
updatovanie props potomkov
**componentDidUpdate()** - vhodne na side effects, nie na zmenu stavu

Update udalosti(sposobene internou zmenou) - poradie:
**shouldComponentUpdate(nextProps, nextState)**- rozhodovanie ci pokracovat, takto mozme setrit vykon a zabranovat nechceny updatom, ak zastavime update komponentu zastavime aj jeho potomkov, nepouzivat na side effects
**componentWillUpdate(nextProps, nextState)**-synchronizacia stavu a props, nepouzivat na side effects
**render()**
updatovanie props potomkov
**componentDidUpdate()**- vhodne na side effects, nie na zmenu stavu

**sedovanie zmien v DOMe** - dev tools -> ... -> more tools -> rendering -> enable flashing

**PureComponent** - mozno importovat z kniznice react, ak vytvorime novy komponent ako potomka Pure komponentu, bude tento komponent automaticky sledovat zmeny stavu a props a rozhodovat ci je nutny update komponentu alebo nie, !nepouzivat vsade - zbytocne by zralo vykon

updatovanie realneho DOMu v reacte -> najskor sa vytvori novy (virtualny) DOM, potom sa porovna realny a virtualny DOM, len pokial sa najdu rozdiely, prebehne update realneho domu

ak chcem rendrovat elementy v komponente treba ich zabalit do divu, !ALE pole vyrendrovat ide, takze mozem rovno pouzit map alebo v render funkcii vratit pole JSX elementov -> return[<>,<>,<>,...<>]

### manipulacia so state
-priradenim mozme stav menit len v konstruktore
`this.setState(state)` -> aktualny stav sa zmerguje so stavom ktory dodame v parametri
-ak vychadza stav z povodneho stavu `this.setState(povodnyState => povodnyState + nejakaMutacia)`

### 2-way databinding
-v rodicovi vytvorim funkciu->eventHandler, s parametrom e (event), v tejto funkcii potom mozem menit state a priradit don hodnotu `e.target.value` (pokial bude event zavolany z komponentu ktory ma value, napr. input)
-v JSX rodica potom zavolame potomka aj s property, napr. changed do ktorej priradime handler, napr. `changed={this.nejakaHandlerFunkcia()}`
-v potomkovi vezmeme handler funkciu z props a priradime ju k `onChange` eventu nad input elementom, input elementu tiez nastavime value na props.nejakaHodnotaPreberanaOdRodica aby bol binding obojstranny,
napr.: `<input type="text" onChange={props.changed} value={props.name} />`

### styling
nezabudnut importovat css subory do js -> funguje to vdaka webpacku, importujeme aj s priponou css
-webpack dynamicky injektuje styly do html sablon -> v zdrojaku potom vidiet nove `<style>` tagy
!vsetky styly definovane v css subore su globalne

-namiesto css suborov mozme pouzivat aj inline styly, napr. ich v render funkcii vlozim do konstanty ako objekt {}, ale vsetky nazvy css properties pouzivaju namiesto pomlciek cammelCase - vyhoda - su v jednom scope s komponetom, nie su globalne

-dynamicka zmena stylu -> staci prepisat atributy v nadefinovanom style objekte
alebo pridame celu triedu -> mozme ich spajat
`let classes = ["red", "bold"].join(" ");`

**RADIUM**
-riesipseudoselektory(:hover) a mediaquery(@media)
`npm install --save radium`
`import Radium from 'radium'`
pouzijeme ho tak ze nim obalime exportovany App komponent v subore App.js (alebo ine komponenty)
export default Radium(App);

potom mozme do objektu styles pridat napr:
```js
styles = {
  ':hover':{...}
} 
```
alebo takto prepisat:
`styles[':hover']={...}`

rovnako aj media query
`’@media (min-width: 500px)':{...}`
!ale naviac je potreba 
`import{ StyleRoot } from "radium";`
a potom obalit cele JSX do `<StyleRoot>` komponentu

**CSS modules**
odtienenie stylov komponentov
potrebna konfiguracia:
najskor zavolame script npm run eject aby sme povolilizmeny konfiguracie
pribudnu 2 zlozky: config, scripts
otvorime config -> **webpack.config.js** a upravime
```
test: cssRegex,
exclude: cssModuleRegex,
use: getStyleLoaders({
importLoaders: 1,
modules: true,
localIdentName: '[name]__[local]__[hash:base64:5]',
}),
```

potom mozem importovat css subor ako:
`import classes from "Subor.css"` (namiesto classes moze byt cokolvek)
-classy potom nepriradujeme ako string (className="App") ale ako objeky(`className={classes.App}`)

ked aplikujem styl na element, napr. App, v css subore mozem stylovat aj vsetky jeho zanorene elementy, napr. pouzijem selector .App button {} - pre vsetky tlacidla zanorene do App elementu

viac info o moduloch: [https://github.com/css-modules/css-modules](https://github.com/css-modules/css-modules) 

### Zobrazit/skryt elementy (toggle)
-elementy dame do {}
-vnutri {} mozme pouzit terarny operator (if nemozno pouzit pretoze ide o blokovy prikaz a v {} mozme pouzit len jednoduche)
-v render funkcii sa da teda napisat: `{ podmienka ? <elementy> : null }`

! lepsie:podmienka && <element>

2.sposob (doporuceny):
-render funkcia je volana vzdy ked sa zmeni stav, este pred return() si mozem do nejakej premennej nahrat JSX komponentu ktory chcem skryvat (resp. podla podmienok rozhodnem co do tejto premennej zapisem, napr. aj null),
-v render funkcii v return() potom vlozim tuto premennu

### Debugging*
-normalne v chrome alebo vs code
vyhodenie chyby -> throw new Error('Chybova hlaska');

**ErrorBoundary**
-nova feature react 16
-rovnomenny komponent
```js
import React, { Component } from "react";

class ErrorBoundary extends Component {
 state = {
  hasError: false,
  errorMessage: ""
  };

 componentDidCatch = (error, info) => {
  this.setState({ hasError: true, errorMessage: error });
  };

 render() {
  if (this.state.hasError) {
   return <h1>{this.state.errorMessage}</h1>;
  } else {
   return this.props.children;
  }
  }
}

export default ErrorBoundary;
```

potom zabalim lubovolny komponent do `<ErrorBoundary key={nieco co pouzijem ako kluc}>`
-zabalujeme len komponenty pri ktorych ocakavame chybu ale nemozme ich inak kontrolovat, nepouzivame na celu apku

### Struktura projektu, adresare
doporucene zlozky/adresare -> Components(komponenty, zanorene komponenty zanorujeme aj v FS), Containers(kontajnery, styly, testy)

### Higher-order components (HOCs)*
davame do zlozky ./hoc, obaluju komponenty (a pridavaju nejaku logiku (napr pridavat css triedy)), sluzia namiesto obyc. divu, su to funkcionalne komponenty(stateless)
najjednoduhsia podoba:
`const HocKomponent= props => props.children;`
`export default HocKomponent;`

ina konvencia sa pise s malym pismenom a vyzera takto (funkcia ktora vrati funkciu):
```js
const withClass = (WrappedComponent, className) => {
return (props) => (
 <div className={className}>
  <WrappedComponent {...props} />
 </div>
 )
}

export default withClass;
```

-tuto funkciu mozem potom zapojit do exportu nejakeho komponentu, napr.:
`export default withClass(Komponent, cssTrieda);`

-ked potrebujem statefull komponent, mozem rovnako definovat funkcu ktora vrati tiedu:
```js
const withClass = (WrappedComponent, className) => {
 return class extends Component {
  render(props) {
   return (
    <div className={className}>
     <WrappedComponent {...this.props} />
    </div>
   )
  }
 }
}
```

### Fragment 
-da sa ale pouzit aj uplne prazdny komponent (od reactu 16.2) - vsetky elemeny v render funkcii zabalime do <> </>

### Spravne pouzivanie setState
-v setState by sme sa nemali odkazovat na this.state_ - napr. ak by sme nastavovali 
pocitadlo
`this.setState({pocitadlo: this.state.pocitadlo + 1})` -> spatne
```js
this.setState((prevState, props) => {
  return { counter: prevState.pocitadlo + 1}
})
```   
-> spravne je parametrom setState funkcia

### Validovanie properties
**PropTypes**: number, string, func, object, bool, array, symbol, node, element, oneOfType([...]), arrayOf([...]) ............

`npm install --save prop-types`
`import PropTypes from 'prop-types'`

zakladne definovanie typov (definujeme mimo triedu ale este pred exportom samozrejme):
```js
komponent.propTypes = {
 click: PropTypes.func,
 name: PropTypes.string,
  age: PropTypes.number,
  changed:PropTypes.func
}
```
dalej mozme retazit s typom napr. .isRequired

-pri nedodrzani typov nemusi nutne spadnut ale do konzoly sa vypisuju varovania
-nefunguje s funkcionalnymi komponentami
 [https://reactjs.org/docs/typechecking-with-proptypes.html](https://reactjs.org/docs/typechecking-with-proptypes.html) 

### Reference("ref")
ref je specialna property ako napr. key, vdaka nej mozme vytvorit odkaz na element
len v statefull komponentoch
napr. mame v JSX element <input>, dame mu property `ref={(inp) => {this.odkazNaInput = inp}}`
teraz mame vytvoreny odkazNaInput ku ktoremu mozme v ramci triedy pristupovat cez this
!!!nepouzivat na stylovanie

v novej verzii reactu (od 16.3) uz v konstruktore definujem:
`this.odkazNaInput = react.createRef()`
v rendrovanom JSX potom elementu `<input>` dam property `ref={this.odkazNaInput}`

!!!ak by som chcel volat napr. metodu focus() nad inputom, v prvom pripade by stacilo
`this.inputElement.focus()` ale v druhom pripade musim napisat (specifikovat **current**)
`this.inputElement.current.focus()`

### forward reference 
`React.forwardRef()`
vyuzitie pre HOCs - aby sme mohli pouzivat referencie medzi komponentami, ktore su obalene v HOCs, v HOC komponente(ktory je definovany ako funkcia ale vracia triedu ako konstantu),namiesto vratenia triedy rovno si ju ulozime do konstanty a potom vratime:
```js
return React.forwardRef((props, ref) => {
return <NejakaClass {...props} forwardedRef={ref} />;
})
```
poznamka - prop forwardedRef nie je pevne dany nazov, mozem si pomenovat jak chcem

dalsi krok je doplnenie WrappedComponentu:
verzia bez ref `<WrappedComponent{...this.props} />`
verzia s ref `<WrappedComponent ref={this.props.forwardedRef} {...this.props} />`

### Contex API
opat v Reacte 16.3
niektory element bude provider, dalsi consumer
vyhoda je ze medzi providerom a consumerom moze byt x zanoreni a nemusime retazit donekonecna props, pouziva sa hlavne pre nejake globalne nastavenia (napr. farba temy vybrana pouzivatelom)

**Provider**
definujeme mimo triedy, napr:
```js
export const SomeContext = React.createContext(false)
//false je default value, v triede kde rendrujem providera napisem:
<SomeContext.Provider value={this.state.nejakaValue}>
 {/*nejake JSX, v komponentoch ktore su tu mozem dotahovat context*/}
</SomeContext.Provider>
```

**Consumer**
klasicky `import { SomeContext } from '...'`
potom v JSX napr.:
```
<SomeContext.Consumer>
{hodnota=> (hodnota? <p>Hodnota je true</p> : null)}
</SomeContext.Consumer>
```
### Nove Lifecycle metody (react 16.3)*

**static getDerivedStateFromProps(nextProps, prevState)**
-spusti sa vzdy ked su updatovane props a dava to sancu zaroven updatovat aj stav, casto to nechceme, casto chceme mat props a state ako 2 nezavisle veci, ale niekedy sa hodi, funkcia moze nastavit stav cez setState alebo staci ze vrati stav, ak vratime prevState z parametra, stav sa nikdy zmenou props nezmeni, nemalo by sa pouzivat zaroven componentWillMount a componentWillUpdate

**getSnapshotBeforeUpdate()**
-umoznuje vytvorit snapshot DOMu, pred tym nez sa updatuje, dobry usecase je napr. ulozenie pozicie scrollbaru a v componentDidUpdate mozme presunut

### Memo
-pri exporte mozme obalit komponent do export default React.memo(komponent), komponent sa bude updatovat len ked sa props skutocne zmenia

### Planovanie a setup novej aplikacie
-strom komponentov_struktura -> stavy_data -> komponenty v containeri
-plan je dolezity aj ked sa pri implementacii zrejme niekolkokrat zmeni
-stav ma byt definovany v komponente ktory je na nom naozaj zavisly a nie vzdy nad celou aplikaciou
-ak chcem pouzivat css moduly, je potreba pouzit npm run eject,...(zmenu konfiguracie pozri vyssie)
-ak chcem iportovat fonty (najlepsie z google fonts) vlozim link do hlavicky v ._public_index.html
-bezna struktura na zaciatku:
.src/
  -components/   --> stateless
   -layout/
   -layout.js
  -containers/ --> statefull
  -assets/
 -hoc/

### .reduce(prev, curr)
-obyc. JS funkcia
-transformuje pole
-parametre: predchadzajuca hodnota, sucastna hodnota

### HTTP
-json placeholder: [https://jsonplaceholder.typicode.com/](https://jsonplaceholder.typicode.com/) 
-jeden zo sposobov je vyuzit js objekt XMLHttpRequest - vytvorim si vlastny request

-lepsia moznost je pouzit nejaku kniznicu
-> **AXIOS**
 [https://www.npmjs.com/package/axios](https://www.npmjs.com/package/axios) 
 [https://github.com/axios/axios](https://github.com/axios/axios) 
```js
npminstallaxios --save
import axios from 'axios';
axios.get('url-metodaMusiMatMinimalneTentoParameter')
  .then(response => { console.log(response)})
```
ajax volania su side-effects a patria v ramci creation lifecycle do `componentDidMount()`

`axios.post('url', postedData)`
`axios.delete('url');`

**odchytenie chyb** 
-klasicky: `axios.get('url').catch(e => {})`

**Interceptors**
-umoznuju globalny setup http requestov a responsov
-napr. ak chceme pridat autentikaciu do headera alebo pri odchyteni chyb
-pouzivame v nejakom nadradenom subore, napr. index.js

interceptor pre **request**:
```js
axios.interceptors.request.use(
request => {
 console.log(request);
 // Edit request config
 return request;
 },
error => {
 console.log(error);
 return Promise.reject(error);
 }
);
```
-bez return request by sme ostatne requesty blokovali, tento request nie je request v pravom zmysle, je to **request setup**

-rovnako napiseme interceptor pre **response**:
```js
axios.interceptors.response.use(
response => {
 console.log(response);
 // Edit response config
 return response;
 },
error => {
 console.log(error);
 return Promise.reject(error);
 }
);
```

**odpojenie interceptora**
1. ulozim si ho do premennej
  `var myInterceptor = axios.interceptors...`
2. zavolam nad nim eject
 `axios.interceptors.request.eject(myInterceptor);`

**defaultne nastavenia axios**
-napr. pre zaciatok URL adresy ktora je vzdy rovnaka, pri http requestoch potom pouzijem uz len doplnky k URL ako napr. /posts
-tiez v index.js
`axios.defaults.baseURL = ";`

-alebo mozem nastavit defaultne headre:
`axios.defaults.headers.common["Authorization"] = "AUTH TOKEN";`
`axios.defaults.headers.post["Content-Type"] = "application/json";`

**instance**
-mozem vytvorit v samostatnom subore novu instanciu axios a nastavit jej nejake defaultne parametre, potom ju mozem niekde importovat ako axios a pouzivat uplne rovnako, napr.:
```js
import axios from "axios";
const instance = axios.create({
 baseURL: "https://jsonplaceholder.typicode.com"
});
instance.defaults.headers.common["Authorization"] = "AUTH TOKEN FROM INSTANCE";
export default instance;
```

### ROUTING
-nie je defaultne v reacte(teda od fcbk) ale je to de facto standard
-existuje potreba priniest rovnaky UX ako v multipage aplikacii, preto sa vytvoria rozne cesty(routes) ktore budu zobrazovat rozny obsah

`npm install --save react-router react-router-dom`
(rr - logika, pre routing nie je nutna, na bezny vyvoj staci rrd, rrd - rendrovanie)

-potom obalim hlavny root komponent (index.js/App.js alebo komponent kde chcem robit routing)
```js
import { BrowserRouter } from 'react-router-dom’

<BrowserRouter>/*max 1 potomok*/</BrowserRouter>
```
-taktiez mozem obalit aj cely <App /> komponent priamo v atribute render funkcie v index.js

potom v komponente ktory bude mat vlastnu path puzijem route komponent
```js
import { Route } from "react-router-dom";

<Route path="/" exact render={() => <h1>Home</h1>}/>
```
(**exact** pouzivame preto ze defaultne chape react router tuto cestu "_" len ako prefix ktory splnia vsetky paths ktore zacinaju na "_", vdaka exact sa berie do uvahy len cesta "/" a ziadna ina)
-**viacere Route** elementy mozu mat tu istu path -> vyrendruju sa naraz
-taktiez mozem na Route elemente namiesto render property definovat `compoenent={nejakyKomponent}` a ten sa v ramci path cely vyrendruje
`<Routepath="/"*exact*component={NajakyKomponent}`
pripadne s premennou
`<Routepath="/:id"*exact*component={NajakyKomponent}`
k premennej sa dostanem cez `props.match.params`
do jednotlivych paths sa uzivatal dostava pomocou klasickeho odkazu `<a href="/relativna-cesta">`

**Routing bez reloadu**
-doteraz spominanym sposobom nastane po preroutovani reload stranky a stratime state
-na zamedzenie tohto javu pouzijeme Link importovany z react-router-dom vkomponente kde definujeme jednotilive <Route /> elementy
`import { Route, Link } from 'react-router-dom';`

ako odkazy potom nepouzivame elementy `<a>` ale `<Link>` a to nasledovne:
`<Link to="/">Home</Link>`

`<Link to={{pathname: '/new-post', hash: '#submit', search: '?quck-submit=true'}}>New Post</Link>`

-v linku mozem takisto pouzit prop. **exact** - to iste ako v route

**Nove props pri routingu**
-pri pouziti routra sa nam automaticky doplnia niektore props (history, location, match...), nie su dostupne u potomkov, na to aby boli, treba pri volani potomka doplnit:
`<Potomok vlastnaProp={} {...this.props}>`

-lepsi sposob je pouzitv potomkovi HOC -> withRouter
`import { withRouter } from "react-router-dom"`
-potom exportovat:
`export default withRouter(mojKomponent)`

-takto budu vsetky **props** ktore suvisia s routovanim(history, location, match...) dostupne aj v komponente ktory je potomok

**Absolutne vs relativne cesty**
'/cesta' -> vzdy sa nalepi rovno za domenu, defaultne je to teda absolutna cesta
-ked chceme pouzit relativnu cestu mozme vyuzit url porperty z props.match
`<Link to={props.match.url + '/new'}>`

**Zistovanie aktivnej route**
namiesto Link pouzijem **NavLink** (obsahuje nejake props naviac)
`import { Route, NavLink } from "react-router-dom"`
-bezny Link sa po kompilacii premeni na `<a>`, NavLink vytvori `<a>` s pridanou triedou -> `<a class="active">`
-takto mozem nastylovat link ktory je prave aktivny, v reacte moze byt aktivnych aj viac linkov naraz, preto napr. pri path "/" pouzivame **exact** aby bola aktivna vyhradne jedna cesta

-nemusime pouzivat defaultnu triedu .active ale vytvorit vlastnu, staci ked v `<NavLink>` doplnime `activeClassName="nazovTriedy”`
-alebo inline styl pomocou `<NavLink activeStyle={{color: 'red'}}>`

**Query parametre v paths**
2 sposoby:
`<Link to="/my-path?start=5">`
`<Link to={pathname:'/my-path',search:'?start=5'}}>`

-React router umoznuje pristup k parametrom cez `props.location.search`
-tu sa ale ukladaju len retazce, napr. `?start=5`
-pre **rozparsovanie** sa da pouzit tento snippet:
```js
componentDidMount() {
 const query = new URLSearchParams(this.props.location.search);
 for (let param of query.entries()) {
  console.log(param); // yields ['start', '5']
 }
}
```

**Fragmenty v paths**
podobne ako query
`<Link to="/my-path#start-position">`
`<Link to={{pathname: '/my-path', hash: 'start-position'}}>`
ulozene v `props.location.hash`

**Switch**
zabezpeci ze sa bude rendrovat vzdy len jedna route, vsetky routes teda obalime do jedneho switchu (samozrejme mozu stale niektore routes existovat aj mimo switch, pripadne moze existovat niekolko switchov )
```js
import { Route, NavLink, Switch} from "react-router-dom"

<Switch>
<Route path="/" exact component={} />
<Route path="/new-post" component={} />
<Route path="/:id" exact component={} />
</Switch>
```
!pozor na poradie, switch vyberie len prvu zhodu, takze ak vymenim /new-post a /:id ktore ma premennu, moze byt za premennu povazovane aj new-post a zavola sa nespravna route

**history.push**
-ked chceme naprogramovat odnavigovanie na nejaku path
-pri routingu sa tvori zasobnik paths v ramci ktoreho sa vieme dostat k historii
-funkciou `this.props.history.push({pathname: '/' + id})` pridame do zasobnika novu path/route, tiez postaci `this.props.history.push('/' + id)`

**Nested routes**
-ked nejaka route odkazuje na komponent v ktorom je dalsia routa odkazujuca na element
-pri zanoreni mozme namiesto "hardcoded" path napisat nieco ako:
`<Routepath={this.props.match.url + "/:id"} exactcomponent={...}/>`
-pri dynamickej zmene route (napr. ked sa zmeni :id) sa nemusi zavolatunmount a remount ale update ano

**Redirection**
-bezne_ presmerovanie
`<Redirect from="/" to="/posts" />`

**presmerovanie pomocou redirect**
-v render funkcii:
```js
let redirect = null;
if (nejakaPodmienka) {
 redirect = <Redirect to="/niekam" />;
}

return (
  ...
  {redirect}
  ...
)
```

**presmerovania pomocou history.push**
`this.props.history.push("/niekam")`

**presmerovanie pomocou history.replace**
-podobne ale ked pojdem v historii spat uz sa nevratim na stranku z ktorej som sa presmeroval
`this.props.history.replace("/niekam")`

**Navigation guards**
paths ktore mozem navstivit len po autentizacii (pripadne na zaklade role alebo inej podmienky)
-znamena to ze vykreslenie elementu `<Route />` bude podmienene, pokial sa v JSX namiesto neho dosadi null, po snahe prejst na danu route budeme presmerovany naspat

**404 case** - presmerovanie k neznamemu zdroju
-staci ak do `<switch>` zabalim ako poslednu <route> bez definovanej path, nieco ako:
`<Route render={() => <h1>Not found</h1>}>}</Route>`
-funguje to ako default option v klasickom switch-case

### CODE SPLITTING/LAZY LOADING
-technika postupneho nacitavania - aby sa pouzivatelovi nestiahla zbytocne cela stranka ale len casti na ktore sa routuje
-oplati sa len pri vacsich aplikaciach (nema zmysel postupne dotahovat stranku po par kB)

**po starom**
pridal som komponent pomocou `import Komponent from "../cesta";`

**po novom**
pridame HOC - asyncComponent.js
```js
import React, { Component } from "react”

const asyncComponent = importComponent => {
 return class extends Component {
  state = {
   component: null
  };

  componentDidMount() {
   importComponent().then(cmp => {
    this.setState({ component: cmp.default });
   });
  }

  render() {
   const C = this.state.component;
   return C ? <C {...this.props} /> : null;
  }
 };
};

export default asyncComponent;
```

-pridame komponent ako konstantu AsyncKomponent
```js
import asyncComponent from "../cesta";
const AsyncKomponent= asyncComponent(() =>import("./cesta-ku-komponentu"));
```
-ked otvorim network v nastrojoch pre vyvojara a sledujem zdrojove subory, mozem si vsimnut ze ku zakladnemu **bundle.js** pribudaju **chunk.js**


**lazy loading v react 16**
-nepotrebujem vytvarat HOC
-staci importovat komponent cez **React.lazy()**:
`const Komponent = React.lazy(() => import('./cesta-ku-komponentu'))`

-potom este importujem
`import { Suspense } from "react";`

-a definujem route ktora ma byt lazy
```js
<Route
 path="/posts"
 render={() => (
  <Suspense fallback={<div>Loading...</div>}>
    <Posts />
  </Suspense>
 )}
/>
```
-lazy loading nemusim pouzivat len subezne s routingom ale aj pri beznom toggle

**!!!ROUTING AND SERVER DEPLOYMENT**
-bezny web funguje tak ze uzivatel posle poziadavku na server, server ju obdrzi a vrati dokument zo zdroja (ak ho najde), v pripade React Appky by ale nenasiel ziadnu z nadefinovanych routes pretoze aplikacia je single page, preto musi byt server, na ktory React Appku umiestnime, nastaveny tak aby vzdyvracal **index.html!!!**, dokonca aj v pripade 404 -> chyby si uz osetrime sami v React Appke, pretoze server vrati index.html aj v pripade ze bude cast URL nespravna

-ak mam vlastnu domenu napr. mojaapka.bla/ nie je treba nic specialne konfigurovat
-rozdiel je ak je moja apka na inej domene napr. nejakadomena.bla/mojaapka musim nastavit base path
  -tam kde mam browser router pridam prop **basename=**"/mojaapka"

### React.Fragment
-nahradzuje predosle auxilary HOC

### String to Number
retazec = '89';
cislo = +retazec;

alebo
retazec = '89.5';
cislo = Number.parseFloat(retazec);

### event.preventDefault
-ak chcem zabranit odoslaniu requestu

### zaokruhlovanie
cislo.toFix(pocetMiest);



## REDUX
 [https://redux.js.org](https://redux.js.org/) 
management stavu, ktory je ulozeny v centralnom store
npm install --save redux

const redux = require('redux');

**1.**najskor treba vytvorit**reducer** - 2 argumenty - state (sem priradim pociatocny stav) a action
const rootReducer = (state = initialState, action) => {
  return state;
};

**2.**potom vytvorim **store**(najlepsie v index.js, vid bod 6)
### const store = redux.createStore(rootReducer);


k stavu sa potom dostanem cez store
store.getState();

**3.**definovanie dispatching **action** (povinny argument type a hodnota je uppercase, dalsie argumenty su additional info) - nemusim ju rovno dispatchovat, funkcie si vacsinou preddefinujem v mapDispatchToProps, nasledujuci priklad je priame dispatchovanie(vykonanie) akcie:
store.dispatch({ type: 'ADD_COUNTER', value: 10 });

**4.**pridanie funkcii do reducera
const rootReducer = (state = initialState, action) => {
 if (action.type === "INC_COUNTER") {
   const newState = Object.assign({}, state);
   newState.counter = state.counter + 1;
   returnnewState;
  }
 return state;
};

kratsi zapis
constrootReducer= (state=initialState,action)=>{
 if(action.type==="INC_COUNTER") {
  return{
    ...state,
    counter: state.counter+ 1
   };
  }
 returnstate;
};

!!!state niekdy priamo nemenime, vzdy treba urobit kopiu a tu vratit

**5. Subscription** - definujem hned po store - reaguje na kazdu zmenu/akciu, napr:
store.subscribe(() => {
 console.log("[Subscription]", store.getState());
});

-store vytvorim v index.js, reducer sem importujem
import { createStore } from 'redux';
const store = createStore(reducer);

-reducery vytvaram v samostatnych suboroch, root reducer:._src_store/reducer.js
-v rootReduceri inicializujem aj stav (initialState)

**6.**pripojenie reduxu do reactu - potrebujem package react-redux
npm install --save react-redux

potom v index.js importujem **provider** a zabalim do neho app komponent
import { Provider} from "react-redux";
ReactDOM.render(<Provider store={store}><App _><_Provider>, document....
**pripojenie store do reactu** -v jednotlivych komponentoch potom nastavujem pripojenie:
import { connect } from "react-redux";

pred exportom komponentu musim vyriesit namapovanie stavu a dispatch do props,
connect nie je HOC ale funkcia ktora vracia HOC, exportujemnasledovne:

const mapStateToProps = state => {
 return {
  ctr: state.counter
 }
}

const mapDispatchToProps = dispatch => {
 return {
  onIncrementCounter: () => dispatch({ type: "INCREMENT" })
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(Counter);

-namapovanim state do props (mimo triedu) si vyberiem atributy stavu a budem ich pouzivat ako props, takze uz sa nebudem odkazovat stylom state.counterale props.ctr, zaroven uz nepotrebujem uchovavat state v komponente

!ked pridavam prvok do pola pomocou**push()** menim povodne pole, ak pouzijem **concat()** vytaram novu kopiu pola, pomocou **filter()** tiez vytvaram kopiu

**Outsorising typovakcii v reduxe**
actions do samostatneho suboru ._src_store/actions.js - tu exportujem konstanty s nazvami akcii aby v buducnosti nedochadzalo ku preklepom:

export const NEJAKA_AKCIA = 'NEJAKA_AKCIA';

potom do reducer.js importujem
import * as actionTypes from './actions';

case actionTypes.NEJAKA_AKCIA....

**Kombinovanie reducerov**
v index.js
import { combineReducers } from 'redux';

const rootReducer = combineReducers({
 ctr: counterReducer,
 res: resultReducer
});

potom pristupujem kstate s jednym zanorenim naviac: state.ctr.counter
-nevyhoda - kazdy reducer ma svoj stav, takze nemozem v jednom reduceri nahliadnut do stavu druheho

**Typy stavu**
nie vsetok state musime manageovat pomocou reduxu
-local UI state - napr. show/hide modal - netreba redux
-persistentny stav - napr. uzivatelia, orders,... - vacsina stavu je na servri (DB), relevantna cast moze byt manageovana reduxom
-client state - napr. is Auth? filters set by user - definitivne manageovat reduxom

**REDUX ADVANCED**
-zapojenie middleware do projektu (medzi dispach akcii a reducery - ked chcem urobit nieco z akciou pred tym ako dojde do reducera (napr. log))
-middleware - funkcie/kod ktore hooknem do procesu a ktore sa potom vykonaju ako sucast procesu bez toho zeby sa zastavil

v index.js (tam kde vytvaram store)

import {**applyMiddleware compose** } from "redux";

const **logger** = store => {
 return next => {
 return action => {
  console.log("[Midleware] Dispatching", action);
  const result = next(action);
  console.log("[Middleware] next state", store.getState());
  return result;
  };
 };
};

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
 rootReducer,
 composeEnhancers(applyMiddleware(logger))
);


-toto bude vypisovat logy vzdy, ked sa dispatchne nejaka akcia

**redux devtool for chrome**
 [https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en) 
 [https://github.com/zalmoxisus/redux-devtools-extension](https://github.com/zalmoxisus/redux-devtools-extension) 
-zapojenie je o nieco zlozitejsie ked pouzivame middleware
-prave na to pouzivame **composeEnhancers**
-potom mozem vo vyvojarskych nastrojoch sledovat postupne zmeny stavu a prechadzat medzi nimi

Asynchronne funkcie (vracajuce propmisy) - nejdu len tak pridat do reducera
**async kod** v reduceri volame cez **action creator** (definujeme v ._src_store_actions_actions.js)
definovanie akcie:
export const increment = () => {
 return {
  type: INCREMENT
 };
};

-konvencia je pomenovat funkciu rovnako ako typ ale s cammelCase

tam kde ju dispatchujem potompridam import
import { increment } from ".._.._store_actions_actions";

v mapDsipatchToProps potom zapisem:
onIncrementCounter: () => dispatch(increment()),

vacsinou je akcii viac takze zapisem:
```js
import * as actionCreators from "../../store/actions/actions";

onIncrementCounter: () => dispatch(actionCreators.increment()),...
```

**Asynchronne volanie**
pre asynchronne spravanie pridam middleware (hook medzia akciou a reducerom), pouzijem kniznicu **redux-thunk**
`npm install --save redux-thunk`

**v index.js**
```js
import thunk from "redux-thunk";
const store = createStore(
 rootReducer,
 composeEnhancers(applyMiddleware(logger, **thunk**))
);
```
### teraz mozem v actions.js definovat akciu (storeResult), ktora bue asynchronne volat inu akciu(saveResult)

export const **storeResult** = res => {
 console.log(res);
 return dispatch => {
  setTimeout(() => {
   dispatch(**saveResult**(res));
    }, 2000);
  };
};

**!**Kde umiestnit **logicke operacie**? Do **Action Creatora**alebo do**Reducera**
action creator - tu moze byt asynchronny kod, nemal by sa tu prilis pripravovat update stavu
reducer - len synchronny kod, zakladny redux koncept - update stavu

**getState v action creatore**
action creator- redux-thunk moze mat v creatore dalsi atribut - getState
takto mozme pristupit k stavuhned pred tym nez sa asynchronnedispatchne nejaka akcia:

export const storeResult = res => {
  console.log(res);
 return (dispatch, **getState**) => {
  setTimeout(() => {
   const oldCounter = **getState()**.ctr.counter;
   console.log("old counter:", oldCounter);
   dispatch(saveResult(res));
  }, 2000);
 };
};

**pridanie utility funkcii**
-rozne funkcie na ulahcenie prace/zprehladnenie kodu
-v zlozke store -> utility.js
-napr. utilita pre update objektu:
export const updateObject = (oldObject, updatedValues) => {
return {
  ...oldObject,
  ...updatedValues
 };
};

po starom som v reduceri zapisal:
case actionTypes.DECREMENT:
return {
  ...state,
 counter: state.counter - 1
};

po importe utility updateObject mozem zapisat:
case actionTypes.DECREMENT:
 return updateObject(state, { counter: state.counter - 1 });

**zostihlenie switch-case v reduceri**
-vpodstate tu ide len o to aby bol na kazdy case len 1 riadok kodu (return)
-vsetko naviac dame do samostatnej funkcie v tom istom subore a pomenujeme ju tak ako sa vola akcia v danom case (akurat v cammelCase)

**Adresarova struktura ./store**
./store
  ./actions
   ./actionTypes.js
   ./index.js
   ./volaco1.js
   ./volaco2.js
  ./reducers
   ./volaco1.js
   ./volaco2.js

**--koniec reduxu**-dostudovat dokumentaciu, hlavne [https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns](https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns) 


**TESTOVANIE**
 [https://airbnb.io/enzyme/docs/api/](https://airbnb.io/enzyme/docs/api/) 
 [https://jestjs.io/docs/en/getting-started](https://jestjs.io/docs/en/getting-started) 


build app -> manualne testovanie ->**automaticke testy** (unit) -> ship app to server
tools:
  -test runner
	- [ ] vykonava testy a poskytuje Validation Library
  -> Jest(defaultne pri create-react-app)
  -testing utilities
	- [ ] simuluju react apku (stavba DOMu...)
  -> react test utils (official), enzyme (by rbnb)

co **ne**testovat?
-3rd party libraries
-complex connections (prilis zlozite prepojenia)

co testovat?
-izolovane jednotky
-podmienene vystupy

jest jepridany defaultne, takze este potrebujem doinstalovat:
npm install --save enzyme react-test-renderer enzyme-adapter-react-16

**subory** pomenuvam: _komponent.test.js_

import { configure, shallow } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
shallow - rendrovanie komponentov (len jeden zo sposobov)
adapter - potrebujem pre nakonfigurovanie enzyme


priklad - otestovanie ci komponent obsahuje 2 zanorene elementy (zanorene elementy su funkcionalne komponenty) a ci obsahuje 3 zanorene komponenty ak jeauthenticated

import React from "react";

import { configure, shallow } from "enzyme";
import Adapter from "enzyme-adapter-react-16";

import NavigationItems from "./NavigationItems";
import NavigationItem from "._NavigationItem_NavigationItem";

configure({ adapter: new Adapter() });

**describe**("<NavigationItems />", () => {
**it(**
"should render two <NavigationItem /> elements if not autenticatied", 
 () => {
 const wrapper = shallow(<NavigationItems />);
 **expect(**wrapper.find(NavigationItem)**).to**HaveLength(2);
  }
**)**;
 
 **it(**
  "should render three <NavigationItem /> elements if autenticatied",
  () => {
  const wrapper = shallow(<NavigationItems isAuthenticated />);
  **expect(**wrapper.find(NavigationItem)**).to**HaveLength(3);
  }
 **)**;
});



helper metody
beforeEach() - vykonana pred testami
afterEach() - po testoch

napr pre rendrovanie elementu zakazdym mozme napisat:
let wrapper;
beforeEach(() => {
wrapper = shallow(<NavigationItems />);
});

**nastavovanie props** - umoznuje to shallow, takze pre komponent v premennej wrapper mozem zavolat .setProps({key: value})

**expect metody**
 [https://jestjs.io/docs/en/expect](https://jestjs.io/docs/en/expect) 

**mock metody**
 [https://jestjs.io/docs/en/mock-function-api](https://jestjs.io/docs/en/mock-function-api) 


pri **testovani kontajnerov** je problemom redux store
 -pred class dame **export** - aby sme mali okrem default exportu aj pomenovany(default moze byt obaleny v hoc) 
-v teste potom importujem import { Kontainer } from 'path';

-**pri reduxe** musime dat pozor co testovat - nic komplexne, ziadna velka logika
-testujeme vlastne reducer, tiez ma vlastny test file, **nepotrebujeme enzyme**, testujeme obyc. funkcie, napr.:
import reducer from "./auth";
import * as actionTypes from ".._actions_actionTypes";

describe("auth reducer", () => {
 it("should return initial state", () => {
  expect(reducer(undefined, {})).toEqual({
   token: null,
   userId: null,
   error: null,
   loading: false,
   authRedirectPath: "/"
  });
 });
});





**NEXT JS**
-minimalisticky framework pre servrom rendrovane react aplikacie
ssr - aj kvoli seo
### https://github.com/zeit/next.js/

[https://github.com/zeit/next.js/](https://github.com/zeit/next.js/)


### npm init

npm install --save next react react-dom

nepouzivam create-react-app
do package pridame scripty (nahradime defaultne pre react)
"scripts": {
"dev": "next",
"build": "next build",
"start": "next start"
}

v next.js sa uz nepouziva react-router, ale zlozky a subory

npm run dev -spusti prostredie, localhost:3000, hot reloading, serverside

opat mame fuckcne aj class-based komponenty

**linkovanie** - pouzijem next/link element a odkazujem sa na zlozku s index.js suborom (v tomto pripade auth)
import Link from 'next/link';
<Link href="_auth"><a>Auth<_a></Link>

**routing** - pouzijem next/router a ako path nastavim cestu k zlozke s index.js suborom
import Router from 'next/router';
<button onClick={() => Router.push('_auth')}>Go to auth<_button>

**Zlozky**
-**components** - normalne funkcne komponenty
-**pages** - stranky, index.js, zanorovanie vytvara zaroven strom paths, sem importujeme komponenty

**Styly**
-tiez sa daju pouzit inline styly, radium, css modules,...
-style-jsx
 [https://github.com/zeit/styled-jsx](https://github.com/zeit/styled-jsx) 

<style jsx>{`
...sem pridam styly...
`}

</style>

**Custom error handling**
 [https://github.com/zeit/next.js/#custom-error-handling](https://github.com/zeit/next.js/#custom-error-handling) 
v adresari so strankami vytvorim subor _error.js (override defaultneho error.js, po zapojeni musim restartovat dev prostredie)
import React from "react";
import Link from "next/link";

const errorPage = () => {
 return (
  <div>
   <h1>Oops, something went wrong.</h1>
   <p>
    Try{" "}
    <Link href="/">
     <a>going back</a>
    </Link>
    .
   </p>
  </div>
 );
};

export default errorPage;

**getInitialProps**

 [https://github.com/zeit/next.js/#fetching-data-and-component-lifecycle](https://github.com/zeit/next.js/#fetching-data-and-component-lifecycle) 
_getInitialProps(context)_ - ked chcem asynchronne inicializovat props
**static async getInitialProps**(context) {
console.log(context);
return {};
}
-log sa vypise do terminalu(tam kde bezi server), moze vratit promise

staticasyncgetInitialProps(context) {
console.log(context);
  const promise = new Promise(
  (resolve, reject)=>...resolve({appName: 'apka'})...
  );
return promise;
}

**depoloyment**
npm run build
-potom cely projektovy adresar zavesim na server kde bezi node.js
-tam potom zavolam npm start

**uzitocne linky**
 [https://nextjs.org/docs](https://nextjs.org/docs) 
 [https://nextjs.org/learn](https://nextjs.org/learn) 


**ANIMACIE**
mnoho sposobov:
**1.css transitions:**
.Modal {
 ...
 transition: all 0.3s ease-out;
}
.ModalOpen {
 opacity: 1;
 transform: translateY(0);
}
.ModalClosed {
 opacity: 0;
 transform: translateY(-100%);
}
**2. css animations**
**.ModalOpen {**
**animation: openModal 0.4s ease-out forwards;**
**}**
**@keyframes openModal {**
**0% {**
**opacity: 0;**
**transform: translateY(-100%);**
**}**
**50% {**
**opacity: 1;**
**transform: translateY(-20%);**
**}**
**100% {**
**opacity: 1;**
**transform: translateY(0);**
**}**
**}**

**3. react transition group**
 [https://reactcommunity.org/react-transition-group/](https://reactcommunity.org/react-transition-group/) 
npm install react-transition-group --save

import **Transition** from 'react-transition-group/Transition';

**<Transition**
 **in**={this.state.showBlock}
 **timeout**={1000}
 mountOnEnter  _-> znamena ze na zaciatku prida element do domu_
 unmountOnExit _->znamena ze nakonciodoberie element z domu_
>
 {**state** => (   _-> mozne stavy: entering, entered, exiting, exited_
  <div
   style={{
    backgroundColor: "red",
    width: 200,
    height: 100,
    margin: "auto",
    transition: "opacity 1s ease-out",
    opacity: state === "exiting" ? 0 : 1
   }}
  >
   {state}
  </div>
 )}
**</Transition>**
-ak chcem rozny timing pre zobrazenie a zmiznutie nastavim do timeout objekt: 
**timeout** = {{enter: 400, exit: 1000}}

**Transition events**
onEnter={()=>...}
onEnter
onEntering
onEntered
onExit
onExiting
onExited

-mozem vyuzit napr. ak ma jedna animacia cakat na druhu

**CSSTransition**
-namiesto import Transition a <Transition> pouzijem import **CSSTransitio**n a <CSSTransition>
-rozdiel je, ze v CSSTransition uz nepouzijeme funkcie ale len cisty JSX kod
-pridame property **classNames**="tiredy-pre-zanorene-jsx"

sposob A
1. nastavim className="fade-slide"
2. potom v css mozem vytvorit triedy:
.fade-slide-enter.fade-slide-enter-active .fade-slide-exit.fade-slide-exit-active

(napr. ...-enter {opacity: 0} ...-enter-active {opacity: 1} -> postupne objavenie)

sposobB
-rovno do className vlozim objekt obsahujuci vsetky potrebne css tiredy naviazane na transition lifecycle
-napr.:
classNames={{
 enter: "",
 enterActive: "ModalOpen",
 exit: "",
 exitActive: "ModalClosed",
 appear: "",
 apperarActive: ""
}}

**Animovanielistov**
import **TransitionGroup** from "react-transition-group/TransitionGroup";

-namiesto <ul> pouzijem<TransitionGroup component="ul">
-elementy <li> potom obalim do <CSSTransition classNames="fade" timeout={300}>
-nedefinujem property in
**Alternativy k react-transition-group**
react-motion [https://github.com/chenglou/react-motion](https://github.com/chenglou/react-motion) 
react-move (emuluje real world fyziku) [https://github.com/react-tools/react-move](https://github.com/react-tools/react-move) 
react-router-transition [https://github.com/maisano/react-router-transition](https://github.com/maisano/react-router-transition) 


**Autentikacia**
princip v spa - apka posle auth data na server, server kontroluje validnost a zasle spat token (napr. JWT), spa si ulozi token do local storage, potom s kazdou poziadavkou na server posiela aj token v headeri

-pri autentifikacii netreba redux, staci local state v auth kontajneri

firebase auth rest api (tu si skopirujem entry point pre overenie uzivatela alebo registraciu noveho uzivatela a i.)
 [https://firebase.google.com/docs/reference/rest/auth/](https://firebase.google.com/docs/reference/rest/auth/) 

predpripravim si auth data objekt (tento priklad je pre prihlaenie pomocou emailu a hesla)
const authData = {
 email: email,
 password: password,
 returnSecureToken: true
};

a potom poslem post
axios
 .post(
 "https://www.googleapis.com/identity...?key=AIzaSyAtaUwKffwLVXMfjktUVkHEcdbnKs_MVz8",
 authData
 )
 .then(response => {
  console.log(response);
 })
 .catch(err => {
  console.log(err);
 });

**automaticky logout** - nastavim pomocou klasickeho asynchronneho setTimeout a ako hodnotu v milisekundach mozem pouzit hodnotu zaslanu servrom pri overeni (nieco ako expiration time, vo firebase expiresIn) - treba dat pozor na spravne jednotky, v js je timeout v milisekundach, po uplynuti casu vynulujem ulozeny token a user id
**zmena prav vo firebase**
namiesto nastvenia read a write na true nastavim:
{
 "rules": {
  ".read": "auth!=null",
  ".write": "auth!=null"
 }
}
Pripadne to specifikujem len pre urcity objekt (v tomto pripade objednavky - orders)
{
 “rules”: {
    “ingredients”: {
   “.read”: “true”,
  “.write”: “true”
  },
  “orders”: {
   “.read”:”auth != null”,
   “.write”:”auth != null”
  }
 }
}

**Zaslanie tokenu v post requeste**
musim do post request pridat
…url…?auth=“ + token







**Webpack**

feb 2019 zatial pouzivam v3, v4 ma odlisnu syntax
npm install —save-dev  [webpack@3](mailto:webpack@3) 

-webpack je v prvom rade bundler - concatuje subory
-okrem tohoumoznuje optimalizovat subory (napr. obrazky) a zapajat rozlicne pluginy (loaders), transformovat subory, transpilovat new gen JS do klasickeho JS


￼

fugovanie:
-> entry point (moze ich byt aj viac)
->Loaders (babel-loader/css-loader)
  ->Plugins
   -> output (dist adresar alebo bundle.js) - spravne usporiadany vystup

tutorial - poziadavky:
  next-gen JS, JSX, CSS autoprefix, img imports, optimize code (shrink code)
  -pokial pouzivam git necham ho ignorovat /dist

npm install --save-dev webpack@3 webpack-dev-server

pridame script do package (nazov npm modulu)
"start": "webpack-dev-server"

do projektoveho adresara pridam subor
webpack.config.js

**Loaders**
ako loader pouzijem **babel**
npm install --save-dev babel-loader babel-core babel-preset-react babel-preset-env

aby pouzival presety musime ho konfigurovat
**.babelrc**

{
 "presets": [
 [
  "env",
  {
   "targets": {
    "browsers": ["> 1%", "last 2 versions"]
       }
  }
 ],
 "react"
 ]
}

dalej potrebujem loadnut **css styly**
npm install --save-dev css-loader style-loader
npm install --save-devpostcss-loader
npm install --save-devautoprefixer
pre obrazky
npm install --save-dev file-loader

v mojom pripade bolo potrebne doinstalovat
npm i -D webpack-cli

dalej
npm install --save-dev html-webpack-plugin

# Hooky

-nova feature ,react 16.8
-novy sposob ako pisat react komponenty, daju sa pouzit aj len cisto funkcionalne komponenty, pomocou hookov mozem managovat stav
-ak mam vsetky komponenty funkcionalne, nemusim riesit pripadnu konverziu komponentu na class based
-hooky su extra funkcie/fetaures ktore mozem volat v SFC komponentoch a pridavaju im vlastnosti ktore mali doteraz len class based
-config - traba mat spravnu verziu reactu >= 16.8

**useState**
import { useState } from 'react'
-funkcia ktora ma ako argument initialState a vracia inputState( pole o 2 elementoch - _1. current state_, _2. func._ pre manipulovanie so stavom )

const inputState = useState(''); -v parametri rovno inicializujem stav
inputState[0] - stav
inputState[1] - funkcia (parametrom je novy stav)

const inputChangeHandler = (event) => {
  inputState[1](event.target.value)
}- nastavenie noveho stavu, handler sa zavola na onChange na inpute

cez destructuring (to iste ale namiesto pola mame oddelene konstanty)
const [todoName, setTodoName] = useState('')

jednoduchy priklad s todolistom:
import React, { useState } from 'react';

const todo = props => {
 const [todoName, setTodoName] = useState('');
 
 const [todoList, setTodoList] = useState([]);

 const inputChangeHandler = event => {
  setTodoName(event.target.value);
 };

 const todoAddHandler = () => {
  setTodoList(todoList.concat(todoName));
 };

 return (
  <>
   <input
    type=“text”
    placeholder=“Todo”
    onChange={inputChangeHandler}
    value={todoName}
   />
   <button type=“button” onClick={todoAddHandler}>
    Add
   </button>
   <ul>
    {todoList.map((todo,i) => (
     <li key={I}>{todo}</li>
    ))}
   </ul>
   <p>aaa</p>
  </>
 );
};

export default todo;
-tu boli v podstate pouzite 2 stavy, moze to byt prehladnejsie ale dali by sa zmergovat do jedneho stavu (nie je optimalne)

const [todoState, setTodoState] = useState({
userInput: ‘’,
todoList: []
})
const inputChangeHandler = event => {
setTodoState({
userInput: event.target.value,
todoList: todoState.todoList
})
}

const todoAddHandler = () => {
setTodoState({
userInput: todoState.userInput,
todoList: todoState.todoList.concat(todoState.userInput)
})
}

**Dolezite pravidlo pre vsetky hooky**
* hooky (napr. useState) len v top level komponentovej funkcii(nie zanorene vo funkciach komponenu, ani v if else alebo for loop)

**useEffect**
 Axios alebo akykolvek http dotaz by som nemal volat v sfc len tak, pretoze sa vykona pri rendrovani (cele sfc je v podstate render funkcia), co nema dobry vplyv na vykon, pouzijeme useEffect hook

useEffect ma dva parametre:
	1. Funkcia ktora sa vykonava (bez druheho parametru by sa vykonavala do nekonecna)
	2. Pole premennych, ktorych zmenu sledujeme (funkcia sa teda druhy krat vykona len po zmene tychto premennych)

## Recompose
Kniznica tretej strany/npm modul, sada funkcii a HOC, umoznuje napr. Pouzivat stav a lifecycle vo funkc. komponentoch
### withState
-HOC, funguje takmer ako useState hook
```js
import {withState} from 'recompose'
...
const MyComponent = ({open, setOpen, ...}) => {...}
...
export default withState(‘open’, ‘setOpen’, true)(MyComponent)
```
### branch + renderComponent
-moze posluzit na podmienene rendrovanie
```js
import {branch, renderComponent} from 'recompose'
...
export default branch(
	props => props.items.length === 0, //nejaka podmienka
	renderComponent(Spinner)
)(MyComponent)
```
### lifecycle
-umozni pouzivat hooky z class componentov
```js
import {lifecycle} from 'recompose'
...
export default lifecycle({
	componentDidMount(){
		this.props.funkcia()
	}
})(MyComponent)  
```
### compose
-umoznuje skladat viacero funkcii vracajucich HOC za sebou
```js
import {compose} from 'recompose'
...
const enhance = compose( // nazov konstanty je samozrejme lubovolny
	prvaFunkcia(...),
	druhaFunkcia(...)
) 
export default enhance(MyComponent)
```
### withHandlers (+withState)
-nevytvorim handler funkciu (`<... onClick={()=>HandlerFunkcia()}`) v JSX, pretoze sa vytvara vzdy ked je prerendrovany komponent, namiesto toho pouzijem withHandlers
```js
import {withHandlers, compose, withState} from 'recompose'
...
const MyComponent = ({open, handleClick, ... }) => {
	...
		<... onClick={handleClick}/>
	...
}
...
const enhance = compose(
	withState('open','setOpen', true),
	withHandlers({
		handleClick: props => event => props.setOpen(!props.open)
	})
)
export default enhance(MyComponent)
```
