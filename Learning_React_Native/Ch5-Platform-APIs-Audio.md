# Learning React Native
## Chapter 5: Platform APIs
### Video 1: Using AsynchStorage
- Thus far, we've created a small app that allows us to add and visualize colors. It seems to be working just fine for us, but when we reload the application, we lose all the colors that we've added. So we need to save these colors in some sort of persistent storage. And that's exactly what AsyncStorage allows us to do. This means it allows us to save data locally on our phone. Let's go ahead and implement AsyncStorage. Now I'm in the hooks.js file because the values that I want to save are our colors. And we already have a useColors hook, that manages all of the functionality that has to do with colors. So, first, let's go ahead and import AsyncStorage from react-native. Next, we're going to need a function that we can use to load colors asynchronously, so we'll create an async function. We'll create a constant called colorData, this is where we will save any data that we load from AsyncStorage. We can load data from AsyncStorage using the getItem function. Notice that I used the await keyword. Don't forget to do that because AsyncStorage getItem returns a promise and we need to wait for it to resolve before we actually have any color data. Then we'll need to provide the key for the data we want to load. In this case, we'll create a key called ColorListStore:Colors. Now maybe this is the very first time that you've loaded the app, and you don't actually have any color data, so we're going to to check for that using an if statement. So if you do have color data, colorData is going to come back as a string, so I'm going to use JSON.parse to convert that string into an array of colors. Then we'll go ahead and replace our colors and state using setColors. Now we have a function that we can use at any point to load colors from AsyncStorage. The next thing that we're going to add is some functionality for saving colors. I want this useEffect hooked to fire any time our colors array changes. If there's a change to the color array, I'm going to go ahead and save that change by calling AsyncStorage setItem. I'll use the same key that we've added to the getItem call within our loadColors function, the ColorListStore:Colors key. And we're going to save this data as a string, so I'm going to JSON.stringify our array of colors. So this useEffect hook is going to be invoked any time the colors array changes, and it will save the new data from that array to AsyncStorage. The other thing I need to do is initially load our color data. This useEffect hook will have an empty dependency array, meaning that it will only get invoked once, after the initial render. Now if we already have colors, meaning that there are already some items in this colors array, we're going to go ahead and break out of this hook by invoking a return. Next, we're going to go ahead and invoke loadColors, which calls our colors to initially load. So now we'll go ahead and test this out. It looks like I have an error, so I need to make sure that I also import useEffect. There we go, so let's try adding a color. Every time we added a color, the colors array changed, so this useEffect hook should have fired and saved our colors. When we reload our application, we still see blue, green, and red. This is because the first time any component invokes this hook, this useEffect hook will fire and load our initial colors. There you have it, asynchronous storage. We're now saving and loading colors locally on the device.

