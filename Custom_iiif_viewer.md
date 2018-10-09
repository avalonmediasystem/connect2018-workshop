# Adding a custom IIIF viewer

Create a new file in your Hyrax app at `app/views/hyrax/base/iiif_viewers/_my_iiif_viewer.html.erb`:
```
<h3>My IIIF Viewer!</h3>
<a href=<%= main_app.polymorphic_path([main_app, :manifest, presenter], { locale: nil }) %>>Manifest</a>
```

Uncomment `#iiif_viewer` in `app/presenters/hyrax/work_presenter.rb` and change it's return value:
```
def iiif_viewer
  :my_iiif_viewer
end
```

Reload the avalon container: `docker-compose restart avalon`
