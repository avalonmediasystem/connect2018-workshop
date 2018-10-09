## Setup
```
docker load workshop_images.tar
```

## Hyrax-iiif_av
```
cp -R /path/to/usb/vendor_cache vendor/cache
tar xvzf /path/to/usb/node_modules.tgz /tmp
docker cp /tmp/node_modules connect2018-workshop_avalon_1:/home/node_modules
```

## Hyrax-active_encode
```
cp ../vendor_cache/* vendor/cache/
bundle update active_encode # Need to do this step online
```
