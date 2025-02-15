---
title: Analytics, User Feedback, SEO
date: 2019-06-05
description: >-
  Add Google Analytics tracking to your site, collect user feedback and learn
  about the page description meta tag.
weight: 8
---

## Adding Analytics

The Docsy theme contains built-in support for
[Google Analytics](https://analytics.google.com/analytics/web/) via Hugo's
[internal template](https://gohugo.io/templates/internal/#google-analytics),
which is included in the theme. Once you set Analytics up as described below,
usage information for your site (such as page views) is sent to your Google
Analytics account.

### Setup

1.  Ensure you have
    [set up a Google Analytics property](https://support.google.com/analytics/answer/1042508)
    for your site: this gives you an Analytics ID to add to your config, which
    Docsy in turn adds to all your site's pages.
2.  Open `config.toml`/`config.yaml`/`config.json`.
3.  Enable Google Analytics by setting the Tracking ID property to your site's
    Analytics ID.

    {{< tabpane persistLang=false >}}

{{< tab header="Configuration file:" disabled=true />}}

{{< tab header="config.toml" lang="toml" >}}

[services.googleAnalytics]
id = "UA-00000000-0"

{{< /tab >}} {{< tab header="config.yaml" lang="yaml" >}}

services:
  googleAnalytics:
    id: UA-00000000-0

{{< /tab >}} {{< tab header="config.json" lang="json" >}}

{
  "services": {
    "googleAnalytics": {
      "id": "UA-00000000-0"
    }
  }
}

{{< /tab >}}
{{< /tabpane >}}

4.  Save and close `config.toml`/`config.yaml`/`config.json`.
5.  Ensure that your site is built with `HUGO_ENV="production"`, as Docsy only
    adds Analytics tracking to production-ready sites. You can specify this
    variable as a command line flag to Hugo:

    ```console
    $ env HUGO_ENV="production" hugo
    ```

    Alternatively, if you're using Netlify, you can specify it as a Netlify
    [deployment setting](https://www.netlify.com/docs/continuous-deployment/#build-environment-variables)
    in `netlify.toml` or the Netlify UI, along with the Hugo version.

## User Feedback

By default Docsy puts a "was this page helpful?" feedback widget at the bottom
of every documentation page, as shown in Figure 1.

<figure>
  <img src="/images/feedback.png"
       alt="The user is presented with the text 'Was this page helpful?' followed
            by 'Yes' and 'No' buttons."/>
  <figcaption>Figure 1. The feedback widget, outlined in red</figcaption>
</figure>

After clicking **Yes** the user should see a response like Figure 2. You can
configure the response text in `config.toml`.

<figure>
  <img src="/images/yes.png"
       alt="After clicking 'Yes' the widget responds with 'Glad to hear it!
            Please tell us how we can improve.' and the second sentence is a link which,
            when clicked, opens GitHub and lets the user create an issue on the
            documentation repository."/>
  <figcaption>
    Figure 2. An example <b>Yes</b> response
  </figcaption>
</figure>

### How is this data useful?

When you have a lot of documentation, and not enough time to update it all, you
can use the "was this page helpful?" feedback data to help you decide which
pages to prioritize. In general, start with the pages with a lot of pageviews
and low ratings. "Low ratings" in this context means the pages where users are
clicking **No** --- the page wasn't helpful --- more often than **Yes** --- the
page was helpful. You can also study your highly-rated pages to develop
hypotheses around why your users find them helpful.

In general, you can develop more certainty around what patterns your users find
helpful or unhelpful if you introduce isolated changes in your documentation
whenever possible. For example, suppose that you find a tutorial that no longer
matches the product. You update the instructions, check back in a month, and the
score has improved. You now have a correlation between up-to-date instructions
and higher ratings. Or, suppose you study your highly-rated pages and discover
that they all start with code samples. You find 10 other pages with their code
samples at the bottom, move the samples to the top, and discover that each
page's score has improved. Since this was the only change you introduced on each
page, it's more reasonable to believe that your users find code samples at the
top of pages helpful. The scientific method, applied to technical writing, in
other words!

### Setup

1.  Open `config.toml`/`config.yaml`/`config.json`.
2.  Ensure that Google Analytics is enabled, as described [above](#setup).
3.  Set the response text that users see after clicking **Yes** or **No**.

    {{< tabpane persistLang=false >}}
    {{< tab header="Configuration file:" disabled=true />}}

{{< tab header="config.toml" lang="toml" >}}
[params.ui.feedback]
enable = true
yes = 'Glad to hear it! Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">tell us how we can improve</a>.'
no = 'Sorry to hear that. Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">tell us how we can improve</a>.'
{{< /tab >}}
{{< tab header="config.yaml" lang="yaml" >}}
params:
  ui:
    feedback:
      enable: true
      'yes': >-
        Glad to hear it! Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">
        tell us how we can improve</a>.
      'no': >-
        Sorry to hear that. Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">
        tell us how we can improve</a>.

{{< /tab >}}{{< tab header="config.json" lang="json" >}}

{
  "params": {
    "ui": {
      "feedback": {
        "enable": true,
        "yes": "Glad to hear it! Please <a href=\"https://github.com/USERNAME/REPOSITORY/issues/new\"> tell us how we can improve</a>.",
        "no": "Sorry to hear that. Please <a href=\"https://github.com/USERNAME/REPOSITORY/issues/new\"> tell us how we can improve</a>."
      }
    }
  }
}

{{< /tab >}}
{{< /tabpane >}}

4.  Save and close `config.toml`/`config.yaml`/`config.json`.

### Access the feedback data

This section assumes basic familiarity with Google Analytics. For example, you
should know how to check pageviews over a certain time range and navigate
between accounts if you have access to multiple documentation sites.

1. Open Google Analytics.
2. Open **Behavior** > **Events** > **Overview**.
3. In the **Event Category** table click the **Helpful** row. Click **view full
   report** if you don't see the **Helpful** row.
4. Click **Event Label**. You now have a page-by-page breakdown of ratings.

Here's what the 4 columns represent:

- **Total Events** is the total number of times that users clicked _either_
  **Yes** or **No**.
- **Unique Events** provides a rough indication of how frequently users are
  rating your pages per session. For example, suppose your **Total Events** is
  5000, and **Unique Events** is 2500. This means that you have 2500 users who
  are rating 2 pages per session.
- **Event Value** isn't that useful.
- **Avg. Value** is the aggregated rating for that page. The value is always
  between 0 and 1. When users click **No** a value of 0 is sent to Google
  Analytics. When users click **Yes** a value of 1 is sent. You can think of it
  as a percentage. If a page has an **Avg. Value** of 0.67, it means that 67% of
  users clicked **Yes** and 33% clicked **No**.

[events]:
  https://developers.google.com/analytics/devguides/collection/analyticsjs/events
[pr]: https://github.com/google/docsy/pull/1/files

The underlying Google Analytics infrastructure that stores the "was this page
helpful?" data is called [Events][events]. See [docsy pull request #1][pr] to
see exactly what happens when a user clicks **Yes** or **No**. It's just a
`click` event listener that fires the Google Analytics JavaScript function for
logging an Event, disables the **Yes** and **No** buttons, and shows the
response text.

### Disable feedback on a single page

Add the parameter `hide_feedback` to the page's front matter and set it to
`true`.

{{< tabpane persistLang=false >}}
{{< tab header="Front matter:" disabled=true />}}
{{< tab header="toml" lang="toml" >}}
+++
hide_feedback = true
+++
{{< /tab >}}
{{< tab header="yaml" lang="yaml" >}}
---
hide_feedback: true
---
{{< /tab >}}
{{< tab header="json" lang="json" >}}
{
    "hide_feedback": true
}
{{< /tab >}}
{{< /tabpane >}}

### Disable feedback on all pages

Set `params.ui.feedback.enable` to `false` in
`config.toml`/`config.yaml`/`config.json`:

{{< tabpane persistLang=false >}}
{{< tab header="Configuration file:" disabled=true />}}
{{< tab header="config.toml" lang="toml" >}}
[params.ui.feedback]
enable = false
{{< /tab >}}
{{< tab header="config.yaml" lang="yaml" >}}
params:
  ui:
    feedback:
      enable: false
{{< /tab >}}
{{< tab header="config.json" lang="json" >}}
{
  "params": {
    "ui": {
      "feedback": {
        "enable": false
      }
    }
  }
}
{{< /tab >}}
{{< /tabpane >}}

## Add a contact form with Fabform

You can create a contact form for your site and collect your form submissions at
[fabform.io](https://fabform.io). To use this feature, you first need to sign up
for an account with Fabform. The following example shows how to add a simple
form that collects the user's email address to your site source:

```html
<form action="https://fabform.io/f/{form-id}" method="post">
 <label for="email">Your Email</label>
 <input name="email" type="email">
 <button type="submit">Submit</button>
</form>
```

For more details, see
[Add a Hugo contact form](https://fabform.io/a/hugo-contact-form) in the Fabform
documentation.

## Search Engine Optimization meta tags

To learn how to optimize your site for SEO see,
[Search Engine Optimization (SEO) Starter Guide](https://developers.google.com/search/docs/beginner/seo-starter-guide).

Google
[recommends](https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=en%2F#descriptionmeta)
using the `description` meta tag to tell search engines what your page is about.
For each generated page, Docsy will set the content of the meta `description` by
using the first of the following that is defined:

- The page `description` [frontmatter field]({{< ref
"content#page-frontmatter" >}})
- For non-index pages, the page [summary][], as computed by Hugo
- The site description taken from the [site `params`][]

For the template code used to perform this computation, see
[layouts/partials/page-description.html][].

Add more meta tags as needed to your project's copy of the `head-end.html`
partial. For details, see [Customizing templates]({{< ref "lookandfeel#customizing-templates"
>}}).

[layouts/partials/page-description.html]: https://github.com/google/docsy/blob/main/layouts/partials/page-description.html
[site `params`]: https://gohugo.io/variables/site/#the-siteparams-variable
[summary]: https://gohugo.io/content-management/summaries/
