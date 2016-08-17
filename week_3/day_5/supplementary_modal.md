# Creating Modals using the Bootstrap Framework

- Referring to the bootstrap framework, you construct modals by building your html:
  ```
    <div class="modal fade" id="<your-modal-id>">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <h1> Hello World </h1>
          </div>
        </div>
      </div>
    </div>
  ```

- To toggle your modal with for example, a button:
  ```
    <div class="btn btn-success" data-toggle="modal" data-target="<your-modal-id-with-the-hashtag-example: #hello-world>">
  ```

- Alternatively, you can use javascript as well to toggle you modals.
  ```$('#<your-modal-id>').modal(options)

- Refer to your available options [here](https://getbootstrap.com/javascript/#modals)

### Challenge:

- Move your login and registration page to modals.

- You can do this by moving the form into a partial which is rendered inside `application.html.erb`. This ensures that we get our modal no matter which page we visit.

- Remember that if you are using `@user` for your form object, you will need to switch it to `:user` to make it easily available no matter which page you are on.

- Look up the various features that the framework has to offer you and try using some of them. Here's a few interesting ones:
  - [Carouse](https://getbootstrap.com/javascript/#carousel)
  - [Accordions](https://getbootstrap.com/javascript/#accordions)
  - [Popovers](https://getbootstrap.com/javascript/#popovers)
