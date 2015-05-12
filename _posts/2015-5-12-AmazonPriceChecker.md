---
layout: post
title: "Taking advantage of Amazon's price drop policy"
published = true
---

If you purchase an item sold by Amazon and the price drops within 7 days, they will refund the difference. It used to be thirty days but too many folks took advantage of that. I quite like this policy, but I have to admit that I rarely ever go and check whether the prices of dropped within a week on any of my items. Since I have been exploring APIs from different companies lately, I decided this would be an excellent opportunity to work with Google's Gmail API and combine it with scraping Amazon price data to automatically determine whether I am eligible for a refund. 

I broke this project into several sub-problems. First I extracted and processed all order confirmation email that Amazon has sent to my mailbox. Next I scraped the price data from Amazon for all items that were purchased within the last 7 days. This was slightly tricky since Amazon does not allow html requests from robots. In order to get around this limitation I went into the developer console of my browser to extract the headers of a "browser-based" html request.

```html
GET / HTTP/1.1
Host: www.amazon.com
Connection: keep-alive
Cache-Control: max-age=0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.135 Safari/537.36
Accept-Encoding: gzip, deflate, sdch
Accept-Language: en-US,en;q=0.8
Cookie: x-wl-uid=1KtoC6ENCLv2eHX1tihsendl3ViWKpTy60AyS+OYWZxDaGt9qmZCOsG+2k32zvX53cpgxM+EFETnycNMvCQZKbPXx3cT2Ywf7d1M45ElJn47uJiDdzUjUHxBFZUoOHY8OcAsePlQOPoY=; appstore-devportal-locale=en_US; x-main="nKZMwHi?WDeMPa56X4gnBq0ggtGfPTzT"; session-token="fgQpj6zW6t9FM/fI9himLeQ86HY7xo60p7kkwpQN3z3qVSeMvkA70ZNddjMgD4jKwAyfMNZK2l9qYhlcJcGLQbdJQGC4cxlHvEnFB7+BJw/fJ/OxCr3TVTnY/+9RK/Vh/bmJ49RT0LyzBiRoLFYDUuOQG/9PDrlb1govpLGWEM0MQE0bVNUzwfEYHa+Q+/uznlz9yWDMxv9k5+fZ0T4bMw=="; s_cc=true; s_fid=0D00BCD6130993FC-2911D224B6E21834; s_nr=1431213418630-Repeat; s_vnum=1433142000856%26vn%3D2; s_sq=%5B%5BB%5D%5D; skin=noskin; b2b-main=0; csm-hit=s-0EVGT30S70BV35R4Q186|1431465081867; ubid-main=188-8202718-7770715; session-id-time=2082787201l; session-id=190-6280221-3660231
```


