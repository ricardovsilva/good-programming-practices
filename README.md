# Good programming practices:
Some easy good practices of programming to use and improve your skills. Specially usefull for beginners, but anyone can use that.

If you want to contribute, just pull request your contribution, I will be glad to read and approve :).

# Frontend good practices
## Decouple CSS from Javascript
This is really really easy to do, and improve a lot the mantainability of your code. Just add js- prefix to classes that will be used by javascript. Simple as that, stop select element by id, or by css class, select by your new and fresh js- prefixed class.

**Reasons to start use js- prefix:**
  - Your js behaviour is reusable;
  - Frontend developers can change button style without worry about break behaviour, they will be happier with you;
  - You understand what that button do only by reading html;
  - If you have to change behaviour for that class, you know exactly what looking for into js;
  - No more thoughts: 'Oh my god, where in js it's element was being selected? It was selecting by id? by type? by parent? by xpath?

**Example:**

```html
  <!-- index.html -->
  <input type="text" class="awesome-green-input js-search-content"/>
  <button type="button" class="btn js-search-on-click">Basic</button>
```
```javascript
  //my_script.js
  $('.js-search-on-click').click(function(){
    var query = $('.js-search-content').val();
    $.get('/some-awesome-resource?query=' + query)
      .done(function(result){ alert(result); }
  });
```
