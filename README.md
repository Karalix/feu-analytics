# Feu Analytics

## Setup
1. Create a [Firebase](https://firebase.google.com) project
2. Create a Realtime Database (no Firestore, eeww)
3. Include [feu-analytics-min.js](/feu-analytics-min.js) to the web pages you want to analyze :
```html
    <script>var __KX_ID = '[A VERY NICE IDENTIFIER FOR YOUR WEBSITE]' ; var __KX_URL = 'https://[YOUR FIREBASE ID].firebaseio.com/'</script>
    <script src="https://cdn.jsdelivr.net/gh/Karalix/feu-analytics/feu-analytics-min.js" defer></script>
```
4. Use the following rules for your realtime database (there are ~~probably~~ __for sure__ holes in them) :
```json
{
  "rules": {
    "$site":{
      "click": {
        "$click": {
          "$clickattribute": {
            ".validate": "($clickattribute == 'sessionId' || $clickattribute == 'time' || $clickattribute == 'target' || $clickattribute == 'page' || $clickattribute == 'cta')"
          },
          ".validate": "newData.hasChildren(['sessionId', 'time', 'target', 'page', 'cta'])"
        }
      },
      "session": {
        "$session": {
          "$sessionattribute" : {
              ".validate": "($sessionattribute == 'sessionId' || $sessionattribute == 'time' || $sessionattribute == 'referrer' || $sessionattribute == 'page' || $sessionattribute == 'mobile' || $sessionattribute == 'country')"
          },
          ".validate": "newData.hasChildren(['sessionId', 'time', 'referrer', 'page', 'mobile','country'])"
        }
      },
      "view": {
        "$view": {
          "$viewattribute" : {
            ".validate": "($viewattribute == 'sessionId' || $viewattribute == 'openingTime' || $viewattribute == 'closingTime' || $viewattribute == 'page')"
          },
          ".validate": "newData.hasChildren(['sessionId', 'openingTime', 'closingTime', 'page'])"
        }
      },
      "$other": {
        ".validate":false
      }
    },
    ".read": true,
    ".write": "!data.exists() || newData.exists()",
    
  }
}
```
5. Copy and modify the file [dashboard.html](/dashboard.html) at *line 173*:
```js
      let firebaseurlbase = 'https://[YOUR FIREBASE ID].firebaseio.com'
```
6. Host somewhere the file (hint: you can use Firebase here too)
7. Et voilà !
