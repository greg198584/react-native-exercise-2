# React native - Exercises - 2

## Exercice 4 : Utilisation des States et des Props

### Objectif

Dans cet exercice, nous allons introduire les concepts de `State` et `Props` en React Native. Les states sont des données qui peuvent changer et affecter le rendu de l'application. Les props sont des paramètres que l'on peut passer aux composants enfants.

### Les states

### Instructions

1. **Ajout d'un State à l'écran d'accueil**

Dans React (et donc React Native), un state est une sorte de donnée qui peut être modifiée au fil du temps et qui peut affecter le rendu du composant. C'est l'une des façons de gérer les données dynamiques dans un composant.

La fonction `useState` est un hook de React qui vous permet de déclarer un state dans votre composant. 
C'est une fonction qui prend un argument, qui est la valeur initiale de votre state, et renvoie un tableau de deux éléments.

Dans `const [count, setCount] = useState(0);`, l'argument de `useState` est `0`, ce qui signifie que `count` aura une valeur initiale de `0`.

Le tableau renvoyé par `useState` contient deux éléments :
- Le premier est la valeur actuelle du state (`count` dans cet exemple).
- Le deuxième est une fonction qui permet de modifier ce state (`setCount` dans cet exemple).

Voici comment cela fonctionne :

1. `count` est la valeur actuelle du state. Vous pouvez l'utiliser dans votre code JSX pour afficher la valeur actuelle.

2. `setCount` est une fonction qui vous permet de changer la valeur de `count`. Par exemple, si vous voulez augmenter `count` de 1, vous pouvez appeler `setCount(count + 1)`.

C'est une façon de gérer les données locales à un composant. Chaque fois que le state est modifié (par exemple via `setCount`), le composant sera re-rendu avec la nouvelle valeur.

Nous allons d'abord ajouter un state dans notre écran d'accueil `HomeScreen`. Ce state nous permettra de compter le nombre de fois que l'utilisateur a visité cette page.

   Ajoutez le code suivant dans `HomeScreen` :

   ```jsx
   import React, { useState } from 'react';
   import { View, Text, Image, StyleSheet, Button } from 'react-native';

   const HomeScreen = () => {
       const [count, setCount] = useState(0);

       return (
           <View style={styles.container}>
               <Text style={styles.title}>Bienvenue à la maison !</Text>
               <Image
                   style={styles.image}
                   source={require('../assets/home.png')}
               />
               <Text style={styles.text}>Ceci est une démonstration de React Native.</Text>
               <Text style={styles.text}>Nombre de visites : {count}</Text>
               <Button title="Visiter" onPress={() => setCount(count + 1)} />
           </View>
       );
   };
   // Rest of the code...
   ```

   Le state `count` est initialisé à 0 et chaque fois que le bouton "Visiter" est pressé, nous augmentons `count` de 1. Le nombre de visites est affiché dans le texte en dessous de l'image.

### Les Props

1. **Introduction aux props**

   Créons maintenant un nouveau composant `ProfileButton` qui prendra un prop `onPress`.

   ```bash
   > $ mkdir components
   > $ cd components && touch ProfileButton.js
   ```
   
   ProfilButton.js:

   ```jsx
   import React from 'react';
   import { Button } from 'react-native';

   const ProfileButton = ({ onPress }) => (
       <Button title="Voir le profil" onPress={onPress} />
   );
   
   export default ProfileButton;
   ```

2. Ensuite, ajoutez `ProfileButton` à votre `HomeScreen` et passez une fonction au prop `onPress`.

   ```jsx
   import ProfileButton from '../components/ProfileButton';
   
   // ... autres imports ...
   
   const HomeScreen = ({ navigation }) => { // add navigation props
       const [count, setCount] = useState(0);
   
       return (
           <View style={styles.container}>
               {/* ... autres composants ... */}
               <ProfileButton onPress={() => navigation.navigate('Profile')} /> // add profilButton
           </View>
       );
   };
   ```

   Ici, nous avons passé une fonction qui navigue vers le `ProfileScreen` lorsque le bouton est pressé.


