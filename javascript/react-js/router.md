### Introduction

In this lecture we are going to learn a little bit about routing in react. Until now we have only written one-page applications. Once your application has multiple pages you need to set up a reliable routing system to handle the component or page that should be rendered when navigating to a certain route. For this we are going to use a package called `react-router-dom`.

Let's go through it step by step. First of all, create a new project using `create-react-app` and get rid of the boilerplate code as you already learned in previous lessons. (Just leave the index.js and the App.js files)

Once you've done that, let's create a new file called `Profile.js` inside the src directory, and add a basic functional component to it, which just contains an `h1` element.

```
import React from "react";

const Profile = () => {
  return (
    <div>
      <h1>Hello from Profile</h1>
    </div>
  );
};

export default Profile
```

And also make sure your `App.js` file looks like this:

```
import React from "react";

const App = () => {
  return (
    <div>
      <h1>Hello from App</h1>
    </div>
  );
};

export default App
```

Once you have this let's go and install the package. Open a terminal and run `npm i react-router-dom`. This is going to install all the dependencies of the package.
While this is happening we can already create a new file called `Routes.js`. Routes.js is going to be the file that handles all of our routes.

Once the package is finished installing, you can add this code to your Routes.js file.

```
import React from "react";
import { BrowserRouter, Switch, Route } from "react-router-dom";
import App from "./App";
import Profile from "./Profile";

const Routes = () => {
  return (
    <BrowserRouter>
      <Switch>
        <Route path="/" component={App} />
        <Route path="/profile" component={Profile} />
      </Switch>
    </BrowserRouter>
  );
};

export default Routes;
```

And there is already doing all the magic. So what is happending here? First we are importing React, our two components Profile and App, as well as a couple of things from the package we installed.

1. Route: Those are our routes with a path, which equals the url path, and a component that should be rendered when we navigate to this url.

2. BrowserRouter: Is a router, which uses the history API (pushState, replaceState and the popstate event) to keep your UI in sync with the URL. For completion I have to mention that there are other options than BroswerRouter, but for your current projects you can assume that BrowserRouter is at the root of all your projects.

3. Switch: Renders the first child Route that matches the location. In easier words that means, that the Switch component is going to look through all your Route components and checks their path. The first Route, who's path matches the url will be rendered, all others will be ignored. Important to note is, that in our above example, both routes, the homepage ("/") as well as the profile route ("/profile") contain at first a / in their path. This means, that when you go to "/profile" you will see the App component render, because it is the first path that matches the url. And our profile route get's ignored, because it's second place. Therefore, our Profile component never get's rendered.

Ok, too fast? Let us check this behavior in our Browser for better understanding. However, before we can do that we have to do one more thing. We have to change our `index.js` file, because now of course we don't want our `App.js` file to be the first component to be called when our applications runs. Instead we want our `Routes.js` to be the first one. Your `index.js` should look something like this:

```
import React from "react";
import ReactDOM from "react-dom";
import Routes from "./Routes";

ReactDOM.render(
  <React.StrictMode>
    <Routes />
  </React.StrictMode>,
  document.getElementById("root")
);
```

Once this is done, go ahead and run `npm start` and then check out both routes.
Do you see what we have been talking about? In both cases the "Hello from App" from our App component shows.

Now there are two solutions to this problem.

1. Order your Routes by specificy from top to bottom. Change the order of your two Routes, so that you have the Route with the path equal to "/profile" on top of the homepage. If you check if out in the browser now, you should see the App component on the home route and the Profile component on the "/profile" route. The problem with this solution is, that it might get a lot of work sorting them once your application has a lot of routes.

2. Add the `exact` keyword to your routes. So make them look like this:

```
<Route exact path="/" component={App} />
<Route exact path="/profile" component={Profile} />

```

As you can see this also works. The exact keyword just specifies, that the routes path has to math exactly the url path.

Great now you should have enough basics to get started with react routing. There are a lot more features to react-router-dom, which are extremely useful, but out of the scope of this lesson. Just naming two, the history or match object. Definitifely go and check out more advanced concepts once you are familiar with the basic.

### Assignment

<div class="lesson-content__panel" markdown="1">
1. Go and add a few new routes to the application we created above, playing around with it is the best practice. Maybe delete it completely and rewrite it.
2. Browse a little bit through their [documentation](https://reacttraining.com/react-router/web/guides/quick-start). You don't need to read through all of it, neither understand all of it. Just browse through the concepts we discussed here and re-read them. I'm mentioning it here, because it's a great ressource to refere back to.
3. Watch [this](https://www.youtube.com/watch?v=Law7wfdg_ls&t=850s) video for reviewing. You should be already familiar with those concepts.
</div>

### Additional Resources
