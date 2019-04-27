[back to intro](intro.md)
# Creating a Surface
For rustyview, I want to be able to create a grey background via cairo so I dont have to load an image. Lets see we are going to do that.

I believe that I can use the create_similar call to create a cairo surface.
```rust
pub fn create_similar(
    &self, 
    content: Content, 
    width: i32, 
    height: i32
) -> Surface

// and content is an enum
cairo::Content::{
  Color, Alpha, ColorAlpha,...
}

// so we do this
let surface = cairo::Surface::create_similar(cairo::Content::Color, 1024, 768);
```
