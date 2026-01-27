# Build a [[firewall]] with rust
This comes from a very interesting [[øredev]] talk: [Camilo Tapia - Stop the Noise: Build Your Own Smart App Firewall with Rust and Pingora](https://www.youtube.com/watch?v=tcMho35Uv6k)
<iframe width="560" height="315" src="https://www.youtube.com/embed/tcMho35Uv6k?si=bqzjHK9amDqjkyiF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Summary
The internet has become a wild place and everything that is uploaded and available will be scanned and bad people will try to hack it. For a fact. But it is pretty easy to build a small [[proxy]] in front of our application to filter all of that noise.

## Tools
- [[Rust]]
- [Pingora](https://github.com/cloudflare/pingora)


## Thoughts 
- Eye opener, the threads are real and we can't forget that our public webs, apis or whatever are exposed.
- We are a target. It is a fact. Don't think that nobody cares about your small app or API. They will attack!
- We can fairly easily protect us from a big part of the attacks. Not from all threads but from a big majority.