## Digital Analytics Program government-wide code

Provides a JavaScript file for US federal agencies to link or embed in their websites to participate in the Digital Analytics Program.

The most current version of DAP GA code is:

* [`Universal-Federated-Analytics.js`](Universal-Federated-Analytics.js) (full)
* [`Universal-Federated-Analytics-Min.js`](Universal-Federated-Analytics-Min.js) (minified)

### Participating in the DAP

On November 8, 2016, the Office of Management and Budget (OMB) released a memorandum on ["Policies for Federal Agency Public Websites and Digital Services"](https://www.whitehouse.gov/sites/default/files/omb/memoranda/2017/m-17-06.pdf), which requires federal agencies to implement the DAP javascript code on all public facing federal websites.

The Digital Analytics Program offers a central hosting server for its JavaScript files at `dap.digitalgov.gov`.

Agencies are encouraged to use the following HTML snippet to participate in the Digital Analytics Program:

```html
<!-- We participate in the US government's analytics program. See the data at analytics.usa.gov. -->
<script async type="text/javascript" src="https://dap.digitalgov.gov/Universal-Federated-Analytics-Min.js?agency=AGENCY" id="_fed_an_ua_tag"></script>
```

Replace `AGENCY` with your agency's standard abbreviation, such as DHS or EPA.

For more details on implementing the DAP script on your site, including adding other custom parameters, please refer to:
* [DAP Implementation Instructions](https://www.digitalgov.gov/services/dap/analytics-tool-instructions/)
* [Implementation Guide](https://s3.amazonaws.com/sitesusa/wp-content/uploads/sites/212/2014/05/DAP_v3.1_QuickGuide_Aug2016-1.pdf)
* [Code Capabilities Summary](https://s3.amazonaws.com/sitesusa/wp-content/uploads/sites/212/2014/05/DAP_v3.1_CodeSummary_Aug2016-1-1.pdf)
* [Version 3.1 Release Notes](https://s3.amazonaws.com/sitesusa/wp-content/uploads/sites/212/2014/05/DAP_v3.1_ReleaseNotes_Aug2016-1.pdf)

#### Known implementation issues

*Issue:* The Federated code is designed to work on all government sites whether
they already have inline site specific GA trackers or not. There is only one scenario
that is not fully supported by the Federated code, which is when a Universal
Analytics tracking code (that is using a custom/non-default tracking object) is added
right after the Federated code. In this specific scenario the Federated code will fail
in reporting the first page hit and will be able to track normally all the consecutive
hits.

Supported Scenarios:
* UA Site Specific before the Federated code (Default Tracking Object)
* UA Site Specific after the Federated code (Default Tracking Object)
* UA Site Specific before the Federated code (Custom Tracking Object)
* Classic GA Site Specific before the Federated code
* Classic GA Site Specific after the Federated code

*Issue:* The Federated tracking code doesn’t fully support the older versions of
Microsoft Internet Explorer. While the Federated tracking code works with all
known browsers, some features (e.g. the YouTube tracker) may not work properly
on IE 8 and earlier versions because of YouTube API limitations

#### Transport security

The centrally hosted DAP JS is **only available over HTTPS**. Plain HTTP requests will not be successfully redirected, and data collection will not function. Agencies should use only `https://` URLs, not protocol-relative URLs.

Additionally, an [HTTP Strict Transport Security](https://https.cio.gov/hsts/) header is set with a length of 1 year, and is prepared for preloading into major web browsers. As of this writing, that header looks like this:

```
Strict-Transport-Security: max-age=31536000;preload
```

Browsers that support HSTS and which have observed this HSTS policy (either from a prior visit or through HSTS preloading) will not issue HTTP requests to `dap.digitalgov.gov` at all, even if instructed.

Together, HTTPS and HSTS offer a strong, necessary level of transport security and integrity.

#### Data integrity

The `dap.digitalgov.gov` domain is currently served by a third party content delivery network (CDN) that automatically serves the current JavaScript versioned in the `master` branch of this GitHub repository.

**There is no intermediate manual step** between committing to `master` and updating the code on `dap.digitalgov.gov`, though there may be a delay between a commit and a live update.

This means that, barring the compromise of GitHub's systems or the CDN's systems, **all** changes to the code that appears on `dap.digitalgov.gov` should be publicly reflected in [this repository's commit history](https://github.com/digital-analytics-program/gov-wide-code/commits/master).

#### Access controls

This repository is maintained in its own GitHub organization, `digital-analytics-program`, and is operated by the Digital Analytics Program team.

Only Digital Analytics Program staff have been granted write access to this repository.

**All members of the digital-analytics-program GitHub organization are required to have two-factor authentication enabled.**

## Minifying for production

The production build of the script, `Universal-Federated-Analytics-Min.js`, is minified with [UglifyJS](https://github.com/mishoo/UglifyJS2). To build the production script:

1. Clone this repository.
1. Ensure that you have [Node.js](https://nodejs.org/en/) installed.
1. Run `npm install --dev` in the project directory.
1. Run `npm run minify` to regenerate the minified JS and corresponding [source map](https://developers.google.com/web/tools/chrome-devtools/javascript/source-maps).
1. Commit changes to `Universal-Federated-Analytics-Min.js` and the `Universal-Federated-Analytics-Min.js.map` with any chances to the source script, `Universal-Federated-Analytics.js`.
