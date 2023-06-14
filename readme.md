# React Navigation
### Installation
```
npm install @react-navigation/native
```
Then also run following command
```
npx expo install react-native-screens react-native-safe-area-context
```
Wrapping your app in ”NavigationContainer”
Now, we need to wrap the whole app in NavigationContainer. Usually you'd do this in your entry file, such as index.js or App.js:
```
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';

export default function App() {
  return (
<NavigationContainer>{
/* Rest of your app code */
}</NavigationContainer>
  );
}
```
🔑 Note: 
In a typical React Native app, the NavigationContainer should be only used once in your app at the root. You shouldn't nest multiple NavigationContainers unless you have a specific use case for them.

# Stack Navigator
Now we need to understand which type of navigation we want to use in the app, like Drawer, tab, stack navigation etc.
To use stack navigation we use to add following dependency.
```
npm install @react-navigation/native-stack
```
### Creating a native stack navigator
`createNativeStackNavigator` is a function that returns an object containing 2 properties: `Screen` and `Navigator`. Both of them are React components used for configuring the navigator. 
NavigationContainer is a component which manages our navigation tree and contains the navigation state. This component must wrap all navigators structure. Usually, we'd render this component at the root of our app, which is usually the component exported from App.js.
```
import { createNativeStackNavigator } from '@react-navigation/native-stack';
```
```
const Stack = createNativeStackNavigator();
```
```
<NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
</NavigationContainer>
```
### Complete code for simple navigation of one screen
```
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

### Now consider you have two sceen and you have to move between and have one initial route
Making multiple route
```
<NavigationContainer>
      <Stack.Navigator initialRouteName='Home'>
        <Stack.Screen options={{title:"My Home Screen"}} name="Home" component={HomeScreen} />
        <Stack.Screen name='Details' component={DetailsScreen} />
      </Stack.Navigator>
</NavigationContainer>
```
Navigation on button click
don’t forget to pass {navigation} prop.
```
function DetailsScreen({navigation}) {
```
On button
```
<Button 
      title='Go to Details' 
      onPress={
        ()=>navigation.navigate("Details")
      }>
</Button>
```
### Complete code
```
import { StatusBar } from 'expo-status-bar';
import { Button, StyleSheet, Text, View } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen({navigation}) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button 
      title='Go to Details' 
      onPress={
        ()=>navigation.navigate("Details")
      }>
      </Button>
    </View>
  );
}

function DetailsScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName='Home'>
        <Stack.Screen options={{title:"My Home Screen"}} name="Home" component={HomeScreen} />
        <Stack.Screen name='Details' component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
### navigation.navigate(routeName, params): 
This function is used to navigate to a specific screen in your app. It takes two parameters:
params (optional): An object containing any additional parameters or data you want to pass to the target screen.
The navigation.navigate function performs a stack-based navigation, meaning it adds the target screen to the navigation stack. If the screen is not yet in the stack, it will be pushed onto the stack, and if it is already in the stack, it will navigate to the existing instance of that screen means it don’t push same instance again. It also provides a back button to return to the previous screen.
### navigation.push(routeName, params): 
This function is similar to navigation.navigate, but it always adds a new instance of the target screen to the navigation stack, even if it is already present. It can be useful in cases where you want to create multiple instances of the same screen, such as in a chat application where each chat screen represents a separate conversation.
### navigation.popToTop(): 
This function allows you to navigate back to the first screen in the stack, effectively resetting the navigation stack to its initial state. It removes all screens from the stack except for the first screen. This can be useful when you want to provide a way for the user to quickly navigate back to the main or home screen of your app.

### navigation.goBack()
The navigation.goBack() function is used to navigate back to the previous screen in the navigation stack. It is commonly used when you want to provide a way for the user to go back to the previous screen or when you want to programmatically trigger a back navigation.
When you call navigation.goBack(), it will pop the current screen from the navigation stack and transition to the previous screen.
### Passing Data Between Screens in navigation

