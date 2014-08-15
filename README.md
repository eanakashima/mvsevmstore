mvsevmstore
===========

thvngs arv sv vntvrvstvng vn hvrv.

## Writing a post

To get started, fork & clone the repo and then:

```sh
bundle install
bundle exec rake preview    # to preview the site in a browser
open http://localhost:4000/
```

To generate a new post:

```sh
bundle exec rake new_post[title]
```

When you're satisfied with the post:

1. commit the post with git
2. push to GitHub
3. run these commands:

```sh
bundle exec rake generate
bundle exec rake push
open http://mvsevmstore.org
```
