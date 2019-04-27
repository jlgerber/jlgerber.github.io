### Rust<span class="tag" data-tag-name="rust"></span>
Rust is my new favorite language (as of this writing). It mixes Haskell concepts liked algebraic datatypes, pattern matching, and type classes, with c concepts like explicit memory management and zero overhead abstractions, with its own take on managing mutability and ownership. 

#### Internal Pages
- gui
    - [gtk](rust/gtk/intro.md)

#### Main Sites

- [main rust page](https://www.rust-lang.org/documentation.html)
- [rust std library](https://doc.rust-lang.org/std/)
- [rust tutorials](http://hackr.io/tutorials/rust)<span class="tag" data-tag-name="tutorial"></span>
- [Learning Rust](https://github.com/ctjhoa/rust-learning)
- [rustup](https://github.com/rust-lang-nursery/rustup.rs) - working with multiple versions of rust


#### Crates of Note
I try and update this list from time to time
- ipc / rpc / networking
    - [zeromq rust](https://github.com/erickt/rust-zmq)
    - [ssh2](https://github.com/alexcrichton/ssh2-rs)
    - [low level networking](https://github.com/libpnet/libpnet)
- Compression
    - [zip](https://github.com/slackito/zip)
    - [tar](https://github.com/alexcrichton/tar-rs)
    - [bzip2](https://github.com/alexcrichton/bzip2-rs)
- cli defining command line interfaces
    - structopt - macros that makes it simpler to use ``clap``
        - [docs](https://docs.rs/structopt/0.2.15/structopt/)
        - [repo](https://github.com/TeXitoi/structopt)
    - clap
        - [webpage](https://clap.rs/)
        - [docs](https://docs.rs/clap/2.33.0/clap/)
        - [repo](https://github.com/clap-rs/clap)
    - [docopt](https://github.com/docopt/docopt.rs)
- ffi
  - python
    - [rust cpython](https://github.com/dgrunwald/rust-cpython)
- [rust container system](https://github.com/tailhook/vagga)
- Filesystem
    - [walkdir](https://github.com/BurntSushi/walkdir) Great crate from Burntsuhsi
    - [fuse file system](https://github.com/zargony/rust-fuse)
- Databases
    - [elasticsearch](https://github.com/benashford/rs-es)
    - [postgres](https://github.com/sfackler/rust-postgres)
    - [mongo](https://github.com/mongodb-labs/mongo-rust-driver-prototype)
- [sharedlib](https://tyleo.github.io/sharedlib/doc/sharedlib/index.html) loading a library at runtime
- Serialization
    - [serde~json~ crate documentation](https://serde-rs.github.io/json/serde_json/index.html)
    - [using serde JSON](https://github.com/serde-rs/json)
- Parsers
    - pest - my go to parser for most things. 
        - [website](https://pest.rs/)
        - [docs](https://docs.rs/pest/2.1.1/pest/)
        - [book](https://pest.rs/book/)
    - nom (parser combinator which is super popular)
        - [docs](https://docs.rs/nom/5.0.0-alpha1/nom/)
        - [repo](https://github.com/Geal/nom)
        - [book](https://stevedonovan.github.io/rust-gentle-intro/nom-intro.html)
    - [combine](https://marwes.github.io/combine/combine/index.html)<span class="tag" data-tag-name="5star"></span>
- Formatting
    - [prettytable-rs console table printing](https://github.com/phsym/prettytable-rs)
- web -ish
    - [memcached](https://github.com/jaysonsantos/bmemcached-rs)
#### Videos
- regex
    - [regex in rust](rust/videos/main.md)
#### Resources

- [rust by example](http://rustbyexample.com/)
- [rust design patterns](https://github.com/nrc/patterns)
- [rust guidelines](http://aturon.github.io/)
- [rustcamp 2015](http://confreaks.tv/events/rustcamp2015)
- [rust learning](https://github.com/ctjhoa/rust-learning)
- [rustlings](https://github.com/carols10cents/rustlings)

#### podcasts

- [new rustacean](http://www.newrustacean.com/)
- [rusty radio](https://soundcloud.com/posix4e/sets/rustyradio)

#### Blogs

- [baby steps](http://smallcultfollowing.com/babysteps/)

#### Articles

- [using the option type effectively](https://blog.8thlight.com/uku-taht/2015/04/29/using-the-option-type-effectively.html)
- [Non Lexical Lifetimes (babysteps)](http://smallcultfollowing.com/babysteps/blog/2016/04/27/non-lexical-lifetimes-introduction/)

#### Books
- [24 days of rust](http://zsiciarz.github.io/24daysofrust/) (list of neat crates)
- [Too Many Lists](http://cglab.ca/~abeinges/blah/too-many-lists/book/README.html)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="book"></span><span class="tag" data-tag-name="5"></span><span class="tag" data-tag-name="star"></span>

great tutorial on rust. Goes through the exercise of iterating on a
linked list. Along the way, you explore rust. Probably the best learning
tutorial out there.

- [glium - opengl in rust](https://tomaka.github.io/glium/book/index.html)

#### Tutorials<span class="tag" data-tag-name="tutorial"></span>

- [ArcadeRs - learn rust by writing a game](https://jadpole.github.io/arcaders/arcaders-1-0)
- [building a web server in rust](https://dfockler.github.io/2016/05/20/web-server.html)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="web"></span>
- [controlling executable size](https://lifthrasiir.github.io/rustlog/why-is-a-rust-executable-large.html)
- [creating a basic web service in rust](http://hermanradtke.com/2016/05/16/creating-a-basic-webservice-in-rust.html)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="web"></span>
- [creating a rust function that accepts string or str](http://hermanradtke.com/2015/05/06/creating-a-rust-function-that-accepts-string-or-str.html)

- [error handling in rust](https://doc.rust-lang.org/book/error-handling.html#the-try-macro)<span class="tag" data-tag-name="5star"></span>
- [rust error handling](http://blog.burntsushi.net/rust-error-handling/)<span class="tag" data-tag-name="5star"></span>
- [set up emacs for rust development](http://julienblanchard.com/2016/fancy-rust-development-with-emacs/)
- [setting up local crates](https://gillesleblanc.wordpress.com/2014/10/10/using-a-local-crate-with-cargo/)
- [string vs str in functions](http://hermanradtke.com/2015/05/03/string-vs-str-in-rust-functions.html)
- [using Copy on Write for optional allocation](http://hermanradtke.com/2015/05/29/creating-a-rust-function-that-returns-string-or-str.html)<span class="tag" data-tag-name="5star"></span>
- [The Guide to Rust Strings](http://www.steveklabnik.com/rust-issue-17340/)
- [writing an OS in rust](https://os.phil-opp.com/)<span class="tag" data-tag-name="tutorial"></span><span class="tag" data-tag-name="rust"></span><span class="tag" data-tag-name="os"></span>
- [more on borrowing, lifetimes, and moves](https://medium.com/@bugaevc/understanding-rust-ownership-borrowing-lifetimes-ff9ee9f79a9c#.s9rmhcok1)