Firstly, create a stack navigator with some routes and navigate between those routes.
Pass params to a route by putting them in an object as a second parameter to the navigation.navigate function: 
```
navigation.navigate('RouteName', { /* params go here */ })
```
e.g.
```
onPress={() => navigation.navigate('enrollmentScreen', {name:"Ali", reg:85, course:"JavaScript", paid:"$50"})}
```
Now to access this data on next screen
Read the params in your screen component: `route.params`.
```
export default function EnrollmentHistoryScreen({ route, navigation }) {
    console.log(route.params);
```
Use it on screen
```
export default function EnrollmentHistoryScreen({ route, navigation }) {
    console.log(route.params);
    return (
        <View style={{margin:28}}>
            <Text style={{ fontSize: 30 }}>Enrollment Details</Text>
            <Text style={{ fontSize: 20 }}>Name : {route.params.name}</Text>
            <Text style={{ fontSize: 20 }}>Reg # : {route.params.reg}</Text>
            <Text style={{ fontSize: 20 }}>Course : {route.params.course}</Text>
            <Text style={{ fontSize: 20 }}>Paid : {route.params.paid}</Text>

        </View>
    );
}
```
Initial params
You can pass some initial params to a screen. If you didn't specify any params when navigating to this screen, the initial params will be used. They are also shallow merged with any params that you pass. Initial params can be specified with an initialParams prop:
```
<Stack.Screen
name="Details"
component={DetailsScreen}
initialParams={{ itemId: 42 }}
/>
```
Accessing it
```
function HomeScreen({ route }) {
  const { itemId } = route.params;

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Text>Initial param: itemId - {itemId}</Text>
    </View>
  );
}
```
### Updating params
```
navigation.setParams({
  query: 'someText',
});
```
```
import { View, Text, Button } from "react-native";

export default function EnrollmentHistoryScreen({ route, navigation }) {
    console.log(route.params);
    return (
        <View style={{ margin: 28 }}>
            // . . . other code
            <Text style={{ fontSize: 20 }}>Paid : {route.params.paid}</Text>
            <Text style={{ fontSize: 20 }}>Payment Method : {route.params.paymentMethod}</Text>

            <Button title="Clear Data" onPress={() => {
                navigation.setParams({
                    paymentMethod: "none"
                })
            }}></Button>
        </View>
    );
}
```
### Passing Params to previous screen
Instead of goBack use navigate then you should be able to pass data to previous screen.
# Configure Header
A Screen component accepts options prop which is either an object or a function that returns an object, that contains various configuration options
```
return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{ title: 'My home' }} <-- this here
      />
    </Stack.Navigator>
  );
```
More configuration
```
options={{
        // --- changing Title of Header
        title: 'Home', 
        // --- Adjusting Header Style
        headerStyle: {
          backgroundColor: 'lightblue',
        }, 
        // --- Adding button on header
        headerRight: () => (
          <FontAwesome
            style={{ paddingRight: 20 }}
            name='graduation-cap' size={25}
            // passing data to next screen
            onPress={() => navigation.navigate('enrollmentScreen', {name:"Ali", reg:85, course:"JavaScript", paid:"$50", paymentMethod:"Visa"})}
            color="black"
          />
        ),
      }}
```

