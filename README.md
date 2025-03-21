# Astro Blog Schema Markup Enhancer

Plugin for use on the Astro blog to automatically incorporate FAQs from Markdown files. It must be applied from the "[astro-seo-complement](https://github.com/EloyEMC/astro-seo-complement)" plugin in my repository.

This repository contains a powerful enhancement for Astro-based blogs, enabling seamless integration of **Schema Markup** for **BlogPosting**, **BreadcrumbList**, **Author**, and **FAQPage** directly into your blog posts. This tool ensures your blog is optimized for SEO, making it easier for search engines to understand and display your content in rich results.

## Features

- **BlogPosting Schema**: Automatically generates JSON-LD for blog posts, including metadata like title, description, author, and publication date.
- **BreadcrumbList Schema**: Provides structured data for breadcrumbs, improving navigation and SEO.
- **Author Schema**: Generates JSON-LD for authors, including social media links and bio.
- **FAQPage Schema**: Optionally adds FAQ structured data to blog posts, enhancing visibility in search results.
- **Dynamic Integration**: Works seamlessly with Astro's content collections and Markdown files.
- **Customizable**: Easily extend or modify the schema to fit your blog's needs.

## Installation

1. Copy of this repository [astro-seo-complement](https://github.com/EloyEMC/astro-seo-complement) the relevant files into your Astro project.
2. Ensure you have the `astro:content` module installed.
3. Add the provided layout and configuration files to your project.

## Usage

### 1. Define FAQs in Markdown Frontmatter

Add FAQs to your blog post's frontmatter in Markdown files:

```markdown
---
title: "My Blog Post"
description: "A detailed guide on using Schema Markup in Astro."
date: 2023-10-01
faqs:
  - question: "What is Schema Markup?"
    answer: "Schema Markup is a structured data vocabulary that helps search engines understand your content."
  - question: "Why use Schema Markup?"
    answer: "It improves SEO and can lead to rich results in search engines."
---
```
### 2. Add the SEO Component to Your Layout
In your layout file (e.g., src/layouts/BlogPost.astro), import and use the SEO component:
``` astro
---
import SEO from "../components/SEO.astro";
import { siteConfig } from "../config";

const { post, siteUrl, defaultImage } = Astro.props;
---

<SEO
  entry={post}
  siteUrl={siteUrl}
  defaultImage={defaultImage}
/>
```
### 3. Customize the Schema
Modify the faqJSON objects in the layout file to fit your blog's structure and requirements.

```astro
---
import type { CollectionEntry } from 'astro:content';

interface Props {
  entry: CollectionEntry<'blog'>;
 }
const { data } = entry;

// Schema.org (JSON-LD) para FAQs (solo si hay FAQs)
const faqJSON = faqs && faqs.length > 0 ? {
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": faqs.map(faq => ({
    "@type": "Question",
    "name": faq.question,
    "acceptedAnswer": {
      "@type": "Answer",
      "text": faq.answer,
    },
  })),
} : null;
---
   <!DOCTYPE html>
<html lang="en">
  <head>
   <!-- Your code here -->
    <!-- FAQs (JSON-LD) -->
    {faqJSON && (
      <script type="application/ld+json" set:html={` ${JSON.stringify(faqJSON)} `}></script>
    )}
      </head>
      <body>
        <!-- Your content here -->
      </body>
    </html>
´´´


## Example Output

## FAQPage Schema
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is Schema Markup?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Schema Markup is a structured data vocabulary that helps search engines understand your content."
      }
    }
  ]
}
```
## Contributing

Contributions are welcome! If you have ideas for improvements or find any issues, please open an issue or submit a pull request.

---
Enhance your Astro blog's SEO with structured data today! 🚀
