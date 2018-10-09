## Setup
```
docker load workshop_images.tar
```

## Hyrax-iiif_av
```
cp -R ../vendor_cache vendor/cache
docker cp ../node_modules connect2018-workshop_avalon_1:/home/node_modules
```

## Hyrax-active_encode
```
cp ../vendor_cache/* vendor/cache/
```
