# How to Handle HTTP Request with Async Await in React

React async / await tutorial; In this guide, we will learn how to send an HTTP request using Fetch API and handle the HTTP response using JavaScript’s async and await syntaxes.

In this comprehensive guide, we will create a simple react app; this app will show the countries data. For displaying the countries information, we will send the Asynchronous HTTP Get Request using the Fetch API and [**REST countries API**](https://restcountries.com/).

Asynchronous requests are really helpful in web development, and it provides flexibility in handling responses that may take unexpected time.  
The asynchronous paradigm quantifies the efficiency of the application by not hampering the process of other programmes.

There are a couple of other ways through which you can achieve a similar thing. However, we would like to shed light on the most efficient method, atleast that’s what we think.

Let us understand what async and await are?

The async is a function that is bound with a function that function returns the Promise object. A promise object returns the resolve and reject response. To pull out the response from the promise object, we use then and catch methods.

The await operator is declared with the Promise function, and it is always defined inside the async function as a top-level javascript module. The await operator holds the execution of the Promise function as long as the Promise is completely fulfilled.

Pulling out the Promise values from await function doesn’t require using the then and catch block; you can directly access the properties of Promise response.

## React Async Await Handle HTTP Request with API Example

* **Step 1:** Create React Application
    
* **Step 2:** Install Bootstrap Module
    
* **Step 3:** Make Component File
    
* **Step 4:** Handle HTTP Response with Async Await
    
* **Step 5:** Add Component in App Js
    
* **Step 6:** Run Development Server
    

## Create React Application

In this step, you will get the instructions to create a new React application.

If you already have the project setup then jump on to the subsequent step else execute the following command.

```bash
npx create-react-app react-git
```

Now, make sure to enter into the project folder.

```bash
cd react-git
```

## Install Bootstrap Module

In this step, you will be installing the bootstrap library in React. Here is the command that needs to be invoked from the terminal window.

```bash
npm install bootstrap
```

Next, get inside the **App.js** file, here you have to import the Bootstrap module CSS.

```javascript
import "bootstrap/dist/css/bootstrap.min.css"
```

## Make Component File

In this step, you will be creating a new component in React. In React component means a simple JavaScript function or a EcmaScript class object.

It is good to keep all the components intact at one place, for that, ensure that you created a **components/** directory,

Next, in this directory make a new file **Country.js**. Here is the basic structure of a function component in React.

```javascript
import React from 'react'
export default function Country() {
    return (
        <div>
            
        </div>
    )
}
```

## Handle HTTP Response with Async Await

In this step, you will find out how to make the HTTP request, fetch the data using the REST API and manage the HTTP response with async function and await operator.

Add the following code in **Country.js** file.

```javascript
import React, { useState, useEffect } from 'react'
export default function Country() {
  const [countryItems, initCountry] = useState([])
  const fetchData = async () => {
    const response = await fetch('https://restcountries.com/v3.1/all')
    if (!response.ok) {
      throw new Error('Data coud not be fetched!')
    } else {
      return response.json()
    }
  }
  useEffect(() => {
    fetchData()
      .then((res) => {
        initCountry(res)
      })
      .catch((e) => {
        console.log(e.message)
      })
  }, [])
  return (
    <div className="row">
      <h2 className="mb-3">React HTTP Reqeust with Async Await Example</h2>
      {countryItems.map((item, idx) => {
        return (
          <div className="col-lg-3 col-md-4 col-sm-6 mb-3" key={idx}>
            <div className="card h-100">
              <div className="img-block">
                <img
                  src={item.flags.svg}
                  className="card-img-top"
                  alt={item.name.common}
                />
              </div>
              <div className="card-body">
                <h5 className="card-title">{item.name.common}</h5>
              </div>
              <ul className="list-group list-group-flush">
                <li className="list-group-item">
                  <strong>Capital:</strong> {item.capital}
                </li>
                <li className="list-group-item">
                  <strong>Population:</strong> {item.population}
                </li>
                <li className="list-group-item">
                  <strong>Continent:</strong> {item.continents[0]}
                </li>
              </ul>
              <div className="card-body">
                <div className="d-grid">
                  <a
                    className="btn btn-dark"
                    href="{item.maps.googleMaps}"
                    target="_blank"
                  >
                    View Map
                  </a>
                </div>
              </div>
            </div>
          </div>
        )
      })}
    </div>
  )
}
```

## Add Component in App Js

In this step, you will register the Country component in the global app component.

Make sure to add the given code inside the **src/App.js** file.

```javascript
import React from 'react'
import "bootstrap/dist/css/bootstrap.min.css"
import Country from './components/http/Country'
export default function App() {
  return (
    <div className="container mt-5">
      <Country />
    </div>
  )
}
```

## Run Development Server

We have eventually reached at the final step, in the last section you will start the react app and check the feature on the browser.

```bash
npm start
```

Here is the url where you can see the countries data.

```bash
http://localhost:3000
```

Thanks for reading.

<iframe src="https://github.com/sponsors/cutesquirrel519/card" height="225" width="600" style="border:0"></iframe>