### Video 2: React Navigation
- React Navigation is a library that we can use to add functionality that allows our users to navigate from screen to screen. In our case, we want to add a details screen for each color. So if the user selects a color, we can go to a screen and see details about that color. In this lesson, we're going to have to move some code around, and we're going to have to create some files. So the first thing I'm going to do is open up the App.js file, and I'm going to copy all of the contents of this file to the clipboard. Now with all of our app code on the clipboard, I'm going to go over to the components folder, and I'll click this icon, which will allow me to add a new file. Let's create a file called ColorList.js. And inside of our color list, I will paste the contents of the App.js file. We'll make a couple small changes, like naming this component the ColorList instead of the App, and also, we are on the same folder as the ColorButton and the ColorForm, so I'll remove this reference to the components folder. So that gives us a ColorList component. Let's come over to our files and also add a new file for ColorDetails. Now, inside of our ColorDetails file, just to prevent us from having to type a whole bunch of code, I'm also going to paste all of the code from our app. And then we'll come into our code and prune out all of the stuff we don't want. So first, that's any reference to other components or hooks. We also want to use the View and the Text component, so we don't need the FlatList or the StyleSheet. We're also not going to have states, so let's get rid of the useState import. And I'll name the component ColorDetails, remove any state functionality, and then essentially remove all of the children that are being rendered. So I'm going to go ahead and create a view where I will use the same container styles that we previously had. And within that view, I'm just going to go ahead and add some text to let us know that we're on the Color Details screen. I also want to make sure we see that text in the center of the screen, so we're going to make sure that we align the items to the center, and we justify the content to the center. So now we have a ColorList component and a ColorDetails component. And we need to switch between these components when the user selects to color. So let's go into our now empty App.js file and start to write the new app component. Now, if we go take a look at the documentation at reactnavigation.org, we can get to the docs by clicking on Read docs. And if we take a look at Getting started, we see that we need to install React Navigation using npm, but we also additionally have to install all of these other dependencies. Here's a little Expo install coming in for all of the other dependencies that we're going to need to make React Native work. I'll go ahead and click this copy button and copy those dependencies to the clipboard. And then I'm going to come over to the terminal window. This is where our application is actually running. And I'm going to go ahead and stop it. So I'm going to go ahead and install @react-navigation/native and @react-navigation/stack. Those are the two main npms that we're going to be using. I also have to install all of those related dependencies. I have that command saved to my clipboard, so I'll go ahead and paste it in here and run it. And now we're installing all of the related dependencies as well. Now that we have React Navigation, let's go ahead and use it. So let's go over to the App.js file. And we're going to import React. We're going to import the ColorList component. We're going to import the ColorDetails component. We're also going to import something called the Navigation Container from React Navigation. And finally, the specific navigator that we're going to create is called the Stack Navigator. So we need to import the Stack Navigator function from react-navigation/stack. Down here on line seven, I'm going to declare two components, the Navigator and the Stack. And we obtain those components by destructuring our call to createStackNavigator. And whenever we use React Navigation, the first thing that we need to do is wrap everything within the NavigationContainer. Within the NavigationContainer, I can add the Stack Navigator. So this is going to be the actual thing that flips between screen. I didn't catch myself, and I accidentally imported the Navigator and the Stack. What I needed to import is a Screen. I'm going to use the Screen component to create the Details screen. And we'll give this screen a name of Details, and we'll tell it to use the ColorDetails component. So right now, we've added a stack navigator, and it only has one screen. It also looks like I spelled ColorDetails wrong. Let me go ahead and fix that in my import statement. So let's go ahead and save this code and run our application. And when we run our app, it looks like we have an error. Okay, so the module hooks isn't available within the ColorList. So the ColorList component is now within the components folder, so I need to make sure that I am importing the hooks from the correct place, which is actually one folder up from the components folder. And I also deleted the style sheet when I shouldn't have. So let me go over to the ColorDetails component and also import the style sheet because, yep, we did leave a style sheet there. So our stack navigator is currently working, but we only see one screen, the Color Details screen that we created. Within the stack navigator, all we need to do is render a screen for every screen that we want to use. So I'm going to go ahead and render a home screen. Now, the order matters here. Because the home screen is listed first, we're going to see it first. There we go. So saving this code, we can see that our home screen is now our ColorList.

### Video 3: Navigating Between Screens
- In the last lesson we created our stack of screens using react navigation, specifically, the stack navigator. And we have a home screen that renders the color list and a detailed screen that renders the color details. What we want to do is navigate between these screens when the user clicks on any other color buttons. But before we get started, I want to do a little bit of cleanup. First, you'll notice that the title for the first screen is home. That's because it defaults back to our name property, but we can change that. We can add options to our screen component and rename the title, colorless. This form looks a little funky now that we have a react navigation header rendered above it. So let me come into the color form and we're going to do some CSS cleanup. Let's get rid of the margin, and then let's also make sure that the background color for the form is white. Here we go. And now let's come back over to the color list. So within the color list component, I'm actually going to get rid of the background color. When we click the button, we're going to navigate to the details page, not change the background. So I'll get rid of the on press functionality within the color button as well. So the initial screen that's rendered is the home screen. So that's where our navigation is going to begin. Now, when this screen component is rendered, it actually passes more properties to our color list component. So because the color list component is rendered by the screen component, it also has some additional prompts available. So there's a navigation object that's passed to props, and I'm going to go ahead and grab it within my color list component. And I could use this navigation object to navigate between screens. So down here in our flat list component, when we render each of these color buttons, I'm going to go ahead and add an on press property to each color button. And when the user presses the color button, I'm going to call navigation.navigate, and I want to go to the details screen. So I'm going to go ahead and reference that screen by name. So now if we go try it out. So what you'll see is when I click on green, I'm over on the color detail screen and react navigation has already added the functionality to get back to the previous color lists, but I don't just want to navigate to the color detail screen, I want to pass some information, specifically the color that the user just clicked on. Then the second argument of the navigate function, we can add a parameters object. And within this object, we can add any data that we want to pass along with the navigation request. In this case, I want to pass the name of the color, and that name is available to us within this render item function as item.color. So now I'll come into the color details component. This component is also rendered by a screen component. So some react navigation properties are also passed to this component. The one that I want to use is called the route, route.params points to the parameters object that we sent along with the request. So I can grab route.params.color and that should be the name of the color button that was clicked on. When I come in here and click on green, we see that we need to render color details for the green color. So to recap, our stack navigator renders a list of screens and then passes properties to each of those screens that we can use to control navigation. From our color list component, we navigate to the color details component using navigation.navigate. And we also pass the color and the parameters object. The color details page can capture that color using the route property, specifically, route.params.color.

