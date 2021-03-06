# Resource plugin for Play Framework 2
Use it like an amazon s3 bucket.
It will only store one version of a file since it computes the sha1 hash of the file and uses that as a pathname. `res/5564/ac5e/5564ac5e3968e77b4022f55a23d36630bdeb0274.jpg`

## Add plugin to dependencies
```scala
val appDependencies = Seq(
	"se.digiplant" %% "play-res" % "INSERT_LATEST_VERSION"
)

val root = PlayProject(appName, appVersion, appDependencies, mainLang = SCALA).settings(
  resolvers += "OSS Sonatype Snapshots" at "http://oss.sonatype.org/content/repositories/snapshots/",
  // To simplify the reverse routing we can import the digiPlant namespace
  templatesImport ++= Seq(
    "se.digiplant._"
  )
)
```

## Add to `conf/application.conf`
```
# Resource plugin save directory
# is relative to app, but can be absolute to filesystem also
res.default=res/default
```

## Add to `conf/routes`
```
GET    /res/:file      se.digiplant.res.ResAssets.at(file)

# If you wan't to separate your resources into separate sources, you can add multiple routes

GET    /images/:file      se.digiplant.res.ResAssets.at(file, "images")

```

# Usage

## Reverse routing
The play-res plugin generates a unique sha1 hash of the file and stores the file on disk in an optimal `4char/4char/sha1hash.ext` directory structure.
By only having to save the fileuid (5564ac5e3968e77b4022f55a23d36630bdeb0274.jpg) to the db it's easy to retrieve the image or other document.

Here are a few examples on how to get files
```html
<img src="@res.routes.ResAssets.at("5564ac5e3968e77b4022f55a23d36630bdeb0274.jpg")" alt="" />
```

Or if you wan't to get the route to an file stored in a different source
```html
<img src="@res.routes.ResAssets.at("5564ac5e3968e77b4022f55a23d36630bdeb0274.jpg", "images")" alt="" />
```

# Advanced Usage
You can specify multiple sources if you would like to split you resources into categories, such as profile images / uploads.
By then adding another source in `conf/application.conf`

```
res.profile=res/profile
```

you will also have to add another route

```
GET    /profile/:file      se.digiplant.res.ResAssets.at(file, "profile")
```



- Source code: [https://github.com/digiPlant/play-res][source]
- CI: [http://travis-ci.org/#!/digiPlant/play-res][ci]

play-res is an Open Source project under the Apache License v2.

[source]: https://github.com/digiPlant/play-res
[ci]: http://travis-ci.org/#!/digiPlant/play-res
