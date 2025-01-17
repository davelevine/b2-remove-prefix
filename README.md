# B2 Remove Prefix Worker

// Created a Gist to fully write up the steps needed to make this work. It can be found [here](https://gist.github.com/davelevine/229d3fc8d9c75d99c35950e564dbb907). The only thing assumed is that a CDN has already been setup with Cloudflare and Backblaze. The official write up for this can be found [here](https://help.backblaze.com/hc/en-us/articles/217666928-Using-Backblaze-B2-with-the-Cloudflare-CDN). While not absolutely necessary, some familiarity with [Cloudflare Workers](https://workers.cloudflare.com/) is helpful.

## Original README Begins Here

A CloudFlare worker script that removes the `/file/{bucket-name}/` portion of
the URL when access a B2 Bucket.

 - Works with the free tier of CloudFlare and CloudFlare Workers
 - It's still free egress from your B2 bucket
 - No performance penalty, just prettier URLs for your B2 assets
 - Doesn't expose the name of your B2 bucket in the URL

## Why
If you followed [this guide to set up a CloudFlare domain in front of 
Backblaze B2](https://help.backblaze.com/hc/en-us/articles/217666928-Using-Backblaze-B2-with-the-Cloudflare-CDN),
you may have been disappointed by the length of the URL.

For instance, let's say you set up `i.example.com` as a proxied CNAME on
CloudFlare in front of `f001.backblazeb2.com`. Then for you to download a file
called `/test.txt` in the root of the bucket `example-bucket`, the URL would be
`https://i.example.com/file/example-bucket/test.txt`. By putting this Worker
script in front of it, your URL becomes a simple
`https://i.example.com/test.txt`.

## License
[The Unlicense](https://unlicense.org/)

## Installation
1. Use 
[wrangler](https://developers.cloudflare.com/workers/tooling/wrangler/install) 
to generate a new app
```sh
wrangler generate myapp https://github.com/phistrom/b2-remove-prefix
```
2. Overwrite the wrangler-generated `wrangler.toml` with a copy of 
`wrangler.toml.example`.
3. Fill in `zone_id`, `account_id`, and `routes` fields as usual in 
`wrangler.toml`
4. Also set the `B2_BUCKET_NAME` under `[vars]` in `wrangler.toml` to your B2 
bucket name
5. Upload to the routes you specified with `wrangler publish` 

**Alternatively**, you could also just copy and paste the code from `index.js` 
into the *Quick Edit* editor in your CloudFlare dashboard. Just be sure to also 
set the `B2_BUCKET_NAME` and `CF_CACHE_TTL` environment variables for this 
Worker by going into its settings.