### Video 4: Color Details Screen
- So far we're successfully navigating between the list and details screens. In this next lesson, we're actually going to render some pretty specific details about each color to the user. So the first thing we want to do is display that color as the background of the screen. So on line five, I'm going to go ahead and destructure the color from route params and then on line seven, I'm going to ahead and add a style array where I overwrite the background color with the color that was passed along with the route params. Alright, so we can see the color in the background. But the other thing that we want to do, is we want to display all sorts of information about this color. To do that, I'm going to use an npm package called Color. So let me open up the terminal and we'll install the color package. Next let's import a variable called color from our color package. I'm going to rename this color parameter name 'cause that's what it is, the color name, and then I can use this name to get an instance of the color object. So all we have to do is send it the color name, and then we're going to get this object that we can do all sorts of stuff with. For instance, we can display information about our colors. Now we'll use color.rgb to return an object that gives us the red, green, and blue values for our color. I'll convert that object to a string so we can render it in this text element. We can also get the HSL values for our color, and I will convert those to a string. There's all sorts of information about the color that can be obtained using this color package. In this last text component, we'll use color.luminosity to give us the lightness value of the color. Luminosity is a number that tells us how light or dark our colors are. Colors that are darker, have a luminosity value that's closer to zero, and colors that are light or bright will have a luminosity value that's closer to one. So let me create a variable up here called text color so that we can display the text using the inverse of the color that was sent. color.negate will return the inverse color, and then we will convert it to a string, and then use it in each of these text elements. Whoops, I forgot to define my variable was a constant. So I'll go ahead and add the same text color to all the other text elements, and I'm also going to take my little text color style and add a font size of 30 so that we can see this information. We can come in and test our app, and we can also see specific information about our red or blue colors. So to recap, the color details component receives the color as a route param, we're using that color to create an instance of a powerful color object using a special npm package called Color, and we can use this color object to give us all sorts of information about the color. Thus far we've created a pretty cool application. We can add colors to our application, and then we can click on those colors to see specific details about each color. This is going to be the last iteration that we take on this application for this course. But I challenge you to continue to make iterations on this application, by adding features that you might find useful.

