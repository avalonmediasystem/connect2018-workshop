# Samvera Connect 2018 Avalon Workshop

The start of fulfilling the promise of the componentization of Avalon from Samvera Connect 2017.

## Background Presentation

https://docs.google.com/presentation/d/1RGcTRwfDxIwRYkrkjRFLX5gUtRKoPKGmzylfQpGgVcs/edit?usp=sharing

## Getting Started

1. Install [Docker](https://docs.docker.com/engine/installation/) and [docker-compose](https://docs.docker.com/compose/install/)

2. `git clone https://github.com/avalonmediasystem/connect2018-workshop.git`

3. `cd connect2018-workshop`

4. `./start.sh` to setup and start the Docker stack

5. Setup initial admin set:
    - Go to http://localhost:3000

    - Go to Login then Sign up.  Register with the email `archivist1@example.com` and any password.

    - Click on Collections in the left sidebar

    - Click New Collection and select Admin Set.  Give the admin set any title and save.


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

2. Enter the rails container: `./connect.sh`

3. Add `hyrax-iiif_av` to `Gemfile`:
```
printf "\ngem 'hyrax-iiif_av'" >> Gemfile
```

4. Run `bundle install`

5. Run generator:
```
rails g hyrax:iiif_av:add_to_work_type Work
```
Notice that this generator includes mixins in the work controller and work show presenter.

6. Run avalon player generator:
```
rails g hyrax:iiif_av:install_avalon_player
```
This will install `webpacker`, `react-rails`, and the avalon player (view partial, react JS, and yarn dependency).  This might take a while.

7. Workaround webpacker issue with non-standard node_modules location:
```
/home/app/node_modules/.bin/webpack --config config/webpack/development.js
```

8. Leave rails container and restart it:
```
docker-compose restart avalon
```
Go back to video work and see the new player and inspect the IIIF Presentation 3.0 manifest.

### How does it work?

- How do I add a custom IIIF viewer? (https://github.com/avalonmediasystem/connect2018-workshop/blob/master/Customizing-the-iiif-viewer.md)
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

1. Enter the rails container: `./connect.sh`

2. Add `hyrax-active_encode` to `Gemfile`:
```
printf "\ngem 'hyrax-active_encode'" >> Gemfile
```

3. Run `bundle install`

4. Run generator:
```
rails g hyrax:active_encode:install
```
Notice that this will include a mixin module in the `FileSet` and set it's indexer.

5. Leave rails container and restart it:
```
docker-compose restart avalon
```

6. Create a new work and checkout the IIIF manifest.  Notice that there is still a mp4 and webm derivative but that the filenames changed.  It really did do something different!

### How does it work?

 - How is `hyrax-active_encode` storing derivative metadata in Fedora?
 - Why isn't it storing technical metadata yet?
 - How is it indexing it into solr?
 - How does it call ActiveEncode?
 - How does it store and serve derivative files?
 - How do I configure it to build different outputs with ffmpeg? (https://github.com/avalonmediasystem/connect2018-workshop/blob/master/Customizing-encoding.md)
 - How do I configure it to use a custom ActiveEncode::Base subclass? (https://github.com/avalonmediasystem/connect2018-workshop/blob/master/Customizing-encoding.md)
 - How do I use Amazon Elastic Transcoder?
 - How do I check on the status of an encode?
 - How do I use the callbacks?
 - How do I use an external streaming server?
 - How do I add my own adapter to ActiveEncode?
 - How will the progress bar work?
 - How is the dashboard going to work?
