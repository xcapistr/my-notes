[< back](../README.md)
# NODE

-behove prostredie JS, vyuziva **V8** engine (open source, napisany v c++, kompiluje js do strojoveho kodu)

-Node ma **non-blocking IO** - to znamena ze necaka na vysledok kazdej funkcie, napr. dotiahnutie dat z db, ale rovno vola dalsie prikazy, vyhodou je vacsia rychlost, zaroven je jednovlaknovy (**single-thread**, prave tento fakt ma vyvazit non-blocking IO) 


**Co pouzivam**

* **API** - https://nodejs.org/api/index.html

    -definujem ako const nazov = require(‘api’);

* **prettier** for VS

    -code formater, alternativa eslint

    -v atome nefunguje pri chaining syntaxi (typicky jQuery)

* **nodemon** 

    -automaticky restartuje server po zmenach) 

    -`npm install --save-dev nodemon`

    -v package.json pridam do "scripts":  

    `"start": "nodemon server.js"`

    -potom spustam server prikazom npm start
     
* **morgan** - logovanie node JS 
    
    -`npm install --save morgan`

    -do app.js pridam `const morgan = require("morgan")`

    -namiesto "morgan" mozem dat co chcem a potom `app.use(morgan('dev'))`

* **body parser** 

    -`npm install --save body-parser`
     
* **commander** 
    -`npm install --save commander`

    -praca s cmd/terminalovymi prikazmi

    -inspirovane commanderom pre Ruby 

