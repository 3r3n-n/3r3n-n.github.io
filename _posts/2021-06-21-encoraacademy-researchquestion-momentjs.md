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

If we wanted to provide the whole configuration of our global locale, for Gujarati for example, we would write this:

<details>
  <summary>
    
```jsx
  moment.defineLocale('gu', {
    months: 'જાન્યુઆરી_ફેબ્રુઆરી_માર્ચ_એપ્રિલ_મે_જૂન_જુલાઈ_ઑગસ્ટ_સપ્ટેમ્બર_ઑક્ટ્બર_નવેમ્બર_ડિસેમ્બર'.split(
        '_'<br>
    ),<br>
    monthsShort: 'જાન્યુ._ફેબ્રુ._માર્ચ_એપ્રિ._મે_જૂન_જુલા._ઑગ._સપ્ટે._ઑક્ટ્._નવે._ડિસે.'.split(
        '_'
    ),
    monthsParseExact: true,
    weekdays: 'રવિવાર_સોમવાર_મંગળવાર_બુધ્વાર_ગુરુવાર_શુક્રવાર_શનિવાર'.split(
        '_'
    ),
    weekdaysShort: 'રવિ_સોમ_મંગળ_બુધ્_ગુરુ_શુક્ર_શનિ'.split('_'),
    weekdaysMin: 'ર_સો_મં_બુ_ગુ_શુ_શ'.split('_'),
    longDateFormat: {
        LT: 'A h:mm વાગ્યે',
        LTS: 'A h:mm:ss વાગ્યે',
        L: 'DD/MM/YYYY',
        LL: 'D MMMM YYYY',
        LLL: 'D MMMM YYYY, A h:mm વાગ્યે',
        LLLL: 'dddd, D MMMM YYYY, A h:mm વાગ્યે',
      },
```
    
  </summary>
  
```
    calendar: {
        sameDay: '[આજ] LT',
        nextDay: '[કાલે] LT',
        nextWeek: 'dddd, LT',
        lastDay: '[ગઇકાલે] LT',
        lastWeek: '[પાછલા] dddd, LT',
        sameElse: 'L',
    },
    relativeTime: {
        future: '%s મા',
        past: '%s પહેલા',
        s: 'અમુક પળો',
        ss: '%d સેકંડ',
        m: 'એક મિનિટ',
        mm: '%d મિનિટ',
        h: 'એક કલાક',
        hh: '%d કલાક',
        d: 'એક દિવસ',
        dd: '%d દિવસ',
        M: 'એક મહિનો',
        MM: '%d મહિનો',
        y: 'એક વર્ષ',
        yy: '%d વર્ષ',
    },
    preparse: function (string) {
        return string.replace(/[૧૨૩૪૫૬૭૮૯૦]/g, function (match) {
            return numberMap[match];
        });
    },
    postformat: function (string) {
        return string.replace(/\d/g, function (match) {
            return symbolMap[match];
        });
    },
    // Gujarati notation for meridiems are quite fuzzy in practice. While there exists
    // a rigid notion of a 'Pahar' it is not used as rigidly in modern Gujarati.
    meridiemParse: /રાત|બપોર|સવાર|સાંજ/,
    meridiemHour: function (hour, meridiem) {
        if (hour === 12) {
            hour = 0;
        }
        if (meridiem === 'રાત') {
            return hour < 4 ? hour : hour + 12;
        } else if (meridiem === 'સવાર') {
            return hour;
        } else if (meridiem === 'બપોર') {
            return hour >= 10 ? hour : hour + 12;
        } else if (meridiem === 'સાંજ') {
            return hour + 12;
        }
    },
    meridiem: function (hour, minute, isLower) {
        if (hour < 4) {
            return 'રાત';
        } else if (hour < 10) {
            return 'સવાર';
        } else if (hour < 17) {
            return 'બપોર';
        } else if (hour < 20) {
            return 'સાંજ';
        } else {
            return 'રાત';
        }
    },
    week: {
        dow: 0, // Sunday is the first day of the week.
        doy: 6, // The week that contains Jan 6th is the first week of the year.
    },
});
```
  
</details>

As we see, the locale configuration contains many ways to name the different time units in the specified language and region, as well as abbreviations. Then, momentJS would use its `defineLocale` internal function to create a new locale with this configuration. 

The `defineLocale` function works like this: if the new locale's name already exists in moment's *lib* folder, it shows a warning message and it does not let you create the locale with that name. Otherwise, the new locale is created. 

Actually, Gujarati is included in the *momentJS* files so we would not really need to do this, just write the *gu* key-name as our input parameter. It was just for the example.

We just saw a lot of something-locale functions but don't worry. As mentioned above, they are just internal to momentJS so as users of this library we just need to know which parameters we need to provide `moment.locale()` to fit our purpose.