3. **Testez votre application.**

   Lancez votre application et vous devriez être capable de naviguer vers l'écran de profil en appuyant sur le bouton "Voir le profil".


   Dans React Navigation, l'objet `navigation` est un prop qui est transmis à tous les écrans qui sont dans un navigateur. Il contient diverses méthodes et informations qui permettent de naviguer entre les différents écrans.

   Dans ce cas précis, `{ navigation }` est une déstructuration de l'objet `props`. 
   
   Cela signifie que nous extrayons directement la propriété `navigation` de l'objet `props` qui est transmis à `HomeScreen`.

   Voici quelques méthodes utiles disponibles dans l'objet `navigation` :

- `navigation.navigate('RouteName')` : Cette méthode permet de naviguer vers une route spécifique dans le navigateur. Dans notre cas, nous utilisons cette méthode pour naviguer vers l'écran 'Profile'.

- `navigation.goBack()` : Cette méthode permet de revenir à l'écran précédent dans la pile de navigation.

- `navigation.push('RouteName')` : Cette méthode permet d'ajouter une nouvelle route à la pile de navigation, même si cette route est déjà dans la pile.

- `navigation.popToTop()` : Cette méthode permet de revenir à la première route de la pile de navigation, en supprimant toutes les autres routes.

## Exercice 5 : Introduction au Contexte en React Native

### Objectif

Le but de cet exercice est de partager des données entre différents composants en utilisant le Contexte en React Native. Le Contexte fournit un moyen de partager des valeurs entre différents composants, sans avoir à passer explicitement des props à chaque niveau de l'arborescence des composants.

### Instructions

1. **Créer un contexte**

   Ajoutez le code suivant dans `App.js` pour créer le contexte:

   ```jsx
   import React, { useState, createContext } from 'react';
   
   // ... autres imports ...
   
   export const CountContext = createContext();
   
   // ... reste du code ...
   ```

2. **Utiliser le contexte dans `App.js`**

   Modifiez ensuite `App.js` pour utiliser le contexte comme suit :

   ```jsx
   const App = () => {
       const [count, setCount] = useState(0);

       return (
           <CountContext.Provider value={{ count, setCount }}>
               <NavigationContainer>
                   <Tab.Navigator>
                       <Tab.Screen name="Home" component={HomeScreen} />
                       <Tab.Screen name="Profile" component={ProfileScreen} />
                   </Tab.Navigator>
               </NavigationContainer>
           </CountContext.Provider>
       );
   }
   ```

3. **Utiliser le contexte dans `HomeScreen` et `ProfileScreen`**

   Enfin, nous allons utiliser le contexte dans nos écrans `HomeScreen` et `ProfileScreen`.

   Modifiez `HomeScreen` comme suit :

   ```jsx
   import React, {useContext} from 'react';
   import { CountContext } from '../App';

   // ... autres imports ...
   
   const HomeScreen = ({ navigation }) => {
       const { count, setCount } = useContext(CountContext);

       // ... reste du code ...
   };
   ```

   De la même façon, utilisez le contexte dans `ProfileScreen` pour afficher le nombre de visites :

   ```jsx
   import React, {useContext} from 'react';
   import { CountContext } from '../App';
   
   // ... autres imports ...
   
   const ProfileScreen = () => {
       const { count } = useContext(CountContext);
        
       return (
            <View style={styles.container}>
                // ... reste du code ...
                <Text style={styles.text}>Nombre de visites à la page d'accueil : {count}</Text> // ADD VISITE
            </View>
        );
   };
   ```

   Maintenant, le `HomeScreen` et le `ProfileScreen` ont tous les deux accès au nombre de visites de la page d'accueil, et le `HomeScreen` peut augmenter ce nombre. 

   Le nombre de visites est partagé entre ces deux écrans grâce au contexte.