Video 4: Fetching Data
- Whenever you're building React Native applications you're going to need to be able to get information from the internet and send information to it. To demonstrate this we're going to build a new application that gets some information from the internet. And I'm going to build this application using the Expo Snack tool. So remember Expo Snack is a pretty powerful InBrowser IDE that we can use to get started building React Native applications. So whenever you create a new Snack, Expo gives you some default code. You might notice some fun things in this code. Like you can get constants from Expo Constants and then we can use that value within our CSS to get the exact status bar height for each device. So I'm going to come in here to the center of this code and I'm going to delete our default app component. We're going to go ahead and create our own component and our component is going to be a function. So as you can see as I make changes Expo Snack is already giving us a preview which is an error because our app isn't returning anything. So I'm going to use a special type of view called the safe area view. And our safe area view is just like a regular view, it just makes sure that it renders the view in the main area of our screen. Now within the safe area view I'm going to add a scroll view. This view also works like a regular view but it can hold a lot of content which you can scroll to by dragging the screen. I'm going to go ahead and add a text component that renders a to-do message to load a pet and this'll probably be a good time to tell you about the application that we're building. This is going to be a random pet generator. We're going to load the name and image of a pet randomly from the internet and display it on the screen. So in order to do that we're going to need a state variable where we can save the pet. So using the useState hook I can create our pet variable as well as our function for setting the pet variable. Now if we don't have a pet I'm just going to return null. Returning null doesn't cause an error it just doesn't render anything. Remember the useEffect hook takes in a function and a dependency array. If I leave this dependency array empty this useEffect hook will be invoked once after my component is initially rendered. So I'll just go ahead and make a call to a function called loadPet. Because loading data from the internet requires an asynchronous request it's just easiest to make the loadPet function an async function. So where can I fetch information from? We're going to use a little API called the pet library. So at pet-library.moonhighway.com/api/randompet and when you make a request of this route you're going to get a JSON object that contains details about a pet at random. So that's what we want to do, we want to load this object within our application. So I'm going to go ahead and copy the URL and that's the route that we're going to use for our fetch request. Once we receive a response we can parse the data in that response as JSON simply by awaiting a res.json request. Let's test it out. I'm going to go ahead and just log the data to the console. Then I'm going to test this on an iOS device using Expo Snack. Expo Snack gives us the ability to see our console logs by clicking on this little Editor menu and then turning the Panel on. So within the Panel we get to see errors and logs. And if I scroll down here you can actually see our application has rendered and we've logged a pet to the console. So we don't want to log this pet to the console, we actually want to use it. I'm going to go ahead and call set pet with the data that was returned from our fetch request. So for right now I'm just going to go ahead and render the pet name and the pet category. And I can also render a picture of the pet into an image component. So to do this I'm going to send the image source, a URI of pet.photo.full to use the full size image. Let me create some styles for this image and also I need to import the image component. Doing so we see our application refresh and we see a picture of Luna the dog. So in this application we made a fetch request to an API which gave us some random information about a pet. We then rendered that information on the screen including that pet's name, their category as well as the full sized image of that pet.

Video 5: Using RefreshControl
- So far, we broke ground on a new application using Expo Snack. This application loads information about a pet at random from the internet, and then renders that information on the screen. Right now our application is loading and displaying only one pet. In this lesson, we want to give our users the ability to refetch another pet. So let's get started. The first thing I'm going to do, is import an activity indicator to handle our latency a little bit better. So in the initial render, instead of rendering just no, I'm going to go ahead and render an activity indicator. So our users see a loading animation, which lets them know that we're requesting the initial data. Instead of using a button, we want users to be able to load another pet, simply by dragging the screen down. So I'm also going to include a component called refresh control. This component will indicate when we drag the screen down, and it will also display a loading animation while we are making our requests. So because we're dealing with latency and requests, I need to create a state variable for loading. And whenever we start to load a pet, I'm going to set the loading variable to true, and then once we have successfully loaded a pet, I will set the loading variable to false. So now we have a variable that lets us know the loading state of our request. So I'm going to use that variable in my refresh control, the refresh control is rendered as a property of our scroll view. And this property will point to a refresh control component. The refreshing property of this component, is whether or not we're loading or making the data requests. And when the user triggers a refresh by dragging down on the screen, we want to invoke the load pet function. So now every time I drag down on the scroll view, I'm loading a new pet, we see a little spinner which lets us know that we're making an asynchronous request for each of these pets. Now, for users on a slow 3G network, I want to make sure that I load not only the request, but the full pet image file, before rendering the new pet within our component. And I can do that, by going to our load pet function and on line 21, awaiting an image prefetch. So this line tells JavaScript to pause, once the full image has completely loaded, we'll set the pet data, and we'll set the loading value to false. So now, when we refetch each pet, we'll make sure the full image is loaded before rendering a new pet component. We built this random pet generator pretty quickly using Expo Snack. It shows us that we can load data into our applications by making fetch requests.