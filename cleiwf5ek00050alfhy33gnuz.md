---
title: "How to optimize Next.js app bundle and improve its performance"
datePublished: Fri Feb 24 2023 19:00:44 GMT+0000 (Coordinated Universal Time)
cuid: cleiwf5ek00050alfhy33gnuz
slug: how-to-optimize-nextjs-app-bundle-and-improve-its-performance
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677265206245/3ee190fd-9609-40d1-a852-3cd6ffb50e99.jpeg

---

In this article, we will learn how to optimize the Next.js app ([link to the app](https://www.moneycontrol.com/stocksmarketsindia/heat-map-advance-decline-ratio-nse-bse)) by reducing the bundle size by 43% and increase the score to 73 from 36 in Google’s [PageSpeed Insights](https://pagespeed.web.dev/).

Let’s start with analyzing the production build of Next.js. When we execute **npm run build**, Next.js gives us a statistic of the production build. It specifies the size of pages and chunks that will be delivered to the browser.

Next.js creates files that are common and will be shared across pages. These files are called the First Load JS. The First Load JS files contain framework code and the code that is used by multiple pages.

If the component reuses more than 50% of the files, then Next.js includes that component in the First Load JS. We cannot change this behavior, but we can optimize our code.

Following is the output of the **npm run build** command and Google PageSpeed Insights score. As you can see, the sizes of the files are in red and yellow. Our goal is to make them green.

![before-optimization-bundle-size](https://www.syncfusion.com/blogs/wp-content/uploads/2022/09/before-optimization-bundle-size.jpg align="left")

![before-optimization-google-page-insights](https://www.syncfusion.com/blogs/wp-content/uploads/2022/09/before-optimization-google-page-insights.jpg align="left")

Let’s go.

## Analyze First Load JS

We start by analyzing and identifying the bundles included in the First Load JS. In order to find that, we need to install the following dev dependencies.

<table><tbody><tr><td colspan="1" rowspan="1"><p><code>npm install –save-dev @next/bundle-analyzer cross-env</code></p></td></tr></tbody></table>

After installation, add the following code in the **package.json** under **scripts**.

```plaintext
"scripts": {
    "analyze": "cross-env ANALYZE=true next build",
    "analyze:server": "cross-env BUNDLE_ANALYZE=server next build",
    "analyze:browser": "cross-env BUNDLE_ANALYZE=browser next build"
  },
```

Add the following code in next.config.js. If you don’t have this file, then create it in the root directory of your project.

```plaintext
const withBundleAnalyzer = require('@next/bundle-analyzer')({
    enabled: process.env.ANALYZE === 'true'
})
  
module.exports = withBundleAnalyzer({
    env: {
        NEXT_PUBLIC_ENV: 'PRODUCTION', //your next configs goes here
    },
})
```

Next, execute this command **npm run analyze**, which will open two new tabs in your browser with graphs. Focus on the **client.html** graph.

Then, replace the larger libraries with smaller, equivalent ones and remove the irrelevant libraries. In order to replace any library, go to [https://bundlephobia.com/](https://bundlephobia.com/),  see the size of your library, and check whether you have any alternatives with smaller sizes. Otherwise, try to write your own custom code. In this demo:

* We removed the **js-cookie** library, which was used to read and write cookies, and replaced it with custom code.
    
* We replaced the **framer-motion** library with react-transition-group because it was 10 times smaller.
    
* Instead of using **react-gpt**, which is used to display Google ads, we created our own custom ad component (more details following).
    
* We moved the **momentjs** file to the server side from the client, as the size of **momentjs** is big. By doing this, we can either send the formatted date from the API or format the date on the server side, as follows.
    

```plaintext
export async function getServerSideProps(context) {
  const moment = (await import('moment')).default(); //default method is to access default export
  return {
    date: moment.format('dddd D MMMM YYYY'),
  }
}
```

Now this date will be sent as a prop to the component, which can be used **(getServerSideProps can only be used in pages not inside components)**. Now, **momentjs** will not be sent to the browser.

## Dynamic imports

Components that aren’t visible in the initial load of the page and components that are displayed based on certain conditions should be imported dynamically instead of normally. This ensures that these components are sent to the browser only when they are needed. Refer to the following code example.

```plaintext
import dynamic from 'next/dynamic'

const Modal = dynamic(() => import('../components/header'));

export default function Home() {
  return (
    {showModal && <Modal />}
  )
}
```

Open the Network tab. When the condition is fulfilled, you’ll see that a new network request is made to fetch the dynamic component (click the button to open a modal).

## Lazy load images using next/image

Next.js has a built-in component named **next/image**. It loads images only when they are in the viewport.

```plaintext
import Image from "next/image";

const Index = () => {
  return (
    <>
      <p>
        External domains must be configured in <Code>next.config.js</Code> using
        the <Code>domains</Code> property.
      </p>
      <Image
        alt="Next.js logo"
        src="https://assets.vercel.com/image/upload/v1538361091/repositories/next-js/next-js-bg.png"
        width={1200}
        height={400}
      />
    </>
  );
};
```

## Lazy load Google Ads

The Google Ads script usually blocks the main thread, so we are going to load the script eight seconds after the page load. Initially, we were using the **react-gpt** package to load the Google Ads script. We are going to replace that with our optimized custom code.

For this, we have created a custom hook to inject any scripts. It will return the status depending on the state of the injection when the Google Ads script is injected. Once it returns **ready**, we run the Ads code.

```plaintext
import React from "react";

const useScript = (src, delay = null) => {
  const [status, setStatus] = React.useState(src ? "loading" : "idle");

  React.useEffect(() => {
    if (!src) {
      setStatus("idle");
      return "idle";
    }

    let script = document.querySelector(`script[src="${src}"]`);
    let timeout = null;

    if (!script) {
      if (delay) {
        timeout = setTimeout(() => {
          injectScript();
          // Add event listener after the script is added
          script.addEventListener("load", setStateStatus);
          script.addEventListener("error", setStateStatus);
        }, delay);
      } else {
        injectScript();
      }
    } else {
      setStatus(script.getAttribute("data-status"));
    }

    const setStateStatus = (event) => {
      setStatus(event.type === "load" ? "ready" : "error");
    };

    //code to inject script
    function injectScript() {
      script = document.createElement("script");
      script.src = src;
      script.async = true;
      script.setAttribute("data-status", "loading");
      document.body.appendChild(script);

      const setDataStatus = (event) => {
        script.setAttribute(
          "data-status",
          event.type === "load" ? "ready" : "error"
        );
      };

      script.addEventListener("load", setDataStatus);
      script.addEventListener("error", setDataStatus);
    }

    if (script) {
      //script will be be undefined available when its delayed hence check it before adding listener
      script.addEventListener("load", setStateStatus);
      script.addEventListener("error", setStateStatus);
    }

    return () => {
      if (script) {
        script.removeEventListener("load", setStateStatus);
        script.removeEventListener("error", setStateStatus);
      }
      if (timeout) {
        clearTimeout(timeout);
      }
    };
  }, [src]);

  return status;
};

export default useScript;
```

Use this hook in **pages/index.js**.

```plaintext
const MyApp = () => {
  const status = useScript(script, 8000);
  return (
    <>
      <p>Parent status: {status}</p>
      {status === ‘idle’ && <Ads />}
    </>
  );
};
```

If you want to create a custom ad component like this, then follow [this article](https://medium.com/js-dojo/how-to-implement-dfp-doubleclick-for-publishers-in-react-js-vue-js-and-amp-653bd31c6e43).

You can also lazy load other scripts like the Google Analytics script.

## Specific imports

If you are using libraries like **lodash** and **date-fn**, we can easily reduce bundle size by just importing specific functions instead of a full import, like this.

```plaintext
//Old
Import _get from ‘lodash’

//New
Import _get from ‘lodash/get’
```

We can optimize the usage of multiple other libraries. For more details, [check out this article](https://github.com/GoogleChromeLabs/webpack-libs-optimizations). Don’t forget to remove unused imports from the project.

## Optimize next/link

If you are using **next/link** in your project, add the prefetch prop to it and set it to false. Next.js by default prefetches the pages whose links are in the viewport. Let’s say you have a header with two links, **‘/home’** and **‘/about’**. Even though the users are on the home page, assets of the about page will also be loaded because the about link can be seen in the viewport.

When prefetch is set to **false**, prefetching will still occur but only when the link is hovered over.

```plaintext
<li>
	<Link href="/about" prefetch={false}>
		<a>About Us</a>
    </Link>
</li>
<li>
	<Link href="/blog/hello-world" prefetch={false}>>
		<a>Blog Post</a>
    </Link>
</li>
```

## Optimize fonts

When we use icon libraries like **font-awesome**, we only use a maximum of 15 icons, but the full library is loaded. The problem is that it is render-blocking resources. So, instead of loading the complete library, you can just download the required icons as SVG files and use them. You can also lazy load these SVG images.

You can also use **font-display: swap;** for your fonts because it doesn’t block the rendering. The font face is instead given an infinite swap period and a minimal block period.

```plaintext
@font-face {
  font-family: ExampleFont;
  src: url(/path/to/fonts/examplefont.woff) format('woff'),
       url(/path/to/fonts/examplefont.eot) format('eot');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}
```

If you are using Google Fonts directly from the link, then download the fonts and self-host them.

## Lazy load React components (optional)

We can also load a component only when it is in the viewport using the **react-lazyload** library, which also supports SSR. We can provide offset, too, so users will be unaware of this lazy-loading behavior.

```plaintext
import LazyLoad from 'react-lazyload';

<LazyLoad offset={100}>
    <Footer />
</LazyLoad>
```

## Exclude big libraries from bundle

As I discussed earlier, libraries are added to First Load JS. In our project, we are using the [Syncfusion Charts](https://www.syncfusion.com/react-ui-components/react-charts) library on multiple pages, so the **Syncfusion charts** library was bundled into the first load.

Pages that were not using **Syncfusion charts** were also loading the **Syncfusion charts** library, since it was added into to First Load JS or the main bundle. In order to optimize it, we followed this awesome article by Robert S on [how to exclude big libraries](https://betterprogramming.pub/next-js-reducing-bundle-size-when-using-third-party-libraries-db5407252d59#:~:text=The%20first%20thing%20we%20can,the%20AnyChart%20chart(s)).

## Final results

![after-optimization-bundle-size](https://www.syncfusion.com/blogs/wp-content/uploads/2022/09/after-optimization-bundle-size.jpg align="left")

\*We are currently optimizing the last red page

## Bonus

If you want to convince your manager or client to remove some sections from the website or load them later, then visit [https://www.performancebudget.io/](https://www.performancebudget.io/).

Enter the number of seconds you want your web app to take to load. Then, select the 3G network (because Google PageSpeed Insights uses 3G speed to test). Press **Calculate**.

You will get your size budget (total size of resources that should be sent to the browser) that will allow your web app to load in the specified number of seconds.

Simply start your project from this step.

## Conclusion

If you want good performance and a better Google PageSpeed Insights score, then ship fewer resources to the browser.

Before adding any package, check the size in [https://bundlephobia.com/](https://bundlephobia.com/). You can’t keep multiple, complex features and have great performance at the same time.

I hope you now have a good idea of how to optimize the Next.js app to improve its score on Google PageSpeed Insights. Use these techniques to optimize your old website to make it faster.

Thank you for reading!