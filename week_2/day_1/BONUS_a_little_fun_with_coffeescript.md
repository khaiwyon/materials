# Day 1

## Bonus - Previewing Images before you upload them.

- We're going to learn a little CoffeeScript, CoffeeScript is a variant of Javascript. Simply put, coffeescript compiles into Javascript when run.

```
  imagePreviewFunctions = () ->
    $('.image-uploader').on 'change', (event) ->

      files = event.target.files
      image = files[0]
      reader = new FileReader()

      reader.readAsDataURL(image)

      reader.onload = (file) ->
        img = new Image()
        img.src = file.target.result
        img.classList = "img-responsive img-settings center-block"
        $('#preview-image').html(img)

  $(document).on 'turbolinks:load', imagePreviewFunctions
```

On your `posts` `new.html.erb`, modify the following:

- On your `file_field`, add the class called `image-uploader`.

- Create new div on top of it with an id called `preview-image` (Make sure to size the div appropriately - e.g "300px by 300px?")

- This is a simple script that reads your `file_field` with the class `image-uploader` declared.

- It then creates a new `img` tag using `Image()`. After that, it adds the `src` and css `classes` `img-repsonsive`, `img-settings` and `center-block` to it.

- Finally, it looks for the `id` `preview-image` and renders the image on it.
