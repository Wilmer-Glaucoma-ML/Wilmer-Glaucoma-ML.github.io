There are two branches to this repo: `source` and `master`. Update `source` with the changes and then run `bundle exec jekyll build --destination master` to update master and automatically rebuild the website with the new changes.

To update `source`, you can follow the instructions for the "Local Setup" part (yes it says no longer supported but it works with the version this was forked from) here: https://github.com/alshedivat/al-folio/blob/main/INSTALL.md

Namely, install Ruby and Bundler and then run these to install and then host a local version of the repo to view your edits:
```bash
$ bundle install
$ bundle exec jekyll serve
```

To update the bibtex file, I recommend using Zotero and the Zotero Connect extension for your browser and then exporting a bibtex citation for a publication which you can copy to `papers.bib` and add flags like `preview` for the right journal cover image and `selected` if you wish to display it on the front page or not. 