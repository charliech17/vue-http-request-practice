# Vue 讀寫firebase 資料練習
## 重點一: Vue 用 fetch 去執行http request ，包括 POST、GET 和 Delete
1. POST: 可見於LearningSurvey.vue，在fetch中需設定method、header跟body。
2. GET: 可見於UserExperience.vue，在fetch中GET是預設值，不必設定其他參數。
3. DELETE: 可見於UserExperience.vue，在fetch中需設定 method、header等參數，在原本Get 的 url的後面加上要刪除的id，並把.json移到最後面

## 重點二: 執行完POST或Delete後，自動更新到資料庫中最新的資料
1. POST: 詳見 LearningSurvey.vue，以下列出關鍵程式碼部分: 在response ok 後，執行$emit('submitclick')。
```js
    fetch(
        'https://vue-http-demo-e1e10-default-rtdb.firebaseio.com/surveys.json',
        {
          //... 略
        }
      )
        .then((response) => {
          if (response.ok) {
            this.$emit('submitClick');
          } else {
            throw new Error('could not save data');
          }
        })
        .catch((error) => {
          this.error = error.message;
        });
```
2. DELETE: 詳見UserExperience.vue，以下列出關鍵程式碼部分: 在response ok 後，執行this.loadExperiences();。
```js
      fetch(
        `https://vue-http-demo-e1e10-default-rtdb.firebaseio.com/surveys/${id}.json`,
        {
          //... 略
          },
        }
      )
        .then((response) => {
          if (response.ok) {
            this.loadExperiences();
          } else {
            throw new Error('could not save data');
          }
        })
```