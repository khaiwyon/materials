# Day 3 Lessons

## BONUS - Fading Messages

- Let's now set that notifications naturally fade out after a time.

  We'll first create a new `coffeescript` file and add the following code:

  ```
    flashMessagesFunctions = () ->

    clearFlashMessages = () ->
      setTimeout( ()->
        $('.flash-messages').addClass('fadeOut')
      , 3000)
      setTimeout( ()->
        $('#flash-messages-container').html("")
      , 8000)

    if $('.flash-messages').length > 0
      clearFlashMessages()

  $(document).on 'turbolinks:load', flashMessagesFunctions
  ```

- We've set 2 timers, one that adds a `fadeOut` css Class to `flash-messages` and another to clear the message from
`flash-messages-container` after 8 seconds.

- Next, we need to wrap the `flash_messages` partial with a new div with the id `flash-messages-container`

- Finally, we're going to add a new css class called `.fadeOut`...

  ```
  .fadeOut {
    opacity: 0;
    transition: opacity 5s ease-in-out;
  }
  ```

- This sets that the element with `fadeOut` will gradually fade out through 5 seconds.
