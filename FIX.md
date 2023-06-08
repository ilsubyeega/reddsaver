# Fix 1: Fixed the code to prevent renaming the extension incorrectly by removing the extra dot in the file extension concatenation.
```rust
if (media_type == MediaType::RedditVideoWithoutAudio
        || media_type == MediaType::RedditVideoWithAudio)
        && !extension.ends_with(".mp4") {
    extension = format!("{}.{}", extension, ".mp4");
}
```
This will rename as `test.mp3..mp4` for example of `extension=test.mp3`. So Fixing into:
```rust
if (media_type == MediaType::RedditVideoWithoutAudio
        || media_type == MediaType::RedditVideoWithAudio)
        && !extension.ends_with(".mp4") {
    extension = format!("{}.{}", extension, "mp4");
}
```

# Fix 2: Do not undo when the file is not downloaded
```rust
if self.undo {
    self.user.undo(post_name, listing_type).await?;
}
```
This will undo when the file is not downloaded. So Fixing into:
```rust
if self.undo && is_success {
    self.user.undo(post_name, listing_type).await?;
}
```