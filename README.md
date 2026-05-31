# What is UZone?
It's United Wholesale Mortgage's internal site for company news, chatting, social media etc.

# The Mission
In the bio section, you had a low-feature rich text editor to write a little bit about yourself. 

In my personal site's [about section](https://tigershi.com/about) I write about how back in my gaming forum participation days, figuring how to inject css and javascript into your signature made you cool.

So when I saw you could enter html edit mode in the bio section, muscle memory kinda just took over.

# The Obstacles
1. I only had access to the html file. No .css files.
2. I had a 4096 character limit, including non-leading whitespaces if I recall correctly. Its pretty unusual to have to code with a character limit.
3. Image hosting sites were all blocked.

# The Solution
1. Use inline styles! I even started working in button elements and started trying to get inline javascript functions working.
2. Luckily there was a counter in the corner. The editor would autoformat somewhat so I couldn't game the whitespaces as much as I would have liked.
3. By uploading then grabbing a preview of the image that was hashed to a subdomain which wasn't blocked.

# The Consequence
1. Random people who didn't know me like Underwriters started messaging me about how cool the page was. One even told me he wants to be a frontend developer and that I should teach him.
2. After a while, I found my profile quietly wiped, and no one was allowed to edit the html anymore. I later was told to not do that anymore.

# Other Hacks I Could Have Used
The editor only ever filtered `<script>` tags, so JavaScript could still ride in on plain HTML attributes. The catch was the 4096 character limit: every byte spent on a hack was a byte stolen from the actual content. Decided it wasn't worth it.

1. **`onerror` on a broken image.** `<img src="x" onerror="...">` fires automatically with no user interaction. A classic for auto run functions. The handler body itself eats into the budget fast though.
2. **Auto-firing handlers.** `<body onload="...">`, `<svg onload="...">`, and `onfocus` paired with `autofocus` all run on their own without a click. Same problem: the more interesting the payload, the less room left for the page.
3. **`javascript:` URIs.** `<a href="javascript:...">` still works in anchor hrefs, but this was exactly the kind of thing that got sanitized, so it was a non-starter.
4. **SVG scripting surface.** `<svg><animate onbegin="..." .../></svg>` and friends open a whole second set of event hooks, but the wrapper markup is expensive for what little space remained.

Ultimately, a sanitizer that only blocks `<script>` is doing naive tag-filtering and missing the real attack surface — the `on*` attributes and `javascript:` URIs. Which is probably why the profile eventually got quietly wiped. If I had those I imagine it would have gotten loudly wiped lmao.
