# Mobu
Version 1.0.0

Fork of [Mobu](https://github.com/neighborland/mobu) by Tee Parham

Mobu provides a Rails controller concern called `DetectMobile`.
Mobu does server-side User Agent detection to categorize requests as mobile, tablet, or default.

Mobu modifies your rails view paths based on the request type.
It does not require custom MIME types or separate subdomains.

_Disclaimer: It is rarely a good idea to use User Agent detection to render different HTML content. 
Please read this first:_

https://developer.mozilla.org/en-US/docs/Browser_detection_using_the_user_agent

## Install

Add this line to your Gemfile:

```ruby
gem 'mobu',:git => 'https://github.com/autronix/mobu.git'
```

Include the module in your ApplicationController:

```ruby
class ApplicationController
  include Mobu::DetectMobile
```

Create directories for `views_mobile` and `views_tablet` (optional):

```sh
$ mkdir app/views_mobile
$ mkdir app/views_tablet
```
This version allows to have only the 'views_mobile' path. If 'views_tablet' is not present, it will fall back to 'views_mobile' and use the default mobile layout.

## Usage

Put the view/partial files that you want to override in the appropriate directories.

When you receive a mobile request, your app will first look for view files in `app/views_mobile`
directory, then in `app/views`.

Alternately, you can switch your rendering logic using the `mobile_request?` and `tablet_request?` helper methods
in your view files or helpers:

```haml
- if mobile_request?
  .small-thing Short Text
- else
  .regular-thing Much Longer Text
```

To allow mobile users to switch to the full site view, add a link to a mobile view. For example:

##### app/views_mobile/_footer.haml
```haml
= link_to("View Full Site", prefer_full_site_url)
```

To allow full site users to switch to the mobile view, add a link to a default view. For example:

##### app/views_mobile/_footer.haml
```haml
- if mobile_browser?
  = link_to("View Mobile Site", prefer_mobile_site_url)
```

## Credits

Original code @
[GitHub by Tee Parham](https://github.com/neighborland/mobu).

The view path modification technique was taken from 
[this post by Scott W. Bradley](http://scottwb.com/blog/2012/02/23/a-better-way-to-add-mobile-pages-to-a-rails-site/).

The user agent regex came from a similar project, Brendan Lim's 
[mobile-fu](https://github.com/brendanlim/mobile-fu).
