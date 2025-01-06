+++
date = '2025-01-06T14:21:46+08:00'
draft = false
title = 'Hugo New Voice: Adding Cusdis for Dynamic Discussions'
tags = ['coding', 'comments system']
ShowToc = true
+++
## why i choose Cusdis over Discus?
When comparing Cusdis and Disqus, several pros and cons emerge for each commenting system. Here’s a summary of the advantages and disadvantages of using Cusdis over Disqus:

## Pros of Using Cusdis

1. **Open Source and Self-Hosted**: Cusdis is an open-source solution that allows users to host their own commenting system, giving them complete control over their data[1][2][7].

2. **Lightweight**: The JavaScript SDK for Cusdis is approximately 5kb gzipped, making it significantly lighter than Disqus, which is around 24kb gzipped[1][3][9].

3. **Privacy-First**: Cusdis does not use cookies and does not track users, aligning with privacy-focused web practices[1][3][5].

4. **No User Sign-In Required**: Users can comment without needing to create an account, which can enhance user engagement by lowering barriers to participation[3][7].

5. **Email Notifications and Quick Approvals**: Cusdis provides email notifications for new comments and a quick approval process via links in the emails, allowing moderation on-the-go[1][5].

6. **Extensible Integration**: It can easily integrate with various frameworks and platforms, making it versatile for different website setups[1][2].

## Cons of Using Cusdis

1. **Manual Moderation Required**: Comments are not displayed until they are manually approved by a moderator, which may require more effort compared to automated moderation systems in Disqus[3][7][9].

2. **Limited Customization Options**: The comment widget lacks extensive customization features, meaning users cannot modify colors or fonts to match their site’s design[3][5].

3. **Early Development Stage**: As a newer solution, Cusdis may not have all the features or stability of more established platforms like Disqus[7][9].

4. **No Built-In Spam Protection**: Unlike Disqus, which has built-in spam filtering mechanisms, Cusdis requires manual moderation without automated spam detection tools[7][9].

In summary, while Cusdis offers a lightweight, privacy-focused alternative with greater control over data and ease of integration, it also necessitates more manual oversight and lacks some customization options compared to Disqus.

## Step 1: Create a Cusdis Account

- Visit **cusdis.com** to create an account.
- After registration, you'll be directed to your dashboard where you can manage comments.
- Obtain the embed code from the dashboard, which looks like this:

    ```html
    <div id="cusdis_thread"
    data-host="https://cusdis.com"
    data-app-id="{{ APP_ID }}"
    data-page-id="{{ PAGE_ID }}"
    data-page-url="{{ PAGE_URL }}"
    data-page-title="{{ PAGE_TITLE }}">
    </div>
    <script async defer src="https://cusdis.com/js/cusdis.es.js"></script>
    ```

## Step 2: Modify Your Hugo Theme

1. **Embed Code**: Add the Cusdis embed code into your site's `layouts/partials/cusdis.html`. Replace placeholders with:

    ```html
    <div id="cusdis_thread"
    data-host="https://cusdis.com"
    data-app-id="{{ APP_ID }}"
    data-page-id="{{ .Permalink }}"
    data-page-url="{{ .Permalink }}"
    data-page-title="{{ .Title }}"
    data-theme="auto">
    </div>
    <script async defer src="https://cusdis.com/js/cusdis.es.js"></script>
    ```
2. then you create a `comments.html` in the same place and write, which utilize the hugo template system to easy intergrate your own code to theme:
   ```html
   {{ partial "cusdis.html" . }}
   ```

Citations:
[1] https://awsmfoss.com/cusdis/
[2] https://cusdis.com/doc
[3] https://dev.to/lubiah/cusdis-a-privacy-friendly-comment-system-p51
[4] https://news.ycombinator.com/item?id=26878153
[5] https://cusdis.com
[6] https://www.ionos.com/digitalguide/hosting/technical-matters/object-oriented-databases/
[7] https://github.com/djyde/cusdis
[8] https://stackshare.io/cusdis
[9] https://blog.ccknbc.cc/posts/cusdis-or-disqus/
[10] https://dev.to/djyde/i-m-working-on-an-open-source-lightweight-alternative-to-disqus-2c5i
