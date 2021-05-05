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

### 
