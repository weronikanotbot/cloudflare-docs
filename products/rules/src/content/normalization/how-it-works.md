---
title: How it works
pcx-content-type: concept
order: 2
---

# How URL normalization works

Cloudflare URL normalization is similar to [rfc3986](https://www.ietf.org/rfc/rfc3986.txt), and modifies separators, encoded elements, and literal bytes in incoming URLs as follows:

* The following unreserved characters are [percent decoded](https://tools.ietf.org/html/rfc3986#section-2.1):
    * Alphabetical characters: `a`-`z`, `A`-`Z` (decoded from `%41`-`%5A` and `%61`-`%7A`)
    * Digit characters: `0`-`9` (decoded from `%30`-`%39`)
    * hyphen `-` (`%2D`), period `.` (`%2E`), underscore `_` (`%5F`), and tilde `~` (`%7E`)
* These reserved characters are not encoded or decoded: `: / ? # [ ] @ ! $ & ' ( ) * + , ; =`
* Other characters, for example literal byte values, are percent encoded.
* Percent encoded representations are converted to upper case.
* URL paths are normalized according to the [Remove Dot Segments](https://tools.ietf.org/html/rfc3986#section-5.2.4) protocol. Deviations from this protocol include modifications to the following separators:
    * `\` becomes `/`
    * `//` becomes `/`

Consider a Firewall Rule that blocks requests whose URLs match `www.example.com/hello`. The rule would not block a request containing an encoded element `www.example.com/%68ello`. Normalizing incoming URLs at the edge helps simplify Cloudflare Firewall Rules expressions that use URLs.

The following table shows some examples of URL normalization:

<TableWrap>

URL                                  | Normalized URL
-------------------------------------|--------------------------------
`www.example.com/hello/`             | `www.example.com/hello/`
`www.example.com/%68ello`            | `www.example.com/hello`
`www.example.com\hello`              | `www.example.com/hello`
`www.example.com/./lang//en/hello./` | `www.example.com/lang/en/hello./`

</TableWrap>

The exact URL normalization performed by Cloudflare varies according to the configured settings. For more information, refer to [URL normalization settings](/normalization/settings).