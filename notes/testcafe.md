[< back](../README.md)
# TESTCAFE
![](../img/cafe.jpeg)
### Instalacia
**globalne**
`npm i -g testcafe`

**lokalne**
`npm i --save-dev testcafe`

### Spustenie testov
`testcafe chrome <zlozka>`

* headless mode (bez prehliadaca)

  `testcafe chrome:headless <zlozka>`

### Basic file structure
.ts or .js file
```js
import { Selector } from 'testcafe'

fixture `Cafe example`
    .page `localhost:8080`

test('First test', async t => {
    ...
});
```

### Usefull functions
- typeText('#input-id', 'text')
- click('#element-id')
- expect(premenna)
- eql(hodnota, msg, options)
- contains(hodnota, msg, options)
- find('selektor')
- withText('text')
- withAttribute('atribut')
- parent()

### Selecting elements
**Selector()** - umoznuje vybrat element zo stranky (ako querySelector v js)
```js
import { Selector } from 'testcafe'`

const el = await Selector('.content-class').find('h1')
const text = await el.innerText
```

dalsie priklady:
```js
const windowsRadioButton  = Selector('.radio-button').withText('Windows')
const selectedRadioButton = Selector('.radio-button').withAttribute('selected')
const buttonWrapper = Selector('.article-content').find('#share-button').parent()
```

### Asertions
**expect()** - ako parameter mozem pouzit premennu, selector alebo aj promisu (testcafe pocka na odpoved)

priklad:
```js
test('Check property of element', async t => {
   const developerNameInput = Selector('#developer-name');

   await t
       .expect(developerNameInput.value).eql('', 'input is empty')
       .typeText(developerNameInput, 'Peter Parker')
       .expect(developerNameInput.value).contains('Peter', 'input contains text "Peter"');
});   
```

- e2e testy su async, zmeny sa logicky nedeju ihned ani ich cas zalezi na mnohych faktoroch, neda sa predvidat
- preto je potreba mat na niektore akcie extra timeout co ale spomaluje testy
- Testcafe ma preto **smart assertion query mechanizmus** 
    - spusta sa pokial ako atribut expect funkcie pouzijem DOM selektor alebo promisu
    - ak tvrdenie neprejde, test hned spadne
    - potom sa test opakuje, vzdy s aktualnou hodnotou az kym neprejde alebo nevyprsi timeout (default 3000ms)
    ![](../img/query-mechanism.png)

**options**
- timeout: number
- allowUnawaitedPromise: boolean

priklad:
```js
await t.expect(Selector('#elementId').innerText).eql('text', 'check element text', { timeout: 500 });
await t.expect(doSomethingAsync()).ok('check that a promise is returned', { allowUnawaitedPromise: true });
```

### Integracia testov v GitLabe
2 moznosti:
  - pouzit TestCafe docker image
  - instalovat testcafe v projektovom docker image 

viac info:
  [https://devexpress.github.io/testcafe/documentation/continuous-integration/gitlab.html](https://devexpress.github.io/testcafe/documentation/continuous-integration/gitlab.html)


### Ukazkovy testfile
*qdp.test.ts*

```ts
import { Selector } from 'testcafe'; // first import testcafe selectors

fixture `gutefrage question detail page` // declare the fixture
  .page `https://www.gutefrage.net/frage/powerpoint-animationen-gleichzeitig`;  // specify the start page


//then create a test and place your code there
test('Question author must be michael170', async t => {
  const menuButton = Selector('button.MenuButton[data-ref=menu-button]', { timeout: 30000 });
  await t
    .click(menuButton)

    // Use the assertion to check if the actual header text is equal to the expected one
    .expect(
      Selector('li.ContentMeta-contextMenuLabel:not(.Link) a.Link').innerText
    ).eql('michael170');
});
```
