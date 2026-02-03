---
layout: home
title: Serban Ghita | My thoughts on software
---

## About

### Software:

[MobileDetect](https://mobiledetect.net/) - PHP library to detect mobile devices.
[form-to-object](https://github.com/serbanghita/form-to-object) - Convert HTML forms to field/values to JSON object.
[pathfinding](https://github.com/serbanghita/pathfinding) - library that implements A* with Euclidean distance.
[ecs](https://github.com/serbanghita/ecs) - Entity Component System library.

### Music:

- "It's Personal" (EP) 2025 - [YouTube Music](https://music.youtube.com/playlist?list=OLAK5uy_koWVcODkWu42DSFkpuJAnncPt1-peIRIk&si=al3qdRT2FkXQRWUf), [Spotify](https://open.spotify.com/album/6eMmkMnXgsVK1qhDd5TEbz?si=O6YzJkL6SKWiD5MHXewsOA)
- [Soundcloud](https://soundcloud.com/galigg) - recordings, mostly guitar tracks

### Games:

[Isle of Dinosaurs 2D](https://store.steampowered.com/app/736290/Isle_of_Dinosaurs_2D/)

My profiles:

[Github](https://github.com/serbanghita) - check my software related projects
[LinkedIn](https://www.linkedin.com/in/serbanghita/) - get in contact for job offers

Follow me:

[Serban TV](https://www.youtube.com/@SerbanTV) - YouTube channel, I typically do live stream programming
[Serban TTV](https://www.twitch.tv/serbanttv) - Twitch channel
[Discord](https://discord.gg/hKz3URNwBA) - join my chat server

Check out the [blog posts](/blog) or visit the [contact page](/contact) to get in touch.

## Latest Posts

{% for post in site.posts limit:3 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
{% endfor %}

[View all posts →](/blog)
