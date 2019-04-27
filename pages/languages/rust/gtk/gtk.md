[back to intro](intro.md)
# Creating an Application in gtk
There are a number of ways of creating an application in gtk-rs. You can create an `Application`, then create a `Window`, but the best way of doing this is to use the `gtk::ApplicationWindow` like so:

```rust

fn main() {
    let application = gtk::Application::new(
        "com.github.problem.myname",
        gio::ApplicationFlags::empty()
    ).expect("Application initialization failed");

    // this is where the widget creation lives
    application.connect_startup(|application| {
        let window = ApplicationWindow::new(application);
        window.set_title("my new application");
    ));
    
    application.connect_activate(|_| {});
    application.run(&env::args().collect::<Vec<_>>());
}
```
`gtk::Application::new` takes an application name in the form of a reverse url, and [gio::ApplicationFlags](https://gtk-rs.org/docs/gio/struct.ApplicationFlags.html). The flags may be used to declare that the application is a service, is non unique, hankdles opening files,etc. 

I tend to set this to `gio::ApplicationFlags::empty()`, and handle parsing env vars via `structopt`.
To do this, it is best to pass an empty Vec to `application.run` like so:
```rust
let args : Vec<String> = Vec::new();
application.run(&args);
```

# Bringing Window to the Front
After creating an application, you probably want the main window to raise to the front. This can be achieved by calling `present()` on the window. WHOOPS. The following code makes it impossible to access menu items. Dont do this until they fix the bug. 

```rust
let application = Application::new(
        "com.github.problem.open_segfaults",
        ApplicationFlags::empty()
    ).expect("Application initialization failed");

application.connect_startup( |application| {
   let window = ApplicationWindow::new(application);
   window.set_title("Foobar");
   // Startup Code
   window.show_all();
   window.present();
});
```
