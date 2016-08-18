# Finding Stuff with JQuery

- `$` or `jQuery` is the function call you make to initialize your `jQuery` library

- Most of your `elements` in your html file can be accessed using `$()`

- $('.class') : the `.` is identifier for html `class`.

- $('.#id') : the `#` is identified for html `id`.

- $('meta / div / p') : you can also pass in the `html` `tag`.

- $('.class-1.class-2') : if two classes are attached to each other, you are looking for an element
that has two classes. For example - `<div class="class-1 class-2"></div>`

- $('.class-1 .class-2') : separation between classes means that you are looking for an element
with that class nested under the first class. For example -

  ```
  <div class="class-1">
    <div class="class-2">
    </div>
  </div>
  ```

- Note that the 2 rules above apply to the rest as well (ids, tags etc)
