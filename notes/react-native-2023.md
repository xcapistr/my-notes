[< back](../README.md)

# REACT NATIVE 2023

React bez react-dom je vlstne **platform-agnostic**
React-native je teda alternativa k react-domu (kolekcia specialnych react komponentov, kompilovane do nativnych ui elementov + native platform APIs exosed to JS)

React komponenty sa kompiluju do nativnych
Logika sa nekompiluje, bezi na JS threade

#### Sposoby vyvoja: Expo CLI vs React Native CLI

(da sa switchnut pocas vyvoja)

##### expo

- 3rd party service, free
- managed app development, pohodlnejsie

##### rn

- by RN team a community
- bare-bone development, viac setupu, nie tak pohodlne features ako expo
- lepsia integracia s nativnym kodom (pokial ho potrebujem miesat)

#### Vytovrenie projektu

expo cli

(`npm u -g expo-cli`)

`expo init <nazov projektu>`

- centralny point `App.js`
- configuracia `app.json`

spustenie

- `npm run start`

reset expo cache

- `expo r -c`

#### Styles

- rn styles != css, zalozene na css ale ide len o subset features a properties

- View element pouziva flexbox by default, direction je column

* Styly nie su v kaskade (napr. Text nepodedi farbu z View)

##### rozdielny styling iOS a Android

- borderRadius na Text elemente - funguje len na Androide

- ScrollView property alwaysBounceVertical/alwaysBounceHorizontal = true/false funguje len na iOS

- celkovy background sa nastavuje v configu app.json:

```json
    "expo" : {
        ...
        "backgroundColor" : "#1e085a"
    }
```

- tiene:

  - android: elevation
  - ios: shaddow..., funguje len ked mam aj backgroundColor

```js
    elevation: 5,
    shadowColor: 'black',
    shadowOffset: { width: 0, height: 4},
    shadowRadius: 6,
    shadowOpacity: 0.25
```

- linear gradient: https://docs.expo.dev/versions/latest/sdk/linear-gradient/

#### Casto pouzivane komponenty:

- View

- Text

  - ked menim text, nemusim pouzit `onChange` event listener ale `onChangeText`, atributom handler funkcie bude novy text
  - moze obsahovat dalsi `<Text>`, styly sa dedia

- ScrollView

  - rendruje vsetko naraz (aj elementy ktore nie su na obrazovke), hodi sa na obmedene velky content

- FlatList

  - rendruje len visible elements, hodi sa ked rendrujem list
  - dolezite props: **data** (mali by to byt pole objektov s key property, pripadne pouzijeme keyExtractor), **renderItem** (atributom funkcie je objekt {item, index, separators}), **keyExtractor** (item, index) => ...

- SafeAreaView - okraje pre iOS zariadenia

- Pressable - novsia nahrada za Touchable, TouchableHighlight, TouchableOpacity, TouchableNativeFeedback...

  ```jsx
  <Pressable
    onPress={onDelete.bind(this, index)}
    android_ripple={{ color: '#339' }}
    style={({pressed})=> pressed && styles.pressedItem }
  >
  ```

  ```jsx
  <Pressable
    style={({ pressed }) =>
      pressed ? [styles.pressable, styles.pressed] : styles.pressable
    }
    onPress={handlePress}
    android_ripple={{ color: '#640233' }}
  >
    <Text style={styles.text}>{children}</Text>
  </Pressable>
  ```

- StatusBar (from 'expo-status-bar'): `<StatusBar style='light'/>`

- Image:

  - importujem pomocou required
    `<Image source={require('../assets/image.png')} />`

  - alebo z webu (musim nastavit width a height)
  `<Image source={{ uri: imageUrl }} />`

- ImageBackground:

  ```jsx
  <ImageBackground
    style={styles.background}
    source={require('./assets/images/background.png')}
    resizeMode="cover"
    imageStyle={styles.backgroundImage}
    blurRadius={5}
  >
    <StartGameScreen />
  </ImageBackground>
  ```

- LinearGradient: `import { LinearGradient } from 'expo-linear-gradient'`

  `<LinearGradient colors={['#4e0329', '#ddb52f']} style={styles.container}>...`

- KeyboardAvoidingView: behaviour prop = 'padding' | 'height' | 'position'
  ```jsx
  <KeyboardAvoidingView
      behavior={Platform.OS === "ios" ? "padding" : "height"}
      style={styles.container}
  >
  ```

##### Icons

https://docs.expo.dev/guides/icons/

https://icons.expo.fyi/

`import { Ionicons } from '@expo/vector-icons'`

```jsx
<Ionicons name="md-remove" size={24} color="white" />
```

##### Fonts

- kym sa fonty nenacitaju, zobrazim loading screen

`npx expo install expo-font`
`npx expo install expo-app-loading`

- v root componente:

```js
import { useFonts } from 'expo-font'
import AppLoading from 'expo-app-loading'
```

```jsx
const [fontsLoaded] = useFonts({
    'open-sans': require('./assets/fonts/OpenSans-Regular.ttf'),
    'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf')
})

if (!fontsLoaded) {
    return <AppLoading />
}
...
```

- pouzitie v styleSheets

```js
...
container: {
    fontFamily: 'open-sans-bold',
},
...
```

