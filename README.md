# GTM-integration-Hugo
Google Tag Manager integration for Hugo static websites

In this article I will provide a step-by-step instruction and code samples on how to implement Google Tag Manager and a datalayer to your Hugo static website. As a digital analytics consultant I can appreciate the value a good Google Tag Manager (GTM) implementation can have for your business or even to optimize your user’s blog experience.

## How to add the standard Google Tag Manager container to a Hugo website

To make sure that Google Tag Manager is enabled, we will have to include the Google Tag Manager container. This is a piece of standard javascript that allows us to start using Google Tag Manager on our website in the most basic sense. This javascript tracking code will be included in the head section of the website and a small iframe version of the snippet will be included right after the opening body tag. The iframe version allows us to enable basic Google Tag Manager functionalities on browsers that don’t have javascript enabled.

### What does the Google Tag Manager container do?

The Google Tag Manager container enables the Google Tag Manager basic functionalities by including a reference to the Google Tag Manager javascript library. This allows you to manage tags from Google Tag Manager and trigger them on your website.

### Include Google Tag Manager partial in your website

In order to run Google Tag Manager on your website, I’ve created a Hugo partial that you can include in your Hugo partials folder. The partials folder is usually located in the themes folder. Select your theme in this folder to find the layouts folder. That’s where you’ll find the partials folder. In my case it is: `\themes\beautifulhugo\layouts\partials`

