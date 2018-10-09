# Customizing encoding

## Customizing encoding options

Create a new file in your Hyrax app at `app/services/my_option_service.rb`
```ruby
class MyOptionService
  # Returns default audio/video output options for ActiveEncode.
  # @param fileset the data to be persisted
  # @return an array of hashes of output options for audio/video
  def self.call(file_set)
    # Defaults adapted from hydra-derivatives
    audio_encoder = Hydra::Derivatives::AudioEncoder.new
    if file_set.audio?
      [{ outputs: [{ label: 'mp3', extension: 'mp3' },
                   { label: 'ogg', extension: 'ogg' }] }]
    elsif file_set.video?
      [{ outputs:
                  [
                    { label: 'high', extension: 'mp4', ffmpeg_opt: "-s 640x480 -g 30 -b:v 700k -ac 2 -ab 96k -ar 44100 -vcodec libx264 -acodec #{audio_encoder.audio_encoder}" },
                    { label: 'low', extension: 'mp4', ffmpeg_opt: "-s 320x240 -g 30 -b:v 345k -ac 2 -ab 96k -ar 44100 -vcodec libx264 -acodec #{audio_encoder.audio_encoder}" }
                  ]
      }]
    else
      []
    end
  end
end
```

Add the following line to `config/initializers/hyrax.rb`:
```
Hyrax::ActiveEncode::ActiveEncodeDerivativeService.default_options_service_class = MyOptionService
```

Restart the avalon container: `docker-compose restart avalon`

Load in a new work and see that the outputs are different.

## Using a custom ActiveEncode::Base subclass

Create a new file in your Hyrax app at `app/services/custom_encode.rb`
```ruby
class CustomEncode < ActiveEncode::Base
  include ActiveEncode::Polling
  include ActiveEncode::Persistence
end
```

Add the following line to `config/initializers/hyrax.rb`:
```
Hyrax::ActiveEncode::ActiveEncodeDerivativeService.default_encode_class = BetterCustomEncode
```

Restart the avalon container: `docker-compose restart avalon`

Load in a new work and see that the polling job ran and that the encode record is persisted.
