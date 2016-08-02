# Day 2 Lessons

## Bootstrap Hovertexts

Challenge: Find 4 images and create them as follows:

  ![Example](images/4images.png)

- Hint: Refer to what you've learnt with flexboxes so far, there are also examples inside `flexboxes.html`
and `hovertext.html` to help you.

- Once you've completed it, add the following into your `landing.scss` file

  ```
    .<your-class-name>:hover .invisible-layout {
      opacity: 0.5;
      transition: all 0.3s ease-in-out;
    }

    .invisible-layout {
      position: absolute;
      height: 100%;
      width: 100%;
      opacity: 0;
      display: flex;
      justify-content: center;
      background-color: yellow;
      align-items: center;
    }

  ```

- After that, add `position: relative ` to `<your-class-name>`

- When you're done, you can add the following html into your flex elements:

  ```
  <div class="invisible-layout">
    <div class="content">
      <h3>I'm a watch!</h3>
    </div>
  </div>
  ```
