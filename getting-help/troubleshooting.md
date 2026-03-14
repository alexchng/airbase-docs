# Troubleshooting

## Troubleshooting

_we might not have all the answers for your questions, but we’ll aim to keep improving._

### Application Packaging

<details>

<summary>Why do some of my assets in /public and other directories not get shipped?</summary>

💡

If you need to include assets explicitly, see [How do I include resources?](../faqs.md#how-do-i-include-resources-static-assets-and-other-public-files-eg-public-directories)”.

The amount of space available on serverless infrastructure is limited. Dependency tracing is used to ship only the necessary files, and discard the rest. We recommend importing your assets in this way to reduce your bundle size (max 50 MB).

**✅ Encouraged: Import assets as code**

```javascript
import Image from 'next/image';
import myFancyPicture from "../public/fancypicture.png";

...

<Image alt="caption" src={myFancyPicture}/>
```

**🚫 Avoid: Reference assets by path**

```jsx
<Image alt="caption" src="/public/fancypicture.png"/>
```

</details>

### Application Platform

<details>

<summary>Why can’t my project name be longer than `36` characters?</summary>

This is a temporary limitation in the infrastructure: the length of project names and environment names must fit within 36 characters each.

</details>

[Get Started](../get-started/)
