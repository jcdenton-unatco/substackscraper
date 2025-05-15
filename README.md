## Editor's Note

I forked this repo because it was the only substack scraping tool I could find that wasn't either completely broken or full of bloated dependencies (webdriver, etc). It hasn't been kept up to date, but I'd rather hack on this than start my own from scratch.


### TODO

- [ ] Traverse pagination on posts list so the tool can download more than the most recent 23 posts
- [ ] Verify that the cookie strategy for downloading premium posts still works
- [ ] Option to work with custom domain names (formats other than blogname.substack.com)
- [ ] Handle downloading podcast episodes
- [ ] Support other non-image embedded media
- [ ] Support Substack's non-newsletter content (maybe, if I ever find anything good there)
- [ ] Some kind of options for filtering or single-post downloading or whatever


# Substack Scraper

A small cli tool to grab substack posts (free or paid) and outputs it into markdown files.

```
Usage of substackscraper:
  -cookie substack.sid
        Substack API cookie (remove the substack.sid prefix)
  -dest string
        Destination folder to write output to (default ".")
  -output string
        Output format: html or md (default "md")
  -pub string
        Name of the Substack publication to scrape (required)
  -since string
        Fetch posts since this date (YYYY-MM-DD) (default "1970-01-01")
```

## Usage

Building:

```
git clone https://github.com/jcdenton-unatco/substackscraper
go build
```

Running:

You will need to grab your substack cookie from your browser. Without the cookie, it can only fetch the public preview of paid posts.

```
./substackscraper -pub samkriss -cookie COOKIE_SUBSTACK.SID_VALUE_ONLY
```

## Note

This CLI relies on a few undocumented substack API version 1 endpoints. The endpoints might change again at any time, breaking the tool.
