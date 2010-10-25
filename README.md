# CoffeeKup
Markup as CoffeeScript

In celebration of [whyday](http://whyday.org/), here's a revisiting of Markaby's concept, this time with the fine flavour of fresh [CoffeeScript](http://coffeescript.org):

    doctype 5
    html ->
      head ->
        meta charset: 'utf-8'
        title "#{@title} | My awesome website"
        meta(name: 'description', content: @description) if @description?
        link rel: 'stylesheet', href: '/stylesheets/app.css'
        style '''
          body {font-family: sans-serif}
          header, nav, section, footer {display: block}
        '''
        script src: '/javascripts/jquery.js'
        coffeescript ->
          $(document).ready ->
            alert 'Alerts are so annoying...'
      body ->
        header ->
          h1 (@title or 'No Title')
          nav ->
            ul ->
              (li -> a href: '/', -> 'Home') unless @path is '/'
              li -> a href: '/chunky', -> 'Chunky'
              li -> a href: '/bacon', -> 'Bacon!'
        section ->
          h2 "Let's count to 10:"
          p i for i in [1..10]
        footer ->
          p 'Bye!'

## _Why?

* Your rendering logic in a hell of a clean, expressive and flexible language.

* Now with a very fast implementation (see `cake benchmark`).

* Precompile your templates to JS on the server and execute them on the browser with only a 2k download.

* The same template language _and_ implementation in node.js or the browser.

* The most efficient CoffeeScript "filter" possible in a template engine (based on cutting edge ".toString()" technology).

* Embedded templates (one-file apps FTW!) that you can still recognize.

* Easily extend it into your own higher-level "dsl" by defining helpers as locals (see below).

* Works with both coffeescript and javascript apps.

* HTML 5 ready! Boring legacy doctypes and elements also available.

* It's just coffeescript! Syntax checking, syntax highlighting and other goodies are [already available](http://jashkenas.github.com/coffee-script/#resources).

## Installing

Just grab [node.js](http://nodejs.org/#download) and [npm](http://github.com/isaacs/npm) and you're set:

    npm install coffeekup

## Using

    coffeekup = require 'coffeekup'

    puts coffeekup.render -> h1 "You can feed me templates in code form."
    puts coffeekup.render "h1 'Or raw strings. I am not too picky.'"

    template = ->
      h1 @title
      form method: 'post', action: 'login', ->
        textbox id: 'username'
        textbox id: 'password'
        button @title

    helpers =
      textbox: (attrs) ->
        attrs.type = 'text'
        attrs.name = attrs.id
        @input attrs

    puts coffeekup.render template, context: {title: 'Log In'}, locals: helpers

With [zappa](http://github.com/mauricemach/zappa):

    get '/': ->
      @users = ['bob', 'alice', 'sinatra', 'zappa']
      render 'default'

    view ->
      for u in @users
        a href: "mailto:#{u}@gmail.com", -> u

With [express](http://expressjs.com):

    app.register '.coffee', require('coffeekup')
    app.set 'view engine', 'coffee'

    app.get '/', (req, res) ->
      # Will render views/index.coffee:
      res.render 'index', context: {foo: 'bar'}

With [meryl](http://github.com/coffeemate/meryl/blob/master/examples/jade-template/app.js) (see also [their own take](http://github.com/coffeemate/meryl/blob/master/examples/coffeekup-template)):

    meryl.h 'GET /', (req, resp) ->
      people = ['bob', 'alice', 'meryl']
      resp.render 'layout', content: 'index', context: {people: people}

    meryl.run
      templateDir: 'templates'
      templateExt: '.coffee'
      templateFunc: coffeekup.adapters.meryl

On the browser:

    <script src="coffeekup.js"></script>
    <script src="app.js"></script>

    # app.coffee before compiling:
    template = -> h1 "Hello #{@world}"
    alert CoffeeKup.render template, context: {world: 'mars'}

Note: this is one of many browser deployment possibilities, compiling on the server, thus requiring only coffeekup.js (under 2k minified, gzipped) on the client-side. To see all serving suggestions, check out [regular](http://github.com/mauricemach/coffeekup/blob/master/examples/browser/regular.html), [decaf](http://github.com/mauricemach/coffeekup/blob/master/examples/browser/decaf.html) and [crème](http://github.com/mauricemach/coffeekup/blob/master/examples/browser/creme.html).

Command-line:

    coffeekup FILE [> OUTPUT]

See [/examples](http://github.com/mauricemach/coffeekup/tree/master/examples) for complete versions. Please note that even though all examples are given in coffeescript, you can also use their plain javascript counterparts just fine.

## Compatibility

Latest version tested with node 0.2.3 and CoffeeScript 0.9.4.
