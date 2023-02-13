# Creating a toast notification system for you react web app

> Toast notifications are used to communicate low severity level information to users in an unobtrusive way.

## Tools used in Project

* React (I will use [Nextjs](https://nextjs.org/) for this project)
    
* [Tailwind](https://tailwindcss.com/) for styling with a plugin [DaisyUI](https://daisyui.com/), a simple tailwind component for making customisable styles for our projects
    
* [Framer motion](https://www.framer.com/motion/) for animations
    

## Pre-requisite knowledge

Since you are here, I can guess you know a little of React but these are the main react hooks I will use in this project

* useReducer
    
* useContext
    
* react custom hooks
    
* useCallback
    

All these can be accessed int the [Official React documentation](https://reactjs.org/).

## Project setup

Let's begin by creating our project and installing the necessary packages.

1. Create next app and give it any name of your choice  
    `npx create-next-app notification-system`
    
2. Move into the project directory and install necessary dependencies
    

```plaintext
cd notification-system
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npm install framer-motion
```

1. Set up tailwind for styling, you'll find all these setup config in [Tailwind framework setup](https://tailwindcss.com/docs/guides/nextjs)
    
2. add these to your global.css file in the main directory
    

```plaintext
@tailwind base;
@tailwind components;
@tailwind utilities;
```

* add these to your tailwind.config.js file
    

```plaintext
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

* initialize project `npm run dev`
    

## Project logic

Let's goooo, we are done with the project set up. The main point of this is to get the use notified with messages. We will use the reducer function to put all our notifications in one place, serve it to the whole app using useContext and use it within our project using react custom hook. I will explain as we progress.

## Create Reducer

We will need to create Actions for the notification. Create a src folder in your main directory and add a Reducer folder.

* Create a file and name it notificationAction.js. We will fill it with our Actions
    

```plaintext
// These are actions set for our notification and it will make more sense as we progress
export const notificationAction = {
  SUCCESS: "SUCCESS",
  WARNING: "WARNING",
  ERROR: "ERROR",
  ALERT: "ALERT",
  DELETE: "DELETE",
  ADD: "ADD",
  INACTIVE: "INACTIVE",
};

```

* Create a file in the same (Reducer) folder and name it notificationReducer.js.
    

```plaintext
import { notificationAction } from "./notificationAction";

// Initial notification state. It's empty for now
export const notificationInitialState = {
  notifications: [],
};

// Our reducer function
export default (state = notificationInitialState, { type, payload }) => {
  switch (type) {
    case notificationAction.ADD:
      // Add notification to the list (state..notifications)
      return { notifications: [...state.notifications, payload.notification] };
    case notificationAction.DELETE:
      // Remove/Delete notification
      const deleteNotifcation = state.notifications?.filter(
        (notification) => notification.id !== payload.id
      );
      return { notifications: [...deleteNotifcation] };
    case notificationAction.INACTIVE:
      // Make notifcation inactive
      const notifications = state.notifications?.map((notification) => {
        if (notification.id === payload.id) {
          return {
            ...notification,
            active: false,
          };
        }
        return notification;
      });
      return { notifications: [...notifications] };
    default:
      return state;
  }
};

```

* Create a last file in the Reducer Folder with name index.js, this file will export all reducer functions and action to organize our app.
    

```plaintext
import { notificationAction } from "./notificationAction";
import notificationReducer from "./notificationReducer";
import { notificationInitialState } from "./notificationReducer";

export { notificationInitialState, notificationReducer, notificationAction };

```

## Create Context

This folder holds the notification context and serves it to our app. Create a folder in the src folder and name it Context. Create a file with name Notification.js. In this folder we use useReducer hook to serve our Reducer to the app, useReducer hook takes the reducer function as the first parameter and the intitial state as the second parameter and returns an array that contains the state and dispatch function respectively. The state acts like useState's state.

* First, we create our function and add our reducer to the useReducer hook.
    

```plaintext
function Notification({ children }) {
  const [state, dispatch] = useReducer(
    notificationReducer,
    notificationInitialState
  );
return ()
}
export default Notification;

```

* Next create a function called notify that we will call each time we need to make a toast notification. it will take two (2) parameters, type and message (will explain that in that soon). Set the id of the notification (I will use their index count) and dispatch with type ADD notification. Our notification will have an id (which we will use to track a notification), type (whether our notification is a success toast, warning toast, information/alert toast or an error toast), message (the text for our notification) and active to display or hide our notification. Clear the notification with setTimeout function (feel free to use any length of time, I am using 6 seconds) . We return the id of the notification.
    

```plaintext
...

  const notify = (type, message) => {
    const notificationId = state.notifications.length;
    dispatch({
      type: notificationAction.ADD,
      payload: {
        notification: {
          id: notificationId,
          type: type,
          message: message,
          active: true,
        },
      },
    });
    setTimeout(() => {
      closeNotification(notificationId);
    }, 6000);
    return notificationId;
  };
...

```

* To hide our notification create a closeNotification function that will set the active key of our notification to false. To delete our notification after hiding it, create a deleteNotification function that will delete a second after the notification is hidden.
    

```plaintext
  const deleteNotifcation = (id) => {
    dispatch({
      type: notificationAction.DELETE,
      payload: {
        id: id,
      },
    });
  };

  const closeNotification = (id) => {
    dispatch({
      type: notificationAction.INACTIVE,
      payload: {
        id: id,
      },
    });
    setTimeout(() => {
      deleteNotifcation(id);
    }, 1000);
  };

```

* Create a function called showNotification athat will display our notifications. For this function we use useCallback hook which will run our function whenever the state change. This function will map all our notifications at the top of our website . We will create the NotificationCard Component later.
    

```plaintext
...
const showNotifications = useCallback(
    () => (
      <>
        {state.notifications.map((notification) => (
          <AnimatePresence key={notification?.id}>
            {notification?.active && (
              <motion.div
                initial={{
                  opacity: 0,
                  scale: 0.8,
                  y: "10%",
                }}
                animate={{
                  opacity: 1,
                  scale: 1,
                  y: "0%",
                }}
                exit={{
                  opacity: 0,
                  scale: 0.8,
                  y: "10%",
                }}
              >
                <NotificationCard
                  type={notification?.type}
                  message={notification?.message}
                />
              </motion.div>
            )}
          </AnimatePresence>
        ))}
      </>
    ),
    [state]
  );
...
```

* Create the Context to serve the app with our notification. Add it before the react function.
    

```plaintext
export const NotificationContext = createContext();
```

In the Notification function return the context provider and the value. Display the showNotification function on top of the site.

```plaintext
  return (
    <>
      <NotificationContext.Provider value={value}>
        <div className="w-full h-fit fixed left-0 top-0 pt-10 flex flex-col justify-center items-center gap-3 z-50">
          {showNotifications()}
        </div>
        {children}
      </NotificationContext.Provider>
    </>
  );
```

* Go to the \_app.js in the pages folder wrap the app with our context
    

```plaintext
import Notification from "../src/Context/Notification";
import "../styles/globals.css";

function MyApp({ Component, pageProps }) {
  return (
    <Notification>
      <Component {...pageProps} />
    </Notification>
  );
}

export default MyApp;

```

* Set the value of the Context Provider
    

```plaintext
  const value = {
    notifications: state?.notifications,
    notify,
    closeNotification,
  };
```

* The Notification.js should have this in it
    

```plaintext
import { AnimatePresence, motion } from "framer-motion";
import React, {
  createContext,
  useCallback,
  useEffect,
  useReducer,
} from "react";
import NotificationCard from "../Components/NotificationCard";
import {
  notificationAction,
  notificationInitialState,
  notificationReducer,
} from "../Reducer";

export const NotificationContext = createContext();
function Notification({ children }) {
  const [state, dispatch] = useReducer(
    notificationReducer,
    notificationInitialState
  );

  const deleteNotifcation = (id) => {
    dispatch({
      type: notificationAction.DELETE,
      payload: {
        id: id,
      },
    });
  };

  const closeNotification = (id) => {
    dispatch({
      type: notificationAction.INACTIVE,
      payload: {
        id: id,
      },
    });
    setTimeout(() => {
      deleteNotifcation(id);
    }, 1000);
  };

  const notify = (type, message) => {
    const notificationId = state.notifications.length;
    dispatch({
      type: notificationAction.ADD,
      payload: {
        notification: {
          id: notificationId,
          type: type,
          message: message,
          active: true,
        },
      },
    });
    setTimeout(() => {
      closeNotification(notificationId);
    }, 6000);
    return notificationId;
  };

  const showNotifications = useCallback(
    () => (
      <>
        {state.notifications.map((notification) => (
          <AnimatePresence key={notification?.id}>
            {notification?.active && (
              <motion.div
                initial={{
                  opacity: 0,
                  scale: 0.8,
                  y: "10%",
                }}
                animate={{
                  opacity: 1,
                  scale: 1,
                  y: "0%",
                }}
                exit={{
                  opacity: 0,
                  scale: 0.8,
                  y: "10%",
                }}
              >
                <NotificationCard
                  type={notification?.type}
                  message={notification?.message}
                />
              </motion.div>
            )}
          </AnimatePresence>
        ))}
      </>
    ),
    [state]
  );

  useEffect(() => {
    state;
  }, [state]);

  const value = {
    notifications: state?.notifications,
    notify,
    closeNotification,
  };
  return (
    <>
      <NotificationContext.Provider value={value}>
        <div className="w-full h-fit fixed left-0 top-0 pt-10 flex flex-col justify-center items-center gap-3 z-50">
          {showNotifications()}
        </div>
        {children}
      </NotificationContext.Provider>
    </>
  );
}

export default Notification;

```

## Create Custom hook

Use a custom hook to access the values of the context easily. Create a folder in the src folder and name it Hooks. Create a folder in the Hooks folder and name it useNotification. We use useContext to get our context from notification and use it in any component in the app provided the component is a child of the Notification Context.

```plaintext
import React, { useContext } from "react";
import { NotificationContext } from "../Context/Notification";

function useNotification() {
  const context = useContext(NotificationContext);

  if (context === undefined) {
    throw new Error("useNotification must be used within NotificationContext");
  }
  return context;
}

export default useNotification;

```

## Usage

Before we use the notification system we need to create the NotificationCard component. Create a Components folder in the src folder and create a file called Notification.js. Add a react function in the folder with props type and message. I am using the [card UI](https://daisyui.com/components/card/) from Daisy UI.

```plaintext
import React, { useEffect, useState } from "react";
import { notificationAction } from "../Reducer";

function NotificationCard({ type, message }) {
  const [bgColor, setBgColor] = useState("bg-white");
  useEffect(() => {
    switch (type) {
      case notificationAction.ALERT:
        setBgColor("bg-info");
        break;
      case notificationAction.ERROR:
        setBgColor("bg-error");
        break;
      case notificationAction.SUCCESS:
        setBgColor("bg-success");
        break;
      case notificationAction.WARNING:
        setBgColor("bg-warning");
        break;
      default:
        setBgColor("bg-gray-300");
        break;
    }
  }, [type, message]);

  return (
    <div className={`card w-96 ${bgColor} text-primary-content`}>
      <div className="card-body">
        <h2 className="card-title">Notification</h2>
        <p>{message}</p>
      </div>
    </div>
  );
}

export default NotificationCard;

```

we use useEffect and a switch statement to change the color and message for each notification.

* open index.js in the pages folder and clear the default text in the main tag leaving the function .
    

```plaintext
import Head from "next/head";

```

```plaintext
export default function Home() {

  return (
    <div className="">
      <Head>
        <title>Notification Web App</title>
        <meta name="description" content="Generated by create next app" />
        <link rel="icon" href="/favicon.ico" />
      </Head>
<main>
</main>

    </div>
  );
}

```

* Add 4 [buttons](https://daisyui.com/components/button/) from daisyUI inside the main tag, this will be used to trigger the notifications. These button will be use for the four different notification type.
    

```plaintext
<button className="btn btn-info">Info</button>
<button className="btn btn-success">Success</button>
<button className="btn btn-warning">Warning</button>
<button className="btn btn-error">Error</button>
```

* Use our custom useNotification hook and destructor the notify function.  
    `const { notify } = useNotification();`
    
    * Add an onClick function to the buttons and run the notify in them. Add the notificationAction as the first parameter for the type of notification we want and the message as the second parameter.
        

```plaintext
...
<button
          className="btn btn-info"
          onClick={() =>
            notify(
              notificationAction.ALERT,
              "This is an  information notification"
            )
          }
        >
          Info
        </button>
        <button
          className="btn btn-success"
          onClick={() =>
            notify(
              notificationAction.SUCCESS,
              "This is an success notification"
            )
          }
        >
          Success
        </button>
        <button
          className="btn btn-warning"
          onClick={() =>
            notify(
              notificationAction.WARNING,
              "This is an warning notification"
            )
          }
        >
          Warning
        </button>
        <button
          className="btn btn-error"
          onClick={() =>
            notify(notificationAction.ERROR, "This is an error notification")
          }
        >
          Error
        </button>
...
```

* index.js should have this code
    

```plaintext
import Head from "next/head";
import Image from "next/image";
import useNotification from "../src/Hook/useNotification";
import { notificationAction } from "../src/Reducer";
import styles from "../styles/Home.module.css";

export default function Home() {
  const { notify } = useNotification();

  return (
    <div className="w-full h-screen">
      <Head>
        <title>Notification Web App</title>
        <meta name="description" content="Generated by create next app" />
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main className="w-full h-full flex justify-center gap-3 items-center">
        <button
          className="btn btn-info"
          onClick={() =>
            notify(
              notificationAction.ALERT,
              "This is an  information notification"
            )
          }
        >
          Info
        </button>
        <button
          className="btn btn-success"
          onClick={() =>
            notify(
              notificationAction.SUCCESS,
              "This is an success notification"
            )
          }
        >
          Success
        </button>
        <button
          className="btn btn-warning"
          onClick={() =>
            notify(
              notificationAction.WARNING,
              "This is an warning notification"
            )
          }
        >
          Warning
        </button>
        <button
          className="btn btn-error"
          onClick={() =>
            notify(notificationAction.ERROR, "This is an error notification")
          }
        >
          Error
        </button>
      </main>
    </div>
  );
}

```

## Enjoy

Open the console and run the app, and it should be open in [http://localhost:3000](http://localhost:3000)

```plaintext
npm run dev
```