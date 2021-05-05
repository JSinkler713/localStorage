# LocalStorage

Local Storage is a great way to store some things that are specific to the user on your site right now. Things that you don't need in an entire databse, but want to but nonetheless want to save to help your user out later on. Examples could include a user's dark/light mode preferences. If a user comes in and changes to the dark mode of your site (given you have the option), you would want to store this so that the **next time** they come on the site they don't have to set it again.

Local Storage is the right choice here, because you want the settings to persist **even when the browser is closed and reopened.** vs sessionStorage. (See more here [mdn](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)). So something like dark mode preference, or in the case of a recent app I've been working on, **recent searches** in an input field. These you want access to whenever your user opens up your site again.

Example on weather app

![weather-app-ex-searches](https://github.com/JSinkler713/localStorage/blob/6ee78a7dbf9267344ad887532b151e09c9549adb/Screen%20Shot%202021-05-04%20at%205.58.52%20PM.png)



### No database, no problem

So why even use a database when we have localStorage? Well, when your databases start to get more complex, you may want there to be associations that relate to other parts of your database. Local Storage is not going to do complex things like that. It can really only keep track of **strings**. One of the **benefits** of localStorage is how simple it is. Items are stored in key, value pairs. Example

```javascript
  localStorage.setItem('darkMode', 'true');
  const isbackgroundDark = localStorage.getItem('darkMode');
```

Notice the `'`'s. The keys and values are strings. The methods `setItem()` and `getItem()` allow us to store or retrieve data from the browser. This may have you starting to get dissapointed. Strings are great, but what about other data types, the string `'true'` for instance is not as useful as the Boolean data type javascript`true`.

So you may be wondering if you can store complex datatypes in there. The answer is a murky **yes and no**.

### JSON.parse && JSON.stringify

These two methods are going to increase localStorage's powers immensely. You have probably seen these used here and there, and they are great for helping convert **between strings and complex datatypes**. Let's look at the following code block. This is taken from a project I was working on where I wanted to store the recent searches of my users, so that they wouldn't have to type them over again.

The important part is it's **searches** not **search** which with javascript leads us to want to use an **array**. So

```javascript
    // step 1. retrieve searches
    let searches = localStorage.getItem('searches') // comes back like this '["brazil", "costa rica", "hawaii"]'
    // step 2. since it's really just a string at this point we need to parse it
    searches = JSON.parse(searches)
```

You could do all the steps at once, I've simply broken it down to two lines so that we can think about the process. Local Storage will only save strings, but you can wrap anything in quotes to make it a string. In order to **unwrap** it, we use `JSON.parse()`.

Likewise when we want to store something like an array in localStorage we need to wrap it up in quotes. Javscript's `JSON.stringify()` will go ahead and wrap up our array and convert it into a string for storage. Let's imagine we call a function with a newSearch we want to add in.

```javascript
  function updateSearches(newSearch) {
    // step 1. prep our new array of searches with spread operator
    // ** I am assuming we have already retrieved and parsed searches from localStorage
    let newSearchList = [ ...searches, newSearch]
    // step 2. stringify
    newSearchList = JSON.stringify(newSearchList) // it's now wrapped up '[...]'
    // step 3. store it as a string
    localStorage.setItem('searches', newSearchList)
  }
```
## Clear and remove
There are a few more things you want to be aware of with localStorage. This is the function I use to set mySearches back to an empty array. You don't have to do this, it's simply how my app is structured that it made sense to do this for me. It will always overwrite the value that is currently there, if we want to clear it fully we can choose one of the options below that will fully remove the key and value.
```
  const clearSearchHistory = ()=> {
    // to reset but keep a key reference, it will overwrite value there
    localStorage.setItem('searches', JSON.stringify([]))
    // localStorage.removeItem('searches') // this would fully remove the values, and key from localStorage
    // localStorage.clear() // this would remove ALL keys and values
    
  }
```

### Where is it?

To see what's in your localStorage you can open up your **developer tools** and go to **Application**. You will see a number of different options below, under **Storage** look at **Local Storage**, and you should be able to see your key value pairs. You may also notice Session Storage which works similarly but will not last after the browser closes. See diagram below.

![shows where it is](https://github.com/JSinkler713/localStorage/blob/0aecbf28b0542257af8cb663fd6fa16ea171285f/Screen%20Shot%202021-05-04%20at%205.46.29%20PM.png)

Alright. That's it. Have fun with it. 

I have a basic little rustic version on [this codesandbox](https://codesandbox.io/s/basic-localstorage-recent-search-sle1x) if you would like to see it with React and @reach/combobox to show some previous searches. The example is primitive. Feel free to fork it and improve it. Adding a way to clear for instance would be a good extension.


Happy Coding,

James
