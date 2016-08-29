# BONUS - Tired of writing with plain old HTML? Why not try one of these beautiful templating engines:

- [HAML](https://github.com/k0kubun/hamlit)
- [SLIM](https://github.com/slim-template/slim)

- They essentially turn your html like these:

```
<div class="landing index">
  <section class="hero">
    <div class="banner">
      <div class="overlay">
        <div class="content text-center">
          <h1 class="white giant">
            ROTI TELUR DISCUSSIONS
          </h1>
          <hr class="line">
          <% if current_user %>
            <%= link_to "Topics", topics_path, class: "btn btn-settings btn-seagreen", id: "topics-index" %>
          <% else %>
            <%= link_to "Join Us", new_user_path, class: "btn btn-settings btn-seagreen", id: "join-us" %>
          <% end %>
        </div>
      </div>
    </div>
  </section>
</div>
```

- INTO:

- SLIM format:

```
.landing.index
  section.hero
    .banner
      .overlay
        .content.text-center
          h1.white.giant
            ROTI TELUR DISCUSSIONS
          hr.line

          - if current_user
            = link_to "Topics", topics_path, class: 'btn btn-settings btn-seagreen', id: 'topics-index'
          - else
            = link_to "Join Us", new_user_path, class: 'btn btn-settings btn-seagreen', id: 'join-us'
```

- HAML format:

```
.landing.index
  %section.hero
    .banner
      .overlay
        .content.text-center
          %h1.white.giant
            ROTI TELUR DISCUSSIONS
          %hr.line

          - if current_user
            = link_to "Topics", topics_path, class: 'btn btn-settings btn-seagreen', id: 'topics-index'
          - else
            = link_to "Join Us", new_user_path, class: 'btn btn-settings btn-seagreen', id: 'join-us'
```

- A lot cleaner, no?
