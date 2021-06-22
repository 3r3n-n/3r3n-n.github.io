---
layout: post
title: "How does multiple locales work in momentJS ?"
---
We all have a very intuitive definition of time as a concept. It can be very abstract but we know what we mean. It is a physical quantity and thus, it can be measured in different units. However, things start getting messy as soon as we look at the human interpretation of time. 

Leaving aside the very systematic scientific measurements of time (based on seconds and the metric system), our day-to-day units are so weird: they are based on years and days and these ones depend on the movements of the planet that we inhabit. Seven days make a week and four weeks make a month... well, kind of because four weeks are 28 days and months are usually 30 days long... well, sometimes they are 31, except for February, that one actually is 28... except every 4 years it is 29... and then we have the time of day, it depends on our timezone (our place on earth) and we have different ways to name all these terms, according to our language (which is usually related to out timezone). Ok, you get it, naming dates and times is not as straightforward.

In some situations, we may need to build a product that needs to communicate dates and times, so we need to make sure that these dates and times are displayed in a coherent way with the user's context. This is when locales come handy.

According to Wikipedia, a locale in computer software is a set of parameters that defines the user's language, region and any special variant preferences that the user wants to see in their user interface. This is also referred to as internationalization. Usually a locale identifier consists of at least a language code and a country/region code. *MomentJS* is a *JavaScript* library that handles the time aspect of locales.

With *momentJS*, we can specify the global locale of our project:

```jsx
moment.locale(String, Object);
```

The `moment.locale` function has a different name inside *momentJS*, this is `getSetGlobalLocale`. The `String` input is the locale name (for example, for Spanish we would write `es-mx`). The `Object` input contains the configuration of our locale. It is normal if we don't know the whole configuration of our locale so it is fine if we just provide its name, *momentJS* will look for it in its *lib* files through the `getLocale` internal function. 

The `getLocale` function makes use of two other internal library functions:

a) `loadLocale` takes a string with a locale name as input, retrieves it from the momentJS lib files and returns it, in a very simple way it is like `return locales[name]`. If it was not found, it returns `null`.

b) `chooseLocale` takes an array of strings with locales names. This function iterates through the array to find the most specific name in it (remember that locale names can have the more-specific form *language-region* but sometimes they are just *language* so this function splits the strings to check this).

So `getLocale` applies loadLocale if the input name is just a string or `chooseLocale` if it is a string array.

If we wanted to provide the whole configuration of our global locale, for Gujarati for example, we would write something similar to this [file](https://github.com/moment/moment/blob/develop/src/locale/gu.js#L32).

As we see, the locale configuration contains many ways to name the different time units in the specified language and region, as well as abbreviations. Then, momentJS would use its `defineLocale` internal function to create a new locale with this configuration. 

The `defineLocale` function works like this: if the new locale's name already exists in moment's *lib* folder, it shows a warning message and it does not let you create the locale with that name. Otherwise, the new locale is created. 

Actually, Gujarati is included in the *momentJS* files so we would not really need to do this, just write the *gu* key-name as our input parameter. It was just for the example.

We just saw a lot of something-locale functions but don't worry. As mentioned above, they are just internal to momentJS so as users of this library we just need to know which parameters we need to provide `moment.locale()` to fit our purpose.