* **heroku**
    -[heroku.com](https://www.heroku.com)

    -cloud application platform

    -dobre pre free deploying, oplati sa vyuzit heroku toolbelt pre pracu v terminali

-----------------------------------------------

### Spajanie retazcov
A.  `Hello ${user.username}!`

B. `('Hello', useer.username)`

C. `'Hello' + user.username`

### Export funkcie
    `module.exports.nazovFunkcie = () => {}`

- objekt module ma atribut exports kam mozem nacpat exporty, potom staci importovat cely JS subor pomocou `require(cesta)`

- alebo najskor definujem funkciu a pak ju zahrniem do exports

    `module.exports = { nazovFunkcie: nazovFunkcie }`

- ak sa nazvy zhoduju staci napisat (od ES6):

    `module.exports = { nazovFunkcie }`

### es6 destructive assignment
```js
var a, b, rest;
[a, b] = [10, 20];

console.log(a);
// expected output: 10

console.log(b);
// expected output: 20

[a, b, ...rest] = [10, 20, 30, 40, 50];

console.log(rest);
// expected output: [30,40,50]

var user = {name: 'Andrew', age: 25};
var {name} = user;
console.log(name);
// vypise sa 'Andrew'
```

### Inicializacia npm
`npm init` - prikaz na inicializaciu npm

### Modul lodash
- v npm je modul lodash - super vec, ulahcenie vyvoja, mnozstvo funkcii, napr. vymazanie duplicit z pola (uniq()), typove kontroly g(isString()),....
- zvykne sa importovat do konstanty `_`
- funkcie mozem prehladavat na [lodash.com](https://lodash.com/)


### Automaticke restartovanie
- po zmene kodu pri vyvoji
- pomocou **nodemon**

    `nodemon subor.js`

    `nodemon subor.js -e js,hbs .` -> ak chcem aby nodemon reagoval aj na vybrane rozsirenia

### Argumenty z terminalu
    `console.log(process.argv)` 

- vypise vektor argumentov, kt. sa zadaju z prikazoveho riadku/terminalu pri spusteni
- resp. premenna `process.argv` obsahuje pole argumentov

#### Modul yargs 
- parsovanie atributov prikazoveho riadku (ak mam napr. `name="Blabla"`, v `process.argv` je zluceny, vdaka modulu yargs mozem vytiahnut hodnotu !!!na windowse nefunguju single quotes, preto vzdy pouzit double)
- instalacia: `npm install yargs@4.7.1 --save`
    - bez save flagu by yargs modul nebol pridany do package.json (uz neplati)

    `yargs.argv` - tu args kniznica uklada verziu argumentov z kt. sa aplikacia spustila, oproti process.argv su tu rozparsovane atributy:
    `--volaco="hodnota"` alebo `--volaco "hodnota"`  => `volaco: hodnota`

#### vytvorenie helpu/napovedy k argv
namiesto `= yargs.argv` pouzijeme:
```js
= yargs
    .options(a:{...}) . // option je to co sa pridava -a
    .command('prikaz', 'popis', {
        argument1: {
            describe: 'popis argumentu',
            demand: true // povinnost/required
            alias: 'b' // pri spustani cez alias sa pouziva notacia s pomlckou: -alias
        })
    .help()
    .alias()
    .argv
```
### JSON konvertovanie
- objekt na string: `JSON.stringify(obj)`
- string na objekt: `JSON.parse(str)`
- rozdiel vo vypise json retazca a objektu: v json retazci su aj atributy v uvodzovkach

- stringify ma v skutocnosti viac argumentov
    1. json objekt
    2. filter, vacsinou ponechavame undefined
    3. pocet medzier pre odsadenie (default 0 a ziadne konce riadkov)

### Zistenie typu
- **typeof**, napr. `console.log(typeof stringObj)`

### Praca zo subormi 
**modul fs**
napr. `fs.writeFileSync(subor, objekt)`, `fs.readFileSync()` ...

### Filtrovanie pola
- vyberie len prvky kde je splnena podmienka
`const pole2 = pole.filter(prvok => podmienka)`

### Rodelenie retazca
* rozseka retazec podla znaku, vznikne pole retazcov

    `retazec.split('znak')`

### Prevod na velke/male znaky
`string.toUpperCase()`
`string.toLowerCase()`

### Debuggovanie 
- **V cmd/terminali:**
    
    `node inspect subor.js`
    
    `list(10)` - zobrazuje naraz 10 riadkov
    
    `n` - next
    
    `c` - continue

    - v debug mode mozem prejst do repl modu prikazom `repl` - vypisovanie a manipulacia s objektami (nevypisujem pomocou logu ale len napisem nazov objektu)

    - zarazka v kode - `debugger;`

    - pri zmene v subore sa debugger resetuje

- **V chrome**

    `node --inspect-brk subor.js`

    alebo `nodemon --inspect ...`

    - v chrome - `chrome://inspect`
    - skry/zobraz konzolu - Esc

### foreach
- pouziva callback
```js
pole.forEach(prvok => {
     console.log(prvok);
});
```

### Callback
- mam funkciu x
- potom definujem funkciu y
```js
y = function(callback){
    ...
    callback();
}
```
- potom mozem dosadit do argumentu `y(x)`

- callback argument je vhodne osetrit (`typeof function`)

- callback funkcia moze byt aj argumentom funkcie sort, ktora radi pole

- tu nadefinujeme rozhodovacie kriterium podla ktoreho funkcia vrati 1 alebo -1

### Rozdiel medzi == a ===
(resp. != a !==)

`==` - abstract equality

`===` - strict equiality

napr.:

`3 == '3'` -> `true`

`3 === '3'` -> `false`

### Prikaz clear
- v terminali vycisti vystup
- na osX lepsie: cmd + K 

### arrow funkcie
- prisli s ES6
- mozu mat **statement syntax** `=> {...}` (klasicky sled prikazov) alebo **expression syntax** `=> expression` (vyraz suvisiaci s parametrom funkcie (spracuva ho), nepotrebuje return, vzdy vracia hodnotu vyrazu)
- arrow funkcie nebinduju klucove slovo **this** - nemozeme sa teda odkazovat na atributy a metody objektu, pri funkcii v objekte pouzijeme zapis (nove v ES6):
```js
objekt = {
    atribut: 'hodnota',
    funkcia () {
        console.log(this.atribut);
        // da sa tu narozdiel od arrow fkce pouzit napr.: console.log(arguments);
        // arrow funkcia by chapala arguments globalne
     }
}
```

### Timeout
`setTimeout(callbackFunkcia, milisekundy)`

### ASYNC princip
- **Call Stack** - cast V8, jednoducha datova struktura ktora sleduje vykonavanie programu vnutri V8 (doslova zasobnik volani) - na spodku zasobniku je main()
- asynchronny program pouziva okrem Call Stack aj:
    - **Node APIs** - sem putuju asynchronne volania, napr. setTimeout
    - **Callback Queue** - sem idu funkcie ked sa vykonaju v Node APIs, napr. Timeout na 2000 ms
    - **Event Loop** - ak je Call Stack prazdny, Event Loop don prihodi procesy z Callback Queue

### HTTP Request
`npm install request --save`

`request(options object, callback function)`

`request({url: 'url', json: true}, (error, response, body) => {})`

- lepsie je pouzivat kniznicu napr. Axios

### Kodovanie URI
- node ma na to funkcie `encodeURI` a `decodeURI`, resp. `encodeURIComponent` a `decodeURIComponent `

### ES6 promises
- kedysi len pomocou kniznic, teraz sucastou ES6

- promsie je instanciou objektu `= new Promise((resolve, reject)=>{});`
- zabranuje sa naprilklad zavolani callback funkcie 2x za sebou
- mozu byt zretazene, za kazde volanie promise funkcie mozme dodat dalsie:

    `.then((result)=>{}, (errorMessage)=>{})`

- alebo dame error message az do bloku catch

    `.then((result)=>{}).then((result)=>{}).catch((errorMessage)=>{})`

- aj kniznicu ktora nema vstavane promises(napr. request) mozme zabalit do promisy

* **stavy promisy:**
    - resolve - bol naplneny
    - reject - bol zamietnuty
    - pending - caka na vyhodnotenie
    - settled - bol vyhodnoteny (prva alebo druha moznost)

- vyvojari pouzivaju radi promisy namiesto tradicnych callback funkcii aj kvoli moznosti zretazenia, kod sa lahsie udrziava
`... .then((response)=>{}).then((response)=>{}).then((response)=>{}).catch((e)=>{})`

### Axios
- promise based HTTP client for the browser and node.js

    `npm install axios --save`

- umoznuje napr. **get** - `axios.get(url).then((reponse) => {}).catch((e) => {})`

### Vyhodenie vlastnej chyby
`throw new Error('Unable to find that address.')`

### Express
- npm kniznica / framework podporujuci tvorbu webservra / API

    `npm install express save`

- zaklad je naimportovat const express, vytvorit premennu `app=express()`
- potom volame funkciu 

    ```js
    app.get.('/', (req, res) => {
         res.send(volaco);
    });
    ```

- pre spustenie aplikacie na porte volame `app.listen(cisloportu)`

- basic express apka:

    ```js
    const express = require('express');

    var app = express();

    app.get('/', (req, res) => {
    res.send('Hello world!');
    });

    app.listen(3000);
    ```

- potom spustime node server.js

### HTML subory na webservri
- davam do priecinka *./public* 
- potom su dostupne priamo na **localhost:port/subor.html**
- ale v hlavnom js subore musi byt:

    `app.use(express.static(__dirname + '/public'))`

- `app.use()` -> registracia akehokolvek middleware

### Handlebars
- sablonovaci system pre node.js, presnejsie view engin (existuju aj dalsie - pug, ejs...)

    `npm install hbs --save`

- node aplikacii musime nastavit view engine s ktorym chceme pracovat 

    `app.set('view engine', 'hbs')`

- subory maju koncovku **.hbs**
- rendrujeme v `app.get()`, tak ze namiesto `res.app` (pozri Express) pouzijeme `res.render(subor.hbs, {par1: hodnota, par2:hodnota})`
- bindovanie pomocou `{{ }}`
- vsetky hodnoty ktore chcem bindovat musim do hbs suboru poslat v ramci jedneho objektu pri volani `res.render(..., {})`

#### Partials
- znovapouzitelne casti sablony

    `hbs.registerPartials(__dirname + '/views/partials');`

- injektovanie parial elementu v hbs subore `{{> footer}}`

- ak chcem injektovat v partials nejaku funkciu definovanu v js subore

    `hbs.registerHelper('nazovFunkcie', () => {});`
- v hbs injektujem pomocou {{}}
- ak mala funkcia aj atribut injektujem ako: `<p>{{nazovFunkcie atributFunkcie}}</p>`

#### Middleware
- middleware kontrola aplikacie pomocou `next`
- v js subore

    ```js
    app.use((req, res, next) => {
        next();
    });
    ```

- pokial sa nezavola `next()`, zvysok aplikacie neprebehne

    ```js
    app.use((req, res, next) => {
        res.render('stranka.hbs');
        next();
    });
    ```

### Testovanie - Mocha
- testovaci framework **Mocha** - [mocha.org](https://mochajs.org)

    `npm install mocha --save-dev`

    -ulozi len pre vyvojove prostredie, kde potrebujeme testy

- `it('',()=>{})` -definuje novy case

- do *package.json* pridame skript test

    `"test":"mocha **/*.test.js"`

    -testuje vsetky subory ktorych nazvy koncia zvolenym retazcom

- vyhodenie chyby
    
    `throw new Error(`Popis chyby`)`

- spustenie testovacieho skriptu pomocou nodemon

    `nodemon --exec "npm test"`

    -alebo pridame skript, napr. `test-watch` do *package.json*
    
    `"test-watch": "nodemon --exec \"npm test\""`
    
    -potom spustime `npm run test-watch`

### Testovanie - Assertions - expect
- **expect** je kniznica pre tvrdenia/argumenty (assertions) - [link](https://github.com/mjackson/expect)

    `npm install expect@1.20.2 --save-dev`

- pozor - nekompatibilita verzie 1.20 s novsimi

```js
const expect = require('expect');
expect(res).toBe(33);
```

- vyhoda 
    - mam to aj s chybovymi hlaskami
    - retazenie - expect(res).toBe(33).toBeA('number');
- zakladne assertions
    - toBe (neda sa pouzit na porovnanie celych objektov a poli)
    - toNotBe
    - toBeA
    - toEquals
    - toInclude
    - toExclude

- celkovo moze teda test vyzerat nasledovne

    ```js
    it('Should add two numbers', () => {
    var res = utils.add(11, 22);
    expect(res).toBe(33).toBeA('number');
    });
    ```

- pre objekt napr.:

    ```js
    expect({name:"Bla", age:20}).toInclude({age:20});
    ```

- pokial testujeme asynchronnu metodu, musime pridat argument done a v tele funkcie zavolat `done();`

    ```js
    it('Should ...', (done) => {
         utils.asyncAdd(4, 3, (sum) => {
             expect(sum).toBe(8).toBeA('number');
             done();
         });

    });
    ```

- pozor!!! - hranica pre uspesny test je 2000ms

### Testovanie - Express - supertest
- pre vacsiu efektivitu sa pouziva kniznica supertest - od tvorcov expressu (a skombinovat s assertions)

    `npm i supertest@2.0.0 --save-dev`

- v *server.js*, na konci exportovat app

    `module.exports.app = app;`

- v testfile : 
    ```js
    const request = require('supertest');
    var app = require('./server').app;

    it('Should return hello world response', (done) => {
         request(app)
            .get('/')  // url
            .expect(200) // status code
            .expect('Hello world!') // response
            .end(done) // wraping
    })
    ```

- tak isto mozme pouzit expect

    ```js
    const request = require('supertest');
    const expect = require('expect');

    var app = require('./server').app;

    it('Should not return hello world response', (done) => {
        request(app)
            .get('/')
            .expect(404)
            .expect((res)=>{
                expect(res.body).toInclude({
                    error: 'Page not found'
                });
            })
            .end(done);
    })
    ```

#### Organizovanie testov pomocou describe()
- pochadza z mocha

    `describe('Nazov skupiny', () => { ...sem dam metody ...});`

#### Test Spies

- overenie ci bolo zavolane spy


    ```js
    it('Should call the spy correctly', () => {
       var spy = expect.createSpy();
       spy();
       expect(spy).toHaveBeenCalled();
    });
    ```

#### simulovanie/mockovanie db objektov
- rewire

    `npm install rewire@2.5.2 --save-dev`

### MONGO DB

- treba stiahnut mongo server pre prislusnu platformu
premenujem na mongo

- niekde mimo vytvorim priecinok mongo-data

- dolezite subory v mongo/bin:

    - *mongod* - nastartuje mongo server

    - *mongo* - umozni pripojenie k servru a pouzivanie prikazov

- spustenie:

    `./mongod --dbpath ~/adresar s datami`

- verzia:
    
    `mongod --version`

- mal by som vidiet port, napr.  27017

- potom v inom terminali spustime `./mongo`

- potom mozem spustat prikazy:

    - vlozenie

        `db.Todos.insert({js objekt})`

    - vypisat zaznamy

        `db.Todos.find()`

**Robomongo - GUI**

- na zaciatku dam create, jedine co staci zadat je nazov db spojenia

- noSQL
    - nie tabulky ale kolekcie
    - nie zaznamy/riadky ale document
    - nie stlpce ale polia v dokumente(rozne properties vramci kolekcie)

- spojenie node JS a mongo db - npm modul node mongodb native
    
    `npm install mongodb --save`

- pripojenie k db:
    ```js
    const MongoClient = require('mongodb').MongoClient;

    MongoClient.connect('mongodb://localhost:27017/TodoApp', (err, db) => {
      if (err) {
        return console.log('Unable to connect mongo db server');
      }
      console.log('Connected to mongo db server');

      db.close();
    });
    ```

**3.verzia mongoDB modulu s vlozenim zaznamu do kolekcie**

```js
const MongoClient = require('mongodb').MongoClient;

MongoClient.connect('mongodb://localhost:27017/TodoApp', (err, client) => {
  if (err) {
    return console.log('Unable to connect mongo db server');
  }
  console.log('Connected to mongo db server');
  const db = client.db('TodoApp');

  db.collection('Todos').insertOne({
    text: 'Something to do',
    completed: false
  }, (err, result) => {
    if (err) {
      return console.log('Unable to insert todo', err);
    }
    console.log(JSON.stringify(result.ops, undefined, 2));
  });

  client.close();
}); 
```

- ID objektu
    - timestamp je zakodovany v _id objektu ak nespecifikujem vlastne _id
mongo nenecha vlozit zaznam s uz existujucim _id

    - z _id mozem vytiahnut timestamp pomocou funkcie getTimestamp()
result.ops[0]._id.getTimestamp()

    - ak chcem vytvorit ozajstny id objekt importujem najskor do konstanty ObjectID z mongodb

        `const {MongoClient, ObjectId} = require('mongodb');`
    - potom vytvorim `var obj = new ObjectID();`

**Object destructuring (uz spominane es6 feature)**
```js
var user = {name: 'Andrew', age: 25};
var {name} = user;
console.log(name);
// vypise sa 'Andrew'
```

- mozme to vyuzit pri importe z kniznice

    `const {MongoClient, ObjectID} = require('mongodb');`

- `.find()` vlastne vracia cursor

**Praca s mongodb api**

- do pola - `.find().toArray()`
```js
db.collection('Todos').find({parametre}).toArray().then((docs) => {
    ...
}, (err) => {
    ...
});
```
- API: http://mongodb.github.io/node-mongodb-native/2.0/api/index.html
- napr. metoda count: http://mongodb.github.io/node-mongodb-native/2.0/api/Collection.html#count
- dalsie:
    - deleteMany
    - deleteOne
    - findOneAndDelete (vyhoda ze vramci result mam cely mazany objekt)
    - findOneAndUpdate(filter, update, options, callback)

- pri update pouzivame operatory, napr. `$set: {}, $int: {}`

**Mongoose ORM**
https://mongoosejs.com/

- instalacia cez npm
- import: `var mongoose = require('mongoose');`
- treba pridat promisy(defaultne asi obsahuje len callbacky)

    `mongoose.Promise = global.Promise;`


- pripojenie k db:

    `mongoose.connect('mongodb://localhost:27017/Todoapp');`


- vytvorenie datoveho modelu

    ```js
    varTodo = mongoose.model('Todo', {
        text: {
            type:String
        },
        completed: {
            type:Boolean
        },
        completedAt: {
            type:Number
        }
    });
    ```

- priprava noveho dokumentu

    ```js
    var newTodo = new Todo({
        text: 'cook dinner'
    });
    ```

- ulozenie

    ```js
    newTodo.save().then(
        doc => {
            console.log('Saving todo...', doc);
        },
        e => {
            console.log('Unable to save todo');
        }
    );
    ```

- mongoose validatory
    https://mongoosejs.com/docs/validation.html

    type - obmedzenie typu, required - povinnost, trim - oreze medzery pred a za v texte, default- preddefinovana hodnota

- mongoose schemy
    https://mongoosejs.com/docs/guide.html

###express js

`npm i express@4.14.0 body-parser@1.15.2 --save`

**minimal server app**

*server.js*

```js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

- spustenie: `node app.js`

- spustenie s automatickym reloadom `nodemon app.js` 

    (vyzaduje instalacu npm baliku nodemon)

**body parser**

```js
var bodyParser = require('body-parser');
app.use(bodyParser.json());
```

### Importovanie bootstrap kniznice

`import 'bootstrap/dist/css/bootstrap.css'`


### Praca s databazou postgres

https://node-postgres.com/features/connecting

https://knexjs.org/