# How to Learn React in 2023

As the most popular JavaScript library for building frontend applications, there has never been a better year to learn React than 2023.

In this guide, I'm going to show you the most valuable resources and tips that I believe will help you learn React faster. You'll also save a lot of precious time and energy in the process.

## **Need to Learn JavaScript? You Can Learn it While Learning React**

The question I'm most frequently asked by beginners looking to learn React is: "do I need to learn JavaScript?"

React is a JavaScript library and is often advertised as being "just JavaScript." This suggests that to really learn React you must know JavaScript first. The way I would characterize JavaScript's relationship to React is–*the better you know JavaScript, the better a React developer you will become*.

With that being said, you do **not** need to learn all of JavaScript first. Many JavaScript concepts can be learned at the same time you are learning React.

Here is a shortlist of some of the concepts in JavaScript that you will need to understand to be able to use React effectively. These include:

* Variables
    
* Arrays (and the .map() function)
    
* Objects
    
* JavaScript events
    
* Functions and arrow functions
    
* Scopes and closures
    
* Promises and async-await syntax.
    
* Basic error handling
    

These are all the JavaScript concepts that, in my experience, you truly need to know to work with React at any level.

You will encounter and learn more React concepts as you start building your own projects and looking at the code of others.

If you want a helpful guide to start learning these concepts and more today, I've written an [extensive cheatsheet on freeCodeCamp](https://www.freecodecamp.org/news/javascript-skills-you-need-for-react-practical-examples/) that covers all the JavaScript that you need to be confident with React.

## **Looking for a Tutorial? Use the New React Docs**

When you first begin your React learning path, your first question will likely be: "where do I learn all the React-related information I need?" You might be asking whether you should watch courses on YouTube or purchase a course on Udemy.