In this folder, add the file called gtm.html from [my Github repository](https://github.com/martijnvv/GTM-integration-Hugo). It is the partial that will be needed to included the proper code to run Google Tag Manager.

The partial includes three components:

-   The basic Google Tag Manager snippet
-   A prefetch and preconnect functionality to improve the loading time of the code
-   A datalayer component to leverage additional data in Google Tag Manager

Don’t worry, I’m going to cover all three components in this article.

### Include the code in your current website partials and basic layout

You will have to reference the Google Tag Manager partial in your current setup for it the become part of the sourcecode of your page. Have a closer look at your partials folder. Look for the partial called `head.html`. You will have to include the following reference to the Google Tag Manager partial to include it.

`{{ if .Site.Params.gtm_id}}{{- partial "gtm.html" . }}{{ end }}`

You should add it anywhere between the `<head>` tags.

To include the additional iframe version of Google Tag Manager, we will have to update an additional file. Go to your `\themes\beautifulhugo\layouts\_default` folder and open the `baseof.html` file. Right below the `<body>` element, include the following code snippet.

`{{ if .Site.Params.gtm_id}}<noscript><iframe src="//www.googletagmanager.com/ns.html?id={{ .Site.Params.gtm_id }}" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>{{ end }}`

### Updating your config.toml file

You may want to enable or disable the Google Tag Manager code altogether. That’s why we will add an additional site parameters to the config.toml file which will enable or disable the code and allow you to easily change the Google Tag Manager unique ID.

First, create a Google Tag Manager container on the Google Tag Manager website. Check out the video below to learn how to do this.

<iframe src="https://www.youtube.com/embed/P4suvDuj0kI?enablejsapi=1&amp;color=white&amp;modestbranding=1" allowfullscreen="" data-gtm-yt-inspected-1332276_43="true" id="377488793" data-gtm-yt-inspected-1_27="true"></iframe>

Once you have created your container, grab the unique container ID from the code snippet (should start with GTM-). In your config.toml file, add an additional parameter called gtm\_id. Add the container ID as a value, like in the image below.

 ![GTM parameter config.toml](https://martijnvanvreeden.nl/img/gtm/params.jpg) 

GTM parameter config.toml

After you added the additional param, the GTM container code will be included in your website. Let’s test if that worked.

### Testing the implementation

Now it’s time to build your website and see Google Tag Manager in action. If all went well, you should see the Google Tag Manager code being load in your browser. Here are a few ways to validate if the code is included properly:

-   Check your browser developer console
-   Use a browser extension to check if it worked
-   Enable preview mode in Google Tag Manager

#### Check your browser developer console

Load the page of your website where Google Tag Manager is included and hit F12. This will open the DevTools panel. When you go to the Network tab and reload the page, all the elements of the page will be shown here. Look for one that starts with `gtm.js` (or use the filter option). If it is in there, you have successfully implemented Google Tag Manager on the page.

#### Use a browser extension

Your second option is to use a browser plugin. They will provide you with a similar insight, but might be a bit more user-friendly. These are a few to chose from:

-   [Tag assistant (by Google)](https://chrome.google.com/webstore/detail/tag-assistant-by-google/kejbdjndbnbjgmefkgdddjlbokphdefk?hl=nl)
-   [GTM debug extension (by GTM expert David Vallejo)](https://www.thyngster.com/tools/gtm-debug-extension/)

Once you’ve installed either one of these extensions, it will be easy to validate if your implementation worked.

#### Enable preview mode in Google Tag Manager

If you are a bit familiar with how Google Tag Manager works, this is a great way to check if Google Tag Manager is implemented on your website. Simply go to the Google Tag Manager and click on the container you are looking to implement.In the right top corner you will find a button saying “preview”. Click on this and wait until the orange panel appears. The previewer is now enabled on your browser.

Now go to your website. You should now see a preview panel appear at the bottom of your screen. If this is the case, you have implemented the Google Tag Manager container successfully. Below is a video explaining the debug functionality in more detail.

<iframe src="https://www.youtube.com/embed/uUAKkgQGBT0?enablejsapi=1&amp;color=white&amp;modestbranding=1" allowfullscreen="" data-gtm-yt-inspected-1332276_43="true" id="481371381" data-gtm-yt-inspected-1_27="true"></iframe>

## Make sure Google Tag Manager loads fast

Google Tag Manager is loading a 3rd party javascript library in your website. [This will impact the performance of your website.](https://martijnvanvreeden.nl/10-ways-to-improve-your-hugo-website-performance/) Depending on how you use Google Tag Manager, it can be slowing your website down. That’s why we take some additional measures to decrease the negative impact on performance and keep your website lightning fast.

### Add preconnect and prefetch features for better performance

In a previous article I shared some recommendations to [make your Hugo website load super-fast](https://martijnvanvreeden.nl/10-ways-to-improve-your-hugo-website-performance/). One of those tips was to use dns-prefetch and preconnect to increase the performance of your javascript files. This is what we’ll do for our Google Tag Manager javascript library as well.

In the gtm.html partial, I already included some additional code to help you achieve this.

`{{ if .Site.Params.gtm_id}}<link href="https://www.googletagmanager.com" rel="preconnect" crossorigin> <link rel="dns-prefetch" href="https://www.googletagmanager.com">{{ end }}`

## Add a standard datalayer to Google Tag Manager

If you have walked through the steps in this article up until now, you have probably successfully implemented the basic Google Tag Manager script. If you want to move your analytics and online marketing efforts to the next level, continue reading. The next part is about the datalayer, where the real magic happens.

### What is a datalayer and why is it important?

To put it shortly, a datalayer is a data structure which ideally holds all data that you want to process and pass from your website (or other digital context) to other applications that you have linked to. We use a datalayer to decouple information that is shown on a page or website in some way and provide it to our tools in an easy to digest format.

You can often see important information on a page that you want to use in analysics or online marketing tools, like the price of a product, name of a product or author of an article. We use the datalayer to capture that type of information and send it to Google Tag Manager in a simple format. If you want to read a more detailed explanation or definition of the datalayer, I recommend [an article by Google Tag Manager expert Simo Ahava](https://www.simoahava.com/analytics/data-layer/).

 ![GTM datalayer example](https://martijnvanvreeden.nl/img/gtm/google-tag-manager-data-layer.jpg) 

GTM datalayer example

### How can you use the datalayer?

The datalayer has several important purposes:

-   to help understand what is shown on a page
-   be a translation of the content of the page into a structured format
-   function as a datastructure to pass important information to other tools

From a technical perspective, it is possible to scrape most of this data from each page, from with Google Tag Manager itself. However, this can be very time-consuming, have a negative impact on website performance and error prone (the page and elements on a page may change). That’s why we usually decouple the elements a user sees on a page from what we need in our dataset.

Tracking solutions like Google Analytics, Facebook advertising and many others can use the data in the datalayer to access certain information. The tags can be triggered based on specific logic from the datalayer or populate themselves with the data for analysis. For a more detailed article about this mechanism, please check out the article from [Bounteous about datalayers](https://www.bounteous.com/insights/2013/10/15/unlock-data-layer-non-developers-guide-google-tag-manager/).

## Include Hugo variables in the datalayer

Hugo comes with tons of useful variables to include in a datalayer. The most relevant variables for the datalayer can be found in the [Page Variables section](https://gohugo.io/variables/page/) of the Hugo documentation.

There are several page related variables in the Hugo Page Variables overview I want to include in the datalayer:

-   .Title - the title of the page
-   .Permalink - A less beautified page title (ie. /tags/tagname/)
-   .FuzzyWordCount - the approximate wordcount of the page
-   .WordCount - the number of words on a page
-   .ReadingTime - the estimated time to read the content in minutes
-   .ExpiryDate - to show when the content expires
-   .PublishDate - to show the date of publication of the article
-   .Lastmod - to show the date the article was last modified
-   .Language - tells us what language the page is in (for multilingual sites)
-   .IsTranslated - shows if there are alternative languages available (for multilingual sites)
-   .Kind - to show what type of page is shown (ie home, page, etc.)
-   .Type - the type of content of a page (ie post, page, etc.)
-   .File.UniqueID - An unique page ID

All of these variables are provided by Hugo out-of-the-box. We can add these variables to our datalayer to use in our Google Tag Manager solution.

In addition, there are also a few very common variables I have included in the basic datalayer. These variables are often generated when you create a new article:

-   page category
-   page author
-   page tags

In addition to these variables, I’ve also included some extra ones I will be using.

-   Reading time in seconds (minutes can be a bit too rounded)
-   Page type 2 - where I use .Type and .Kind to create a combined variable for page types

The datalayer that is created based on the page variables will look something like this:

 ![GTM basic datalayer](https://martijnvanvreeden.nl/img/gtm/datalayer_basic.jpg) 

GTM basic datalayer

By adding an additional parameter to your config.toml file, the entire datalayer will be included in your website.

`gtm_datalayer = "basic"`

The new configuration should look something like this:

 ![GTM basic datalayer config.toml](https://martijnvanvreeden.nl/img/gtm/params_basic_datalayer.jpg) 

GTM basic datalayer config.toml

The variables in the datalayer are based on Pages variables in Hugo. On some pages, some of the variables will be missing (ie homepage). This happens because not all pages variables from Hugo are available for all page types.

## Update 4-5-2021 - Support for Proxy Google Tag Manager Web Container

With the introduction of GTM server side, it is now easier to render the gtm.js file from your own serverside endpoint. This will allow for even better performance and less dependency of the Google servers. Instead of loading the gtm.js file from [https://www.googletagmanager.com](https://www.googletagmanager.com/), you can now do this from any custom (sub)domain. This update is also reflected in the GTM template for Hugo.

The additional site param “gtm\_endpoint” is now part of the config.toml and template. Simply add the custom domain (ie gtm.martijnvanvreeden.nl) to this parameter to include tracking from your own domain.

For example:

`gtm_endpoint = "gtm.martijnvanvreeden.nl"`

For a detailed outline of Serverside Google Tag Manager, please check out [Simo Ahava’s blog on this SSGTM](https://www.simoahava.com/analytics/server-side-tagging-google-tag-manager/).

If you wish to continue to use the standard webcontainer version of GTM, simply exclude the parameter from the config.toml.

## Conclusion

Hopefully this article will help you configure Google Tag Manager on your Hugo website. I’ve currently included the instruction for the first few steps. In the future, I may include additional ways to enrich the datalayer or an article on how to leverage the datalayer for Google Tag Manager and Google Analytics use.

If you have any questions, feel free to reach out via [Twitter](https://twitter.com/martijnvv) or [Mail](https://martijnvanvreeden.nl/how-to-add-google-tag-manager-to-hugo-static-website/mail@martijnvanvreeden.nl).



Please read the detailed instruction on how to implement on my blog: [How to add Google Tag Manager to a Hugo static website](https:///martijnvanvreeden.nl/how-to-add-google-tag-manager-to-hugo-static-website/)

## Updates:

* 04-05-2021: Included additional site param to manage a custom endpoint for Server Side GTM "gtm_endpoint" 
