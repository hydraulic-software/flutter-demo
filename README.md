# Conveyor [Flutter](https://flutter.dev/) Demo

This repository shows how to package a demo [Flutter](https://flutter.dev/) app with [Conveyor](https://hydraulic.software/).
It features:

- Automatic online updates, checked at each launch.
- Files released via GitHub Releases.
- The [generated download page](https://hydraulic-software.github.io/flutter-demo/download.html) being hosted by GitHub pages.

Conveyor is a tool that makes distributing desktop apps easier. It builds, signs and notarizes self-updating
packages for you and offers many useful features, like cross-building/signing of packages, different update modes (silent/background and
update-at-launch), and [much more](https://conveyor.hydraulic.dev/).

## Packaging

Open [conveyor.conf](conveyor.conf), which contains a basic config for Flutter. The package can be built by installing Conveyor and 
then running:

```
conveyor make site
```

The output directory will now contain packages for Windows, Mac Intel and Mac ARM along with update repository metadata and a generated
download page.

The `conveyor.conf` file imports the raw files to package from the output of a [GitHub Actions CI job](.github/workflows/build.yml). This is
useful because because the Flutter SDK doesn't support cross-compilation. Conveyor can make packages for all supported OS' from whatever you choose to
run it on, and it would be possible to extend the Flutter build system to support the same feature to allow arbitrary cross-builds.

## Releasing

To do a release: 

* Run `conveyor make site --rerun=all`.
* Put the output files into a new GitHub release, except for `download.html` and `icon.svg`.
* Put `download.html` and `icon.svg` into the `docs` subdirectory, commit and push.

## Packaging features used

This repo demos:

1. Importing `pubspec.yaml` to avoid redundant configuration. This requires installing [yq](https://mikefarah.gitbook.io/yq/) in the machine where you run Conveyor.
2. Downloading the results of GitHub Actions.

## License

**[MIT](LICENSE)**
