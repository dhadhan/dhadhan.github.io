# Thanks To https://github.com/jstrieb/link-lock

# Link Lock

[Password-protect URLs using AES in the
browser.](https://jstrieb.github.io/link-lock)

Link Lock now supports secure, hidden bookmarks via bookmark knocking! Read
more [here](https://jstrieb.github.io/projects/hidden-bookmarks).



## About

Link Lock is a tool for encrypting and decrypting URLs. When a user visits an
encrypted URL, they will be prompted for a password. If the password is
correct, Link Lock retrieves the original URL and then redirects there.
Otherwise, an error is displayed. Users can also add hints to display near the
password prompt.

Each encrypted URL is stored entirely within the link generated by the
application. As a result, users control all the data they create with Link
Lock. Nothing is ever stored on a server, and there are no cookies, tracking,
or signups.

Link Lock has many uses:

- Store private bookmarks on a shared computer
- Encrypt entire web pages (via [URL
  Pages](https://github.com/jstrieb/urlpages))
- Send sensitive links over public or insecure channels (e.g., posting links
  to a public website that require a password to access)
- Implement simple CAPTCHAs – particularly effective against basic web scrapers
  that do not respect `robots.txt`
- Add a password to shared Dropbox or Google Drive links
- Share password-protected magnet links and torrents
- [Evade censorship](#evading-censorship)

Link Lock uses AES in GCM mode to securely encrypt passwords, and PBKDF2 and
salted SHA-256 (100,000 iterations) for secure key derivation. Encryption,
decryption, and key derivation are all performed by the [`SubtleCrypto`
API](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto). The
initialization vector is randomized by default, but the salt is not.
Randomization of both the initialization vector and salt can be enabled or
disabled by the user via "advanced options." The salt and initialization vector
are sent with the encrypted data if they are randomly generated. The API is
versioned such that old encrypted links will always work, even if later
versions of Link Lock are updated to be more secure. Please read the code
([`api.js`](https://github.com/jstrieb/link-lock/blob/master/api.js) in
particular) for more information.

Read the Hacker News discussion [here](https://news.ycombinator.com/item?id=23242290).

Also [discussed on
r/netsec](https://www.reddit.com/r/netsec/comments/i3n4sm/link_lock_password_protect_urls_using_aes_in_the/)
and [discussed on
r/programming](https://www.reddit.com/r/programming/comments/i5kpjx/link_lock_is_a_tool_for_encrypting_and_decrypting/).



## Examples

- [Regular link encryption](https://jstrieb.github.io/link-lock/#eyJ2IjoiMC4wLjEiLCJlIjoiRkVJR0FzdE53NXFzM1N1N0pVQUtlOEJpWWtYWERaU012WmFDdWxZYWo2eFJzY2lDRGJXd3VZRmFkdVpqbEN4UDN4S0lWQXRoMmJHcjhHWUciLCJoIjoiXG5QYXNzd29yZDogaGFja2VybmV3cyIsImkiOiJBOWJMZkFKbXJ1RFplWktJIn0=) - Password: hackernews
- [Encrypt entire
  pages](https://jstrieb.github.io/link-lock/#eyJ2IjoiMC4wLjEiLCJlIjoiWWhjbG0xeE9uZTJWU2tvc3N1WERwKytyN1lscW1nMVNNemRoSUVER2xVZVBTUFZ3MFA3WTVwQXdnVFVKZkt4WHJ4Nlg1KytCU09RNlVTTlI3M244VEdTeWJGMmJFTG5wc0x6WVRtZnQ3aDFZSzJ5VW16TEpBTk5VOThqZFMvTVFNUG93cWdoRjVUVnYyRWF1VkVHVVlJeE5iT3BtaldCNWJyMWpXemMyakJTNUxZVGVSajNTbVI5UWNwWlRWWmVrbit4Rzd3VzNIcEttRTdVRWNtbkhZS2dydGVmaHp5eTJGNVd6N1NKSm55OTJPWnJUOEFHUE9XY3JUbmxYV0NsTDB5QjVsQmZnUTJkcHk4Y3RmMHNvdVlvb1l2LzQ1U3krZUNtdHl2WkVDd25IeUhwUForamxsaDhuNUV5U2N1ZVRWTmRtRmlmOFBhM0FtdUpQOTdTYWZXbzNwbUo4cU40UFYvMllQbHlwSGFtTmI1dnBBQkc2cU1yUWlLMVp3WHBUSnF4OG9NNFdVVGh3L3B5S0QzOWRNNml2RlNzQzVRUWpaVHl0ODlSNDNVOVdkRDVMWHprdlZ1bVpNSmM2WDExTkI4V0ZSKzdyOGVvVU8wR21rRkxTU0JlaDJickt3bzkwWjRlZkJHTkZiYWE2dU9SWnQzSm1YU0NSSGZyclVRQ053cU96R2pCKzBYZHJFeC9NbHd3QkFKNTIvY0EraW9IUDk5RkszUDN1MlN6Sk1uQzVVSFg1NGNDd1Z2dWdiMzAvUmNsMjZvZzFxUDU0NWJlMGFiak9wYnZ5aFp6RjhkdDNUUjJFLzBMY2dUQUg4dE5wSVAyYzJoM2d4NlJEQUNTZ25LRzlteW4xdFU4Y0IwbWMrd1NPdkxIRlVXVXhIYnpGSkR0aS9MSDg1RDFvdVRNWTFjM3BsSSsxRFFROG5lbjVrR2hmRUhELzdsSFhIY1ZWTHNCbi9HOTFJZU02T2pTeS9aZFcySGZ4d050VzR2WEE3em1FdjhYRDNHL3M2ZTVqdVdQWjV3ck5JWFdzcDVROHdUSlI3U2JQUi94VDNwUUZncW9LaDF2OXVEWGZBaE5xYStXaElzNTlaR1UzdFlkRVFOZEVLdGpIcnF1bzJkcVpuNnB4eTU1ZDJiOVBrcFRLNGh5TEtDOEc1TmN3TEE3dUIzYTNlNlZ2NjVVVHcrdS9oWTBoMy9Nb3ZJaERmT3k2aGZiN2FQaEIyMStxSGZSeWt2VlFPUFZrbE41ak5EK1hKZURialgvd0NUWXJJVm0yOFZkTHppZURob2ZpRGpJRjdyakFQNlF6dWJjaGJYRGFtbFZQWUhOaGVNMWdTeGROSGw5a1lRVE5kbjA1WlcvbVhXNkQwbHk0VkwrOHRwZzdxQjU2YTRyL3lIWHA0Q0tSUkdIaEVWQUptbmh2ZnBaWE11QWdneGVoSkRibVdVKy9VMUgwM2JicUZub2h5R0VGRUxQV2JjZ05kdDJwWU1Cdy81TVNqSkdWWWRPQk5nTUsxbHA2ZVRxRGhwTVdJT2E4a1dSYWx3RzV1bDhuQjhnUVBkcXBCYVdxc3I3V242SVZoZHdLc0FvTGtsdTlnL0JoelNlZEQxRjcyblprN2tSS2l3a3BJbVhOeW9TQk1SSFJSMURjSm9qdU1ZVWlrZ2JxM0dpR2ZqNmMwTTBlU2lyMlhJRnRCTzd2VkJyRmpZL1pvVnJBQ1kwTzJ2UVlGcHovaEprNElKN0daOUpmc3U4ajl5Umc5S3IrNFU3MFhoZHRLY1VYeEtrbCt1VDBtN1owb2puR0xWOGRtampzTVdna3ZhV0FYNkJpK3cycVJKYnVYRW5yUEN5dUZGODhiZ2k3UDNYUVhOMHZTY3h2Uk4wVktKQ1MvR2RVWTJsZ0lDSXVBWFlUVE9KTGNsRkJPQWxialRmZThoTG5saTkzQm4xcnZOamhnM0Y2UkJ2N3NQOTlzODlGT3pwcEZHeHVKS1RhNEg4Y2NSRmxMWDBWbE9kR0RhNWM0NGVTdzh5dCsxWWJndDlvMlExcWNSYVZsaVdadSs5VjdxM1pqcWIxcDdKb2FUN0pDQ1U2ZXR6b0dJWjBQT1JqL3pVNUlVQkRjYXdHZWszZ0djSDBLdDcxa1NSN0F2TWRYeTR3WVI4ZmdTTlpoR3gwSTZYczZ5Vy9oWFB1WERPRjNHTVBTRFFmNGNhUjBuc3pmYTl3MXdGMzVSYktodEVkZnIwU0NLQzhIRXFzNWdsQ0M4RmIxN04wbGtBVlFwSWFRRGJrN254TjVINEFhQ3RKbU5JNHFYUDhocUV6aVhySGhhZWNzNkVBUDBvdjg2cWp4dz09IiwiaCI6InVybHBhZ2U1IiwiaSI6InJNZ2xiSEpzK3pSL2dteFAifQ==) - Password: urlpage5
- [Implement a simple
  CAPTCHA](https://jstrieb.github.io/link-lock/#eyJ2IjoiMC4wLjEiLCJlIjoiZEx3Yi9CNitlK0ZjM1B3ZURrbUY2NjdQWFlIV1dsS3dpclhvZmkvRXBFTXU0ZERlVkJuSmUrN1loS2JxQ3RrPSIsImgiOiIxICsgMSA9ID8iLCJpIjoiRDJYd1MyK1EzaHpuUDV1NyJ9)
- [Emoji
  support](https://jstrieb.github.io/link-lock/#eyJ2IjoiMC4wLjEiLCJlIjoiU1ZBemc0NUVoeXJMR1hXYmRUMXpLSFFIa0hiR2F3SzlMaWZzWW5SL0ZiaGp1cnZqMGg5VTE0bG9kVGs3S3B0TjdhcjZ2T3FvRjJLNkxMcDByL05PZE5nUTJ3UlhVOWM2RmFJdXNGajdrNkFkTC82OVJ6dmlFV2R0dWVacFM1dS9SN2w4L3Mzc1pMTVJNeHdhTVhVenYxTjZUVkdWTGloaXc3ZXlGY093Nkp2ZVN3aGl0OW9XWW84Yk9CMkpkTTF4ZnFRSGExbEoiLCJoIjoi8J+lkSIsImkiOiI5L3pmdHFmeHdoWFh4bDc4In0=) - Password: avocado
- [Riddles in the dark](https://jstrieb.github.io/link-lock/#eyJ2IjoiMC4wLjEiLCJlIjoiYkV5TzZDZ0VwVXZOU2xucWZiQlo3YUdYa1pSL1lub00vekJNaTBMS1VQRXVicktJQTBhS00zNkNEK0tDWXYxd3l0L2dxWTFMS0NPQnFLTldSSEpsYXVodEQ4L0dKVkU3eTFnb2JZalRmcXFtWmpRQTlVMmNhL3dJb2JvMk5DdENnWTZYMHE5MHJzZDk1L0Z6K2orNGlYSHJBa2htN01kL2pJRnNIeDIwZlNVTEFadUpyNjF6TTRucS9nQTZNcW9OS1VKaUxpSWdEakdUd0dhRmdER29OdHJ5Kzd1cUtVbW1BbWQvbWlISERqRDY3bjRtSDA5eWtndlIyVnkzanRsdU44S0puMWdkYWlTeGQvWVhXcUR2WUhrMWIzOVFlSnlIYUV5Q3ZlQkJoQ1RzZDVJbG1obU45WjB0UTdoYUpGVUl1R2hxOTNrOC82MDhpbUxKR0p0M1VNbHRHcEZJck9wUGJuSTlSVkg4WGhwVFh5aEc0Q0ZWajNNU1FGWHdLOXRkc1M4MGF0VGc1MXliZHBEcVIwV1dtWHJBN0hIQ1RCV253WEQ4SGR3SWZidkNmR2s2aW5ZajRHaHpjYUJ4UmV2c08xaGs0M1FmblQya0JHSHhBd0JqQkZTeVlrWCtldGlINHhVVFY3L295ZWtINWxzMmFSczZxc290RDNhS25jWDR3dm1VRUZNKzd3d3Z0UTlhUSt0NFprUWNyWmxGOFBUSVNRRmN3M0ZDMG5zREt1b2lHWWx1S044SGk1dW1QVjZvSC91ckdHSFJBd0VmWDg3VDNubzFBOHRJaEFhYURjSnBpQ2xvZERId3FYOWZmZC9US2JWVUd3a0cxMHdERGN1QUMrR3lJZUVNWExpR2VtNC9DSWNINnRRNTV1UGRZTmZ2aTVuNkU3UHZydFhOSkh5a21jdHpSUVZlSGlOeXp4TDZNY1BSSmZqLy9xZ0dWSWQ0M3Z6aFJBZDFzZHpYa1ZtREhJNFpneDBrQ3dxd05FUllLU3JTNGIwdmV3c3dOQjRVa2lIUFMwMDFpUjVCNG1hUjAvMkhhSFJnWjNMT0RFU1U4djdVNVhrUHllNC85WTZmY3BNeDdWRXg2OUYrem1GL1JsNml3bU1FYk55Ry9YQlU3bERGYXB3Uk1HUmExLzNSTWRObzFoVm5oQ08rNzRMNkxRQlNBSk04VldiZUE2bXVydG1vVHpxb2gvaWtqckxHKzFGUUVvM1lSMnhwQ1VlWVhxOXFSK2YwRGFCM3d4NFk4S0IrcmdUelhLc1JDZXBCTHRjVkhtcFV5UW4rdWswVitDZ0RCOXlPSkJFSXpYSmNnUWxHVlMyOGF4RjFmTnR4UTZkbzdiTU5xbFlqN1RvVzhaQ1NNU0JOVFU5cU92dlNESFBxTFZZOXEzYlhhc3gvZDVwRDBiTzdHTzVsdjlubm91MTZHNUpVTFRlM2IrN0I2azFpZFE3akd0VDFmOTNhUEg0bzZodWlLQXBZdVZQOVJjVHBHVlVyd3dhMzlPcFF4eFNJS2xxeW53d0xwZGxOMjJ0NUJIRm92c1IyUkZCeDhXTFA1a0tuNG9HdTJJcDVBVThDeGs4MG5GZ2VjUGIzS3RuL1lteHJqTVNVVkxmSXMyVUxmbHhqbkE5Q01aWi9ZdG0zZXpFc0VveU1wM3FZWnN1SGh4Rld2ajZXalFkUXdxRXlHY2Z6ZG94dDFvTXh1WVB3ZjF3N3JxWlR6eDVwVkptRGZ1V1VDUUxub2c5c0RqWmRKSHhZVTRadCt2NEYrRDJXdlZ1c0FZSzNJRGJZSnQzOWxSRUxXcUp4dkpaa2pHeXdSaWo1OGsvUzZtTmg0SnJaTk5hNmZFT08rcjU2U2RMcHI1dmh3enorbjFURExiWWZILzdsU0xvc0VaZk4vMFQyOFBrdlJEMlcrMnVvWm5QaVFRV2FnQXdIcmRCc0UrUjdEbFNsdnJWNmJtN0tuSlBWc2NqbXRVVlc3SXBDZ1NNMWh4QWhSZm5hRDJucGdRMmVocUhJZXFlZkpFc281WE1pVDNiWVUwRS92WVdSaHROMTJWdkwxa0k2ejA1UlR3PT0iLCJoIjoiVm9pY2VsZXNzIGl0IGNyaWVzLFxuV2luZ2xlc3MgZmx1dHRlcnMsXG5Ub290aGxlc3MgYml0ZXMsXG5Nb3V0aGxlc3MgbXV0dGVycy4iLCJpIjoiWW91TVFyMmJXRHdGMW1BVSJ9) - The password is a single, lowercase word
- [Share
  torrents](https://jstrieb.github.io/link-lock/#eyJ2IjoiMC4wLjEiLCJlIjoieVJqZnVGdlJETGFTdk4vRVYzUlg3OG9GZHRlWW81U04wcFlvSkFScFRaeXFwZTVoV1lESjFBeDVWRUswMDBNUlQ2ZVAwZ2tCTmlyaVdrYnNsVFdrZTNtNVVOVnoxSW43Z3BST1hQZDhsVmVDTkpJZi81Wm1PWFdzSDZ6dVJmdkVrald0UTRndkZBUE9VSm9id00rdnhtWGtuZW5TZ0pHeW9mMjg3L01pTERDN085NFoxTUwrMzlaNUkwdCtsaW1CaDFaNElWZ1p1QkpQUURvM2NodWZXemdTNU05Zk1FOFlxNXVUV1ZoZjVLV2VaTUR1Q0VWSmN2TjRXbDByZHl6MFpBPT0iLCJoIjoiXG5QYXNzd29yZDogdG9ycmVudGluZ19pcy1sZWdhbCEiLCJpIjoiUlIvNnJtRFhzb1lGblhiOSJ9) - Password: torrenting_is-legal!



## Disclaimer

The code was written to be read. Please read it, especially if you don't trust
me to build a secure encryption application. In particular:

- I am a college student, not a security professional – there may be
  best practices I am not aware of.
- Once someone decrypts a link, they can share the original URL as much as they
  want. Only share encrypted links with trusted people.
- I am not comfortable using JavaScript, and I don't have a firm grasp of the
  nuances of the language – there may be bugs that I don't even know to check
  for.
- This is the first project I have ever done using encryption – there is likely
  a subtle mistake somewhere.
- Most of the encryption/decryption code is based on [MDN
  tutorials](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/deriveKey#PBKDF2_2)
  for the `SubtleCrypto` API.



## Usage

- Create a locked link here: [https://jstrieb.github.io/link-lock](https://jstrieb.github.io/link-lock).
- Once you have a locked link, create a hidden bookmark here:
  <https://jstrieb.github.io/link-lock/hidden>.
- Use the advanced options when creating a link to make the encryption more
  secure (at the cost of a longer link).
    - By default, the initialization vector is randomized for security, but
      this can be disabled, even though doing so is a vulnerability.
    - By default, the salt used to hash the password during key derivation is
      not randomized, but this can be enabled.
- To bookmark a locked link, drag it from the output box to the bookmarks bar.
  Alternatively, visit the locked link and bookmark it before entering the
  password.
- If you lose the password, it is almost impossible to recover the original
  link. The strong security guaranteed by encryption can be a blessing or a
  curse if you are not careful!
- Currently, the only way to recover a lost password is by trying all possible
  options (very slowly) by brute force. An example application to brute force
  Link Lock URLs in the browser can be found here:
  [https://jstrieb.github.com/link-lock/bruteforce](https://jstrieb.github.com/link-lock/bruteforce/).
- A parallelized, cross-platform, CPU-based brute forcer can be found here:
  <https://github.com/jstrieb/bruteforce-link-lock>
- If you receive a Link Lock URL that you do not trust, decrypt it using this
  interface that does not automatically redirect:
  [https://jstrieb.github.com/link-lock/decrypt](https://jstrieb.github.com/link-lock/decrypt/).

### Evading Censorship

Link Lock can be used to evade censorship. If you are concerned that sending
links with the `jstrieb.github.io` domain name will put you at risk, just
replace the domain with another. For example, share

```
https://wikipedia.org/#eyJ2IjoiMC4wLjEiLCJlIjoiYUgrNDhISkpBWWhkeFFMc0l0VlIzeFlma21mYlZCOFJ5Zz09In0=
```

instead of

```
https://jstrieb.github.io/link-lock/#eyJ2IjoiMC4wLjEiLCJlIjoiYUgrNDhISkpBWWhkeFFMc0l0VlIzeFlma21mYlZCOFJ5Zz09In0=
```

Any domain can be used in place of `wikipedia.org`. That way, a malicious
third-party who clicks the altered link will be taken to a valid page, which
helps alleviate suspicion. When sharing the password to unlock the link,
explain how to switch out the domain name with either
`jstrieb.github.io/link-lock`, or with the path to a local clone of Link Lock.
Using a local copy is particularly recommended for evading censorship, since no
request to my domain is ever made.

Alternatively paste the altered link directly into the [decrypt
page](https://jstrieb.github.io/link-lock/decrypt/). This page does not check
the domain name of the pasted link, only the "fragment" (the part after the
`#`). So, for example, the Wikipedia link above can be pasted directly in there
and decrypted without changing the domain.

Using a local copy of [URL Pages](https://github.com/jstrieb/urlpages) is also
recommended. Entire web pages can be shared safely and secretly this way.



## Project Status

This project is actively maintained. If there are no recent commits, it means
that everything has been running smoothly! Even if the link storage protocol
is updated, Link Lock is designed to be 100% backwards-compatible, so your
locked links will never break.

Even if something were to happen to me, and I could not continue to work on
the project, Link Lock will continue to work as long as my GitHub account is
open and the [jstrieb.github.io](https://jstrieb.github.io) domain is online.



## Other Versions & Related Projects

- French translation: [cacheton.site](https://cacheton.site)
- German translation:
  [ebildungslabor.github.io/link-lock](https://ebildungslabor.github.io/link-lock/)




## Acknowledgments

Thank you to those who offered feedback on this program before its release. Thanks also to the Hacker News second-chance pool.

Thanks to [@IAmMandatory](https://twitter.com/iammandatory) for discovering a
reflected XSS vulnerability resulting from allowing non-hypertext protocols in
the URL. The vulnerability has since been fixed.

Thank you to Guillaume ([@gverdun](https://twitter.com/gverdun)) for translating
Link Lock into French, and hosting a translated version. Likewise, thanks to
Nele Hirsch ([@eBildungslabor](https://github.com/eBildungslabor/)) for
translating and hosting a German version!