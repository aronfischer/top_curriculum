## Handle inputs and render lists

Now before we actually go any further, let's do a quick check of how well you understand the topic. Following I will describe the quick assignment, for those who want to try it themselves. I encourage everyone to at least try it yourself, if you've done the reading from the last lectures, you should be able to to it. And if not, you can always come back to this section if you get stuck, because we will discuss the assignment in detail together. Don't be demotivated if you can't solve it, it includes some tricky parts that we haven't actually discussed so far.

Before you start, make sure you understood the concept of `state` and `props` from the previous lessons.

### Learning Outcomes

- Understanding how to render lists in React
- Understanding how to handle input fields and forms in React.

### Assignment

### DIY Guide

Our application will be made of two components, Tasks.js and Display.js. Your application should render an input field and a submit button. With the submit button you can add the content from your input to a task array managed in state. (So we will use class components, because we haven't learned about hooks yet). Your application should render a html list element for each element in the task array and display it on the screen.

1. Run `npx create-react-app my-first-react-app`, `cd` into your project and open it. You can delete everything in the return statement of the App component and just return an empty `div`. You can also delete all the boilerplate React provides and just leave `index.js` and `App.js` in the src directory. Just make sure to clean up the import statements and the `serviceWorker` in the two remaining files.
   If you are overwhelmed with all those files, consider redoing Lecture 01.
2. Create a `components` folder in your `src` directory, which includes a files calles `Overview.js`. `Overview.js` should just render all of our tasks, while `App.js` is going to handle the input field with all the logic.
3. Go implement it yourself. You can do it. For styling use Bootstrap or another CSS Framework if you're familiar with one, or leave it out. Styling isn't really the point of this lecture.
4. Quick tip: Use the Javascript function `maps` to map over your tasks array. You will need to provide a unique key to each item. And there is a difference between handling input fields in plain Javascript or in React. Try figuring it out for your self, but don't worry we will go over it in detail here as well.

### Detailed Guid

1. Let's get started! First run `npx create-react-app task-app` in your terminal, and open the project in your text editor.

2. Delete all files in the src directory and just leave `index.js` and `App.js`.

3. Open your `App.js` file in your `src` directory and make sure it looks like this.

```
// App.js

import React, {Component} from "react";

class App extends Component {
  return (
    <div>
    </div>
  );
}

export default App;

```

4. Make sure to clean the `index.js` as well. It should look something similar to this:

```
// Index.js

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

4. For this tutorial we will also use Bootstrap, to make our application look a little bit nicer. For those who don't know how Bootstrap works, in short. It is a CSS Framework, that helps us style our html easily. You add the styling through classnames. So if you're not familiar with it, just ignore all the `className="someClassName"` type of code. Anyways for now let's include it in our code. Get the bootstrap CDN from their website [here](https://getbootstrap.com/docs/4.3/getting-started/introduction/). Just copy the link element under the CSS section, it should be the first. Then go to your `public` folder in your `task-app` and open the `index.html` file. Ignore the code in there for now, just paste the link you just copied **above** the `title` element and save the changes.

5. There we go, now go back to your `src` directory and create a new folder called components with one file in it. You can name the file `Overview.js`. This and our `App.js` file will be our main parts of the project. In `Overview.js` we will just display all our tasks, while the App component in `App.js` will contain all the logic and manage state. Don't forget to capitalize the names of your components. It doesn't change their functionality, but it is widely seen as a "best practice" to do so.

6. So finally let's write some code. First of all, in our `App.js` file, our class component should look like this.

```
// App.js

import React, { Component } from "react";

class App extends Component {
  constructor() {
    super();

    this.state = {
      task: "",
      tasks: []
    };

  }

  render() {
    return (
      <div className='col-6 mx-auto mt-5'>
        <form>
          <div className='form-group'>
            <label htmlFor='taskInput'>Enter task</label>
            <input
              type='text'
              id='taskInput'
              className='form-control'
            />
          </div>
          <div className='form-group'>
            <button type="submit" className='btn btn-primary'>
              Add Task
            </button>
          </div>
        </form>
      </div>
    );
  }
}

export default App;

```

We mainly just created the skeleton of our component. First we imported React and Component from react, then we initialized the constructor. In the constructor we defined state with

```
this.state = {
    task: "",
    tasks: []
}
```

We assigned `task` to an empty string, this will be the state handling what we type in our input field. And `tasks` will initially be set to an empty array. Later we include all our tasks here.
After that we render a form element, with an `input` and a `button` element. As I said, don't worry about all the `className="form-group"` ect. attributes. Just copy past them if you don't want to learn Bootstrap. What they do is just style our html. For example the `className="btn-primary"` that we provided to our `button` element insures that out button will be displayed blue.

Ok so far so goo, let's have a look at our application. Run `npm start` in your terminal to see what we already got. You should now see an input field with a label and a submit button. When you click the button nothing happens, the page only refreshes.

Let's add some functionality to it. Go back to your `App.js` component and add the following two function. Make sure to add those functions between your constructor and the render method.

```
   handleChange = e => {
     this.setState({
       task: e.target.value
     });
   };

   onSubmitTask = e => {
     e.preventDefault();
     this.setState({
       tasks: this.state.tasks.concat(this.state.task),
       task: ""
     });
   };
```

So far so good, but without calling those function nothing will change in our application. So let's call them. The `handleChange` function will be out `onChange` handler for our input field. It just set's the current `task` in state to whatever we type in our input field. The `onSubmitTask` function instead, will be our `onSubmit` handler for our `form` element. The `onSubmit` handler of the form should be invoked by a click on our button.

In the `onSubmitTask` function we first call `e.preventDefault()` because we don't want the default behavior of refreshing the form anytime we submit the form. After that we modify state.

The following line does the magic.

```
tasks: this.state.tasks.concat(this.state.task),
```

It adds the task (whatever is in our input field by the time we submit the form) to our `tasks` array. Later we can map over this array to display all the tasks we submitted. Make sure that you **DON'T** directly assign state. That is also the reason we don't use the `push` method here. It would give us an error.
After that we just set our current task in state to an empty string, because we want our input field to be empty, to be able to add another task.

So far we haven't invoked those functions yet, so let's do that.
In your `App.js` component in your render method add an onChange handler to your input element like so:

```
<input
    onChange={this.handleChange}
    value={this.state.task}
    type='text'
    id='taskInput'
    className='form-control'
  />
```

Notice that we also have to specify the `value` attribute for React input elements. In this case we want the value of the input field to be what we saved in our `task` state.
And also add the `onSubmitTask` function to our form element like so:

```
<form onSubmit={this.onSubmitTask}>
    // Leave all your code. Just add the onSubmit handler to the form element, or as an onClick handler to the submit button, as you prefere
</form>
```

If you add an onSubmit handler to the form, your button must be of `type="submit"`, otherwise it won't work. Alternatively you can add an onClick event to the button which alles the `onSubmitTask` function

Great, if you run your application now with `npm start` (or just refresh the browser if you have it still running), you will still see no changes, except that the page doesn't refresh when you submit something. That's because we haven't actually displayed anything yet. Let's do that now.

Go to your `Overview.js` file in the components folder and add the following code:

```
// Overview.js

import React from "react";

const Overview = (props) => {
  const {tasks} = props

  return (
    <ul>
      {tasks.map(task => {
        return <li key={task}>{task}</li>;
      })}
    </ul>
  );
};

export default Overview;

```

It just takes the `tasks` from the `props` and maps over it. For each task it will then display a `li` element with the content of tasks. You might be wondering why we assign a `key` attribute. React requires that each element we return through a map function has a **unique** key property. Here we assigned `key={task}`, this is not a very good practice, because as you might have though, our tasks are not necessarily unique. In reality you would usually have a unique `id` assigned to each element as a key. If you are working with a backend, you can often use the id the database gives you for this. Anyways, for simplicity we used for this short example our `task` as key.

Almost done, the only thing we need to do is importing our `Overview.js` component to our `App.js` component and adding it in our render method, as well as passing down the `tasks` array as props.

Add this line to the top of your `App.js` component, right below where we import React.

```
import Overview from "./components/Overview";
```

And then add the Overview component to your render method in `App.js`. Add this line of code right before the last closing `div`, and right before the closing `form` tag in `App.js`.

```
<Overview tasks={this.state.tasks} />
```

Here we go, one last time run `npm start`. If you've done everything right, you should now be able to type a task into the input field and click submit to display it right below the input field. Fell free to play around a little bit and maybe change or style it as you like.

Your finished files should look like this:

```
// App.js

import React, {Component} from "react"
import Overview from "./components/Overview"

class App extends Component {
  constructor() {
    super();

    this.state = {
      task: "",
      tasks: []
    };

  }

  handleChange = e => {
    this.setState({
      task: e.target.value
    });
  };

  onSubmitTask = e => {
    e.preventDefault();
    this.setState({
      tasks: this.state.tasks.concat(this.state.task),
      task: ""
    });
  };

  render() {
    return (
      <div className='col-6 mx-auto mt-5'>
        <form onSubmit={this.onSubmitTask}>
          <div className='form-group'>
            <label htmlFor='taskInput'>Enter task</label>
            <input
              onChange={this.handleChange}
              value={this.state.task}
              type='text'
              id='taskInput'
              className='form-control'
            />

          </div>
          <div className='form-group'>
            <button type="submit" className='btn btn-primary'>
              Add Task
            </button>
          </div>
        </form>

        <Overview tasks={this.state.tasks}/>
      </div>
    );
  }
}

export default App;

```

```
// Overview.js

import React from "react"

const Overview = (props) => {

  const {tasks} = props

  return (
    <div>
       {tasks.map(task => {
        return <li key={task}>{task}</li>;
      })}
    </div>

  )
}

export default Overview
```

### Optional Tasks / Ideas to play around

Here are a few optional tasks for you to practice. Try them out, if you can't solve them, continue with the curriculum and make sure to come back later to give them another try and see how you advanced.

### **Easy**

1. Try changing the button text.
2. Make each task an object, which has two keys, a title as well as an id. Assign a unique Id to each task, which you can then use as a "key" when mapping over the tasks array. The title should be displayed.
3. Instead of displaying unordered list items, manage the amount of tasks in state, and let each task display it's number. Yes, you could also do that with a simple ordered list, but where's the fun about that? Try using state.
4. Implement a delete button for each task. The delete button should remove the specific task from the state array. Don't forget to never directly assign state. If you want you can use [fontawesome](https://fontawesome.com/) for the icon.

### **Medium:**

1. Implement an edit button for each item. Of course you should be able to resubmit the edited task.

### **Hard:**

1. Make that edit button changeable. When the task is not in edit mode, display the edit button. If the task is in edit mode, display a small submit button instead of the edit button.

### Additional Resources
