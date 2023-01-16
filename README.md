# Vehicle Information Filtering Inquiry
## Deployment link

[https://pre-onboarding-7th-2-1-4-five.vercel.app](https://pre-onboarding-7th-2-1-4-five.vercel.app/)

# How to set up and run your environment

## Configuration Settings

1. It is recommended to run on NodeJS 16.14.2.

## Installation

```
npm ci

```

## Execute

```
npm start

```

# Directory structure

```
📦src
┣ ┣ 📂Components
┃ ┃ ┣ 📂CarItem
┃ ┃ ┣ 📂CarItemList
┃ ┃ ┣ 📂CarProfile
┃ ┃ ┣ 📂Category
┃ ┃ ┣ 📂ItemTag
┃ ┃ ┣ 📂Layout
┃ ┃ ┃ ┣ 📂HeaderBar
┃ ┃ ┃ ┣ 📂IconBack
┃ ┃ ┣ 📂ListHeader
┃ ┃ ┣ 📂ListItem
┃ ┃ ┗ 📂Tag
┣ ┣ 📂Pages
┃ ┃ ┣ 📂Detail
┃ ┃ ┗ 📂Home
┣ ┣ 📂Routes
┣ ┣ 📂lib
┃ ┃ ┣ 📂api
┃ ┃ ┣ 📂hooks
┃ ┃ ┣ 📂styles
┃ ┃ ┣ 📂types
┃ ┃ ┣ 📂utils
┣ ┣ 📜App.tsx
┣ ┣ 📜index.tsx
┗ ┗ 📜react-app-env.d.ts

```

# Implementation

## 1. Replacement of data using custom hooks

- Data retrieved from the server was returned using a hook such as useChangeDetailData and reflected in the component.
- This design has the advantage of changing the way data is created in only one place, which is reflected in multiple components. I think it is advantageous for reuse and maintenance.

## 2. Implementing SEO

- Initially, SEO was implemented using react-helmet-async. However, there was a problem that did not work properly after deployment. So we looked at the network window, and the static file in index.html contained only the basic meta tag that was in the build phase. react-helmet-async injects the meta tag into the built static index.html file using javascript. However, at the moment of copying the address and pasting it on SNS or blog, the object Crawler was looking at was an empty index.html file, so SEO did not work as intended because it thought there was no meta tag.
- To solve this problem, you need to create an html static file that you need to get initially through SSR or SSG.
- Pre-rendering is a method of rendering a specific page in advance and creating an html file when building. It is a method of specifying the routing path through the react-snap library, but the crawler of the SNS or search engine did not render on the client, so it was impossible to take the header-related information designated by helmet, etc. as well as the page contents.
- React official documentation says [ReactDOMServer] ([https://reactjs](https://reactjs/)).We opened the express server because we could use org/docs/react-dom-server.html#gatsby-focus-wrapper) and then opened the express server to send static html using the html file in the same folder as express.static() to implement Helmet.renderStatic(act) and React-tender(Standing-tender) with HTTP.
- However, in the deployment stage, the method of opening express simultaneously had to be a deployment that supports computing in the Linux environment, so in the environment such as vercel and netlify, there was no way to open it in the same folder, so we decided that it was suitable for projects with the backend.

## 3. Recall data using React Query

- After putting segment and fuelType in query key array, data was imported from API by detecting key change.
- By specifying options in the queryClient, you have integrated management without adding options for each useQuery.
- Using staleTime and cacheTime as options, the same call is made to take advantage of the existing values.
- We used the select option to cache and utilize filtered data.

## 4. Error and loading processing

- In the case of errors, we provided not only the NotFound page but also a separate page if another error occurs. Error handling was resolved using React Query and Axios together. Because the basic error type provided by JavaScript is difficult to handle. So, I used AxiosError to handle the error by referring to the error handling method provided by React Query blog.
- In the case of loading, user convenience was provided by showing the loading spinner when data was first loaded and when data was additionally loaded according to scrolling.
- In addition, we learned that global error processing or loading processing can be performed using Error Boundary or Suspend.

## 5. Common Header

- Rather than placing the header in the layout surrounding the entire viewport after receiving the child, the component corresponding to the layout path other than the header is displayed with the react-router-dom function to prevent the header component from being rerendered.
- I checked the pattern name and reused the header by adding a back button if the pattern name is not '/'.

## TypeScript

- I tried to use enum, but there was an inconvenience with the enum in the type script. This should be "this guy," but it's hard to use anywhere else because it's written in the same number as zero.
- Therefore, after declaring the object as 'as const', the key and value of the object were extracted as union type and used.

# Usage Library

### Production

- styled-components
    
    Used for CSS style.
    
- react-router-dom
    
    It was used to design page movement between SPAs more conveniently on react.
    
- tanstack/react-query
    
    It was used for data caching, efficient error handling, and patching.
    
- react-helmet-async
    
    It was used for SEO implementation.
    

### Dev

- eslint
- prettier
- husky
