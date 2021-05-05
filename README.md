# LocalStorage

Local Storage is a great way to store some things that are specific to the user on your site right now. Things that you don't want to be part of a saved database, but nonetheless want to save to help your user out later on. Examples could include your dark/light mode preferences. If a user comes in and changes to the darkmode of your site (given you have the option), you would want to store this so that the **next time** they come on the site they don't have to set it again.

LocalStorage is the right choice here, because you want the settings to persist **even when the browser is closed and reopened.** vs sessionStorage. (See more here [mdn](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)). So something like dark mode preference, or in the case of a recent app I've been working on, recent searches in an input field, you may want access to these whenever your user opens up your site again.

### No database, no problem

So why even use a database when we have localStorage? Well, when your databases start to get more complex, you may want there to be associations that relate to other parts of your database. One of the **benefits** of localStorage is how simple it is. Items are stored in key, value pairs. Example

```javascript
  localStorage.setItem('darkMode', 'true');
  const isbackgroundDark = localStorage.getItem('darkMode');
```

Notice the `'`'s. The keys and values are strings. This may have you starting to get dissapointed. You may be wondering if you can store complex datatypes in there. The answer is a murky **yes and no**.

### JSON.parse && JSON.stringify

You have probably seen these used here and there, and they are great for helping convert complex datatypes into something localStorage can understand. Let's look at the following code block. This is taken from a project I was working on where I wanted to store the recent searches of my users, so that they wouldn't have to type them over again.

The important part is it's **searches** not **search** which with javascript leads us to want to use an array. So

```javascript
    // step 1. retrieve searches
    let searches = localStorage.getItem('searches') // comes back like this '["brazil", "costa rica", "hawaii"]'
    // step 2. since it's really just a string at this point we need to parse it
    searches = JSON.parse(searches)
```

You could do all the steps at once, I've simply broken it down to two lines so that we can think about the process. Local Storage will only save strings, but you can wrap anything in quotes to make it a string. In order to **unwrap** it, we use `JSON.parse()`.

Likewise when we want to store something like an array in localStorage we need to wrap it up in quotes. Javscript's `Json.stringify()` will go ahead and wrap up our array and convert it into a string for storage. Let's imagine we call a function with a newSearch we want to add in.

```javascript
  function updateSearches(newSearch) {
  // step 1. prep our new array of searches with spread operator
  // ** I an assuming we have already retrieved and parsed searches from localStorage
  let newSearchList = [ ...searches, newSearch]
  // step 2. stringify
  newSearchList = JSON.stringify(newSearchList) // it's now wrapped up '[...]'
  // step 3. store it as a string
  localStorage.setItem('searches', newSearchList)
  }
```

There are a few more things you want to be aware of with localStorage. This is the function I use to set mySearches back to an empty array. The weather app I am using this in right now is built with React, so you can see that I also update my react state when I change my localStorage. You don't have to do this, it's simply how my app is structured that it made sense to do this for me.
```
  const clearSearchHistory = ()=> {
    localStorage.setItem('searches', JSON.stringify([]))
    setPreviousSearches([]) // only relevent for React State management
  }
```

### Where is it?

To see what's in your localStorage you can open up your developer tools and go to Application. You will see a number of different options below, under Storage look into Local Storage, and you should be able to see your key value pairs. You will also notice Session Storage which works similarly but will not last after the browser closes. See diagram below.

![here it is](https://github.com/JSinkler713/localStorage/blob/0cf4886416f71c03145a5acd267d685dea189ef1/Screen%20Shot%202021-05-04%20at%205.16.55%20PM.png)

Alright. That's it. Have fun with it. Did I say you could store objects too?
!()[]

Happy Coding,

James
