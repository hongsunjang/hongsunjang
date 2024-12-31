<p align="center">
    <h2 align="center"> Hongsun's Blog - fork from <a href="https://sergiokopplin.github.io/indigo/">Demo</a></h2>
</p>

<p align="center"> Hello, I'm hongsun who likes to eat noodels. </p>

***
## 맨날 까먹어서 적는 정리

- mac에서 설치하는 법

```
brew install rbenv ruby-build
rbenv install 3.2.2
rbenv global 3.2.2
eval "$(rbenv init -)"
gem install bundler
bundle install
```
- _config.yml 파일을 수정하면 된다.

- 실행은 

```
bundle exec jekyll serve
```

- 목차를 추가하고 싶으면 nav.html을 수정하고 root 폴더에 html 파일을 추가해야함.
목차 추가시 title: blog로 고정  절대 바꾸지 말것

## Setup

0. :star: to the project. :metal:
1. Fork the project [Indigo](https://github.com/sergiokopplin/indigo/fork)
2. Edit `_config.yml` with your data (check <a href="README.md#settings">settings</a> section)
3. Write some posts :bowtie:

If you want to test locally on your machine, do the following steps also:

1. Install [Jekyll](https://jekyllrb.com) and [Bundler](https://bundler.io/).
2. Clone the forked repo on your machine
3. Enter the cloned folder via terminal and run `bundle install`
4. Then run `bundle exec jekyll serve`
5. Open it in your browser: `http://localhost:4000`
6. Do you want an admin panel to edit your posts? You can install this plugin [jekyll-admin](https://jekyll.github.io/jekyll-admin/).

## Settings

You must fill some informations on `_config.yml` to customize your site.

```
name: John Doe
bio: 'A Man who travels the world eating noodles'
picture: 'assets/images/profile.jpg'
...

and lot of other options, like width, projects, pages, read-time, tags, related posts, animations, multiple-authors, etc.
```


## What has inside

- [Jekyll](https://jekyllrb.com/), [Sass](https://sass-lang.com/) ~[RSCSS](https://rscss.io/)~ and [SVG](https://www.w3.org/Graphics/SVG/);
- Google Speed: [98/100](https://developers.google.com/speed/pagespeed/insights/?url=http%3A%2F%2Fsergiokopplin.github.io%2Findigo%2F);
- No JS. :sunglasses:

## FAQ

Check the [FAQ](./FAQ.md) if you have any doubt or problem.

---
## License

[MIT](https://kopplin.mit-license.org/) License © Sérgio Kopplin