While there are great and extensive React courses across many different sites, the first and primary site you should rely on is the official React documentation site: [reactjs.org](http://reactjs.org).

What's very special about learning React in 2023 versus learning it in previous years is that in the past year, the React documentation that has been completely updated and improved. It is entirely up-to-date with the current React version, has tons of practical examples, and even interactive code sandboxes so that you can start learning react in the browser without having to create projects on your own machine.

> If you ever want to spin up a new React project, you can do it very quickly in the browser using the link [react.new](http://react.new). This will create a CodeSandbox with a complete React application with which you can mess around. It's much faster and easier to do that creating a project on your own computer.

You can find the new React documentation at [beta.reactjs.org](http://beta.reactjs.org). In due time, you will be able to find it simply on [reactjs.org](http://reactjs.org).

![Screen-Shot-2023-01-02-at-5.27.01-AM](https://www.freecodecamp.org/news/content/images/2023/01/Screen-Shot-2023-01-02-at-5.27.01-AM.png align="left")

[beta.reactjs.org](http://beta.reactjs.org)

Another big reason to use the new React documentation is that it is very beginner-friendly and will, in my opinion, allow you to learn concepts much faster than you would otherwise. If you have been intimidated to reading technical documentation in the past, I think you will be pleasantly surprised.

## **You Don't Need to Learn Class Components**

If you are wondering whether you need to learn class components in 2023, you do not.

As you begin to learn React, you will hear about this thing called a **class component** which is based around a normal JavaScript class. This is no longer necessary to learn as a React developer, but it can still benefit you to learn it.

After the introduction of a feature called **React hooks** in 2018, React developers have moved over to using function components, which are made with JavaScript functions.

Class components are still a part of React and they are still used in production code in a number of instances (such as error boundaries). But just be aware that you do not need to learn them in order to be skilled at React.

In fact, I've rarely encountered them unless I'm looking at an older code base. Most of them have been migrated over to function components and React hooks already.

## **Do Yourself a Favor and Learn React Query**

Any serious application requires data. Often the data that you need will exist outside your application, so you will need a strategy to fetch it and use in your project.

When you get to the point of fetching data I would highly recommend that you learn a library called **React Query**.

You can install React Query through the npm package @tanstack/react-query.

![Screen-Shot-2023-01-02-at-5.28.02-AM](https://www.freecodecamp.org/news/content/images/2023/01/Screen-Shot-2023-01-02-at-5.28.02-AM.png align="left")

React Query, also called Tanstack Query

React Query has become the go-to library for fetching and managing external data in React applications.

What's the benefit of using React Query? On top of being a library so many React developers and companies are now familiar with and rely upon, it will benefit you greatly to learn React Query because it is arguably the most powerful way for you to fetch and manage external data in your React applications.

The greatest benefit of using React Query comes from that fact that is gives you a cache (a store) that lets you hold on to the result of each query, so you can "save" the data that you've fetched. Plus it has many tools to update that cache exactly the way you want.

It also features relevant information about the status of every query you make such as whether it is loading or it resulted in an error.

I would learn React Query after you are comfortable manually fetching data using tools like the Fetch API or Axios with the useEffect hook. Once you are, you will see the benefit of using React Query across all of your React projects.

## **You Don't Need to Learn Redux**

If you've tried to learn React in years past, you might have gotten the impression that to learn React, you ultimately had to learn another library called Redux.

**Redux** is a library that helps us manage state in our React applications. You can think of state as any data that might change in a React app. Redux enables us to manage one piece of state (like, is our user logged in) across every part of our React application.

Redux is no longer required to learn like it was in years past. The first reason for this is the release of the **React Context** API. In many cases, React developers used Redux to just pass data around their application's components. This is a job that React Context can do for us.

Additionally, if you need to change or update state in many different parts of your app (which is a different requirement that simply reading your state), there are many competitors to Redux. Unlike Redux, these libraries do not require that you learn new concepts, such as reducers, actions, and so on.

![image](https://www.freecodecamp.org/news/content/images/2023/01/image.png align="left")

Zustand - "Bearbone" state management

Some of these newer, "lighter" state management libraries include:

* **Zustand**
    
* Jotai
    
* Recoil
    

Once you get past the basics of React and begin building slightly larger applications beyond the standard todo app, you will understand the need for having a state management library. Redux is still a great library, but first look to a library with a simpler API such as Zustand.

## **Don't Necessarily Reach for Create React App**

Once you are at the point of creating a React project on your own that lives on your computer, know that there are many ways of doing this.

Web development in general is moving more and more online – not every React project has to be made on your computer. In many cases you can spin up a brand new project in the browser using tools like **CodeSandbox** or **StackBlitz** entirely free of charge.

There will be a point, however, where you will need or want to create a React project on your local machine. It is easy to reach for a tool like **Create React App**, which allows you to make a React project by running a single command.

Create React App is still a great tool to make React projects. Just like with Redux, new alternatives have emerged give you all the conveniences of Create React, but in a smaller, often faster package.

![Screen-Shot-2023-01-02-at-5.30.02-AM](https://www.freecodecamp.org/news/content/images/2023/01/Screen-Shot-2023-01-02-at-5.30.02-AM.png align="left")

Vite, an alternative to Create React App

The primary example of this is **Vite**. Vite is a build tool that allows you to spin up a new React project as well as many other kinds of JavaScript projects.

The benefit of using Vite the over Create React App is that it gives you a much faster development server.

To develop and make changes to your app, it must run on a development server. Create React Appl, by comparison, can take a relatively long time to start up and respond to changes in code that you make (this is called **hot reloading**). Vite provides a much faster experience in addition to requiring far fewer dependencies.

Another great alternative to Create React App is **Next.js**. Next is technically a React framework, which comes with many conveniences to make development a lot easier. It comes with its own router and data fetching tools, to name a couple.

The benefit of using a framework like Next.js is that you simply have to make fewer decisions when it comes to building your project. You need to pick far fewer dependencies upon which your project relies. Most everything that you need to build your React app comes built in to Next.js

With that being said, Next operates quite differently than normal React projects due to the fact that it's server-rendered by default. This can comes with its own problems and potential pitfalls to avoid. But if you're interested in building serious, production-ready React projects, Next is arguably the best choice.