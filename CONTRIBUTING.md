Contributing to DongleAuth.info
=======================

All the data is managed through a series of [Yaml][yaml] files so it may be
useful to read up on the Yaml syntax.

To add a new site, go to the [`data/devices` files](_data/devices/) and get familiar with how it
is setup. There is a section and corresponding file for each Category. Site icons are stored in folders corresponding to each of those categories in their own [folder](img/).

Note: Please do *not* modify yaml files that are directly within the `data` directory. Be sure you're only working on yaml files in the `data/devices` sub-directory. This project is a fork of [the TFA.org project](https://github.com/2factorauth/twofactorauth) and we occasionally pull upstream changes, and this setup helps us avoid unnecessary conflicts.

## Guidelines

1. **Don't break the build**: We have a simple continuous integration system
   setup with [Travis][travis]. If your pull request doesn't pass, it won't be
   merged.

   To manually test the build, just run the following:

    ```bash
    $ ruby verify.rb
    ```

2. **Use a Nice Icon**: The icon must be 32x32 in dimension. Earlier we were
   using 16x16 but upgraded for various high density screens.
3. **Be Awesome**: You need to be awesome. That is all.

## Running Locally

It's easy to run everything locally to test it out. Either you can have plain
[Jekyll][jekyll] installed or you can use [Bundler][bundler] to manage
everything for you.

### Using Bundler

1. To install Bundler, just run `gem install bundler`.
2. Install dependencies in the [Gemfile][gemfile], `bundle install`.
3. Run Jekyll: `bundle exec jekyll serve --watch`. The `--watch` is optional and
   makes Jekyll watch for file changes.

### Using Vanilla Jekyll

1. Install Jekyll if you don't already have it: `gem install jekyll`.
2. Run Jekyll: `jekyll serve --watch`. The `--watch` is again optional.

## Site Criteria

The following section contains rough criteria and explanations regarding
what websites should be listed on DongleAuth.info. If one of the following
criteria is met, it belongs on DongleAuth.info:

1. **Personal Info/Image**: Any site that deals with personal info or a person's
   image. An example of a site with **Personal Info** would be their Amazon
   account and a site regarding **Personal Image** would be one like Twitter.
2. **Data**: This criteria relates to data that is either important or sensitive.
   Websites detailed in criteria 1 also fit this criteria.
3. **Money**: Any site that deals with money.
4. **Control**: This criteria is more general, in that it includes sites that
   give access to things that may infringe upon criteria 1, 2, or 3. An example
   of this is a website that allows remote access to a device.

If you have any questions regarding whether or not a site matches one of the
criteria, simply open an Issue and we'll take a look.

### Excluded Sites

A list for excluded sites has also been created to ensure sites that have been
removed are not added in the future. The list also contains the reason for
its removal.

View the complete list in the [EXCLUSION.md file][exclude].

## New Sections

To add a new section, modify the `sections` value in [main.yml](_data/devices/main.yml)
and follow the template below:

```yml
sections:
  - id: category-id
    title: Category Name
    icon: icon-class
```

Then create a new file in the `_data/devices/` directory named the same as your section's
id with the `.yml` extension.

## New Sites