### Using params in the title
**Step 1:**
```
onPress={() =>
          navigation.navigate('Profile', { name: 'Custom profile header' }) // <-- 1 See here
        }
```
**Step 2:**
```
 <Stack.Screen
          name="Profile"
          component={ProfileScreen}
          options={({ route }) => ({ title: route.params.name })} // <-- 2. see here
        />
```
### Complete code:
```
import * as React from 'react';
import { View, Text, Button } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Profile"
        onPress={() =>
          navigation.navigate('Profile', { name: 'Custom profile header' }) // <-- 1 See here
        }
      />
    </View>
  );
}

function ProfileScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Profile screen</Text>
      <Button title="Go back" onPress={() => navigation.goBack()} />
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen
          name="Home"
          component={HomeScreen}
          options={{ title: 'My home' }}
        />
        <Stack.Screen
          name="Profile"
          component={ProfileScreen}
          options={({ route }) => ({ title: route.params.name })} // <-- 2. see here
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```
![](https://github.com/muhammadnaqeeb/React-Native-Navigation/blob/main/images/001.png)

# Nested Navigation
Installing all dependency at once
```
npm install @react-navigation/native
```
Then also run following commands
```
npx expo install react-native-screens react-native-safe-area-context
```
```
npm install @react-navigation/native-stack
```
```
npm install @react-navigation/bottom-tabs
```
```
npm install @react-navigation/drawer
```
```
npx expo install react-native-gesture-handler react-native-reanimated
```

## 1st Stack Navigation
```
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
```
```
const Stack = createNativeStackNavigator();
```

**Wrap app with NavigationContainer**
Make two more component
```
function Login() {
  return (
    <View style={styles.container}>
      <Text>Login</Text>
    </View>
  );
}
```
```
function SignUp() {
  return (
    <View style={styles.container}>
      <Text>SignUp</Text>
    </View>
  );
}
```
Now App function
```
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name='login' component={Login} />
        <Stack.Screen name='signup' component={SignUp} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
This will make Login as initial route.
Now to move between Screens
We need to pass navigation props in functional component that we make.
```
function Login({navigation}) {
  return (
    <View style={styles.container}>
      <Text>Login</Text>
      <Button title='Need an account? signup' onPress={
        ()=>navigation.navigate("signup")
      }></Button>
    </View>
  );
}

function SignUp({navigation}) {
  return (
    <View style={styles.container}>
      <Text>SignUp</Text>
      <Button title='Need to login' onPress={
        ()=>navigation.navigate("login")
      }></Button>
    </View>
  );
}
```

## 2nd Tab Navigation
```
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
```
```
const BottomTab = createBottomTabNavigator();
```
Now create components for bottom tab
```
function Home(){
  return(
   <View style={styles.container}>
    <Text>Home Page</Text>
   </View>
  );
}
function Details(){
  return(
   <View style={styles.container}>
    <Text>Details</Text>
   </View>
  );
}
```

Now Making a different component for Tab navigation
```
function Tab(){
  return(
    <BottomTab.Navigator>
      <BottomTab.Screen name='Home' component={Home} />
      <BottomTab.Screen name='Details' component={Details} />
    </BottomTab.Navigator>
  );
}
```
Now this Tab() is also a component so we can bring it in to Stack Navigator
```
<Stack.Screen name='Tab' component={Tab}/> 
```
Now when you run the app you see two Header so remove any Header you want
```
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator screenOptions={{headerShown:false}}>
        <Stack.Screen name='Tab' component={Tab}/> 
        <Stack.Screen name='login' component={Login} />
        <Stack.Screen name='signup' component={SignUp} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

Now you can move any where just by
```
<Button title='Logout' onPress={
        ()=>navigation.navigate("login")
}></Button>
```
```
<Button title='Need an account? signup' onPress={
        ()=>navigation.navigate("Tab")
      }></Button>
```
## 3rd Drawer
Add following line in babel.config.js
```
module.exports = function(api) {
  api.cache(true);
  return {
    presets: ['babel-preset-expo'],
    plugin:['react-native-reanimated/plugin'] // <-- this here
  };
};
```
Then restart the app with --clear (in terminal)
```
npm start --clear
```
```
import { createDrawerNavigator } from '@react-navigation/drawer';
```
```
const Drawer = createDrawerNavigator();
```

Create component to show
```
function Profile(){
  return(
   <View style={styles.container}>
    <Text>Profile</Text>
   </View>
  );
}
```
Create Drawer function for navigation
```
function Drawer() {
  return (
    <DrawerNav.Navigator>
      <DrawerNav.Screen name='Profile' component={Profile} />
      {/* can have more Drawer */}
    </DrawerNav.Navigator>
  );
}
```
Attach Drawer where you want to attach
```
function Tab() {
  return (
    <BottomTab.Navigator>
      <BottomTab.Screen name='Home' component={Drawer} />
      <BottomTab.Screen name='Details' component={Details} />
    </BottomTab.Navigator>
  );
}
```

If it give reanimated error go to:

https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/installation/

After adding drawer run app using following commands
```
npx expo start --clear
```




