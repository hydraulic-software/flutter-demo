# Conveyor [Flutter](https://flutter.dev/) Demo

This repository shows how to package a demo [Flutter](https://flutter.dev/) app with [Conveyor](https://hydraulic.software/).
It features:

- Automatic online updates, checked at each launch.
- The [generated download page](https://hydraulic-software.github.io/flutter-demo/download.html) being hosted by GitHub pages.
- Importing version or other data from the `pubspec.yaml` file.

Conveyor is a tool that makes distributing desktop apps easier. It builds, signs and notarizes self-updating
packages for you and offers many useful features like cross-building/signing of packages, different update modes (silent/background and
update-at-launch), and [much more](https://conveyor.hydraulic.dev/).

## Packaging it 

Installi [yq](https://mikefarah.gitbook.io/yq/) in the machine where you run Conveyor, and then [install Conveyor](https://conveyor.hydraulic.dev/latest/download-conveyor/).

Open [conveyor.conf](conveyor.conf) and **edit the marked lines**. The config in this repository uses example deployment URLs.
The package can be built by installing Conveyor and then running:

```
conveyor make site
```

The output directory will now contain packages for Windows, Mac Intel and Mac ARM along with update repository metadata and a generated
download page.

Conveyor can make packages for all supported operating systems from whatever you choose to run it on, but the Flutter build system can't do the same.
The `conveyor.conf` file therefore imports the raw files to package from the output of a [GitHub Actions CI job](.github/workflows/build.yml).

## Releasing

### To a standard website

* Make sure the `app.site.base-url` points to it, and if wanted, set `app.site.copy-to` to an SFTP URL for file upload. 
* Run `conveyor make site --rerun=all`. The `--rerun=all` flag is needed to force a re-download of whatever the latest CI run has produced.
  In a real app you would point the inputs at a versioned URL and forcing a rerun wouldn't be necessary. Use `copied-site` instead of `site`
  if Conveyor can upload the files for you. Otherwise, upload the results yourself.

### To GitHub Releases

* Delete the `app.site` object from `conveyor.conf`. The defaults for open source projects on GitHub is to use Releases.
* Run `conveyor make site --rerun=all`. The `--rerun=all` flag is needed to force a re-download of whatever the latest CI run has produced.
  In a real app you would point the inputs at a versioned URL and forcing a rerun wouldn't be necessary.
* Create a new release with the contents of the `output` directory, except for `download.html` and `icon.svg`. Those files should go into
  the `docs` directory.

## Packaging features used

This repo demos:

1. Importing `pubspec.yaml` to avoid redundant configuration.
2. Customizing the generated default icon.
3. Downloading the results of GitHub Actions.
4. Hosting the download page on GitHub Pages.