https://docs.expo.dev/versions/v47.0.0/sdk/font/

#### API

- Alert API: parametre title, description, buttons

  ```js
  Alert.alert('Wrong value', 'Choose a number between 1 and 99', [
    {
      text: 'Okay',
      style: 'destructive',
      onPress: resetInputHandler
    }
  ])
  ```

- Dimensions API: `import { Dimensions } from 'react-native'`

  - objekt z ktoreho ziskavam udaje

  ```js
  Dimensions.get('window').height/.width/.scale/.fontScale
  ```

  ```js
  ...
  const deviceWidth = Dimensions.get('window').width

  const styles = StyleSheet.create({
  imageWrapper: {
      width: deviceWidth < 420 ? 250 : 400,
      height: deviceWidth < 420 ? 250 : 400,
  ...
  ```

  - alebo pouzijem **hook** (lepsie, aby sa refreshovalo pri zmene orientacie)

  `import { useWindowDimensions } from 'react-native`

  `const { width, height, scale, fontScale } = useWindowDimensions()`

- Platform API: platformy android, ios, macos, web, windows
  `import { Platform } from 'react-native'`
  `Platform.OS === "ios" ? ....`

  - alternativa select:
    `borderWidth: Platform.select({ios: 0, android: 2})`

  - dalsia alternativa, filenames (pridam k nazvu platformu, importy budu fungovat automaticky, plati pre vsetky subory. nielen komponenty)
    `Title.android.js`
    `Title.ios.js`

#### DEV Tools & Debugging

- console.log vidim v terminali

- **dev menu** na testovacich devices otvorim zadanim **'m'** do terminalu (pri spustenom expo), alebo **'cmd+D'** na iOS emulatore a **'cmd+M'** na android emulatore

- react dev tools: `npm i -g react-devtools`, spusta sa commandom `react-devtools`

#### Navigacia

https://reactnavigation.org/docs/getting-started

` npm install @react-navigation/native`
`npx expo install react-native-screens react-native-safe-area-context`
`npx expo install @react-navigation/native-stack`

- native-stack vs stack - native pouziva nativne platform elementy, stack len emuluje (horsi vykon)

```jsx
import { NavigationContainer } from '@react-navigation/native'
import { createNativeStackNavigator } from '@react-navigation/native-stack'

const Stack = createNativeStackNavigator()
...
export default function App() {
    ...
    return (
        <Stack.Navigator
          screenOptions={{
            headerStyle: { backgroundColor: '#351401' },
            headerTintColor: 'white',
            contentStyle: { backgroundColor: '#3f2f25' }
          }}
        >
          <Stack.Screen
            name="MealsCategories"
            component={CategoriesScreen}
            options={{
              title: 'All Categories'
            }}
          />
          <Stack.Screen name="MealsOverview" component={MealsOverviewScreen} />
        </Stack.Navigator>
    )
```

pouzitie: 

- kazda zaregistrovana screen ma prop **navigation** (druhy parameter je lubovolny objekt), tiez ma **route** prop (tu najdeme params)

    `navigation.navigate('ScreenName',  { categoryId: itemData.item.id })`

    `route.params`

- useNavigation hook (obodobne useRoute) `import { useNavigation } from '@react-navigation/native'`

    `const navigation = useNavigation()`


- dynamicke options:

    1.  v Stack.Screen:
        ```jsx
        <Stack.Screen ... 
            options={
                ({route, navigation}) => ({
                    title: route.params.categoryTitle,
                    headerRight: () => <Button title="Test" />
                })
            }
        > ....
        ```
    2. v screen componente pomocou navigation.setOptions
    (musi byt v useEffect, este lepsie v useLayoutEffect)

        ```jsx
        useLayoutEffect(() => {
            navigation.setOptions({ 
                title: categoryTitle,
                headerRight: () => <Button title="Test" /> 
            })
        }, [categoryId, navigation])
        ```

- Drawer navigation: https://reactnavigation.org/docs/drawer-navigator

#### State management
##### Context

- v samostatnom subore si vytvorim context provider

```jsx
import { createContext, useState } from 'react'

export const FavoritesContext = createContext({
  ids: [],
  addFavorite: id => {},
  removeFavorite: id => {}
})

function FavoritesContextProvider({ children }) {
  const [favoriteMealIds, setFavoriteMealIds] = useState([])

  function addFavorite(id) {
    console.log('addFav', id);
    setFavoriteMealIds(prev => [...prev, id])
  }

  function removeFavorite(id) {
    console.log('removeFav', id);
    setFavoriteMealIds(prev => prev.filter(mealId => mealId !== id))
  }

  const value = {
    ids: favoriteMealIds,
    addFavorite: addFavorite,
    removeFavorite: removeFavorite
  }
  return <FavoritesContext.Provider value={value}>{children}</FavoritesContext.Provider>
}

export default FavoritesContextProvider
```

- obalim komponenty v App.js

```jsx
...
import FavoritesContextProvider from './store/context/favorites-context'
...
    <FavoritesContextProvider>
    ...
    </FavoritesContextProvider>
```

- pouzitie pomocou useContext hooku
```jsx
const favoriteMealCtx = useContext(FavoritesContext)
favoriteMealCtx.addFavorite(mealId)
```



