[back to intro](intro.md)
# Brining Window to the Front
After creating an application, you probably want the main window to raise to the front. This can be achieved by calling `present()` on the window.

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
