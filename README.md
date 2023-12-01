**DNM!** __This branch is intended to help build Catch Co's MTB emails. It
should not be merged into `main`.__ -GC

# Chord Email Templates

Special thanks to [Gus Childs](https://github.com/guschilds) for creating this awesome tool.  The original code can be found [here](https://github.com/guschilds/chord-emails-template).

## Overview

A tool to help implement and test customizations to Chord email templates.

If you can achieve your needs by overriding a few styles within the Contentful
Email's Custom Styles field, this tool isn't necessary. It exists for those
looking to completely customize the branding of their email templates.

Chord email templates are powered by [Foundation for Emails] and this repository
is based on their [foundation/foundation-emails-template] project. See the
[foundation/foundation-emails] repository README for additional information.

[Foundation for Emails]: https://get.foundation/emails/docs/
[foundation/foundation-emails-template]: https://github.com/foundation/foundation-emails-template
[foundation/foundation-emails]: https://github.com/foundation/foundation-emails

## Getting started

1. Clone/fork the repository.
1. `cd` into the repository root.
1. Run `npm install` to install dependencies.

## Develop

1. Run `npm start` to start the development server. This will open
   `http://localhost:3000/` in the browser.
1. Click the name of the email template you're interested in working with.
1. Make changes to the email template's HTML file within this repository's
   `/pages` directory. The top of the file defines sample content for the
   variables that are used throughout the template.
1. Make changes to the repository's `/assets/scss/_settings.scss` and
   `/assets/scss/_chord-emails.scss` files as needed. The more you can
   accomplish by overriding variables and not adding custom styles, the better.
1. As changes are made to the HTML and Sass files, BrowserSync will refresh the
   browser to reflect these changes.

_Note: It also appears possible to send test emails from this tool using AWS,
but I have not yet tested that. See the Foundation documentation for more info._

## Deploy

_To move the changes into Contentful..._

1. Kill the `npm start` process if it is currently running.
1. Run `npm run build --production` to build the Sass in `production` mode.
   (This will run it through `uncss` to remove any unused styles, as well as
   compress it.)
1. Edit the relevant Email content item within Contentful.
1. Select "No" on the "Include default styles" field (`includeDefaultStyles`).
1. Find and open `/dist/css/app.css` within this repository. It should contain
   compressed CSS constructed from Foundation using your settings, its default
   styles, and your style overrides.
1. Paste the compiled CSS into Contentful's "Custom Styles" field.
1. Copy the template from this repository's HTML file and paste it into
   Contentful's "Message" field. (Do not include the sample content at the top
   of your template file.)
1. Save the Contentful content item.

## Known issue(s)

Using the `{{#variable_name}}...{{/variable_name}}` syntax for `if`  statements
(as opposed to `{{#if variable_name}})...{{/if}}` and seems to cause unexpected
behavior in the browser when using this tool, including:

* The `if` statement will evaluate to `true`, but will not render the variable
  within.
* If the conditional is against a `url` variable that is then rendered within a
  button's `href`, the button will appear collapsed and not properly sized. But
  it will appear properly when the email is sent by Contentful.

When I tried to change the `if`/`each` statement to use the alternate syntax,
the OMS did not send the email. My guess is that the Ruby version of
`foundation-emails` that powers the OMS emails requires this alternate syntax,
but I have not confirmed that.
