# Samvera Connect 2018 Avalon Workshop

The start of fulfilling the promise of the componentization of Avalon from last year's Samvera Connect.

## Background

Link to presentation slides

## Getting Started

### Docker

1. Install [Docker](https://docs.docker.com/engine/installation/) and [docker-compose](https://docs.docker.com/compose/install/)

1. `git clone https://github.com/avalonmediasystem/connect2018-workshop.git`

1.  `cd connect2018-workshop`

1. Use current user as the user for Avalon container `` export AVALON_UID=`id -u` AVALON_GID=`id -g` ``

1. `docker-compose up`

1. Boostrap the environment:
```ruby
docker-compose exec avalon /bin/bash
# In avalon container
rails hyrax:default_admin_set:create # If necessary
rails c
```

### Manual instructions

1. Clone this repository

1. Install dependencies:
 - https://github.com/samvera/hyrax#characterization
 - https://github.com/samvera/hyrax#transcoding
 - https://github.com/samvera/hyrax#redis
 - Mediainfo:
    - On Mac OS X: `brew install mediainfo`
    - Otherwise see: https://mediaarea.net/en/MediaInfo/Download

1. Bootstrap the environment:
```ruby
bundle install
rails db:migrate
rails hydra:server
rails hyrax:default_admin_set:create
```

## Introducing the Hyrax-iiif_av Plugin

### Why use `hyrax-iiif_av`?

- IIIF Presentation 3.0 support for time-based media
- Content negotiation for backwards compatibility of IIIF Presentation 2.0 viewers
- Ability to include structure within files for IIIF Presentation 3.0 manifests
- IIIF Viewer choice per work type
- Avalon react IIIF viewer
- Shared specs

### Let's do it!

1. Ingest a video file into a new work and look at native html5 player and fairly empty IIIF Presentation 2.0 manifest.  Let's make it more awesomer! :unicorn:

1. Add `hyrax-iiif_av` to `Gemfile`:
```ruby
  gem 'hyrax-iiif_av'
```

1. Run `bundle install`

1. Run generator: `rails g hyrax:iiif_av:add_to_work_type Work`
Notice that this generator includes mixins in the work controller and work show presenter.

1. Run avalon player generator: `rails g hyrax:iiif_av:install_avalon_player`
This will install `webpacker`, `react-rails`, and the avalon player (view partial, react JS, and yarn dependency).

1. Restart rails server.
Go back to video work and see the new player and inspect the IIIF Presentation 3.0 manifest.

### How does it work?

- How do I add a custom IIIF viewer?
- How do I include structure metadata in my manifests?
- Why did you choose react and how does it work within a hyrax app?
- Why did you choose mediaelement.js for the player?
- How do I run the shared specs?


## Introducing the Hyrax-active_encode Plugin

### Why use `hyrax-active_encode`?

- The story of Opencast Matterhorn in Avalon
- Cloud! Cloud! Cloud!
- The simplicity of Hyrax's derivative storage and serving
- Flexible! Robust! Scalable! <insert other buzz words>
- ActiveEncode's abstraction of encoding services
  - Support for local FFmpeg, Elastic Transcoder, Zencoder, and your encoding system
- External storage and streaming
- Information on running and previously run encodes
- Callbacks galore
- Future work:
  - Progress display on work views
  - Dashboard to monitor running, errored, and completed encodes

### Let's do it!

1. Add `hyrax-active_encode` to `Gemfile`:
```ruby
  gem 'hyrax-active_encode'
```

1. Run `bundle install`

1. Run generator: `rails g hyrax:active_encode:install`
Notice that this will include a mixin module in the `FileSet` and set it's indexer.

1. Restart rails server.

1. Create a new work and checkout the IIIF manifest.  Notice that there is still a mp4 and webm derivative but that the filenames changed.  It really did do something different!

### How does it work?

 - How is `hyrax-active_encode` storing derivative metadata in Fedora?
 - Why isn't it storing technical metadata yet?
 - How is it indexing it into solr?
 - How does it call ActiveEncode?
 - How does it store and serve derivative files?
 - How do I configure it to build different outputs with ffmpeg?
 - How do I use Amazon Elastic Transcoder?
 - How do I check on the status of an encode?
 - How do I use the callbacks?
 - How do I use an external streaming server?
 - How do I add my own adapter to ActiveEncode?
 - How will the progress bar work?
 - How is the dashboard going to work?