First and foremost, make sure the new site meets our [definition
requirements](#a-note-on-definitions) for Two Factor Auth.

If you are adding multiple sites to the DongleAuth.info list, please create a new
git branch for each website, and submit a separate pull request for each branch.
More information regarding how to create new git branches can be on
[GitHub's Help Page](https://help.github.com/articles/creating-and-deleting-branches-within-your-repository/)
or [DigitalOcean's Tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-git-branches).

#### Adding a site that *supports* USB dongle auth

If a site does provide auth via USB dongle, it is strongly recommended that you add the `doc:`
field where public documentation is available. Other fields should be included
if the website supports them. Any services that are not supported can be excluded.
Sites supporting auth via USB dongle should not have a Twitter field.

The following is an example of a website that *supports* TFA:

```yml
  - name: YouTube
    url: https://www.youtube.com/
    img: youtube.png
    tfa: Yes
    otp: Yes
    u2f: Yes
    doc: http://www.google.com/intl/en-US/landing/2step/features.html
```

#### Adding a site that *does not* support USB dongle auth

If a site does not provide TFA, the `twitter:` field should be included if the site uses
Twitter. The fields `sms:`, `email:`, `phone:`, `software:` and `hardware:` can be
completely removed.

The following is an example of a website that *does not* supports TFA:

```yml
  - name: Netflix
    url: https://www.netflix.com/us/
    twitter: Netflixhelps
    img: netflix.png
    tfa: No
```

### Exceptions & Restrictions

If a site doesn't support authentication with a USB dongle in certain countries,
you can note this on the website. There are 4 ways to customize how it is displayed:

1. A default message acknowledging restrictions will be used with the following
   config:

   ```yml
    - name: Site Name
      url: https://www.site.com/
      twitter: SiteTwitter
      img: site.png
      tfa: Yes
      otp: Yes
      exceptions: Yes
      doc: <link to site 2FA documentation>
   ```
2. The message can be replaced with a custom set of words:

   ```yml
    - name: Site Name
      url: https://www.site.com/
      twitter: SiteTwitter
      img: site.png
      tfa: Yes
      otp: Yes
      exceptions:
          text: "Specific text goes here."
      doc: <link to site 2FA documentation>
   ```
3. The icon can be made into a link in which more details can be revealed such
   as country specific info and anything else.

   ```yml
    - name: Site Name
      url: https://www.site.com/
      twitter: SiteTwitter
      img: site.png
      tfa: Yes
      otp: Yes
      exceptions:
          link: Yes
      doc: <link to site 2FA documentation>
   ```
4. 2 and 3 can be combined into:

   ```yml
    - name: Site Name
      url: https://www.site.com/
      twitter: SiteTwitter
      img: site.png
      tfa: Yes
      otp: Yes
      exceptions:
          link: Yes
          text: "Specific text can go here as well."
      doc: <link to site 2FA documentation>
   ```

### Pro Tips

- See Guideline #2 about icons. The png file should go in the corresponding
  `img/section` folder.

- Only the auth methods implemented by a site need a `yes` tag, the others can
  just be left off completely.

- For the sake of organization and readability, it is appreciated if you insert
  new sites alphabetically and that your site chunk follows the same order as the
  example above.

- If a site supports auth via USB dongle, their Twitter handle is not needed and can be left out
  for cleanliness.

- If a site does not have auth via USB dongle but there is documentation that they are adding
  it, then use:

  ```yml
  tfa: No
  status: <url to documentation>
  ```

## A Note on Definitions

There are many forms of Two Factor Auth, but DongleAuth.info is **only** interested
in listing sites that support Two Factor Authentication using USB dongles.
Currently that means the site must support either One Time Passwords (OTP)
or Universal 2nd Factor (U2F).

A lot of people have different ideas of what constitutes Two Factor Auth and
what doesn't, so it stands to reason that we should clarify a bit. For the
purposes of this site, Two Factor Auth is defined as any service provided as a
redundant layer for account *authentication*. Services that provide
*authorization* redundancy are certainly appreciated, but should not be
considered Two Factor Authentication.

As an example, a site that prompts you for an authentication token following
user login would be considered Two Factor Auth. A site that does not prompt you
for a token upon login, but prompts you for a token when you try to perform a
sensitive action would not be considered Two Factor Auth.

For context, check out the discussion in [#242][242].

### New Dongles

Rather than split out a list USB Dongle providers on the main page, we elected to keep the main
page clean and add another page dedicated to USB Dongle providers.

To add a new provider simply add to the [`providers.yml` files](_data/devices/providers.yml) file,
marking `Yes` where appropriate.

```yml
  - name: Company Name
    url: https://example.com
    img: company.png
    otp: Yes
    u2f: Yes
```

[242]: https://github.com/2factorauth/twofactorauth/issues/242
[exclude]: /EXCLUSION.md
[bundler]: http://bundler.io/
[gemfile]: /Gemfile
[jekyll]: http://jekyllrb.com/
[travis]: https://travis-ci.org/2factorauth/twofactorauth
[yaml]: http://www.yaml.org/
