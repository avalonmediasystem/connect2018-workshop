# Samvera Connect 2018 Avalon Workshop

The start of fulfilling the promise of the componentization of Avalon from Samvera Connect 2017.

## Background

Link to presentation slides will eventually go here.

## Getting Started

1. Install [Docker](https://docs.docker.com/engine/installation/) and [docker-compose](https://docs.docker.com/compose/install/)

1. `git clone https://github.com/avalonmediasystem/connect2018-workshop.git`

1.  `cd connect2018-workshop`

1. Use current user as the user for Avalon container `` export AVALON_UID=`id -u` AVALON_GID=`id -g` ``

1. `docker-compose up`

1. Setup initial admin set:
  1. Go to http://localhost:3000

  1. Go to Login then Sign up.  Register with the email `archivist1@example.com` and any password.

  1. Click on Collections in the left sidebar

  1. Click New Collection and select Admin Set.  Give the admin set any title and save.

1. Enter the rails container: `docker-compose exec avalon /bin/bash`

## Introducing the Hyrax-iiif_av Plugin

### Why use `hyrax-iiif_av`?

- IIIF Presentation 3.0 support for time-based media
- Content negotiation for backwards compatibility of IIIF Presentation 2.0 viewers
- Ability to include structure within files for IIIF Presentation 3.0 manifests
- IIIF Viewer choice per work type
- Avalon react IIIF viewer
- Shared specs

### Let's do it!

1. Ingest a video file into a new work and look at native html5 player and fairly empty IIIF Presentation 2.0 manifest.
Let's make it more awesomer! :unicorn:

1. Add `hyrax-iiif_av` to `Gemfile`:
```
printf "\ngem 'hyrax-iiif_av'" >> Gemfile
```

1. Run `bundle install`

1. Run generator:
```
rails g hyrax:iiif_av:add_to_work_type Work
```
Notice that this generator includes mixins in the work controller and work show presenter.

1. Run avalon player generator:
```
rails g hyrax:iiif_av:install_avalon_player
```
This will install `webpacker`, `react-rails`, and the avalon player (view partial, react JS, and yarn dependency).  This might take a while.

1. Workaround webpacker issue with non-standard node_modules location:
```
/home/app/node_modules/.bin/webpack --config config/webpack/development.js
```

1. Leave rails container and restart it:
```
docker-compose restart avalon
```
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
```
printf "\ngem 'hyrax-active_encode'" >> Gemfile
```

1. Run `bundle install`

1. Run generator:
```
rails g hyrax:active_encode:install
```
Notice that this will include a mixin module in the `FileSet` and set it's indexer.

1. Leave rails container and restart it:
```
docker-compose restart avalon
```

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
