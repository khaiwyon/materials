# Setting up your first Rails Project

- Start by typing ``` rails new -T your-app-name --database=postgresql ```

- Once terminal has finished creating and bundling your new application, cd into the folder.

- Inside your newly create application, type ``` atom . ```, or open your favourite editor
if you do not use atom.

- You should a see a structure like the image below.

![Rails Tree](images/rails_tree.png)

- Next, run rails db:create and db:migrate to initialize your database.

- Finally, run ``` rails s ``` and proceed to ```localhost:3000``` on your browser.
If you see the welcoming image to rails 5, you're all set!

![Yay](images/yay.png)
